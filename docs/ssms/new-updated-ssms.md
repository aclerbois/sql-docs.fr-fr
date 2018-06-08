---
title: Mise à jour - Documentation de SSMS pour SQL Server | Microsoft Docs
description: Affichez des extraits de contenu mis à jour récemment dans la documentation, pour SQL Server Management Studio (SSMS) pour Microsoft SQL Server.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.prod_service: sql-non-specified
ms.component: ssms
ms.date: 04/28/2018
ms.openlocfilehash: 75c3dafdfea93a1cb16b9101d187c208e6de2796
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34585911"
---
# <a name="new-and-recently-updated-sql-server-management-studio-ssms-for-sql-server"></a>Nouveautés et mises à jour récentes : SQL Server Management Studio (SSMS) pour SQL Server



Presque tous les jours, Microsoft met à jour certains de ses articles sur son site web de documentation [Docs.Microsoft.com](http://docs.microsoft.com/). Cet article contient des extraits d’articles récemment mis à jour. Des liens vers de nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme qui est réexécuté régulièrement. Un extrait peut parfois apparaître avec une mise en forme imparfaite ou au format Markdown de l’article source. Les images ne sont jamais affichées ici.

Les mises à jour récentes sont signalées pour la plage de dates et le sujet suivants :



- *Plage de dates des mises à jour :* &nbsp; **03-02-2018** &nbsp; au &nbsp; **28-04-2018**
- *Domaine :* &nbsp; **SQL Server Management Studio (SSMS)**.




&nbsp;

## <a name="new-articles-created-recently"></a>Nouveaux articles créés récemment

Les liens suivants renvoient aux nouveaux articles ajoutés récemment.


1. [Tutoriel : Se connecter à une instance SQL Server et l’interroger en utilisant SQL Server Management Studio](tutorials/connect-query-sql-server.md)
2. [Tutoriel : Générer des scripts d’objets dans SQL Server Management Studio](tutorials/scripting-ssms.md)
3. [Tutoriel : Composants et configuration de SQL Server Management Studio](tutorials/ssms-configuration.md)
4. [Tutoriel : Conseils et astuces supplémentaires sur l’utilisation de SSMS](tutorials/ssms-tricks.md)
5. [Tutoriel : Utilisation de modèles dans SQL Server Management Studio](tutorials/templates-ssms.md)



&nbsp;

## <a name="updated-articles-with-excerpts"></a>Articles mis à jour avec des extraits

Cette section affiche les extraits des mises à jour collectés dans des articles qui ont récemment fait l’objet d’une mise à jour importante.

Les extraits affichés ici apparaissent séparés de leur contexte sémantique propre. Un extrait est parfois séparé de la syntaxe Markdown importante qui l’entoure dans l’article. Ces extraits sont donc donnés à titre indicatif uniquement. Les extraits vous permettent seulement de savoir si les articles correspondants vont vous intéresser et si oui, de cliquer dessus pour les consulter.

Pour cela et pour d’autres raisons, ne copiez pas le code de ces extraits et ne prenez pas à la lettre les extraits de texte. Consultez plutôt l’article.





&nbsp;

<a name="compactupdatedlist"/>

### <a name="compact-list-of-articles-updated-recently"></a>Liste compacte d’articles mis à jour récemment

Cette liste compacte fournit des liens vers tous les articles mis à jour qui sont répertoriés dans la section des extraits.

1. [SQL Server Management Studio : journal des modifications (SSMS)](#TitleNum_1)
2. [Tutoriels pour SQL Server Management Studio (SSMS)](#TitleNum_2)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-sql-server-management-studio---changelog-ssmssql-server-management-studio-changelog-ssmsmd"></a>1. &nbsp; [SQL Server Management Studio - Journal des modifications (SSMS)](sql-server-management-studio-changelog-ssms.md)

*Mise à jour : 25/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Suivant](#TitleNum_2))

<!-- Source markdown line 27.  ms.author= "sstein".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 eb641ac39386a26a76dc303f5bd55eb3f9f4c78d f560f09b51ea30b255c10f7eb86857c3090881d9  (PR=5676  ,  Filename=sql-server-management-studio-changelog-ssms.md  ,  Dirpath=docs\ssms\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



**[SSMS 17.6]**


Numéro de version : 17.6<br>
Numéro de build : 14.0.17230.0<br>
Date de sortie : 20 mars 2018

**Nouveautés**


**SSMS général**

SQL Database Managed Instance :

- Ajout de la prise en charge d’[Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance). Azure SQL Database Managed Instance (en préversion) est une nouvelle version d’Azure SQL Database offrant une compatibilité proche de 100 % avec SQL Server local, une implémentation native de [réseau virtuel (VNet)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) qui traite les problèmes de sécurité courants et un [modèle d’entreprise](https://azure.microsoft.com/pricing/details/sql-database/) propice pour les clients de SQL Server local.
- Prise en charge de scénarios de gestion courants tels que les suivants :
   - Créer et modifier des bases de données.
   - Sauvegarder et restaurer des bases de données.
   - Importation, exportation, extraction et publication d’applications de la couche Données.
   - Affichage et modification de propriétés du serveur.
   - Prise en charge complète de l’Explorateur d’objets.
   - Écriture d’objets de base de données sous forme de scripts.
   - Prise en charge des travaux de l’Agent SQL.
   - Prise en charge des serveurs liés.
- Vous pouvez en savoir plus sur les instances managées [ici](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).

Explorateur d'objets :
- Paramètres ajoutés pour ne pas imposer des crochets autour des noms lors d’un glisser-déplacer à partir de l’Explorateur d’objets vers une fenêtre de requête. (Suggestions des utilisateurs [32911933](https://feedback.azure.com/forums/908035-sql-server/suggestions/32911933) et [32671051](https://feedback.azure.com/forums/908035-sql-server/suggestions/32671051).)

Classification des données :
- Améliorations générales et résolutions de bogues.

**Integration Services (IS)**

- Ajout de la prise en charge du déploiement de packages sur une instance [SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance).



&nbsp;

&nbsp;

---

<a name="TitleNum_2"/>

### <a name="2-nbsp-tutorials-for-sql-server-management-studio-ssmstutorialstutorial-sql-server-management-studiomd"></a>2. &nbsp;[Tutoriels pour SQL Server Management Studio (SSMS)](tutorials/tutorial-sql-server-management-studio.md)

*Mise à jour : 25/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_1))

<!-- Source markdown line 49.  ms.author= "mathoma".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 b272c75bab04912ebb706098cf6b6a6c80d9e2b8 d453ebc8251fb83ffcb1af58c9a08bb65b3e9fa2  (PR=5676  ,  Filename=tutorial-sql-server-management-studio.md  ,  Dirpath=docs\ssms\tutorials\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



- Tutoriel : Se connecter à SQL Server et l’interroger à l’aide de SSMS

    Dans ce tutoriel, vous allez apprendre à vous connecter à votre instance SQL Server. Vous allez également étudier quelques commandes T-SQL (Transact-SQL) de base pour créer, puis interroger une base de données.

- Tutoriel : Génération de scripts d’objets dans SSMS

    Dans ce tutoriel, vous allez apprendre à générer le script de différents objets dans SSMS, dont les bases de données et les requêtes.

- Tutoriel : Utilisation de modèles dans SSMS

    Dans ce tutoriel, vous allez apprendre à utiliser les modèles prédéfinis dans SSMS. Les modèles sont une fonctionnalité peu connue qui stocke un nombre d’extraits de code Transact-SQL pour diverses tâches d’administration de base de données.

- Tutoriel : Configuration de SSMS

    Dans ce tutoriel, vous apprenez les bases de la configuration de votre environnement SSMS, comme le changement de la disposition de l’environnement. Ce tutoriel explique également quels sont les différents composants SSMS.


- Tutoriel : Conseils et astuces supplémentaires sur l’utilisation de SSMS

    Dans ce tutoriel, vous allez découvrir d’autres conseils et astuces pour utiliser SSMS. Le tutoriel inclut les thèmes suivants :
    - Ajout et suppression de commentaires dans le texte
    - Mise en retrait du texte
    - Filtrage des objets dans l’Explorateur d’objets
    - Accès à votre journal des erreurs SQL Server
    - Recherche du nom de votre instance








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

