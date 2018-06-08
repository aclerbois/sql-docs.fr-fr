---
title: À distance du traitement (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0e7afa72ef5a2f3ad9c27f0d8586b622c033be73
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="remote-processing-analysis-services"></a>Traitement à distance (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez exécuter un traitement planifié ou sans assistance sur une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . La demande de traitement proviendra d'un ordinateur, mais s'exécutera sur un autre ordinateur du même réseau.  
  
## <a name="prerequisites"></a>Conditions préalables  
  
-   Si vous exécutez différentes versions de SQL Server sur chaque ordinateur, les bibliothèques clientes doivent correspondre à la version de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui traite le modèle.
  
-   Sur le serveur distant, l'option **Autoriser les connexions à distance à cet ordinateur** doit être activée, et le compte qui émet la demande de traitement doit être répertorié en tant qu'utilisateur autorisé.  
  
-   Les règles du Pare-feu Windows doivent être configurées pour autoriser les connexions entrantes à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Vérifiez que vous pouvez vous connecter à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distante à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Consultez [Configurer le pare-feu Windows pour autoriser l'accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
-   Résolvez les erreurs de traitement local existantes avant de tenter le traitement à distance. Quand le traitement de la demande est local, vérifiez que les données peuvent être récupérées à partir de la source de données relationnelles externe. Pour obtenir des instructions sur la spécification des informations d’identification utilisées pour récupérer des données, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md).  
  
## <a name="on-demand-remote-processing"></a>Traitement à distance à la demande  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]accepte les demandes de traitement des comptes d’utilisateur ou une application qui ont [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] des autorisations d’administrateur. Si vous êtes administrateur, vérifiez que vous pouvez vous connecter à l'instance distante et traiter la base de données manuellement via la connexion à distance.  
  
1.  Sur l'ordinateur qui sera utilisé pour planifier le traitement, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , puis connectez-vous à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distante.  
  
2.  Cliquez avec le bouton droit sur la base de données, sélectionnez **Traiter**, pointez sur **Script** , puis choisissez **Action de script dans une nouvelle fenêtre de requête**. Les commandes utilisées pour appeler le traitement apparaîtront dans la fenêtre de requête.  
  
3.  Cliquez sur **OK** pour commencer le traitement.  
  
     Si cette tâche se termine correctement, vous obtiendrez une requête XMLA que vous pourrez incorporer dans une tâche planifiée. Par ailleurs, si la tâche se termine correctement, cela confirme qu'il n'existe aucun problème de connexion.  
  
## <a name="schedule-remote-processing-using-sql-server-agent-service"></a>Planifier le traitement à distance à l'aide du service SQL Server Agent  
 Par défaut, le service SQL Server Agent s'exécute sous un compte virtuel, avec des connexions réseau établies avec le compte d'ordinateur. Pour éviter d'avoir à accorder des droits d'administration à un compte d'ordinateur sur l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distante, vous devez modifier le compte du service SQL Server Agent pour qu'il soit s'exécuté en tant que compte d'utilisateur de domaine doté de privilèges minimum.  
  
 Veillez à accorder toutes les autorisations nécessaires, notamment en attribuant au compte **sysadmin** les droits sur l'instance du moteur de base de données qui fournit le service.  
  
 Utilisez les liens suivants pour définir les autorisations :  
  
-   [Configurer SQL Server Agent](http://msdn.microsoft.com/library/2e361a62-9e92-4fcd-80d7-d6960f127900)  
  
-   [SQL Server Agent Components](http://msdn.microsoft.com/library/8d1dc600-aabb-416f-b3af-fbc9fccfd0ec) suggère d'autres rôles serveur fixes s'il n'est pas possible d'accorder des autorisations à **sysadmin** .  
  
 Après avoir configuré les autorisations du compte, passez aux étapes suivantes.  
  
#### <a name="grant-the-sql-server-agent-account-administrator-permission-on-ssas"></a>Accorder des droits d'administration au compte SQL Server Agent sur SSAS  
  
1.  À l'aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], connectez-vous à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distante.  
  
2.  Cliquez avec le bouton droit sur le nom du serveur, puis cliquez sur**Propriétés**et sur **Sécurité**.  
  
3.  Cliquez sur **Ajouter** pour ajouter le compte SQL Server Agent.  
  
#### <a name="create-the-job"></a>Créer un travail  
  
1.  Dans Management Studio, connectez-vous à l'instance locale du moteur de base de données. SQL Server Agent est le dernier élément dans l'Explorateur d'objets. Si nécessaire, démarrez le service.  
  
2.  Cliquez avec le bouton droit sur **Travaux**, cliquez sur **Nouveau travail** , puis entrez un nom.  
  
3.  Dans Étapes, cliquez sur **Nouveau** , puis entrez un nom.  
  
4.  Dans Type, sélectionnez **Commande SQL Server Analysis Services**.  
  
5.  Dans Serveur, entrez le nom de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distante.  
  
6.  Dans Commande, collez la commande XMLA pour le traitement de la base de données. Il s'agit du script XMLA que vous avez généré à l'étape de vérification pour le traitement à distance à la demande. Cliquez sur **OK** pour enregistrer le travail.  
  
#### <a name="start-sql-server-profiler"></a>Démarrer SQL Server Profiler  
  
1.  Sur l'ordinateur distant, démarrez SQL Server Profiler. Connectez-vous à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis cliquez sur **Exécuter** pour démarrer la trace à l'aide des événements par défaut.  
  
     Vous allez utiliser SQL Server Profiler pour surveiller les événements de traitement à mesure qu'ils se produisent.  
  
2.  Si vous le souhaitez, vous pouvez définir des propriétés de trace pour envoyer la trace vers un fichier ou une table dans une base de données.  
  
#### <a name="run-the-job"></a>Exécuter le travail  
  
1.  Sur l'ordinateur utilisé pour exécuter le travail, vérifiez que le travail peut effectuer l'opération de base. Dans l’Explorateur d’objets, sous SQL Server Agent, développez **Travaux**, cliquez avec le bouton droit sur le travail que vous venez de créer, puis cliquez sur **Démarrer le travail à l’étape**. Le travail démarre immédiatement. Vous pouvez surveiller la progression dans SQL Server Profiler.  
  
2.  Dans la dernière étape, modifiez le travail pour qu'il soit exécuté selon la planification de votre choix, en ajoutant toutes les alertes ou notifications nécessaires à la gestion du travail. Vous pouvez également affiner le script de traitement ou créer plusieurs étapes au sein du travail pour traiter les objets indépendamment.  
  
## <a name="see-also"></a>Voir aussi  
 [Composants de SQL Server Agent](http://msdn.microsoft.com/library/8d1dc600-aabb-416f-b3af-fbc9fccfd0ec)   
 [Planification des tâches administratives SSAS avec SQL Server Agent](../../analysis-services/instances/schedule-ssas-administrative-tasks-with-sql-server-agent.md)   
 [Le traitement par lots & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/batch-processing-analysis-services.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
 [Le traitement des objets & #40 ; XMLA & #41 ;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/processing-objects-xmla.md)  
  
  
