---
title: Mappage des caractères SQL Server et MySQL définie (MySQLToSQL) | Documents Microsoft
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
ms.assetid: 20b3f22e-16a2-4a87-b4eb-c277be6bf5c8
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 5c31cdc9bab3881452c3a03cc0a97cc382481587
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-mysql-and-sql-server-character-set-mysqltosql"></a>Mappage des caractères SQL Server et MySQL définie (MySQLToSQL)
Jeu de caractères (jeu de caractères) peut être spécifié pour les types de données caractères, les expressions et les littéraux de MySQL.  
  
## <a name="charset-mapping"></a>Mappage de jeu de caractères  
Mappage de jeu de caractères est défini pour chaque jeu de caractères MySQL et utilisé pendant la conversion de type de données caractère. Il spécifie comment convertir des types de données de chaînes de caractères d’un jeu de caractères particulier :  
  
-   Types de caractères national SQL Server (NCHAR/NVARCHAR), ou  
  
-   Types de caractère de SQL Server standard (CHAR/VARCHAR)  
  
1.  **national** sont des types de données de caractères de base de données cible :  
  
    1.  **nchar**  
  
    2.  **nvarchar**  
  
2.  **régulière** sont des types de données de caractères de base de données cible :  
  
    1.  **char**  
  
    2.  **varchar**  
  
3.  Mappage de type autorise uniquement le mappage à **national** types de données caractère. Une fois que le type de données MySQL caractère est converti en fonction de mappage de type, jeu de caractères est appliqué.  
  
> [!NOTE]  
> Mappage de jeu de caractères peuvent être définis sur chaque niveau de nœud de l’Explorateur de métadonnées et représenter tous les jeux de caractères lues à partir de MySQL.  
  
## <a name="charset-mapping-on-different-node-levels"></a>Mappage de jeu de caractères sur différents niveaux de nœud  
Mappage de jeu de caractères varie à différents niveaux de nœud, à savoir :  
  
1.  Sur le niveau de nœud de métadonnées racine  
  
2.  Sur la base de données, la catégorie et niveau de nœuds d’objet  
  
> [!NOTE]  
> L’onglet est sélectionné pour la modification du mappage de jeu de caractères, contient trois boutons, quel que soit le mappage sur les différents niveaux de nœud.  
>   
> Celles-ci sont les suivantes :  
>   
> 1.  **S’appliquent :** applique les modifications apportées par l’utilisateur activé uniquement lorsque le mappage de jeu de caractères est modifié et pas encore été enregistré.  
> 2.  **Annuler :** annule les modifications apportées par l’utilisateur. Le bouton est activé lorsque le mappage de jeu de caractères est modifiée, mais pas enregistré.  
> 3.  **Réinitialiser les valeurs par défaut :** réinitialise tous les mappages de valeurs par défaut.  
  
1.  **On racine au niveau du nœud de métadonnées :** grille de mappage de jeu de caractères contient la grille de jeu de caractères avec une colonne distincte pour chaque jeu de caractères. Les colonnes de la grille sont :  
  
    1.  La première colonne de la grille nommée **nom de jeu de caractères** contient le nom du jeu de caractères.  
  
    2.  L’autre nommé **Description du jeu de caractères** contient la description du jeu de caractères.  
  
    3.  La troisième colonne intitulée **le Type de jeu de caractères cible** contient les paramètres de mappage de jeu de caractères particulier. Les valeurs de cette colonne sont :  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
    > [!IMPORTANT]  
    > Les valeurs par défaut pour un jeu de caractères particulier ont le préfixe « (valeur par défaut) » après CHAR/VARCHAR, NCHAR/NVARCHAR.  
  
    Le mappage de jeu de caractères entre la base de données MySQL et de la base de données cible au niveau du nœud racine de métadonnées est donné ci-dessous :  
  
    ||||  
    |-|-|-|  
    |**Nom du jeu de caractères**|**Description du jeu de caractères**|**Type de jeu de caractères cible (par défaut)**|  
    |Big5|Chinois traditionnel Big5|NCHAR/NVARCHAR (par défaut)|  
    |dec8|DEC ouest européenne|CHAR/VARCHAR (par défaut)|  
    |CP850|Europe de l’ouest de déni de service|CHAR/VARCHAR (par défaut)|  
    |hp8|HP ouest européenne|CHAR/VARCHAR (par défaut)|  
    |koi8r|KOI8-R Relcom russe|CHAR/VARCHAR (par défaut)|  
    |Latin 1|cp1252 ouest européenne|CHAR/VARCHAR (par défaut)|  
    |Latin2|ISO 8859-2 Europe de l'|CHAR/VARCHAR (par défaut)|  
    |swe7|7 bits suédois|CHAR/VARCHAR (par défaut)|  
    |ASCII|US ASCII|CHAR/VARCHAR (par défaut)|  
    |ujis|EUC-JP japonais|NCHAR/NVARCHAR (par défaut)|  
    |SJIS|Shift-JIS japonais|NCHAR/NVARCHAR (par défaut)|  
    |Hébreu|ISO 8859-8 hébreu|CHAR/VARCHAR (par défaut)|  
    |TIS620|TIS620 thaï|CHAR/VARCHAR (par défaut)|  
    |eucKR|EUC-KR coréen|NCHAR/NVARCHAR (par défaut)|  
    |koi8u|KOI8-U ukrainien|CHAR/VARCHAR (par défaut)|  
    |gb2312|GB2312 En chinois simplifié|NCHAR/NVARCHAR (par défaut)|  
    |Grec|ISO 8859-7 grec|CHAR/VARCHAR (par défaut)|  
    |CP 1250|Europe centrale de Windows|CHAR/VARCHAR (par défaut)|  
    |GBK|Chinois simplifié de GBK|NCHAR/NVARCHAR (par défaut)|  
    |Latin5|ISO 8859-9 turc|CHAR/VARCHAR (par défaut)|  
    |armscii8|ARMSCII-8 arménien|CHAR/VARCHAR (par défaut)|  
    |UTF-8|UTF-8 Unicode|NCHAR/NVARCHAR (par défaut)|  
    |UCS2|UCS-2 Unicode|NCHAR/NVARCHAR (par défaut)|  
    |cp866|Russe de déni de service|CHAR/VARCHAR (par défaut)|  
    |keybcs2|DOS Kamenicky tchèque-slovaque|CHAR/VARCHAR (par défaut)|  
    |macce|Europe centrale du Mac|CHAR/VARCHAR (par défaut)|  
    |MacRoman|Mac ouest européenne|CHAR/VARCHAR (par défaut)|  
    |cp852|Centre de déni de service européenne|CHAR/VARCHAR (par défaut)|  
    |latin7|ISO 8859-13 Baltique|CHAR/VARCHAR (par défaut)|  
    |CP 1251|Windows cyrillique|CHAR/VARCHAR (par défaut)|  
    |CP 1256|Windows arabe|CHAR/VARCHAR (par défaut)|  
    |CP 1257|Windows : Baltique|CHAR/VARCHAR (par défaut)|  
    |binary|Jeu de caractères binaires pseudo|CHAR/VARCHAR (par défaut)|  
    |geostd8|GEOSTD8 géorgien|CHAR/VARCHAR (par défaut)|  
    |cp932|SJIS pour Windows version japonaise|NCHAR/NVARCHAR (par défaut)|  
    |eucjpms|UJIS pour Windows version japonaise|NCHAR/NVARCHAR (par défaut)|  
  
2.  **Sur la base de données, de catégorie ou de niveaux de nœuds d’objet :** au niveau base de données, de catégorie ou de nœuds d’objet, grille de mappage de jeu de caractères contient les mêmes lignes que sur le niveau de métadonnées du nœud racine, notamment. :  
  
    1.  La première colonne de la grille intitulée **définir un nom de caractère** contient le nom du jeu de caractères.  
  
    2.  La deuxième colonne intitulée **caractère définir la Description** contient la description du jeu de caractères.  
  
    3.  La seule différence est les valeurs dans la troisième colonne de la grille. La troisième colonne intitulée **Type de données cible** contient les paramètres de mappage de jeu de caractères particulier. Les valeurs de la colonne sont :  
  
        -   Héritée (CHAR/VARCHAR ou NCHAR/NVARCHAR)  
  
        -   CHAR/VARCHAR  
  
        -   NCHAR/NVARCHAR  
  
> [!IMPORTANT]  
> -   Dans le mappage de jeu de caractères entre la base de données MySQL et de la base de données cible sur base de données, de catégorie et de niveaux de nœuds d’objet, les valeurs par défaut pour un jeu de caractères particulier sur chaque niveau de la racine de la colonne **Type de données cible** doit être « hérité ».  
> -   Dans la grille, la valeur **Inherited** est suivi du suffixe avec l’option '(CHAR/VARCHAR)' ou '(NCHAR/NVARCHAR)' en fonction de la valeur a été héritée parent par ce jeu de caractères particulier.  
  
