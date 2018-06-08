---
title: MSSQLSERVER_30053 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 8ad23889-e243-4bd7-bc3e-150403399d89
caps.latest.revision: 11
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: d7f14aed745e4421a6ea43746d1286712834a47d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver30053"></a>MSSQLSERVER_30053
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|30053|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|FTXT_QUERY_E_WORDBREAKINGTIMEOUT|  
|Texte du message|L'analyse lexicale a expiré pour la chaîne de requête de texte intégral. Cela peut se produire si l'analyseur lexical a mis beaucoup de temps à traiter la chaîne de requête de texte intégral ou si un grand nombre de requêtes sont exécutées sur le serveur. Essayez de réexécuter la requête en condition de charge moins élevée.|  
  
## <a name="explanation"></a>Explication  
Une erreur de dépassement de délai de l'analyse lexicale peut se produire dans les cas de figure suivants :  
  
-   L'analyseur lexical pour le langage de requête est configuré de manière incorrecte ; par exemple, ses paramètres du Registre sont incorrects.  
  
-   L'analyseur lexical présente des dysfonctionnements pour une chaîne de requête spécifique.  
  
-   L'analyseur lexical retourne une trop grande quantité de données pour une chaîne de requête spécifique. Les données excédentaires sont considérées comme une attaque de dépassement de mémoire tampon potentielle, ce qui entraîne l'arrêt du processus de démon de filtre (fdhost.exe), qui héberge les services d'analyse lexicale.  
  
-   La configuration du processus de démon de filtre est incorrecte.  
  
    Les problèmes de configuration les plus courants sont les suivants : expiration du mot de passe ou existence d'une stratégie de domaine qui empêche la connexion du compte de démon de filtre.  
  
-   Une charge de travail de requête très importante s'exécute sur l'instance de serveur ; par exemple, l'analyseur lexical prend beaucoup de temps pour traiter la chaîne de requête de texte intégral, ou un grand nombre de requêtes s'exécutent sur le serveur. Notez qu'il s'agit de la cause la plus improbable.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Sélectionnez l'action utilisateur appropriée en fonction de la cause probable du délai d'attente, comme suit :  
  
|Cause probable|Action de l’utilisateur|  
|------------------|---------------|  
|L'analyseur lexical du langage de requête est configuré de manière incorrecte.|Si vous utilisez un analyseur lexical tiers, il est peut-être enregistré de manière incorrecte auprès du système d'exploitation. Dans ce cas, réenregistrez l'analyseur lexical. Pour plus d’informations, consultez [Rétablir la version précédente des analyseurs lexicaux utilisés par la recherche](~/relational-databases/search/revert-the-word-breakers-used-by-search-to-the-previous-version.md).|  
|L'analyseur lexical présente des dysfonctionnements pour une chaîne de requête spécifique.|Si l'analyseur lexical est pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contactez le service clientèle et le support technique Microsoft.|  
|L'analyseur lexical retourne une trop grande quantité de données pour une chaîne de requête spécifique.|Si l'analyseur lexical est pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contactez le service clientèle et le support technique Microsoft.|  
|La configuration du processus de démon de filtre est incorrecte.|Assurez-vous que vous utilisez le mot de passe actuel et qu'une stratégie de domaine n'empêche pas le compte du démon du filtre de se connecter.|  
|Une charge de traitement de requêtes très importante s'exécute sur l'instance de serveur.|Essayez de réexécuter la requête en condition de charge moins élevée.|  
  
## <a name="see-also"></a> Voir aussi  
[Définir le compte du service du Lanceur de démon de filtre de texte intégral](~/relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md)  
[Recherche en texte intégral](~/relational-databases/search/full-text-search.md)  
[sp_help_fulltext_system_components &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
[Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](~/relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)  
[Configurer et gérer des filtres pour la recherche](~/relational-databases/search/configure-and-manage-filters-for-search.md)  
  
