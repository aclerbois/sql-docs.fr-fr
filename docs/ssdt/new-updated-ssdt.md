---
title: Mise à jour - Documentation de SSDT pour SQL Server | Microsoft Docs
description: Affichez des extraits de contenu mis à jour récemment dans la documentation pour SQL Server Data Tools (SSDT) pour Microsoft SQL Server.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.prod_service: sql-non-specified
ms.component: ssdt
ms.date: 04/28/2018
ms.openlocfilehash: 99b0844803e1ce95bd6f73b0d45a2baf867428ba
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-and-recently-updated-sql-server-data-tools-ssdt"></a>Nouveautés et mises à jour récentes : SQL Server Data Tools (SSDT)



Presque tous les jours, Microsoft met à jour certains de ses articles sur son site web de documentation [Docs.Microsoft.com](http://docs.microsoft.com/). Cet article contient des extraits d’articles récemment mis à jour. Des liens vers de nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme qui est réexécuté régulièrement. Un extrait peut parfois apparaître avec une mise en forme imparfaite ou au format Markdown de l’article source. Les images ne sont jamais affichées ici.

Les mises à jour récentes sont signalées pour la plage de dates et le sujet suivants :



- *Plage de dates des mises à jour :* &nbsp; **03-02-2018** &nbsp; au &nbsp; **28-04-2018**
- *Domaine :* &nbsp; **SQL Server Data Tools (SSDT)**.




&nbsp;

## <a name="new-articles-created-recently"></a>Nouveaux articles créés récemment

Les liens suivants renvoient aux nouveaux articles ajoutés récemment.


1. [Prise en charge d’Azure Active Directory dans SQL Server Data Tools (SSDT)](azure-active-directory.md)



&nbsp;

## <a name="updated-articles-with-excerpts"></a>Articles mis à jour avec des extraits

Cette section affiche les extraits des mises à jour collectés dans des articles qui ont récemment fait l’objet d’une mise à jour importante.

Les extraits affichés ici apparaissent séparés de leur contexte sémantique propre. Un extrait est parfois séparé de la syntaxe Markdown importante qui l’entoure dans l’article. Ces extraits sont donc donnés à titre indicatif uniquement. Les extraits vous permettent seulement de savoir si les articles correspondants vont vous intéresser et si oui, de cliquer dessus pour les consulter.

Pour cela et pour d’autres raisons, ne copiez pas le code de ces extraits et ne prenez pas à la lettre les extraits de texte. Consultez plutôt l’article.





&nbsp;

<a name="compactupdatedlist"/>

### <a name="compact-list-of-articles-updated-recently"></a>Liste compacte d’articles mis à jour récemment

Cette liste compacte fournit des liens vers tous les articles mis à jour qui sont répertoriés dans la section des extraits.

1. [Journal des modifications de SQL Server Data Tools (SSDT)](#TitleNum_1)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-changelog-for-sql-server-data-tools-ssdtchangelog-for-sql-server-data-tools-ssdtmd"></a>1. &nbsp; [Journal des modifications de SQL Server Data Tools (SSDT)](changelog-for-sql-server-data-tools-ssdt.md)

*Mis à jour : 25-04-2018*&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 

<!-- Source markdown line 29.  ms.author= "sstein".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 de18314845cffa197b3fd2ed868f2c330760bedb 5487de1ed57c16a6517a3a8849f412c208f1889f  (PR=5676  ,  Filename=changelog-for-sql-server-data-tools-ssdt.md  ,  Dirpath=docs\ssdt\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->





**SSDT pour Visual Studio 2017 (15.6.0)**

Numéro de build : 14.0.16162.0 Date de publication : 10 avril 2018

**Nouveautés**


**SSIS :**

1.  Résolution du problème où la tâche de traitement AS ne journalisait aucune étape de traitement quand SQLServer2016 et SQLServer2017 étaient ciblés
2.  Résolution du problème où une violation d’accès se produisait à l’ouverture de dtsx avec des noms de tâche très longs non anglais dans SSDT
3.  Résolution du problème où parfois la liste des variables de ScriptTask disparaissait de l’interface utilisateur de la tâche
4.  Résolution du problème où l’ajout d’une copie du package existant échouait quand l’emplacement du package était SQL Server
5.  Résolution du problème où le focus restait coincé lors de l’accès à la zone de liste déroulante dans une boîte de dialogue d’éditeur.
6.  Résolution du problème où l’arrière-plan ne changeait pas alors que le thème de Visual Studio changeait.
7.  Résolution du problème où l’étiquette d’annotation et de chargement était invisible dans le thème foncé.
8.  Résolution du problème où la propriété d’état n’était pas correctement définie pour les éléments désactivés de la boîte à outils SSIS.
9.  Résolution du problème où l’exécution de WebServiceTask échouait tout le temps.
10. Résolution du problème où le déploiement du package échouait si la chaîne de connexion était définie sur une variable avec une expression dépendante des paramètres du projet.

**Programme d’installation :**

1.  Ajoutez le lien du « Programme d’amélioration du produit pour SQL Server Data Tools » dans la clause d’exclusion de confidentialité.
2.  Résolution du problème où la fenêtre du programme d’installation de Visual Studio apparaissait quand « Installer la nouvelle instance SQL Server Data Tools pour Visual Studio 2017 » était sélectionné

**Problèmes connus :**

1.  La tâche d’exécution de package SSIS ne prend pas en charge le débogage quand ExecuteOutOfProcess a la valeur True. Ce problème s’applique uniquement au débogage. L’enregistrement, le déploiement et l’exécution via DTExec.exe ou le catalogue SSIS ne sont pas impactés.



**SSDT pour Visual Studio 2017 (15.5.2)**

Numéro de build : 14.0.16156.0

**Nouveautés**


**SSIS**
1.  Correction d’un problème qui entraînait l’échec de la migration des projets SSIS 2008 quand SSAS et SSIS étaient tous deux installés sur la même instance de Visual Studio 2017.
2.  Correction d’un problème qui faisait que les projets Rdlc ne pouvaient pas être générés quand le Concepteur de rapports Rdlc et SSIS étaient installés sur la même instance de Visual Studio 2017.







## <a name="similar-articles-about-new-or-updated-articles"></a>Articles similaires sur les articles nouveaux ou mis à jour

Cette section liste les articles très similaires récemment mis à jour dans d’autres domaines, dans notre dépôt public GitHub.com : [MicrosoftDocs/sql-docs](https://github.com/MicrosoftDocs/sql-docs/).



#### <a name="subject-areas-that-do-have-new-or-recently-updated-articles"></a>Domaines *avec* des articles nouveaux ou mis à jour récemment

- [Nouveaux + Mis à jour (11 + 6) :&nbsp; &nbsp;**Advanced Analytics pour SQL** (documentation)](../advanced-analytics/new-updated-advanced-analytics.md)
- [Nouveaux + Mis à jour (18 + 0) : &nbsp; &nbsp;**Analysis Services pour SQL**(documentation)](../analysis-services/new-updated-analysis-services.md)
- [Nouveaux + Mis à jour (218 + 14) : **Connexion à SQL** (documentation)](../connect/new-updated-connect.md)
- [Nouveaux + Mis à jour (14 + 0) :&nbsp; &nbsp;**Moteur de base de données pour SQL** (documentation)](../database-engine/new-updated-database-engine.md)
- [Nouveaux + Mis à jour (3 + 2) :&nbsp; &nbsp; **Integration Services pour SQL** (documentation)](../integration-services/new-updated-integration-services.md)
- [Nouveaux + Mis à jour (3 + 3) :&nbsp; &nbsp; **Linux pour SQL** (documentation)](../linux/new-updated-linux.md)
- [Nouveaux + Mis à jour (7 + 10) :&nbsp; &nbsp;**Bases de données relationnelles pour SQL** (documentation)](../relational-databases/new-updated-relational-databases.md)
- [Nouveaux + Mis à jour (0 + 2) :&nbsp; &nbsp; **Reporting Services pour SQL** (documentation)](../reporting-services/new-updated-reporting-services.md)
- [Nouveaux + Mis à jour (1 + 3) :&nbsp; &nbsp; **SQL Operations Studio** (documentation)](../sql-operations-studio/new-updated-sql-operations-studio.md)
- [Nouveaux + Mis à jour (2 + 3) :&nbsp; &nbsp; **Microsoft SQL Server** (documentation)](../sql-server/new-updated-sql-server.md)
- [Nouveaux + Mis à jour (1 + 1) :&nbsp; &nbsp; **SQL Server Data Tools (SSDT)** (documentation)](../ssdt/new-updated-ssdt.md)
- [Nouveaux + Mis à jour (5 + 2) :&nbsp; &nbsp; **SQL Server Management Studio (SSMS)** (documentation)](../ssms/new-updated-ssms.md)
- [Nouveaux + Mis à jour (0 + 2) :&nbsp; &nbsp; **Transact-SQL** (documentation)](../t-sql/new-updated-t-sql.md)
- [Nouveaux + Mis à jour (1 + 1) : &nbsp; &nbsp; **Outils pour SQL** documentation](../tools/new-updated-tools.md)



#### <a name="subject-areas-that-do-not-have-any-new-or-recently-updated-articles"></a>Domaines *sans* article nouveau ou mis à jour récemment

- [Nouveaux + Mis à jour (0 + 0) : **Système de plateforme d’analyse pour SQL** documentation](../analytics-platform-system/new-updated-analytics-platform-system.md)
- [Nouveaux + Mis à jour (0 + 0) : **Data Quality Services pour SQL** (documentation)](../data-quality-services/new-updated-data-quality-services.md)
- [Nouveaux + Mis à jour (0 + 0) : **Extensions DMX (Data Mining Extensions) pour SQL** (documentation)](../dmx/new-updated-dmx.md)
- [Nouveaux + Mis à jour (0 + 0) : **Master Data Services (MDS) for SQL** (documentation)](../master-data-services/new-updated-master-data-services.md)
- [Nouveaux + Mis à jour (0 + 0) : **Expressions MDX (Multidimensional Expressions) pour SQL** (documentation)](../mdx/new-updated-mdx.md)
- [Nouveaux + Mis à jour (0 + 0) : **ODBC (Open Database Connectivity) pour SQL** (documentation)](../odbc/new-updated-odbc.md)
- [Nouveaux + Mis à jour (0 + 0) : **PowerShell pour SQL** (documentation)](../powershell/new-updated-powershell.md)
- [Nouveaux + Mis à jour (0 + 0) : **Exemples pour SQL** (documentation)](../samples/new-updated-samples.md)
- [Nouveaux + Mis à jour (0 + 0) : **SQL Server Migration Assistant (SSMA)** (documentation)](../ssma/new-updated-ssma.md)
- [Nouveaux + Mis à jour (0 + 0) : **XQuery pour SQL** (documentation)](../xquery/new-updated-xquery.md)

