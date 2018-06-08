---
title: Résoudre les problèmes de traitement des rapports Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: bb309231-68be-4d68-a44c-c098999c67a2
caps.latest.revision: 4
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 8e7e4ac8f6bcd7caa3267ee783befd65d0855257
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-processing-of-reporting-services-reports"></a>Résoudre les problèmes de traitement des rapports Reporting Services
Une fois les données de rapport récupérées, le processeur de rapports combine les données et les informations de mise en page. Chaque propriété d'élément de rapport qui possède une expression est évaluée dans le contexte des données et de la mise en page combinées. Utilisez cette rubrique pour vous aider à résoudre ces problèmes.   
  
## <a name="my-report-definition-is-not-valid"></a>Ma définition de rapport n'est pas valide.  
Au moment de l'exécution, le processeur de rapports combine les données et les éléments de mise en page dans la définition de rapport, puis évalue les expressions pour les propriétés d'élément de rapport.   
  
Le processeur de rapports vérifie que la définition de rapport (fichier .rdl) est conforme au schéma spécifié dans la déclaration d'espace de noms au début du fichier .rdl. Pour plus d’informations sur les schémas RDL, voir [Rechercher la version du schéma de définition de rapport (SSRS)](../../reporting-services/reports/find-the-report-definition-schema-version-ssrs.md).  
  
Par ailleurs, les expressions de rapport qui sont évaluées au moment de l'exécution doivent respecter un ensemble de règles qui garantissent que les données de rapport et la mise en page peuvent être combinés correctement. Lorsque le processeur de rapports détecte un problème, le message suivant peut s'afficher : « La définition du rapport `<report name>` n'est pas valide ».  
  
### <a name="report-item-expressions-can-only-refer-to-fields-within-the-current-dataset-scope-or-if-inside-an-aggregate-the-specified-dataset-scope"></a>Les expressions d'élément de rapport peuvent uniquement faire référence aux champs dans l'étendue actuelle du dataset ou, à l'intérieur d'un agrégat, dans l'étendue du dataset spécifiée.  
  
Utilisez la liste suivante pour vous aider à déterminer la cause de l'erreur :  
* Lorsqu'un rapport comprend plusieurs datasets, une expression d'agrégation dans une zone de texte sur le corps du rapport doit spécifier un paramètre d'étendue. Par exemple, `=First(Fields!FieldName.Value, "DataSet1")`.  
  
Pour spécifier un paramètre d'étendue, fournissez le nom d'un dataset, d'une région de données ou d'un groupe qui se trouve dans l'étendue de l'élément de rapport. Pour plus d’informations, voir [Présentation de l’étendue des expressions pour les totaux, les agrégats et les collections intégrées (Générateur de rapports version 3.0 et SSRS)](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md) et [Référence d’expression (Générateur de rapports version 3.0 et SSRS)](../../reporting-services/report-design/expression-reference-report-builder-and-ssrs.md).  
  
### <a name="names-of-objects-must-be-greater-than-0-and-less-than-or-equal-to-256-characters"></a>Les noms d'objets doivent être supérieurs à 0 et inférieurs ou égaux à 256 caractères.  
La longueur des identificateurs d'objet dans une définition de rapport ne peut pas dépasser 256 caractères. Les identificateurs doivent respecter la casse et doivent être conformes CLS. Les noms doivent commencer par une lettre et contenir des lettres, des chiffres, un trait de soulignement (_) et aucun espace. Par exemple, les noms de zone de texte ou les noms de région de données doivent respecter ces indications.   
  
Pour modifier le nom d’un objet, dans la barre d’outils du volet Propriétés, sélectionnez l’élément dans la liste déroulante, faites défiler la liste jusqu’à **Nom** et entrez un nom d’objet valide.   
  
## <a name="a-text-box-displays-error-how-do-i-fix-it"></a>Une zone de texte affiche « #Erreur ».  
Le message « #Erreur » s'affiche lorsque le processeur de rapports évalue des expressions dans les propriétés d'élément de rapport au moment de l'exécution et détecte une conversion de type de données, une étendue ou une autre erreur.   
  
Une erreur de type de données signifie généralement que le type par défaut ou le type de données spécifié n'est pas pris en charge. Une erreur d'étendue signifie que l'étendue spécifiée n'était pas disponible au moment de l'évaluation de l'expression.   
  
Pour éliminer le message « #Erreur », vous devez réécrire l'expression à l'origine de celui-ci. Pour obtenir plus de détails sur le problème, consultez le message d'erreur détaillé.   
  
Dans l'aperçu, dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull.md)], examinez la fenêtre Sortie. Sur le serveur de rapports, examinez la pile des appels. 
  
  
## <a name="see-also"></a> Voir aussi  
[Erreurs et événements (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)
[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]

