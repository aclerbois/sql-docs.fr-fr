---
title: Type d’élément (DimensionAttribute) (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: f1b0a959e57d4df5db8fa4616cf8a31536cf1689
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="type-element-dimensionattribute-assl"></a>Élément Type (DimensionAttribute) (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient le type de l'attribut.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DimensionAttribute>  
      ...  
   <Type>...</Type>  
   ...  
</DimensionAttribute>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (énumération)|  
|Valeur par défaut|*Regular*|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de cet élément est limitée à l'une des chaînes répertoriées dans le tableau suivant.  
  
|Valeur| Description|  
|-----------|-----------------|  
|*Compte*|L'attribut représente le nom d'un compte.|  
|*AccountNumber*|L'attribut représente le numéro d'un compte.|  
|*Type de compte*|L'attribut représente le type d'un compte.|  
|*Adresse*|L'attribut représente une adresse.|  
|*AddressBuilding*|L'attribut représente un identificateur d'immeuble d'une adresse.|  
|*AddressCity*|L'attribut représente une ville d'une adresse.|  
|*AddressCountry*|L'attribut représente un pays ou une région d'une adresse.|  
|*AddressFax*|L'attribut représente un numéro de télécopie.|  
|*AddressFloor*|L'attribut représente un identificateur d'étage d'une adresse.|  
|*AddressHouse*|L'attribut représente un numéro de maison d'une adresse.|  
|*AddressPhone*|L'attribut représente un numéro de téléphone.|  
|*AddressQuarter*|L'attribut représente un quartier d'une adresse.|  
|*AddressRoom*|L'attribut représente un identificateur de pièce d'une adresse.|  
|*AddressStateOrProvince*|L'attribut représente un département ou une région d'une adresse.|  
|*AddressStreet*|L'attribut représente la rue d'une adresse.|  
|*AddressZip*|L'attribut représente un code postal d'une adresse.|  
|*BOMResource*|L'attribut représente une ressource d'une nomenclature.|  
|*Légende*|L'attribut représente une légende.|  
|*CaptionAbbreviation*|L'attribut représente une abréviation.|  
|*CaptionDescription*|L'attribut représente une description.|  
|*Channel*|L'attribut représente un canal.|  
|*Ville*|L'attribut représente une ville.|  
|*Société*|L'attribut représente une société.|  
|*Continent*|L'attribut représente un continent.|  
|*Pays*|L'attribut représente un pays ou une région.|  
|*Comté*|L'attribut représente un pays.|  
|*CurrencyDestination*|L'attribut représente la devise de destination d'une opération de change.|  
|*CurrencyISOcode*|L'attribut représente le code ISO d'une devise.|  
|*CurrencName*|L'attribut représente le nom d'une devise.|  
|*CurrencySource*|L'attribut représente la devise source d'une opération de change.|  
|*CustomerGroup*|L'attribut représente un groupe de clients.|  
|*CustomerHousehold*|L'attribut représente une famille de clients.|  
|*Customers*|L'attribut représente un client.|  
|*Date*|L'attribut représente une date.|  
|*DateCanceled*|L'attribut représente une date d'annulation.|  
|*DateDuration*|L'attribut représente une durée.|  
|*DateEnded*|L'attribut représente une date de fin.|  
|*DateModified*|L'attribut représente une date de modification.|  
|*DateStart*|L'attribut représente une date de début.|  
|*DayOfHalfYears*|L'attribut représente l'ordinal de jour d'un semestre.|  
|*dayOfMonth*|L'attribut représente l'ordinal de jour d'un mois.|  
|*DayOfQuarter*|L'attribut représente l'ordinal de jour d'un trimestre.|  
|*DayOfTrimester*|L'attribut représente l'ordinal de jour d'un quadrimestre.|  
|*dayOfWeek*|L'attribut représente l'ordinal de jour d'une semaine.|  
|*dayOfYear*|L'attribut représente l'ordinal de jour d'une année.|  
|*Jours d’utilisation*|L'attribut représente des jours.|  
|*DaysOfTenDays*|L'attribut représente l'ordinal de jour d'une période de dix jours.|  
|*FiscalDay*|L'attribut représente des jours dans un calendrier fiscal.|  
|*FiscalDayOfHalfYears*|L'attribut représente l'ordinal de jour d'un semestre dans un calendrier fiscal.|  
|*FiscalDayOfMonth*|L'attribut représente l'ordinal de jour d'un mois dans un calendrier fiscal.|  
|*FiscalDayOfQuarter*|L'attribut représente l'ordinal de jour d'un trimestre dans un calendrier fiscal.|  
|*FiscalDayOfTrimester*|L'attribut représente l'ordinal de jour d'un quadrimestre dans un calendrier fiscal.|  
|*FiscalDayOfWeek*|L'attribut représente l'ordinal de jour d'une semaine dans un calendrier fiscal.|  
|*FiscalDayOfYear*|L'attribut représente l'ordinal de jour d'une année dans un calendrier fiscal.|  
|*FiscalHalfYears*|L'attribut représente des semestres dans un calendrier fiscal.|  
|*FiscalHalfYearsOfYear*|L'attribut représente l'ordinal de semestre d'une année dans un calendrier fiscal.|  
|*Fiscal*|L'attribut représente des mois dans un calendrier fiscal.|  
|*FiscalMonthOfHalfYears*|L'attribut représente l'ordinal de mois d'un semestre dans un calendrier fiscal.|  
|*FiscalMonthOfQuarter*|L'attribut représente l'ordinal de mois d'un trimestre dans un calendrier fiscal.|  
|*FiscalMonthOfTrimester*|L'attribut représente l'ordinal de mois d'un quadrimestre dans un calendrier fiscal.|  
|*FiscalMonthOfYear*|L'attribut représente l'ordinal de mois d'une année dans un calendrier fiscal.|  
|*FiscalQuarter*|L'attribut représente des trimestres dans un calendrier fiscal.|  
|*FiscalQuarterOfHalfYear*|L'attribut représente l'ordinal de trimestre d'un semestre dans un calendrier fiscal.|  
|*FiscalQuarterOfYear*|L'attribut représente l'ordinal de trimestre d'une année dans un calendrier fiscal.|  
|*FiscalTrimester*|L'attribut représente des quadrimestres dans un calendrier fiscal.|  
|*FiscalTrimesterOfYear*|L'attribut représente l'ordinal de quadrimestre d'une année dans un calendrier fiscal.|  
|*FiscalWeek*|L'attribut représente des semaines dans un calendrier fiscal.|  
|*FiscalWeekOfHalfYears*|L'attribut représente l'ordinal de semaine d'un semestre dans un calendrier fiscal.|  
|*FiscalWeekOfMonth*|L'attribut représente l'ordinal de semaine d'un mois dans un calendrier fiscal.|  
|*FiscalWeekOfQuarter*|L'attribut représente l'ordinal de semaine d'un trimestre dans un calendrier fiscal.|  
|*FiscalWeekOfTrimester*|L'attribut représente l'ordinal de semaine d'un quadrimestre dans un calendrier fiscal.|  
|*FiscalWeekOfYear*|L'attribut représente l'ordinal de semaine d'une année dans un calendrier fiscal.|  
|*FiscalYear*|L'attribut représente des années dans un calendrier fiscal.|  
|*FormattingColor*|L'attribut représente la couleur utilisée dans une mise en forme.|  
|*FormattingFont*|L'attribut représente la police utilisée dans une mise en forme.|  
|*FormattingFontEffects*|L'attribut représente les effets de police utilisés dans une mise en forme.|  
|*FormattingFontSize*|L'attribut représente la taille de police utilisée dans une mise en forme.|  
|*FormattingOrder*|L'attribut représente le classement utilisé dans une mise en forme.|  
|*FormattingSubtotal*|L'attribut représente un sous-total.|  
|*GeoBoundaryBottom*|L'attribut représente la valeur inférieure d'une limite géographique.|  
|*GeoBoundaryFront*|L'attribut représente la valeur avant d'une limite géographique.|  
|*GeoBoundaryLeft*|L'attribut représente la valeur la plus à gauche d'une limite géographique.|  
|*GeoBoundaryPolygon*|L'attribut représente la définition polygonale d'une limite géographique.|  
|*GeoBoundaryRear*|L'attribut représente la valeur arrière d'une limite géographique.|  
|*GeoBoundaryRight*|L'attribut représente la valeur la plus à droite d'une limite géographique.|  
|*GeoBoundaryTop*|L'attribut représente la valeur supérieure d'une limite géographique.|  
|*GeoCentroidX*|L'attribut représente le centroïde de l'axe des X d'une région géographique.|  
|*GeoCentroidY*|L'attribut représente le centroïde de l'axe des Y d'une région géographique.|  
|*GeoCentroidZ*|L'attribut représente le centroïde de l'axe des Z d'une région géographique.|  
|*HalfYears*|L'attribut représente des semestres.|  
|*HalfYearsOfYear*|L'attribut représente l'ordinal de semestre d'une année.|  
|*Heures*|L'attribut représente des heures.|  
|*Id*|L'attribut représente un identificateur ou une clé.|  
|*IsHoliday*|L'attribut indique si une date est un congé.|  
|*ISO8601DayOfWeek*|L'attribut représente l'ordinal de jour d'une semaine dans un calendrier ISO 8601.|  
|*ISO8601DayOfYear*|L'attribut représente l'ordinal de jour d'une année dans un calendrier ISO 8601.|  
|*ISO8601Days*|L'attribut représente des jours dans un calendrier ISO 8601.|  
|*ISO8601Week*|L'attribut représente des semaines dans un calendrier ISO 8601.|  
|*ISO8601WeekOfYear*|L'attribut représente l'ordinal de semaine d'une année dans un calendrier ISO 8601.|  
|*ISO8601Year*|L'attribut représente des années dans un calendrier ISO 8601.|  
|*IsWeekDay*|L'attribut indique si une date est un jour de semaine.|  
|*IsWorkingDay*|L'attribut indique si une date est un jour ouvré.|  
|*ManufacturingDay*|L'attribut représente des jours dans un calendrier de production.|  
|*ManufacturingDayOfHalfYears*|L'attribut représente l'ordinal de jour d'un semestre dans un calendrier de production.|  
|*ManufacturingDayOfMonth*|L'attribut représente l'ordinal de jour d'un mois dans un calendrier de production.|  
|*ManufacturingDayOfQuarter*|L'attribut représente l'ordinal de jour d'un trimestre dans un calendrier de production.|  
|*ManufacturingDayOfTrimester*|L'attribut représente l'ordinal de jour d'un quadrimestre dans un calendrier de production.|  
|*ManufacturingDayOfWeek*|L'attribut représente l'ordinal de jour d'une semaine dans un calendrier de production.|  
|*ManufacturingDayOfYear*|L'attribut représente l'ordinal de jour d'une année dans un calendrier de production.|  
|*ManufacturingHalfYears*|L'attribut représente les semestres dans un calendrier de production.|  
|*ManufacturingHalfYearsOfYear*|L'attribut représente l'ordinal de semestre d'une année dans un calendrier de production.|  
|*ManufacturingMonth*|L'attribut représente des mois dans un calendrier de production.|  
|*ManufacturingMonthOfHalfYears*|L'attribut représente l'ordinal de mois d'un semestre dans un calendrier de production.|  
|*ManufacturingMonthOfQuarter*|L'attribut représente l'ordinal de mois d'un trimestre dans un calendrier de production.|  
|*ManufacturingMonthOfTrimester*|L'attribut représente l'ordinal de mois d'un quadrimestre dans un calendrier de production.|  
|*ManufacturingMonthOfYear*|L'attribut représente l'ordinal de mois d'une année dans un calendrier de production.|  
|*ManufacturingQuarter*|L'attribut représente des trimestres dans un calendrier de production.|  
|*ManufacturingQuarterOfHalfYear*|L'attribut représente l'ordinal de trimestre d'un semestre dans un calendrier de production.|  
|*ManufacturingQuarterOfYear*|L'attribut représente l'ordinal de trimestre d'une année dans un calendrier de production.|  
|*ManufacturingTrimester*|L'attribut représente des quadrimestres dans un calendrier de production.|  
|*ManufacturingTrimesterOfYear*|L'attribut représente l'ordinal de quadrimestre d'une année dans un calendrier de production.|  
|*ManufacturingWeek*|L'attribut représente des semaines dans un calendrier de production.|  
|*ManufacturingWeekOfHalfYears*|L'attribut représente l'ordinal de semaine d'un semestre dans un calendrier de production.|  
|*ManufacturingWeekOfMonth*|L'attribut représente l'ordinal de semaine d'un mois dans un calendrier de production.|  
|*ManufacturingWeekOfQuarter*|L'attribut représente l'ordinal de semaine d'un trimestre dans un calendrier de production.|  
|*ManufacturingWeekOfTrimester*|L'attribut représente l'ordinal de semaine d'un quadrimestre dans un calendrier de production.|  
|*ManufacturingWeekOfYear*|L'attribut représente l'ordinal de semaine d'une année dans un calendrier de production.|  
|*ManufacturingYear*|L'attribut représente des années dans un calendrier de production.|  
|*Minutes*|L'attribut représente des minutes.|  
|*MonthOfHalfYears*|L'attribut représente l'ordinal de mois d'un semestre.|  
|*MonthOfQuarter*|L'attribut représente l'ordinal de mois d'un trimestre.|  
|*MonthOfTrimester*|L'attribut représente l'ordinal de mois d'un quadrimestre.|  
|*MonthOfYear*|L'attribut représente l'ordinal de mois d'une année.|  
|*Mois*|L'attribut représente des mois.|  
|*Unité d’organisation*|L'attribut représente une unité d'organisation.|  
|*OrgTitle*|L'attribut représente un titre d'organisation.|  
|*PercentOwnership*|L'attribut représente un pourcentage de propriété.|  
|*PercentVoteRight*|L'attribut représente un pourcentage de droits de vote.|  
|*Personne*|L'attribut représente une personne.|  
|*PersonContact*|L'attribut représente les coordonnées d'une personne.|  
|*PersonDemographic*|L'attribut représente les informations démographiques d'une personne.|  
|*PersonFirstName*|L'attribut représente le prénom d'une personne.|  
|*PersonFullName*|L'attribut représente le nom complet d'une personne.|  
|*PersonLastName*|L'attribut représente le nom d'une personne.|  
|*PersonMiddleName*|L'attribut représente le deuxième prénom d'une personne.|  
|*PhysicalColor*|L'attribut représente une couleur.|  
|*PhysicalDensity*|L'attribut représente une densité.|  
|*PhysicalDepth*|L'attribut représente une profondeur.|  
|*PhysicalHeight*|L'attribut représente une hauteur.|  
|*PhysicalSize*|L'attribut représente une taille.|  
|*PhysicalVolume*|L'attribut représente un volume.|  
|*PhysicalWeight*|L'attribut représente un poids.|  
|*PhysicalWidth*|L'attribut représente une largeur.|  
|*Point*|L'attribut représente un point.|  
|*PostalCode*|L'attribut représente un code postal.|  
|*Product*|L'attribut représente un produit.|  
|*ProductBrand*|L'attribut représente une marque de produit.|  
|*ProductCategory*|L'attribut représente une catégorie de produit.|  
|*ProductGroup*|L'attribut représente un groupe de produits.|  
|*ProductSKU*|L'attribut représente un SKU de produit.|  
|*ProjectCode*|L'attribut représente un code de projet.|  
|*Projectcompletion*|L'attribut représente l'état d'achèvement d'un projet.|  
|*ProjectEnddate*|L'attribut représente une date de fin de projet.|  
|*Nom du projet*|L'attribut représente un nom de projet.|  
|*ProjectStartDate*|L'attribut représente une date de début de projet.|  
|*Promotion*|L'attribut représente une promotion.|  
|*QtyRangeHigh*|L'attribut représente la valeur maximale d'une gamme de quantités.|  
|*QtyRangeLow*|L'attribut représente la valeur minimale d'une gamme de quantités.|  
|*Quantitative*|L'attribut représente un attribut quantitatif.|  
|*QuarterOfHalfYear*|L'attribut représente l'ordinal de trimestre d'un semestre.|  
|*QuarterOfYear*|L'attribut représente l'ordinal de trimestre d'une année.|  
|*Trimestres*|L'attribut représente des trimestres.|  
|*Taux de*|L'attribut représente un taux.|  
|*RateType*|L'attribut représente un type de taux.|  
|*Region*|L'attribut représente une région définie par le client.|  
|*Regular*|L'attribut représente un attribut régulier.|  
|*RelationToParent*|L'attribut représente une relation avec un parent.|  
|*ReportingDay*|L'attribut représente des jours dans un calendrier de rapports.|  
|*ReportingDayOfHalfYears*|L'attribut représente l'ordinal de jour d'un semestre dans un calendrier de rapports.|  
|*ReportingDayOfMonth*|L'attribut représente l'ordinal de jour d'un mois dans un calendrier de rapports.|  
|*ReportingDayOfQuarter*|L'attribut représente l'ordinal de jour d'un trimestre dans un calendrier de rapports.|  
|*ReportingDayOfTrimester*|L'attribut représente l'ordinal de jour d'un quadrimestre dans un calendrier de rapports.|  
|*ReportingDayOfWeek*|L'attribut représente l'ordinal de jour d'une semaine dans un calendrier de rapports.|  
|*ReportingDayOfYear*|L'attribut représente l'ordinal de jour d'une année dans un calendrier de rapports.|  
|*ReportingHalfYears*|L'attribut représente des semestres dans un calendrier de rapports.|  
|*ReportingHalfYearsOfYear*|L'attribut représente l'ordinal de semestre d'une année dans un calendrier de rapports.|  
|*ReportingMonth*|L'attribut représente des mois dans un calendrier de rapports.|  
|*ReportingMonthOfHalfYears*|L'attribut représente l'ordinal de mois d'un semestre dans un calendrier de rapports.|  
|*ReportingMonthOfQuarter*|L'attribut représente l'ordinal de mois d'un trimestre dans un calendrier de rapports.|  
|*ReportingMonthOfTrimester*|L'attribut représente l'ordinal de mois d'un quadrimestre dans un calendrier de rapports.|  
|*ReportingMonthOfYear*|L'attribut représente l'ordinal de mois d'une année dans un calendrier de rapports.|  
|*ReportingQuarter*|L'attribut représente des trimestres dans un calendrier de rapports.|  
|*ReportingQuarterOfHalfYear*|L'attribut représente l'ordinal de trimestre d'un semestre dans un calendrier de rapports.|  
|*ReportingQuarterOfYear*|L'attribut représente l'ordinal de trimestre d'une année dans un calendrier de rapports.|  
|*ReportingTrimester*|L'attribut représente des quadrimestres dans un calendrier de rapports.|  
|*ReportingTrimesterOfYear*|L'attribut représente l'ordinal de quadrimestre d'une année dans un calendrier de rapports.|  
|*ReportingWeek*|L'attribut représente des semaines dans un calendrier de rapports.|  
|*ReportingWeekOfHalfYears*|L'attribut représente l'ordinal de semaine d'un semestre dans un calendrier de rapports.|  
|*ReportingWeekOfMonth*|L'attribut représente l'ordinal de semaine d'un mois dans un calendrier de rapports.|  
|*ReportingWeekOfQuarter*|L'attribut représente l'ordinal de semaine d'un trimestre dans un calendrier de rapports.|  
|*ReportingWeekOfTrimester*|L'attribut représente l'ordinal de semaine d'un quadrimestre dans un calendrier de rapports.|  
|*ReportingWeekOfYear*|L'attribut représente l'ordinal de semaine d'une année dans un calendrier de rapports.|  
|*ReportingYear*|L'attribut représente des années dans un calendrier de rapports.|  
|*Représentant*|L'attribut représente un représentant.|  
|*Scénario*|L'attribut représente un scénario.|  
|*Secondes*|L'attribut représente des secondes.|  
|*Sequence*|L'attribut représente un attribut séquentiel.|  
|*ShortCaption*|L'attribut représente une courte légende.|  
|*StateOrProvince*|L'attribut représente un département ou une région.|  
|*TenDayOfHalfYears*|L'attribut représente l'ordinal d'une période de dix jours d'un semestre.|  
|*TenDayOfQuarter*|L'attribut représente l'ordinal d'une période de dix jours d'un trimestre.|  
|*TenDayOfTrimester*|L'attribut représente l'ordinal d'une période de dix jours d'un quadrimestre.|  
|*TenDayOfYear*|L'attribut représente l'ordinal d'une période de dix jours d'une année.|  
|*TenDays*|L'attribut représente des périodes de dix jours.|  
|*TenDaysOfMonth*|L'attribut représente l'ordinal d'une période de dix jours d'un mois.|  
|*Quadrimestre*|L'attribut représente des quadrimestres.|  
|*TrimesterOfYear*|L'attribut représente l'ordinal de quadrimestre d'une année.|  
|*UndefinedTime*|L'attribut représente une période indéfinie.|  
|*Utilitaire*|L'attribut représente un utilitaire.|  
|*Version*|L'attribut représente une version.|  
|*WebHtml*|L'attribut représente du contenu HTML.|  
|*WebMailAlias*|L'attribut représente un alias de messagerie électronique.|  
|*WebUrl*|L'attribut représente une adresse URL.|  
|*WebXmlOrXsl*|L'attribut représente du contenu XML ou XSL.|  
|*WeekOfHalfYears*|L'attribut représente l'ordinal de semaine d'un semestre.|  
|*WeekOfMonth*|L'attribut représente l'ordinal de semaine d'un mois.|  
|*WeekOfQuarter*|L'attribut représente l'ordinal de semaine d'un trimestre.|  
|*WeekOfTrimester*|L'attribut représente l'ordinal de semaine d'un quadrimestre.|  
|*WeekOfYear*|L'attribut représente l'ordinal de semaine d'une année.|  
|*Semaines*|L'attribut représente des semaines.|  
|*Années*|L'attribut représente des années.|  
  
 L’énumération qui correspond aux valeurs autorisées pour **Type** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.AttributeType>.  
  
 L’élément qui correspond au parent de **Type** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.DimensionAttribute>.  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs d’élément &#40;ASSL&#41;](../../../analysis-services/scripting/collections/attributes-element-assl.md)   
 [Élément dimension & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/dimension-element-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
