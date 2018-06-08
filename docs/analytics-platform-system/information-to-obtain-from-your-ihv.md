---
title: Obtenir des informations à partir du fabricant de matériel - système de plateforme Analytique | Documents Microsoft
description: Informations à obtenir à partir de votre fabricant de matériel concernant le matériel de système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 57b61ed7741bc6d36b7531a62416893e7cc10fb7
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="information-to-obtain-from-your-ihv"></a>Pour plus d’informations à obtenir à partir de votre fabricant de matériel
Lorsque votre fabricant de matériel (matériel) remet votre nouveau matériel SQL Server PDW pour vous, ils seront également fournir des informations sur le matériel et la configuration qu’ils ont effectuées sur votre appliance. Vous devez ces informations pour administrer votre appliance.  
  
La liste suivante montre les informations qui ne sont généralement nécessaire de votre fabricant de matériel. Dans certains cas, des informations supplémentaires ou d’autres sont nécessaire. Vérifiez auprès de votre fabricant de matériel pour vous assurer que toutes les informations pertinentes a été transférées à la livraison de l’appliance.  
  
|||  
|-|-|  
|**Plus d’informations ou de Document**|**Description**|  
|Nomenclature (BOM)|Votre nomenclature répertorie les composants qui sont inclus dans votre solution. Ces informations sont nécessaires pour confirmer que tous les composants ont été remis.<br /><br />**Important :** votre nomenclature doit inclure des poids pour chaque nœud de la solution et pour chaque rack complète. Cette information est importante lorsque vous planifiez comment gérer et déplacer les composants d’application et de s’assurer que votre centre de données peut prendre en charge l’application. Si votre marque BOM n’inclut pas les pondérations des nœuds, assurez-vous d’obtenir ces informations à partir de votre fabricant de matériel pour tous les nœuds.|  
|Schémas de câblage|Diagrammes de câblage montrent comment se connecter au réseau, câbles d’alimentation et autres pour chaque appareil en rack. Ces diagrammes sont nécessaires lors de l’installation de l’application dans votre centre de données, et chaque fois que vous devez supprimer ou remplacer un composant.|  
|Besoins d’un rack|Avant que votre application peut être installée dans votre centre de données, vous devez savoir si votre centre de données répond à la ventilation et des exigences de longueur de câble pour l’application, ainsi que la taille et consommation pour les composants. Voir aussi nomenclature (nomenclature) ci-dessus pour plus d’informations sur le poids des composants matériel, ce qui est également nécessaire.|  
  
