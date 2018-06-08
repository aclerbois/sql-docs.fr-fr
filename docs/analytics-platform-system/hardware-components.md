---
title: Composants matériels - système de plateforme Analytique | Documents Microsoft
description: Système de plateforme Analytique (APS) utilise des composants afin que vous pouvez vous procurer le bon volume de stockage et le traitement en fonction des besoins de votre entreprise. Lorsque vous commandez des points d’accès, vous devez une combinaison de ces composants matériels de base.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 9bb7b67a896164fe29611da2091e02c700c46970
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="hardware-components-for-analytics-platform-system"></a>Composants matériels pour un système de plateforme Analytique

Système de plateforme Analytique (APS) utilise des composants afin que vous pouvez vous procurer le bon volume de stockage et le traitement en fonction des besoins de votre entreprise. Lorsque vous commandez des points d’accès, vous devez une combinaison de ces composants matériels de base. Fournisseurs de matériel spécifiques peuvent utiliser différentes conventions d’affectation de noms ou comportent des composants supplémentaires.  
 
  
## <a name="rackandnetwork"></a>Rack et réseau 
 
Composants de points d’accès sont stockés dans un ou plusieurs racks qui entrent dans votre centre de données. Chaque rack est fourni avec deux commutateurs Ethernet, unités de distribution d’alimentation (PDU) et deux commutateurs InfiniBand.  
  
![Rack et réseau](media/rack-and-network.png "APS en rack et réseau")  
  
## <a name="datascaleunit"></a>Unité d’échelle de données
 
Une unité d’échelle de données contient les hôtes de données et le stockage en attachement direct (DAS) pour le traitement et stockage des données utilisateur. Pour augmenter la capacité, vous ajoutez des unités d’échelle de données en fonction des configurations prises en charge par votre fournisseur de matériel. À mesure que le nombre d’unités d’échelle de données augmente, vous devez ajouter le Rack supplémentaire et des composants de réseau, si nécessaire, pour fournir plus de l’alimentation, réseau, en rack infrastructure.  
  
### <a name="data-host"></a>Hôte de données  

Un hôte de données est un serveur dédié au traitement des données de l’utilisateur. Parallel Data Warehouse (PDW) s’exécute à un nœud de calcul sur chaque hôte de données. Pour les équipements de HPE, l’unité d’échelle de données comporte deux hôtes de données. Pour les équipements Dell et Quanta, l’unité d’échelle de données a trois hôtes de données.  
  
### <a name="direct-attached-storage"></a>Stockage en attachement direct
 
Le stockage en attachement direct (DAS) est un pool de disques connectés aux hôtes de données. Tous les hôtes de données peuvent accéder à des disques. Dans le cadre de la sans partage architecture, les nœuds de calcul en cours d’exécution sur les ordinateurs hôtes de données ne partagent pas de disques individuels. Toutefois, pour la haute disponibilité, l’accès au stockage partagé et chacun des hôtes de données peut accéder à des disques.  
  
### <a name="data-scale-unit-architecture---dell-and-quanta"></a>Architecture d’unité données échelle - DELL et Quanta
  
![Unité d’extensibilité](media/scalability-unit-dell.png "unité d’extensibilité de Dell")  
  
### <a name="data-scale-unit-architecture---hpe"></a>Architecture d’unité de montée en puissance données - HPE 
 
![Unité d’extensibilité de HPE](media/scalability-unit-hpe.png "unité d’extensibilité de HPE")  
  
### <a name="data-scale-unit-description"></a>Description de l’unité données montée en puissance

Une unité d’échelle de données possède un serveur (hôte) pour chaque nœud de calcul et d’un tableau d’un disque en attachement direct qui est attaché avec SCSI SAS (Serial Attached). Dans le fichier CAB de stockage, la baie de disques est divisée en deux moitiés ayant chacune des alimentations redondantes. Espaces de stockage de Windows Server gère les données utilisateur par duplication des données entre les paires de disque en miroir RAID 1. Les disques dans chaque paire de disque sont stockés dans différentes parties de la baie de disques.  
  
La baie de disques contient également des disques de secours à chaud et un disque du système. Si un disque tombe en panne, espaces de stockage utilise la copie en bon état des données sur le disque fonctionne pour reconstruire une copie des données sur un échange à chaud. Il s’agit d’une fonctionnalité de réparation automatique importantes qui vous aide à protéger contre la perte de données.  
  
Le nombre total de disques pour les nœuds de calcul :  
  
-   DELL possède des 96 disques = (3 serveurs) * (16 disques par serveur) \* (2 disques redondant).  
  
-   HPE a 64 disques = (2 serveurs) * (16 disques par serveur) \* (2 disques redondant).  
  
-   En outre, chaque baie de disques possède des disques de secours à chaud et un disque du système.  
  
**Pour la haute disponibilité**, lorsqu’un nœud en bascule de calcul peut fonctionnent toujours et accéder à ses données utilisateur via l’autre hôte dans l’unité d’échelle des données. Au moins un des hôtes physiques directs attachés doit fonctionner ou l’accès des données vers le stockage est perdu.  
  
**Pour les tailles de disque**, le stockage en attachement direct peut avoir 1, 2 ou 3 disques téraoctet. Toutes les unités d’échelle de données doivent avoir des disques de la même taille.  
  
## <a name="basescaleunit"></a>Unité d’échelle de base 
 
L’unité d’échelle de Base contient le nombre minimal de puissance cerveau hôtes, des hôtes de données et stockage en attachement direct qui est requis pour l’application. Il inclut ces composants.  
  
### <a name="orchestration-host"></a>hôte d’orchestration  
Ce serveur exécute le cerveau de PDW.
  
### <a name="passive-host"></a>hôte passif  
Ce serveur offre une haute disponibilité. Il est en ligne et prêt à exécuter des tâches en cas de panne de l’orchestration ou un hôte de données. L’hôte d’orchestration, hôte passif et les serveurs d’unité de l’échelle des données sont configurés en tant qu’un cluster de basculement Windows. Chaque rack dans l’application nécessite un hôte de passif.  
  
### <a name="optional-passive-host"></a>hôte passif facultatif  
Pour renforcer la redondance, vous avez la possibilité d’ajouter un second hôte passif à l’unité d’échelle de Base.  
  
### <a name="data-scale-unit"></a>Unité d’échelle de données  
L’unité d’échelle de Base inclut une unité d’échelle de données qui est placée en bas du rack.  
  
Ce diagramme illustre l’unité d’échelle de Base ainsi que le Rack et le réseau. Il s’agit de la configuration minimale requise pour un dispositif de système de plateforme Analytique.  
  
![Unité d’échelle base](media/base-scale-unit.png "unité d’échelle de Base")  
 
