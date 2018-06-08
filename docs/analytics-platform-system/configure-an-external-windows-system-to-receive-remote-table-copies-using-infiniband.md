---
title: Configurer Windows pour recevoir une copie de la table distante - Parallel Data Warehouse | Documents Microsoft
description: Décrit comment acheter et configurer un système de Windows sur le matériel non connecté à l’aide du réseau InfiniBand pour une utilisation avec la fonctionnalité de copie de table distante dans Parallel Data Warehouse. Le système Windows destiné à héberger la base de données SQL Server qui reçoit la copie de la table distante à partir d’une base de données SQL Server PDW. Il est achetée séparément à partir de l’application et connecté au réseau InfiniBand appliance.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: ed7122f497b0bdebd893eec75606bbb6382e9a73
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-an-external-windows-system-to-receive-remote-table-copies-using-infiniband---parallel-data-warehouse"></a>Configurer un système Windows externe pour recevoir une copie de table distante à l’aide de InfiniBand - Parallel Data Warehouse
Décrit comment acheter et configurer un système de Windows sur le matériel non connecté à l’aide du réseau InfiniBand pour une utilisation avec la fonctionnalité de copie de table distante dans SQL Server PDW. Le système Windows destiné à héberger la base de données SQL Server qui reçoit la copie de la table distante à partir d’une base de données SQL Server PDW. Il est achetée séparément à partir de l’application et connecté au réseau InfiniBand appliance.  
  
> [!NOTE]  
> Connexion via le réseau InfiniBand n’est pas requise pour utiliser la copie de la table distante. Connexion via le réseau Ethernet peut être effectuée si la bande passante Ethernet répond à vos besoins.  
  
Cette rubrique décrit l’une des étapes de configuration pour la configuration de copie de la table distante. Pour obtenir la liste de toutes les étapes de configuration, consultez [copie de la Table distante](remote-table-copy.md)  
  
## <a name="before-you-begin"></a>Avant de commencer  
Avant de configurer le système Windows externe, vous devez :  
  
1.  Achat ou fournir un système Windows qui reçoit la copie à distance.  
  
2.  Le système Windows dans le rack de contrôle de rack (s’il y a suffisamment d’espace) ou suffisamment proches pour l’application afin que vous pouvez vous connecter au réseau InfiniBand appliance.  
  
3.  À partir de votre fournisseur de matériel appliance d’achat InfiniBand câbles et un adaptateur de réseau InfiniBand. Nous vous recommandons d’acheter une carte réseau avec deux ports pour la tolérance de panne lors de la réception des données exportées. Une carte réseau de deux ports est recommandée, mais n’est pas obligatoire.  
  
## <a name="HowToWindows"></a>Configuration d’un système Windows externe pour recevoir une copie de la Table distante  
Pour configurer le système Windows externe, procédez comme suit :  
  
1.  Installez la carte réseau InfiniBand dans votre système Windows.  
  
2.  Connectez la carte réseau InfiniBand au commutateur InfiniBand principal dans le rack de contrôle à l’aide de câbles de InfiniBand.  
  
3.  Installez et configurez le pilote Windows approprié pour la carte réseau InfiniBand.  
  
    Pilotes InfiniBand pour Windows sont développées par Alliance OpenFabrics, un consortium industriel InfiniBand fournisseurs.  Le pilote peut ont été distribué avec la carte réseau InfiniBand. Si ce n’est pas le cas, le pilote peut être téléchargé à partir de www.openfabrics.org.  
  
4.  Configurez l’adresse IP pour chaque port sur la carte. Systèmes SMP sont requis pour utiliser des adresses IP statiques à partir d’une plage d’adresses réservées à cet effet. Configurez le premier port en fonction des paramètres suivants :  
  
    -   Adresse réseau IP : 172.16.132.x  
  
    -   Masque de sous-réseau IP : 255.255.128.0  
  
    -   Plage d’hôte IP : 1 et 254.  
  
    Pour les cartes réseau InfiniBand avec deux ports, configurez le deuxième port en fonction des paramètres suivants :  
  
    -   Adresse réseau IP : 172.16.132.x  
  
    -   Masque de sous-réseau IP : 255.255.128.0  
  
    -   Plage d’hôte IP : 1 et 254.  
  
5.  Si une carte à deux ports est utilisée, ou plusieurs systèmes Windows externes sont connectés à un appareil, vous pouvez affecter chaque système un numéro de hôte différent dans chaque sous-réseau IP.  
  
<!-- MISSING LINKS 
## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
  
