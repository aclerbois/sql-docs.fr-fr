---
title: Installer, de distribuer et de faire référence à l’objet de modèle tabulaire | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ebbe325b96c81c17b563bf5b612e011d76b2cfe7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="install-distribute-and-reference-the-tabular-object-model"></a>Installer, de distribuer et de faire référence à l’objet de modèle tabulaire
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Cet article explique comment télécharger, référencer et redistribuer Analysis Services tabulaire objet modèle (TOM), une bibliothèque c# pour créer et gérer les modèles tabulaires et les bases de données dans le code managé.  
  
TOM est une extension de la bibliothèque client AMO (Microsoft.AnalysisServices.dll) qui est fourni avec SQL Server 2016. Il fonctionne avec les modèles tabulaires, ciblez le moteur de métadonnées tabulaire dans la version de SQL Server 2016. Pour utiliser TOM, le modèle et la base de données doivent être au niveau de compatibilité 1200 ou supérieur.  

## <a name="amo-tom-assemblies"></a>Assemblys TOM AMO

SQL Server 2016 refactorise et développe AMO pour inclure les nouveaux assemblys principaux, tabulaires et JSON. Il inclut également l’assembly d’origine AMO, Microsoft.AnalysisServices.dll, qui fait partie d’Analysis Services depuis sa première version. Un AMO restructuré décharge les classes communes à un seul assembly et fournit une division logique entre les API multidimensionnel et tabulaire par le biais des assemblys supplémentaires. 

Le tableau suivant décrit chaque assembly.
  
Assembly  |Fonctionnalité  |Classes importantes |
---------|---------|--------------  |
Noyau <br/>Microsoft.AnalysisServices.Core.dll | Commun aux bases de données tabulaires et multidimensionnelles. <br/><br/>Fournit la gestion des exceptions, des connexions génériques à une instance Analysis Services et la base de données et l’accès aux propriétés et méthodes pour les objets serveur et base de données communes. <br/><br/>Il est obligatoire pour une solution AMO ciblant SQL Server 2016. | Core&nbsp;Server<br/>Core&nbsp;base de données<br/>AmoException
TOM<br/> Microsoft.AnalysisServices.Tabular.dll, version 13.0.1601.5 ou version ultérieure.| Créer et gérer des objets de métadonnées tabulaires. | TOM&nbsp;Server <br/>TOM&nbsp;base de données<br /> Modèle<br /> Table<br /> Colonne<br /> Relation
  AMO<br /> Microsoft.AnalysisServices.dll| Créer et gérer des objets de métadonnées multidimensionnelles, y compris les bases de données tabulaire 1050-1103. | AMO&nbsp;Server <br />AMO&nbsp;base de données <br /> Cube <br /> Dimension <br /> MeasureGroup 
JSON<br/>Microsoft.AnalysisServices.Tabular.Json.dll | Une DLL qui encapsule la NewtonSoftJson.dll (JSON.NET) pour contrôler les mises à jour, en supprimant le risque d’introduire des modifications fonctionnelles pour la sérialisation JSON dans les charges de travail Analysis Services d’assistance. <br /> <br />Cette DLL existe en tant que dépendance de TOM et n’est pas destinée à être utilisée directement dans votre code. | Aucun.  
  
 ### <a name="understanding-assembly-dependencies"></a>Présentation des dépendances d’assembly
  
Pour programmer sur AMO, votre solution doit inclure des références aux DLL dépendantes. AMO et TOM dépendent noyaux, car elle fournit des classes de base.

AMO dépend TOM, car certaines classes dans AMO référencent des classes à partir de TOM. Par exemple, l’objet de base de données AMO a une propriété de modèle de type modèle, implémentée dans la dll TOM. 

![Dépendances de AMO TOM](../../analysis-services/tabular-model-programming-compatibility-level-1200/media/amo-tom-dependencies.png)

Vous ne pouvez pas distribuer Microsoft.AnalysisServices.dll sans Microsoft.AnalysisServices.Tabular.dll, mais vous pouvez référencer les espaces de noms respectifs sans l’autre.

### <a name="choosing-which-namespace-to-use-in-code"></a>Choix de l’espace de noms à utiliser dans le code

Dans le [hiérarchie d’objets](../../analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo.md) , n’importe quel objet sous la base de données est une construction de métadonnées tabulaires via l’objet de modèle, ou d’une construction de métadonnées multidimensionnelles via un objet de Cube, une Dimension ou groupe de mesures. Opérations de niveau supérieur au niveau du serveur, base de données, rôle ou Trace, le choix de l’espace de noms pour faire référence à varient selon les charges de travail votre code doit prendre en charge.

* Utilisez Tabular.Server ou Tabular.Database si votre solution est le niveau de compatibilité 1200 ou supérieur, et l’objet de base de données que vous utilisez doit fournir un accès au modèle, Table, colonnes et d’autres objets exprimée sous la forme des constructions de métadonnées tabulaires.
* Utilisez AnalysisServices.Server ou AnalysisServices.Database si le code en aval fait référence à des objets multidimensionnels tels que des Cubes, des sources de données, des DataSourceViews et des Dimensions.

Vous devez les deux espaces de noms pour les outils et applications prenant en charge une combinaison de bases de données et les types de modèle. 

Faisant référence à l’espace de noms principal dans votre code est inutile ; les classes de base sont créés afin de fournir des propriétés communes, telles que le nom et une Description pour les objets principaux des classes de base.  
  
## <a name="determine-whether-amo-and-tom-installation-is-required"></a>Déterminer si l’installation AMO et TOM est requise  
   
 AMO et TOM sont regroupées dans une bibliothèque cliente unique. Si vous avez installé déjà une instance de SQL Server 2016 Analysis Services, les bibliothèques clientes ou une version de SQL Server Data Tools qui cible une instance 2016 de SQL Server, Microsoft.AnalysisServices.dll est déjà installé.  
  
 Pour déterminer si les assemblys sont déjà installés, vous pouvez vérifier ces emplacements :
* C:\Windows\Microsoft.NET\assembly\GAC_MSIL
* C:\Program Files\Microsoft SQL Server\130\SDK\Assemblies
* C:\Program Files\Microsoft SQL Server\130\DTS\Tasks
* C:\Program Files\Microsoft SQL Server\MSAS13. MSSQLSERVER\OLAP\bin
 
 Vérifiez que les assemblys suivants existent :
*  Microsoft.AnalysisServices.Core.dll
*  Microsoft.AnalysisServices.dll
*  Microsoft.AnalysisServices.Tabular.dll
*  Microsoft.AnalysisServices.Tabular.Json.dll   
   
## <a name="download-sqlasamo"></a>Télécharger SQL_AS_AMO  
 
 Notez que Microsoft.AnalysisServices.dll n’est pas disponible via le Gestionnaire de NuGet.
  
1. Accédez à [page de téléchargement de SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=52676).  
  
2. Cliquez sur **Télécharger**.  
  
3. Sélectionnez **\X64\SQL_AS_AMO.msi** ou **\X86\SQL_AS_AMO.msi**. Vous pouvez choisir le des deux : AMO et TOM assemblys sont indépendantes de la plateforme.
  
4. Cliquez sur **suivant** pour poursuivre le téléchargement. Vous trouverez les fichiers .msi dans votre **télécharge** dossier.  
  
## <a name="install-sqlasamo"></a>Installer SQL_AS_AMO  
  
1. Double-cliquez sur **SQL_AS_AMO.msi** et parcourir l’installation.  
  
2. Accédez à **C:\Program Files\Microsoft SQL Server\130\SDK\Assemblies** pour confirmer le placement de Microsoft.AnalysisServices.Core.dll, Microsoft.AnalysisServices.dll, Microsoft.AnalysisServices.Tabular.dll et Microsoft.AnalysisServices.Tabular.Json.dll.   
  
## <a name="add-references"></a>Ajouter des références  
  
1. Dans **l’Explorateur de solutions** > **ajouter une référence** > **Parcourir**.  
2. Accédez à **C:\Program Files\Microsoft SQL Server\130\SDK\Assemblies** et sélectionnez :  
   * Microsoft.AnalysisServices.Core  
   * Microsoft.AnalysisServices.Tabular  
   * Microsoft.AnalysisSerivces.Tabular.Json  
  
3. Cliquez sur **OK**.  Dans **l’Explorateur de solutions**, vérifiez les assemblys existent dans le dossier Références.
  
4. Dans votre page de code, ajoutez l’espace de noms Microsoft.AnalysisServces.Tabular si les modèles et les bases de données sont tabulaires 1200 ou niveau de compatibilité supérieur. 
  
   ```   
   using Microsoft.AnalysisServices; 
   using Microsoft.AnalysisServices.Tabular;
   ```  
    Si vous le souhaitez, ajoutez Microsoft.AnalysisServces pour prendre en charge un ensemble plus large de connexions aux instances d’Analysis Services qui ne sont pas spécifiquement SQL Server 2016 en mode serveur tabulaire. 
 
    Lorsque vous incluez les espaces de noms qui ont en commun des classes pour les objets de serveur, base de données, rôle et Trace, éviter les références ambiguës à qualifier l’espace de noms que vous souhaitez utiliser (par exemple, Microsoft.AnalysisServices.Tabular.Server instancie un objet de serveur à l’aide de l’espace de noms tabulaire).

## <a name="redistribute-amo-and-tom-with-your-application"></a>Redistribuer AMO et TOM avec votre application  
  
Redistribution de AMO et TOM s’effectue via le **sql_as_amo.msi** package d’installation. Si vous générez un programme d’installation pour une application cliente qui appelle dans AMO ou TOM, ajoutez **sql_as_amo.msi** à votre fichier exécutable. Ceci est le seul mécanisme pris en charge pour la redistribution des bibliothèques clientes AMO et TOM.  
  
Le package est autonome et fournit tous les assemblys requis pour appeler AMO et TOM dans votre code. Autres packages, tels que SQL_AS_OLEDB.msi ou SQL_AS_ADOMD.msi, ne sont pas spécifiquement requis pour les scénarios de programmation TOM.
