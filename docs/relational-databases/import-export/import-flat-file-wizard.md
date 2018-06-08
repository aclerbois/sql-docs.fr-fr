---
title: Importer un fichier plat dans SQL | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: import-export
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.importflatfile.f1
author: yualan
ms.author: alayu
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 00ff0d0eb75ea6ad78135ac85d93494d77b8c581
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-flat-file-to-sql-wizard"></a>Assistant Importation d’un fichier plat dans SQL
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
> Pour tout contenu associé à l’Assistant Importation et exportation, consultez [Assistant Importation et exportation SQL Server](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard).

L’Assistant Importation d’un fichier plat offre un moyen simple de copier des données d’un fichier plat vers une destination. Cette présentation décrit les raisons d’utiliser cet Assistant et où le trouver, puis fournit un exemple simple à suivre.

## <a name="why-would-i-use-this-wizard"></a>Pourquoi utiliser cet Assistant ?
Cet Assistant a été créé pour améliorer l’expérience d’importation actuelle en tirant parti d’un framework intelligent appelé [PROSE](https://microsoft.github.io/prose/) (Program Synthesis using Examples). Pour un utilisateur sans connaissance technique du domaine, l’importation de données peut souvent être une tâche complexe, sujette aux erreurs et fastidieuse. Avec cet Assistant, le processus d’importation se résume simplement à la sélection d’un fichier d’entrée et d’un nom unique de table, le framework PROSE s’occupe du reste.

PROSE analyse les modèles de données dans votre fichier d’entrée pour déduire les noms de colonnes, les types, les séparateurs, etc. Ce framework apprend la structure du fichier et gère tout le travail ingrat à la place des utilisateurs.

Pour mieux comprendre l’amélioration apportée à l’expérience utilisateur de l’Assistant Importation de fichier plat, regardez cette vidéo :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173/player]

## <a name="prerequisites"></a>Conditions préalables requises
Cette fonctionnalité est uniquement disponible dans SQL Server Management Studio (SSMS) version 17.3 ou ultérieure. Veillez à utiliser la version la plus récente. Vous trouverez la dernière version [ici.](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)
 
## <a id="started"></a>Commencer
Pour accéder à l’Assistant Importation d’un fichier plat, procédez comme suit :

1. Ouvrez **SQL Server Management Studio**.
2. Connectez-vous à une instance du Moteur de base de données SQL Server ou à un hôte local.
3. Développez **Bases de données**, cliquez avec le bouton droit sur une base de données (test dans l’exemple ci-dessous), pointez sur **Tâches** et cliquez sur **Importer un fichier plat** au-dessus de « Importer des données ».

![Menu de l’Assistant](media/import-flat-file-wizard/importffmenu.png)

Pour en savoir plus sur les différentes fonctions de l’Assistant, suivez le tutoriel ci-dessous :

## <a name="tutorial"></a>Didacticiel
Pour les besoins de ce didacticiel, n’hésitez pas à utiliser votre propre fichier plat. Sinon, ce didacticiel utilise le fichier CSV Excel suivant que vous êtes libre de copier. Si vous utilisez ce fichier CSV, nommez-le **example.csv** et enregistrez-le au format csv à un emplacement pratique comme votre Bureau.

![Assistant - Excel](media/import-flat-file-wizard/importffexample.png)

### <a name="step-1-access-wizard-and-intro-page"></a>Étape 1 : Accéder à l’Assistant et à la page d’introduction
Accédez à l’Assistant, comme décrit [ici](#started).

La première page de l’Assistant est la page d’accueil. Si vous ne souhaitez plus voir cette page, n’hésitez pas à cliquer sur **Ne plus afficher cette page de démarrage.**

![Assistant - Introduction](media/import-flat-file-wizard/importffintro.png)

### <a name="step-2-specify-input-file"></a>Étape 2 : Spécifier le fichier d’entrée
Cliquez sur Parcourir pour sélectionner votre fichier d’entrée. Par défaut, l’Assistant recherche les fichiers .csv et .txt. 

Le nouveau nom de table doit être unique. S’il ne l’est pas, l’Assistant ne vous autorise pas à aller plus loin.

![Assistant - Spécification](media/import-flat-file-wizard/importffspecify.png)

### <a name="step-3-preview-data"></a>Étape 3 : Prévisualiser les données
L’Assistant génère un aperçu où vous pouvez voir les 50 premières lignes. Si vous rencontrez des problèmes, cliquez sur Annuler, sinon passez à la page suivante.

![Assistant - Aperçu](media/import-flat-file-wizard/importffpreview.png)

### <a name="step-4-modify-columns"></a>Étape 4 : Modifier les colonnes
L’Assistant identifie ce qu’il pense être les bons noms de colonnes, de types de données, etc. Voici où vous pouvez modifier les champs s’ils sont incorrects (par exemple, le type de données doit être une valeur float et non une valeur int).

Quand vous êtes prêt, poursuivez.

![Assistant - Modification](media/import-flat-file-wizard/importffmodify.png)

### <a name="step-5-summary"></a>Étape 5 : Résumé
Il s’agit simplement d’une page qui résume votre configuration actuelle. Si vous voyez des problèmes, vous pouvez revenir à des sections précédentes. Sinon, cliquez sur Terminer pour tenter le processus d’importation.

![Assistant - Résumé](media/import-flat-file-wizard/importffsummary.png)

### <a name="step-6-results"></a>Étape 6 : Résultats
Cette page indique si l’importation a réussi. Si une coche verte s’affiche, elle a réussi. Dans le cas contraire, vous devrez peut-être rechercher des erreurs dans votre configuration ou votre fichier d’entrée.

![Assistant - Résultats](media/import-flat-file-wizard/importffresults.png)

## <a name="learn-more"></a>En savoir plus

Découvrez-en plus sur l’Assistant.
 
- **En savoir plus sur l’importation d’autres sources.** Si vous souhaitez importer plusieurs fichiers plats, consultez [Assistant Importation et exportation SQL Server](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard).
- **En savoir plus sur la connexion à des sources de fichiers plats.** Si vous recherchez plus d’informations sur la connexion à des sources de fichiers plats, consultez [Se connecter à une source de données de fichiers plats](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard).
- **En savoir plus sur PROSE.** Si vous recherchez une vue d’ensemble du framework intelligent utilisé par cet Assistant, consultez [PROSE SDK](https://microsoft.github.io/prose/).

