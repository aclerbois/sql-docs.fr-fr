---
title: Commutateur de fonctionnalité (système de plateforme Analytique)
description: Affiche des informations sur les commutateurs de deux fonctionnalités qui ont été introduites dans AU7 de système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: e384a252584fed0e7543a7a4000fc9a70f6533e7
ms.sourcegitcommit: fc3cd23685c6b9b6972d6a7bab2cc2fc5ebab5f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
#<a name="appliance-feature-switch"></a>Commutateur de fonctionnalité d’équipement
Le **fonctionnalité commutateur** page affiche des informations sur les commutateurs de deux fonctionnalités qui ont été introduites dans AU7 de système de plateforme Analytique. Utilisez cette page pour activer/désactiver les fonctionnalités et les paramètres de système de plateforme d’Analytique ou de mettre à jour. Modification des valeurs de commutateur de fonctionnalité nécessite un redémarrage du service.

![Commutateur de la fonctionnalité Appliance DWConig](media/feature-switch/SQL_Server_PDW_DWConfig_feature_switch.png "DWConig Appliance fonctionnalité commutateur") 

##<a name="autostatsenabled-switch"></a>Commutateur de AutoStatsEnabled
Contrôle la fonctionnalité de statistiques automatique. Cette fonctionnalité de commutateur est définie sur true par défaut après la mise à niveau vers AU7. Toute base de données créée après la mise à niveau hérite de la création automatique et la mise à jour asynchrone des statistiques. Pour les bases de données existantes, les administrateurs de base de données peuvent activer des automatique des statistiques avec [Modifier la base de données] (/ sql/t-sql/statements/alter-database-parallel-data-warehouse). Pour plus d’informations sur les statistiques, consultez [statistiques](../relational-databases/statistics/statistics.md).

##<a name="dmsprocessstopmessagetimeoutinseconds-switch"></a>Commutateur de DmsProcessStopMessageTimeoutInSeconds
Contrôle le délai d’attente de Service de déplacement des données (DMS) à synchroniser sur un système occupé lorsqu’une requête impliquant le déplacement des données est annulée. Mise à jour vers AU7 définit cette valeur à 900 secondes (15 minutes) par défaut. La plage valide est 0 et 3 600 secondes.
