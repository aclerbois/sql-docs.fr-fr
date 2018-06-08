---
title: Gestionnaire de connexions Hadoop - SQL Server Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.hadoopconn.f1
ms.assetid: 8bb15b97-9827-46bc-aca6-068534ab18c4
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d0f00f507a7347e729a6531261c963cdeae41b62
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hadoop-connection-manager"></a>Gestionnaire de connexions Hadoop
  Le Gestionnaire de connexions Hadoop permet à un package SSIS (SQL Server Integration Services) de se connecter à un cluster Hadoop en utilisant les valeurs que vous spécifiez pour les propriétés.  
  
## <a name="configure-the-hadoop-connection-manager"></a>Configurer le Gestionnaire de connexions Hadoop  
  
1.  Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS** , sélectionnez **Hadoop** > **Ajouter**. La boîte de dialogue **Éditeur du Gestionnaire de connexions Hadoop** s’affiche.  
  
2.  Pour configurer les informations du cluster Hadoop associées, choisissez l’onglet **WebHCat** ou **WebHDFS** dans le volet gauche.
  
3.  Si vous activez l’option **WebHCat** pour appeler un travail Hive ou Pig sur Hadoop, procédez comme suit : 
  
    1.  Pour **Hôte WebHCat**, entrez le serveur qui héberge le service WebHCat.  
  
    2.  Dans la zone **Port WebHCat**, entrez le port du service WebHCat, défini par défaut sur 50111.  
  
    3.  Dans la zone **Authentification** , sélectionnez la méthode d’authentification pour l’accès au service WebHCat. Les valeurs disponibles sont **De base** et **Kerberos**.  
  
         ![Capture d’écran de l’éditeur du gestionnaire de connexions Hadoop avec authentification de base](../../integration-services/connection-manager/media/hadoop-cm-basic.png "Éditeur du gestionnaire de connexions Hadoop avec authentification de base")  
  
         ![Capture d’écran de l’éditeur du gestionnaire de connexions Hadoop avec authentification Kerberos](../../integration-services/connection-manager/media/hadoop-cm-kerberos.png "Éditeur du gestionnaire de connexions Hadoop avec authentification Kerberos")  
  
    4.  Dans la zone **Utilisateur WebHCat**, entrez l’ **Utilisateur** autorisé à accéder à WebHCat.  
  
    5.  Si vous sélectionnez l’authentification **Kerberos** , entrez le **Mot de passe** et le **Domaine**de l’utilisateur.  
  
4.  Si vous activez l’option **WebHDFS** pour copier des données depuis ou vers HDFS, procédez comme suit : 
  
    1.  Dans la zone **Hôte WebHDFS**, entrez le serveur qui héberge le service WebHDFS.  
  
    2.  Dans la zone **Port WebHDFS**, entrez le port du service WebHDFS, défini par défaut sur 50070.  
  
    3.  Dans la zone **Authentification** , sélectionnez la méthode d’authentification pour l’accès au service WebHDFS. Les valeurs disponibles sont **De base** et **Kerberos**.  
  
    4.  Dans la zone **Utilisateur WebHDFS**, entrez l’utilisateur autorisé à accéder à HDFS.  
  
    5.  Si vous sélectionnez l’authentification **Kerberos** , entrez le **Mot de passe** et le **Domaine**de l’utilisateur.  
  
5.  Sélectionnez **Tester la connexion**. (Le test ne porte que sur la connexion que vous avez activée.)  
  
6.  Sélectionnez **OK** pour fermer la boîte de dialogue.  

## <a name="connect-with-kerberos-authentication"></a>Se connecter avec l’authentification Kerberos
Il existe deux options permettant de configurer l’environnement local pour pouvoir utiliser l’authentification Kerberos avec le Gestionnaire de connexions Hadoop. Vous pouvez choisir l’option qui correspond le mieux à votre situation.
-   Option 1 : [joindre l’ordinateur SSIS au domaine Kerberos](#kerberos-join-realm)
-   Option 2 : [Activer l’approbation mutuelle entre le domaine Windows et le domaine Kerberos](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Option 1 : Joindre l’ordinateur SSIS au domaine Kerberos

#### <a name="requirements"></a>Conditions requises :

-   L’ordinateur servant de passerelle doit se joindre au domaine Kerberos et ne peut se joindre à aucun domaine Windows.

#### <a name="how-to-configure"></a>Pour effectuer la configuration :

Sur l’ordinateur SSIS :

1.  Exécutez l’utilitaire **Ksetup** pour configurer le serveur et le domaine du centre de distribution de clés Kerberos.

    L’ordinateur doit être configuré en tant que membre d’un groupe de travail, car un domaine Kerberos est différent d’un domaine Windows. Définissez le domaine Kerberos et ajoutez un serveur de centre de distribution de clés Kerberos, comme indiqué dans l’exemple suivant. Remplacez `REALM.COM` par votre propre domaine.

    ```    
    C:> Ksetup /setdomain REALM.COM`
    C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
    ```

    Après avoir exécuté ces commandes, redémarrez l’ordinateur.

2.  Vérifiez la configuration avec la commande **Ksetup**. Le résultat doit ressembler à l’exemple suivant :

    ```
    C:> Ksetup
    default realm = REALM.COM (external)
    REALM.com:
        kdc = <your_kdc_server_address>
    ```

### <a name="kerberos-mutual-trust"></a>Option 2 : activer l’approbation mutuelle entre le domaine Windows et le domaine Kerberos

#### <a name="requirements"></a>Conditions requises :
-   L’ordinateur servant de passerelle doit se joindre à un domaine Windows.
-   Vous devez avoir l’autorisation de mettre à jour les paramètres du contrôleur de domaine.

#### <a name="how-to-configure"></a>Pour effectuer la configuration :

> [!NOTE]
> Remplacez `REALM.COM` et `AD.COM` dans ce didacticiel par votre propre domaine et votre propre contrôleur de domaine.

Sur le serveur du centre de distribution de clés Kerberos :

1.  Modifiez la configuration du centre de distribution de clés Kerberos dans le fichier **krb5.conf**. Autorisez le centre de distribution de clés Kerberos à approuver le domaine Windows en référençant le modèle de configuration suivant. Par défaut, la configuration se trouve dans **/etc/krb5.conf**.

    ```
    [logging]
    default = FILE:/var/log/krb5libs.log
    kdc = FILE:/var/log/krb5kdc.log
    admin_server = FILE:/var/log/kadmind.log

    [libdefaults]
    default_realm = REALM.COM
    dns_lookup_realm = false
    dns_lookup_kdc = false
    ticket_lifetime = 24h
    renew_lifetime = 7d
    forwardable = true

    [realms]
    REALM.COM = {
        kdc = node.REALM.COM
        admin_server = node.REALM.COM
        }
    AD.COM = {
        kdc = windc.ad.com
        admin_server = windc.ad.com
        }

    [domain_realm]
    .REALM.COM = REALM.COM
    REALM.COM = REALM.COM
    .ad.com = AD.COM
    ad.com = AD.COM

    [capaths]
    AD.COM = {
        REALM.COM = .
        }
    ```

    Après la configuration, redémarrez le service du centre de distribution de clés Kerberos.

2.  Préparez un principal nommé **krbtgt/REALM.COM@AD.COM**  sur le serveur du centre de distribution de clés Kerberos. Exécutez la commande suivante :

    `Kadmin> addprinc krbtgt/REALM.COM@AD.COM`

3.  Dans le fichier de configuration du service HDFS **hadoop.security.auth_to_local**, ajoutez `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

Sur le contrôleur de domaine :

1.  Exécutez la commande **Ksetup** suivante pour ajouter une entrée de domaine :

    ```
    C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
    C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM
    ```

2.  Établissez l’approbation du domaine Windows pour le domaine Kerberos. Dans l’exemple suivant, `[password]` est le mot de passe pour le principal **krbtgt/REALM.COM@AD.COM** .

    `C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /password:[password]`

3.  Sélectionnez un algorithme de chiffrement à utiliser avec Kerberos.

    1. Accédez à **Gestionnaire de serveur** > **Gestion des stratégies de groupe** > **Domaine**. À partir de là, accédez à **Objets de stratégie de groupe** > **Stratégie de domaine Par défaut ou Active** > **Modifier**.

    2. Dans la fenêtre contextuelle **Éditeur de gestion des stratégies de groupe**, accédez à **Configuration de l’ordinateur** > **Stratégies** > **Paramètres Windows**. À partir de là, accédez à **Paramètres de sécurité** > **Stratégies locales** > **Options de sécurité**. Configurez **Sécurité du réseau : Configurer les types de chiffrement autorisés pour Kerberos**.

    3. Sélectionnez l’algorithme de chiffrement à utiliser pour se connecter au centre de distribution de clés Kerberos. En règle générale, vous pouvez sélectionner n’importe quelle option.

        ![Capture d’écran des types de chiffrement pour Kerberos](media/hadoop-connection-manager/config-encryption-types-for-kerberos.png)

    4. Utilisez la commande **Ksetup** pour spécifier l’algorithme de chiffrement à utiliser sur le domaine spécifique.

        `C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96`

4.  Pour utiliser le principal Kerberos dans le domaine Windows, créez le mappage entre le compte de domaine et le principal Kerberos.

    1. Accédez à **Outils d’administration** > **Utilisateurs et ordinateurs Active Directory**.

    2. Configurez les fonctionnalités avancées en sélectionnant **Afficher** > **Fonctionnalités avancées**.

    3. Recherchez le compte pour lequel vous voulez créer des mappages, cliquez avec le bouton droit pour afficher **Mappages des noms**, puis sélectionnez l’onglet **Noms Kerberos**.

    4. Ajoutez un principal à partir du domaine.

        ![Capture d’écran de la boîte de dialogue Mappage des identités de sécurité](media/hadoop-connection-manager/map-security-identity.png)

Sur l’ordinateur servant de passerelle :

Exécutez la commande **Ksetup** suivante pour ajouter une entrée de domaine.

    ```
    C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
    C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM
    ```

## <a name="see-also"></a>Voir aussi  
 [Tâche Hive Hadoop](../../integration-services/control-flow/hadoop-hive-task.md)   
 [Hadoop Pig, tâche](../../integration-services/control-flow/hadoop-pig-task.md)   
 [Tâche du système de fichiers Hadoop](../../integration-services/control-flow/hadoop-file-system-task.md)  
  
  
