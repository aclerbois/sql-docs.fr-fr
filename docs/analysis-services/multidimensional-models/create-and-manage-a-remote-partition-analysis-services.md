---
title: Créer et gérer une Partition distante (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 32c687ee8bb2d3c7efc323f71652c511c0272c42
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-manage-a-remote-partition-analysis-services"></a>Créer et gérer une partition distante (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous partitionnez un groupe de mesures, vous pouvez configurer une base de données secondaire sur une instance distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] comme stockage de partition.  
  
 Les partitions distantes d'un cube (appelé la base de données master) sont stockées dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dédiée sur l'instance distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (appelée base de données secondaire).  
  
 Une base de données secondaire dédiée peut stocker les partitions distantes pour une seule base de données master, mais la base de données master peut utiliser plusieurs bases de données secondaires, tant que toutes les bases de données secondaires se trouvent sur la même instance distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les dimensions dans une base de données dédiée aux partitions distantes sont créées en tant que dimensions liées.  
  
## <a name="prerequisites"></a>Conditions préalables  
 Pour vous permettre de créer une partition distante, les conditions suivantes doivent être remplies :  
  
-   Vous devez disposer d’une seconde instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et d’une base de données dédiée pour stocker les partitions. La base de données secondaire a une fonction unique ; elle fournit le stockage des partitions distantes pour une base de données master.  
  
-   Les deux instances du serveur doivent présenter la même version. Les deux bases de données doivent avoir le même niveau fonctionnel.  
  
-   Les instances doivent être configurées en vue de connexions TCP. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]ne prend pas en charge la création de partitions distantes à l’aide du protocole HTTP.  
  
-   Les paramètres de pare-feu sur les deux ordinateurs doivent être définis de manière à accepter les connexions externes. Pour plus d’informations sur la configuration du pare-feu, consultez [Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
-   Le compte de service pour l’instance exécutant la base de données master doit bénéficier de l’accès administratif à l’instante distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Si le compte de service change, vous devez mettre à jour les autorisations à la fois sur le serveur et sur la base de données.  
  
-   Vous devez être administrateur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur les deux ordinateurs.  
  
-   Vous devez vous assurer que votre plan de récupération d'urgence est adapté à une sauvegarde et à une restauration des partitions distantes. L'utilisation de partitions distantes peut compliquer les opérations de sauvegarde et de restauration. Veillez à tester soigneusement votre plan pour être certain de pouvoir restaurer les données nécessaires.  
  
## <a name="configure-remote-partitions"></a>Configurer les partitions distantes  
 Deux ordinateurs distincts qui exécutent une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sont nécessaire pour créer une organisation de partitions distantes qui indique un ordinateur en tant que serveur maître et l’autre ordinateur en tant que serveur subordonné.  
  
 La procédure suivante suppose que vous disposez de deux instances de serveur, avec une base de données de cube déployée sur le serveur maître. Dans le cadre de cette procédure, il est fait référence à la base de données de cube sous le terme db-master. La base de données de stockage contenant les partitions distantes est désignée sous le terme de db-storage.  
  
 Vous allez utiliser à la fois [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour exécuter cette procédure.  
  
> [!NOTE]  
>  Les partitions distantes peuvent être fusionnées uniquement avec d'autres partitions distantes. Si vous utilisez une combinaison de partitions locales et distantes, une autre méthode consiste à créer des partitions qui contiennent les données combinées, en supprimant les partitions que vous n'utilisez plus.  
  
#### <a name="specify-valid-server-names-for-cube-deployment-in-ssdt"></a>Spécifier les noms de serveur valides pour le déploiement du cube (dans SSDT)  
  
1.  Sur le serveur maître : dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nom de la solution et sélectionnez **Propriétés**. Dans la boîte de dialogue **Propriétés** , cliquez sur **Propriétés de configuration**, puis sur **Déploiement**et sur **Serveur** , et définissez le nom du serveur maître.  
  
2.  Sur le serveur subordonné : dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nom de la solution et sélectionnez **Propriétés**. Dans la boîte de dialogue **Propriétés** , cliquez sur **Propriétés de configuration**, puis sur **Déploiement**et sur **Serveur** , et définissez le nom du serveur subordonné.  
  
#### <a name="create-and-deploy-a-secondary-database-in-ssdt"></a>Créer et déployer une base de données secondaire (dans SSDT)  
  
1.  Sur le serveur subordonné : créez un projet Analysis Services pour la base de données de stockage.  
  
2.  Sur le serveur subordonné : dans l'explorateur de solutions, créez une source de données désignant la base de données de cube, db-master. Utilisez le fournisseur **OLE DB natif\Fournisseur Microsoft OLE DB pour Analysis Services 11.0**.  
  
3.  Sur le serveur subordonné : déployez la solution.  
  
#### <a name="enable-features-in-ssms"></a>Activer les fonctionnalités (dans SSMS)  
  
1.  Sur le serveur subordonné : dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur l’instance connectée de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans l’Explorateur d’objets, puis sélectionnez **Propriétés**. Affectez la valeur **True** à **Feature\LinkToOtherInstanceEnabled** et **Feature\LinkFromOtherInstanceEnabled**.  
  
2.  Sur le serveur subordonné : redémarrez le serveur en cliquant avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets, puis en sélectionnant **Redémarrer**.  
  
3.  Sur le serveur maître : dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur l’instance connectée de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans l’Explorateur d’objets, puis sélectionnez **Propriétés**. Affectez la valeur **True** à **Feature\LinkToOtherInstanceEnabled** et **Feature\LinkFromOtherInstanceEnabled**.  
  
4.  Sur le serveur maître : pour redémarrer le serveur, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets, puis sélectionnez **Redémarrer**.  
  
#### <a name="set-the-masterdatasourceid-database-property-on-the-remote-server-in-ssms"></a>Définir la propriété de base de données MasterDataSourceID sur le serveur distant (dans SSMS)  
  
1.  Sur le serveur subordonné : cliquez avec le bouton droit sur la base de données de stockage, db-storage, pointez sur **Générer un script de la base de données en tant que** | **ALTER To** | **Nouvelle fenêtre d’éditeur de requête**.  
  
2.  Ajoutez **MasterDataSourceID** à XMLA, puis spécifiez l’ID de la base de données de cube, db-master, comme valeur. Le code XMLA doit ressembler à ce qui suit.  
  
    ```  
    <Alter ObjectExpansion="ExpandFull" xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <Object>  
       <DatabaseID>DB-Storage</DatabaseID>  
    </Object>  
    <ObjectDefinition>  
       <Database xmlns:xsd="http://www.w3.org/2001/XMLSchema" 400"   
          <ID>DB-Storage</ID>  
          <Name>DB-StorageB</Name>  
          <ddl200:CompatibilityLevel>1100</ddl200:CompatibilityLevel>  
          <Language>1033</Language>  
          <Collation>Latin1_General_CI_AS</Collation>  
          <DataSourceImpersonationInfo>  
    <ImpersonationMode>ImpersonateAccount</ImpersonationMode>  
             <Account>*********</Account>  
          </DataSourceImpersonationInfo>  
          <MasterDataSourceID>DB-Master</MasterDataSourceID>  
       </Database>  
    </ObjectDefinition>  
    </Alter>  
    ```  
  
3.  Appuyez sur F5 pour exécuter le script.  
  
#### <a name="set-up-the-remote-partition-in-ssdt"></a>Configurer la partition distante (dans SSDT)  
  
1.  Sur le serveur maître : ouvrez le cube dans le Concepteur de cube et cliquez sur l’onglet **Partitions** . Développez le groupe de mesures. Cliquez sur **Nouvelle partition** si le groupe de mesures est déjà configuré pour plusieurs partitions, ou cliquez sur le bouton de navigation (. . ) dans la colonne Source pour modifier la partition existante.  
  
2.  Dans l’Assistant Partition, dans **Spécifier des informations sur la source**, sélectionnez la table des faits et la vue de source de données d’origine.  
  
3.  Si vous utilisez une liaison de requête, spécifiez une clause WHERE qui segmente les données de la nouvelle partition que vous créez.  
  
4.  Dans **Emplacements pour le traitement et le stockage**, sous **Emplacement du traitement**, choisissez **Source de données Analysis Services distante** , puis cliquez sur **Nouveau** pour créer une source de données qui désigne la base de données subordonnée, db-storage.  
  
    > [!NOTE]  
    >  Si vous obtenez une erreur indiquant que la source de données n'existe pas dans la collection, vous devez ouvrir le projet de la base de données de stockage, db-storage, et créer une source de données qui désigne la base de données master, db-master.  
  
5.  Sur le serveur maître : cliquez avec le bouton droit sur le nom du cube dans l’Explorateur de solutions, sélectionnez **Traiter** et traitez entièrement le cube.  
  
## <a name="administering-remote-partitions"></a>Administration de partitions distantes  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]prend en charge le traitement des partitions distantes des parallèle et séquentiel. La base de données master, dans laquelle les partitions ont été définies, coordonne les transactions entre toutes les instances qui participent au traitement des partitions d'un cube. Les rapports de traitement sont ensuite envoyés à toutes les instances qui ont traité une partition.  
  
 Un cube qui contient des partitions distantes peut être administré avec ses partitions sur une instance unique de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Toutefois, les métadonnées de la partition distante ne peuvent être affichées et mises à jour que sur l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] où la partition et son cube parent ont été définis. La partition distante ne peut pas être visualisée ni mise à jour sur l’instance distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  Bien que les bases de données dédiées au stockage des partitions distantes ne soient pas exposées aux ensembles de lignes de schéma, les applications utilisant des objets AMO (Analysis Management Objects) peuvent toujours découvrir une base de données dédiée à l'aide de XML pour la commande Analysis Discover. Toute commande CREATE ou DELETE envoyée directement à une base de données dédiée à l'aide d'un protocole TCP ou client HTTP réussit, mais le serveur retourne un avertissement indiquant que l'action peut endommager cette base de données étroitement gérée.  
  
## <a name="see-also"></a>Voir aussi  
 [Partitions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data.md)  
  
  
