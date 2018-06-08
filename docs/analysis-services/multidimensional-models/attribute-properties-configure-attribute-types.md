---
title: Configurer les Types d’attributs | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8e7be9da1b7405aa522dc29057764e7351924b41
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="attribute-properties---configure-attribute-types"></a>Propriétés d’attribut - configurer des Types d’attributs
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], les types d’attributs permettent de classer un attribut en termes de fonctionnalité métier. Il existe un grand nombre de types d'attributs que la plupart des applications clientes utilisent pour afficher ou prendre en charge un attribut. Cependant, certains types d’attributs ont une signification particulière pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Par exemple, certains types d'attributs identifient des attributs qui représentent des périodes dans divers calendriers des dimensions de temps.  
  
##  <a name="setting_attibute_types"></a> Définition des types d'attributs  
 La valeur de la propriété **Type** d’un attribut détermine le type d’attribut de cet attribut. Différents assistants d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] définissent les types d’attributs lors de la définition des dimensions ou des attributs. Ces Assistants d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] définissent également les types d’attributs quand ils ajoutent des fonctionnalités à des dimensions. Par exemple, l'Assistant Business Intelligence applique des types d'attributs aux attributs d'une dimension lorsqu'il ajoute l'intelligence comptable pour identifier les attributs qui contiennent les noms, les codes, les numéros et la structure des comptes de la dimension. L'Assistant Business Intelligence utilise également les types d'attributs, par exemple pour la conversion de devises. Pour plus d’informations, consultez [Créer une dimension de type devise](../../analysis-services/multidimensional-models/database-dimensions-create-a-currency-type-dimension.md).  
  
 Les tableaux suivants répertorient les types d’attributs disponibles dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les types d'attributs sont classés dans les catégories suivantes dans les tableaux :  
  
|Terme|Définition|  
|----------|----------------|  
|[Types d'attributs généraux](#general_attribute_types)|Ces valeurs sont disponibles pour tous les attributs et existent uniquement pour classer les attributs pour les applications clientes.|  
|[Types d’attributs de dimension de comptes](#account_dimension_attribute_types)|Ces valeurs identifient un attribut qui appartient à une dimension de comptes. Pour plus d’informations sur les dimensions de compte, consultez [Créer un compte Finance de la dimension de type parent-enfant](../../analysis-services/multidimensional-models/database-dimensions-finance-account-of-parent-child-type.md).|  
|[Type d'attribut de dimension monétaire](#currency_dimension_attribute_types)|Ces valeurs identifient un attribut qui appartient à une dimension monétaire. Pour plus d’informations sur les dimensions de devise, consultez [Créer une dimension de type devise](../../analysis-services/multidimensional-models/database-dimensions-create-a-currency-type-dimension.md).|  
|[Attributs de dimension à variation lente](#slowly_changing_dimension_attribute_types)|Ces valeurs identifient un attribut qui appartient à une dimension à variation lente.|  
|[Attributs de dimension de temps](#time_dimension_attribute_types)|Ces valeurs identifient un attribut qui appartient à une dimension de temps. Pour plus d’informations sur les dimensions de temps, consultez [Créer une dimension de type date](../../analysis-services/multidimensional-models/database-dimensions-create-a-date-type-dimension.md).|  
  
###  <a name="general_attribute_types"></a> General Attribute Types  
  
|Valeur de type d'attribut| Description|  
|--------------------------|-----------------|  
|**Adresse**|Représente une adresse.|  
|**AddressBuilding**|Représente un identificateur d'immeuble d'une adresse.|  
|**AddressCity**|Représente une ville d'une adresse.|  
|**AddressCountry**|Représente un pays ou une région d'une adresse.|  
|**AddressFax**|Représente un numéro de télécopie.|  
|**AddressFloor**|Représente un identificateur d'étage d'une adresse.|  
|**AddressHouse**|Représente un numéro de maison d'une adresse.|  
|**AddressPhone**|Représente un numéro de téléphone.|  
|**AddressQuarter**|Représente un quartier d'une adresse.|  
|**AddressRoom**|Représente un identificateur de salle d'une adresse.|  
|**AddressStateOrProvince**|Représente un département ou une région d'une adresse.|  
|**AddressStreet**|Représente une rue d'une adresse.|  
|**AddressZip**|Représente un code postal d'une adresse.|  
|**BomResource**|Représente une ressource d'une nomenclature.|  
|**Légende**|Représente une légende.|  
|**CaptionAbbreviation**|Représente une abréviation.|  
|**CaptionDescription**|Représente une description.|  
|**Channel**|Représente un canal.|  
|**Ville**|Représente une ville.|  
|**Company**|Représente une société.|  
|**Continent**|Représente un continent.|  
|**Pays**|Représente un pays ou une région.|  
|**Comté**|Représente un pays.|  
|**CustomerGroup**|Représente un groupe de clients.|  
|**CustomerHousehold**|Représente une famille de clients.|  
|**Customers**|Représente un client.|  
|**DateCanceled**|Représente une date d'annulation.|  
|**DateDuration**|Représente une durée.|  
|**DateEnded**|Représente une date de fin.|  
|**DateModified**|Représente une date de modification.|  
|**DateStart**|Représente une date de début.|  
|**DeletedFlag**|Indique si un membre est ou doit être supprimé (en terme de fonctionnalité commerciale).<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] n’utilise pas ce type d’attribut pour déterminer si un membre doit être supprimé. Ce type d'attribut doit être utilisé par les applications clientes à des fins d'affichage uniquement.|  
|**FormattingColor**|Représente la couleur utilisée pour la mise en forme.|  
|**FormattingFont**|Représente la police utilisée pour la mise en forme.|  
|**FormattingFontEffects**|Représente les effets de police utilisés pour la mise en forme.|  
|**FormattingFontSize**|Représente la taille de police utilisée pour la mise en forme.|  
|**FormattingOrder**|Représente l'ordre utilisé pour la mise en forme.|  
|**FormattingSubtotal**|Représente un sous-total.|  
|**GeoBoundaryBottom**|Représente la valeur inférieure d'une limite géographique.|  
|**GeoBoundaryFront**|Représente la valeur avant d'une limite géographique.|  
|**GeoBoundaryLeft**|Représente la valeur la plus à gauche d'une limite géographique.|  
|**GeoBoundaryPolygon**|Représente la définition polygonale d'une limite géographique.|  
|**GeoBoundaryRear**|Représente la valeur arrière d'une limite géographique.|  
|**GeoBoundaryRight**|Représente la valeur la plus à droite d'une limite géographique.|  
|**GeoBoundaryTop**|Représente la valeur supérieure d'une limite géographique.|  
|**GeoCentroidX**|Représente le centroïde de l'axe des X d'une région géographique.|  
|**GeoCentroidY**|Représente le centroïde de l'axe des Y d'une région géographique.|  
|**GeoCentroidZ**|Représente le centroïde de l'axe des Z d'une région géographique.|  
|**ID**|Représente un identificateur (ID) ou une clé.|  
|**Image**|Représente une image dans un format graphique non défini.|  
|**ImageBmp**|Représente une image dans un format graphique bitmap.|  
|**ImageGif**|Représente une image de format graphique GIF (Graphics Interchange Format).|  
|**ImageJpg**|Représente une image de format graphique JPEG (Joint Photographic Experts Group).|  
|**ImagePng**|Représente une image de format graphique PNG (Portable Network Graphics).|  
|**ImageTiff**|Représente une image de format graphique TIFF (Tagged Image File Format).|  
|**OrganizationalUnit**|Représente une unité d'organisation.|  
|**OrgTitle**|Représente un titre d'organisation.|  
|**PercentOwnership**|Représente un pourcentage de propriété.|  
|**PercentVoteRight**|Représente un pourcentage de droits de vote.|  
|**Person**|Représente une personne.|  
|**PersonContact**|Représente les informations de contact d'une personne.|  
|**PersonDemographic**|Représente les informations démographiques d'une personne.|  
|**PersonFirstName**|Représente le prénom d'une personne.|  
|**PersonFullName**|Représente le nom complet d'une personne.|  
|**PersonLastName**|Représente le nom d'une personne.|  
|**PersonMiddleName**|Représente le deuxième prénom d'une personne.|  
|**PhysicalColor**|Représente une couleur.|  
|**PhysicalDensity**|Représente une densité.|  
|**PhysicalDepth**|Représente une profondeur.|  
|**PhysicalHeight**|Représente une hauteur.|  
|**PhysicalSize**|Représente une taille.|  
|**PhysicalVolume**|Représente un volume.|  
|**PhysicalWeight**|Représente un poids.|  
|**PhysicalWidth**|Représente une largeur.|  
|**Point**|Représente un point.|  
|**PostalCode**|Représente un code postal.|  
|**Product**|Représente un produit.|  
|**ProductBrand**|Représente une marque de produit.|  
|**ProductCategory**|Représente une catégorie de produit.|  
|**ProductGroup**|Représente un groupe de produits.|  
|**ProductSKU**|Représente un SKU (Stock Keeping Unit) de produit.|  
|**Projet**|Représente un projet.|  
|**ProjectCode**|Représente un code de projet.|  
|**ProjectCompletion**|Représente l'état d'achèvement d'un projet.|  
|**ProjectEndDate**|Représente la date de fin d'un projet.|  
|**ProjectName**|Représente un nom de projet.|  
|**ProjectStartDate**|Représente la date de début d'un projet.|  
|**Promotion**|Représente une promotion.|  
|**QtyRangeHigh**|Représente la valeur maximale d'une gamme de quantités.|  
|**QtyRangeLow**|Représente la valeur minimale d'une gamme de quantités.|  
|**Quantitative**|Représente un attribut quantitatif.|  
|**Taux**|Représente un taux.|  
|**RateType**|Représente un type de taux.|  
|**Région**|Représente une région définie par le client.|  
|**Regular**|Représente un attribut régulier.|  
|**RelationToParent**|Représente une relation avec un parent.|  
|**Representative**|Représente un représentant.|  
|**Scénario**|Représente un scénario.|  
|**Sequence**|Représente un attribut de séquence.|  
|**ShortCaption**|Représente une courte légende.|  
|**StateOrProvince**|Représente un département ou une région.|  
|**Utilitaire**|Représente un utilitaire.|  
|**Version**|Représente une version.|  
|**WebHtml**|Représente un contenu HTML.|  
|**WebMailAlias**|Représente un alias de messagerie électronique.|  
|**WebUrl**|Représente une adresse URL.|  
|**WebXmlOrXsl**|Représente un contenu XML ou XSL.|  
  
###  <a name="account_dimension_attribute_types"></a> Account Dimension Attribute Types  
  
|Valeur de type d'attribut| Description|  
|--------------------------|-----------------|  
|**Compte**|Représente le parent d'un compte. Ce type d'attribut est généralement appliqué à l'attribut parent d'une dimension de comptes.|  
|**AccountName**|Représente le nom du compte. Ce type d'attribut est généralement appliqué aux attributs clés d'une dimension de comptes.|  
|**AccountNumber**|Représente le numéro d'un compte.|  
|**AccountType**|Représente le type d'un compte. Ce type d'attribut identifie la fonction d'agrégation d'un membre de compte dans une dimension de type de compte dans la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
  
###  <a name="currency_dimension_attribute_types"></a> Types d'attributs de dimension monétaire  
  
|Valeur de type d'attribut| Description|  
|--------------------------|-----------------|  
|**CurrencyDestination**|Représente la devise de destination d'une opération de change. Ce type d'attribut est généralement appliqué à l'attribut clé d'une dimension de rapport pour utilisation dans une conversion monétaire. Pour plus d’informations sur la conversion monétaire, consultez [Conversions monétaires &#40;Analysis Services&#41;](../../analysis-services/currency-conversions-analysis-services.md).|  
|**CurrencyIsoCode**|Représente le code ISO (International Standards Organization) d'une devise. Pour plus d’informations sur la conversion monétaire, consultez [Conversions monétaires &#40;Analysis Services&#41;](../../analysis-services/currency-conversions-analysis-services.md).|  
|**CurrencyName**|Représente le nom d'une devise. Pour plus d’informations sur la conversion monétaire, consultez [Conversions monétaires &#40;Analysis Services&#41;](../../analysis-services/currency-conversions-analysis-services.md).|  
|**CurrencySource**|Représente la devise source d'une opération de change. Ce type d'attribut est généralement appliqué à l'attribut clé d'une dimension monétaire pour utilisation dans une conversion monétaire. Pour plus d’informations sur la conversion monétaire, consultez [Conversions monétaires &#40;Analysis Services&#41;](../../analysis-services/currency-conversions-analysis-services.md).|  
  
###  <a name="slowly_changing_dimension_attribute_types"></a> Types d'attributs de dimension à variation lente  
  
|Valeur de type d'attribut| Description|  
|--------------------------|-----------------|  
|**ScdEndDate**|Représente la date de fin effective d'un membre dans une dimension à variation lente.|  
|**ScdOriginalID**|Représente l'identificateur d'origine d'un membre dans une dimension à variation lente.|  
|**ScdStartDate**|Représente la date de début effective d'un membre dans une dimension à variation lente.|  
|**ScdStatus**|Représente l'état effectif d'un membre dans une dimension à variation lente.|  
  
###  <a name="time_dimension_attribute_types"></a> Types d'attributs de dimension de temps  
  
|Valeur de type d'attribut| Description|  
|--------------------------|-----------------|  
|**Date**|Représente une date. Ce type d'attribut est généralement appliqué à l'attribut clé d'une dimension de temps ou d'une dimension de temps du serveur.|  
|**DayOfHalfYear**|Représente l'ordinal de jour d'un semestre.|  
|**DayOfMonth**|Représente l'ordinal de jour d'un mois.|  
|**DayOfQuarter**|Représente l'ordinal de jour d'un trimestre.|  
|**DayOfTenDays**|Représente l'ordinal de jour d'une période de dix jours.|  
|**DayOfTrimester**|Représente l'ordinal de jour d'un quadrimestre.|  
|**DayOfWeek**|Représente l'ordinal de jour d'une semaine.|  
|**DayOfYear**|Représente l'ordinal de jour d'une année.|  
|**Jours**|Représente des jours.|  
|**FiscalDate**|Représente une date dans un calendrier fiscal.|  
|**FiscalDayOfHalfYear**|Représente l'ordinal de jour d'un semestre dans un calendrier fiscal.|  
|**FiscalDayOfMonth**|Représente l'ordinal de jour d'un mois dans un calendrier fiscal.|  
|**FiscalDayOfQuarter**|Représente l'ordinal de jour d'un trimestre dans un calendrier fiscal.|  
|**FiscalDayOfTrimester**|Représente l'ordinal de jour d'un quadrimestre dans un calendrier fiscal.|  
|**FiscalDayOfWeek**|Représente l'ordinal de jour d'une semaine dans un calendrier fiscal.|  
|**FiscalDayOfYear**|Représente l'ordinal de jour d'une année dans un calendrier fiscal.|  
|**FiscalHalfYears**|Représente des semestres dans un calendrier fiscal.|  
|**FiscalHalfYearOfYear**|Représente l'ordinal de semestre d'une année dans un calendrier fiscal.|  
|**FiscalMonths**|Représente des mois dans un calendrier fiscal.|  
|**FiscalMonthOfHalfYear**|Représente l'ordinal de mois d'un semestre dans un calendrier fiscal.|  
|**FiscalMonthOfQuarter**|Représente l'ordinal de mois d'un trimestre dans un calendrier fiscal.|  
|**FiscalMonthOfTrimester**|Représente l'ordinal de mois d'un quadrimestre dans un calendrier fiscal.|  
|**FiscalMonthOfYear**|Représente l'ordinal de mois d'une année dans un calendrier fiscal.|  
|**FiscalQuarters**|Représente des trimestres dans un calendrier fiscal.|  
|**FiscalQuarterOfHalfYear**|Représente l'ordinal de trimestre d'un semestre dans un calendrier fiscal.|  
|**FiscalQuarterOfYear**|Représente l'ordinal de trimestre d'une année dans un calendrier fiscal.|  
|**FiscalTrimesters**|Représente des quadrimestres dans un calendrier fiscal.|  
|**FiscalTrimesterOfYear**|Représente l'ordinal de quadrimestre d'une année dans un calendrier fiscal.|  
|**FiscalWeeks**|Représente des semaines dans un calendrier fiscal.|  
|**FiscalWeekOfHalfYear**|Représente l'ordinal de semaine d'un semestre dans un calendrier fiscal.|  
|**FiscalWeekOfMonth**|Représente l'ordinal de semaine d'un mois dans un calendrier fiscal.|  
|**FiscalWeekOfQuarter**|Représente l'ordinal de semaine d'un trimestre dans un calendrier fiscal.|  
|**FiscalWeekOfTrimester**|Représente l'ordinal de semaine d'un quadrimestre dans un calendrier fiscal.|  
|**FiscalWeekOfYear**|Représente l'ordinal de semaine d'une année dans un calendrier fiscal.|  
|**FiscalYears**|Représente des années dans un calendrier fiscal.|  
|**HalfYears**|Représente des semestres.|  
|**HalfYearOfYear**|Représente l'ordinal de semestre d'une année.|  
|**Heures**|Représente des heures.|  
|**IsHoliday**|Indique si une date est un congé.|  
|**ISO8601Date**|Représente une date dans un calendrier ISO 8601.|  
|**ISO8601DayOfWeek**|Représente l'ordinal de jour d'une semaine dans un calendrier ISO 8601.|  
|**ISO8601DayOfYear**|Représente l'ordinal de jour d'une année dans un calendrier ISO 8601.|  
|**ISO8601Weeks**|Représente des semaines dans un calendrier ISO 8601.|  
|**ISO8601WeekOfYear**|Représente l'ordinal de semaine d'une année dans un calendrier ISO 8601.|  
|**ISO8601Years**|Représente des années dans un calendrier ISO 8601.|  
|**IsPeakDay**|Indique si une date est un jour de pointe.|  
|**IsWeekDay**|Indique si une date est un jour de semaine.|  
|**IsWorkingDay**|Indique si une date est un jour ouvré.|  
|**ManufacturingDate**|Représente une date dans calendrier de fabrication.|  
|**ManufacturingDayOfHalfYear**|Représente l'ordinal de jour d'un semestre dans un calendrier de fabrication.|  
|**ManufacturingDayOfMonth**|Représente l'ordinal de jour d'un mois dans un calendrier de fabrication.|  
|**ManufacturingDayOfQuarter**|Représente l'ordinal de jour d'un trimestre dans un calendrier de fabrication.|  
|**ManufacturingDayOfTrimester**|Représente l'ordinal de jour d'un quadrimestre dans un calendrier de fabrication.|  
|**ManufacturingDayOfWeek**|Représente l'ordinal de jour d'une semaine dans un calendrier de fabrication.|  
|**ManufacturingDayOfYear**|Représente l'ordinal de jour d'une année dans un calendrier de fabrication.|  
|**ManufacturingHalfYears**|Représente des semestres dans calendrier de fabrication.|  
|**ManufacturingHalfYearOfYear**|Représente l'ordinal de semestre d'une année dans un calendrier de fabrication.|  
|**ManufacturingMonths**|Représente des mois dans un calendrier de fabrication.|  
|**ManufacturingMonthOfHalfYear**|Représente l'ordinal de mois d'un semestre dans un calendrier de fabrication.|  
|**ManufacturingMonthOfQuarter**|Représente l'ordinal de mois d'un trimestre dans un calendrier de fabrication.|  
|**ManufacturingMonthOfTrimester**|Représente l'ordinal de mois d'un quadrimestre dans un calendrier de fabrication.|  
|**ManufacturingMonthOfYear**|Représente l'ordinal de mois d'une année dans un calendrier de fabrication.|  
|**ManufacturingQuarters**|Représente des trimestres dans un calendrier de fabrication.|  
|**ManufacturingQuarterOfHalfYear**|Représente l'ordinal de trimestre d'un semestre dans un calendrier de fabrication.|  
|**ManufacturingQuarterOfYear**|Représente l'ordinal de trimestre d'une année dans un calendrier de fabrication.|  
|**ManufacturingWeeks**|Représente des semaines dans un calendrier de fabrication.|  
|**ManufacturingWeekOfHalfYear**|Représente l'ordinal de semaine d'un semestre dans un calendrier de fabrication.|  
|**ManufacturingWeekOfMonth**|Représente l'ordinal de semaine d'un mois dans un calendrier de fabrication.|  
|**ManufacturingWeekOfQuarter**|Représente l'ordinal de semaine d'un trimestre dans un calendrier de fabrication.|  
|**ManufacturingWeekOfTrimester**|Représente l'ordinal de semaine d'un quadrimestre dans un calendrier de fabrication.|  
|**ManufacturingWeekOfYear**|Représente l'ordinal de semaine d'une année dans un calendrier de fabrication.|  
|**ManufacturingYears**|Représente des années dans un calendrier de fabrication.|  
|**Minutes**|Représente les minutes.|  
|**Mois**|Représente des mois.|  
|**MonthOfHalfYear**|Représente l’ordinal du mois d’un semestre.|  
|**MonthOfQuarter**|Représente l'ordinal de mois d'un trimestre.|  
|**MonthOfTrimester**|Représente l'ordinal de mois d'un quadrimestre.|  
|**MonthOfYear**|Représente l'ordinal de mois d'une année.|  
|**Quarters**|Représente des trimestres.|  
|**QuarterOfHalfYear**|Représente l'ordinal de trimestre d'un semestre.|  
|**QuarterOfYear**|Représente l'ordinal de trimestre d'une année.|  
|**ReportingDate**|Représente une date dans un calendrier de rapports.|  
|**ReportingDayOfHalfYear**|Représente l'ordinal de jour d'un semestre dans un calendrier de rapports.|  
|**ReportingDayOfMonth**|Représente l'ordinal de jour d'un mois dans un calendrier de rapports.|  
|**ReportingDayOfQuarter**|Représente l'ordinal de jour d'un trimestre dans un calendrier de rapports.|  
|**ReportingDayOfTrimester**|Représente l'ordinal de jour d'un quadrimestre dans un calendrier de rapports.|  
|**ReportingDayOfWeek**|Représente l'ordinal de jour d'une semaine dans un calendrier de rapports.|  
|**ReportingDayOfYear**|Représente l'ordinal de jour d'une année dans un calendrier de rapports.|  
|**ReportingHalfYears**|Représente des semestres dans un calendrier de rapports.|  
|**ReportingHalfYearOfYear**|Représente l'ordinal de semestre d'une année dans un calendrier de rapports.|  
|**ReportingMonths**|Représente les mois dans un calendrier de rapports.|  
|**ReportingMonthOfHalfYear**|Représente l'ordinal de mois d'un semestre dans un calendrier de rapports.|  
|**ReportingMonthOfQuarter**|Représente l'ordinal de mois d'un trimestre dans un calendrier de rapports.|  
|**ReportingMonthOfTrimester**|Représente l'ordinal de mois d'un quadrimestre dans un calendrier de rapports.|  
|**ReportingMonthOfYear**|Représente l'ordinal de mois d'une année dans un calendrier de rapports.|  
|**ReportingQuarters**|Représente des trimestres dans un calendrier de rapports.|  
|**ReportingQuarterOfHalfYear**|Représente l'ordinal de trimestre d'un semestre dans un calendrier de rapports.|  
|**ReportingQuarterOfYear**|Représente l'ordinal de trimestre d'une année dans un calendrier de rapports.|  
|**ReportingTrimesters**|Représente des quadrimestres dans un calendrier de rapports.|  
|**ReportingTrimesterOfYear**|Représente l'ordinal de quadrimestre d'une année dans un calendrier de rapports.|  
|**ReportingWeeks**|Représente des semaines dans un calendrier de rapports.|  
|**ReportingWeekOfHalfYear**|Représente l'ordinal de semaine d'un semestre dans un calendrier de rapports.|  
|**ReportingWeekOfMonth**|Représente l'ordinal de semaine d'un mois dans un calendrier de rapports.|  
|**ReportingWeekOfQuarter**|Représente l'ordinal de semaine d'un trimestre dans un calendrier de rapports.|  
|**ReportingWeekOfTrimester**|Représente l'ordinal de semaine d'un quadrimestre dans un calendrier de rapports.|  
|**ReportingWeekOfYear**|Représente l'ordinal de semaine d'une année dans un calendrier de rapports.|  
|**ReportingYears**|Représente des années dans un calendrier de rapports.|  
|**Secondes**|Représente des secondes.|  
|**TenDayOfHalfYear**|Représente l’ordinal d’une période de dix jours d’un semestre.|  
|**TenDayOfMonth**|Représente l'ordinal d'une période de dix jours d'un mois.|  
|**TenDayOfQuarter**|Représente l'ordinal d'une période de dix jours d'un trimestre.|  
|**TenDayOfTrimester**|Représente l'ordinal d'une période de dix jours d'un quadrimestre.|  
|**TenDayOfYear**|Représente l'ordinal d'une période de dix jours d'une année.|  
|**TenDays**|Représente des périodes de dix jours.|  
|**Trimesters**|Représente des quadrimestres.|  
|**TrimesterOfYear**|Représente l'ordinal de quadrimestre d'une année.|  
|**UndefinedTime**|Représente une période indéfinie.|  
|**WeekOfYear**|Représente l'ordinal de semaine d'une année.|  
|**Semaines**|Représente des semaines.|  
|**WinterSummerSeason**|Indique si la date fait partie de la saison hiver/été.|  
|**Années**|Représente des années.|  
  
## <a name="see-also"></a>Voir aussi  
 [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)   
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
