---
title: L’Assistant Migration (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Migration Wizard dialog box
- Migration Wizard, adding Access databases
- Migration Wizard, Connect to SQL Azure
- Migration Wizard, Connect to SQL Server
- Migration Wizard, Link Tables
- Migration Wizard, Migration status
- Migration Wizard, New Project
- Migration Wizard, Selecting objects to migrate
ms.assetid: 5bab5914-b2ae-4795-8cf5-83e42d64bef2
caps.latest.revision: 22
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 356f06ec66e0fa18c0406f34ce706eae0eaf2886
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migration-wizard-accesstosql"></a>Assistant Migration (AccessToSQL)
L’Assistant Migration vous guide à travers la migration d’une ou plusieurs bases de données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. À l’aide de l’Assistant, vous serez créer un projet, ajouter des bases de données au projet, sélectionnez les objets à migrer, puis se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Vous serez également convertir, charger et migrer les données et les schémas d’accès. Si vous le souhaitez, vous pouvez lier les tables de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] les tables ou SQL Azure.  
  
La plupart des pages de l’Assistant de Migration contiennent les mêmes options que les boîtes de dialogue SSMA existantes. Par conséquent, les pages de l’Assistant sont décrites ici, et ensuite des liens sont fournis afin que vous pouvez en savoir plus sur les options individuelles. Si une page contient des options, ils sont décrits ici.  
  
## <a name="starting-the-migration-wizard"></a>Démarrage de l’Assistant Migration  
Par défaut, l’Assistant de Migration s’affiche lorsque vous démarrez SSMA. Vous pouvez également démarrer l’Assistant sur le **fichier** menu en sélectionnant **Assistant Migration**.  
  
## <a name="welcome-page"></a>Page d'accueil  
La page d’accueil présente l’Assistant de Migration et fournit l’option suivante pour démarrer l’Assistant.  
  
**Lancez cet Assistant au démarrage.**  
Par défaut, SSMA démarre l’Assistant de Migration lorsque vous démarrez SSMA. Pour empêcher le démarrage automatique de l’Assistant, désactivez cette case à cocher.  
  
## <a name="create-new-project-page"></a>Créer la nouvelle Page de projet  
La page Créer un nouveau projet est où vous entrez le nom, l’emplacement et la migration projet type de fichier projet (la version de SQL Server utilisée pour la migration de cible). Pour plus d’informations, consultez [nouveau projet (SSMA)](http://msdn.microsoft.com/en-us/ca294f6d-eeb5-42ca-9306-156281a3f0f3)  
  
## <a name="add-access-databases-page"></a>Ajouter la Page d’accès aux bases de données  
La page Ajouter des bases de données Access est où vous ajoutez une ou plusieurs bases de données Access au projet. Vous pouvez ajouter des bases de données en cliquant sur **ajouter les bases de données**, puis en sélectionnant les bases de données à partir de la **ouvrir** fenêtre. Ou, vous pouvez rechercher des bases de données à l’aide de la **trouver les bases de données** bouton. Pour plus d'informations, consultez les rubriques suivantes :  
  
-   [Ajout et suppression de fichiers de base de données Access](http://msdn.microsoft.com/en-us/e944c740-4c8a-4bc1-b0ed-be57bc06dced)  
  
-   [Assistant de bases de données (sélectionnez emplacements) de la recherche](http://msdn.microsoft.com/en-us/00b2d32a-998b-47a7-b25c-589b5bd6777a)  
  
-   [Assistant de bases de données (fichiers Select) de la recherche](http://msdn.microsoft.com/en-us/2f574a34-4bab-40a4-89a8-ad4907ffc3fd)  
  
-   [Assistant Rechercher des bases de données (Vérifier la sélection)](http://msdn.microsoft.com/en-us/62e20e03-50cc-4ac8-8072-524d194d2ec3)  
  
## <a name="select-objects-to-migrate-page"></a>Sélectionnez les objets à migrer les pages  
Sur les objets de sélectionner à la page de la migration, vous sélectionnez des objets à convertir. Vous pouvez sélectionner tous les objets, groupes d’objets ou des objets individuels.  
  
**Pour sélectionner des objets**  
  
1.  Développez **accès à la métabase**, puis développez **bases de données**.  
  
2.  Effectuez une ou plusieurs des opérations suivantes :  
  
    -   Pour convertir toutes les bases de données, sélectionnez la case à cocher à côté **bases de données**.  
  
    -   Pour convertir ou omettre des bases de données, activez ou désactivez la case à cocher en regard du nom de la base de données.  
  
    -   Pour convertir ou omettre des requêtes, développez la base de données, puis activez ou désactivez le **requêtes** case à cocher.  
  
    -   Pour convertir ou omettre des tables individuelles, développez la base de données, **Tables**, puis activez ou désactivez la case à cocher en regard de la table.  
  
Si vous avez de nombreux objets, vous souhaiterez utiliser le **sélection avancée de l’objet** options dans le volet droit pour filtrer l’accès des objets de base de données. Par exemple, si vous sélectionnez **Tables** dans le volet gauche, vous pouvez ensuite filtrer la liste des tables en entrant des chaînes dans le **filtre** boîte. Vous pouvez ensuite sélectionner ou effacer les tables filtrées pour la migration en utilisant les boutons en haut du volet.  
  
Pour plus d’informations sur le filtrage, consultez la section Options de [objet sélection avancée (SSMA commun)](http://msdn.microsoft.com/en-us/f53b0c79-5473-410a-a0dc-d8f544f7a63c).  
  
## <a name="connect-to-sql-server-page"></a>Se connecter à la Page de SQL Server  
Sur la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] page, vous spécifiez les propriétés de connexion, puis connectez-vous à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations, consultez [se connecter à SQL Server](http://msdn.microsoft.com/en-us/00e0432e-ec26-4ab4-af64-c9ca760e3541)  
  
> [!IMPORTANT]  
> Dès que la connexion réussit, vous rencontrerez **lier les Tables** page où vous avez la possibilité de lier les tables. Cliquez sur **suivant** et commence à la migration.  
  
## <a name="connect-to-sql-azure-page"></a>Se connecter à SQL Azure Page  
Sur la page connexion à SQL Azure, vous spécifiez les propriétés de connexion et puis se connectez à SQL Azure. Pour créer une nouvelle base de données azure, vous pouvez le faire à l’aide de **créer la base de données Azure** option qui apparaît sur le clic de **Parcourir** bouton. Pour plus d’informations, consultez [se connecter à SQL Azure](http://msdn.microsoft.com/en-us/bf44b236-d9be-41ae-a5fd-bd73038e505f)  
  
> [!IMPORTANT]  
> Dès que la connexion réussit, vous rencontrerez **lier les Tables** page où vous avez la possibilité de lier les tables. Cliquez sur **suivant** bouton sur la page de liens pour démarrer la migration.  
  
## <a name="link-tables-page"></a>Page des Tables de lien  
La page de lier les Tables vous permet de lier vos tables Access d’origine à l’objet d’une migration [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des tables SQL Azure. Liaison de tables modifie votre base de données Access afin que vos pages d’accès aux requêtes, formulaires, rapports et données utilisent les données de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de la base de données SQL Azure au lieu des données dans votre base de données Access.  
  
**Lier les tables**  
Sélectionnez le **liez des tables** case à cocher pour lier des tables de l’accès à migrées [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des tables SQL Azure. Pour démarrer la migration, vous devez cliquer sur **suivant** bouton.  
  
## <a name="migration-status-page"></a>Page d’état de migration  
La page de l’état de Migration affiche la progression de la conversion les schémas d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des schémas de SQL Azure, le chargement des schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, et la migration des données.  
  
Pour plus d’informations sur cette page, consultez [convertir, charger et migrer](http://msdn.microsoft.com/en-us/4ec83e96-88a5-4b7b-8d5a-f3429d9a936b)  
  
## <a name="see-also"></a>Voir aussi  
[Prise en main de SQL Server Migration Assistant pour Access &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
[Reference(Access) d’Interface utilisateur](http://msdn.microsoft.com/en-us/af24c303-4a41-449b-9c86-d6558a97e839)  
  
