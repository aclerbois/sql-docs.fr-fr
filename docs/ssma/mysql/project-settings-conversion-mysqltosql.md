---
title: Paramètres (Conversion) (MySQLToSQL) du projet | Documents Microsoft
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
ms.assetid: 7ad5fe44-6445-4ba8-a457-5af792631f11
caps.latest.revision: 19
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d9f35b745693caf0270638a7590f74de7e1ff875
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-conversion-mysqltosql"></a>Paramètres du projet (Conversion) (MySQLToSQL)
La page de Conversion de la **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA convertit la syntaxe de MySQL à la syntaxe SQL Server ou SQL Azure.  
  
Le volet de la Conversion est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Utilisez le **les paramètres de projet par défaut** boîte de dialogue pour définir les options de configuration pour tous les projets. Pour accéder aux paramètres de conversion, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichés / a été remplacée par **Version cible de la Migration** liste déroulante, cliquez sur **général** en bas du volet gauche, puis **Conversion**.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, cliquez sur **les paramètres de projet**, puis cliquez sur **général** au bas de la gauche, puis cliquez sur **Conversion**.  
  
## <a name="options"></a>Options  
  
### <a name="collate-clause"></a>Collate, Clause  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de la clause COLLATE explicite**|Option de conversion de clause COLLATE explicite spécifie comment convertir des clauses COLLATE explicites dans le code de MySQL. Les choix possibles : Ignorer et marquez un message d’avertissement / génèrent une erreur<br /><br />**Mode par défaut**: ignorer et d’interrogation avec un avertissement<br /><br />**Mode optimisé**: ignorer et d’interrogation avec un avertissement<br /><br />**Mode plein**: ignorer et d’interrogation avec un avertissement|  
  
### <a name="column-constraints"></a>Contraintes de colonne  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Générer de contrainte pour les colonnes de type de données ENUM**|Génère la contrainte pour les colonnes de type de données ENUM dans la table SQL Server ou SQL Azure, si elle n’est pas présent dans la table de MySQL. Si Oui, toutes les colonnes converties ENUM du type de données seront associées avec contrôle de la valeur de contrainte de validation.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
|**Générer de contrainte pour les colonnes de type de jeu de données**|Génère la contrainte pour les colonnes de type de données défini dans la table SQL Server ou SQL Azure, si elle n’est pas présent dans la table de MySQL. Si Oui, toutes les colonnes converties du type de données défini seront associées avec contrôle de la valeur de contrainte de validation.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
|**Générer de contrainte pour les colonnes de colonnes de type de données numérique non signé**|Ajouter la vérification de valeur non négative à des colonnes de types de données numériques non signé.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
|**Générer de contrainte pour les colonnes de type de données année**|Génère la contrainte pour les colonnes de type de données année dans la table SQL Server ou SQL Azure, si elle n’est pas présent dans la table de MySQL. Si Oui, tous convertis en colonnes de données de l’année type sera associée avec contrôle de la valeur de contrainte de validation.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
  
### <a name="data-types"></a>Types de données  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de types de données ENUM**|Spécifie comment le type de données MySQL ENUM doit être convertie en tant que de la convertir en NVARCHAR ou convertir en numérique<br /><br />**Mode par défaut**: convertir en NVARCHAR<br /><br />**Mode optimisé**: convertir en NVARCHAR<br /><br />**Mode plein**: convertir en NVARCHAR|  
|**Conversion de type de jeu de données**|Spécifie comment MySQL définir le type de données doit être converti, convertir NVARCHAR (L) / convertir BINARY(L)<br /><br />**Mode par défaut**: convertir NVARCHAR(L)<br /><br />**Mode optimisé**: convertir NVARCHAR(L)<br /><br />**Mode plein**: convertir NVARCHAR(L)|  
  
### <a name="generic"></a>Générique  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Colonnes sans valeur par défaut dans INSERT et remplacer**|Si Oui, toutes les instructions qui référencent des tables à l’aide de moteurs stockées autres que MyISAM et InnoDb doivent être marquées avec les messages d’avertissement conversion.<br /><br />**Mode par défaut**: ajouter à la liste des colonnes<br /><br />**Mode optimisé**: ajouter à la liste des colonnes<br /><br />**Mode plein**: ajouter à la liste des colonnes|  
|**Division par zéro génère de Conversion**|Spécifie d’émuler MySQL sans le comportement ERROR_FOR_DIVISION_BY_ZERO ou non.<br /><br />**Mode par défaut**: erreur<br /><br />**Mode optimisé**: erreur<br /><br />**Mode plein**: NULL|  
|**IN, opérateur**|Spécifie comment convertir opérateur MySQL dans.<br /><br />**Mode par défaut**: toujours convertir en<br /><br />**Mode optimisé**: toujours convertir en<br /><br />**Mode plein**: développez si nécessaire|  
|**Conversion de MySQL (fonction)**|Spécifie comment convertir des fonctions standards de MySQL.<br /><br />**Mode par défaut**: optimiste<br /><br />**Mode optimisé**: optimiste<br /><br />**Mode plein**: précis|  
|**Non pris en charge des moteurs de stockage**|Si Oui, toutes les instructions qui référencent des tables à l’aide de moteurs stockées autres que MyISAM et InnoDb doivent être marquées avec les messages d’avertissement conversion.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
|**Supprimer la génération de colonne auxiliaire ROWID**|Si Oui, interdit la création de la création de colonne auxiliaire ROWD sur les tables cibles. Peut affecter la migration de certaines structures.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: non|  
|**Conversion de l’instruction de troncation**|Spécifie comment convertir des instructions TRUNCATE.<br /><br />**Mode par défaut**: tronquer<br /><br />**Mode optimisé**: tronquer<br /><br />**Mode plein**: tronquer|  
  
### <a name="misc"></a>Divers  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Mappage de schéma par défaut**|Spécifie comment mapper des bases de données MySQL en schémas SQL Server.<br /><br />**Mode par défaut**: base de données à la base de données<br /><br />**Mode optimisé**: base de données à la base de données<br /><br />**Mode plein**: base de données à la base de données|  
  
### <a name="procedures-and-functions"></a>Procédures et fonctions  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Conversion de fonction par défaut**|Indique si les fonctions doivent être par défaut être converties pour les fonctions T-SQL ou des procédures stockées.<br /><br />**Mode par défaut**: convertir (fonction)<br /><br />**Mode optimisé**: convertir (fonction)<br /><br />**Mode plein**: convertir (fonction)|  
|**Générer un SET XACT_ABORT sur**|Spécifie si SET XACT_ABORT ON doit être ajouté au début de la procédure converti ou du déclencheur.<br /><br />**Mode par défaut**: Oui<br /><br />**Mode optimisé**: Oui<br /><br />**Mode plein**: Oui|  
|**Générer un SET NOCOUNT sur**|Spécifie si SET NOCOUNT ON doit être ajouté au début de la procédure converti ou du déclencheur.<br /><br />**Mode par défaut**: Oui<br /><br />**Mode optimisé**: Oui<br /><br />**Mode plein**: Oui|  
  
### <a name="spatial-data-types"></a>Types de données spatiales  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Par défaut de la zone englobante {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} pour les index spatiaux**|Définit la valeur par défaut pour {XMAX&#124;XMIN&#124;YMAX&#124;YMIN} paramètre du cadre englobant utilisé dans les index spatiaux.<br /><br />**Mode par défaut**<br /><br />XMAX : 100<br /><br />XMIN : 0<br /><br />YMAX : 100<br /><br />YMIN : 0<br /><br />**Mode optimisé**<br /><br />XMAX : 100<br /><br />XMIN : 0<br /><br />YMAX : 100<br /><br />YMIN : 0<br /><br />**Mode complet**<br /><br />XMAX : 100<br /><br />XMIN : 0<br /><br />YMAX : 100<br /><br />YMIN : 0|  
|**Densité de grille par défaut pour les index spatiaux**|Définit la valeur par défaut pour LEVEL_1, LEVEL_2, LEVEL_3 et LEVEL_4 de densité de grille utilisée dans les index spatiaux.<br /><br />**Mode par défaut**<br /><br />LEVEL_1 : par défaut<br /><br />LEVEL_2 : par défaut<br /><br />LEVEL_3 : par défaut<br /><br />LEVEL_4 : par défaut<br /><br />**Mode optimisé**<br /><br />LEVEL_1 : par défaut<br /><br />LEVEL_2 : par défaut<br /><br />LEVEL_3 : par défaut<br /><br />LEVEL_4 : par défaut<br /><br />**Mode complet**<br /><br />LEVEL_1 : par défaut<br /><br />LEVEL_2 : par défaut<br /><br />LEVEL_3 : par défaut<br /><br />LEVEL_4 : par défaut|  
  
### <a name="transactions"></a>Transactions  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Tables non transactionnel**|Spécifie si toutes les références à la table qui ne prennent pas en charge les transactions doivent être marquées avec des messages d’avertissement conversion.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
|**Niveau d’isolation de transaction**|Spécifie le niveau d’isolation de transaction doit être utilisé pour les nouvelles transactions.<br /><br />**Mode par défaut**: par défaut<br /><br />**Mode optimisé**: par défaut<br /><br />**Mode plein**: lecture renouvelable|  
  
### <a name="value-control"></a>Contrôle de valeurs  
  
|||  
|-|-|  
|**Terme**|**Définition**|  
|**Caractère à la conversion numérique**|Spécifie comment gérer la conversion implicite et explicite du type de données de caractères pour les types de données numériques.<br /><br />**Mode par défaut**: optimiste<br /><br />**Mode optimisé**: optimiste<br /><br />**Mode plein**: précis|  
|**Contrôler les valeurs numériques non SIGNÉS**|Contrôle affectant des valeurs aux paramètres et les variables numériques non SIGNÉS.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimisé**: non<br /><br />**Mode plein**: Oui|  
|**Contrôler la soustraction non signée**|Modifier les valeurs négatives dans les colonnes de table de type de données non signé.<br /><br />**Mode par défaut**: convertir ' en tant que-est '<br /><br />**Mode optimisé**: convertir ' en tant que-est '<br /><br />**Mode plein**: marque avec un avertissement|  
|**Conversion vers et depuis le type de données binaires**|Spécifie comment gérer la conversion implicite et explicite du type de données binaires.<br /><br />**Mode par défaut**: optimiste<br /><br />**Mode optimisé**: optimiste<br /><br />**Mode plein**: précis|  
|**Type de conversion de données de Date/heure**|Spécifie comment gérer les conversions implicites et explicites à la Date/heure de type de données.<br /><br />**Mode par défaut**: format d’émuler les MySQL<br /><br />**Mode optimisé**: format de l’utilisation de SQL Server<br /><br />**Mode plein**: format d’émuler les MySQL|  
|**Littéraux numériques avec une précision supérieure à 38**|Spécifie comment convertir des littéraux numériques avec une précision supérieure à 38.<br /><br />**Mode par défaut**: arrondir si Possible<br /><br />**Mode optimisé**: arrondir si Possible<br /><br />**Mode plein**: arrondir si Possible|  
|**Zéro-date dans les colonnes NOT NULL**|Spécifie comment gérer l’affectation à des colonnes NOT NULL de zéro-date, date de zéro ou des valeurs de date/heure non valide.<br /><br />**Mode par défaut**: GETDATE()<br /><br />**Mode optimisé**: GETDATE()<br /><br />**Mode plein**: GETDATE()|  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’Interface utilisateur &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
  
