---
title: Prise en charge de Type de données pour OLE DB Date et heure améliorations | Documents Microsoft
description: Prise en charge du type de données pour les améliorations de date et d’heure OLE DB
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB], data type support
- OLE DB, date/time improvements
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 66756bbb8179e9d7644d053dd76c4ab8223032d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-support-for-ole-db-date-and-time-improvements"></a>Prise en charge du Type de données pour OLE DB Date et heure améliorations
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Cet article fournit des informations à propos d’OLE DB (pilote OLE DB pour SQL Server) qui prennent en charge les types [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des types de données date/heure.  
  
## <a name="data-type-mapping-in-rowsets-and-parameters"></a>Mappage de type de données dans les ensembles de lignes et les paramètres  
 OLE DB fournit deux nouveaux types de données pour prendre en charge les nouveaux types de serveurs : DBTYPE_DBTIME2 et DBTYPE_DBTIMESTAMPOFFSET. Le tableau ci-dessous illustre le mappage complet des types de serveurs :  
  
|Type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Type de données OLE DB|Valeur|  
|-----------------------------------------|----------------------|-----------|  
|datetime|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
|smalldatetime|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
|date|DBTYPE_DBDATE|133 (oledb.h)|  
|time|DBTYPE_DBTIME2|145 (msoledbsql.h)|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|146 (msoledbsql.h)|  
|datetime2|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
  
## <a name="data-formats-strings-and-literals"></a>Formats de données : chaînes et littéraux  
  
|Type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Type de données OLE DB|Format de chaîne pour les conversions clientes|  
|-----------------------------------------|----------------------|------------------------------------------|  
|datetime|DBTYPE_DBTIMESTAMP|'aaaa-mm-jj hh:mm:ss[.999]'<br /><br /> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge jusqu'à trois chiffres de fractions de seconde pour le type Datetime.|  
|smalldatetime|DBTYPE_DBTIMESTAMP|'aaaa-mm-jj hh:mm:ss'<br /><br /> Ce type de données a la précision d’une minute. Le composant des secondes sera égal à zéro en sortie et arrondi par le serveur en entrée.|  
|date|DBTYPE_DBDATE|'aaaa-mm-jj'|  
|time|DBTYPE_DBTIME2|'hh:mm:ss[.9999999]'<br /><br /> Le cas échéant, les fractions de seconde peuvent être spécifiées à l'aide de sept chiffres au plus.|  
|datetime2|DBTYPE_DBTIMESTAMP|'aaaa-mm-jj hh:mm:ss[.fffffff]'<br /><br /> Le cas échéant, les fractions de seconde peuvent être spécifiées à l'aide de sept chiffres au plus.|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|'aaaa-mm-jj hh:mm:ss[.fffffff] +/-hh:mm'<br /><br /> Le cas échéant, les fractions de seconde peuvent être spécifiées à l'aide de sept chiffres au plus.|  
  
 Il n'y a pas de modifications aux séquences d'échappement pour les littéraux de type date/time.  
  
 Dans les résultats, les fractions de seconde utilisent un point (.) plutôt que les deux-points (:).  
  
 Les valeurs de chaîne retournées aux applications sont toujours de la même longueur pour une colonne donnée. Les composants année, mois, jour, heure, minute et seconde seront complétés avec des zéros non significatifs pour atteindre leur largeur maximale. Il y aura exactement un espace entre la date et l'heure et exactement un espace entre l'heure et le décalage horaire. Un décalage horaire sera toujours précédé d'un signe. Ce signe est un plus (+) lorsque le décalage est nul. Aucun espace ne figurera entre le signe et la valeur de décalage. Les fractions de seconde seront complétées, si nécessaire, par des zéros à droite, jusqu'à la précision définie pour la colonne, mais sans la dépasser. Pour les colonnes datetime, il y aura trois chiffres de fractions de seconde. Pour les colonnes smalldatetime, il n'y aura pas de chiffres de fractions de seconde et les secondes seront toujours égales à zéro.  
  
 Les conversions de valeurs de chaîne fournies par l'application seront plus flexibles et autoriseront des valeurs de composant de largeur inférieure à la largeur maximale. Les années peuvent être de 1 à 4 chiffres. Les mois, les jours, les heures, les minutes et les secondes peuvent être de 1 à 2 chiffres. Il peut y avoir un espace arbitraire entre la date et l'heure et entre l'heure et le décalage horaire. Le signe d'un décalage correspondant à zéro heure et zéro minute peut être plus ou moins. Les zéros à droite sont autorisés pour les fractions de seconde avec un maximum de 9 chiffres. Un composant heure peut se terminer avec une virgule décimale et aucun chiffre de fractions de seconde.  
  
 Une chaîne vide n'est pas un littéral de date et d'heure valide et ne représente pas une valeur NULL. Une tentative de convertir une chaîne vide en valeur de date ou d'heure provoque des erreurs avec SQLState 22018 et le message « Valeur de caractère non valide pour la spécification de la casse ».  
  
## <a name="data-formats-data-structures"></a>Formats de données : structures de données  
 Dans les structures spécifiques à OLE DB décrites ci-dessous, OLE DB se conforme aux contraintes suivantes. Celles-ci sont tirées du calendrier grégorien :  
  
-   La plage des mois s'étend de 1 à 12.  
  
-   La plage des jours s'étend de 1 au nombre de jours dans le mois, et doit être cohérente avec les années et les mois  en tenant compte des années bissextiles.  
  
-   La plage des heures s'étend de 0 à 23.  
  
-   La plage des minutes s'étend de 0 à 59.  
  
-   La plage des secondes s'étend de 0 à 59. Cela permet d'ajouter deux secondes intercalaires (au plus) pour conserver la synchronisation avec l'heure sidérale.  
  
 Les implémentations pour les structs OLE DB existants suivants ont été modifiées afin de prendre en charge les nouveaux types de données de date et d'heure [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Toutefois, les définitions n'ont pas changé.  
  
-   DBTYPE_DATE (Ceci est un type DATE Automation. Il est représenté en interne comme un **double**... La partie entière correspond au nombre de jours depuis le 30 décembre 1899 et la partie fractionnaire correspond à la fraction d'un jour. Ce type a une précision de 1 seconde et a donc une échelle effective de 0.)  
  
-   DBTYPE_DBDATE  
  
-   DBTYPE_DBTIME  
  
-   DBTYPE_DBTIMESTAMP (le champ de fraction est défini par OLE DB comme le nombre de milliardièmes de seconde (nanosecondes) et il est compris entre 0 et 999 999 999)  
  
-   DBTYPE_FILETIME  
  
### <a name="dbtypedbtime2"></a>DBTYPE_DBTIME2  
 Ce struct est complété jusqu'à 12 octets sur les systèmes d'exploitation 32 bits et 64 bits.  
  
```  
typedef struct tagDBTIME2 {  
    USHORT hour;  
    USHORT minute;  
    USHORT second;  
    ULONG fraction;  
    } DBTIME2;  
```  
  
### <a name="dbtype-dbtimestampoffset"></a>DBTYPE_ DBTIMESTAMPOFFSET  
  
```  
typedef struct tagDBTIMESTAMPOFFSET {  
    SHORT year;  
    USHORT month;  
    USHORT day;  
    USHORT hour;  
    USHORT minute;  
    USHORT second;  
    ULONG fraction;  
    SHORT timezone_hour;  
    SHORT timezone_minute;  
    } DBTIMESTAMPOFFSET;  
```  
  
 Si `timezone_hour` est négatif, `timezone_minute` doit être négatif ou égal à zéro. Si `timezone_hour` est un nombre positif, `timezone minute` doit être positif ou égal à zéro. Si `timezone_hour` est nul, `timezone minute` peut contenir une valeur comprise entre -59 et +59.  
  
### <a name="ssvariant"></a>SSVARIANT  
 Ce struct inclut désormais les nouvelles structures, DBTYPE_DBTIME2 et DBTYPE_DBTIMESTAMPOFFSET, et ajoute une échelle de fractions de seconde pour les types appropriés.  
  
```  
struct SSVARIANT {  
   SSVARTYPE vt;  
   DWORD dwReserved1;  
   DWORD dwReserved2;  
   union {  
// ...  
      DBTIMESTAMP tsDateTimeVal;  
      DBDATE dDateVal;  
      struct _Time2Val {  
         DBTIME2 tTime2Val;  
         BYTE bScale;  
      } Time2Val;  
      struct _DateTimeVal {  
         DBTIMESTAMP tsDateTimeVal;  
         BYTE bScale;  
      } DateTimeVal;  
      struct _DateTimeOffsetVal {   
         DBTIMESTAMPOFFSET tsoDateTimeOffsetVal;  
         BYTE bScale;  
      } DateTimeOffsetVal;  
// ...  
   };  
};  
```  
  
 De plus, l'enum associé à l'encodage de type SSVARIANT, qui détermine le type de l'enum sera étendu comme suit :  
  
```  
enum SQLVARENUM {  
// ...  
   // Datetime  
   VT_SS_DATETIME      = DBTYPE_DBTIMESTAMP,  
   VT_SS_SMALLDATETIME = 206,  
  
   VT_SS_DATE = DBTYPE_DBDATE,  
   VT_SS_TIME2 = DBTYPE_DBTIME2,  
   VT_SS_DATETIME2 = 212  
   VT_SS_DATETIMEOFFSET = DBTYPE_DBTIMESTAMPOFFSET  
};  
```  
  
 Migration vers le pilote OLE DB pour SQL Server les applications qui utilisent **sql_variant** et s’appuient sur la précision limitée de **datetime** devront être mis à jour si le schéma sous-jacent est mis à jour pour utiliser **datetime2** plutôt que **datetime**.  
  
 Les macros d'accès pour SSVARIANT ont également été étendues avec l'addition des éléments suivants :  
  
```  
#define V_SS_DATETIME2(X)       V_SS_UNION(X, DateTimeVal)  
#define V_SS_TIME2(X)           V_SS_UNION(X, Time2Val)  
#define V_SS_DATE(X)            V_SS_UNION(X, dDateVal)  
#define V_SS_DATETIMEOFFSET(X)  V_SS_UNION(X, DateTimeOffsetVal)  
```  
  
## <a name="data-type-mapping-in-itabledefinitioncreatetable"></a>Mappage des types de données dans ITableDefinition::CreateTable  
 Le mappage de type suivant est utilisé avec les structures DBCOLUMNDESC utilisées par ITableDefinition::CreateTable :  
  
|Type de données OLE DB (*wType*)|Type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|Remarques|  
|----------------------------------|-----------------------------------------|-----------|  
|DBTYPE_DBDATE|date||  
|DBTYPE_DBTIMESTAMP|**datetime2**(p)|Le pilote OLE DB pour SQL Server inspecte les membres *bScale* membre pour déterminer la précision en fractions de seconde.|  
|DBTYPE_DBTIME2|**temps**(p)|Le pilote OLE DB pour SQL Server inspecte les membres *bScale* membre pour déterminer la précision en fractions de seconde.|  
|DBTYPE_DBTIMESTAMPOFFSET|**datetimeoffset**(p)|Le pilote OLE DB pour SQL Server inspecte les membres *bScale* membre pour déterminer la précision en fractions de seconde.|  
  
 Lorsqu’une application spécifie DBTYPE_DBTIMESTAMP dans *wType*, il peut remplacer le mappage à **datetime2** , vous devez fournir un nom de type dans *pwszTypeName*. Si **datetime** est spécifié, *bScale* doit être 3. Si **smalldatetime** est spécifié, *bScale* doit être 0. Si *bScale* n’est pas cohérent avec *wType* et *pwszTypeName*, DB_E_BADSCALE est retourné.  
  
## <a name="see-also"></a>Voir aussi  
 [Date et heure améliorations & #40 ; OLE DB & #41 ;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
