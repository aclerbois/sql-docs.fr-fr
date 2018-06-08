---
title: Créer ou supprimer un alias de serveur devant être utilisé par un client | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- server alias
helpviewer_keywords:
- aliases [SQL Server], deleting
- aliases [SQL Server], creating
ms.assetid: b687e376-ee33-470d-b65a-87246bfefe6f
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b8bd808902d6f479606aecc2ba6eb601d5464748
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-or-delete-a-server-alias-for-use-by-a-client"></a>Créer ou supprimer un alias de serveur devant être utilisé par un client
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment créer ou supprimer un alias de serveur dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide du Gestionnaire de configuration SQL Server. Un alias est un nom de remplacement permettant d'établir une connexion. L'alias encapsule les éléments requis d'une chaîne de connexion, puis les expose sous un nom choisi par l'utilisateur. Les alias peuvent être utilisés avec toute application cliente. En créant des alias de serveur, votre ordinateur client peut se connecter à plusieurs serveurs utilisant différents protocoles réseau, sans avoir à spécifier les détails concernant le protocole et la connexion pour chacun d'eux. De plus, vous pouvez également faire en sorte que différents protocoles réseau soient activés en permanence, même si vous n'avez besoin de les utiliser qu'occasionnellement. Si vous avez configuré le serveur de sorte qu'il soit à l'écoute sur un numéro de port ou un canal de communication nommé autre que celui utilisé par défaut, alors que vous avez désactivé le service SQL Server Browser, créez un alias indiquant le nouveau numéro de port ou le nouveau canal de communication nommé.  
  
##  <a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-create-an-alias"></a>Pour créer un alias  
  
1.  Dans le Gestionnaire de configuration SQL Server, développez **Configuration de SQL Native Client**, cliquez avec le bouton droit sur **Alias**, puis sélectionnez **Nouvel alias**.  
  
2.  Dans la zone **Nom de l’alias** , tapez le nom de l’alias. Il s'agit du nom que les applications clientes utiliseront pour se connecter.  
  
3.  Dans la zone **Serveur** , tapez le nom ou l’adresse IP d’un serveur. Pour une instance nommée, ajoutez le nom de l'instance.  
  
4.  Dans la zone **Protocole** , sélectionnez le protocole utilisé pour cet alias. La sélection d'un protocole modifie le titre de la zone des propriétés facultatives en Numéro de port, Nom du canal ou Chaîne de connexion.  
  
 Les chaînes de connexion décrites dans l’aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent s’avérer utiles pour les programmeurs qui créent leurs propres chaînes de connexion. Pour accéder à ces informations, dans la boîte de dialogue **Nouvel alias** , appuyez sur F1 ou cliquez sur **Aide**.  
  
> [!NOTE]  
>  Si un alias configuré se connecte au serveur ou à l'instance incorrect, désactivez puis réactivez le protocole réseau associé. Cette opération efface toute information de connexion mise en cache et permet au client de se connecter correctement.  
  
#### <a name="to-delete-an-alias"></a>Pour supprimer un alias  
  
1.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , développez **Configuration de SQL Server Native Client**et cliquez sur **Alias**.  
  
2.  Dans le volet d’informations, cliquez avec le bouton droit sur l’alias à supprimer, puis cliquez sur **Supprimer**.  
  
  
