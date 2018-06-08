---
title: Remplacer les paramètres de modèle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-templates
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.templates.replaceparameters.f1
helpviewer_keywords:
- template parameters [Template Explorer]
- templates [Transact-SQL], replacing parameters
- Replace (Query) Template Parameters dialog box
- replacing template parameters
ms.assetid: 1234aa14-3464-4a3e-922a-5cfb8fb23627
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 896f34a07cfccbc1f739eb714643ba181b2bb35e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replace-template-parameters"></a>Remplacer les paramètres de modèle
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Les modèles contiennent des paramètres qui peuvent être remplacés par des valeurs spécifiques à l'implémentation chaque fois que le modèle est utilisé. Après avoir ouvert un modèle dans un éditeur de code, vous pouvez remplacer les paramètres par des valeurs pertinentes pour votre implémentation.  
  
## <a name="before-you-begin"></a>Avant de commencer  
La boîte de dialogue **Spécifier les valeurs des paramètres du modèle** est une grille avec trois colonnes. Les colonnes **Paramètre** et **Type** sont en lecture seule et ne peuvent pas être modifiées. Examinez le contenu de la colonne **Valeur** , et remplacez les valeurs par défaut par des valeurs pertinentes pour votre implémentation.  
  
Pour utiliser cette boîte de dialogue, les paramètres du script doivent être placés entre les signes « inférieur à » et « supérieur à » (`< >`) au format `<`*nom_paramètre*`,` *type_de_données*`,` *valeur_par_défaut*`>`.  
  
## <a name="replace-template-parameters"></a>Remplacer les paramètres de modèle  
Après avoir ouvert le modèle dans une fenêtre d'éditeur de code :  
  
1.  Dans le menu **Requête** , cliquez sur **Spécifier les valeurs des paramètres du modèle**.  
  
2.  Dans la boîte de dialogue **Spécifier les valeurs des paramètres du modèle** , la colonne **Valeurs** contient une valeur possible pour chaque paramètre. Acceptez la valeur ou remplacez-la par une autre valeur.  
  
3.  Cliquez sur **OK** pour fermer la boîte de dialogue **Remplacer les paramètres de modèle** et modifier le script dans l’éditeur de requête.  
  
## <a name="see-also"></a> Voir aussi  
[Template Explorer](../../ssms/template/template-explorer.md)  
[Ouvrir un modèle](../../ssms/template/open-a-template.md)  
  
