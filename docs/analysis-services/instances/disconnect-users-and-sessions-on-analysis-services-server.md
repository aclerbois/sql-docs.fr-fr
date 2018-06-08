---
title: Déconnecter des utilisateurs et Sessions sur Analysis Services Server | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 99072a36fe65679dbf81aa0ba3f4efdb3b487533
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="disconnect-users-and-sessions-on-analysis-services-server"></a>Déconnecter des utilisateurs et sessions sur un serveur Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un administrateur de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut vouloir arrêter l'activité des utilisateurs dans le cadre de la gestion de la charge de travail. Pour cela, vous devez annuler les sessions et les connexions. Les sessions peuvent être formées automatiquement lorsqu'une requête est exécutée (implicite) ou nommées au moment de la création par l'administrateur (explicite). Les connexions sont des conduits ouverts par lesquels les requêtes peuvent être exécutées. Les sessions et les connexions peuvent être terminées pendant qu'elles sont actives. Par exemple, un administrateur peut vouloir terminer le processus de traitement d'une session si le traitement dure trop longtemps ou si l'administrateur n'est pas sûr que la commande en cours d'exécution a été écrite correctement.  
  
## <a name="ending-sessions-and-connections"></a>Arrêt des sessions et des connexions  
 Pour gérer les sessions et les connexions, vous pouvez utiliser des vues de gestion dynamique (DMV) et XMLA :  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à une instance d'Analysis Services.  
  
2.  Collez l'une des requêtes DMV suivantes dans une fenêtre de requête MDX pour obtenir la liste de toutes les sessions, connexions et commandes qui s'exécutent actuellement :  
  
     `Select * from $System.Discover_Sessions`  
  
     `Select * from $System.Discover_Connections`  
  
     `Select * from $System.Discover_Commands`  
  
3.  Appuyez sur F5 pour exécuter la requête.  
  
     La requête DMV renvoie des informations relatives à la session et à la connexion sous la forme d'un tableau, facilitant ainsi la lecture et la copie des données qu'il contient.  
  
 Gardez la fenêtre de requête ouverte. Dans la prochaine étape, vous retournerez à cette page pour copier les SPID de la session de laquelle vous voulez vous déconnecter.  
  
 Pour terminer une session, ouvrez une deuxième fenêtre de requête XMLA.  
  
1.  Collez la syntaxe suivante dans une fenêtre de requête MDX, en remplaçant l'espace réservé ConnectionID, SessionID ou SPID par une valeur valide copiée dans l'étape précédente.  
  
    ```  
    <Cancel xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
  
       <ConnectionID>111</ConnectionID>  
       <SessionID>222</SessionID>  
       <SPID>333</SPID>  
  
    <CancelAssociated>1</CancelAssociated>  
    </Cancel>  
  
    ```  
  
2.  Appuyez sur F5 pour exécuter la commande d'annulation.  
  
 Le fait d'arrêter une connexion annule toutes les sessions et SPID, et ferme la session hôte.  
  
 L'arrêt d'une session arrête toutes les commandes (SPID) qui sont en cours d'exécution dans le cadre de cette session.  
  
 L'arrêt d'un SPID annule la commande correspondante.  
  
 Dans de rares cas, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne fermera pas la connexion s’il n’arrive pas à effectuer le suivi de toutes les sessions et tous les SPID associés à la connexion (par exemple quand plusieurs sessions sont ouvertes dans un scénario HTTP).  
  
 Pour plus d’informations sur le code XMLA mentionné dans cette rubrique, consultez [Méthode Execute &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-methods-execute.md) et [Élément Cancel &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/cancel-element-xmla.md).  
  
## <a name="see-also"></a>Voir aussi  
 [La gestion des connexions et Sessions & #40 ; XMLA & #41 ;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)   
 [Élément BeginSession & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-headers/beginsession-element-xmla.md)   
 [Élément EndSession & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-headers/endsession-element-xmla.md)   
 [Élément session & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-headers/session-element-xmla.md)  
  
  
