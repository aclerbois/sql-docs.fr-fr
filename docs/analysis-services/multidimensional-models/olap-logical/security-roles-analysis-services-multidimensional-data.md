---
title: Rôles de sécurité (Analysis Services - données multidimensionnelles) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3a1e373d4e12cc2d050bc963aeb310e6c2ed53b9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="security-roles--analysis-services---multidimensional-data"></a>Rôles de sécurité (Analysis Services - Données multidimensionnelles)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les rôles sont utilisés dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] pour gérer la sécurité pour [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] objets et données. En clair, un rôle associe les identificateurs de sécurité (SID) des utilisateurs de Microsoft Windows et des groupes qui ont des droits d’accès spécifiques et les autorisations définies pour les objets gérés par une instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Deux types de rôles sont fournis dans [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]:  
  
-   le rôle du serveur, un rôle fixe qui fournit un accès administrateur à une instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)];  
  
-   les rôles de base de données, des rôles définis par les administrateurs pour contrôler l'accès des utilisateurs qui ne sont pas administrateurs aux objets et aux données.  
  
 Sécurité dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] la sécurité est gérée à l’aide de rôles et les autorisations. Les rôles sont des groupes d'utilisateurs. Les utilisateurs, également appelés membres, peuvent être ajoutés ou supprimés des rôles. Les autorisations pour les objets sont spécifiées par les rôles et tous les membres dans un rôle peuvent utiliser les objets pour lesquels le rôle a des autorisations. Tous les membres dans un rôle ont des autorisations aux objets égales. Les autorisations sont particulières aux objets. Chaque objet a une collection d'autorisations incluant les autorisations accordées pour cet objet, différents jeux d'autorisations peuvent être accordés sur un objet. Chaque autorisation de la collection d'autorisations de l'objet se voit attribuer un rôle unique.  
  
## <a name="role-and-role-member-objects"></a>Rôle et objets membres du rôle  
 Un rôle est un objet conteneur d'une collection d'utilisateurs (membres). Une définition de rôle établit l’appartenance des utilisateurs dans [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Parce que les autorisations sont assignées par rôle, un utilisateur doit être le membre d'un rôle avant d'avoir accès à tout objet.  
  
 Un objet <xref:Microsoft.AnalysisServices.Role> est composé des paramètres Name, Id et Members. Le paramètre Members est une collection de chaînes. Chaque membre contient le nom d'utilisateur sous la forme « domaine\nom d'utilisateur ». Le nom est une chaîne qui contient le nom du rôle. L'ID est une chaîne qui contient l'identificateur unique du rôle.  
  
### <a name="server-role"></a>Rôle de serveur  
 Le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] rôle serveur définit l’accès d’administration des utilisateurs et groupes Windows à une instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Les membres de ce rôle ont accès à tous les [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] bases de données et des objets sur une instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]et vous pouvez effectuer les tâches suivantes :  
  
-   effectuer des fonctions d'administration de niveau serveur à l'aide de SQL Server Management Studio ou de [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], notamment créer des bases de données et définir des propriétés de niveau serveur ;  
  
-   effectuer des fonctions d'administration par programme avec AMO (Analysis Management Objects) ;  
  
-   maintenir les rôles de base de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ;  
  
-   démarrer des traces (à des fins autres que le traitement des événements, qui peut être effectué par un rôle de base de données ayant accès aux processus).  
  
 Chaque instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] a un rôle de serveur qui définit les utilisateurs pouvant l'administrer. Le nom et ID de ce rôle est Administrateurs et contrairement aux rôles de base de données, le rôle du serveur ne peut pas être supprimé et des autorisations ne peuvent pas être ajoutées ou supprimées. En d’autres termes, un utilisateur est ou n’est pas un administrateur d’une instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], selon qu’il est inclus dans le rôle de serveur pour cette instance de [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
### <a name="database-roles"></a>Rôles de base de données  
 Un [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] rôle de base de données définit l’accès utilisateur aux objets et les données d’une [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] base de données. Un rôle de base de données est créé comme un objet distinct dans une base de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] et s'applique uniquement à la base de données dans laquelle il est créé. Les utilisateurs et les groupes Windows sont inclus dans le rôle par un administrateur, qui définit également les autorisations à l'intérieur du rôle.  
  
 Les autorisations d'un rôle peuvent permettre aux membres d'accéder à la base de données et de l'administrer, y compris les objets et les données qu'elle contient. Chaque autorisation a un ou plusieurs droits d'accès qui lui sont associés, ce qui permet d'affiner le contrôle relatif à l'accès à un objet particulier de la base de données.  
  
## <a name="permission-objects"></a>Objets Permission  
 Les autorisations sont associées à un objet (cube, dimension et autres) pour un rôle particulier. Les autorisations spécifient les opérations que le membre d'un rôle peut effectuer sur un objet.  
  
 La classe <xref:Microsoft.AnalysisServices.Permission> est une classe abstraite. Par conséquent, vous devez utiliser les classes dérivées pour définir des autorisations sur les objets correspondants. Pour chaque objet, une classe d'autorisation dérivée est définie.  
  
|Objet|Classe|  
|------------|-----------|  
|<xref:Microsoft.AnalysisServices.Database>|<xref:Microsoft.AnalysisServices.DatabasePermission>|  
|<xref:Microsoft.AnalysisServices.DataSource>|<xref:Microsoft.AnalysisServices.DataSourcePermission>|  
|<xref:Microsoft.AnalysisServices.Dimension>|<xref:Microsoft.AnalysisServices.DimensionPermission>|  
|<xref:Microsoft.AnalysisServices.Cube>|<xref:Microsoft.AnalysisServices.CubePermission>|  
|<xref:Microsoft.AnalysisServices.MiningStructure>|<xref:Microsoft.AnalysisServices.MiningStructurePermission>|  
|<xref:Microsoft.AnalysisServices.MiningModel>|<xref:Microsoft.AnalysisServices.MiningModelPermission>|  
  
 Les actions possibles activées par les autorisations sont répertoriées dans la liste :  
  
|Action|Valeurs|Explication|  
|------------|------------|-----------------|  
|Traiter|{**true**, **false**}<br /><br /> Valeur par défaut=**false**|Si **true**, les membres peuvent traiter l'objet et tout objet contenu dans l'objet.<br /><br /> Les autorisations de processus ne s'appliquent pas aux modèles d'exploration de données. <xref:Microsoft.AnalysisServices.MiningModel> les autorisations sont toujours héritées de <xref:Microsoft.AnalysisServices.MiningStructure>.|  
|ReadDefinition|{**None**, **Basic**, **Allowed**}<br /><br /> Valeur par défaut=**None**|Spécifie si les membres peuvent lire les définitions de données (ASSL) associées à l'objet.<br /><br /> Si **Allowed**, les membres peuvent lire les définitions ASSL associées à l'objet.<br /><br /> **Basic** et **Allowed** sont hérités par les objets contenus dans l'objet. **Allowed** remplace **Basic** et **None**.<br /><br /> **Allowed** est requis pour DISCOVER_XML_METADATA sur un objet. **Basic** est requis pour créer des objets liés et des cubes locaux.|  
|Lecture|{**None**, **Allowed**}<br /><br /> Valeur par défaut =**None** (sauf pour DimensionPermission, où la valeur par défaut est**Allowed**)|Spécifie si les membres ont l'accès en lecture aux ensembles de lignes et au contenu des données du schéma.<br /><br /> **Allowed** donne l'accès en lecture à une base de données, vous permettant de la découvrir.<br /><br /> **Autorisé** sur un cube permet d’accéder en lecture dans les ensembles de lignes de schéma et l’accès au contenu du cube (sauf si contraint par <xref:Microsoft.AnalysisServices.CellPermission> et <xref:Microsoft.AnalysisServices.CubeDimensionPermission>).<br /><br /> **Autorisé** sur une dimension, accorde autorisation de lecture sur tous les attributs de la dimension (sauf si contraint par <xref:Microsoft.AnalysisServices.CubeDimensionPermission>). L'autorisation de lecture est utilisée uniquement pour l'héritage statique à l'objet <xref:Microsoft.AnalysisServices.CubeDimensionPermission>. **None** sur une dimension masque la dimension et donne uniquement l'accès au membre par défaut pour les attributs pouvant faire l'objet d'une agrégation ; une erreur est levée si la dimension contient un attribut ne pouvant pas faire l'objet d'une agrégation.<br /><br /> **Autorisé** sur un <xref:Microsoft.AnalysisServices.MiningModelPermission> accorde des autorisations pour afficher les objets dans les ensembles de lignes de schéma et d’effectuer des jointures de prédiction.<br /><br /> **NoteAllowed** est nécessaire pour lire ou écrire sur n’importe quel objet dans la base de données.|  
|Write|{**None**, **Allowed**}<br /><br /> Valeur par défaut=**None**|Spécifie si les membres ont un accès en écriture aux données de l'objet parent.<br /><br /> L'accès s'applique aux sous-classes <xref:Microsoft.AnalysisServices.Dimension>, <xref:Microsoft.AnalysisServices.Cube>, et <xref:Microsoft.AnalysisServices.MiningModel>. Il ne s’applique pas à la base de données <xref:Microsoft.AnalysisServices.MiningStructure> sous-classes, ce qui génère une erreur de validation.<br /><br /> **Autorisé** sur un <xref:Microsoft.AnalysisServices.Dimension> accorde les autorisations d’écriture sur tous les attributs de la dimension.<br /><br /> **Autorisé** sur un <xref:Microsoft.AnalysisServices.Cube> accorde l’autorisation sur les cellules du cube pour les partitions définies en tant que Type d’écriture = l’écriture différée.<br /><br /> **Autorisé** sur un <xref:Microsoft.AnalysisServices.MiningModel> accorde l’autorisation de modifier le contenu du modèle.<br /><br /> **Autorisé** sur un <xref:Microsoft.AnalysisServices.MiningStructure> n’a aucune signification spécifique [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].<br /><br /> Remarque : L’écriture ne peut pas être définie sur **autorisées** , sauf si en lecture a également **autorisé**|  
|Administer<br /><br /> Remarque : Uniquement dans les autorisations de base de données|{**true**, **false**}<br /><br /> Valeur par défaut=**false**|Spécifie si les membres peuvent administrer une base de données.<br /><br /> **true** accorde aux membres l'accès à tous les objets dans une base de données.<br /><br /> Un membre peut avoir des autorisations d'administrateur pour une base de données spécifique, mais pas pour les autres.|  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations et droits d’accès & #40 ; Analysis Services - données multidimensionnelles & #41 ;](http://msdn.microsoft.com/library/59fa3573-f985-46cb-8042-7da71bd59a7b)   
 [Autorisation d’accès aux objets et les opérations de & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/authorizing-access-to-objects-and-operations-analysis-services.md)  
  
  
