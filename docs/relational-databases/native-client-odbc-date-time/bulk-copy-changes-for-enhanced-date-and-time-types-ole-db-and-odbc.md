---
title: En bloc a été apportée pour une meilleure Types Date et heure (OLE DB et ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC, bulk copy operations
ms.assetid: c29e0f5e-9b3c-42b3-9856-755f4510832f
caps.latest.revision: 27
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: de93dee05a09da895073a12a30d36d8d26e5d764
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc"></a>Modifications de copie en bloc pour une meilleure Types Date et heure (OLE DB et ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Cette rubrique décrit les améliorations de date/heure permettant de prendre en charge les fonctionnalités de copie en bloc. Les informations fournies dans cette rubrique sont valables pour OLE DB et ODBC dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="format-files"></a>Fichiers de format  
 Lors de la génération interactive de fichiers de format, le tableau ci-dessous décrit l'entrée utilisée pour spécifier des types date/heure et les noms de type de données de fichier hôte correspondants.  
  
|type de stockage de fichier|Type de données du fichier hôte|Réponse à l’invite : « tapez le type de stockage du champ < nom_champ > [\<par défaut >] : »|  
|-----------------------|-------------------------|-----------------------------------------------------------------------------------------------------|  
|DateTime|SQLDATETIME|d|  
|Smalldatetime|SQLDATETIM4|D|  
|Date|SQLDATE|de|  
|Time|SQLTIME|te|  
|Datetime2|SQLDATETIME2|d2|  
|Datetimeoffset|SQLDATETIMEOFFSET|do|  
  
 Le fichier au format XML XSD possédera les ajouts suivants :  
  
```  
<xs:complexType name="SQLDATETIME2">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATETIMEOFFSET">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATE">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLTIME">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
```  
  
## <a name="character-data-files"></a>Fichiers de données de type caractère  
 Dans les fichiers de données de type caractère, les valeurs de date et d’heure sont représentées comme décrit dans la section « Formats de données : chaînes et littéraux » de [prise en charge du Type de données de Date ODBC et les améliorations apportées au](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md) pour ODBC ou [prise en charge du Type de données de Date OLE DB et les améliorations apportées au](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md) pour OLE DB.  
  
 Dans les fichiers de données en mode natif, les valeurs de date et heure pour les quatre nouveaux types sont représentés en tant que leurs représentations TDS avec une échelle de 7 (car il s’agit du nombre maximal pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fichiers de données bcp ne stockent pas l’échelle de ces colonnes). Il n’existe aucune modification pour le stockage existants **datetime** et **smalldatetime** leurs données sous forme de tableau ou type stream représentations (TDS).  
  
 Les tailles de stockage pour les différents types de stockage sont les suivantes pour OLE DB :  
  
|type de stockage de fichier|Taille de stockage en octets|  
|-----------------------|---------------------------|  
|datetime|8|  
|smalldatetime|4|  
|date|3|  
|time|6|  
|datetime2|9|  
|datetimeoffset|11|  
  
 Les tailles sont les suivantes pour ODBC. Notez qu'il n'est pas nécessaire de stocker la précision dans les fichiers de format ou de données, car BCP.exe extraira toujours la précision du serveur.  
  
|type de stockage de fichier|Taille de stockage en octets|Format de stockage|  
|-----------------------|---------------------------|--------------------|  
|datetime (d)|8|TDS (flux de données tabulaires)|  
|smalldatetime (D)|4|TDS (flux de données tabulaires)|  
|date (de)|3|TDS (flux de données tabulaires)|  
|time (te)|6|TDS (flux de données tabulaires)|  
|datetime2 (d2)|9|TDS (flux de données tabulaires)|  
|datetimeoffset (do)|11|TDS (flux de données tabulaires)|  
  
## <a name="bcp-types-in-sqlnclih"></a>Types BCP dans sqlncli.h  
 Les types suivants sont définis dans sqlncli.h pour être utilisés avec les extensions d'API BCP d'ODBC. Ces types sont passés avec le *eUserDataType* paramètre de IBCPSession::BCPColFmt dans OLE DB.  
  
|type de stockage de fichier|Type de données du fichier hôte|Type dans sqlncli.h pour une utilisation avec IBCPSession::BCPColFmt|Valeur|  
|-----------------------|-------------------------|-----------------------------------------------------------|-----------|  
|DateTime|SQLDATETIME|BCP_TYPE_SQLDATETIME|0x3d|  
|Smalldatetime|SQLDATETIM4|BCP_TYPE_SQLDATETIME4|0x3a|  
|Date|SQLDATE|BCP_TYPE_SQLDATE|0x28|  
|Time|SQLTIME|BCP_TYPE_SQLTIME|0x29|  
|Datetime2|SQLDATETIME2|BCP_TYPE_SQLDATETIME2|0x2a|  
|Datetimeoffset|SQLDATETIMEOFFSET|BCP_TYPE_SQLDATETIMEOFFSET|0x2b|  
  
## <a name="bcp-data-type-conversions"></a>Conversions des types de données BCP  
 Les tableaux ci-dessous fournissent des informations de conversion.  
  
 **Remarque OLE DB** les conversions suivantes sont effectuées par IBCPSession. IRowsetFastLoad utilise les conversions OLE DB tel que défini dans [Conversions effectuées à partir du Client au serveur](../../relational-databases/native-client-ole-db-date-time/conversions-performed-from-client-to-server.md). Notez que les valeurs datetime sont arrondies au 1/300e de seconde et que les secondes des valeurs smalldatetime sont mises à zéro après l'exécution des conversions clientes décrites ci-dessous. L'arrondi des valeurs datetime est propagé aux heures et aux minutes, mais pas à la date.  
  
|À --><br /><br /> From|date|time|smalldatetime|datetime|datetime2|datetimeoffset|char|wchar|  
|------------------------|----------|----------|-------------------|--------------|---------------|--------------------|----------|-----------|  
|Date|1|-|1,6|1,6|1,6|1,5,6|1,3|1,3|  
|Time|Néant|1,10|1,7,10|1,7,10|1,7,10|1,5,7,10|1,3|1,3|  
|Smalldatetime|1,2|1,4,10|1|1|1,10|1,5,10|1,11|1,11|  
|DateTime|1,2|1,4,10|1,12|1|1,10|1,5,10|1,11|1,11|  
|Datetime2|1,2|1,4,10|1,10 (ODBC)1,12 (OLE DB)|1,10|1,10|1,5,10|1,3|1,3|  
|Datetimeoffset|1,2,8|1,4,8,10|1,8,10|1,8,10|1,8,10|1,10|1,3|1,3|  
|Char/wchar (date)|9|-|9,6 (ODBC)9,6,12 (OLE DB)|9,6 (ODBC)9,6,12 (OLE DB)|9,6|9,5,6|Néant|Néant|  
|Char/wchar (heure)|-|9,10|9,7,10 (ODBC)9,7,10,12 (OLE DB)|9,7,10 (ODBC)9,7,10, 12 (OLE DB)|9,7,10|9,5,7,10|Néant|Néant|  
|Char/wchar (datetime)|9,2|9,4,10|9,10 (ODBC)9,10,12 (OLE DB)|9,10 (ODBC)9,10,12 (OLE DB)|9,10|9,5,10|Néant|Néant|  
|Char/wchar (datetimeoffset)|9,2,8|9,4,8,10|9,8,10 (ODBC)9,8,10,12 (OLE DB)|9,8,10 (ODBC)9,8,10,12 (OLE DB)|9,8,10|9,10|Néant|Néant|  
  
#### <a name="key-to-symbols"></a>Liste des symboles  
  
|Symbole|Signification|  
|------------|-------------|  
|-|Aucune conversion n'est prise en charge.<br /><br /> Un enregistrement de diagnostic ODBC est généré avec SQLSTATE 07006 et le message « Violation de l'attribut de type de données restreint ».|  
|1|Si les données fournies ne sont pas valides, un enregistrement de diagnostic ODBC est généré avec SQLSTATE 22007 et le message « Format datetime non valide ». Pour les valeurs datetimeoffset, la partie heure doit se situer dans la plage après la conversion au format UTC, même si aucune conversion au format UTC n'est demandée. Ceci tient au fait que TDS et le serveur normalisent toujours l'heure dans les valeurs datetimeoffset pour UTC. Par conséquent, le client doit vérifier que les composants heure se situent dans la plage prise en charge après la conversion au format UTC.|  
|2|Le composant heure est ignoré.|  
|3|Pour ODBC, s'il se produit une troncation avec perte de données, un enregistrement de diagnostic est généré avec SQLSTATE 22001 et le message « Troncation à droite de la chaîne de données ». Le nombre de chiffres des fractions de seconde (l'échelle) est déterminé à partir de la taille de la colonne de destination, conformément au tableau suivant : Pour les tailles de colonne supérieures à la plage du tableau, une échelle de 7 est nécessaire. Cette conversion accepte jusqu'à neuf chiffres de fractions de seconde, valeur maximale autorisée par ODBC.<br /><br /> **Type :** DBTIME2<br /><br /> **Implicite échelle 0** 8<br /><br /> **Implicite échelle 1..7** 10,16<br /><br /> <br /><br /> **Type :** DBTIMESTAMP<br /><br /> **Implicite échelle 0 :** 19<br /><br /> **Implicite échelle 1..7 :** 21..27<br /><br /> <br /><br /> **Type :** DBTIMESTAMPOFFSET<br /><br /> **Implicite échelle 0 :** 26<br /><br /> **Implicite échelle 1..7 :** 28..34<br /><br /> Pour OLE DB, s'il se produit une troncation avec perte de données, une erreur est publiée. Pour datetime2, le nombre de chiffres des fractions de seconde (l'échelle) est déterminé à partir de la taille de la colonne de destination, conformément au tableau suivant : pour les tailles de colonne supérieures à la plage du tableau, une échelle de 9 est nécessaire. Cette conversion accepte jusqu'à neuf chiffres de fractions de seconde, valeur maximale autorisée par OLE DB.<br /><br /> **Type :** DBTIME2<br /><br /> **Implicite échelle 0** 8<br /><br /> **Implicite échelle 1..9** 1..9<br /><br /> <br /><br /> **Type :** DBTIMESTAMP<br /><br /> **Implicite échelle 0 :** 19<br /><br /> **Implicite échelle 1..9 :** 21..29<br /><br /> <br /><br /> **Type :** DBTIMESTAMPOFFSET<br /><br /> **Implicite échelle 0 :** 26<br /><br /> **Implicite échelle 1..9 :** 28..36|  
|4|Le composant date est ignoré.|  
|5|Le composant fuseau horaire est défini au format UTC (par exemple, 00:00).|  
|6|L'heure est définie avec la valeur zéro.|  
|7|La date est définie avec la valeur 1900-01-01.|  
|8|Le décalage horaire est ignoré.|  
|9|La chaîne est analysée et convertie en valeur date, datetime, datetimeoffset ou time, selon le premier caractère de ponctuation rencontré et la présence de composants restants. La chaîne est ensuite convertie en type cible, selon les règles fournies dans le tableau à la fin de cette rubrique pour le type source découvert par ce processus. Si les données fournies ne peuvent pas être analysées sans erreur, ou si un composant quelconque est en dehors de la plage autorisée, ou s'il n'y a aucune conversion du type de littéral au type cible, une erreur est publiée (OLE DB) ou un enregistrement de diagnostic ODBC est généré avec SQLSTATE 22018 et le message « Valeur de caractère non valide pour la spécification de la casse ». Pour les paramètres datetime et smalldatetime, si l'année est en dehors de la plage prise en charge par ces types, une erreur est publiée (OLE DB) ou un enregistrement de diagnostic ODBC est généré avec SQLSTATE 22007 et le message « Format datetime non valide ».<br /><br /> Pour datetimeoffset, la valeur doit se situer dans la plage après la conversion au format UTC, même si aucune conversion au format UTC n'est demandée. Cela tient au fait que TDS et le serveur normalisent toujours l'heure des valeurs datetimeoffset pour UTC, si bien que le client doit vérifier que les composants heure se situent dans la plage prise en charge après la conversion au format UTC. Si la valeur n'est pas dans la plage UTC prise en charge, une erreur est publiée (OLE DB) ou un enregistrement de diagnostic ODBC est généré avec SQLSTATE 22007 et le message « Format datetime non valide ».|  
|10|S'il se produit une troncation avec perte de données lors d'une conversion de client à serveur, une erreur est publiée (OLE DB) ou un enregistrement de diagnostic ODBC est généré avec SQLSTATE 22008 et le message « Dépassement de la capacité du champ datetime ». Cette erreur se produit également si la valeur est située en dehors de la plage qui peut être représentée par la plage UTC utilisée par le serveur. S'il se produit une troncation de secondes ou de fractions de seconde lors d'une conversion de serveur à client, seul un avertissement est émis.|  
|11|S'il se produit une troncation avec perte de données, un enregistrement de diagnostic est généré.<br /><br /> Lors d'une conversion de serveur à client, il s'agit d'un avertissement (ODBC SQLSTATE S1000).<br /><br /> Lors d'une conversion de client à serveur, il s'agit d'une erreur (ODBC SQLSTATE 22001).|  
|12|Les secondes sont réinitialisées et les fractions de seconde sont ignorées. Aucune erreur de troncation n'est possible.|  
|Néant|Le comportement existant et antérieur de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] est conservé.|  
  
## <a name="see-also"></a>Voir aussi  
 [Date et heure améliorations &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)   
 [Date et heure améliorations & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
