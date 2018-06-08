---
title: Mise à jour - opérations de SQL Studio docs | Documents Microsoft
description: Extraits de l’affichage de contenu mis à jour pour obtenir une documentation récemment modifié dans, pour les opérations de SQL Studio.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.prod_service: sql-non-specified
ms.component: ssops
ms.date: 04/28/2018
ms.openlocfilehash: 074ed6176480655d9d87a55eb87cbb76b3011b7e
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-and-recently-updated-sql-operations-studio-docs"></a>Nouveau et récemment mis à jour : les documents Studio des opérations SQL



Presque tous les jours Microsoft met à jour certains de ses articles existants sur son [Docs.Microsoft.com](http://docs.microsoft.com/) site Web de documentation. Cet article affiche des extraits d’articles récemment mis à jour. Des liens vers nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme est exécuté à nouveau régulièrement. Parfois un extrait peut apparaître avec mise en forme imparfait, ou en tant que markdown de l’article de la source. Les images ne sont jamais affichés ici.

Mises à jour récentes sont signalés pour la plage de dates suivante et l’objet :



- *Plage de dates de mises à jour :* &nbsp; **2018-02-03** &nbsp; - à - &nbsp; **2018-04-28**
- *Zone de sujet :* &nbsp; **SQL opérations Studio**.




&nbsp;

## <a name="new-articles-created-recently"></a>Nouveaux Articles vient d’être créés

Les liens suivants renvoient aux nouveaux articles ajoutés récemment.


- [Étendre les fonctionnalités des opérations de SQL Studio (version préliminaire)](extensions.md)

<!-- GeneMi:  I MANUALLY replace the ugly !INCLUDE with the name from inside the includes file. -->


&nbsp;

## <a name="updated-articles-with-excerpts"></a>Articles mis à jour avec des extraits

Cette section affiche les extraits des mises à jour collectés dans des articles qui ont récemment fait l’objet d’une mise à jour importante.

Les extraits affichés ici apparaissent séparés à partir de leur contexte sémantique spécifique. En outre, parfois un extrait est séparé à partir de la syntaxe markdown important qui l’entoure dans l’article. Par conséquent, ces extraits sont pour des conseils généraux. Les extraits permettent uniquement de savoir si votre intérêt justifie pris le temps de cliquer sur et consultez l’article.

Pour celles-ci et d’autres raisons, ne pas copier le code à partir de ces extraits et ne prennent pas en tant que vérité exacte tout extrait de texte. Au lieu de cela, consultez l’article.





&nbsp;

<a name="compactupdatedlist"/>

### <a name="compact-list-of-articles-updated-recently"></a>Liste compacte d’Articles récemment mis à jour

Cette liste compacte fournit des liens vers tous les articles mis à jour qui sont répertoriés dans la section des extraits.

1. [Téléchargez et installez Studio des opérations SQL (version préliminaire)](#TitleNum_1)
2. [Notes de publication SQL opérations Studio (version préliminaire)](#TitleNum_2)
3. [Didacticiel : Ajouter la *cinq premières requêtes* widget exemple au tableau de bord de base de données](#TitleNum_3)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-download-and-install-sql-operations-studio-previewdownloadmd"></a>1. &nbsp; [Téléchargez et installez Studio des opérations SQL (version préliminaire)](download.md)

*Mise à jour : 2018-04-25* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ([suivant](#TitleNum_2))

<!-- Source markdown line 64.  ms.author= "sstein".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 6b4f80ad54c599e4354303a736f62e9715f99a32 18b22f464bbd8676348248ba5717b71acbab1c0d  (PR=5676  ,  Filename=download.md  ,  Dirpath=docs\sql-operations-studio\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



   **Installation de Debian :**
```
   cd ~
   sudo dpkg -i ./Downloads/sqlops-linux-<version string>.deb

   sqlops
```

   **Installation de TPM :**
```
   cd ~
   yum install ./Downloads/sqlops-linux-<version string>.rpm

   sqlops
```

   **TAR.gz Installation :**



&nbsp;

&nbsp;

---

<a name="TitleNum_2"/>

### <a name="2-nbsp-sql-operations-studio-preview-release-notesrelease-notesmd"></a>2. &nbsp; [Notes de publication SQL opérations Studio (version préliminaire)](release-notes.md)

*Mise à jour : 2018-04-25* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ([précédente](#TitleNum_1) | [suivant](#TitleNum_3))

<!-- Source markdown line 20.  ms.author= "sstein".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 e2901d8a197f375f7d10ec93cbc9320362bf9278 0dde1aceceb339cb4cacd744e469f3d85d589668  (PR=5676  ,  Filename=release-notes.md  ,  Dirpath=docs\sql-operations-studio\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



**[Télécharger la version préliminaire publique d’avril]**


**Avril 2018 (avril Public Preview)**


date de publication : le 25 avril 2018 version : 0.28.6

Le *avril Public Preview* contient des correctifs de bogues et améliorations.

- Améliorations apportées à l’extension de l’aperçu de l’Agent SQL.
- Prise en charge des fichiers volumineux et protégés pour l’enregistrement Admin protégé améliorée et > fichiers 256M Studio des opérations SQL.
- Terminal intégré de fractionnement pour travailler avec plusieurs terminaux ouverts à la fois.
- Installation réduit un fichier sur le disque nombre pied d’impression pour des installations plus rapides et les temps de démarrage.
- Continue à résoudre les problèmes de GitHub :
   - Corriger [émettre 37](https://github.com/Microsoft/sqlopsstudio/issues/37): lors de la visionneuse graphique génère une erreur, un comportement inattendu se produit.
   - Corriger [émettre 462](https://github.com/Microsoft/sqlopsstudio/issues/462): demande de fonctionnalité : Option pour les groupes de serveurs à être développé par défaut.
   - Corriger [émettre 606](https://github.com/Microsoft/sqlopsstudio/issues/606): intellisense - suggestion incorrecte pour la commande « update ».
   - Corriger [émettre 967](https://github.com/Microsoft/sqlopsstudio/issues/967): attendez le plan de requête lorsque vous sélectionnez showplan XML dans la grille de résultats.
   - Corriger [émettre 1023](https://github.com/Microsoft/sqlopsstudio/issues/1023): ajouter des crochets pour l’appel de ms_foreachdb de flyfishingdba.
   - Corriger [émettre 1048](https://github.com/Microsoft/sqlopsstudio/issues/1048): erreur de négociation de préconnexion SSL/TLS.
   - Corriger [émettre 1050](https://github.com/Microsoft/sqlopsstudio/issues/1050): effacer insights afficher avant l’affichage d’erreur.
   - Corriger [émettre 1057](https://github.com/Microsoft/sqlopsstudio/issues/1057): nouvelles actions de requête dans le widget d’explorer et de restauration sont interrompues.



&nbsp;

&nbsp;

---

<a name="TitleNum_3"/>

### <a name="3-nbsp-tutorial-add-the-five-slowest-queries-sample-widget-to-the-database-dashboardtutorial-qds-sql-servermd"></a>3. &nbsp; [Didacticiel : Ajouter la *cinq premières requêtes* widget exemple au tableau de bord de base de données](tutorial-qds-sql-server.md)

*Mise à jour : 2018-04-25* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ([précédente](#TitleNum_2))

<!-- Source markdown line 94.  ms.author= "erickang".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 bc128acd966898cafc04381d7d83187d28466052 a47bf93ea4165618e0e514d7900ce1461f691aae  (PR=5676  ,  Filename=tutorial-qds-sql-server.md  ,  Dirpath=docs\sql-operations-studio\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



       },
       {
           "name": "Tasks",
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 1
           },
           "widget": {
               "tasks-widget": {}
           }
       },
       {
           "gridItemConfig": {
               "sizex": 1,
               "sizey": 2
           },
           "widget": {
               "explorer-widget": {}
           }







## <a name="similar-articles-about-new-or-updated-articles"></a>Articles similaires sur les articles nouveaux ou mis à jour

Cette section liste les articles très similaires récemment mis à jour dans d’autres domaines, dans notre dépôt public GitHub.com : [MicrosoftDocs/sql-docs](https://github.com/MicrosoftDocs/sql-docs/).



#### <a name="subject-areas-that-do-have-new-or-recently-updated-articles"></a>Domaines *avec* des articles nouveaux ou mis à jour récemment

- [Nouveau + mis à jour (11 + 6) : &nbsp; &nbsp; **avancées d’Analytique pour SQL** documents](../advanced-analytics/new-updated-advanced-analytics.md)
- [Nouveau + mis à jour (18 + 0) : &nbsp; &nbsp; **Analysis Services pour SQL** documents](../analysis-services/new-updated-analysis-services.md)
- [Nouveau + mis à jour (218 + 14) : **se connecter à SQL** documents](../connect/new-updated-connect.md)
- [Nouveau + mis à jour (14 + 0) : &nbsp; &nbsp; **moteur de base de données pour SQL** documents](../database-engine/new-updated-database-engine.md)
- [Nouveau + mis à jour (3 + 2) : &nbsp; &nbsp; **Integration Services pour SQL** documents](../integration-services/new-updated-integration-services.md)
- [Nouveau + mis à jour (3 + 3) : &nbsp; &nbsp; **Linux pour SQL** documents](../linux/new-updated-linux.md)
- [Nouveau + mis à jour (7 + 10) : &nbsp; &nbsp; **des bases de données relationnelles pour SQL** documents](../relational-databases/new-updated-relational-databases.md)
- [Nouveau + mis à jour (0 + 2) : &nbsp; &nbsp; **Reporting Services pour SQL** documents](../reporting-services/new-updated-reporting-services.md)
- [Nouveau + mis à jour (1 + 3) : &nbsp; &nbsp; **SQL opérations Studio** documents](../sql-operations-studio/new-updated-sql-operations-studio.md)
- [Nouveau + mis à jour (2 + 3) : &nbsp; &nbsp; **Microsoft SQL Server** documents](../sql-server/new-updated-sql-server.md)
- [Nouveau + mis à jour (1 + 1) : &nbsp; &nbsp; **SQL Server Data Tools (SSDT)** documents](../ssdt/new-updated-ssdt.md)
- [Nouveau + mis à jour (5 + 2) : &nbsp; &nbsp; **SQL Server Management Studio (SSMS)** documents](../ssms/new-updated-ssms.md)
- [Nouveau + mis à jour (0 + 2) : &nbsp; &nbsp; **Transact-SQL** documents](../t-sql/new-updated-t-sql.md)
- [Nouveau + mis à jour (1 + 1) : &nbsp; &nbsp; **Tools pour SQL** documents](../tools/new-updated-tools.md)



#### <a name="subject-areas-that-do-not-have-any-new-or-recently-updated-articles"></a>Domaines *sans* article nouveau ou mis à jour récemment

- [Nouveau + mis à jour (0 0 +) : **système de plateforme d’Analytique pour SQL** documents](../analytics-platform-system/new-updated-analytics-platform-system.md)
- [Nouveau + mis à jour (0 0 +) : **Data Quality Services pour SQL** documents](../data-quality-services/new-updated-data-quality-services.md)
- [Nouveau + mis à jour (0 0 +) : **Extensions DMX (Data Mining) pour SQL** documents](../dmx/new-updated-dmx.md)
- [Nouveaux + Mis à jour (0 + 0) : **Master Data Services (MDS) for SQL** (documentation)](../master-data-services/new-updated-master-data-services.md)
- [Nouveau + mis à jour (0 0 +) : **MDX (Multidimensional Expressions) pour SQL** documents](../mdx/new-updated-mdx.md)
- [Nouveau + mis à jour (0 0 +) : **ODBC (Open Database Connectivity) pour SQL** documents](../odbc/new-updated-odbc.md)
- [Nouveau + mis à jour (0 0 +) : **PowerShell pour SQL** documents](../powershell/new-updated-powershell.md)
- [Nouveau + mis à jour (0 0 +) : **exemples pour SQL** documents](../samples/new-updated-samples.md)
- [Nouveau + mis à jour (0 0 +) : **SQL Server Migration Assistant (SSMA)** documents](../ssma/new-updated-ssma.md)
- [Nouveau + mis à jour (0 0 +) : **XQuery pour SQL** documents](../xquery/new-updated-xquery.md)

