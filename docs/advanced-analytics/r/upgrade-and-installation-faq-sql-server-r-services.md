---
title: FAQ d’installation et de mise à niveau pour l’apprentissage de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 52ff43f13ff3f17f21ed96313d3134d8ee31dd86
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="upgrade-and-installation-faq-for-sql-server-machine-learning-or-r-server"></a>FAQ d’installation et de mise à niveau pour l’apprentissage de SQL Server ou R Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette rubrique fournit des réponses aux questions courantes sur l’installation de l’apprentissage des fonctionnalités de SQL Server. Elle traite également des questions courantes sur les mises à niveau.

+ Certains problèmes se produisent uniquement avec les mises à niveau à partir de versions préliminaires. Par conséquent, nous vous recommandons d’identifier votre version et l’édition tout d’abord avant de lire ces notes. Pour obtenir des informations de version, exécutez `@@VERSION` dans une requête à partir de SQL Server Management Studio.
+ Mettre à niveau vers la version de service dès que possible, ou d’une version la plus récente pour résoudre les problèmes qui ont été résolus dans les versions récentes.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage Services (de-de base de données)

## <a name="requirements-and-restrictions-on-older-versions-of-sql-server-2016"></a>Exigences et restrictions sur les versions antérieures de SQL Server 2016 

La version de SQL Server que vous installez, certaines des limitations suivantes peuvent s’appliquer :

- Dans les versions antérieures de SQL Server 2016 R Services, modèle 8.3 notation était nécessaire sur le lecteur qui contient le répertoire de travail. Si vous avez installé une version préliminaire, la mise à niveau vers SQL Server 2016 Service Pack 1 doit résoudre ce problème. Cette exigence ne s’applique pas aux versions après le SP1.

- Installation côte à côte avec une autre version de R, ou avec d’autres versions de Revolution Analytique, n’est pas pris en charge.

- Désactiver l’analyse antivirus, avant de commencer l’installation. Une fois le programme d’installation est terminée, nous vous recommandons d’interruption analyse antivirus sur les dossiers utilisés par [!INCLUDE[ssnoversion](../../includes/ssnoversion.md)]. De préférence, suspendre l’analyse sur l’ensemble du [!INCLUDE[ssnoversion](../../includes/ssnoversion.md)] arborescence.

 - L’installation de Microsoft R Server sur une instance de SQL Server est installé sur Windows Core. Dans la version RTM de SQL Server 2016, il a été un problème connu lors de l’ajout de Microsoft R Server pour une instance sur l’édition de Windows Server Core. Ce problème a été résolu. Si vous rencontrez ce problème, vous pouvez appliquer le correctif décrit dans [KB3164398](https://support.microsoft.com/kb/3164398) pour ajouter la fonctionnalité de R à l’instance existante sur Windows Server Core. Pour plus d’informations, consultez [Impossible d’installation Microsoft R Server (autonome) sur un système d’exploitation Windows Server Core](https://support.microsoft.com/kb/3168691).


## <a name="offline-installation-of-machine-learning-components-for-a-localized-version-of-sql-server-2016"></a>Installation hors connexion des composants d’apprentissage machine pour une version localisée de SQL Server 2016

Échec de la libération anticipée des versions de SQL Server 2016 installer les fichiers .cab de paramètres régionaux spécifiques lors de l’installation en mode hors connexion sans connexion internet. Ce problème a été résolu dans les versions ultérieures, mais si le programme d’installation renvoie un message indiquant qu’il ne peut pas installer la langue appropriée, vous pouvez modifier le nom de fichier pour le programme d’installation continuer.

+ Modifier manuellement le fichier de programme d’installation pour vous assurer que la bonne langue est installée. Par exemple, pour installer la version japonaise de SQL Server, vous modifierait le nom du fichier à partir de SRS_8.0.3.0_**1033**.cab à SRS_8.0.3.0_**1041**.cab.
+ L’identificateur de langue utilisé pour les composants d’apprentissage automatique doit être identique à la langue d’installation de SQL Server, ou vous ne pouvez pas terminer l’installation.

## <a name="pre-release-versions-support-policies-upgrade-and-known-issues"></a>Versions préliminaires : prend en charge les stratégies de mise à niveau et problèmes connus

Les nouvelles installations de n’importe quelle version de la version préliminaire de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] n’est plus pris en charge. Si vous utilisez une version préliminaire, mettez à niveau dès que possible.

Cette section contient des instructions détaillées pour les scénarios de mise à niveau spécifiques.

### <a name="how-to-upgrade-sql-server"></a>La mise à niveau de SQL Server

Vous pouvez mettre à niveau votre version de SQL Server en réexécutant l’Assistant installation.

+ [Mise à niveau vers SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)
+ [Mise à niveau SQL Server à l’aide de l’Assistant Installation](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

Vous pouvez mettre à niveau uniquement d’apprentissage des composants à l’aide d’un processus appelé liaison : 
+ [Utilisez SqlBindR pour mettre à niveau les composants de la machine learning](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md)

### <a name="end-of-support-for-in-place-upgrades-from-prerelease-versions"></a>Fin de la prise en charge des mises à niveau sur place à partir de versions préliminaires

Mises à niveau à partir de versions préliminaires de SQL Server 2016 ne sont plus prises en charge. Cela inclut SQL Server 2016 CTP3, CTP3.1, CTP3.2, RC0 ou RC1.

Les versions suivantes ont été installées avec les versions préliminaires de SQL Server 2016.

| Version | Build         |
|---------|---------------|
| CTP 3.0 | 13.0.xxx      |
| CTP 3.1 | 13.0.801.12   |
| CTP 3.2 | 13.0.900.73   |
| CTP 3.3 | 13.0.1000.281 |
| RC1     | 13.0.1200.242 |
| RC2     | 13.0.1300.275 |
| RC3     | 13.0.1400.361 |

Si vous avez un doute sur la version que vous utilisez, exécutez `@@VERSION` dans une requête à partir de SQL Server Management Studio.

En règle générale, le processus de mise à niveau est la suivante :

1. Sauvegardez les scripts et données.
2. Désinstallez la version préliminaire.
3. Installez une version release.

Désinstallation d’une version préliminaire de SQL Server learning les composants d’ordinateur peuvent être complexes et peut nécessiter l’exécution d’un script spécial. Contactez le support technique pour obtenir de l'aide.

###  <a name="bkmk_Uninstall"></a> Désinstallez avant la mise à niveau à partir d’une version antérieure de Microsoft R Server

Si vous avez installé une version préliminaire de Microsoft R Server, commencez par la désinstaller avant de pouvoir mettre à niveau vers une version plus récente.

1.  Dans le **Panneau de configuration**, cliquez sur **Ajout/Suppression de programmes**, et sélectionnez `Microsoft SQL Server 2016 <version number>`.

2.  Dans la boîte de dialogue proposant les options **Ajouter**, **Réparer**ou **Supprimer** les composants, sélectionnez **Supprimer**.
  
3.  Sur la page **Sélectionner les fonctionnalités** sous **Composants partagés**, sélectionnez **R Server (autonome)**. Cliquez sur **Suivant**, puis sur **Terminer** pour désinstaller les composants sélectionnés uniquement.

## <a name="r-services-and-r-server-standalone-side-by-side-errors"></a>R Services et les erreurs de côte-à-côte R Server (autonome) 

Dans les versions antérieures de SQL Server 2016, l’installation de R Server (autonome) et R Services (de-de base de données) en même temps parfois a provoqué le programme d’installation échoue avec un message « accès refusé ». Ce problème a été résolu dans le Service Pack 1 pour SQL Server 2016.

Si vous a rencontré cette erreur et que vous devez mettre à niveau ces fonctionnalités, effectuez une installation intégrée de SQL Server 2016 avec SP1. Il existe deux façons de résoudre le problème, qui nécessitent le désinstaller et réinstaller.

1. Désinstaller les Services de R (de-de base de données) et assurez-vous que les comptes d’utilisateurs SQLRUserGroup sont supprimés.

2. Redémarrez le serveur, puis réinstallez R Server (autonome).

3. Exécuter SQL Server le programme d’installation une fois plus d’informations, et cette fois, sélectionnez **ajouter des fonctionnalités à SQL Server existant**.

4. Choisissez l’instance, puis sélectionnez le **R Services (de-de base de données)** option à ajouter.

Si cette procédure ne parvient pas à résoudre le problème, essayez de la solution de contournement suivante :

1. Désinstallez R Services (de-de base de données) et R Server (autonome) en même temps.

2. Supprimer les comptes d’utilisateur local (SQLRUserGroup).

3. Redémarrez le serveur.

4. Exécutez le programme d’installation de SQL Server et ajouter la fonctionnalité de R Services (de-de base de données) uniquement. Ne sélectionnez pas **R Server (autonome)**.

En règle générale, nous vous recommandons de ne pas installer R Services (de-de base de données) et R Server (autonome) sur le même ordinateur. Toutefois, en supposant que le serveur possède une capacité suffisante, vous pouvez trouver que r serveur autonome peut être utile en tant qu’un outil de développement. Un autre scénario possible est que vous devez utiliser les fonctionnalités à l’Opérationnalisation de R Server, mais que vous souhaitez également accéder aux données de SQL Server sans le déplacement des données.

## <a name="incompatible-version-of-r-client-and-r-server"></a>Version incompatible de R Client et de R Server

Si vous installez Microsoft R Client et l’utiliser pour exécuter R dans un contexte de calcul de SQL Server à distance, vous pouvez obtenir ce type d’erreur :

*Version9.0.0 du client de Microsoft R sont en cours d’exécution sur votre ordinateur, ce qui est incompatible avec la version 8.0.3 Microsoft R Server. Téléchargez et installez une version compatible.*

Dans SQL Server 2016, il était nécessaire que la version de R en cours d’exécution dans SQL Server R Services être exactement le même que les bibliothèques dans le Client Microsoft R. Cette exigence a été supprimée dans les versions ultérieures. Toutefois, nous vous recommandons de vous toujours obtenez les dernières versions des composants d’apprentissage et d’installer tous les service packs. 

Si vous avez une version antérieure de Microsoft R Server et que vous avez besoin assurer la compatibilité avec le Client Microsoft R 9.0.0, installer les mises à jour qui sont décrites dans cette [article du support technique](https://support.microsoft.com/kb/3210262).


## <a name="installation-fails-with-error-only-one-revolution-enterprise-product-can-be-installed-at-a-time"></a>L’installation échoue avec l’erreur : « Seul un produit Revolution Enterprise peut être installé à la fois. »

Vous pouvez rencontrer cette erreur si votre installation de produits Revolution Analytics est ancienne ou si vous disposez d’une version préliminaire de SQL Server R Services. Vous devez désinstaller toute version précédente avant de pouvoir installer une version plus récente de Microsoft R Server. L’installation côte à côte avec d’autres versions des outils Revolution Enterprise n’est pas prise en charge.

Cependant, les installations côte à côte sont prises en charge quand R Server (autonome) est utilisé avec [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] ou SQL Server 2016.

## <a name="registry-cleanup-to-uninstall-older-components"></a>Nettoyage du Registre pour désinstaller les composants plus anciens

Si vous rencontrez des problèmes de suppression d’une version antérieure, vous devrez peut-être modifier le Registre pour supprimer les clés associées.

> [!IMPORTANT]
> Ce problème s’applique uniquement si vous avez installé une version préliminaire de Microsoft R Server ou une version CTP de SQL Server 2016 R Services.
  
1. Ouvrez le Registre Windows et recherchez cette clé : `HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall`.
2. Supprimez toutes les entrées suivantes, le cas échéant, et si la clé contient uniquement la valeur `sEstimatedSize2`:
  
    -   E0B2C29E-B8FC-490B-A043-2CAE75634972        (pour 8.0.2)
  
    -   46695879-954E-4072-9D32-1CC84D4158F4        (pour 8.0.1)
  
    -   2DF16DF8-A2DB-4EC6-808B-CB5A302DA91B        (pour 8.0.0)
  
    -   5A2A1571-B8CD-4AAF-9303-8DF463DABE5A        (pour 7.5.0)

## <a name="see-also"></a>Voir aussi

 [Ordinateur SQL Server Learning Services (de-de base de données)](../r/sql-server-r-services.md)

 [Ordinateur SQL Server apprentissage Server (autonome)](../r/r-server-standalone.md)
