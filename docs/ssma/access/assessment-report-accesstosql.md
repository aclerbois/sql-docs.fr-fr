---
title: Rapport d’évaluation (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Assessment Report dialog box
- Conversion Report dialog box
ms.assetid: ba6f53aa-0049-4c49-8bb8-607a8bfaa737
caps.latest.revision: 14
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d5d2d79c47dd1a819e602e55aad36844445e6c79
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="assessment-report-accesstosql"></a>Rapport d’évaluation (AccessToSQL)
La fenêtre de rapport d’évaluation affiche les résultats de la conversion d’objets de base de données à [!INCLUDE[tsql](../../includes/tsql_md.md)] syntaxe, et vous estimez la complexité et le coût de vos projets de migration.  
  
Pour créer un rapport d’évaluation, sélectionner les objets à convertir dans l’Explorateur de métadonnées source, avec le bouton droit **bases de données**, puis sélectionnez **créer un rapport**. Vous pouvez également afficher ce rapport automatiquement après la conversion de schémas. Toutefois, le nom du rapport sera rapport de Conversion. Pour plus d’informations, consultez [paramètres de projet (GUI) (SSMA commun)](http://msdn.microsoft.com/en-us/cf06baf1-8714-48a3-95dc-781f6ca53693).  
  
## <a name="options"></a>Options  
**Volet de l’Explorateur**  
Contient une hiérarchie d’objets dans le rapport d’évaluation. Développez les dossiers pour afficher les sous-composants et les objets individuels. Lorsque vous cliquez sur une catégorie ou un objet, les statistiques de conversion pour cette catégorie ou l’objet s’affichent dans le volet d’informations.  
  
**Volet d’informations**  
Affiche des messages de statistiques ou d’erreur et d’avertissement pour l’objet sélectionné conversion. Par exemple, si le dossier Tables est sélectionné, le volet d’informations affiche les numéros des clés étrangères, les index, les clés primaires et les tables qui ont été convertis.  
  
**Volet messages**  
Affiche les erreurs, avertissements et messages d’information générés lorsque le rapport d’évaluation a été créé. Les messages sont regroupés par numéro.  
  
Pour afficher les détails du message, cliquez sur **erreurs**, **avertissements**, ou **Messages**, puis développez un message. SSMA affiche la liste des objets qui ont cette erreur. Cliquez sur un objet pour afficher tous les détails de la conversion de l’objet.  
  
## <a name="see-also"></a>Voir aussi  
[Reference(Access) d’Interface utilisateur](http://msdn.microsoft.com/en-us/af24c303-4a41-449b-9c86-d6558a97e839)  
  
