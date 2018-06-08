---
title: Opérations de SQL Studio (version préliminaire) FAQ | Documents Microsoft
description: Forum aux questions (FAQ) pour les opérations de SQL Studio (version préliminaire).
ms.custom: tools|sos
ms.date: 11/15/2017
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2410938dbe57dc5bcb8032cb85daf6fb12bf1a1f
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="includename-sosincludesname-sosmd-faq"></a>[!INCLUDE[name-sos](../includes/name-sos.md)] FAQ

## <a name="what-is-includename-sosincludesname-sos-shortmd"></a>Nouveautés [!INCLUDE[name-sos](../includes/name-sos-short.md)]?

[!INCLUDE[name-sos](../includes/name-sos-short.md)] est un outil de développement et les opérations libre de la base de données léger qui s’exécute sur votre bureau et est disponible pour Windows, Mac OS et Linux. [!INCLUDE[name-sos](../includes/name-sos-short.md)] prend en charge pour la base de données SQL Azure, Azure SQL Data Warehouse et SQL Server exécuté localement ou dans n’importe quel cloud. [!INCLUDE[name-sos](../includes/name-sos-short.md)] offre une expérience cohérente entre les bases de données de votre choix sur vos systèmes d’exploitation favoris.

## <a name="where-can-i-get-includename-sosincludesname-sos-shortmd"></a>Où puis-je obtenir [!INCLUDE[name-sos](../includes/name-sos-short.md)]?

Télécharger [!INCLUDE[name-sos](../includes/name-sos-short.md)] pour Windows, Mac OS et Linux à partir de [http://aka.ms/sqlopsstudio](download.md)

## <a name="how-much-does-includename-sosincludesname-sos-shortmd-cost"></a>Combien [!INCLUDE[name-sos](../includes/name-sos-short.md)] coût ?

[!INCLUDE[name-sos](../includes/name-sos-short.md)] est disponible pour une utilisation privée ou professionnelle.

## <a name="who-should-use-includename-sosincludesname-sos-shortmd"></a>Qui doit utiliser [!INCLUDE[name-sos](../includes/name-sos-short.md)]?

Toute personne peut utiliser [!INCLUDE[name-sos](../includes/name-sos-short.md)]. Toutefois, il est conçu pour simplifier les tâches effectuées par les développeurs de base de données, les administrateurs de base de données, les administrateurs système et aux éditeurs de logiciels.


## <a name="what-can-i-do-with-includename-sosincludesname-sos-shortmd"></a>Que puis-je faire avec [!INCLUDE[name-sos](../includes/name-sos-short.md)]? 

[!INCLUDE[name-sos](../includes/name-sos-short.md)] s’appuie sur Visual Studio Code et offre un léger et expérience de flux de travail de code modernes de focus du clavier lors de l’utilisation avec SQL. 

[!INCLUDE[name-sos](../includes/name-sos-short.md)] rend les fonctionnalités essentielles qui dépendent de tous les jours simple et facile avec les fonctionnalités intégrées telles que plusieurs fenêtres de l’onglet, un éditeur SQL riche, IntelliSense, fin de mot clé, des extraits de code & navigation du code et intégration du contrôle de code source (Git et TFS). Vous pouvez exécuter des requêtes à la demande, Afficher & Enregistrer les résultats en tant que texte, JSON ou Excel, modifier des données, organiser et gérer vos connexions de base de données favoris et parcourir les objets de base de données dans un objet familier, l’expérience de navigation.

Utilisez vos outils de ligne de commande préférés (par exemple, Bash, PowerShell, sqlcmd, bcp, psql et ssh) dans la fenêtre de Terminal Server intégré dans le [!INCLUDE[name-sos](../includes/name-sos-short.md)] interface utilisateur. Générer et exécuter facilement et insérer des scripts pour vos objets de base de données créer des copies de votre base de données à des fins de tests ou de développement. Améliorez votre productivité avec code actives des extraits de code et les riches graphiques expériences qui créent des nouvelles bases de données et les objets de base de données (tels que des tables, vues, procédures stockées, les utilisateurs, connexions, rôles, etc.) ou de mettre à jour des objets de base de données existants. Utilisez bord personnalisable complets pour surveiller et résoudre rapidement les goulots d’étranglement de performances dans vos bases de données locale, dans Azure ou de n’importe quel cloud.

[!INCLUDE[name-sos](../includes/name-sos-short.md)] offre une expérience cohérente pour sauvegarder et restaurer vos bases de données. Planifié la prise en charge pour SQL Server groupes de disponibilité AlwaysOn, vous pouvez facilement configurer, surveiller et résoudre les groupes de disponibilité pour vos bases de données SQL Server critiques et rapidement le basculement vers une base de données secondaire pendant une panne.
[!INCLUDE[name-sos](../includes/name-sos-short.md)] a été conçu pour améliorer votre productivité dans le cycle de vie DevOps vos bases de données de choix sur les systèmes d’exploitation de votre choix. Par conséquent, vous pouvez toujours contrôler, et vous pouvez réduire les risques, résoudre les problèmes plus rapidement et en permanence offrir des performances dépasse les attentes des clients.


## <a name="is-includename-sosincludesname-sos-shortmd-open-source"></a>Est [!INCLUDE[name-sos](../includes/name-sos-short.md)] Open Source ? 

Le code source pour [!INCLUDE[name-sos](../includes/name-sos-short.md)] et ses fournisseurs de données est disponible sur GitHub. Le code source pour le serveur frontal de [!INCLUDE[name-sos](../includes/name-sos-short.md)] (qui est basé sur Visual Studio Code) est disponible sous un contrat qui fournit des droits pour modifier et utiliser le logiciel, mais pas de redistribuer ou de l’héberger dans un service cloud du code source. Le code source pour les fournisseurs de données est disponible sous la licence du MIT [ https://github.com/Microsoft/sqltoolsservice ](https://github.com/Microsoft/sqltoolsservice).

## <a name="do-you-plan-to-open-source-ssms"></a>Vous souhaitez ouvrir la source de SSMS

Non. Toutefois, la génération suivante multi-OS CLI et l’interface graphique utilisateur des outils sont open source. Par exemple, l’extension mssql pour VS Code mssql-Générateur de script et msql-CLI sont tous les open source sur GitHub. Le code source pour [!INCLUDE[name-sos](../includes/name-sos-short.md)] est disponible sur GitHub.


## <a name="now-that-there-is-includename-sosincludesname-sos-shortmd-does-microsoft-plan-to-deprecate-ssms-and-ssdt"></a>Maintenant qu’il y ait [!INCLUDE[name-sos](../includes/name-sos-short.md)], Microsoft prévoit-elle de déconseiller SSMS et SSDT ?

Non. En plus de la génération suivante de multi-OS et outils CLI et l’interface graphique utilisateur de multi-DB continue investissements dans les outils de Windows phare (SSMS, SSDT, PowerShell).
L’objectif est de proposer aux clients le choix de l’aide des outils qu’ils souhaitent sur les plateformes de leur choix pour les scénarios.


## <a name="includename-sosincludesname-sos-shortmd-is-missing-a-feature-that-is-in-ssmsssdt-will-you-add-it"></a>[!INCLUDE[name-sos](../includes/name-sos-short.md)] Il manque une fonctionnalité qui se trouve dans SSMS/SSDT. Vous ajouterez il ?
Il dépend de la nécessité de scénario et de client / d’entreprise. Pour vous aider à hiérarchiser, des fichiers une suggestion sur [GitHub](https://github.com/microsoft/sqlopsstudio/issues).


## <a name="i-understand-includename-sosincludesname-sos-shortmd-and-the-mssql-extension-for-vs-code-are-powered-by-a-new-tools-service-that-uses-smo-apis-under-the-covers-is-smo-available-on-linux-and-macos"></a>Je comprends [!INCLUDE[name-sos](../includes/name-sos-short.md)] et l’extension mssql pour le Code de Visual Studio sont alimentées par un nouveau *outils service* qui utilise l’API de SMO en arrière-plan. SMO n’est disponible sur Linux et macOS ?

L’API SMO ne soient disponibles sur Linux ou macOS d’une manière utilisable. Nous avons porté sur un sous-ensemble de l’API SMO vers le .NET Core qui nous nécessaires pour [!INCLUDE[name-sos](../includes/name-sos-short.md)], et nous prévoyons dans le cadre de la feuille de route.
Le Service des outils SQL se trouve sur GitHub : [ https://github.com/Microsoft/sqltoolsservice ](https://github.com/Microsoft/sqltoolsservice).


## <a name="do-you-plan-to-port-the-dacfx-apis-andor-sqlpackageexe-andor-ssdt-to-linux-and-macos"></a>Vous souhaitez la DACFx APIs et/ou de sqlpackage.exe et/ou de SSDT pour Linux et macOS de port

Il se trouve sur la feuille de route à plus long terme. Pour vous aider à hiérarchiser, des fichiers une suggestion sur [GitHub](https://github.com/microsoft/sqlopsstudio/issues).


## <a name="will-sql-powershell-cmdlets-be-available-on-linux-and-macos"></a>Applets de commande PowerShell de SQL sera disponible sur Linux et macOS ?

SQL PowerShell est disponible aujourd'hui dans la galerie PowerShell et vous pouvez l’utiliser sous Windows pour fonctionner avec SQL Server est en cours d’exécution n’importe où, y compris SQL sur Linux. Applets de commande sur Linux & macOS offre le SQL PowerShell est dans la feuille de route. Pour vous aider à hiérarchiser, des fichiers une suggestion sur [GitHub](https://github.com/microsoft/sqlopsstudio/issues).

