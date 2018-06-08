---
title: Solutions pour accéder aux données à distance | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS [ADO]
ms.assetid: d311cc67-7db7-4c43-9590-d465564695e4
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 44b68255004047c9008fa8e79fccc4f047419e46
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="solutions-for-remote-data-access"></a>Solutions pour l’accès aux données distantes
## <a name="the-issue"></a>Le problème  
 ADO permet à votre application directement accéder à et modifier des sources de données (parfois appelés un système à deux niveaux). Par exemple, si votre connexion est la source de données qui contient vos données, qui est une connexion directe dans un système à deux niveaux.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Toutefois, vous souhaiterez accéder indirectement les sources de données via un intermédiaire tel que Microsoft® Internet Information Services (IIS). Ce procédé est parfois appelé un système à trois niveaux. IIS est un système client/serveur qui offre un moyen efficace pour une application locale ou cliente appeler un programme distant ou serveur, via Internet ou un intranet. Le programme serveur accède à la source de données et éventuellement traite les données acquises.  
  
 Par exemple, votre page Web intranet contient une application écrite en Microsoft® Visual Basic Scripting Edition (VBScript), qui se connecte à IIS. IIS à son tour se connecte à la source de données, récupère les données, la traite d’une certaine façon et puis retourne les informations traitées à votre application.  
  
 Dans cet exemple, votre application jamais connecté directement à la source de données ; IIS a. Et IIS accédé aux données au moyen d’ADO.  
  
> [!NOTE]
>  L’application client/serveur pas nécessairement être basés sur Internet ou un intranet (autrement dit, basée sur le Web), il pourrait se composer uniquement des programmes compilés sur un réseau local. Toutefois, le cas par défaut est une application basée sur le Web.  
  
 Étant donné que certains contrôles visuels, comme une grille, la case à cocher ou la liste, peut utiliser les informations renvoyées, les informations renvoyées doivent facilement utilisées par un contrôle visuel.  
  
 Vous souhaitez une interface de programmation d’application simple et efficace qui prend en charge les systèmes à trois niveaux et retourne des informations comme facilement comme si elle avait été récupérée sur un système à deux niveaux. Service de données à distance (RDS) est cette interface.  
  
## <a name="the-solution"></a>La solution  
 RDS définit un modèle de programmation, la séquence d’activités nécessaires pour accéder à et de mettre à jour une source de données, pour accéder aux données via un intermédiaire, par exemple Internet Information Services (IIS). Le modèle de programmation résume toutes les fonctionnalités de RDS.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle de programmation RDS de base](../../../ado/guide/remote-data-service/basic-rds-programming-model.md)   
 [Scénario des services Bureau à distance](../../../ado/guide/remote-data-service/rds-scenario.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
 [Utilisation et sécurité de RDS](../../../ado/guide/remote-data-service/rds-usage-and-security.md)


