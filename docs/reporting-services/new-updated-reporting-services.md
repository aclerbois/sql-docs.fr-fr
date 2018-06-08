---
title: Mise à jour - Documentations de Reporting Services pour SQL Server | Microsoft Docs
description: Affichez des extraits de contenu mis à jour récemment dans la documentation de Reporting Services pour Microsoft SQL Server.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.prod_service: sql-non-specified
ms.component: ssrs
ms.date: 04/28/2018
ms.openlocfilehash: 5fc0153c45e12f8cef47ef2b9eef10f73343b87e
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-and-recently-updated-reporting-services-for-sql-server"></a>Nouveau et mis à jour récemment : Reporting Services pour SQL Server



Presque tous les jours, Microsoft met à jour certains de ses articles sur son site web de documentation [Docs.Microsoft.com](http://docs.microsoft.com/). Cet article contient des extraits d’articles récemment mis à jour. Des liens vers de nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme qui est réexécuté régulièrement. Un extrait peut parfois apparaître avec une mise en forme imparfaite ou au format Markdown de l’article source. Les images ne sont jamais affichées ici.

Les mises à jour récentes sont signalées pour la plage de dates et le sujet suivants :



- *Plage de dates des mises à jour :* &nbsp; **03-02-2018** &nbsp; au &nbsp; **28-04-2018**
- *Domaine :* &nbsp; **Reporting Services pour SQL Server**.




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

1. [Journal des changements pour SQL Server Reporting Services](#TitleNum_1)
2. [Déployer le composant WebPart Visionneuse de rapports de SQL Server Reporting Services sur un site SharePoint](#TitleNum_2)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-change-log-for-sql-server-reporting-serviceschange-log-sql-server-reporting-servicesmd"></a>1. &nbsp; [Journal des changements pour SQL Server Reporting Services](change-log-sql-server-reporting-services.md)

*Mise à jour : 27/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Suivant](#TitleNum_2))

<!-- Source markdown line 28.  ms.author= "edugonz".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 025d58c01310215df423e7c3848e929b935bcfff 89310524b904dfba41e301cf1323b9a2a5e05064  (PR=575  ,  Filename=change-log-sql-server-reporting-services.md  ,  Dirpath=docs\reporting-services\  ,  MergeCommitSha40=98b04e2388872d1c7b8d819438c150c4ff2df94c) -->




- *Version 14.0.600.744, Date de publication : 25 avril 2018*
    - Résolutions de bogues :
        - La page Abonnement contrôlé par les données n’affiche pas l’option de remise qui a été créée
        - La mise à niveau de SSRS 2012 avec SSRS 2017 dans RSManagement génère une exception toutes les deux ou trois secondes
        - Impossible de modifier les valeurs par défaut pour les paramètres à valeurs multiples dans IE11
        - Les planifications sont vides chaque fois qu’une planification partagée est exécutée

- *Version 14.0.600.689, Date de publication : 28 février 2018*
    - Résolutions de bogues :
        - La visibilité de Paramètre de rapport dans un rapport lié est rétablie après la modification de ses propriétés
        - Le paramètre d’URL rc:Toolbar=false ne fonctionne pas dans les éditions Express
        - Les valeurs ne s’affichent pas en cas de présence d’expressions dans la zone de texte avec la propriété CanGrow définie sur false
        - Lien « En savoir plus » ajouté pour la clé de produit dans le programme d’installation
        - Le portail web avec l’authentification par formulaires personnalisée ignore le cookie d’expiration décalé
        - L’exportation vers Word crée une hauteur de ligne inégale si le contenu de la ligne est vide



&nbsp;

&nbsp;

---

<a name="TitleNum_2"/>

### <a name="2-nbsp-deploy-the-sql-server-reporting-services-report-viewer-web-part-on-a-sharepoint-sitereport-server-sharepointdeploy-report-viewer-web-partmd"></a>2. &nbsp; [Déployer le composant WebPart Visionneuse de rapports de SQL Server Reporting Services sur un site SharePoint](report-server-sharepoint/deploy-report-viewer-web-part.md)

*Mise à jour : 10/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_1))

<!-- Source markdown line 154.  ms.author= "maghan".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 42b763a610d378a7cedc9b5778bc5048d65fa730 ce7e2619689fd50be51a2689b07e3137470d9adf  (PR=5481  ,  Filename=deploy-report-viewer-web-part.md  ,  Dirpath=docs\reporting-services\report-server-sharepoint\  ,  MergeCommitSha40=31e0b2ebfc08c0b57870ac412fbd49bf3a1dffb8) -->



**Dépanner**


* Erreur lors de la désinstallation de SSRS si le mode intégré SharePoint est configuré :

    Install-SPRSService : [A] Impossible de caster Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService en [B]Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService. Le type A provient de 'Microsoft.ReportingServices.SharePoint.SharedService,Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' dans le contexte 'Default' à l’emplacement 'C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll'. Le type B provient de 'Microsoft.ReportingServices.SharePoint.SharedService,Version=12.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' dans le contexte 'Default' à l’emplacement 'C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll'.

    Solution :
    1. Supprimez le composant WebPart de la Visionneuse de rapports.
    2. Désinstallez SSRS.
    3. Réinstallez le composant WebPart de la Visionneuse de rapports.

* Erreur lors de la tentative de mise à niveau de SharePoint si le mode intégré SharePoint est configuré :

    Impossible de charger le fichier ou l’assembly 'Microsoft.ReportingServices.Alerting.ServiceContract, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' ou une de ses dépendances. Le système ne trouve pas le fichier spécifié. 00000000-0000-0000-0000-000000000000

    Solution :
    1. Supprimez le composant WebPart de la Visionneuse de rapports.
    2. Désinstallez SSRS.
    3. Réinstallez le composant WebPart de la Visionneuse de rapports.







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

