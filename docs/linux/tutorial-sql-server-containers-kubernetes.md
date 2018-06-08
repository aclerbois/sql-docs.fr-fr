---
title: Configurer un conteneur de SQL Server dans Kubernetes pour la haute disponibilité | Documents Microsoft
description: Ce didacticiel montre comment déployer une solution de haute disponibilité de SQL Server avec Kubernetes sur le Service de conteneur Azure.
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 01/10/2018
ms.topic: tutorial
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux,mvc
ms.technology: linux
ms.openlocfilehash: 4aaaee69ab9c81df2161f465c2c725d5b2be3c17
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="configure-a-sql-server-container-in-kubernetes-for-high-availability"></a>Configurer un conteneur de SQL Server dans Kubernetes pour la haute disponibilité

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Découvrez comment configurer une instance de SQL Server sur Kubernetes dans Azure conteneur de Service (AKS), le stockage persistant pour la haute disponibilité (HA). La solution fournit la résilience. Si l’instance de SQL Server échoue, Kubernetes automatiquement recrée dans un nouveau module. AKS fournit la résilience contre une défaillance de nœud Kubernetes. 

Ce didacticiel montre comment configurer une instance de SQL Server à haute disponibilité dans des conteneurs qui utilisent AKS. 

> [!div class="checklist"]
> * Créer un mot de passe
> * Création de stockage
> * Créer le déploiement
> * Se connecter à SQL Server Management Studio (SSMS)
> * Vérifiez que la défaillance et récupération

## <a name="ha-solution-that-uses-kubernetes-running-in-azure-container-service"></a>Haute disponibilité solution qui utilise Kubernetes en cours d’exécution dans le Service de conteneur Azure

Kubernetes 1.6 et versions ultérieures prend en charge [classes de stockage](http://kubernetes.io/docs/concepts/storage/storage-classes/), [volume persistant revendications](http://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims)et le [type de volume de disque Azure](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). Vous pouvez créer et gérer vos instances de SQL Server en mode natif dans Kubernetes. L’exemple dans cet article montre comment créer un [déploiement](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) pour obtenir une configuration haute disponibilité similaire à une instance de cluster de basculement de disque partagé. Dans cette configuration, Kubernetes joue le rôle de l’orchestrateur du cluster. En cas d’échec d’une instance de SQL Server dans un conteneur, l’orchestrateur amorce une autre instance du conteneur qui s’attache à un même stockage persistant.

![Diagramme de cluster Kubernetes SQL Server](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

Dans le diagramme précédent, `mssql-server` est un conteneur dans un [pod](http://kubernetes.io/docs/concepts/workloads/pods/pod/). Kubernetes orchestre les ressources du cluster. A [jeu de réplicas](http://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) garantit que le bloc est automatiquement récupérée après une défaillance de nœud. Les applications se connecter au service. Dans ce cas, le service représente un équilibreur de charge qui héberge une adresse IP qui reste le même après l’échec de la `mssql-server`.

Dans le diagramme suivant, la `mssql-server` conteneur a échoué. Comme l’orchestrateur, Kubernetes garantit le nombre correct d’instances saines dans le réplica défini et crée un conteneur en fonction de la configuration. L’orchestrateur démarre un nouveau module sur le même nœud, et `mssql-server` se reconnecte au même stockage persistant. Le service se connecte à nouveau créé `mssql-server`.

![Diagramme de cluster Kubernetes SQL Server](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-pod-fail.png)

Dans le diagramme suivant, le nœud qui héberge le `mssql-server` conteneur a échoué. L’orchestrateur démarre le nouveau module sur un autre nœud, et `mssql-server` se reconnecte au même stockage persistant. Le service se connecte à nouveau créé `mssql-server`.

![Diagramme de cluster Kubernetes SQL Server](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-node-fail.png)

## <a name="prerequisites"></a>Configuration requise

* **Kubernetes cluster**
   - Le didacticiel requiert un cluster Kubernetes. Les étapes utilisent [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) pour gérer le cluster. 

   - Consultez [déployer un cluster du Service de conteneur Azure (AKS)](http://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster) pour créer et de se connecter à un cluster à nœud unique Kubernetes dans AKS avec `kubectl`. 

   >[!NOTE]
   >Pour protéger contre les défaillances de nœud, un cluster Kubernetes nécessite plus d’un nœud.

* **CLI Azure 2.0.23**
   - Les instructions de ce didacticiel ont été validées par rapport à 2.0.23 CLI d’Azure.

## <a name="create-an-sa-password"></a>Créer un mot de passe

Créer un mot de passe dans le cluster Kubernetes. Kubernetes peuvent gérer les informations de configuration sensibles, telles que les mots de passe en tant que [secrets](http://kubernetes.io/docs/concepts/configuration/secret/).

La commande suivante crée un mot de passe pour le compte d’administrateur système :

   ```azurecli
   kubectl create secret generic mssql --from-literal=SA_PASSWORD="MyC0m9l&xP@ssw0rd"
   ```  

   Remplacez `MyC0m9l&xP@ssw0rd` avec un mot de passe complexe.

   Pour créer une clé secrète dans Kubernetes nommé `mssql` qui contient la valeur `MyC0m9l&xP@ssw0rd` pour le `SA_PASSWORD`, exécutez la commande.


## <a name="create-storage"></a>Création de stockage

Configurer un [volume persistant](http://kubernetes.io/docs/concepts/storage/persistent-volumes/) et [volume persistant revendication](http://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-protection) dans le cluster Kubernetes. Procédez comme suit : 

1. Créer un manifeste pour définir la classe de stockage et le volume persistant revendication.  Le manifeste spécifie le de fournisseur de stockage, paramètres, et [récupérer la stratégie](http://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming). Le cluster Kubernetes utilise ce manifeste pour créer le stockage persistant. 

   L’exemple yaml suivant définit une classe de stockage et le volume persistant revendication. Un fournisseur de la classe de stockage est `azure-disk`, car ce cluster Kubernetes dans Azure. Le type de compte de stockage est `Standard_LRS`. La revendication volume persistant est nommée `mssql-data`. Les métadonnées de la revendication volume persistant incluent une annotation se connecter à nouveau à la classe de stockage. 

   ```yaml
   kind: StorageClass
   apiVersion: storage.k8s.io/v1beta1
   metadata:
        name: azure-disk
   provisioner: kubernetes.io/azure-disk
   parameters:
     storageaccounttype: Standard_LRS
     kind: Managed
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mssql-data
     annotations:
       volume.beta.kubernetes.io/storage-class: azure-disk
   spec:
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
   ```

   Enregistrez le fichier (par exemple, **pvc.yaml**).

1. Créez la revendication volume persistant dans Kubernetes.

   ```azurecli
   kubectl apply -f <Path to pvc.yaml file>
   ```

   `<Path to pvc.yaml file>` est l’emplacement où vous avez enregistré le fichier.

   Le volume persistant est automatiquement créé en tant que compte de stockage Azure et lié à la revendication de volume persistant. 

    ![Capture d’écran de la commande de revendication de volume persistant](media/tutorial-sql-server-containers-kubernetes/02_pvc_cmd.png)

1. Vérifiez que la revendication volume persistant.

   ```azurecli
   kubectl describe pvc <PersistentVolumeClaim>
   ```

   `<PersistentVolumeClaim>` est le nom de la revendication volume persistant.

   Dans l’étape précédente, la revendication de volume persistant est nommée `mssql-data`. Pour voir les métadonnées relatives à la revendication de volume persistant, exécutez la commande suivante :

   ```azurecli
   kubectl describe pvc mssql-data
   ```

   Les métadonnées retournées incluent une valeur appelée `Volume`. Cette valeur correspond au nom de l’objet blob.

   ![Capture d’écran de métadonnées retournées, y compris le Volume](media/tutorial-sql-server-containers-kubernetes/describe-volume.png)

   La valeur pour le volume correspond à une partie du nom de l’objet blob dans l’image suivante à partir du portail Azure : 

   ![Nom d’objet blob de portail de capture d’écran de Azure](media/tutorial-sql-server-containers-kubernetes/describe-volume-portal.png)

1. Vérifiez que le volume persistant.

   ```azurecli
   kubectl describe pv
   ```

   `kubectl` Retourne des métadonnées sur le volume persistant qui a été automatiquement créé et lié à la revendication de volume persistant. 

## <a name="create-the-deployment"></a>Créer le déploiement

Dans cet exemple, le conteneur qui héberge l’instance de SQL Server est décrit comme un objet de déploiement Kubernetes. Le déploiement crée un jeu de réplicas. Le jeu de réplicas crée le bloc. 

Dans cette étape, créez un manifeste pour décrire le conteneur basés sur le serveur SQL Server [mssql-server-linux](https://hub.docker.com/r/microsoft/mssql-server-linux/) image Docker. Le manifeste fait référence à la `mssql-server` volume persistant revendication et le `mssql` secret que vous avez déjà appliqué au cluster Kubernetes. Le manifeste décrit également un [service](http://kubernetes.io/docs/concepts/services-networking/service/). Ce service est un équilibreur de charge. L’équilibrage de charge garantit que l’adresse IP persiste une fois que l’instance de SQL Server est récupérée. 

1. Créez un manifeste (fichier YAML) pour décrire le déploiement. L’exemple suivant décrit un déploiement, y compris un conteneur en fonction de l’image de conteneur de SQL Server.

   ```yaml
   apiVersion: apps/v1beta1
   kind: Deployment
   metadata:
     name: mssql-deployment
   spec:
     replicas: 1
     template:
       metadata:
         labels:
           app: mssql
       spec:
         terminationGracePeriodSeconds: 10
         containers:
         - name: mssql
           image: microsoft/mssql-server-linux
           ports:
           - containerPort: 1433
           env:
           - name: ACCEPT_EULA
             value: "Y"
           - name: SA_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: mssql
                 key: SA_PASSWORD 
           volumeMounts:
           - name: mssqldb
             mountPath: /var/opt/mssql
         volumes:
         - name: mssqldb
           persistentVolumeClaim:
             claimName: mssql-data
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: mssql-deployment
   spec:
     selector:
       app: mssql
     ports:
       - protocol: TCP
         port: 1433
         targetPort: 1433
     type: LoadBalancer
   ```

   Copiez le code précédent dans un nouveau fichier nommé `sqldeployment.yaml`. Mettre à jour les valeurs suivantes : 

   * `value: "Developer"`: Définit le conteneur pour exécuter SQL Server Developer edition. Édition développeur n’est pas autorisée à des données de production. Si le déploiement est pour la production, définir l’édition appropriée (`Enterprise`, `Standard`, ou `Express`). 

      >[!NOTE]
      >Pour plus d’informations, consultez [la licence SQL Server](http://www.microsoft.com/sql-server/sql-server-2017-pricing).

   * `persistentVolumeClaim`: Cette valeur nécessite une entrée pour `claimName:` qui mappe au nom utilisé pour la revendication volume persistant. Ce didacticiel utilise `mssql-data`. 

   * `name: SA_PASSWORD`: Configure l’image de conteneur pour définir le mot de passe SA, tel que défini dans cette section.

     ```yaml
     valueFrom:
       secretKeyRef:
         name: mssql
         key: SA_PASSWORD 
     ```

     Lorsque Kubernetes déploie le conteneur, il fait référence à la clé secrète nommée `mssql` pour obtenir la valeur du mot de passe. 

   >[!NOTE]
   >À l’aide de la `LoadBalancer` type de service, l’instance SQL Server est accessible à distance (via internet) sur le port 1433.

   Enregistrez le fichier (par exemple, **sqldeployment.yaml**).

1. Créer le déploiement.

   ```azurecli
   kubectl apply -f <Path to sqldeployment.yaml file>
   ```

   `<Path to sqldeployment.yaml file>` est l’emplacement où vous avez enregistré le fichier.

   ![Capture d’écran de la commande de déploiement](media/tutorial-sql-server-containers-kubernetes/04_deploy_cmd.png)

   Le déploiement et le service sont créés. L’instance de SQL Server est dans un conteneur connecté à un stockage persistant.

   Pour afficher l’état de la pod, tapez `kubectl get pod`.

   ![Capture d’écran de la commande get pod](media/tutorial-sql-server-containers-kubernetes/05_get_pod_cmd.png)

   Dans l’image précédente, le bloc a le statut `Running`. Cet état indique que le conteneur est prêt. Cette opération peut prendre plusieurs minutes.

   >[!NOTE]
   >Une fois le déploiement est créé, il peut prendre quelques minutes avant que le bloc est visible. Le délai d’attente est parce que le cluster extrait le [mssql-server-linux](https://hub.docker.com/r/microsoft/mssql-server-linux/) image à partir du hub Docker. Une fois que l’image est extraite de la première fois, les déploiements suivants peuvent être plus rapides si le déploiement est à un nœud qui possède déjà l’image mise en cache sur ce dernier. 

1. Vérifiez que les services sont en cours d’exécution. Exécutez la commande suivante :

   ```azurecli
   kubectl get services 
   ```

   Cette commande renvoie les services qui sont en cours d’exécution, ainsi que les adresses IP internes et externes pour les services. Notez l’adresse IP externe pour le `mssql-deployment` service. Utilisez cette adresse IP pour se connecter à SQL Server. 

   ![Capture d’écran de la commande de service get](media/tutorial-sql-server-containers-kubernetes/06_get_service_cmd.png)

   Pour plus d’informations sur l’état des objets dans le cluster Kubernetes, exécutez :

   ```azurecli
   az aks browse --resource-group <MyResourceGroup> --name <MyKubernetesClustername>
   ```  

## <a name="connect-to-the-sql-server-instance"></a>Connectez-vous à l’instance de SQL Server

Si vous avez configuré le conteneur comme décrit, vous pouvez vous connecter à une application à partir de l’extérieur du réseau virtuel Azure. Utilisez le `sa` compte et l’adresse IP externe d’adresses pour le service. Utilisez le mot de passe que vous avez configuré en tant que le secret Kubernetes. 

Vous pouvez utiliser les applications suivantes pour vous connecter à l’instance de SQL Server. 

* [SSMS](http://docs.microsoft.com/sql/linux/sql-server-linux-manage-ssms)

* [SSDT](http://docs.microsoft.com/en-us/sql/linux/sql-server-linux-develop-use-ssdt)

* sqlcmd
   
   Pour se connecter avec `sqlcmd`, exécutez la commande suivante :

   ```cmd
   sqlcmd -S <External IP Address> -U sa -P "MyC0m9l&xP@ssw0rd"
   ```

   Remplacez les valeurs suivantes :
      
    - `<External IP Address>` avec l’adresse IP pour le `mssql-deployment` service 
    - `MyC0m9l&xP@ssw0rd` avec votre mot de passe

## <a name="verify-failure-and-recovery"></a>Vérifiez que la défaillance et récupération

Pour vérifier la défaillance et récupération, vous pouvez supprimer le bloc. Procédez comme suit :

1. Liste du bloc de SQL Server en cours d’exécution.

   ```azurecli
   kubectl get pods
   ```

   Notez le nom de la pod exécutant SQL Server.

1. Supprimer le bloc.

   ```azurecli
   kubectl delete pod mssql-deployment-0
   ```
   `mssql-deployment-0` la valeur est retournée à partir de l’étape précédente pour le nom du bloc. 

Kubernetes recrée automatiquement du bloc pour récupérer une instance de SQL Server, puis connectez le stockage permanent. Utilisez `kubectl get pods` pour vérifier qu’un nouveau module est déployé. Utilisez `kubectl get services` pour vérifier que l’adresse IP pour le nouveau conteneur est le même. 

## <a name="summary"></a>Résumé

Dans ce didacticiel, vous avez appris comment déployer des conteneurs de SQL Server sur un cluster Kubernetes pour la haute disponibilité. 

> [!div class="checklist"]
> * Créer un mot de passe
> * Création de stockage
> * Créer le déploiement
> * Se connecter à SQL Server Management Studio (SSMS)
> * Vérifiez que la défaillance et récupération

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
>[Introduction aux Kubernetes](http://docs.microsoft.com/en-us/azure/aks/intro-kubernetes)


