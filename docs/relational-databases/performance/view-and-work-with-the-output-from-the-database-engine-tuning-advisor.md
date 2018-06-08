---
title: Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dta.sessionmonitor.f1
- sql13.dta.reports.f1
- sql13.dta.recommendations.f1
- sql13.dta.applyrecommendations.f1
helpviewer_keywords:
- viewing logs
- recommendations [Database Engine Tuning Advisor]
- summary tuning information [SQL Server]
- Database Engine Tuning Advisor [SQL Server], recommendations
- Database Engine Tuning Advisor [SQL Server], logs
- Database Engine Tuning Advisor [SQL Server], viewing output
- Database Engine Tuning Advisor [SQL Server], reports
- logs [SQL Server], tuning
- reports [SQL Server], tuning
- viewing tuning output
ms.assetid: 47f9d9a7-80b0-416d-9d9a-9e265bc190dc
caps.latest.revision: 34
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: de3d07abb1b4e422662c2283873de739c59c6f9b
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="view-and-work-with-the-output-from-the-database-engine-tuning-advisor"></a>Afficher et utiliser la sortie de l’Assistant Paramétrage du moteur de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Lorsque l'Assistant Paramétrage du moteur de base de données paramètre des bases de données, il génère des résumés, des recommandations, des rapports et des journaux de paramétrage. Vous pouvez utiliser les résultats affichés dans le journal de paramétrage pour résoudre les problèmes liés aux sessions de l'Assistant Paramétrage du moteur de base de données. Vous pouvez vous aider des résumés, des recommandations et des rapports pour déterminer si les recommandations de paramétrage doivent être implémentées ou si le paramétrage doit se poursuivre afin d'atteindre un niveau de performance des requêtes répondant aux critères définis pour votre installation [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d'informations sur l'utilisation de l'Assistant Paramétrage de base de données pour créer des charges de travail et paramétrer une base de données, consultez [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
##  <a name="View"></a> Afficher les résultats d'un paramétrage  
 Les procédures suivantes indiquent comment afficher les recommandations, les résumés, les rapports et les journaux de paramétrage dans l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Pour plus d'informations sur les options de l'interface utilisateur, consultez [Descriptions de l'interface utilisateur](#UI) plus loin dans cette rubrique.  
  
 Cette interface permet en outre d’afficher les résultats du paramétrage générés par l’utilitaire de ligne de commande **dta** .  
  
> [!NOTE]  
>  Si vous utilisez l’utilitaire de ligne de commande **dta** et précisez que les résultats doivent être écrits dans un fichier XML par le biais de l’argument **-ox** , vous pouvez ouvrir et consulter le fichier de sortie XML en cliquant sur **Ouvrir un fichier** dans le menu **Fichier** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Use SQL Server Management Studio](http://msdn.microsoft.com/library/f289e978-14ca-46ef-9e61-e1fe5fd593be). Pour plus d’informations sur l’utilitaire de ligne de commande **dta** , consultez [Utilitaire dta](../../tools/dta/dta-utility.md).  
  
#### <a name="to-view-tuning-recommendations-with-the-database-engine-tuning-advisor-gui"></a>Pour afficher les recommandations de paramétrage dans l'interface de l'Assistant Paramétrage du moteur de base de données  
  
1.  Paramétrez une base de données dans l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données ou dans l’utilitaire de ligne de commande **dta** . Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez utiliser une session de paramétrage existante, ignorez cette étape et passez directement à l'étape 2.  
  
2.  Démarrez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez consulter les recommandations de paramétrage relatives à une session de paramétrage existante, ouvrez-la en double-cliquant sur son nom dans la fenêtre **Moniteur de session**.  
  
     Une fois la nouvelle session de paramétrage terminée ou à l'issue du chargement de la session existante par l'outil, la page **Recommandations** s'affiche.  
  
3.  Dans la page **Recommandations** , cliquez sur **Recommandations de partition** et sur **Recommandations d'index** pour afficher les volets de présentation des résultats de la session de paramétrage. Si vous n'avez pas spécifié de partitionnement au moment de la définition des options de paramétrage de cette session, le volet **Recommandations de partition** est vide.  
  
4.  Dans le volet **Recommandations de partition** ou **Recommandations d'index** , utilisez les barres de défilement pour visualiser l'ensemble des informations affichées dans la grille.  
  
5.  Désactivez la case à cocher **Afficher les objets existants** située au bas de la page à onglets **Recommandations** . La grille n'affiche alors que les objets de base de données mentionnés dans la recommandation. Utilisez la barre de défilement inférieure pour visualiser la colonne située à l’extrême droite de la grille des recommandations, puis cliquez sur un élément de la colonne **Définition** pour visualiser ou copier le script [!INCLUDE[tsql](../../includes/tsql-md.md)] générateur de cet objet dans votre base de données.  
  
6.  Si vous souhaitez enregistrer dans un fichier de script l'ensemble des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] qui créent ou suppriment les objets de base de données mentionnés dans cette recommandation, cliquez sur **Enregistrer les recommandations** dans le menu **Actions** .  
  
#### <a name="to-view-the-tuning-summary-and-reports-with-the-database-engine-tuning-advisor-gui"></a>Pour afficher le résumé et les rapports de paramétrage dans l'interface graphique de l'Assistant Paramétrage du moteur de base de données  
  
1.  Paramétrez une base de données dans l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données ou dans l’utilitaire de ligne de commande **dta** . Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez utiliser une session de paramétrage existante, ignorez cette étape et passez directement à l'étape 2.  
  
2.  Démarrez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez consulter les résumés et les rapports de paramétrage relatifs à une session de paramétrage existante, ouvrez-la en double-cliquant sur son nom dans la fenêtre **Moniteur de session**.  
  
3.  Une fois la nouvelle session de paramétrage terminée ou à l'issue du chargement de la session existante par l'outil, cliquez sur l'onglet **Rapports** .  
  
4.  Le volet **Résumé de paramétrage** contient des informations sur la session de paramétrage. Les informations fournies par les options **Pourcentage d'amélioration attendu** et **Espace occupé par la recommandation** peuvent s'avérer particulièrement utiles si vous souhaitez implémenter la recommandation.  
  
5.  Dans le volet **Rapports de paramétrage** , cliquez sur **Sélectionnez un rapport** pour choisir le rapport de paramétrage à afficher.  
  
#### <a name="to-view-tuning-logs-with-the-database-engine-tuning-advisor-gui"></a>Pour afficher les journaux de paramétrage dans l'interface graphique de l'Assistant Paramétrage du moteur de base de données  
  
1.  Paramétrez une base de données dans l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données ou dans l’utilitaire de ligne de commande **dta** . N'oubliez pas d'activer **Enregistrer le journal de paramétrage** dans l'onglet **Général** lorsque vous paramétrez la charge de travail. Si vous souhaitez utiliser une session de paramétrage existante, ignorez cette étape et passez directement à l'étape 2.  
  
2.  Démarrez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez consulter les résumés et les rapports de paramétrage relatifs à une session de paramétrage existante, ouvrez-la en double-cliquant sur son nom dans la fenêtre **Moniteur de session** .  
  
3.  Une fois la nouvelle session de paramétrage terminée ou à l'issue du chargement de la session existante par l'outil, cliquez sur l'onglet **Progression** . Le volet **Journal de paramétrage** présente le contenu du journal. Le journal contient des informations sur les événements de charge de travail que l'Assistant Paramétrage du moteur de base de données n'a pas pu analyser.  
  
     Si l'ensemble des événements de la session de paramétrage ont été analysés par l'Assistant Paramétrage du moteur de base de données, vous obtenez un message indiquant que le journal de paramétrage est vide pour la session. Si la case à cocher **Enregistrer le journal de paramétrage** n'a pas été activée sous l'onglet **Général** au moment de l'exécution effective de la session de paramétrage, un message vous en informe.  
  
##  <a name="Implement"></a> Mettre en œuvre des recommandations de paramétrage  
 Les recommandations de l'Assistant Paramétrage du moteur de base de données peuvent être mises en œuvre soit manuellement, soit automatiquement dans le cadre d'une session de paramétrage. Pour examiner les résultats du paramétrage avant de les mettre en œuvre, utilisez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Vous pouvez ensuite utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour exécuter manuellement les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] que l'Assistant Paramétrage du moteur de base de données génère suite à l'analyse d'une charge de travail afin d'appliquer les recommandations. Si vous n’avez pas besoin d’examiner les résultats avant leur mise en œuvre, vous pouvez utiliser l’option **-a** avec l’utilitaire d’invite de commandes **dta** . Cela a pour effet l'implémentation automatique par l'utilitaire des recommandations de paramétrage après l'analyse de votre charge de travail. Les procédures suivantes expliquent comment utiliser les interfaces de l'Assistant Paramétrage du moteur de base de données afin de mettre en œuvre les recommandations de paramétrage.  
  
#### <a name="to-manually-implement-tuning-recommendations-with-the-database-engine-tuning-advisor-gui"></a>Pour mettre en œuvre manuellement les recommandations au moyen de l'interface de l'Assistant Paramétrage du moteur de base de données  
  
1.  Paramétrez une base de données à l’aide de l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données ou de l’utilitaire d’invite de commandes **dta** . Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez utiliser une session de paramétrage existante, ignorez cette étape et passez directement à l'étape 2.  
  
2.  Démarrez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Si vous souhaitez appliquer les recommandations de paramétrage relatives à une session de paramétrage existante, ouvrez-la en double-cliquant sur son nom dans la fenêtre **Moniteur de session**.  
  
3.  Arrivé au terme de la nouvelle session, ou après chargement de la session existante par l'outil, cliquez sur **Appliquer les recommandations** dans le menu **Actions** .  
  
4.  Dans la boîte de dialogue **Appliquer les recommandations** , choisissez **Appliquer maintenant** ou **Planifier pour une date ultérieure**. Si vous choisissez **Planifier pour une date ultérieure**, sélectionnez la date et l'heure de l'application des recommandations.  
  
5.  Cliquez sur **OK** pour appliquer les recommandations.  
  
#### <a name="to-automatically-implement-tuning-recommendations-using-the-dta-command-prompt-utility"></a>Pour mettre en œuvre automatiquement les recommandations de paramétrage au moyen de l'utilitaire d'invite de commandes dta  
  
1.  Déterminez les fonctionnalités de base de données (index, vues indexées, partitionnement) que l'Assistant Paramétrage du moteur de base de données doit ajouter, supprimer ou conserver pendant l'analyse.  
  
     Gardez les éléments suivants à l'esprit avant de commencer à paramétrer :  
  
    -   Lors de l'utilisation d'une table de trace comme charge de travail, cette table doit exister sur le même serveur que celui où l'Assistant Paramétrage du moteur de base de données effectue le paramétrage. Si vous créez la table de trace sur un autre serveur, déplacez-la sur le serveur où a lieu le paramétrage par l'Assistant Paramétrage du moteur de base de données.  
  
    -   Si une session de paramétrage est plus longue que prévu, appuyez sur CTRL+C pour y mettre fin. Dans ce cas, cette combinaison de touches oblige **dta** à produire la meilleure recommandation possible en fonction de la quantité de charge de travail consommée, sans perdre le temps déjà utilisé par l’outil pour paramétrer la charge de travail.  
  
2.  À partir d'une invite de commandes, saisissez la commande suivante :  
  
    ```  
    dta -E -D DatabaseName -if WorkloadFile -s SessionName -a  
    ```  
  
     où **-E** spécifie que votre session de paramétrage utilise une connexion approuvée (au lieu d’un ID de connexion et d’un mot de passe) ; **-D** spécifie le nom de la base de données que vous voulez paramétrer ou une liste de bases de données utilisées par la charge de travail, séparées par des virgules ; **-if** spécifie le nom et le chemin d’accès du fichier de charge de travail, **-s** spécifie le nom de votre session de paramétrage ; **-a** spécifie que vous voulez que l’utilitaire d’invite de commandes **dta** applique automatiquement les recommandations de paramétrage sans vous poser la question une fois que la charge de travail a été analysée. Pour plus d’informations sur l’emploi de l’utilitaire d’invite de commandes **dta** pour le paramétrage de bases de données, consultez [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
3.  Appuyez sur Entrée.  
  
##  <a name="Analysis"></a> Effectuer une analyse exploratoire  
 La fonction de configuration spécifiée par l'utilisateur de l'Assistant Paramétrage du moteur de base de données permet aux administrateurs de bases de données de réaliser une analyse exploratoire. À l'aide de cette fonction, les administrateurs de bases de données spécifient la conception souhaitée d'une base de données physique à l'Assistant Paramétrage du moteur de base de données, après quoi ils peuvent évaluer les effets de cette conception sur les performances sans pour autant la mettre en œuvre. La configuration spécifiée par l'utilisateur est prise en charge par l'interface utilisateur graphique (GUI) de l'Assistant Paramétrage du moteur de base de données et par l'utilitaire de ligne de commande. Toutefois, l'utilitaire de ligne de commande présente la plus grande flexibilité.  
  
 Si vous utilisez l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données, vous pouvez évaluer les effets de la mise en œuvre d'un sous-ensemble de sa recommandation de paramétrage. En revanche, vous ne pouvez pas ajouter de structures PDS (Physical Design Structures) hypothétiques à évaluer par l'Assistant Paramétrage du moteur de base de données.  
  
 Les procédures suivantes expliquent comment utiliser la fonction de configuration spécifiée par l'utilisateur avec l'interface des deux outils.  
  
### <a name="using-database-engine-tuning-advisor-gui-to-evaluate-tuning-recommendations"></a>Utilisation de l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données pour évaluer les recommandations de paramétrage  
 La procédure suivante décrit comment évaluer une recommandation générée par l'Assistant Paramétrage du moteur de base de données. L'interface utilisateur ne vous permet cependant pas de spécifier de nouvelles structures PDS à des fins d'évaluation.  
  
##### <a name="to-evaluate-tuning-recommendations-with-the-database-engine-tuning-advisor-gui"></a>Pour évaluer les recommandations de paramétrage à l'aide de l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données  
  
1.  Utilisez l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données pour paramétrer une base de données. Pour plus d’informations, consultez [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Pour évaluer une session de paramétrage existante, double-cliquez dessus dans **Moniteur de session**.  
  
2.  Sous l'onglet **Recommandations** , effacez les structures PDS recommandées que vous ne souhaitez pas utiliser.  
  
3.  Dans le menu **Actions** , cliquez sur **Évaluer les recommandations**. Une nouvelle session de paramétrage est créée.  
  
4.  Tapez le nouveau **Nom de session**. Pour afficher la configuration de la structure PDS de la base de données en cours d'évaluation, choisissez **Cliquez ici pour afficher la section de configuration** , dans la zone **Description** en bas de la fenêtre d'application de l'Assistant Paramétrage du moteur de base de données.  
  
5.  Dans la barre d'outils, cliquez sur le bouton **Démarrer l'analyse** . Lorsque l'Assistant Paramétrage du moteur de base de données a terminé, vous pouvez afficher les résultats sous l'onglet **Recommandations** .  
  
### <a name="using-database-engine-tuning-advisor-gui-to-export-tuning-session-results-for-what-if-tuning-analysis"></a>Utilisation de l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données pour exporter les résultats de la session de paramétrage pour l'analyse du paramétrage d'un scénario  
 La procédure suivante décrit comment exporter les résultats d’une session de paramétrage de l’Assistant Paramétrage du moteur de base de données vers un fichier XML, que vous pouvez modifier, pour ensuite le paramétrer avec l’utilitaire de ligne de commande **dta** . Ceci vous permet d'effectuer une analyse de paramétrage sur de nouvelles structures PDS hypothétiques, sans engager de charge supplémentaire de mise en œuvre dans votre base de données avant d'avoir déterminé si elles produisent les améliorations de performances recherchées. L’utilisation, dans un premier temps, de l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données pour paramétrer votre base de données, puis l’exportation de ses résultats vers un fichier **.xml** , constitue une bonne méthode pour les utilisateurs non familiers du langage XML, qui peuvent ainsi exploiter la flexibilité du schéma XML de l’Assistant Paramétrage du moteur de base de données pour effectuer des analyses de scénarios.  
  
##### <a name="to-export-tuning-session-results-from-the-database-engine-tuning-advisor-gui-for-what-if-analysis-with-the-dta-command-line-utility"></a>Pour exporter les résultats de la session de paramétrage de l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données pour une analyse de scénario à l'aide de l'utilitaire de ligne de commande dta  
  
1.  Utilisez l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données pour paramétrer une base de données. Pour plus d’informations, consultez [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md). Pour évaluer une session de paramétrage existante, double-cliquez dessus dans **Moniteur de session**.  
  
2.  Dans le menu **Fichier** , cliquez sur **Exporter les résultats de la session** et enregistrez-la en tant que fichier XML.  
  
3.  Ouvrez le fichier XML créé à l'étape 2 dans l'éditeur XML ou de texte de votre choix ou dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Faites défiler jusqu'à l'élément **Configuration** . Copiez et collez la section de l'élément **Configuration** dans un modèle de fichier d'entrée XML après l'élément **TuningOptions** . Enregistrez ce fichier d'entrée XML.  
  
4.  Dans le nouveau fichier d’entrée XML créé à l’étape 3, spécifiez les options de paramétrage voulues dans l’élément **TuningOptions**, modifiez la section de l’élément **Configuration** (en ajoutant ou en supprimant les structures PDS comme approprié pour votre analyse), enregistrez le fichier et validez-le par rapport au schéma XML de l’Assistant Paramétrage du moteur de base de données. Pour plus d’informations sur la modification de ce fichier XML, consultez [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md).  
  
5.  Utilisez le fichier XML créé à l’étape 4 en tant qu’entrée dans l’utilitaire de ligne de commandes **dta** . Pour plus d'informations sur l'utilisation de fichiers d'entrée XML avec cet outil, consultez la section « Paramétrer une base de données à l'aide de l'utilitaire dta » dans [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
### <a name="using-the-user-specified-configuration-feature-with-the-dta-command-line-utility"></a>Utilisation de la fonction de configuration spécifiée par l'utilisateur avec l'utilitaire de ligne de commande dta  
 Si vous êtes un développeur XML expérimenté, vous pouvez créer un fichier d'entrée XML d'Assistant Paramétrage du moteur de base de données, dans lequel vous pouvez spécifier une charge de travail et une configuration hypothétique de structures PDS, telles que des index, des vues indexées ou un partitionnement. Vous pouvez ensuite recourir à l’utilitaire de ligne de commande **dta** pour analyser les effets de cette configuration hypothétique sur les performances des requêtes pour votre base de données. La procédure suivante détaille ce processus :  
  
##### <a name="to-use-the-user-specified-configuration-feature-with-the-dta-command-line-utility"></a>Pour utiliser la fonction de configuration spécifiée par l'utilisateur avec l'utilitaire de ligne de commande dta  
  
1.  Créez une charge de travail de paramétrage. Pour plus d'informations sur l'exécution de cette tâche, consultez [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
2.  Copiez et collez [l’Exemple de fichier d’entrée XML avec une configuration spécifiée par l’utilisateur &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md) dans votre éditeur XML ou dans un éditeur de texte. Utilisez cet exemple pour créer un fichier d'entrée XML pour votre session de paramétrage. Pour plus d'informations sur l'exécution de cette tâche, consultez la section « Créer des fichiers d'entrée XML » dans [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
3.  Modifiez les éléments **TuningOptions** et **Configuration** dans l'exemple de fichier d'entrée XML. Dans l'élément **TuningOptions** , spécifiez les structures de création physique PDS dont doit tenir compte l'Assistant Paramétrage du moteur de base de données au cours de la session de paramétrage. Dans l'élément **Configuration** , spécifiez les structures PDS qui correspondent à la configuration hypothétique des structures PDS de la base de données que doit analyser l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations sur les attributs et les éléments enfants utilisables avec les éléments parents **TuningOptions** et **Configuration**, consultez [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md).  
  
4.  Enregistrez le fichier d'entrée avec l'extension **.xml** .  
  
5.  Validez le fichier d'entrée XML enregistré à l'étape 4 par rapport au schéma XML de l'Assistant Paramétrage du moteur de base de données. Ce schéma est installé dans l'emplacement suivant lors de l'installation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
    ```  
    C:\Program Files\Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd  
    ```  
  
     Le schéma XML de l’Assistant Paramétrage du moteur de base de données est également disponible en ligne sur [http://schemas.microsoft.com/sqlserver/2004/07/dta](http://schemas.microsoft.com/sqlserver/2004/07/dta).  
  
6.  Après avoir créé une charge de travail et un fichier d’entrée XML, vous êtes prêt à envoyer le fichier d’entrée vers l’utilitaire de ligne de commande **dta** pour l’analyser. Veillez à donner un nom au fichier de sortie XML pour l’argument **-ox** de l’utilitaire. Cette opération crée un fichier de sortie XML dont la configuration recommandée est spécifiée dans l'élément **Configuration** . Si vous souhaitez exécuter de nouveau l'Assistant Paramétrage du moteur de base de données pour vérifier une autre configuration hypothétique basée sur la sortie, vous pouvez copier et coller le contenu de l'élément **Configuration** du fichier de sortie dans un nouveau fichier d'entrée XML ou dans votre fichier d'origine. Pour plus d’informations sur l’utilisation d’un fichier d’entrée XML avec l’utilitaire **dta** , consultez la section « Paramétrer une base de données à l’aide de l’utilitaire dta » de l’article [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
     Une fois le paramétrage terminé, utilisez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données pour afficher les rapports de paramétrage, ou bien ouvrez le fichier de sortie XML pour afficher les éléments **TuningSummary** et **Configuration** , ainsi que les recommandations de l'Assistant Paramétrage du moteur de base de données. Pour plus d'informations sur l'affichage des résultats de votre session de paramétrage, consultez [Afficher les résultats d'un paramétrage](#View) plus haut dans cette rubrique. Notez également que le fichier de sortie XML peut contenir des rapports d'analyse de l'Assistant Paramétrage du moteur de base de données.  
  
7.  Répétez les étapes 6 et 7 jusqu'à ce que vous ayez créé la configuration hypothétique qui produit l'amélioration des performances de requête recherchée. Vous pouvez alors mettre en œuvre la nouvelle configuration. Pour plus d'informations, consultez [Mettre en œuvre des recommandations de paramétrage](#Implement) plus haut dans cette rubrique.  
  
##  <a name="ReviewEvaluateClone"></a> Examiner, évaluer et cloner les sessions de paramétrage  
 L'Assistant Paramétrage du moteur de base de données crée une nouvelle session de paramétrage chaque fois que vous le démarrez pour analyser les effets d'une charge de travail sur votre ou vos bases de données. Vous pouvez utiliser le **Moniteur de session** de l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données pour afficher ou recharger toutes les sessions de paramétrage qui ont été exécutées sur une instance donnée de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le fait de pouvoir examiner l'ensemble des sessions de paramétrage existantes facilite : le clonage de sessions, l'édition de recommandations de paramétrage existantes et l'utilisation de l'Assistant Paramétrage du moteur de base de données pour évaluer la session éditée, ou la réalisation régulière d'opérations de paramétrage afin de surveiller le modèle physique de vos bases de données. Par exemple, vous pouvez être amené à planifier l'analyse mensuelle d'une base de données.  
  
 Avant de pouvoir examiner une session de paramétrage d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez créer des sessions de paramétrage sur l'instance serveur, en analysant des charges de travail via l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
### <a name="review-existing-tuning-sessions"></a>Examiner les sessions de paramétrage existantes  
 Suivez les étapes ci-dessous pour parcourir les sessions de paramétrage existantes d'une instance donnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##### <a name="to-review-existing-tuning-sessions"></a>Pour examiner des sessions de paramétrage existantes  
  
1.  Démarrez l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données. Pour plus d’informations, consultez [Démarrer et utiliser l'Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md).  
  
2.  Toutes les sessions de paramétrage existantes s'affichent dans la partie supérieure de la fenêtre du **Moniteur de session** . Le nombre de sessions affichées dépend du nombre de fois que vous avez analysé des bases de données de cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez les barres de défilement pour afficher l'ensemble des sessions de paramétrage.  
  
3.  Cliquez une fois sur le nom d'une session de paramétrage pour en afficher les détails dans la moitié inférieure de la fenêtre du **Moniteur de session** .  
  
4.  Double cliquez sur le nom d'une session de paramétrage pour charger ses données dans l'Assistant Paramétrage du moteur de base de données. Une fois les données de la session chargées, vous pouvez sélectionner n'importe quel onglet afin d'afficher les informations relatives à cette session de paramétrage.  
  
### <a name="evaluate-existing-tuning-sessions-as-hypothetical-configurations"></a>Évaluer des sessions de paramétrage existantes en tant que configurations hypothétiques  
 Suivez les étapes suivantes pour évaluer une session de paramétrage existante. L'évaluation d'une session de paramétrage existante implique l'examen et l'édition de ses recommandations, puis le reparamétrage. Par exemple, si vous décidez que vous voulez uniquement créer des index sur la **table1**, vous pouvez supprimer la création des vues indexées et du partitionnement d'une recommandation de paramétrage existante. L'Assistant Paramétrage du moteur de base de données crée alors une nouvelle session de paramétrage et ajuste la charge de travail de vos bases de données au moyen des recommandations éditées, envisagées en tant que configuration hypothétique. Cela signifie que l'Assistant Paramétrage du moteur de base de données ajuste la charge de travail des bases de données comme si les recommandations éditées avaient été mises en œuvre, ce qui vous permet de réaliser une analyse de simulation limitée. Il s'agit d'une analyse de simulation limitée dans la mesure où, lorsque vous utilisez le GUI de l'Assistant Paramétrage du moteur de base de données, vous ne pouvez choisir qu'un sous-ensemble d'une recommandation existante. Pour réaliser une analyse complète de scénarios spécifiant une configuration hypothétique entièrement nouvelle, non basée sur un sous-ensemble d’une session précédente de paramétrage, vous devez utiliser le fichier d’entrée XML de l’Assistant Paramétrage du moteur de base de données avec l’utilitaire de ligne de commande **dta** .  
  
##### <a name="to-evaluate-an-existing-tuning-session"></a>Pour évaluer une session de paramétrage existante  
  
1.  Après avoir démarré l’Assistant Paramétrage du moteur de base de données, double cliquez sur une session de paramétrage dans la moitié supérieure du **Moniteur de session**, ce qui charge les données de la session dans l’Assistant Paramétrage du moteur de base de données.  
  
2.  Cliquez sur la barre de **Progression** pour vérifier le journal de paramétrage, qui contient des informations sur les erreurs liées à tout événement de la charge de travail que l'Assistant Paramétrage du moteur de base de données ne peut ajuster. Cette information peut vous aider à évaluer l'efficience de la charge de travail.  
  
3.  Si vous souhaitez examiner plus en profondeur les résultats de cette session de paramétrage, cliquez sur l'onglet **Rapports** . Vous pouvez y consulter la synthèse du paramétrage ou choisir un rapport de paramétrage à partir de la liste **Sélectionner un rapport** .  
  
4.  Cliquez sur l'onglet **Recommandations** pour afficher les recommandations de paramétrage.  
  
5.  S'il existe des recommandations dont vous n'êtes pas sûr de la mise en œuvre, décochez-les.  
  
6.  Dans le menu **Actions** , cliquez sur **Évaluer les recommandations**. L'Assistant Paramétrage du moteur de base de données crée une nouvelle session de paramétrage qui utilise les recommandations éditées en tant que configuration hypothétique. Pour afficher la configuration hypothétique au format XML, choisissez **Cliquez ici pour afficher la section configuration**.  
  
7.  Sous l'onglet **Général** , tapez un **Nom de session**et assurez-vous que la **Charge de travail** correcte est spécifiée.  
  
8.  Dans l'onglet **Options de paramétrage** , vous pouvez spécifier une heure de paramétrage ou n'importe laquelle des **Options avancées**.  
  
9. Dans la barre d'outils, cliquez sur le bouton **Démarrer l'analyse** . L'Assistant Paramétrage du moteur de base de données commence à analyser les bases de données au moyen de la configuration hypothétique. Lorsque l'Assistant Paramétrage du moteur de base de données se termine, vous pouvez consulter les résultats de cette session comme vous le feriez normalement pour toute session.  
  
### <a name="clone-existing-tuning-sessions"></a>Cloner des sessions de paramétrage existantes  
 Vous pouvez créer de nouvelles sessions de paramétrage basées sur des sessions existantes en choisissant l'option clonage de l'Assistant Paramétrage du moteur de base de données. Lorsque vous utilisez l'option clonage, vous basez une nouvelle session de paramétrage sur une session existante. Vous pouvez alors changer les options de paramétrage de la nouvelle session si nécessaire. Lorsque vous évaluez une session existante comme décrit dans la procédure précédente, l'Assistant Paramétrage du moteur de base de données crée également une nouvelle session de paramétrage, mais vous ne pouvez en modifier les options.  
  
##### <a name="to-create-new-tuning-sessions-by-cloning-existing-sessions"></a>Pour créer de nouvelles sessions de paramétrage en clonant des sessions existantes  
  
1.  Après avoir démarré l’Assistant Paramétrage du moteur de base de données, double cliquez sur une session de paramétrage dans la moitié supérieure du **Moniteur de session**, ce qui charge les données de la session dans l’Assistant Paramétrage du moteur de base de données.  
  
2.  Dans le menu **Actions** , cliquez sur **Cloner la session**.  
  
3.  Sous l'onglet **Général** , tapez un **Nom de session**et assurez-vous que la **Charge de travail** correcte est spécifiée.  
  
4.  Dans l'onglet **Options de paramétrage** , vous pouvez spécifier une heure de paramétrage, les structures du modèle physique que l'Assistant Paramétrage du moteur de base de données doit penser à créer, ainsi que ce qu'il doit penser à supprimer dans ses recommandations.  
  
5.  Cliquez sur **Options avancées** si vous voulez définir une taille limite pour les recommandations, un nombre maximal de colonnes par index, ainsi que si vous souhaitez que l'Assistant Paramétrage du moteur de base de données génère des recommandations pouvant être implémentées pendant que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en ligne.  
  
6.  Cliquez sur le bouton **Démarrer l'analyse** de la barre d'outils pour analyser les effets de la charge de travail, comme lors de n'importe quelle autre session de paramétrage. Lorsque l'Assistant Paramétrage du moteur de base de données se termine, vous pouvez consulter les résultats de cette session comme vous le feriez normalement pour toute session.  
  
##  <a name="UI"></a> Descriptions de l'interface utilisateur  
  
### <a name="sessions-monitor"></a>Moniteur de sessions  
 Le**Moniteur de session** affiche des informations sur les sessions ouvertes dans l'Assistant Paramétrage du moteur de base de données. Pour afficher des informations sur la session dans la fenêtre des propriétés, sélectionnez un nom de session dans le **Moniteur de session**.  
  
### <a name="recommendations-tab"></a>Onglet Recommandations  
 L'onglet **Recommandations** apparaît une fois que l'Assistant Paramétrage du moteur de base de données a terminé l'analyse d'une charge de travail. Cette grille contient les recommandations pour chaque objet pris en compte. Le cas échéant, les recommandations relatives aux partitions sont présentées dans la grille supérieure tandis que les recommandations relatives aux index sont présentées dans la grille inférieure. Les grilles ne s'affichent pas s'il n'existe aucune recommandation.  
  
 La colonne **Définition** contient la définition de la partition ou de l'index recommandés sous forme de lien hypertexte. Cette colonne est généralement trop étroite pour pouvoir voir la définition en entier. Cliquez sur le lien hypertexte pour ouvrir une boîte de dialogue contenant l'intégralité de la définition et le bouton **Copier dans le Presse-papiers** .  
  
#### <a name="partition-recommendations"></a>Recommandations de partition  
 **Nom de la base de données**  
 Base de données contenant les objets dont la modification est recommandée.  
  
 **Recommandation**  
 Action recommandée pour améliorer les performances. Les valeurs possibles sont Créer et Supprimer.  
  
 **Cible de recommandation**  
 La fonction ou le schéma de partition concernés par la recommandation. L'icône de cette colonne indique s'il est recommandé de supprimer ou d'ajouter la **Cible de recommandation** et s'il s'agit d'une fonction ou d'un schéma de partition.  
  
 **Détails**  
 Description de la **Cible de recommandation**. Les valeurs possibles incluent une plage pour les fonctions de partition ou une valeur vide pour les schémas de partition.  
  
 **Nombre de partitions**  
 Le nombre de partitions définies par les fonctions de partitionnement recommandées. Lorsque cette fonction est utilisée avec un schéma et appliquée ensuite à une table, les données de la table sont divisées selon ce nombre de partitions.  
  
 **Définition**  
 Définition de la **Cible de recommandation**. Cliquez sur la colonne pour ouvrir la boîte de dialogue Aperçu de script SQL qui contient un script pour l'action recommandée.  
  
##### <a name="index-recommendations"></a>Recommandations d'index  
 **Database Name**  
 Base de données contenant les objets dont la modification est recommandée.  
  
 **Nom de l’objet**  
 La table associée à la recommandation.  
  
 **Recommandation**  
 Action recommandée pour améliorer les performances. Les valeurs possibles sont Créer et Supprimer.  
  
 **Cible de recommandation**  
 L'index ou la vue concernés par la recommandation. L'icône de cette colonne indique s'il est recommandé de supprimer ou d'ajouter la **Cible de recommandation**.  
  
 **Détails**  
 Description de la **Cible de recommandation**. Les valeurs possibles incluent index cluster, vue indexée ou une valeur vide indiquant un index non-cluster. Indique également si l'index est unique.  
  
 **Schéma de partition**  
 Le schéma de partition est fourni dans cette colonne si le partitionnement est recommandé.  
  
 **Taille (Ko)**  
 Taille attendue et recommandée pour le nouvel objet. Si cette colonne est vide, cliquez sur **Afficher les rapports sur la taille des objets**.  
  
 **Définition**  
 Définition de la **Cible de recommandation**. Cliquez sur la colonne pour ouvrir la boîte de dialogue Aperçu de script SQL qui contient un script pour l'action recommandée.  
  
 **Afficher les objets existants**  
 Sélectionnez cette option pour afficher tous les objets existants dans la grille, même s'ils ne sont concernés par aucune recommandation de l'Assistant Paramétrage du moteur de base de données.  
  
 **Afficher les rapports sur la taille des objets**  
 Sélectionnez cette option pour afficher des rapports qui indiquent la taille des objets existants dans la grille des recommandations.  
  
### <a name="actions-menuapply-recommendations-options"></a>Options du menu Actions/Appliquer les recommandations  
 Lorsqu'une charge de travail a été analysée et que des recommandations ont été formulées, cliquez sur **Appliquer les recommandations** dans le menu **Actions** pour ouvrir la boîte de dialogue **Appliquer les recommandations** .  
  
 **Appliquer maintenant**  
 Génère un script pour les recommandations et exécute ce script pour mettre en application les recommandations.  
  
 **Planifier pour une date ultérieure**  
 Génère un script pour les recommandations et enregistre les actions en tant que tâche de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Date**  
 Spécifiez la date à laquelle vous voulez exécuter la tâche de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour appliquer les recommandations.  
  
 **Time**  
 Spécifiez l'heure à laquelle vous voulez exécuter le travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d'appliquer les recommandations.  
  
### <a name="reports-tab-options"></a>Options de l'onglet Rapports  
 L'onglet **Rapports** apparaît une fois que l'Assistant Paramétrage du moteur de base de données a terminé l'analyse d'une charge de travail.  
  
 **Résumé de paramétrage**  
 Affiche un résumé des recommandations de l'Assistant Paramétrage du moteur de base de données.  
  
 **Date**  
 La date à laquelle l'Assistant Paramétrage du moteur de base de données a créé le rapport.  
  
 **Time**  
 L'heure à laquelle l'Assistant Paramétrage du moteur de base de données a créé le rapport.  
  
 **Server**  
 Le serveur qui était la cible de la charge de travail de l'Assistant Paramétrage du moteur de base de données.  
  
 **Base(s) de données à analyser**  
 La base de données concernée par les recommandations de l'Assistant Paramétrage du moteur de base de données.  
  
 **Fichier de charge de travail**  
 Apparaît lorsque la charge de travail est un fichier.  
  
 **Table de charge de travail**  
 Apparaît lorsque la charge de travail est une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Charge de travail**  
 Apparaît lorsque la charge de travail a été importée à partir de l'éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Durée maximale d'analyse**  
 La durée maximale autorisée pour l'analyse de l'Assistant Paramétrage du moteur de base de données.  
  
 **Temps écoulé durant l'analyse**  
 La durée réellement utilisée par l'Assistant Paramétrage du moteur de base de données pour analyser la charge de travail.  
  
 **Pourcentage d'amélioration attendu**  
 Le pourcentage d'amélioration attendu pour la charge de travail cible si toutes les recommandations de l'Assistant Paramétrage du moteur de base de données sont mises en œuvre.  
  
 **Espace maximal pour la recommandation (Mo)**  
 L'espace maximal autorisé pour les recommandations. Cette valeur doit être configurée avant d'effectuer l'analyse en cliquant sur le bouton **Options avancées** sous l'onglet **Options de paramétrage** .  
  
 **Espace occupé (Mo)**  
 L'espace actuellement utilisé par les index dans la base de données qui vient d'être analysée.  
  
 **Espace occupé par la recommandation (Mo)**  
 L'espace approximatif utilisé par les index si toutes les recommandations de l'Assistant Paramétrage du moteur de base de données sont mises en œuvre.  
  
 **Nombre d'événements dans la charge de travail**  
 Le nombre d'événements contenus dans la charge de travail.  
  
 **Nombre d'événements analysés**  
 Le nombre d'événements dans la charge de travail qui ont été analysés. Si un événement ne peut pas être analysé, il est répertorié dans le journal des paramétrages qui est disponible sous l'onglet **Progression** .  
  
 **Nombre d'instructions analysées**  
 Le nombre d'instructions dans la charge de travail qui ont été analysées. Si une instruction ne peut pas être analysée, elle est répertoriée dans le journal des paramétrages qui est disponible sous l'onglet **Progression** .  
  
 **Taux d'utilisation des instructions SELECT dans le jeu analysé**  
 Le pourcentage d'instructions SELECT parmi les instructions analysées. Apparaît uniquement si des instructions SELECT ont été analysées.  
  
 **Taux d'utilisation des instructions UPDATE dans le jeu analysé**  
 Le pourcentage d'instructions UPDATE parmi les instructions analysées. Apparaît uniquement si des instructions UPDATE ont été analysées.  
  
 **Nombre d'index à [créer | supprimer] recommandé**  
 Le nombre recommandé d'index à créer ou à supprimer pour la base de données analysée. Apparaît uniquement si les index font partie des recommandations.  
  
 **Nombre d'index sur les vues à [créer | supprimer] recommandé**  
 Le nombre recommandé d'index sur les vues à créer ou à supprimer pour la base de données analysée. Apparaît uniquement si les index sur les vues font partie des recommandations.  
  
 **Nombre de statistiques à créer recommandé**  
 Le nombre recommandé de statistiques à créer pour la base de données analysée. Apparaît uniquement si les statistiques font partie des recommandations.  
  
 **Select Report**  
 Affiche les détails du rapport sélectionné. Les colonnes de la grille varient en fonction du rapport.  
  
## <a name="see-also"></a> Voir aussi  
 [Start and Use the Database Engine Tuning Advisor](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Utilitaire dta](../../tools/dta/dta-utility.md)  
  
  
