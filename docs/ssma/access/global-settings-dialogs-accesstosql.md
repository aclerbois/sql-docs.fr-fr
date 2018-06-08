---
title: Paramètres globaux (boîtes de dialogue) (AccessToSQL) | Documents Microsoft
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
ms.assetid: 6c2204f2-d49e-49ba-9c0f-f14cf07fa561
caps.latest.revision: 3
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: f7d514991dc7af4ef7c030b47ed471d0eb4cddeb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="global-settings-dialogs-accesstosql"></a>Paramètres globaux (boîtes de dialogue) (AccessToSQL)
Utilisez la page de boîtes de dialogue de la **paramètres globaux** boîte de dialogue pour spécifier l’action de l’utilisateur par défaut et les paramètres d’avertissement pour SSMA.  
  
Pour accéder aux paramètres de la boîte de dialogue sur le **outils** menu, sélectionnez **paramètres globaux**, cliquez sur **GUI** en bas du volet gauche, puis **boîtes de dialogue**.  
  
## <a name="options"></a>Options  
**Afficher l’Assistant Migration au démarrage**  
Dans SSMA pour Access, vous avez la possibilité d’activer ou désactiver **Assistant Migration** au démarrage de l’application de SSMA. Par défaut, cette option est **True**.  
  
-   Si l’option est définie sur **True**, la boîte de dialogue Assistant migration est actuellement affichée lorsque vous ouvrez SSMA pour l’application d’accès.  
  
-   Si l’option est définie sur **False**, l’Assistant de migration n’est pas affiché, et vous devez accéder manuellement à partir du **fichier** menu si nécessaire.  
  
**Avertir avant d’écraser des objets**  
Lorsque SSMA convertit des objets SQL Server, certains objets peuvent déjà exister dans les métadonnées du projet SQL Server. Ces objets peuvent avoir déjà été convertis, ou les objets peuvent tout simplement avoir le même nom dans le schéma cible en tant qu’objets que vous vous apprêtez à convertir.  
  
Utilisez cette option pour spécifier si SSMA vous invite à entrer pour remplacer les définitions d’objet en double :  
  
-   Si vous sélectionnez **True**, SSMA affichera une boîte de dialogue d’avertissement lorsqu’il rencontre un objet dupliqué. Dans cette boîte de dialogue, vous pouvez spécifier pour remplacer des objets individuels ou tous les objets en double ou ignorer des objets individuels ou tous les objets en double.  
  
-   Si vous sélectionnez **False**, le **action de remplacement par défaut de l’objet** option apparaît, où vous spécifiez l’action par défaut.  
  
**Action par défaut de remplacement d’objet**  
Cette option apparaît si vous sélectionnez **False** pour le **avertir avant d’écraser des objets** option.  
  
Utilisez cette option pour spécifier l’objet par défaut de remplacer le comportement :  
  
-   Si vous sélectionnez **True**, SSMA remplace automatiquement dans les métadonnées de projet SQL Server, les objets qui ont le même nom et se trouvent dans le même schéma cible en tant que l’objet à convertir.  
  
-   Si vous sélectionnez **False**, SSMA ne remplace pas les métadonnées de l’objet lors de la conversion.  
  
