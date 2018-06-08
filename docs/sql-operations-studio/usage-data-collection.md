---
title: Activer ou désactiver la collecte des données d’utilisation et bloquer la création de rapports pour les opérations de SQL Studio (version préliminaire) | Documents Microsoft
description: Cet article explique comment contrôler si les données de rapport d’incident et l’utilisation sont collectées et envoyées à Microsoft.
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
ms.openlocfilehash: 12022216dddfbcc2c54904340cfc7a13a8d4f799
ms.sourcegitcommit: b3bb41424249de198f22d9c6d40df4996f083aa6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/18/2018
---
# <a name="enable-or-disable-usage-data-collection-for-includename-sosincludesname-sos-shortmd"></a>Activer ou désactiver la collecte des données d’utilisation pour [!INCLUDE[name-sos](../includes/name-sos-short.md)]

## <a name="how-to-disable-telemetry-reporting"></a>Comment désactiver le rapport de télémétrie

[!INCLUDE[name-sos](../includes/name-sos-short.md)] collecte des données d’utilisation et l’envoie à Microsoft pour aider à améliorer nos produits et services. Pour plus d’informations, consultez la [déclaration de confidentialité de](https://go.microsoft.com/fwlink/?LinkID=528096&clcid=0x409).

Si vous ne souhaitez pas envoyer les données d’utilisation à Microsoft, vous pouvez définir le *telemetry.enableTelemetry* à *false*.

Pour exclure tous les événements de télémétrie de [!INCLUDE[name-sos](../includes/name-sos-short.md)], à partir de **fichier** > **préférences** > **paramètres**, ajouter l’option suivante :

```json
    "telemetry.enableTelemetry": false
```

**Avis important**: cette option nécessite un redémarrage de [!INCLUDE[name-sos](../includes/name-sos-short.md)] entrent en vigueur. 

## <a name="how-to-disable-crash-reporting"></a>Comment désactiver le rapport d’incident

Pour désactiver le rapport d’incident, à partir de **fichier** > **préférences** > **paramètres**, ajoutez l’option suivante :

```json
    "telemetry.enableCrashReporter": false
```

**Avis important**: cette option nécessite un redémarrage de [!INCLUDE[name-sos](../includes/name-sos-short.md)] entrent en vigueur.

## <a name="additional-resources"></a>Ressources supplémentaires
- [Espace de travail et les paramètres utilisateur](settings.md)