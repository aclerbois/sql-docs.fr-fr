---
title: Annulation de commandes (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0894379d301cc38084c3ee5e8d48e181d9d91dd4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="canceling-commands-xmla"></a>Annulation de commandes (XMLA)
  En fonction des autorisations d’administration de l’utilisateur qui émet la commande, le [Annuler](../../analysis-services/xmla/xml-elements-commands/cancel-element-xmla.md) commande en XML pour Analysis (XMLA) peut annuler une commande sur une session, une session, une connexion, un processus serveur, ou une session associée ou.  
  
## <a name="canceling-commands"></a>Annulation de commandes  
 Un utilisateur peut annuler la commande en cours d’exécution dans le contexte de la session explicite active en envoyant un **Annuler** avec aucune propriété spécifiée.  
  
> [!NOTE]  
>  Une commande en cours d'exécution dans une session implicite ne peut pas être annulée par un utilisateur.  
  
### <a name="canceling-batch-commands"></a>Annulation de commandes Batch  
 Si un utilisateur annule une **lot** commande, puis tous les autres commandes pas encore exécutées dans le **lot** commande sont annulées. Si le **lot** commande était transactionnelle, les commandes qui ont été exécutées avant le **Annuler** s’exécute la commande est restaurées.  
  
## <a name="canceling-sessions"></a>Annulation de sessions  
 En spécifiant un identificateur de session pour une session explicite dans le [SessionID](../../analysis-services/xmla/xml-elements-properties/sessionid-element-xmla.md) propriété de la **Annuler** de commande, un administrateur de base de données ou un administrateur de serveur peut annuler une session, y compris la commande en cours d’exécution. Un administrateur de base de données ne peut annuler les sessions que pour les bases de données pour lesquelles il dispose d'autorisations administratives.  
  
 Un administrateur de base de données peut récupérer les sessions actives pour une base de données spécifiée en récupérant l'ensemble de lignes de schéma DISCOVER_SESSIONS. Pour récupérer l’ensemble de lignes de schéma DISCOVER_SESSIONS, l’administrateur de base de données utilise le code XMLA **Discover** (méthode) et spécifie l’identificateur de la base de données appropriée pour le type de restriction session_current_database dans les [Restrictions](../../analysis-services/xmla/xml-elements-properties/restrictions-element-xmla.md) propriété de la **Discover** (méthode).  
  
## <a name="canceling-connections"></a>Annulation de connexions  
 En spécifiant un identificateur de connexion dans le [ConnectionID](../../analysis-services/xmla/xml-elements-properties/connectionid-element-xmla.md) propriété de la **Annuler** de commande, un administrateur de serveur peut annuler toutes les sessions associées à une connexion donnée, y compris toutes les commandes en cours d’exécution et annuler la connexion.  
  
> [!NOTE]  
>  Si l’instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne peut pas localiser et annuler les sessions associées à une connexion, telles que lorsque la pompe à données ouvre plusieurs sessions en fournissant une connectivité HTTP, l’instance ne peut pas annuler la connexion. Si ce cas est rencontré pendant l’exécution d’un **Annuler** de commande, une erreur se produit.  
  
 Un administrateur de serveur peut récupérer les connexions actives pour un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance en récupérant l’ensemble de lignes de schéma DISCOVER_CONNECTIONS à l’aide de la XMLA **Discover** (méthode).  
  
## <a name="canceling-server-processes"></a>Annulation de processus serveur  
 En spécifiant un identificateur de processus serveur (SPID) dans le [SPID](../../analysis-services/xmla/xml-elements-properties/spid-element-xmla.md) propriété de la **Annuler** de commande, un administrateur de serveur peut annuler les commandes associées à un SPID donné.  
  
## <a name="canceling-associated-sessions-and-connections"></a>Annulation de sessions et de connexions associées  
 Vous pouvez définir le [CancelAssociated](../../analysis-services/xmla/xml-elements-properties/cancelassociated-element-xmla.md) propriété à true pour annuler les connexions, les sessions et les commandes associées à la connexion, la session ou le SPID spécifié dans le **Annuler** commande.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode Discover &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-methods-discover.md)   
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
