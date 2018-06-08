---
title: Capacité de traitement et de stockage - système de plateforme d’Analytique | Documents Microsoft
description: Vos exigences professionnelles de déterminer le nombre d’unités d’échelle de données et la taille des disques de nœud de calcul dont vous avez besoin dans votre solution de système de plateforme Analytique (APS).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: f552372ac108d219ad410b88ec9911ecaea63ab3
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="processing-and-storage-capacity-in-analytics-platform-system"></a>Capacité de traitement et de stockage dans le système de plateforme Analytique
Vos exigences professionnelles de déterminer le nombre d’unités d’échelle de données et la taille des disques de nœud de calcul dont vous avez besoin dans votre solution de système de plateforme Analytique (APS). Utilisez ces calculs de traitement et de stockage pour guider la capacité de votre achat et les décisions de planification.  
  
  
## <a name="section1"></a>Planification de la capacité de traitement  
Performances des requêtes pour SQL Server Parallel Data Warehouse (PDW) dépendent fortement du nombre de cœurs de processeurs travaillant sur vos données en parallèle. Dans les limites, l’augmentation du parallélisme améliore les performances de requête de traitement parallèle massif (MPP). Même si la taille de vos données est relativement faible, la puissance du moteur de requête MPP est améliorée en ayant de parallélisme supérieur.  
  
Par exemple, un matériel avec les nœuds de calcul 12 a 192 cœurs de processeur qui traitent de vos données en parallèle. C’est le degré de parallélisme 192 ! Un matériel avec les nœuds de calcul 56 a 896 cœurs fonctionnent tous en parallèle. Cet ordre de grandeur de parallélisme n’est pas réalisable sans MPP informatique.  
  
À mesure que le nombre de nœuds de calcul augmente, montée en puissance parallèle de l’application nécessite que l’ajout de plusieurs nœuds de calcul à la fois pour obtenir un avantage notable. Les fournisseurs de matériel prend en charge des configurations spécifiques uniquement des unités d’échelle de données pour vous assurer que l’avantage de la mise à l’échelle de l’application compense le coût de redistribuer les données entre plusieurs nœuds de calcul.  
  
### <a name="data-scale-unit-configuration-examples---hpe"></a>Exemples de configuration de l’unité d’échelle de données - HPE  
Voici des exemples des configurations prises en charge HPE pour Uunits de mise à l’échelle de données. Ils peuvent être différentes des plus récente configurations prises en charge, mais sont fournis sous la forme d’un exemple montrant comment augmenter la capacité en environ 20 pour cent.  
  
Extension est le pourcentage de capacité de gain en augmentant la Uunits de mise à l’échelle de données à partir d’une ligne à la suivante. Par exemple, augmenter les unités d’échelle de données à partir de 6 à 8 donne une augmentation de 33 % des cœurs de processeur et de la mémoire.  Cela augmente l’espace disque qui n’est pas affichée dans cette table.  
  
|Unités d’échelle de données|Nœuds de calcul|Cœurs de processeur|Mémoire (Go)|Extension|  
|--------------------|-----------------|-------------|-----------------|----------|  
|1|2|32|512|-|  
|2|4|64|1024|100%|  
|3|6|96|1536|50|  
|4|8|128|2048|33%|  
|5|10|160|2560|25%|  
|6|12|192|3072|20%|  
|8|16|256|4096|33%|  
|10|20|320|5120|25%|  
|12|24|384|6144|20%|  
|16|32|512|8192|33%|  
|20|40|640|10240|25%|  
|24|48|768|12288|20%|  
|28|56|896|14336|17%|  
  
Explication :  
  
-   **Unités d’échelle de données** par appliance. Pour en savoir plus sur les unités d’échelle de données, consultez [Analytique plateforme composants matériels](hardware-components.md).  
  
-   **Nœuds de calcul** par appliance.  
  
-   **Cœurs de processeur** par appliance. Il y a 16 noyaux par nœud de calcul noyau par chaque paire de disque en miroir. Pour la structure de disque du nœud de calcul, consultez [Analytique plateforme composants matériels](hardware-components.md).  
  
-   **Mémoire** par appliance. Chaque cœur dispose de 256 Go de mémoire.  
  
### <a name="data-scale-unit-configuration-examples--dell-quanta"></a>Échelle unité configuration exemples de données – Dell, Quanta  
Voici des exemples des configurations Dell et Quanta pris en charge pour Uunits de mise à l’échelle de données. Ils peuvent être différentes des plus récente configurations prises en charge, mais sont fournis sous la forme d’un exemple montrant comment augmenter la capacité en environ 20 pour cent.  
  
Extension est le pourcentage de capacité de gain en augmentant la Uunits de mise à l’échelle de données à partir d’une ligne à la suivante. Par exemple, augmenter les unités d’échelle de données à partir de 6 à 8 donne une augmentation de 33 % des cœurs de processeur et de la mémoire. Cela augmente l’espace disque qui n’est pas affichée dans cette table.  
  
|Unités d’échelle de données|Nœuds de calcul|Cœurs de processeur|Mémoire (Go)|Extension|  
|--------------------|-----------------|-------------|-----------------|----------|  
|1|3|48|768|-|  
|2|6|96|1536|100%|  
|3|9|144|2,304|50|  
|4|12|192|3,072|33%|  
|5|15|240|3,840|25%|  
|6|18|288|4,608|20%|  
|7|21|336|5,376|17%|  
|8|24|384|6,144|14%|  
|9|27|432|6,912|13%|  
|12|36|576|9,216|33%|  
|15|45|720|11,520|25%|  
|18|54|864|13,824|20%|  
  
## <a name="section2"></a>Planification de la capacité de stockage  
Cette table estime que vous pouvez charger et stocker jusqu'à 6 plusieurs pétaoctets de données non compressées sur un dispositif de système de plateforme Analytique entièrement intégrée. 
  
|Fournisseur|Taille du lecteur|Nœud de par calcul de stockage physique des données|Nœuds de calcul maximales par rack|Stockage physique des données maximal par rack|Estimation du stockage de données maximal d’utilisateurs par rack|Racks maximales|Estimation de stockage de données maximal d’utilisateurs par appliance|  
|----------|--------------|------------------------------------------|----------------------------------|------------------------------------------|------------------------------------------------|-----------------|-----------------------------------------------------|  
|HPE|1 TO|JUSQU'À 16 TO|8|128 TO|320 TO|7|2,240 TO|  
|HPE|2 TO|32 TO|8|256 TO|640 TO|7|4,480 TO|  
|HPE|3 TO|48 TO|8|384 TO|960 TO|7|6,720 TO|  
|DELL|1 TO|JUSQU'À 16 TO|9|144 TO|360 TO|6|2,160 TO|  
|DELL|2 TO|32 TO|9|288 TO|720 TO|6|4 320 TO|  
|DELL|3 TO|48 TO|9|432 TO|1080 TO|6|6,480 TO|  
  
Explication :  
  
-   **Taille du lecteur** est 1, 2 ou 3 To pour chaque fournisseur de matériel.  
  
-   **Stockage physique des données par le nœud de calcul** = (taille du lecteur) * (16 disques par nœud de calcul). Les disques en miroir ne sont pas inclus, car elles sont pour assurer la redondance.  
  
-   **Les nœuds de calcul maximal par rack** est spécifique au fournisseur de matériel.  
  
-   **Stockage physique des données maximal par rack** = (stockage de données physiques par nœud de calcul) * (nombre maximal de nœuds de calcul par rack).  
  
-   **Estimée de stockage de données maximal d’utilisateurs par rack** = (stockage physique des données maximal par rack) * (5 pour un taux de compression 5:1) \* (50 % pour les journaux et tempDB). Il s’agit d’une estimation classique pour les données utilisateur non compressé qui peuvent être chargées et stockées sur l’appareil. Ceci est une estimation et n’est pas appliquée par le logiciel. Le stockage des données utilisateur réelle dépend de vos données et de votre configuration.  
  
-   **Racks maximales** est spécifique à chaque fournisseur de matériel.  
  
-   **Estimation du stockage de données maximal par appliance** = (stockage de données maximal estimé par rack) * (racks Maximum). Il s’agit de la taille de total général des données utilisateur que vous pouvez charger et stocker sur un matériel totalement intégré pessimiste.  
  
