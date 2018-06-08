---
title: Initialisation d’objets dans un assembly personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: custom-assemblies
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- initializing custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], initializing
- OnInit method
ms.assetid: 26fd74dc-d02f-40f7-aeb3-50ce05e9e6b9
caps.latest.revision: 36
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 02c86872a99f56ad8c9a1454dee45a54a62ca2de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="initializing-custom-assembly-objects"></a>Initialisation d'objets Assembly personnalisés
  Dans certains cas, vous pouvez être amené à initialiser des valeurs de propriété et de champ dans vos classes d'assembly personnalisées lorsque vous les instanciez. Vous aurez vraisemblablement besoin d'initialiser vos classes personnalisées avec des valeurs disponibles à partir des collections d'objets globales du rapport. Pour cela, substituez la méthode **OnInit** de l’objet **Code** d’un rapport. Pour accéder à **OnInit**, utilisez l’élément **Code** de la définition de rapport. Deux techniques vous permettent d’initialiser les valeurs de propriété ou de champ des classes dans un assembly personnalisé que vous projetez d’utiliser dans votre rapport : vous pouvez soit déclarer et créer une instance de votre classe à l’aide **d’OnInit**, soit appeler une méthode publiquement disponible à l’aide **d’OnInit**.  
  
## <a name="global-object-collections-and-initialization"></a>Collections d'objets globales et initialisation  
 Plusieurs collections sont disponibles pour initialiser vos variables de classe personnalisées. Vous pouvez utiliser les collections **Globals** et **User**. Les collections **Parameters**, **Fields** et **ReportItems** ne sont pas disponibles au moment où, dans le cycle de vie du rapport, la méthode **OnInit** est appelée. Pour utiliser les collections partagées, **Globals** ou **User**, vous devez inclure la référence d’objet **Report**. Par exemple, pour initialiser votre classe personnalisée selon la langue actuelle de l’utilisateur qui accède au rapport, votre élément **Code** peut se présenter comme suit :  
  
```  
<Code>  
   Dim m_myClass As MyClass  
  
   Protected Overrides Sub OnInit()  
      m_myClass = new MyClass(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 L'une des méthodes permettant d'initialiser les valeurs de propriété et de champ d'une classe comme montré précédemment consiste à déclarer votre classe et à créer une nouvelle instance de celle-ci en appelant un constructeur substitué.  
  
 Une autre méthode permettant d’initialiser les valeurs de propriété et de champ des classes dans vos assemblys personnalisés consiste à appeler une méthode publiquement disponible que vous définissez à partir de la méthode **OnInit**. Vous devez d'abord ajouter un nom d'instance pour votre classe dans le fichier de définition de rapport. Une fois que vous avez ajouté la référence d'assembly et le nom d'instance appropriés, vous pouvez appeler votre méthode d'initialisation pour initialiser les valeurs de propriété et de champ pour votre classe. Votre méthode **OnInit** peut se présenter comme suit :  
  
```  
<Code>  
   Protected Overrides Sub OnInit()  
      m_myClass.MyInitializationMethod(Report.User!Language, _  
         Report.Globals!ExecutionTime)  
   End Sub  
</Code>  
```  
  
 Pour plus d’informations sur l’ajout d’une référence d’assembly et d’un nom de l’instance pour votre classe personnalisée, consultez [Ajouter une référence d’assembly à un rapport &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md).  
  
 Pour plus d’informations sur les collections d’objets globales, consultez [Collections intégrées dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d'assemblages personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
