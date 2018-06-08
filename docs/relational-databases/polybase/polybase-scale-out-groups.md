---
title: Groupes PolyBase de montée en puissance parallèle| Microsoft Docs
ms.custom: ''
ms.date: 05/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.component: polybase
ms.reviewer: ''
ms.suite: sql
ms.technology: polybase
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- PolyBase
- PolyBase, scale-out groups
- scale-out PolyBase
ms.assetid: c7810135-4d63-4161-93ab-0e75e9d10ab5
caps.latest.revision: 20
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 3fd249645266a7d9477e2dc098817138d8f722d0
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="polybase-scale-out-groups"></a>Groupes de scale-out PolyBase
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une instance de SQL Server autonome avec PolyBase peut se transformer en goulot d’étranglement de performances lors du traitement de gros volumes de jeux données dans Hadoop ou le stockage d’objets Blob Azure. La fonctionnalité Groupe PolyBase vous permet de créer un cluster d’instances de SQL Server pour traiter de grands volumes de jeux de données à partir de sources de données externes telles que Hadoop ou le stockage d’objets Blob Azure, sous forme de montée en puissance (scale-out) parallèle pour des performances de requête optimisées.  
  
 Consultez [Prise en main de PolyBase](../../relational-databases/polybase/get-started-with-polybase.md) et [Guide de PolyBase](../../relational-databases/polybase/polybase-guide.md).  
  
 ![Groupes de scale-out PolyBase](../../relational-databases/polybase/media/polybase-scale-out-groups.png "Groupes de scale-out PolyBase")  
  
## <a name="overview"></a>Vue d'ensemble  
  
### <a name="head-node"></a>Nœud principal  
 Le nœud principal contient l’instance de SQL Server à laquelle les requêtes PolyBase sont envoyées. Chaque groupe PolyBase ne peut avoir qu’un seul nœud principal. Un nœud principal est un regroupement logique du moteur de base de données SQL, du moteur PolyBase et du PolyBase Data Movement Service sur l’instance de SQL Server.  
  
### <a name="compute-node"></a>Nœud de calcul  
 Un nœud de calcul contient l’instance de SQL Server qui assiste dans le traitement des requêtes avec montée en puissance sur des données externes. Un nœud de calcul est un regroupement logique de SQL Server et du PolyBase Data Movement Service sur l’instance de SQL Server. Un groupe PolyBase peut avoir plusieurs nœuds de calcul.  Le nœud principal et les nœuds de calcul doivent tous exécuter la même version de SQL Server.
  
### <a name="distributed-query-processing"></a>Traitement de requêtes distribuées  
 Les requêtes PolyBase sont soumises à SQL Server sur le nœud principal. La partie de la requête qui fait référence à des tables externes est transmise au moteur PolyBase.  
  
 Le moteur PolyBase est le composant clé des requêtes PolyBase. Il analyse l’interrogation de données externes, génère le plan de requête et distribue le travail au service de déplacement des données sur les nœuds de calcul en vue de l’exécution. Une fois l’exécution terminée, il reçoit les résultats depuis les nœuds de calcul et les soumet à SQL Server en vue de leur traitement et de leur renvoi au client.  
  
 Le service de déplacement de données PolyBase reçoit des instructions du moteur PolyBase et transfère les données entre HDFS et SQL Server et entre les instances SQL Server sur les nœuds principal et de calcul.  
  
### <a name="editions-availability"></a>Disponibilité des éditions  
 Après l’installation de SQL Server, l’instance peut être désignée comme un nœud principal ou comme nœud de calcul.  Le choix dépend de la version de SQL Server sur laquelle PolyBase est exécuté. Dans une édition Enterprise, l’instance peut être désignée comme nœud principal ou comme nœud de calcul. Dans une édition Standard, l’instance peut uniquement être désignée comme nœud de calcul.  
  
## <a name="to-configure-a-polybase-group"></a>Pour configurer un groupe PolyBase  
  
### <a name="prerequisites"></a>Conditions préalables requises  
  
-   N machines dans le même domaine  
  
-   Un compte d’utilisateur de domaine pour exécuter les services PolyBase  
  
### <a name="steps"></a>Étapes  
  
1.  Installez la même version de SQL Server avec PolyBase sur N machines.  
  
2.  Sélectionnez une instance de SQL Server en tant que nœud principal. Un nœud principal peut uniquement être désigné sur une instance exécutant SQL Server Entreprise.  
  
3.  Ajoutez les autres instances SQL Server comme nœuds de calcul à l’aide de [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).  
  
4.  Surveillez les nœuds du groupe à l’aide de [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).  
  
5.  Facultatif. Supprimez un nœud de calcul à l’aide de [sp_polybase_leave_group &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-leave-group.md).  
  
## <a name="example-walk-through"></a>Exemple de procédure  
 Cet exemple présente les étapes de configuration d’un groupe PolyBase à l’aide :  
  
1.  de deux machines dans le domaine *PQTH4A* qui ont pour nom :  
  
    -   PQTH4A-CMP01  
  
    -   PQTH4A-CMP02  
  
2.  d’un compte de domaine : *PQTH4A\PolybaseUser*  
  
#### <a name="step-1-install-sql-server-with-polybase-on-all-machines"></a>Étape 1 : installez SQL Server avec PolyBase sur toutes les machines.  
  
1.  Exécutez setup.exe.  
  
2.  Sur la page de sélection de fonctionnalités, sélectionnez **Service de requête PolyBase pour données externes**.  
  
3.  Dans la page Configuration du serveur, utilisez le **compte de domaine** PQTH4A\PolybaseUser pour le moteur SQL Server PolyBase et le service de déplacement de données SQL Server PolyBase.  
  
4.  Dans la page Configuration de PolyBase, sélectionnez l’option **Utiliser ce serveur SQL Server comme composant du groupe de scale-out PolyBase**. Elle ouvre le pare-feu pour autoriser les connexions entrantes aux services PolyBase.  
  
5.  Une fois l’installation terminée, exécutez **services.msc**. Vérifiez que SQL Server, le moteur PolyBase et le service de déplacement de données PolyBase sont en cours d’exécution.  
  
     ![Services PolyBase](../../relational-databases/polybase/media/polybase-services.png "Services PolyBase")  
  
#### <a name="step-2-select-one-sql-server-as-head-node"></a>Étape 2: sélectionnez un serveur SQL Server en tant que nœud principal  
  
-   Une fois l’installation terminée, les deux machines peuvent fonctionner en tant que nœuds principaux d’un groupe PolyBase. Dans cet exemple, nous choisissons « MSSQLSERVER » sur PQTH4A-CMP01 en tant que nœud principal.  
  
#### <a name="step-3-add-other-sql-server-instances-as-compute-nodes"></a>Étape 3 : ajoutez d’autres instances de SQL Server comme nœuds de calcul  
  
1.  Connectez-vous à SQL Server sur PQTH4A-CMP02.  
  
2.  Exécutez la procédure stockée [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).  
  
    ```  
    -- Enter head node details:   
    -- head node machine name, head node dms control channel port, head node sql server name  
    EXEC sp_polybase_join_group 'PQTH4A-CMP01', 16450, 'MSSQLSERVER';  
  
    ```  
  
3.  Exécutez services.msc sur le nœud de calcul (PQTH4A-CMP02).  
  
4.  Arrêtez le moteur PolyBase et redémarrez le service de déplacement des données PolyBase.  
  
#### <a name="optional-remove-a-compute-node"></a>Facultatif : supprimez un nœud de calcul  
  
1.  Connectez-vous au nœud de calcul SQL Server (PQTH4A-CMP02).  
  
2.  Exécutez la procédure stockée sp_polybase_leave_group.  
  
    ```  
    EXEC sp_polybase_leave_group;  
    ```  
  
3.  Exécutez services.msc sur le nœud de calcul en cours de suppression (PQTH4A-CMP02).  
  
4.  Démarrez le moteur PolyBase. Redémarrez le service de déplacement de données PolyBase.  
  
5.  Vérifiez que le nœud a été supprimé en exécutant la DMV sys.dm_exec_compute_nodes sur PQTH4A-CMP01. À présent, PQTH4A-CMP02 fonctionne en tant que nœud principal autonome  
  
## <a name="next-steps"></a>Étapes suivantes  
 Pour le dépannage, consultez [PolyBase troubleshooting with dynamic management views](http://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80).  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en main de PolyBase](../../relational-databases/polybase/get-started-with-polybase.md)   
 [Guide de PolyBase](../../relational-databases/polybase/polybase-guide.md)   
 [PolyBase Connectivity Configuration (Configuration de la connectivité PolyBase) &#40;Transact-SQL&#41;](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md)  
  
  
