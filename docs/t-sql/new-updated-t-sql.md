---
title: 'Mise à jour : T-SQL (documentation) | Microsoft Docs'
description: Affichez les extraits du contenu mis à jour de la documentation changée récemment pour Transact-SQL.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.prod_service: sql-non-specified
ms.component: t-sql
ms.date: 04/28/2018
ms.openlocfilehash: b1bc891bf7edc4cd82c38c8d647c279828190298
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-and-recently-updated-transact-sql-docs"></a>Articles nouveaux et récemment mis à jour : Transact-SQL (documentation)



Presque tous les jours, Microsoft met à jour certains de ses articles sur son site web de documentation [Docs.Microsoft.com](http://docs.microsoft.com/). Cet article contient des extraits d’articles récemment mis à jour. Des liens vers de nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme qui est réexécuté régulièrement. Un extrait peut parfois apparaître avec une mise en forme imparfaite ou au format Markdown de l’article source. Les images ne sont jamais affichées ici.

Les mises à jour récentes sont signalées pour la plage de dates et le sujet suivants :



- *Plage de dates des mises à jour :* &nbsp; **03-02-2018** &nbsp; au &nbsp; **28-04-2018**
- *Domaine :* &nbsp; **T-SQL**.




&nbsp;

## <a name="new-articles-created-recently"></a>Nouveaux articles créés récemment

Les liens suivants renvoient aux nouveaux articles ajoutés récemment.


***Il n’y a aucun nouvel article pour cette fois.***



&nbsp;

## <a name="updated-articles-with-excerpts"></a>Articles mis à jour avec des extraits

Cette section affiche les extraits des mises à jour collectés dans des articles qui ont récemment fait l’objet d’une mise à jour importante.

Les extraits affichés ici apparaissent séparés de leur contexte sémantique propre. Un extrait est parfois séparé de la syntaxe Markdown importante qui l’entoure dans l’article. Ces extraits sont donc donnés à titre indicatif uniquement. Les extraits vous permettent seulement de savoir si les articles correspondants vont vous intéresser et si oui, de cliquer dessus pour les consulter.

Pour cela et pour d’autres raisons, ne copiez pas le code de ces extraits et ne prenez pas à la lettre les extraits de texte. Consultez plutôt l’article.





&nbsp;

<a name="compactupdatedlist"/>

### <a name="compact-list-of-articles-updated-recently"></a>Liste compacte d’articles mis à jour récemment

Cette liste compacte fournit des liens vers tous les articles mis à jour qui sont répertoriés dans la section des extraits.

1. [MODIFIER LA CONFIGURATION DÉLIMITÉE À LA BASE DE DONNÉES (Transact-SQL)](#TitleNum_1)
2. [Instructions RESTORE (Transact-SQL)](#TitleNum_2)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-alter-database-scoped-configuration-transact-sqlstatementsalter-database-scoped-configuration-transact-sqlmd"></a>1. &nbsp; [MODIFIER LA CONFIGURATION DÉLIMITÉE À LA BASE DE DONNÉES (Transact-SQL)](statements/alter-database-scoped-configuration-transact-sql.md)

*Mise à jour : 13/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Suivant](#TitleNum_2))

<!-- Source markdown line 150.  ms.author= "carlrab".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 f6833910b664d0059a9073589807b195052325b3 bf969f123b22e6ebc650380a6905156f26ed6ca6  (PR=0  ,  Filename=alter-database-scoped-configuration-transact-sql.md  ,  Dirpath=docs\t-sql\statements\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



XTP_PROCEDURE_EXECUTION_STATISTICS  **=** { ON | **OFF** }

**S’applique à** :  *{Included-Content-Goes-Here}*

Active ou désactive la collecte de statistiques d’exécution au niveau du module pour les modules T-SQL compilés en mode natif dans la base de données actuelle. La valeur par défaut est OFF. Les statistiques d’exécution sont disponibles dans [sys.dm_exec_procedure_stats].

Les statistiques d’exécution au niveau du module pour les modules T-SQL compilés en mode natif sont collectées si cette option est activée (ON) ou si la collecte des statistiques est activée avec [sp_xtp_control_proc_exec_stats].

XTP_QUERY_EXECUTION_STATISTICS  **=** { ON | **OFF** }

**S’applique à** :  *{Included-Content-Goes-Here}*

Active ou désactive la collecte de statistiques d’exécution au niveau de l’instruction pour les modules T-SQL compilés en mode natif dans la base de données actuelle. La valeur par défaut est OFF. Les statistiques d’exécution sont disponibles dans [sys.dm_exec_query_stats] et dans [Magasin des requêtes].

Les statistiques d’exécution au niveau de l’instruction pour les modules T-SQL compilés en mode natif sont collectées si cette option est activée (ON) ou si la collecte des statistiques est activée avec [sp_xtp_control_query_exec_stats].

Pour plus d’informations sur la surveillance des performances des modules T-SQL compilés en mode natif, consultez [Surveillance des performances des procédures stockées compilées en mode natif].



&nbsp;

&nbsp;

---

<a name="TitleNum_2"/>

### <a name="2-nbsp-restore-statements-transact-sqlstatementsrestore-statements-transact-sqlmd"></a>2. &nbsp; [Instructions RESTORE (Transact-SQL)](statements/restore-statements-transact-sql.md)

*Mise à jour : 13/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_1))

<!-- Source markdown line 339.  ms.author= "barbkess".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 2902efb58bb964d3e9a0660956690d37f0397c00 36186a7cffd26ffa54a83e383ffd752dbc568a4d  (PR=0  ,  Filename=restore-statements-transact-sql.md  ,  Dirpath=docs\t-sql\statements\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



**Remarques générales sur SQL Database Managed Instance**


Dans le cas d’une restauration asynchrone, la restauration continue même si la connexion cliente s’arrête. Si votre connexion est supprimée, vous pouvez afficher la vue [sys.dm_operation_status] pour vérifier l’état d’une opération de restauration (et d’une opération CREATE ou DROP sur une base de données).

Les options de base de données suivantes sont définies/remplacées et ne peuvent pas être modifiées ultérieurement :

- NEW_BROKER (si le service Broker n’est pas activé dans le fichier .bak)
- ENABLE_BROKER (si le service Broker n’est pas activé dans le fichier .bak)
- AUTO_CLOSE=OFF (si une base de données dans le fichier .bak est définie avec AUTO_CLOSE=ON)
- RECOVERY FULL (si une base de données dans le fichier .bak est définie avec le mode de récupération SIMPLE ou BULK_LOGGED)
- Un groupe de fichiers à mémoire optimisée, appelé XTP, est ajouté au fichier source .bak s’il n’y est pas déjà. Chaque groupe de fichiers à mémoire optimisée existant est renommé XTP
- Les options SINGLE_USER et RESTRICTED_USER sont remplacées par l’option MULTI_USER

**Limitations pour SQL Database Managed Instance**

Les limitations suivantes s’appliquent :

- Les fichiers .bak contenant plusieurs jeux de sauvegarde ne peuvent pas être restaurés.
- Les fichiers .bak contenant plusieurs fichiers journaux ne peuvent pas être restaurés.
- La restauration échoue si le fichier .bak contient des données FILESTREAM.
- Les sauvegardes contenant des bases de données avec des objets en mémoire active ne peuvent pas être restaurées.
- Les sauvegardes contenant des bases de données avec des objets en mémoire à certains points ne peuvent pas être restaurées.
- Les sauvegardes contenant des bases de données en mode lecture seule ne peuvent pas être restaurées. Cette limitation sera supprimée prochainement.

Pour plus d’informations, consultez [Instance managée]







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

