---
title: Observations relatives à l’utilisation de serveurs de test | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- overhead [Database Engine Tuning Advisor]
- tuning overhead [SQL Server]
- reducing production server tuning load
- Database Engine Tuning Advisor [SQL Server], test servers
- xp_msver
- test servers [Database Engine Tuning Advisor]
- production servers [SQL Server]
- offload tuning overhead [SQL Server]
ms.assetid: 94e6c3e5-1f09-4616-9da2-4e44d066d494
caps.latest.revision: 27
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 7df20d63ae24216781d4b1e4abc8f01fbd6c9c65
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="considerations-for-using-test-servers"></a>Observations relatives à l'utilisation de serveurs de test
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L’utilisation d’un serveur de test pour paramétrer une base de données sur un serveur de production constitue un avantage important de l’Assistant Paramétrage de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Avec cette fonctionnalité, vous pouvez transférer sur un serveur de test la charge de gestion du paramétrage sans copier les données réelles sur le serveur de test à partir du serveur de production.  
  
> [!NOTE]  
>  La fonction de paramétrage du serveur de test n'est pas prise en charge par l'interface utilisateur graphique de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Pour utiliser cette fonctionnalité efficacement, tenez compte des points répertoriés dans les sections suivantes.  
  
## <a name="setting-up-the-test-serverproduction-server-environment"></a>Configuration de l'environnement serveur de test/serveur de production  
  
-   L'utilisateur qui souhaite se servir d'un serveur de test pour paramétrer une base de données sur un serveur de production doit exister sur les deux serveurs, sinon ce scénario ne fonctionne pas.  
  
-   La procédure stockée étendue( **xp_msver**) doit être activée pour utiliser le scénario serveur de test/serveur de production. [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage utilise cette procédure stockée étendue pour extraire le nombre de processeurs et la mémoire disponible du serveur de production à utiliser pendant le paramétrage du serveur de test. Si **xp_msver** n’est pas activée, l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] suppose les caractéristiques matérielles de l’ordinateur où l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est exécuté. Si les caractéristiques matérielles de l'ordinateur où l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est exécuté ne sont pas accessibles, la présence d'un processeur et de 1 024 mégaoctets (Mo) de mémoire est reconnue par défaut. Cette procédure stockée étendue est activée par défaut lors de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Configuration de la surface d’exposition](../../relational-databases/security/surface-area-configuration.md) et [xp_msver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md).  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage s’attend à ce que les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient les mêmes sur le serveur de test et sur le serveur de production. Si deux versions différentes sont utilisées, la version installée sur le serveur de test a la priorité. Par exemple, si le serveur de test exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard, l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'inclut pas les vues indexées, le partitionnement et les opérations en ligne dans ses recommandations, même si le serveur de production exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise.  
  
## <a name="about-test-serverproduction-server-behavior"></a>À propos du comportement du serveur de test/serveur de production  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage tient compte des différences matérielles entre le serveur de production et le serveur de test lors de la création de recommandations. La recommandation est identique comme si le paramétrage avait été effectué uniquement sur le serveur de production.  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage peut imposer une charge sur le serveur de production pour la collecte de métadonnées, ainsi que pour la création des statistiques nécessaires au paramétrage.  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage ne copie pas de données réelles du serveur de production sur le serveur de test. Il copie uniquement les métadonnées des bases de données ainsi que les statistiques nécessaires.  
  
-   Toutes les informations de session sont stockées dans la base de données **msdb** sur le serveur de production. Tout serveur de test disponible pour le paramétrage est alors utilisable et les informations sur toutes les sessions sont accessibles à un endroit unique (le serveur de production).  
  
## <a name="issues-related-to-the-shell-database"></a>Problèmes liés à la base de données shell  
  
-   Après le paramétrage, l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit supprimer toutes les métadonnées qu'il a créées sur le serveur de test pendant le processus de paramétrage, y compris la structure de base de données. Si vous exécutez une série de sessions de paramétrage avec les mêmes serveurs de production et de test, vous souhaiterez éventuellement conserver cette structure de base de données pour gagner du temps. Dans le fichier d’entrée XML, spécifiez le sous-élément **RetainShellDB** avec les autres sous-éléments sous l’élément parent **TuningOptions** . Si ces options sont utilisées, l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] conserve la base de données shell. Pour plus d’informations, consultez [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md).  
  
-   Des bases de données shell risquent d’être laissées sur le serveur de test après la réussite d’une session de paramétrage du serveur de test ou du serveur de production, même si vous n’avez pas spécifié le sous-élément **RetainShellDB**. Ces bases de données shell indésirables risquent d'interférer avec les sessions de paramétrage suivantes et elles doivent être supprimées avant qu'une autre session de paramétrage du serveur de test ou du serveur de production soit effectuée. De plus, si une session de paramétrage existe de manière inattendue, les bases de données shell sur le serveur de test et les objets de ces bases de données risquent d'être laissés sur le serveur de test. Vous devez également supprimer ces bases de données et ces objets avant de démarrer une nouvelle session de paramétrage du serveur de test ou du serveur de production.  
  
## <a name="issues-related-to-the-tuning-process"></a>Problèmes liés au processus de paramétrage  
  
-   L'utilisateur doit rechercher dans le journal de paramétrage les erreurs de paramétrage dues aux différences entre les serveurs de production et de test, ainsi que les erreurs résultant de la copie de métadonnées du serveur de production au serveur de test. Par exemple, une connexion d'utilisateur peut ne pas exister sur le serveur de test. Si une connexion d'utilisateur n'existe pas sur le serveur de test, les événements dans la charge de travail qui sont émis par cette connexion d'utilisateur risquent de ne pas être paramétrables. Utilisez l'interface utilisateur graphique de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour afficher le journal de paramétrage. Pour plus d’informations, consultez [Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md).  
  
-   Si l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas paramétrer de nombreux événements en raison d'objets manquants dans la base de données shell que l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée sur le serveur de test, l'utilisateur doit vérifier le journal de paramétrage. Les événements ne pouvant pas être paramétrés sont consignés dans le journal. Pour paramétrer correctement la base de données sur le serveur de test, l'utilisateur doit créer les objets manquants dans la structure de base de données, puis démarrer une nouvelle session de paramétrage.  
  
-   Si une base de données portant le même nom existe déjà sur le serveur de test, l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne copie pas les métadonnées, mais continue le paramétrage et collecte les statistiques nécessaires. Cela est utile si l'utilisateur a déjà créé une base de données sur le serveur de test et a copié les métadonnées appropriées avant d'appeler l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
-   Si l'option DATE_CORRELATION_OPTIMIZATION est activée sur une base de données du serveur de production, les métadonnées et les données associées à cette option ne sont pas complètement traduites en script pendant le paramétrage du serveur de test. Lors de l'exécution du paramétrage pour un scénario serveur de test/serveur de production, les problèmes suivants peuvent survenir :  
  
    -   Les utilisateurs peuvent avoir des plans de requête différents sur les serveurs pour les requêtes utilisant l'option DATE_CORRELATION_OPTIMIZATION.  
  
    -   [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage peut suggérer la suppression des vues indexées qui appliquent l’option DATE_CORRELATION_OPTIMIZATION dans le script de recommandation.  
  
     Par conséquent, vous souhaiterez éventuellement ignorer les recommandations que l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] effectue concernant les vues indexées qui contiennent des statistiques de corrélation, car l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] connaît leurs coûts, mais pas leurs avantages. [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage peut ne pas recommander la sélection de certains index, tels que les index cluster sur les colonnes **datetime** , ce qui pourrait être intéressant lorsque l’option DATE_CORRELATION_OPTIMIZATION est activée.  
  
     Pour déterminer si une vue est basée sur des statistiques de corrélation, sélectionnez la colonne **is_date_correlation_view** de l’affichage catalogue [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md) .  
  
  
