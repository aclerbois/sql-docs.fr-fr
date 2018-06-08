---
title: Concepts liés à Replication Management Objects | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- replication [SQL Server], RMO
- programming interfaces [SQL Server replication]
- replication [SQL Server], how-to topics
- RMO [SQL Server]
- Replication Management Objects
- programming [SQL Server replication], RMO
ms.assetid: 37476d50-fb47-49e3-9504-3b163ac381d8
caps.latest.revision: 61
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 436e09d9639d7c90c4561833952e4f1efb21ce0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replication-management-objects-concepts"></a>Concepts liés à Replication Management Objects
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  RMO (Replication Management Objects) est un assembly de code managé qui encapsule les fonctionnalités de réplication pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. RMO est implémenté par l'espace de noms <xref:Microsoft.SqlServer.Replication>.  
  
 Les rubriques des sections suivantes indiquent comment utiliser RMO pour contrôler par programme les tâches de réplication :  
  
 [Configurer la distribution](../../../relational-databases/replication/configure-distribution.md)  
 Les rubriques de cette section indiquent comment utiliser RMO pour configurer la publication et la distribution.  
  
 [Créer, modifier et supprimer des publications et des articles &#40;réplication&#41;](../../../relational-databases/replication/publish/create-modify-and-delete-publications-and-articles-replication.md)  
 Les rubriques de cette section indiquent comment utiliser RMO pour créer, supprimer et modifier des publications et des articles.  
  
 [S'abonner à des publications](../../../relational-databases/replication/subscribe-to-publications.md)  
 Les rubriques de cette section indiquent comment utiliser RMO pour créer, supprimer et modifier des abonnements.  
  
 [Sécuriser une topologie de réplication](../../../relational-databases/replication/security/secure-a-replication-topology.md)  
 Les rubriques de cette section indiquent comment utiliser RMO pour afficher et modifier des paramètres de sécurité.  
  
 [Synchroniser des abonnements &#40;réplication&#41;](../../../relational-databases/replication/synchronize-subscriptions-replication.md)  
 Les rubriques de cette section indiquent comment synchroniser des abonnements.  
  
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
 Les rubriques de cette section indiquent comment analyser par programme une topologie de réplication.  
  
## <a name="introduction-to-rmo-programming"></a>Présentation de la programmation RMO  
 RMO est conçu pour la programmation de tous les aspects de la réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'espace de noms RMO est <xref:Microsoft.SqlServer.Replication>, et il est implémenté par le fichier Microsoft.SqlServer.Rmo.dll, qui est un assembly [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. L'assembly Microsoft.SqlServer.Replication.dll, qui appartient également à l'espace de noms <xref:Microsoft.SqlServer.Replication>, implémente une interface de code managé pour la programmation des différents agents de réplication (Agent d'instantané, Agent de distribution et Agent de fusion). Ses classes sont accessibles à partir de RMO pour synchroniser des abonnements. Les classes de l'espace de noms <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>, implémenté par l'assembly Microsoft.SqlServer.Replication.BusinessLogicSupport.dll, sont utilisées pour créer la logique métier personnalisée pour la réplication de fusion. Cet assembly est indépendant de RMO.  
  
## <a name="deploying-applications-based-on-rmo"></a>Déploiement d'applications basées sur RMO  
 RMO dépend des composants de réplication et des composants de connectivité client qui sont inclus dans toutes les versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sauf SQL Server Compact. Pour déployer une application basée sur RMO, vous devez installer une version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] incluant les composants de réplication et les composants de connectivité client sur l’ordinateur sur lequel l’application s’exécutera.  
  
## <a name="getting-started-with-rmo"></a>Mise en route avec RMO  
 Cette section décrit comment démarrer un projet RMO simple à l'aide de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio.  
  
#### <a name="to-create-a-new-microsoft-visual-c-project"></a>Pour créer un projet Microsoft Visual C#  
  
1.  Démarrez Visual Studio.  
  
2.  Dans le menu **Fichier**, cliquez sur **Nouveau projet**. La boîte de dialogue **Nouveau projet** s'affiche.  
  
3.  Dans la boîte de dialogue **Types de projets**, sélectionnez **Projets Visual C#**. Dans le volet **Modèles**, sélectionnez **Application Windows**.  
  
4.  (Facultatif) Dans la zone **Nom**, tapez le nom de la nouvelle application.  
  
5.  Cliquez sur **OK** pour charger le modèle Windows Visual C#.  
  
6.  Dans le menu **Projet**, sélectionnez l’élément **Ajouter une référence**. La boîte de dialogue **Ajouter une référence** s’affiche.  
  
7.  Sélectionnez les assemblys suivants dans la liste de l’onglet **.NET**, puis cliquez sur **OK**.  
  
    -   Interface de programmation .NET Microsoft.SqlServer.Replication  
  
    -   Microsoft.SqlServer.ConnectionInfo  
  
    -   Bibliothèque de l'Agent de réplication  
  
    > [!NOTE]  
    >  Utilisez la touche CTRL pour sélectionner plusieurs fichiers.  
  
8.  (Facultatif) Répétez l'étape 6. Cliquez sur l’onglet **Parcourir**, accédez à [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM, sélectionnez Microsoft.SqlServer.Replication.BusinessLogicSupport.dll, puis cliquez sur **OK**.  
  
9. Dans le menu **Affichage** , cliquez sur **Code**.  
  
10. Dans le code, avant l’instruction d’espace de noms, tapez les instructions **using** suivantes pour qualifier les types dans les espaces de noms RMO :  
  
    ```  
    // These namespaces are required.  
    using Microsoft.SqlServer.Replication;  
    using Microsoft.SqlServer.Management.Common;  
    // This namespace is only used when creating custom business  
    // logic for merge replication.  
    using Microsoft.SqlServer.Replication.BusinessLogicSupport;   
    ```  
  
#### <a name="to-create-a-new-microsoft-visual-basic-net-project"></a>Pour créer un projet Microsoft Visual Basic .NET  
  
1.  Démarrez Visual Studio.  
  
2.  Dans le menu **Fichier**, sélectionnez **Nouveau projet**. La boîte de dialogue **Nouveau projet** s'affiche.  
  
3.  Dans le volet Types de projets, sélectionnez **Visual Basic**. Dans le volet Modèles, sélectionnez **Application Windows**.  
  
4.  (Facultatif) Dans la zone **Nom**, tapez le nom de la nouvelle application.  
  
5.  Cliquez sur **OK** pour charger le modèle Windows Visual Basic.  
  
6.  Dans le menu **Projet**, sélectionnez **Ajouter une référence**. La boîte de dialogue **Ajouter une référence** s’affiche.  
  
7.  Sélectionnez les assemblys suivants dans la liste de l’onglet **.NET**, puis cliquez sur **OK**.  
  
    -   Interface de programmation .NET Microsoft.SqlServer.Replication  
  
    -   Microsoft.SqlServer.ConnectionInfo  
  
    -   Bibliothèque de l'Agent de réplication  
  
    > [!NOTE]  
    >  Utilisez la touche CTRL pour sélectionner plusieurs fichiers.  
  
8.  (Facultatif) Répétez l'étape 6. Cliquez sur l’onglet **Parcourir**, accédez à [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM, sélectionnez Microsoft.SqlServer.Replication.BusinessLogicSupport.dll, puis cliquez sur **OK**.  
  
9. Dans le menu **Affichage** , cliquez sur **Code**.  
  
10. Dans le code, avant les déclarations, tapez les instructions **Imports** suivantes pour qualifier les types dans les espaces de noms RMO.  
  
    ```  
    ' These namespaces are required.  
    Imports Microsoft.SqlServer.Replication  
    Imports Microsoft.SqlServer.Management.Common  
    ' This namespace is only used when creating custom business  
    ' logic for merge replication.  
    Imports Microsoft.SqlServer.Replication.BusinessLogicSupport   
    ```  
  
## <a name="connecting-to-a-replication-server"></a>Connexion à un serveur de réplication  
 Pour les objets de programmation RMO, il est nécessaire qu'une connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] soit établie en utilisant une instance de la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection>. Cette connexion au serveur est établie indépendamment des objets de programmation RMO. Elle est ensuite transmise à l’objet RMO lors de la création de l’instance ou par assignation à la propriété `P:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContex`t de l’objet. De cette manière, un objet de programmation RMO et les instances d'objet de connexion peuvent être créés et gérés séparément, et un objet de connexion peut être réutilisé avec plusieurs objets de programmation RMO. Les règles suivantes s'appliquent aux connexions à un serveur de réplication :  
  
-   Toutes les propriétés de la connexion sont définies pour un objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> donné.  
  
-   Une connexion à chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit comporter son propre objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
-   L'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> est assigné à la propriété `P:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext` de l'objet de programmation RMO créé ou faisant l'objet d'un accès sur le serveur.  
  
-   La méthode <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> établit la connexion au serveur. Vous devez appeler cette méthode avant d'appeler une méthode qui accède au serveur sur un objet de programmation RMO à l'aide de la connexion.  
  
-   Dans la mesure où RMO et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Objects (SMO) utilisent tous deux la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> pour les connexions à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les objets RMO et SMO peuvent utiliser la même connexion. Pour plus d’informations, consultez [Connexion à une instance de SQL Server](../../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md).  
  
-   Toutes les informations d'authentification nécessaires pour l'établissement de la connexion et l'ouverture d'une session sur le serveur sont fournies dans l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
-   La méthode par défaut est l'authentification Windows. Pour utiliser l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.LoginSecure%2A> doit avoir la valeur **false**, et <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Login%2A> et <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Password%2A> doivent être définis avec un nom d’ouverture de session et un mot de passe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] valides. Les informations d'identification de sécurité doivent toujours être stockées et gérées de manière sécurisée, et elles doivent être fournies au moment de l'exécution chaque fois que cela est possible.  
  
-   Pour les applications multithread, un objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> distinct doit être utilisé dans chaque thread.  
  
 Appelez la méthode <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Disconnect%2A> sur l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> pour fermer les connexions au serveur actives utilisées par les objets RMO.  
  
## <a name="setting-rmo-properties"></a>Définition des propriétés RMO  
 Les propriétés d'objets de programmation RMO représentent les propriétés de ces objets de réplication sur le serveur. Lorsque des objets de réplication sont créés sur le serveur, les propriétés RMO sont utilisées pour définir ces objets. Pour les objets existants, les propriétés RMO représentent les propriétés de l'objet existant, qui peuvent être modifiées uniquement pour les propriétés accessibles en écriture ou définissables. Les propriétés peuvent être définies sur les nouveaux objets ou les objets existants.  
  
### <a name="setting-properties-for-new-replication-objects"></a>Définition des propriétés des nouveaux objets de réplication  
 Quand vous créez un objet de réplication sur le serveur, vous devez spécifier toutes les propriétés requises avant d’appeler la méthode **Create** de l’objet. Pour plus d’informations sur la définition des propriétés d’un nouvel objet de réplication, consultez [Configurer la publication et la distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
### <a name="setting-properties-for-existing-replication-objects"></a>Définition des propriétés des objets de réplication existants  
 Pour un objet de réplication qui réside sur le serveur, en fonction de l'objet, RMO peut être en mesure de modifier certaines de ses propriétés ou toutes ses propriétés. Seules les propriétés accessibles en écriture ou définissables peuvent être modifiées. Avant de modifier les propriétés, vous devez appeler **Load** ou la méthode `M:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties` pour obtenir les propriétés actuelles auprès du serveur. L'appel de ces méthodes indique qu'un objet existant fait l'objet d'une modification.  
  
 Par défaut, lors de la modification des propriétés d'un objet, RMO valide ces modifications sur le serveur selon le mode d'exécution de l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> utilisé. La propriété `P:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject` peut être utilisée pour vérifier qu'un objet existe sur le serveur avant la tentative d'extraction ou de modification de ses propriétés. Pour plus d’informations sur la modification des propriétés d’un objet de réplication, consultez [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
> [!NOTE]  
>  Quand plusieurs clients RMO ou plusieurs instances d’un objet de programmation RMO accèdent au même objet de réplication sur le serveur, la méthode **Refresh** de l’objet RMO peut être appelée pour mettre à jour les propriétés en fonction de l’état actuel de l’objet sur le serveur.  
  
### <a name="caching-property-changes"></a>Mise en cache de modifications de propriété  
 Lorsque la propriété <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes> a la valeur <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes.CaptureSql>, toutes les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] générées par RMO sont capturées afin qu'elles puissent être exécutées manuellement dans un lot unique au moyen de l'une des méthodes d'exécution. RMO vous permet de mettre en cache les modifications de propriété et de les valider globalement dans un lot unique au moyen de la méthode `M:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges` de l'objet. Pour la mise en cache des modifications de propriété, la propriété `P:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges` de l’objet doit avoir la valeur **true**. Lors de la mise en cache des modifications de propriété dans RMO, l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> détermine toujours le moment auquel les modifications sont envoyées au serveur. Pour plus d’informations sur la mise en cache des modifications de propriété pour un objet de réplication, consultez [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
> [!IMPORTANT]  
>  Bien que la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> prenne en charge la déclaration de transactions explicites lors de la définition des propriétés, ces transactions peuvent interférer avec les transactions de réplication internes et produire des résultats inattendus. Par conséquent, elles ne doivent pas être utilisées avec RMO.  

### <a name="enabling-tls-12-support-for-rmo-components"></a>Activation de la prise en charge de TLS 1.2 pour les composants RMO 
 Vous pouvez activer la prise en charge de TLS 1.2 pour les composants RMO sur Windows 2012 et versions antérieures en installant la mise à jour [KB 3140245](http://support.microsoft.com/help/3140245) et en créant des clés de Registre, comme indiqué dans l’article. Sur Windows 2012 R2 et versions ultérieures, seules doivent être créées les clés de Registre telles qu’indiquées dans l’article ci-dessus.
 
## <a name="example"></a> Exemple  
 Cet exemple illustre la mise en cache des modifications de propriété. Les modifications apportées aux attributs d'une publication transactionnelle sont mises en cache tant qu'elles n'ont pas été envoyées explicitement au serveur.  
  
 [!code-cs[HowTo#rmo_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changetranpub_cached)]  
  
## <a name="see-also"></a> Voir aussi  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Concepts de programmation en matière de réplication](../../../relational-databases/replication/concepts/replication-programming-concepts.md)  
  
  
