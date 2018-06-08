---
title: Activer, désactiver et supprimer des points d’arrêt | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 357b5874-273f-43a9-8e30-83872bdea5dc
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 91d2ee0f4e000d5b27b3644532d9c34bc3713637
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="enable-disable-and-delete-breakpoints"></a>Activer, désactiver et supprimer des points d'arrêt
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Pour afficher et gérer tous les points d'arrêt ouverts, vous pouvez utiliser la fenêtre **Points d'arrêt** . Utilisez la fenêtre pour afficher les informations sur les points d'arrêt et entreprendre des actions, telles que la suppression, la désactivation ou l'activation des points d'arrêt.  
  
## <a name="the-breakpoints-window"></a>Fenêtre Points d'arrêt  
 La fenêtre **Points d'arrêt** répertorie des informations telles que la ligne de code sur laquelle se trouve le point d'arrêt. Dans la fenêtre **Points d'arrêt** , vous pouvez aussi supprimer, désactiver et activer des points d'arrêt. Pour plus d'informations sur la fenêtre **Points d'arrêt** , consultez [Points d'arrêt Window](../../relational-databases/scripting/transact-sql-debugger-breakpoints-window.md).  
  
 La désactivation d'un point d'arrêt l'empêche de suspendre l'exécution. Toutefois, la définition reste en place au cas où vous souhaiteriez activer le point d'arrêt ultérieurement. La suppression d'un point d'arrêt le supprime définitivement. Vous devez activer/désactiver un nouveau point d'arrêt pour suspendre l'exécution de l'instruction.  
  
## <a name="to-open-the-breakpoints-window"></a>Pour ouvrir la fenêtre Points d'arrêt  
 **To open the Breakpoints window**  
  
 Vous pouvez ouvrir la fenêtre **Points d'arrêt** de l'une des manières suivantes :  
  
-   Dans le menu **Déboguer** , cliquez sur **Fenêtres**, puis sur **Points d'arrêt**.  
  
-   Dans la barre d'outils **Déboguer** , cliquez sur le bouton **Points d'arrêt** .  
  
-   Appuyez sur CTRL+ALT+B.  
  
## <a name="to-disable-a-single-breakpoint"></a>Pour désactiver un point d'arrêt  
 **To disable a single breakpoint**  
  
 Vous pouvez désactiver un point d'arrêt de l'une des manières suivantes :  
  
-   Dans la fenêtre de l’éditeur de requête, cliquez avec le bouton droit sur le point d’arrêt, puis cliquez sur **Désactiver le point d’arrêt**.  
  
-   Dans la fenêtre Points d'arrêt, désactivez la case à cocher à gauche du point d'arrêt.  
  
## <a name="to-disable-all-breakpoints"></a>Pour désactiver tous les points d'arrêt  
 **To disable all breakpoints**  
  
 Vous pouvez désactiver tous les points d'arrêt de l'une des manières suivantes :  
  
-   Dans le menu **Déboguer** , cliquez sur **Désactiver tous les points d'arrêt**.  
  
-   Dans la barre d'outils de la fenêtre **Points d'arrêt** , cliquez sur le bouton **Désactiver tous les points d'arrêt** .  
  
## <a name="to-enable-a-single-breakpoint"></a>Pour activer un point d'arrêt  
 **To enable a single breakpoint**  
  
 Vous pouvez activer un point d'arrêt de l'une des manières suivantes :  
  
-   Dans la fenêtre de l’éditeur de requête, cliquez avec le bouton droit sur le point d’arrêt, puis cliquez sur **Activer le point d’arrêt**.  
  
-   Dans la fenêtre Points d'arrêt, activez la case à cocher à gauche du point d'arrêt.  
  
## <a name="to-enable-all-breakpoints"></a>Pour activer tous les points d'arrêt  
 **To enable all breakpoints**  
  
 Vous pouvez activer tous les points d'arrêt de l'une des manières suivantes :  
  
-   Dans le menu **Déboguer** , cliquez sur **Activer tous les points d'arrêt**.  
  
-   Dans la barre d'outils de la fenêtre **Points d'arrêt** , cliquez sur le bouton **Activer tous les points d'arrêt** .  
  
## <a name="to-delete-a-single-breakpoint"></a>Pour supprimer un point d'arrêt  
 **To delete a single breakpoint**  
  
 Vous pouvez supprimer un point d'arrêt de l'une des manières suivantes :  
  
-   Dans la fenêtre de l’éditeur de requête, cliquez avec le bouton droit sur le point d’arrêt, puis cliquez sur **Supprimer le point d’arrêt**.  
  
-   Dans la fenêtre Points d’arrêt, cliquez avec le bouton droit sur le point d’arrêt, puis cliquez sur **Supprimer** dans le menu contextuel.  
  
-   Dans la fenêtre Points d'arrêt, sélectionnez le point d'arrêt, puis appuyez sur Suppr.  
  
## <a name="to-delete-all-breakpoints"></a>Pour supprimer tous les points d'arrêt  
 **To delete all breakpoints**  
  
 Vous pouvez supprimer tous les points d'arrêt de l'une des manières suivantes :  
  
-   Dans le menu **Déboguer** , cliquez sur **Supprimer tous les points d'arrêt**.  
  
-   Dans la barre d'outils de la fenêtre **Points d'arrêt** , cliquez sur le bouton **Supprimer tous les points d'arrêt** .  
  
## <a name="see-also"></a> Voir aussi  
 [Basculer un point d’arrêt](../../relational-databases/scripting/toggle-a-breakpoint.md)  
  
  
