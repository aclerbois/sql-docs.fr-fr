---
title: Création de Tables SQL Server | Documents Microsoft
description: Création de tables SQL Server à l’aide du pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-tables-indexes
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- OLE DB Driver for SQL Server, tables
- DBCOLUMNDESC usage
- adding tables
- CreateTable function
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 5497a74c256282fd14f7c5301f7eea4cfa9aa596
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-sql-server-tables"></a>Création de tables SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Le pilote OLE DB pour SQL Server expose la **ITableDefinition::CreateTable** fonction, ce qui permet aux consommateurs de créer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tables. Les consommateurs utilisent **CreateTable** pour créer des tables permanentes nommées consommateur et des tables permanentes ou temporaires avec un nom unique généré par le pilote OLE DB pour SQL Server.  
  
 Lorsque le consommateur appelle **ITableDefinition::CreateTable**, si la valeur de la propriété DBPROP_TBL_TEMPTABLE est VARIANT_TRUE, le pilote OLE DB pour SQL Server génère un nom de table temporaire pour le consommateur. Le consommateur attribue la *pTableID* paramètre de la **CreateTable** méthode avec la valeur NULL. Les tables temporaires avec les noms générés par le pilote OLE DB pour SQL Server n’apparaissent pas dans le **TABLES** ensemble de lignes, mais sont accessibles via la **IOpenRowset** interface.  
  
 Lorsque les consommateurs spécifient le nom de table dans le *pwszName* membre de la *uName* union dans la *pTableID* paramètre, le pilote OLE DB pour SQL Server crée un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]table portant ce nom. Des contraintes en matière d'affectation de noms aux tables [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'appliquent et le nom de la table peut indiquer une table permanente ou une table temporaire locale ou globale. Pour plus d’informations, consultez [CREATE TABLE](../../../t-sql/statements/create-table-transact-sql.md). Le *ppTableID* paramètre peut être NULL.  
  
 Le pilote OLE DB pour SQL Server peut générer les noms des tables permanentes ou temporaires. Lorsque le consommateur définit la *pTableID* paramètre NULL et *ppTableID* pour pointer vers un DBID valid\*, le pilote OLE DB pour SQL Server retourne le nom généré de la table dans le *pwszName* membre de la *uName* union de DBID désigné par la valeur de *ppTableID*. Pour créer un dossier temporaire, pilote OLE DB pour la table nommée de SQL Server, le consommateur inclut la propriété de table OLE DB DBPROP_TBL_TEMPTABLE dans une propriété table référencée dans le *rgPropertySets* paramètre. Pilote OLE DB pour les tables temporaires nommées de SQL Server sont locaux.  
  
 **CreateTable** retourne DB_E_BADTABLEID si le *eKind* membre de la *pTableID* paramètre n’indique pas DBKIND_NAME.  
  
## <a name="dbcolumndesc-usage"></a>Utilisation de la structure DBCOLUMNDESC  
 Le consommateur peut indiquer un type de données de colonne à l’aide du *pwszTypeName* membre ou le *wType* membre. Si le consommateur Spécifie le type de données dans *pwszTypeName*, le pilote OLE DB pour SQL Server ignore la valeur de *wType*.  
  
 Si vous utilisez la *pwszTypeName* membre, le consommateur Spécifie le type de données à l’aide de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les noms de type de données. Les noms de type de données valides sont les noms retournés dans la colonne TYPE_NAME de l'ensemble de lignes de schéma PROVIDER_TYPES.  
  
 Le pilote OLE DB pour SQL Server reconnaît un sous-ensemble de valeurs DBTYPE énumérées OLE DB dans le *wType* membre. Pour plus d’informations, consultez [mappage de Type de données dans ITableDefinition](../../oledb/ole-db-data-types/data-type-mapping-in-itabledefinition.md).  
  
> [!NOTE]  
>  **CreateTable** retourne DB_E_BADTYPE si le consommateur le *pTypeInfo* ou *pclsid* membre pour spécifier le type de données de colonne.  
  
 Le consommateur Spécifie le nom de colonne dans la *pwszName* membre de la *uName* union de la structure DBCOLUMNDESC *dbcid* membre. Le nom de colonne est spécifié en tant que chaîne de caractères Unicode. Le *eKind* membre *dbcid* doit être DBKIND_NAME. **CreateTable** retourne DB_E_BADCOLUMNID si *eKind* n’est pas valide, *pwszName* est NULL, ou si la valeur de *pwszName* n’est pas valide [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] identificateur.  
  
 Toutes les propriétés de colonne sont disponibles dans toutes les colonnes définies pour la table. **CreateTable** peut retourner DB_S_ERRORSOCCURRED ou DB_E_ERRORSOCCURRED si les valeurs de propriété sont définies en conflit. **CreateTable** renvoie une erreur lorsque les paramètres de propriété de colonne non valide provoquent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Échec de la création de la table.  
  
 Les propriétés des colonnes dans une structure DBCOLUMNDESC se définissent de la manière suivante.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|DBPROP_COL_AUTOINCREMENT|R/W : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE Description : définit la propriété d'identité dans la colonne créée. Pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la propriété d'identité est valide pour une colonne unique au sein d'une table. Définissez la propriété à VARIANT_TRUE pour plus d’informations qu’une seule colonne génère une erreur lorsque le pilote OLE DB pour SQL Server tente de créer la table sur le serveur.<br /><br /> Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] propriété d’identité est valide uniquement pour les **entier**, **numérique**, et **décimal** types lors de l’échelle est 0. La propriété à VARIANT_TRUE sur une colonne de tout autre type de données génère une erreur lorsque le pilote OLE DB pour SQL Server tente de créer la table sur le serveur.<br /><br /> Le pilote OLE DB pour SQL Server retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_AUTOINCREMENT et DBPROP_COL_NULLABLE sont les deux VARIANT_TRUE et *dwOption* de DBPROP_COL_NULLABLE n’est pas DBPROPOPTIONS_REQUIRED. DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_AUTOINCREMENT et DBPROP_COL_NULLABLE sont les deux VARIANT_TRUE et *dwOption* de DBPROP_COL_NULLABLE est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] propriété d’identité et la DBPROP_COL_NULLABLE *dwStatus* membre est défini sur DBPROPSTATUS_CONFLICTING.|  
|DBPROP_COL_DEFAULT|R/W : lecture/écriture<br /><br /> Valeur par défaut : aucune<br /><br /> Description : crée une contrainte DEFAULT [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour la colonne.<br /><br /> Le *vValue* membre DBPROP peut être un nombre quelconque de types. Le *vValue.vt* membre doit spécifier un type compatible avec le type de données de la colonne. Par exemple, le choix de BSTR N/A comme valeur par défaut d'une colonne définie en tant que DBTYPE_WSTR offre une valeur de correspondance compatible. Définition de la même valeur par défaut sur une colonne en tant que DBTYPE_R8 une erreur lorsque le pilote OLE DB pour SQL Server tente de créer la table sur le serveur.|  
|DBPROP_COL_DESCRIPTION|R/W : lecture/écriture<br /><br /> Valeur par défaut : aucune<br /><br /> Description : La propriété de colonne DBPROP_COL_DESCRIPTION n’est pas implémentée par le pilote OLE DB pour SQL Server.<br /><br /> Le *dwStatus* membre de la structure DBPROP retourne DBPROPSTATUS_NOTSUPPORTED lorsque le consommateur tente d’écrire la valeur de propriété.<br /><br /> Définition de la propriété ne constitue pas une erreur irrécupérable pour le pilote OLE DB pour SQL Server. Si toutes les autres valeurs de paramètres sont valides, la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est créée.|  
|DBPROP_COL_FIXEDLENGTH|R/W : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : Le pilote OLE DB pour SQL Server utilise DBPROP_COL_FIXEDLENGTH pour déterminer le mappage de type de données lorsque le consommateur définit le type de données d’une colonne à l’aide de la *wType* membre de la structure DBCOLUMNDESC. Pour plus d’informations, consultez [mappage de Type de données dans ITableDefinition](../../oledb/ole-db-data-types/data-type-mapping-in-itabledefinition.md).|  
|DBPROP_COL_NULLABLE|R/W : lecture/écriture<br /><br /> Valeur par défaut : aucune<br /><br /> Description : Lorsque vous créez la table, le pilote OLE DB pour SQL Server indique si la colonne doit accepter les valeurs null si la propriété est définie. Lorsque la propriété n'est pas définie, l'aptitude de la colonne à valider la valeur NULL dépend de l'option de base de données par défaut ANSI_NULLS de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Le pilote OLE DB pour SQL Server est un fournisseur conforme à ISO. Les sessions connectées affichent des comportements ISO. Si le consommateur ne définit pas DBPROP_COL_NULLABLE, les colonnes acceptent des valeurs NULL.|  
|DBPROP_COL_PRIMARYKEY|R/W : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE Description : VARIANT_TRUE lorsque, le pilote OLE DB pour SQL Server crée la colonne avec une contrainte PRIMARY KEY.<br /><br /> Si la propriété est définie comme une propriété de colonne, seule une colonne unique peut déterminer la contrainte. Définition de la propriété sur VARIANT_TRUE pour plusieurs colonnes uniques retourne une erreur lorsque le pilote OLE DB pour SQL Server tente de créer le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table.<br /><br /> Remarque : Le consommateur peut utiliser **IIndexDefinition::CreateIndex** pour créer une contrainte PRIMARY KEY sur deux ou plusieurs colonnes.<br /><br /> Le pilote OLE DB pour SQL Server retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont les deux VARIANT_TRUE et *dwOption* de DBPROP_COL_UNIQUE n’est pas DBPROPOPTIONS_REQUIRED.<br /><br /> DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont les deux VARIANT_TRUE et *dwOption* de DBPROP_COL_UNIQUE est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] propriété d’identité et la DBPROP_COL_PRIMARYKEY *dwStatus* membre est défini sur DBPROPSTATUS_CONFLICTING.<br /><br /> Le pilote OLE DB pour SQL Server retourne une erreur lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_NULLABLE sont les deux VARIANT_TRUE.<br /><br /> Le pilote OLE DB pour SQL Server retourne une erreur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lorsque le consommateur tente de créer une contrainte PRIMARY KEY sur une colonne non valides [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données. Les contraintes PRIMARY KEY ne peut pas être définies sur les colonnes créées avec le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] des types de données **bits**, **texte**, **ntext**, et **image**.|  
|DBPROP_COL_UNIQUE|R/W : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE Description : applique une contrainte UNIQUE [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à la colonne.<br /><br /> Si la propriété est définie comme une propriété de colonne, la contrainte est appliquée dans une seule colonne uniquement. Le consommateur peut utiliser **IIndexDefinition::CreateIndex** pour appliquer une contrainte UNIQUE sur les valeurs combinées des deux colonnes ou plus.<br /><br /> Le pilote OLE DB pour SQL Server retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont les deux VARIANT_TRUE et *dwOption* n’est pas DBPROPOPTIONS_REQUIRED.<br /><br /> DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont les deux VARIANT_TRUE et *dwOption* est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] propriété d’identité et la DBPROP_COL_PRIMARYKEY *dwStatus* membre est défini sur DBPROPSTATUS_CONFLICTING.<br /><br /> Le pilote OLE DB pour SQL Server retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_NULLABLE et DBPROP_COL_UNIQUE sont les deux VARIANT_TRUE et *dwOption* n’est pas DBPROPOPTIONS_REQUIRED.<br /><br /> DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_NULLABLE et DBPROP_COL_UNIQUE sont les deux VARIANT_TRUE et *dwOption* est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] propriété d’identité et la DBPROP_COL_NULLABLE *dwStatus* membre est défini sur DBPROPSTATUS_CONFLICTING.<br /><br /> Le pilote OLE DB pour SQL Server retourne une erreur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] lorsque le consommateur tente de créer une contrainte UNIQUE sur une colonne non valides [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données. Contraintes UNIQUE ne peut pas être définies sur les colonnes créées avec le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **bits** type de données.|  
  
 Lorsque le consommateur appelle **ITableDefinition::CreateTable**, le pilote OLE DB pour SQL Server interprète les propriétés de la table comme suit.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|DBPROP_TBL_TEMPTABLE|R/W : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE Description : par défaut, le pilote OLE DB pour SQL Server crée des tables nommées par le consommateur. Lorsque la valeur VARIANT_TRUE, le pilote OLE DB pour SQL Server génère un nom de table temporaire pour le consommateur. Le consommateur attribue la *pTableID* paramètre de **CreateTable** avec la valeur NULL. Le *ppTableID* paramètre doit contenir un pointeur valide.|  
  
 Si le consommateur demande qu’un ensemble de lignes ouvert sur une table créée avec succès, le pilote OLE DB pour SQL Server ouvre un ensemble de lignes du curseur est pris en charge. Toutes les propriétés de l'ensemble de lignes peuvent être indiquées dans les jeux de propriétés passés.  
  
 L'exemple ci-dessous crée une table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```  
// This CREATE TABLE statement shows the details of the table created by   
// the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//    CONSTRAINT PK_OrderDetails  
//         PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
// The PRIMARY KEY constraint is created in an additional example.  
HRESULT CreateTable  
    (  
    ITableDefinition* pITableDefinition  
    )  
    {  
    DBID            dbidTable;  
    const ULONG     nCols = 5;  
    ULONG           nCol;  
    ULONG           nProp;  
    DBCOLUMNDESC    dbcoldesc[nCols];  
  
    HRESULT         hr;  
  
    // Set up column descriptions. First, set default property values for  
    //  the columns.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbcoldesc[nCol].pwszTypeName = NULL;  
        dbcoldesc[nCol].pTypeInfo = NULL;  
        dbcoldesc[nCol].rgPropertySets = new DBPROPSET;  
        dbcoldesc[nCol].pclsid = NULL;  
        dbcoldesc[nCol].cPropertySets = 1;  
        dbcoldesc[nCol].ulColumnSize = 0;  
        dbcoldesc[nCol].dbcid.eKind = DBKIND_NAME;  
        dbcoldesc[nCol].wType = DBTYPE_I4;  
        dbcoldesc[nCol].bPrecision = 0;  
        dbcoldesc[nCol].bScale = 0;  
  
        dbcoldesc[nCol].rgPropertySets[0].rgProperties =   
            new DBPROP[NCOLPROPS_MAX];  
        dbcoldesc[nCol].rgPropertySets[0].cProperties = NCOLPROPS_MAX;  
        dbcoldesc[nCol].rgPropertySets[0].guidPropertySet =  
            DBPROPSET_COLUMN;  
  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                dwOptions = DBPROPOPTIONS_REQUIRED;  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].colid  
                 = DB_NULLID;  
  
            VariantInit(  
                &(dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                    vValue));  
  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt = VT_BOOL;  
            }  
        }  
  
    // Set the column-specific information.  
    dbcoldesc[0].dbcid.uName.pwszName = L"OrderID";  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[0].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[1].dbcid.uName.pwszName = L"ProductID";  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[1].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[2].dbcid.uName.pwszName = L"UnitPrice";  
    dbcoldesc[2].wType = DBTYPE_CY;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[2].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[3].dbcid.uName.pwszName = L"Quantity";  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[3].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[4].dbcid.uName.pwszName = L"Discount";  
    dbcoldesc[4].wType = DBTYPE_NUMERIC;  
    dbcoldesc[4].bPrecision = 2;  
    dbcoldesc[4].bScale = 2;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].dwPropertyID =   
        DBPROP_COL_DEFAULT;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.vt = VT_BSTR;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.bstrVal =  
        SysAllocString(L"0");  
    dbcoldesc[4].rgPropertySets[0].cProperties = 2;  
  
    // Set up the dbid for OrderDetails.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    if (FAILED(hr = pITableDefinition->CreateTable(NULL, &dbidTable,  
        nCols, dbcoldesc, NULL, 0, NULL, NULL, NULL)))  
        {  
        DumpError(pITableDefinition, IID_ITableDefinition);  
        goto SAFE_EXIT;  
        }  
  
SAFE_EXIT:  
    // Clean up dynamic allocation in the property sets.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            if (dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt == VT_BSTR)  
                {  
                SysFreeString(dbcoldesc[nCol].rgPropertySets[0].  
                    rgProperties[nProp].vValue.bstrVal);  
                }  
            }  
  
        delete [] dbcoldesc[nCol].rgPropertySets[0].rgProperties;  
        delete [] dbcoldesc[nCol].rgPropertySets;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et des index](../../oledb/ole-db-tables-indexes/tables-and-indexes.md)  
  
  
