---
title: Conversion des bases de données MySQL (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
ms.assetid: ac21850b-fb32-4704-9985-5759b7c688c7
caps.latest.revision: 17
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: a1925166a672523f6e4cf5eef7cdc0bc5235c3fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="converting-mysql-databases-mysqltosql"></a>Conversion des bases de données MySQL (MySQLToSQL)
Après vous être connecté à MySQL, connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure et définir le projet et les options de mappage de données, vous pouvez convertir des objets de base de données MySQL à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de base de données SQL Azure.  
  
## <a name="the-conversion-process"></a>Le processus de Conversion  
Convertir des objets de base de données accepte les définitions d’objets à partir de MySQL, les convertit en similaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure objets et charge ensuite ces informations dans les métadonnées SSMA. Il ne charge pas les informations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez ensuite afficher les objets et leurs propriétés à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou de l’Explorateur de métadonnées SQL Azure.  
  
Lors de la conversion, SSMA imprime la sortie des messages vers le volet de sortie et messages d’erreur dans le volet de la liste d’erreurs. Utilisez les informations de sortie et d’erreur pour déterminer si vous devez modifier vos bases de données MySQL ou votre processus de conversion pour obtenir les résultats de la conversion souhaitée.  
  
## <a name="setting-conversion-options"></a>Définition des Options de Conversion  
Avant de convertir les objets, vérifiez les options de conversion de projet dans le **les paramètres de projet** boîte de dialogue. À l’aide de cette boîte de dialogue, vous pouvez définir comment SSMA convertit les tables et les index. Pour plus d’informations, consultez [les paramètres de projet &#40;Conversion&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
## <a name="conversion-results"></a>Résultats de la conversion  
Le tableau suivant présente les objets de MySQL sont convertis et résultant [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] objets :  
  
|||  
|-|-|  
|**Objets de MySQL**|**Résultant d’objets SQL Server**|  
|Tables avec des objets dépendants, tels que les index|SSMA crée des tables avec des objets dépendants. Table est convertie avec tous les index et contraintes. Les index sont convertis en distinct [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] objets.<br /><br />**Mappage de type de données spatiales** peut être effectuée uniquement au niveau de nœud de table.<br /><br />Pour plus d’informations sur les paramètres de Conversion de la Table, consultez [les paramètres de Conversion](http://msdn.microsoft.com/en-us/f551cf6e-1575-4206-9cca-975b5b43a6b8)|  
|Fonctions|Si la fonction peut être directement convertie en Transact-SQL, SSMA crée une fonction. Dans certains cas, la fonction doit être convertie en une procédure stockée. Cela est possible à l’aide de **fonction Conversion** dans Paramètres du projet. Dans ce cas, SSMA crée une procédure stockée et une fonction qui appelle la procédure stockée.<br /><br />**Choix :**<br /><br />Convertir en fonction des paramètres de projet<br /><br />Convertir (fonction)<br /><br />Conversion d’une procédure stockée<br /><br />Pour plus d’informations sur les paramètres de Conversion de la fonction, consultez [les paramètres de Conversion](http://msdn.microsoft.com/en-us/f551cf6e-1575-4206-9cca-975b5b43a6b8)|  
|Procédures|Si la procédure peut être directement convertie en Transact-SQL, SSMA crée une procédure stockée. Dans certains cas, une procédure stockée doit être appelée dans une transaction autonome. Dans ce cas, SSMA crée deux procédures stockées : celui qui implémente la procédure et l’autre est utilisé pour appeler l’implémentation d’une procédure stockée.|  
|Conversion de la base de données|Bases de données en tant qu’objets de MySQL ne sont pas converties directement en SSMA pour MySQL. Bases de données MySQL sont plus traitées comme un nom de schéma et tous les paramètres physiques sont perdues lors de la conversion. SSMA pour MySQL utilise [mappage de bases de données MySQL pour les schémas SQL Server &#40;MySQLToSQL&#41; ](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md) pour mapper des objets de base de données MySQL à une paire de schéma/base de données SQL Server approprié.|  
|Conversion de déclencheur|**SSMA crée des déclencheurs selon les règles suivantes :**<br /><br />AVANT la conversion des déclencheurs dans les déclencheurs INSTEAD OF T-SQL<br /><br />Déclencheurs AFTER sont convertis en déclencheurs après T-SQL avec ou sans itérations par lignes.|  
|Conversion de la vue|SSMA crée des vues avec les objets dépendants|  
|Conversion de l’instruction|-Chaque objet de l’instruction SQL peut contenir une seule instruction MySQL (telles que DDL, DML et autres types d’instructions) ou BEGIN... Bloc de fin.<br />-   **Conversion à instructions multiples : BEGIN... Conversion de bloc de fin**instruction SQL peut également contenir un BEGIN... Bloc de fin comme un dans la définition de procédure, fonction ou un déclencheur. Les blocs doivent être converties de la même façon qu’ils sont en cours de conversion pour les objets d’instruction MySQL uniques.|  
  
## <a name="converting-mysql-database-objects"></a>Convertir des objets de base de données MySQL  
Pour convertir des objets de base de données MySQL, vous tout d’abord sélectionnez les objets que vous souhaitez convertir et puis SSMA effectuer la conversion. Pour afficher les messages de sortie lors de la conversion, sur le **vue** menu, sélectionnez **sortie**.  
  
**Pour convertir des objets de MySQL à la syntaxe SQL Server ou SQL Azure**  
  
1.  Dans l’Explorateur de métadonnées MySQL, MySQL server, puis **bases de données**.  
  
2.  Sélectionner les objets à convertir :  
  
    -   Pour convertir tous les schémas, sélectionnez la case à cocher à côté **bases de données**.  
  
    -   Pour convertir ou omettre une base de données, sélectionnez la case à cocher en regard du nom de la base de données.  
  
    -   Pour convertir ou omettre une catégorie d’objets, développez un schéma, puis sélectionnez ou désactivez la case à cocher en regard de la catégorie.  
  
    -   Pour convertir ou omettre des objets, développez le dossier de la catégorie, puis sélectionnez ou désactivez la case à cocher en regard de l’objet.  
  
3.  Pour convertir tous les objets sélectionnés, cliquez sur **bases de données** et sélectionnez **convertir un schéma**.  
  
    Vous pouvez également convertir des objets individuels ou des catégories d’objets en cliquant sur l’objet ou son dossier parent, puis en sélectionnant **convertir un schéma**.  
  
## <a name="viewing-conversion-problems"></a>Affichage des problèmes de Conversion  
Certains objets MySQL ne peuvent pas être converties. Vous pouvez déterminer le taux de réussite de conversion en affichant le rapport de synthèse de conversion.  
  
**Pour afficher un rapport de synthèse**  
  
1.  Dans l’Explorateur de métadonnées MySQL, sélectionnez **bases de données**.  
  
2.  Dans le volet droit, sélectionnez le **rapport** onglet.  
  
    Ce rapport affiche le rapport d’évaluation de synthèse pour tous les objets de base de données qui ont été évaluée ou converti. Vous pouvez également afficher un rapport de synthèse pour des objets :  
  
    -   Pour afficher le rapport pour un schéma individuel, sélectionnez la base de données dans l’Explorateur de métadonnées de MySQL.  
  
    -   Pour afficher le rapport pour un objet, sélectionnez l’objet dans l’Explorateur de métadonnées MySQL. Les objets qui ont des problèmes de conversion ont une icône d’erreur rouge.  
  
Pour les objets en échec de conversion, vous pouvez afficher la syntaxe qui a entraîné l’échec de la conversion.  
  
**Pour afficher les problèmes de conversion individuels**  
  
1.  Dans l’Explorateur de métadonnées MySQL, développez **bases de données**.  
  
2.  Développez la base de données qui affiche une icône d’erreur rouge.  
  
3.  Sous la base de données, développez un dossier qui a une icône d’erreur rouge.  
  
4.  Sélectionnez l’objet qui a une icône d’erreur rouge.  
  
5.  Dans le volet droit, cliquez sur le **rapport** onglet.  
  
6.  En haut de la **rapport** onglet est une liste déroulante. Si la liste affiche **statistiques**, modifiez la sélection à **Source**.  
  
    SSMA affichera le code source et plusieurs boutons immédiatement au-dessus du code.  
  
7.  Cliquez sur le **problème suivant** bouton. Il s’agit d’une icône d’erreur rouge avec une flèche pointant vers la droite.  
  
    SSMA met en surbrillance le premier code source problématiques qu’elle trouve dans l’objet actuel.  
  
Pour chaque élément ne peut pas être converti, vous devez déterminer ce que vous voulez faire avec cet objet :  
  
-   Vous pouvez modifier l’objet dans la base de données MySQL pour supprimer ou modifier tout code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
-   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou l’Explorateur de métadonnées SQL Azure et l’Explorateur de métadonnées MySQL, désactivez la case à cocher en regard de l’élément avant de charger les objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure et la migration des données de MySQL.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration est [le chargement des objets de base de données convertie dans SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
