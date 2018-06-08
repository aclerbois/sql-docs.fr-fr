---
title: Exemples de tâche de script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting-task-examples
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], examples
- examples [Integration Services]
- SSIS Script task, examples
ms.assetid: b0dd77ee-ee11-4cd9-87aa-61dd67f2fe1c
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cb4f0d3edbcab24dbd40d3a552c57d0cbaeb84a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="script-task-examples"></a>Exemples de tâche de script
  La tâche de script est un outil polyvalent que vous pouvez utiliser dans un package pour remplir presque toutes les conditions requises qui ne sont pas satisfaites par les tâches incluses dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Cette rubrique répertorie des exemples de code de tâche de script qui montrent quelques-unes des fonctionnalités disponibles.  
  
> [!NOTE]  
>  Si vous souhaitez créer des tâches que vous pouvez réutiliser plus facilement dans plusieurs packages, envisagez d'utiliser le code indiqué dans ces exemples de tâche de script comme point de départ pour des tâches personnalisées. Pour plus d’informations, consultez [Développement d’une tâche personnalisée](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
### <a name="example-topics"></a>Rubriques d'exemples  
 Cette section contient des exemples de code qui montrent différentes utilisations des classes [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] que vous pouvez incorporer dans une tâche de script [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
 [Détection d’un fichier plat vide à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/detecting-an-empty-flat-file-with-the-script-task.md)  
 Vérifie un fichier plat pour déterminer s'il contient des lignes de données, puis enregistre le résultat dans une variable à utiliser dans le cadre du branchement de flux de contrôle.  
  
 [Création d’une liste pour la boucle Foreach à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/gathering-a-list-for-the-foreach-loop-with-the-script-task.md)  
 Rassemble la liste des fichiers qui répondent aux critères spécifiés par l'utilisateur et renseigne une variable à des fins d'utilisation ultérieure par l'énumérateur Foreach à partir d'une variable.  
  
 [Interrogation d’Active Directory avec la tâche de script](../../integration-services/extending-packages-scripting-task-examples/querying-the-active-directory-with-the-script-task.md)  
 Extrait des informations utilisateur d’Active Directory en fonction de la valeur d’une variable [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], en utilisant des classes de l’espace de noms System.DirectoryServices.  
  
 [Surveillance des compteurs de performances à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/monitoring-performance-counters-with-the-script-task.md)  
 Crée un compteur de performances personnalisé qui peut être utilisé pour suivre la progression de l’exécution d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], en utilisant des classes de l’espace de noms System.Diagnostics.  
  
 [Utilisation d’images à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/working-with-images-with-the-script-task.md)  
 Compresse des images au format JPEG et crée des images miniatures à partir de celles-ci, en utilisant des classes de l’espace de noms System.Drawing.  
  
 [Recherche d’imprimantes installées à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/finding-installed-printers-with-the-script-task.md)  
 Localise des imprimantes installées qui prennent en charge un format de papier spécifique, en utilisant des classes de l’espace de noms System.Drawing.Printing.  
  
 [Envoi d’un message électronique HTML à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/sending-an-html-mail-message-with-the-script-task.md)  
 Envoie un message électronique au format HTML plutôt qu'au format texte brut.  
  
 [Utilisation de fichiers Excel avec la tâche de script](../../integration-services/extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)  
 Répertorie les feuilles de calcul d'un fichier Excel et vérifie l'existence d'une feuille de calcul spécifique.  
  
 [Envoi vers une file d’attente de messages privée distante à l’aide de la tâche de script](../../integration-services/extending-packages-scripting-task-examples/sending-to-a-remote-private-message-queue-with-the-script-task.md)  
 Envoie un message à une file d'attente de messages privée distante.  
  
### <a name="other-examples"></a>Autres exemples  
 Les rubriques suivantes contiennent également des exemples de code à utiliser avec la tâche de script :  
  
 [Utilisation de variables dans la tâche de script](../../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md)  
 Demande à l'utilisateur de confirmer la poursuite de l'exécution du package, selon la valeur d'une variable du package susceptible de dépasser la limite spécifiée dans une autre variable.  
  
 [Connexion à des sources de données dans la tâche de script](../../integration-services/extending-packages-scripting/task/connecting-to-data-sources-in-the-script-task.md)  
 Extrait une connexion ou des informations de connexion auprès des gestionnaires de connexions définis dans le package.  
  
 [Déclenchement d’événements dans la tâche de script](../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)  
 Génère une erreur, un avertissement ou un message d'information selon l'état de la connexion Internet sur le serveur.  
  
 [Journalisation dans la tâche de script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md)  
 Enregistre le nombre d'éléments traités par la tâche dans les modules fournisseurs d'informations activés.  
  
  
