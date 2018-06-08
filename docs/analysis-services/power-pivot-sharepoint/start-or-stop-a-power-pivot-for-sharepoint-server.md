---
title: Démarrer ou arrêter un PowerPivot pour SharePoint Server | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4b53cc7730f962d790ebdb9a0373bf98e9bf32bf
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="start-or-stop-a-power-pivot-for-sharepoint-server"></a>Démarrer ou arrêter un serveur PowerPivot pour SharePoint
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et une instance de [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] fonctionnent ensemble sur le même serveur d’applications local, pour assurer le traitement coordonné des données et demandes dans une batterie de serveurs SharePoint.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Dépendances du service](#dependencies)  
  
 [Démarrer ou arrêter les services](#startstop)  
  
 [Conséquences de l’arrêt d’un serveur PowerPivot](#effects)  
  
##  <a name="dependencies"></a> Dépendances du service  
 Le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est dépendant de l’instance de serveur Analysis Services locale qui est installée avec ce service sur le même serveur physique. Si vous arrêtez le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , vous devez également arrêter manuellement l’instance de serveur Analysis Services locale. Si un service s’exécute sans l’autre, des erreurs d’allocation des demandes se produisent lors du traitement des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Le serveur Analysis Services doit être exécuté seul uniquement si vous diagnostiquez ou résolvez un problème. Dans tous les autres cas, le serveur nécessite une exécution locale du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur le même serveur.  
  
##  <a name="startstop"></a> Démarrer ou arrêter les services  
 Utilisez toujours l’Administration centrale pour démarrer ou arrêter le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou l’instance du serveur Analysis Services. L'Administration centrale vous permet de démarrer ou d'arrêter les services de la même page ensemble. De plus, l'Administration centrale utilise un travail du minuteur appelé **Des services ont démarré ou se sont arrêtés** afin de redémarrer des services qui pour lui doivent s'exécuter. Si vous arrêtez le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou Analysis Services à l’aide d’un outil autre que SharePoint, les services seront redémarrés au moment de l’exécution du travail du minuteur.  
  
 Le démarrage ou l'arrêt de services est une action qui s'applique à une instance de service physique. Si la batterie contient d’autres serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, ceux-ci continueront d’accepter les demandes de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Vous ne pouvez pas démarrer ou arrêter tous les services physiques simultanément dans la batterie de serveurs. Vous devez sélectionner chaque serveur, puis démarrer ou arrêter un service spécifique.  
  
 Vous ne pouvez pas démarrer, suspendre ou arrêter un service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] d’une application Web spécifique, mais vous pouvez supprimer un service de la liste de connexions par défaut pour le rendre non disponible. Pour plus d’informations, consultez [Connecter une application de service Power Pivot à une application web SharePoint dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/connect-power-pivot-service-app-to-sharepoint-web-app-in-ca.md).  
  
1.  Dans Administration centrale, sous **Paramètres système**, cliquez sur **Gérer les services sur le serveur**.  
  
2.  En haut de la page, sous Serveur, cliquez sur la flèche orientée vers le bas, puis sur **Changer de serveur**.  
  
3.  Sélectionnez le serveur SharePoint sur lequel s’exécute le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou l’instance de [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)] que vous voulez démarrer ou arrêter.  
  
4.  Sélectionnez le service, puis cliquez sur l'action. N'oubliez pas de démarrer ou d'arrêter les services sous forme de paire. Si vous démarrez ou arrêtez le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , pensez également à démarrer ou arrêter l’instance de serveur Analysis Services qui s’exécute sur le même ordinateur.  
  
##  <a name="effects"></a> Conséquences de l’arrêt d’un serveur PowerPivot  
 Le tableau suivant décrit les conséquences de l’arrêt du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et du service Analysis Services sur un serveur SharePoint.  
  
|Éléments affectés| Description|  
|---------------|-----------------|  
|Requêtes existantes|Les requêtes en cours sur un serveur Analysis Services s'arrêtent immédiatement. L'utilisateur reçoit une erreur signalant des données introuvables ou une connexion à la source de données introuvable.|  
|Travaux d'actualisation des données en cours de traitement|Les travaux en cours sur le serveur Analysis Services actif s'arrêtent immédiatement. L'actualisation des données échoue et une erreur est journalisée dans l'historique d'actualisation des données.<br /><br /> Vous pouvez consulter l'état des travaux en cours avant d'arrêter le service en utilisant la page Vérifier l'état du travail dans l'Administration centrale de SharePoint.<br /><br /> Vous avez la possibilité de savoir quels sont les travaux en cours de traitement. En revanche, il est impossible d'afficher la file d'attente pour voir si d'autres travaux sont sur le point de démarrer.|  
|Demandes d'actualisation des données dans la file d'attente|Les demandes d'actualisation planifiée des données restent dans la file d'attente de traitement pendant l'intégralité d'un cycle de la planification (autrement dit, elles sont conservées dans la file d'attente jusqu'à l'heure de début de l'actualisation suivante). Si le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’est pas redémarré dans ce délai, la demande d’actualisation des données est supprimée et une erreur est journalisée.|  
|Nouvelles demandes pour des requêtes ou une actualisation des données|Si vous arrêtez le seul serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint de la batterie, les nouvelles demandes de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne sont pas traitées et une demande de données génère une erreur signalant que les données sont introuvables.<br /><br /> Si vous disposez d’autres serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, la demande est transmise à l’un des serveurs disponibles.|  
|Données d'utilisation|Les données d'utilisation ne seront pas recueillies pendant la période d'arrêt des services.|  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration des comptes de service Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md)  
  
  
