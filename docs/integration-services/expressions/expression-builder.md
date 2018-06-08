---
title: Générateur d’expressions | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.expressionbuilder.f1
helpviewer_keywords:
- Expression Builder dialog box
ms.assetid: 4717ce33-bd4e-44bc-81e0-002de075b4d1
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 50004539743949c565e6fb7c7896e09cce9c1788
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="expression-builder"></a>Générateur d'expressions
  Utilisez la boîte de dialogue **Générateur d’expressions** pour créer et modifier l’expression d’une propriété ou écrire l’expression qui définit la valeur d’une variable à l’aide d’une interface graphique utilisateur qui affiche la liste des variables et fournit une référence intégrée aux fonctions, aux conversions de type et aux opérateurs que contient le langage d’expression [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Une expression de propriété est une expression qui est affectée à une propriété. Lorsque l'expression est évaluée, la propriété est mise à jour de manière dynamique pour utiliser le résultat d'évaluation de l'expression. De la même manière, une expression utilisée dans une variable permet à la valeur de la variable d'être mise à jour avec le résultat de l'évaluation de l'expression.  
  
 Il existe deux manières d'utiliser les expressions :  
  
-   **Tâches** Mettez à jour la ligne À utilisée par une tâche Envoyer un message en insérant une adresse e-mail qui est stockée dans une variable ou mettez à jour la ligne Objet en concaténant une chaîne comme « Ventes pour : » et la date du jour retournée par la fonction GETDATE.  
  
-   **Variables** Affectez à la valeur d'une variable le mois en cours à l'aide d'une expression telle que `DATEPART("mm",GETDATE())`; ou définissez la valeur d'une chaîne en concaténant le littéral de chaîne à l'aide de l'expression `"Today's date is " + (DT_WSTR,30)(GETDATE())`.  
  
-   **Gestionnaire de connexions** Définissez la page de codes d'un gestionnaire de connexions de fichiers plats à l'aide d'une variable qui contient un identificateur de page de codes différent ; ou spécifiez le nombre de lignes dans le fichier de données à ignorer en entrant un entier positif (par exemple, 3) dans l'expression.  
  
 Pour en savoir plus sur les expressions de propriétés et l’écriture d’expressions, consultez [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md) et [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
## <a name="options"></a>Options  
  
|Terme|Définition|  
|----------|----------------|  
|**Variables**|Développez le dossier **Variables** et faites glisser les variables dans la zone **Expression** .|  
|**Fonctions mathématiques**<br /><br /> **Fonctions de chaîne**<br /><br /> **Fonctions de date et d'heure**<br /><br /> **Fonctions NULL**<br /><br /> **Casts de type**<br /><br /> **Opérateurs**|Développez les dossiers et faites glisser les fonctions, les conversions de type et les opérateurs dans la zone **Expression** .|  
|**Expression**|Modifiez ou tapez une expression.`|  
|**Valeur évaluée**|Donne le résultat de l'évaluation de l'expression.|  
|**Évaluer l'expression**|Cliquez sur **Évaluer l'expression** pour afficher le résultat de l'évaluation de l'expression|  
  
## <a name="see-also"></a> Voir aussi  
 [Page Expressions](../../integration-services/expressions/expressions-page.md)   
 [Éditeur d’expressions de la propriété](../../integration-services/expressions/property-expressions-editor.md)   
 [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)   
 [Variables système](../../integration-services/system-variables.md)  
  
  
