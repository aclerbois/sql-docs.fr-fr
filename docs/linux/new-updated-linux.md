---
title: Mise à jour - SQL Server sur Linux docs | Documents Microsoft
description: Extraits de l’affichage de contenu mis à jour pour obtenir une documentation récemment modifié, pour Microsoft SQL Server sur Linux.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.component: linux
ms.date: 04/28/2018
ms.openlocfilehash: 0563ef507b08df483c28266fdd2566c6acdd957d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="new-and-recently-updated-sql-server-on-linux-docs"></a>Nouveau et récemment mis à jour : SQL Server sur Linux docs



Presque tous les jours Microsoft met à jour certains de ses articles existants sur son [Docs.Microsoft.com](http://docs.microsoft.com/) site Web de documentation. Cet article affiche des extraits d’articles récemment mis à jour. Des liens vers nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme est exécuté à nouveau régulièrement. Parfois un extrait peut apparaître avec mise en forme imparfait, ou en tant que markdown de l’article de la source. Les images ne sont jamais affichés ici.

Mises à jour récentes sont signalés pour la plage de dates suivante et l’objet :



- *Plage de dates des mises à jour :* &nbsp; **03-02-2018** &nbsp; au &nbsp; **28-04-2018**
- *Zone de sujet :* &nbsp; **Microsoft SQL Server sur Linux**.




&nbsp;

## <a name="new-articles-created-recently"></a>Nouveaux Articles vient d’être créés

Les liens suivants renvoient aux nouveaux articles ajoutés récemment.


1. [Authentification Active Directory pour SQL Server sur Linux](sql-server-linux-active-directory-auth-overview.md)
2. [Configurer SQL Server groupe de disponibilité AlwaysOn sur Windows et Linux (multiplateforme)](sql-server-linux-availability-group-cross-platform.md)
3. [Fonctionnent toujours sur les groupes de disponibilité sur Linux](sql-server-linux-availability-group-operate-ha.md)



&nbsp;

## <a name="updated-articles-with-excerpts"></a>Articles mis à jour avec des extraits

Cette section affiche les extraits des mises à jour collectés dans des articles qui ont récemment fait l’objet d’une mise à jour importante.

Les extraits affichés ici apparaissent séparés à partir de leur contexte sémantique spécifique. En outre, parfois un extrait est séparé à partir de la syntaxe markdown important qui l’entoure dans l’article. Par conséquent, ces extraits sont pour des conseils généraux. Les extraits permettent uniquement de savoir si votre intérêt justifie pris le temps de cliquer sur et consultez l’article.

Pour celles-ci et d’autres raisons, ne pas copier le code à partir de ces extraits et ne prennent pas en tant que vérité exacte tout extrait de texte. Au lieu de cela, consultez l’article.





&nbsp;

<a name="compactupdatedlist"/>

### <a name="compact-list-of-articles-updated-recently"></a>Liste compacte d’Articles récemment mis à jour

Cette liste compacte fournit des liens vers tous les articles mis à jour qui sont répertoriés dans la section des extraits.

1. [Configurez des référentiels pour l’installation et la mise à niveau de SQL Server sur Linux](#TitleNum_1)
2. [Configurer SQL Server sur Linux avec l’outil mssql-conf](#TitleNum_2)
3. [Notes de publication pour 2017 du serveur SQL sur Linux](#TitleNum_3)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-configure-repositories-for-installing-and-upgrading-sql-server-on-linuxsql-server-linux-change-repomd"></a>1. &nbsp; [Configurez des référentiels pour l’installation et la mise à niveau de SQL Server sur Linux](sql-server-linux-change-repo.md)

*Mise à jour : 25/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Suivant](#TitleNum_2))

<!-- Source markdown line 72.  ms.author= jroth.  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 b5ccaa0fcb8895f25c162e4e0494ad4872773de3 29a959be6ee7d58fe0c53e8f91bdd282fb2e6d29  (PR=5676  ,  Filename=sql-server-linux-change-repo.md  ,  Dirpath=docs\linux\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



- Imprimer le contenu du fichier.

```
   sudo cat /etc/yum.repos.d/mssql-server.repo
```

- La propriété **name** est le réertoire configuré. Vous pouvez identifier avec la table dans la section [référentiels] de cet article.

**Supprimer l’ancien référentiel (RHEL)**

Si nécessaire, supprimez l’ancien référentiel avec la commande suivante.

```
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Cette commande suppose que le fichier identifié dans la section précédente a été nommé **mssql-server.repo**.

**Configurer le nouveau référentiel (RHEL)**

Configurez le nouveau référentiel à utiliser pour les mises à niveau et les installations de SQL Server. Utilisez une des commandes suivantes pour configurer le référentiel de votre choix.

| Référentiel | Command |
|---|---|
| **CU** | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo` |
| **GDR** | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017-gdr.repo` |

**<a id="sles"></a> Configurez des référentiels SLES**

Utilisez les étapes suivantes pour configurer des référentiels SLES.

**Recherchez les référentiels configurées précédemment (SLES)**

Commencez par vérifier si vous avez déjà enregistré un référentiel SQL Server.

- Utilisez **zypper informations** pour obtenir des informations sur un référentiel préconfiguré.

```
   sudo zypper info mssql-server
```

- Le **référentiel** propriété est le référentiel. Vous pouvez identifier avec la table dans la section [référentiels] de cet article.

**Supprimer l’ancien référentiel (SLES)**

Si nécessaire, supprimez l’ancien espace de stockage. Utilisez une des commandes suivantes en fonction du type de référentiel précédemment configuré.

| Référentiel | Commande pour supprimer |
|---|---|
| **Aperçu** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server'` |



&nbsp;

&nbsp;

---

<a name="TitleNum_2"/>

### <a name="2-nbsp-configure-sql-server-on-linux-with-the-mssql-conf-toolsql-server-linux-configure-mssql-confmd"></a>2. &nbsp; [Configurer SQL Server sur Linux avec l’outil mssql-conf](sql-server-linux-configure-mssql-conf.md)

*Mise à jour : 2018-04-25* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ([précédente](#TitleNum_1) | [suivant](#TitleNum_3))

<!-- Source markdown line 151.  ms.author= jroth.  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 3664c4d64ea4840dcbc718461ed04403cc486f30 89f708af45ce262057e967e9047f1328e19248ba  (PR=5676  ,  Filename=sql-server-linux-configure-mssql-conf.md  ,  Dirpath=docs\linux\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->




**<a id="masterdatabasedir"></a> Modifier l’emplacement de répertoire de fichiers de base de données master par défaut**


Le **filelocation.masterdatafile** et **filelocation.masterlogfile** changements de paramètre de l’emplacement où le moteur SQL Server recherche les fichiers de base de données master. Par défaut, cet emplacement est /var/opt/mssql/data.

Pour modifier ces paramètres, procédez comme suit :

- Créer le répertoire cible pour les nouveaux fichiers journaux des erreurs. L’exemple suivant crée un nouveau **masterdatabasedir/tmp/** active :

```
   sudo mkdir /tmp/masterdatabasedir
```

- Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

```
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
```

- Mssql-conf permet de modifier le répertoire de base de données master par défaut pour les fichiers journaux et de données master avec le **définir** commande :

```
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
```

- Arrêtez le service SQL Server :

```
   sudo systemctl stop mssql-server
```

- Déplacez le master.mdf et masterlog.ldf :

```
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
```

- Démarrez le service SQL Server :

```
   sudo systemctl start mssql-server
```

> [!NOTE]
> Si SQL Server ne peut pas trouver les fichiers master.mdf et mastlog.ldf dans le répertoire spécifié, une copie basée sur un modèle de bases de données système sera automatiquement créée dans le répertoire spécifié, et SQL Server démarre avec succès. Toutefois, les métadonnées, telles que les bases de données utilisateur, les connexions de serveur, des certificats de serveur, les clés de chiffrement, les travaux de l’agent SQL ou ancien mot de passe de connexion SA ne seront pas modifiée dans la nouvelle base de données master. Vous devez arrêter SQL Server et de déplacer votre ancien master.mdf mastlog.ldf vers le nouvel emplacement spécifié et de démarrage de SQL Server pour continuer à utiliser les métadonnées existantes.



&nbsp;

&nbsp;

---

<a name="TitleNum_3"/>

### <a name="3-nbsp-release-notes-for-sql-server-2017-on-linuxsql-server-linux-release-notesmd"></a>3. &nbsp; [Notes de publication pour 2017 du serveur SQL sur Linux](sql-server-linux-release-notes.md)

*Mise à jour : 25/04/2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_2))

<!-- Source markdown line 64.  ms.author= jroth.  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 367d112a9427bbdd18e0e52cc82264dd169c91ae 63a67be08fa39ece778cf9ca0b9746dd28694574  (PR=5676  ,  Filename=sql-server-linux-release-notes.md  ,  Dirpath=docs\linux\  ,  MergeCommitSha40=a85a46312acf8b5a59a8a900310cf088369c4150) -->



- [Activer l’Agent SQL Server]

**<a id="CU6"></a> CU6 (avril 2018)**


Il s’agit de la version 6 de mise à jour Cumulative (CU6) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3025.34. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/help/4101464 ](https://support.microsoft.com/help/4101464).

**Détails du package**


Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3025.34-3 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) |
| Package RPM de SLES | 14.0.3025.34-3 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) |
| Package de Debian Ubuntu 16.04 | 14.0.3025.34-3 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |







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
- [Nouveau + mis à jour (0 0 +) : **Data Quality Services pour SQL** documents](../data-quality-services/new-updated-data-quality-services.md)
- [Nouveau + mis à jour (0 0 +) : **Extensions DMX (Data Mining) pour SQL** documents](../dmx/new-updated-dmx.md)
- [Nouveaux + Mis à jour (0 + 0) : **Master Data Services (MDS) for SQL** (documentation)](../master-data-services/new-updated-master-data-services.md)
- [Nouveau + mis à jour (0 0 +) : **MDX (Multidimensional Expressions) pour SQL** documents](../mdx/new-updated-mdx.md)
- [Nouveau + mis à jour (0 0 +) : **ODBC (Open Database Connectivity) pour SQL** documents](../odbc/new-updated-odbc.md)
- [Nouveau + mis à jour (0 0 +) : **PowerShell pour SQL** documents](../powershell/new-updated-powershell.md)
- [Nouveau + mis à jour (0 0 +) : **exemples pour SQL** documents](../samples/new-updated-samples.md)
- [Nouveau + mis à jour (0 0 +) : **SQL Server Migration Assistant (SSMA)** documents](../ssma/new-updated-ssma.md)
- [Nouveau + mis à jour (0 0 +) : **XQuery pour SQL** documents](../xquery/new-updated-xquery.md)

