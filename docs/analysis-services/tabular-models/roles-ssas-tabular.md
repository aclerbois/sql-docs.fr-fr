---
title: Rôles | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e73af245aaddbeb321acd60aeabd8a1617f3972f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="roles"></a>Rôles
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les rôles, dans les modèles tabulaires, définissent des autorisations de membre pour un modèle. Les membres du rôle peuvent effectuer des actions sur le modèle, comme défini par l'autorisation du rôle. Les rôles définis avec des autorisations de lecture peuvent également fournir une sécurité supplémentaire au niveau de la ligne grâce à l'utilisation de filtres au niveau de la ligne. 
  
 Pour SQL Server Analysis Services, rôles contiennent des membres d’un utilisateur par nom d’utilisateur Windows ou par groupe Windows et les autorisations (lecture, process, administrateur). Pour Azure Analysis Services, les utilisateurs doivent être dans votre Azure Active Directory et les noms d’utilisateurs et groupes spécifiés doivent être par adresse de messagerie professionnelle ou UPN. 
  
> [!IMPORTANT]  
>  Pour les utilisateurs pour se connecter à un modèle déployé à l’aide d’une application cliente de création de rapports, vous devez créer au moins un rôle au moins une autorisation à laquelle les utilisateurs sont membres de lecture.  
  
 Informations de cette rubrique sont destinées aux créateurs de modèles tabulaires qui définissent des rôles à l’aide de la boîte de dialogue Gestionnaire de rôles dans SSDT. Les rôles définis lors de la création de modèles s'appliquent à la base de données model de l'espace de travail. Après avoir déployé une base de données de modèle, les administrateurs de base de données de modèle peuvent gérer (ajouter, modifier ou supprimer) des membres de rôle à l’aide de SSMS. Pour en savoir plus sur la gestion des membres des rôles dans une base de données déployée, consultez [des rôles de modèle tabulaire](../../analysis-services/tabular-models/tabular-model-roles-ssas-tabular.md).  
  
##  <a name="bkmk_underst"></a> Understanding roles  
 Les rôles permettent de gérer l’accès aux données de modèle dans Analysis Services. Il existe deux types de rôles :  
  
-   Le rôle de serveur, un rôle fixe qui fournit un accès administrateur à une instance de serveur Analysis Services.  
  
-   les rôles de base de données, les rôles définis par les créateurs de modèles et les administrateurs pour contrôler l'accès des utilisateurs qui ne sont pas administrateurs aux données et à une base de données model.  
  
 Les rôles définis pour un modèle tabulaire sont des rôles de base de données. Autrement dit, les rôles contiennent des membres constitués d’utilisateurs ou groupes qui ont des autorisations spécifiques qui définissent l’action que ces membres peuvent effectuer sur la base de données model. Un rôle de base de données est créé comme un objet distinct dans la base de données et s'applique uniquement à la base de données dans laquelle il est créé. Les utilisateurs et les groupes sont inclus dans le rôle par l’auteur du modèle, qui, par défaut, possède les autorisations d’administrateur sur le serveur de base de données d’espace de travail ; pour un modèle déployé par un administrateur.  
  
 Les rôles dans les modèles tabulaires peuvent être définis plus précisément avec des filtres de lignes. Les filtres de lignes utilisent des expressions DAX pour définir les lignes d'une table, ainsi que toutes les lignes associées dans la direction « plusieurs », qu'un utilisateur peut interroger. Les filtres de lignes utilisant des expressions DAX ne peuvent être définis que pour les autorisations de lecture et de lecture et de traitement. Pour plus d’informations, consultez [les filtres de lignes](#bkmk_rowfliters) plus loin dans cette rubrique.  
  
 Par défaut, lorsque vous créez un projet de modèle tabulaire, le projet de modèle n'a aucun rôle. Les rôles peuvent être définis à l’aide de la boîte de dialogue Gestionnaire de rôles dans SSDT. Lorsque les rôles sont définis lors de la création d'un modèle, ils sont appliqués à la base de données model de l'espace de travail. Lorsque le modèle est déployé, les mêmes rôles sont appliqués au modèle déployé. Après avoir déployé un modèle, les membres du rôle de serveur ([administrateur Analysis Services) et des administrateurs de base de données peuvent gérer les rôles associés au modèle et les membres associés à chaque rôle à l’aide de SSMS.  
  
##  <a name="bkmk_permissions"></a> Autorisations  
 Chaque rôle a une seule autorisation de base de données définie (sauf l'autorisation combinée de lecture et de traitement). Par défaut, un nouveau rôle aura l'autorisation Aucune. Autrement dit, une fois que les membres sont ajoutés au rôle avec l'autorisation Aucune, ils ne peuvent pas modifier la base de données, exécuter une opération de traitement, interroger des données, ni voir la base de données, sauf si une autre autorisation leur est octroyée.  
  
 Un groupe ou un utilisateur peut être membre de plusieurs rôles, chaque rôle disposant d’une autorisation différente. Lorsqu'un utilisateur est membre de plusieurs rôles, les autorisations définies pour chaque rôle se cumulent. Par exemple, si un utilisateur est membre d'un rôle bénéficiant d'un accès en lecture, et qu'il est également membre d'un rôle avec une autorisation Aucune, cet utilisateur disposera d'autorisations de lecture.  
  
 Chaque rôle peut avoir l'une des autorisations suivantes définies :  
  
|Autorisations| Description|Filtres de lignes à l'aide de DAX|  
|-----------------|-----------------|----------------------------|  
|Aucun|Les membres ne peuvent pas apporter de modifications au schéma de la base de données model et ne peuvent pas interroger les données.|Les filtres de lignes ne s'appliquent pas. Aucune donnée n'est visible par les utilisateurs de ce rôle|  
|Lecture|Les membres sont autorisés à interroger des données (selon les filtres au niveau de la ligne), mais ils ne peuvent pas modifier la base de données model dans SSMS, apporter des modifications au schéma de la base de données model et l'utilisateur ne peut pas traiter le modèle.|Des filtres de lignes peuvent être appliqués. Seules les données spécifiées dans la formule DAX de filtre de lignes sont visibles par les utilisateurs.|  
|Lire et traiter|Les membres sont autorisés à interroger des données (selon les filtres au niveau de la ligne) et à exécuter des opérations de traitement à l'aide d'un script ou d'un package contenant une commande Traiter, mais ne peuvent pas apporter des modifications à la base de données. Impossible d’afficher la base de données model dans SSMS.|Des filtres de lignes peuvent être appliqués. Seules les données spécifiées dans la formule DAX de filtre de lignes peuvent être interrogées.|  
|Traiter|Les membres peuvent effectuer des opérations de traitement en exécutant un script ou un package qui contient une commande Traiter. Impossible de modifier le schéma de la base de données model. Ne peuvent pas interroger les données. Impossible d’interroger la base de données model dans SSMS.|Les filtres de lignes ne s'appliquent pas. Aucune donnée ne peut être interrogée dans ce rôle|  
|Administrateur|Les membres peuvent apporter des modifications au schéma de modèle et peuvent interroger toutes les données dans le Générateur de modèles, création de rapports et du client SSMS.|Les filtres de lignes ne s'appliquent pas. Toutes les données peuvent être interrogées dans ce rôle.|  
  
##  <a name="bkmk_rowfliters"></a> Row filters  
 Les filtres de lignes définissent les lignes d'une table qui peuvent être interrogées par les membres d'un rôle donné. Les filtres de lignes sont définis pour chaque table dans un modèle à l'aide de formules DAX.  
  
 Les filtres de lignes peuvent être définis uniquement pour les rôles avec des autorisations de lecture et de lecture et de traitement. Par défaut, si un filtre de lignes n'est pas défini pour une table particulière, les membres d'un rôle disposant de l'autorisation de lecture ou de lecture et traitement peuvent interroger toutes les lignes de la table, sauf si le filtrage croisé s'applique à partir d'une autre table.  
  
 Une fois qu'un filtre de lignes est défini pour une table particulière, une formule DAX, qui doit correspondre à une valeur TRUE/FALSE, définit les lignes qui peuvent être interrogées par les membres de ce rôle particulier. Les lignes non incluses dans la formule DAX ne peuvent pas être interrogées. Par exemple, pour les membres du rôle Sales, si la table Customers comprend l’expression de filtres de lignes suivante, *=Customers [Country] = “USA”*, les membres du rôle Sales voient uniquement les clients aux États-Unis.  
  
 Les filtres de lignes s'appliquent aux lignes spécifiées ainsi qu'aux lignes connexes. Lorsqu'une table contient plusieurs relations, les filtres appliquent la sécurité de la relation qui est active. Les filtres de lignes se croisent avec d'autres filtres de ligne définis pour les tables associées, par exemple :  
  
|Table|Expression DAX|  
|-----------|--------------------|  
|Région|=Region[Country]=”USA”|  
|ProductCategory|=ProductCategory[Name]=”Bicycles”|  
|Transactions|=Transactions[Year]=2008|  
  
 L'effet net de ces autorisations sur la table de transactions est que les membres sont autorisés à interroger les lignes de données pour lesquelles le client réside aux États-unis, la catégorie de produits correspond à des bicyclettes et l'année est 2008. Les utilisateurs ne peuvent pas interroger de transaction en dehors des États-unis, ni de transactions qui ne correspondent pas à des bicyclettes ou des transactions n'ayant pas lieu en 2008, sauf s'ils sont membres d'un autre rôle qui accorde ces autorisations.  
  
 Vous pouvez utiliser le filtre *=FALSE()* pour refuser l’accès à toutes les lignes pour une table entière.  
  
### <a name="dynamic-security"></a>Sécurité dynamique  
 La sécurité dynamique permet de définir la sécurité au niveau de la ligne en fonction du nom de l'utilisateur actuellement connecté ou de la propriété CustomData retournée par une chaîne de connexion. Pour implémenter la sécurité dynamique, vous devez inclure dans votre modèle une table avec des valeurs de connexion (nom d'utilisateur Windows) pour les utilisateurs, ainsi qu'un champ qui peut être utilisé pour définir une autorisation particulière ; par exemple, une table dimEmployees avec un ID de connexion (domaine\nom utilisateur) ainsi qu'une valeur de service pour chaque employé.  
  
 Pour implémenter la sécurité dynamique, vous pouvez utiliser les fonctions suivantes dans le cadre d'une formule DAX pour retourner le nom de l'utilisateur actuellement connecté, ou la propriété CustomData dans une chaîne de connexion :  
  
|Fonction| Description|  
|--------------|-----------------|  
|[Fonction USERNAME (DAX)](http://msdn.microsoft.com/en-us/22dddc4b-1648-4c89-8c93-f1151162b93f)|Retourne le domaine\nom d'utilisateur de l'utilisateur actuellement connecté.|  
|[Fonction CUSTOMDATA (DAX)](http://msdn.microsoft.com/en-us/58235ad8-226c-43cc-8a69-5a52ac19dd4e)|Retourne la propriété CustomData dans une chaîne de connexion.|  
  
 Vous pouvez utiliser la fonction LOOKUPVALUE pour retourner les valeurs d'une colonne dans laquelle le nom d'utilisateur Windows est le même que le nom d'utilisateur retourné par la fonction USERNAME ou une chaîne retournée par la fonction CustomData. Les requêtes peuvent ensuite être restreintes de sorte que les valeurs retournées par LOOKUPVALUE correspondent aux valeurs de la même table ou de la table associée.  
  
 Par exemple, si vous utilisez cette formule :  
  
 `='dimDepartmentGroup'[DepartmentGroupId]=LOOKUPVALUE('dimEmployees'[DepartmentGroupId], 'dimEmployees'[LoginId], USERNAME(), 'dimEmployees'[LoginId], 'dimDepartmentGroup'[DepartmentGroupId])`  
  
 La fonction LOOKUPVALUE retourne des valeurs pour la colonne dimEmployees[DepartmentId], où dimEmployees[LoginId] est identique à la valeur LoginID de l'utilisateur actuellement connecté, retournée par USERNAME, et les valeurs de dimEmployees[DepartmentId] sont les mêmes que les valeurs de dimDepartmentGroup[DepartmentId]. Les valeurs de DepartmentId retournées par LOOKUPVALUE sont ensuite utilisées pour restreindre les lignes interrogées dans la table dimDepartment, puis dans toute table associée à DepartmentId. Seules les lignes où DepartmentId est également une valeur de DepartmentId retournée par la fonction LOOKUPVALUE sont retournées.  
  
 **dimEmployees**  
  
|LastName|FirstName|LoginID|DepartmentName|DepartmentId|  
|--------------|---------------|-------------|--------------------|------------------|  
|Brown|Kevin|Adventure-works\kevin0|Marketing|7|  
|Bradley|David|Adventure-works\david0|Marketing|7|  
|Dobney|JoLynn|Adventure-works\JoLynn0|Production|4|  
|Baretto DeMattos|Paula|Adventure-works\Paula0|Ressources humaines|2|  
  
 **dimDepartment**  
  
|DepartmentId|DepartmentName|  
|------------------|--------------------|  
|1|Entreprise|  
|2|Direction générale et administration|  
|3|Gestion des stocks|  
|4|Fabrication|  
|5|Assurance qualité|  
|6|Recherche et développement|  
|7|Vente et marketing|  
  
##  <a name="bkmk_testroles"></a> Testing roles  
 Lorsque vous créez un projet de modèle, vous pouvez utiliser la fonctionnalité Analyser dans Excel pour tester l'efficacité des rôles que vous avez définis. Dans le menu **Modèle** du générateur de modèles, lorsque vous cliquez sur **Analyser dans Excel**, avant qu'Excel ne s'ouvre, la boîte de dialogue **Choisir les informations d'identification et la perspective** s'affiche. Dans cette boîte de dialogue, vous pouvez spécifier le nom d'utilisateur actuel, un nom d'utilisateur différent, un rôle et une perspective que vous utiliserez pour vous connecter au modèle de l'espace de travail en tant que source de données. Pour plus d’informations, consultez [analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md).  
  
##  <a name="bkmk_rt"></a> Related tasks  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Créer et gérer des rôles](../../analysis-services/tabular-models/create-and-manage-roles-ssas-tabular.md)|Les tâches de cette rubrique décrivent comment créer et gérer des rôles à l'aide de la boîte de dialogue **Gestionnaire de rôles** .|  
  
## <a name="see-also"></a>Voir aussi  
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [Analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md)   
 [Nom d’utilisateur, fonction (DAX)](http://msdn.microsoft.com/en-us/22dddc4b-1648-4c89-8c93-f1151162b93f)   
 [LOOKUPVALUE, fonction (DAX)](http://msdn.microsoft.com/en-us/73a51c4d-131c-4c33-a139-b1342d10caab)   
 [Fonction CUSTOMDATA (DAX)](http://msdn.microsoft.com/en-us/58235ad8-226c-43cc-8a69-5a52ac19dd4e)  
  
  
