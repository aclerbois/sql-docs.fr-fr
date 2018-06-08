---
title: Système de fichiers Azure Data Lake Store, tâche | Microsoft Docs
ms.custom: ''
ms.date: 08/22/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSTASK.F1
- SQL14.DTS.DESIGNER.AFPADLSTASK.F1
author: Lingxi-Li
ms.author: lingxl
manager: craigg
ms.openlocfilehash: e1331900994e61eacb66d0cc4efe5e49fc20e508
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="azure-data-lake-store-file-system-task"></a>Système de fichiers Azure Data Lake Store, tâche

La tâche de système de fichiers Azure Data Lake Store permet aux utilisateurs d’effectuer diverses opérations de système de fichiers sur [Azure Data Lake Store (ADLS)](https://azure.microsoft.com/services/data-lake-store/).

La tâche de système de fichiers Azure Data Lake Store est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

## <a name="configure-the-azure-data-lake-store-file-system-task"></a>Configurer la tâche de système de fichiers Azure Data Lake Store

Pour ajouter une tâche de système de fichiers Azure Data Lake Store à un package, faites-la glisser depuis la boîte à outils SSIS vers la zone de conception. Double-cliquez ensuite sur la tâche, ou cliquez avec le bouton droit sur la tâche et sélectionnez **Modifier**, pour ouvrir la boîte de dialogue **Éditeur de tâches du système de fichiers Azure Data Lake Store**.

La propriété **Opération** indique l’opération de système de fichiers à réaliser. Sélectionnez l’une des opérations suivantes :

- **CopyToADLS :** charger des fichiers dans ADLS.
- **CopyFromADLS :** télécharger des fichiers depuis ADLS.

## <a name="configure-the-properties-for-the-operation"></a>Configurer les propriétés de l’opération
Pour toute opération, vous devez spécifier un gestionnaire de connexions Azure Data Lake.

Voici les propriétés spécifiques à chaque opération :

### <a name="copytoadls"></a>CopyToADLS
- **LocalDirectory :** spécifie le répertoire source local qui contient les fichiers à charger.
- **FileNamePattern :** spécifie un filtre de nom de fichier pour les fichiers sources. Seuls les fichiers dont le nom correspond au modèle spécifié sont chargés. Les caractères génériques `*` et `?` sont pris en charge.
- **SearchRecursively :** indique si des fichiers à charger doivent être recherchés de manière récursive dans le répertoire source.
- **AzureDataLakeDirectory :** spécifie le répertoire de destination ADLS dans lequel charger les fichiers.
- **FileExpiry :** spécifie une date et une heure d’expiration pour les fichiers chargés dans ADLS. Laissez cette propriété vide pour indiquer que les fichiers n’expirent jamais.

### <a name="copyfromadls"></a>CopyFromADLS
- **AzureDataLakeDirectory :** spécifie le répertoire source ADLS qui contient les fichiers à télécharger.
- **SearchRecursively :** indique si des fichiers à télécharger doivent être recherchés de manière récursive dans le répertoire source.
- **LocalDirectory :** spécifie le répertoire de destination dans lequel stocker les fichiers téléchargés.
