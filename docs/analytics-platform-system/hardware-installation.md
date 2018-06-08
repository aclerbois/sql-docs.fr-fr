---
title: Installation matérielle - système de plateforme Analytique | Documents Microsoft
description: Cet article décrit comment déplacer, décompressez et installez le matériel de votre solution SQL Server PDW. Cet article est d’information uniquement et n’est destiné à vous aider à comprendre le processus. Votre application doit être décompressée, installée et vérifiée avant qu’il soit retourné à vous. La participation de client est requise pour les éléments tels que les données du centre accès, alimentation électrique et les connexions Ethernet.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 169b38a1228f909a79d7866eba20b85b4a56c30b
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="hardware-installation-for-analytics-platform-system-appliance"></a>Installation matérielle pour le matériel de système de plateforme Analytique
Cet article décrit comment déplacer, décompressez et installez le matériel de votre solution SQL Server PDW. Cet article est d’information uniquement et n’est destiné à vous aider à comprendre le processus. Votre application doit être décompressée, installée et vérifiée avant qu’il soit retourné à vous. La participation de client est requise pour les éléments tels que les données du centre accès, alimentation électrique et les connexions Ethernet.  
  
## <a name="BeforeMoving"></a>Avant de déplacer tous les composants de la station de chargement  
Effectuer les tâches suivantes avant de vous déplacez, décompressez ou tous les composants de l’appliance du rack.  
  
|Tâche| Description|  
|--------|---------------|  
|Vérifiez que tous les composants sont arrivés.|Utilisez la nomenclature (nomenclature) pour vérifier que tous les composants sont arrivés et sont leurs palettes à la station de réception pour votre centre de données.|  
|Vérifiez que le centre de données répond à toutes les conditions requises pour l’application|Démarrer cette tâche en examinant les spécifications matérielles et de fournir à votre fabricant de matériel des schémas de câblage. Les étapes suivantes fournissent des détails sur le rack exigences d’espace et de connectivité.|  
|Vérifiez que le centre de données dispose d’espace rack approprié|Vérifiez que le centre de données dispose de suffisamment d’espace pour tous les racks de matériel.<br /><br />Vérifiez que l’espace de rack est vide et prêt à recevoir les racks de matériel.|  
|Vérifiez que le centre de données répond aux exigences de connectivité|Vérifiez que le centre de données respecte les exigences de câblage dans les schémas de câblage.<br /><br />Vérifiez qu’il y aura local pour tous les câbles et cordons d’alimentation une fois les nœuds de l’appliance sont montés en rack.|  
|Vérifiez que le plancher entre la station d’accueil et les racks répondent aux exigences de poids|Vérifiez que tous les sol entre les palettes et les racks peut prendre en charge le poids des nœuds de l’équipement, en particulier dans les centres de données avec des planchers déclenchés.<br /><br />Pour plus d’informations sur le poids de chaque composant, contactez votre fabricant de matériel.|  
|Sécuriser le centre de données en rack|Sécuriser le centre de données rack en place à l’aide d’équipement supplémentaire en fonction des besoins de votre centre de données, tels que des bandes tremblement de terre dans des zones géographiques sujets aux tremblements de terre.|  
|Préparer pour obtenir une assistance avec le transport de composants|Déterminer à l’avance le type d’assistance, les équipements et outils que vous devrez gérer chaque composant et en toute sécurité sans provoquer des dommages.|  
  
## <a name="Moving"></a>Déplacer les Racks à partir de la station d’accueil lors du chargement dans le centre de données  
Chaque palette contient tous les composants du rack une appliance, y compris les nœuds, câbles, câbles, etc.  
  
Utilisez la liste de vérification suivante pour déplacer chaque rack de matériel à partir de la palette de la station d’accueil lors du chargement à son emplacement de rack dans le centre de données. Déplacez d’abord le rack de contrôle, puis déplacez les autres racks de données d’appliance.  
  
> [!WARNING]  
> Pour effectuer ces étapes exactement comme décrit pourrait entraîner blessures, endommager votre appliance SQL Server PDW ou d’autres problèmes.  
>   
> Jamais tentative de courbes d’élévation ou déplacer un nœud d’application ou un autre composant importante sans assistance ou équipement approprié. Contactez votre fabricant de matériel pour plus d’informations sur le poids de chaque composant afin que vous puissiez déterminer à l’avance le type d’assistance, les équipements et outils que vous devrez gérer chaque composant et en toute sécurité sans provoquer des dommages.  
  
|Tâche| Description|  
|--------|---------------|  
|Vérifiez que la palette est|Avant de commencer à déplacer ou à décompresser la palette, veillez à ce qu’il est sur le terrain.|  
|Dégager un nœud à partir de la palette|En commençant par le haut de la palette, dégager le nœud supérieur de la palette.|  
|Déplacer le nœud à un chariot ou le panier prenant en charge le poids|Utilisez rampes et techniques de déplacement/levage appropriées pour déplacer le nœud à un chariot ou le panier prenant en charge le poids.|  
|Le nœud de transport dans le centre de données|Utiliser des techniques de déplacement/levage appropriées pour déplacer le nœud à la position dans le rack de centre de données.|  
|Sécuriser le nœud dans le centre de données en rack|Sécuriser le nœud en place dans le centre de données en rack.|  
|Répétez ces étapes pour le composant ou le nœud suivant|Répétez ces étapes pour déplacer le nœud suivant ou un autre composant matériel dans le centre de données.|  
  
## <a name="AfterMoving"></a>Installer des composants supplémentaires  
Utilisez la liste de vérification suivante pour installer les composants supplémentaires.  
  
|Tâche| Description||  
|--------|---------------|-|  
|Décompressez et commutateurs réseau et les PDU en rack|Utilisez les diagrammes de rack pour placer les commutateurs de réseau et les PDU dans l’emplacement approprié dans le rack.||  
|Connectez les câbles Infiniband et Ethernet selon les étiquettes de câble|Consultez le schéma de câblage. Chaque câble a une étiquette à chaque extrémité qui spécifie où il doit être connecté.||  
|Connectez tous les câbles d’alimentation|Consultez le schéma de câblage.||  
|Activer l’alimentation pour les racks et les PDU|Branchez l’alimentation racks et depuis les racks de l’alimentation. **Ne pas en marche tous les autres composants de matériel pour l’instant.**||  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
  
