---
title: Octroi de privilèges invité à un serveur Web | Documents Microsoft
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
- guest privileges in RDS [ADO]
ms.assetid: e851a22d-01bc-4eb0-bc42-92b8f65d1c63
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7ffe80c4182d97725a342738b9df3eb0345f9272
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="granting-guest-privileges-to-a-web-server-computer"></a>Octroi de privilèges invité à un serveur Web
Le compte de serveur Web anonyme (IUSR_*Nom_Ordinateur*) doit être ajouté au groupe local invités sur l’ordinateur serveur Web à utiliser RDS.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
### <a name="to-grant-guest-privileges-to-a-web-server-computer"></a>Pour accorder des privilèges invité à un ordinateur serveur Web  
  
1.  Sur votre ordinateur Microsoft Windows 2000 Server, cliquez sur **Démarrer**, pointez sur **programmes**, pointez sur **outils d’administration**, puis cliquez sur **ordinateur Gestion**.  
  
2.  Dans l’arborescence de la console, dans **utilisateurs et groupes locaux**, cliquez sur **groupes**.  
  
3.  Sélectionnez le **invités** groupe local. À partir de la **Action** menu, choisissez **propriétés**.  
  
4.  Dans le **propriétés d’invités** boîte de dialogue, cliquez sur **ajouter**.  
  
5.  Si le compte anonyme du serveur Web n’apparaît pas dans la liste dans le **sélectionner des utilisateurs ou groupes** boîte de dialogue, tapez son nom (IUSR_*Nom_Ordinateur*) dans la zone vide du bas, puis cliquez sur **ajouter** .  
  
6.  Cliquez sur **OK**.


