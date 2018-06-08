---
title: Lier des Applications d’accès à SQL Server - base de données SQL Azure | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Access databases, linking to SQL Azure
- Access databases, linking to SQL Server
- auto-increment columns
- data types, unsupported
- hyperlink columns
- linking tables
- migrating databases, post-migration issues
- post-migration issues
- reference, post-migration issues
- refreshing linked tables
- slow performance
- unlinking tables
ms.assetid: 82374ad2-7737-4164-a489-13261ba393d4
caps.latest.revision: 19
author: Shamikg
ms.author: Shamikg
manager: murato
ms.openlocfilehash: bcfaa4ffcaaf8a621062f0809831437647b894fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="linking-access-applications-to-sql-server---azure-sql-db-accesstosql"></a>Lier des applications d’accès à SQL Server - base de données SQL Azure (AccessToSQL)
Si vous souhaitez utiliser vos applications Access existantes avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez lier vos tables Access d’origine à l’objet d’une migration [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des tables SQL Azure. La liaison modifie la base de données Access afin que vos pages d’accès aux requêtes, formulaires, rapports et données utilisent les données de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de la base de données SQL Azure au lieu des données dans votre base de données Access.  
  
> [!NOTE]  
> Vos tables Access restent dans Access, mais ne sont pas mis à jour avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou met à jour de SQL Azure. Une fois que vous liez les tables et que vous vérifiez la fonctionnalité, vous souhaiterez supprimer vos tables Access.  
  
## <a name="linking-access-and-sql-server-tables"></a>Liez les tables Access et SQL Server  
Lorsque vous liez une table de l’accès à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, le moteur de base de données Jet stocke des informations de connexion et les métadonnées de la table, mais les données sont stockées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Cette association permet à vos applications Access fonctionnent sur les tables de l’accès, même si les tables réelles et les données sont dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
> [!NOTE]  
> Si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, votre mot de passe est stockée en texte clair sur les tables Access liés. Nous recommandons d’utiliser l’authentification Windows.  
  
**Pour lier des tables**  
  
1.  Dans l’Explorateur de métadonnées de l’accès, sélectionnez les tables que vous souhaitez lier.  
  
2.  Avec le bouton droit **Tables**, puis sélectionnez **lien**.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour Access sauvegarde de la table d’accès d’origine et crée une table liée.  
  
Après avoir lié les tables, les tables de SSMA apparaissent avec une icône de lien petit. Dans Access, les tables apparaissent avec une icône « liée », qui est un globe avec une flèche pointant vers lui.  
  
Lorsque vous ouvrez une table dans Access, les données sont récupérées à l’aide d’un curseur keyset. Par conséquent, pour les grandes tables, toutes les données ne sont récupérées en même temps. Toutefois, lorsque vous parcourez la table, Access extrait les données supplémentaires si nécessaire.  
  
> [!IMPORTANT]  
> Pour lier des tables access avec une base de données Azure, vous devez Client(SNAC) Native de SQL Server version 10.5 ou version ultérieure.   
> Vous pouvez obtenir la dernière version de SNAC de [Microsoft® SQL Server® 2008 R2 Feature Pack](http://go.microsoft.com/fwlink/?LinkId=196940).  
  
## <a name="unlinking-access-tables"></a>Suppression de la liaison accéder aux tables  
Lorsque vous supprimez le lien d’une table Access à partir d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de la table SQL Azure, SSMA restaure la table d’accès d’origine et ses données.  
  
**Pour supprimer la liaison de tables**  
  
1.  Dans l’Explorateur de métadonnées de l’accès, sélectionnez les tables que vous souhaitez dissocier.  
  
2.  Avec le bouton droit **Tables**, puis sélectionnez **dissocier**.  
  
## <a name="linking-tables-to-a-different-server"></a>Liaison des tables vers un autre serveur  
Si vous avez lié des tables de l’accès à une instance de SQL Server et que vous souhaitez ultérieurement modifier les liens vers une autre instance, vous devez relier les tables.  
  
**Pour lier des tables à un autre serveur**  
  
1.  Dans l’Explorateur de métadonnées de l’accès, sélectionnez les tables que vous souhaitez dissocier.  
  
2.  Avec le bouton droit **Tables** , puis sélectionnez **dissocier**.  
  
3.  Cliquez sur le **se reconnecter à SQL Server** bouton.  
  
4.  Connectez-vous à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure à laquelle vous souhaitez lier les tables Access.  
  
5.  Dans l’Explorateur de métadonnées de l’accès, sélectionnez les tables que vous souhaitez lier.  
  
6.  Avec le bouton droit **Tables**, puis sélectionnez **lien**.  
  
## <a name="updating-linked-tables"></a>Mise à jour des tables liées  
Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou modifier des définitions de table SQL Azure, vous pouvez supprimer la liaison et puis reconnectez les tables de SSMA en procédant comme indiqué précédemment dans cette rubrique. Vous pouvez également mettre à jour les tables à l’aide de l’accès.  
  
**Pour mettre à jour des tables liées à l’aide de l’accès**  
  
1.  Ouvrez la base de données Access.  
  
2.  Dans le **objets** , cliquez sur **Tables**.  
  
3.  Cliquez sur une table liée, puis sélectionnez **Gestionnaire de tables liées**.  
  
4.  Activez la case à cocher en regard de chaque table liée que vous souhaitez mettre à jour, puis cliquez sur **OK**.  
  
## <a name="possible-post-migration-issues"></a>Éventuels problèmes après la migration  
Les sections suivantes les problèmes de liste qui peuvent se produire dans les applications Access existantes après la migration des bases de données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, puis lier les tables, ainsi que les causes et les résolutions.  
  
### <a name="slow-performance-with-linked-tables"></a>Ralentissement des performances avec les tables liées  
**Cause :** certaines requêtes peuvent être lentes après la migration pour les raisons suivantes :  
  
-   L’application dépend des fonctions qui n’existent pas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, ce qui amène Jet extraire des tables localement pour exécuter une requête SELECT.  
  
-   Requêtes update ou delete de nombreuses lignes sont envoyées par Jet comme une requête paramétrable pour chaque ligne.  
  
**Résolution :** convertir les requêtes à exécution lente des requêtes directes, des procédures stockées ou des vues. Convertir les requêtes pass-through présente les problèmes suivants :  
  
-   Requêtes directes ne peut pas être modifiés. Modification des résultats de la requête ou l’ajout de nouveaux enregistrements doit être effectuées dans une autre façon, comme en ayant explicite **modifier** ou **ajouter** boutons sur votre formulaire est lié à la requête.  
  
-   Certaines requêtes nécessitent l’entrée d’utilisateur, mais les requêtes pass-through ne prennent pas en charge l’entrée d’utilisateur. L’entrée d’utilisateur peut être obtenue en Visual Basic pour Applications (VBA) qui vous invite à entrer des paramètres, ou par un formulaire qui est utilisé comme un contrôle d’entrée. Dans les deux cas, le code VBA soumet la requête avec l’entrée d’utilisateur sur le serveur.  
  
### <a name="auto-increment-columns-are-not-updated-until-the-record-is-updated"></a>Colonnes à incrémentation automatique ne sont pas mis à jour jusqu'à ce que l’enregistrement est mis à jour.  
**Cause :** après avoir appelé RecordSet.AddNew dans Jet, la colonne à incrémentation automatique est disponible avant l’enregistrement est mis à jour. Cela n’est pas vrai dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. La nouvelle valeur de la valeur nouvelle colonne d’identité est disponible uniquement après l’enregistrement du nouvel enregistrement.  
  
**Résolution :** exécuter Visual Basic pour Applications (VBA) suivant avant d’accéder au champ d’identité :  
  
```  
Recordset.Update  
Recordset.Move 0,  
Recordset.LastModified  
```  
  
### <a name="new-records-are-not-available"></a>Nouveaux enregistrements ne sont pas disponibles  
**Cause :** lorsque vous ajoutez un enregistrement à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une table SQL Azure à l’aide de VBA, si le champ de table unique index a une valeur par défaut et que vous n’affectez pas une valeur de ce champ, le nouvel enregistrement n’apparaît pas tant que vous rouvrez le tableau dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou S SQL Azure. Si vous essayez d’obtenir une valeur de l’enregistrement, le message d’erreur suivant s’affiche :  
  
`Run-time error '3167' Record is deleted.`  
  
**Résolution :** lorsque vous ouvrez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure table à l’aide du code VBA, incluez la `dbSeeChanges` option, comme dans l’exemple suivant :  
  
`Set rs = db.OpenRecordset("TestTable", dbOpenDynaset, dbSeeChanges)`  
  
### <a name="after-migration-some-queries-will-not-allow-the-user-to-add-a-new-record"></a>Après la migration, certaines requêtes n’autorisera pas l’utilisateur d’ajouter un nouvel enregistrement  
**Cause :** si une requête n’inclut pas toutes les colonnes qui sont inclus dans un index unique, vous ne pouvez pas ajouter de nouvelles valeurs à l’aide de la requête.  
  
**Résolution :** garantir que toutes les colonnes incluses dans au moins un index unique font partie de la requête.  
  
### <a name="you-cannot-modify-a-linked-table-schema-with-access"></a>Vous ne pouvez pas modifier un schéma de table liée avec accès  
**Cause :** après la migration des données et la liaison des tables, l’utilisateur ne peut pas modifier le schéma d’une table dans Access.  
  
**Résolution :** modifier le schéma de table à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], puis mettez à jour le lien dans Access.  
  
### <a name="hyperlink-functionality-is-lost-after-migrating-data"></a>Fonctionnalité de lien hypertexte est perdue après la migration des données  
**Cause :** après la migration des données, des liens hypertexte dans les colonnes perdent leur fonctionnalité et devenir simples **nvarchar (max)** colonnes.  
  
**Résolution :** None.  
  
### <a name="some-sql-server-data-types-are-not-supported-by-access"></a>Certains types de données SQL Server ne sont pas pris en charge par Access  
**Cause :** si vous mettez à jour ultérieurement votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des tables SQL Azure pour contenir les types de données qui ne sont pas pris en charge par l’accès, vous ne peut pas ouvrir la table dans Access.  
  
**Résolution :** vous pouvez définir une requête d’accès qui renvoie uniquement les lignes avec des types de données pris en charge.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
