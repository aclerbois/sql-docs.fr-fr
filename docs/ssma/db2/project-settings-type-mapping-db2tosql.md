---
title: Paramètres (Type de mappage) du projet (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
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
ms.assetid: cf426c69-6a8e-4d19-951d-6661d5ae2562
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 523486520f1698c841d9c3e7a09d06fc23978b82
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-type-mapping-db2tosql"></a>Paramètres (Type de mappage) du projet (DB2ToSQL)
La page mappage de Type de la **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA convertit les types de données DB2 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données.  
  
La page mappage de Type est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Pour spécifier les paramètres pour tous les futurs projets SSMA, sur le **outils** menu, cliquez sur **par défaut des paramètres de projet**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** liste déroulante, puis cliquez sur **le mappage de Type** en bas du volet gauche.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, cliquez sur **les paramètres de projet**, puis cliquez sur **le mappage de Type** en bas du volet gauche.  
  
Pour spécifier les paramètres pour l’objet en cours ou de la classe d’objets, utilisez la **le mappage de Type** onglet dans la fenêtre principale de SSMA.  
  
## <a name="options"></a>Options  
Le tableau suivant présente la **le mappage de Type** onglet options :  
  
**Type de source**  
Le type de données DB2 mappé.  
  
**Type de cible**  
La cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données pour le type de données DB2 spécifié.  
  
Consultez les tableaux dans la section suivante pour la valeur par défaut SSMA pour les mappages de type DB2.  
  
**Ajouter**  
Cliquez pour ajouter un type de données à la liste de mappage.  
  
**Modifier**  
Cliquez pour modifier le type de données sélectionné dans la liste de mappage.  
  
**Supprimer**  
Cliquez pour supprimer le mappage de type de données sélectionnées à partir de la liste de mappage.  
  
**Réinitialiser les valeurs par défaut**  
Cliquez pour réinitialiser la liste de mappage de type pour les valeurs par défaut SSMA.  
  
## <a name="default-type-mappings"></a>Mappages de Type par défaut  
Dans SSMA pour DB2, vous pouvez définir des mappages de types personnalisés pour les arguments, les colonnes, les variables locales et les valeurs de retour. Le mappage par défaut pour les arguments et les types de retour est presque identique.  
  
### <a name="default-argument-type-and-return-value-type-mapping"></a>Type d’Argument par défaut et le mappage de Type de valeur de retour  
Le tableau suivant contient le mappage de type de données par défaut pour les arguments et valeurs de retour.  
  
|DB2 Type de données|Par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Type de données|  
|-----------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|BINARY_FLOAT|float [53]|  
|binary_integer|int|  
|objet BLOB|varbinary(max)|  
|boolean|bit|  
|char|varchar(max)|  
|char varying|varchar(max)|  
|caractère|varchar(max)|  
|character varying|varchar(max)|  
|CLOB|varchar(max)|  
|date|datetime2[0]|  
|dec|dec[38][0]|  
|Décimal|float [53]|  
|double précision|float [53]|  
|float|float [53]|  
|int|int|  
|entier|int|  
|long|varchar(max)|  
|long raw|varbinary(max)|  
|long raw [\*... 8000]<sup>*</sup>|varbinary [*]|  
|long raw [8001..\*]<sup>*</sup>|varbinary(max)|  
|national char|nvarchar(max)|  
|national char varying|nvarchar(max)|  
|caractères nationaux|nvarchar(max)|  
|variable de caractères nationaux<sup>**</sup>|nvarchar(max)|  
|variable de caractères nationaux<sup>*</sup>|nvarchar(max)|  
|NCHAR|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|nombre|float [53]|  
|numérique|float [53]|  
|NVARCHAR2|nvarchar(max)|  
|pls_integer|int|  
|brut|varbinary(max)|  
|real|float [53]|  
|ID de ligne|uniqueidentifier|  
|Signtype|smallint|  
|smallint|smallint|  
|chaîne|varchar(max)|  
|TIMESTAMP|datetime2|  
|horodateur avec fuseau horaire local|datetimeoffset|  
|horodateur avec fuseau horaire|datetimeoffset|  
|UROWID|uniqueidentifier|  
|varchar|varchar(max)|  
|VARCHAR2|varchar(max)|  
|XmlType|xml|  
  
<sup>*</sup> S’applique pour retourner la valeur type mappage uniquement.  
  
<sup>**</sup> S’applique à l’argument de type mappage uniquement.  
  
### <a name="default-column-type-mapping"></a>Mappage de Type de colonne par défaut  
Le tableau suivant contient le mappage de type par défaut pour les colonnes.  
  
|DB2 Type de données|Par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Type de données|  
|-----------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|BINARY_FLOAT|float [53]|  
|objet BLOB|varbinary(max)|  
|char|char|  
|char varying [*.. \*]|varchar [*]|  
|Char [*.. \*]|Char [*]|  
|caractère|char|  
|variable de caractères [*.. \*]|varchar [*]|  
|caractère [*.. \*]|Char [*]|  
|CLOB|varchar(max)|  
|date|datetime2[0]|  
|dec|dec[38][0]|  
|DEC [*.. \*]|DEC [*] [0]|  
|DEC [*.. \*][\*.. \*]|dec[*][\*]|  
|Décimal|decimal[38][0]|  
|Decimal [*.. \*]|Decimal [*] [0]|  
|Decimal [*.. \*][\*.. \*]|Decimal [*] [\*]|  
|double précision|float [53]|  
|float|float [53]|  
|float [*.. 53]|float [*]|  
|float [54.. *]|float [53]|  
|int|int|  
|entier|int|  
|long|varchar(max)|  
|long raw|varbinary(max)|  
|long raw [*.. 8000]|varbinary [*]|  
|long raw [8001.. *]|varbinary(max)|  
|long varchar|varchar(max)|  
|long [*.. 8000]|varchar [*]|  
|long [8001.. *]|varchar(max)|  
|national char|NCHAR|  
|national char varying [*.. \*]|nvarchar [*]|  
|national char [*.. \*]|NCHAR [*]|  
|caractères nationaux|NCHAR|  
|variable de caractères nationaux [*.. \*]|nvarchar [*]|  
|les caractères nationaux [*.. \*]|NCHAR [*]|  
|NCHAR|NCHAR|  
|NCHAR [*]|NCHAR [*]|  
|NCLOB|nvarchar(max)|  
|nombre|float [53]|  
|nombre [*.. \*]|numérique [*]|  
|nombre [*.. \*][\*.. \*]|numérique [*] [\*]|  
|numérique|numérique|  
|numérique [*.. \*]|numérique [*]|  
|numérique [*.. \*][\*.. \*]|numérique [*] [\*]|  
|NVARCHAR2 [*.. \*]|nvarchar [*]|  
|RAW [*.. \*]|varbinary [*]|  
|real|float [53]|  
|ID de ligne|uniqueidentifier|  
|smallint|smallint|  
|TIMESTAMP|datetime2|  
|horodateur avec fuseau horaire local|datetimeoffset|  
|horodateur avec fuseau horaire local [*.. \*]|datetimeoffset[*]|  
|horodateur avec fuseau horaire|datetimeoffset|  
|horodateur avec fuseau horaire [*.. \*]|datetimeoffset[*]|  
|timestamp [*.. \*]|datetime2[*]|  
|UROWID|uniqueidentifier|  
|UROWID [*.. \*]|uniqueidentifier|  
|varchar [*.. \*]|varchar [*]|  
|VARCHAR2 [*.. \*]|varchar [*]|  
|XmlType|xml|  
  
### <a name="default-local-variable-type-mapping"></a>Mappage de Type de Variable locale par défaut  
Le tableau suivant contient le mappage de type par défaut pour les variables locales.  
  
|DB2 Type de données|Par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Type de données|  
|-----------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|BINARY_FLOAT|float [53]|  
|binary_interger|int|  
|Objet BLOB|varbinary(max)|  
|Booléen|bit|  
|Char|char|  
|char varying [*.. 8000]|varchar [*]|  
|char varying [8001.. *]|varchar(max)|  
|Char [*.. 8000]|Char [*]|  
|Char [8001.. *]|varchar(max)|  
|Caractère|char|  
|variable de caractères [*.. 8000]|varchar [*]|  
|variable de caractères [8001.. *]|varchar(max)|  
|caractère [*.. 8000]|Char [*]|  
|caractère [8001.. *]|varchar(max)|  
|CLOB|varchar(max)|  
|date|datetime2[0]|  
|dec|dec[38][0]|  
|DEC [*.. \*]|DEC [*] [0]|  
|DEC [*.. \*][\*.. \*]|dec[*][\*]|  
|Décimal|decimal[38][0]|  
|Decimal [*.. \*]|Decimal [*] [0]|  
|Decimal [*.. \*][\*.. \*]|Decimal [*] [\*]|  
|double précision|float [53]|  
|Float|float [53]|  
|float [*.. 53]|float [*]|  
|float [54.. *]|float [53]|  
|Int|int|  
|Entier|int|  
|entier [*.. \*]|numérique [*] [0]|  
|Long|varchar(max)|  
|long raw|varbinary(max)|  
|long raw [*.. 8000]|varbinary [*]|  
|long raw [8001.. *]|varbinary(max)|  
|national char|NCHAR|  
|national char varying [*.. 4000]|nvarchar [*]|  
|national char varying [4001.. *]|nvarchar(max)|  
|national char [*.. 4000]|NCHAR [*]|  
|national char [4001.. *]|nvarchar(max)|  
|caractères nationaux|NCHAR|  
|les caractères nationaux [*.. 4000]|nvarchar [*]|  
|les caractères nationaux [4001.. *]|nvarchar(max)|  
|variable de caractères nationaux [*.. 4000]|nvarchar [*]|  
|variable de caractères nationaux [4001.. *]|nvarchar(max)|  
|Nchar|NCHAR|  
|NCHAR [*.. 4000]|NCHAR [*]|  
|NCHAR [4001.. *]|nvarchar(max)|  
|NCHAR varying [*.. 4000]|nvarchar [*]|  
|NCHAR varying [4001.. *]|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|Number|float [53]|  
|nombre [*.. \*]|numérique [*]|  
|nombre [*.. \*][\*.. \*]|numérique [*] [\*]|  
|Numérique|numérique [38] [0]|  
|numérique [*.. \*]|numérique [*]|  
|numérique [*.. \*][\*.. \*]|numérique [*] [\*]|  
|NVARCHAR2 [*.. 4000]|nvarchar [*]|  
|NVARCHAR2 [4001.. *]|nvarchar(max)|  
|pls_integer|int|  
|RAW [*.. 8000]|varbinary [*]|  
|RAW [8001.. *]|varbinary(max)|  
|Real|float [53]|  
|ID de ligne|uniqueidentifier|  
|Signtype|smallint|  
|Smallint|smallint|  
|chaîne [*.. 8000]|varchar [*]|  
|chaîne [8001.. *]|varchar(max)|  
|TIMESTAMP|datetime2|  
|horodateur avec fuseau horaire local|datetimeoffset|  
|horodateur avec fuseau horaire|datetimeoffset|  
|horodateur avec fuseau horaire local [*.. \*]|datetimeoffset[*]|  
|horodateur avec fuseau horaire [*.. \*]|datetimeoffset[*]|  
|timestamp [*.. \*]|datetime2[*]|  
|UROWID|uniqueidentifier|  
|UROWID [*.. \*]|uniqueidentifier|  
|varchar [*.. 8000]|varchar [*]|  
|varchar [8001.. *]|varchar(max)|  
|VARCHAR2 [*.. 8000]|varchar [*]|  
|VARCHAR2 [8001.. *]|varcha(max)|  
|XmlType|xml|  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’Interface utilisateur &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
