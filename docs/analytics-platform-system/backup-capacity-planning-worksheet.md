---
title: Planification des capacités de sauvegarde du serveur - Parallel Data Warehouse | Documents Microsoft
description: Cette feuille de calcul de la planification de capacité vous aide à déterminer la configuration requise pour un serveur de sauvegarde pour effectuer la sauvegarde de base de données Parallel Data Warehouse et les opérations de restauration. Utilisez-le pour créer votre plan d’achat nouvelle ou mise en service sauvegarde serveurs existants.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 500bebab375a0d0b94032a1855af3844bc2e6fa7
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="backup-server-capacity-planning-worksheet---parallel-data-warehouse"></a>Feuille de planification capacité sauvegarde du serveur - Parallel Data Warehouse
Cette feuille de calcul de la planification de capacité vous aide à déterminer la configuration requise pour un serveur de sauvegarde pour effectuer la sauvegarde de base de données SQL Server PDW et opérations de restauration. Utilisez-le pour créer votre plan d’achat nouvelle ou mise en service sauvegarde serveurs existants.  
  
Cette feuille de calcul est un complément pour les instructions de [acquérir et de configurer un serveur de sauvegarde](acquire-and-configure-backup-server.md).  
  
## <a name="capacity-planning-worksheet-for-backup-servers"></a>Feuille de planification de capacité pour les serveurs de sauvegarde  

### <a name="notes"></a>Remarques  
  
1.  Cette feuille de calcul s’applique aux serveurs à effectuer les opérations de sauvegarde et de restauration des bases de données PDW.  
  
2.  La seule façon pour sauvegarder et restaurer des bases de données PDW est d’utiliser les commandes de base de données de sauvegarde et restauration de base de données SQL. Toutefois, une fois les données de sauvegarde se trouve sur votre serveur de sauvegarde, il existe en tant qu’ensemble de fichiers Windows. Vous pouvez archiver les fichiers de sauvegarde à partir de votre serveur vers un autre emplacement de stockage à l’aide de méthodes traditionnelles de Windows basée sur le fichier sauvegarde.  
  
### <a name="clipboard-iconmediaclipboard-iconpng-clipboard-icon-capacity-planning-worksheet"></a>![Icône de Presse-papiers](media/clipboard-icon.png "icône Presse-papiers") feuille de calcul de planification de capacité 
  
Imprimer cette feuille de calcul et le remplir avec vos propres exigences.  
  
|Composant|Condition requise|Remplissez cette colonne avec vos propres exigences|Recommandations|  
|-------------|---------------|--------------------------------------------------|-------------------|  
|Stockage|Nombre maximal d’octets que vous envisagez de stocker sur le serveur de sauvegarde sur une période donnée.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Pour déterminer les besoins de stockage, déterminez la quantité de données vous envisagez de stocker sur le serveur de sauvegarde sur une période donnée.<br /><br />Les données de sauvegarde sont stockées sur le serveur de sauvegarde dans un format compressé. Taux de compression de données varient selon les caractéristiques de vos données.<br /><br />Par exemple : comme une estimation approximative, nous vous recommandons d’estimer un taux de compression de 7:1 par rapport à la taille de vos données non compressées. Cela suppose d’au moins 80 % des données sont stocké dans des index columnstore en cluster. Par exemple, si vous avez 700 Go de données non compressées dans une base de données et il est stocké dans des index columnstore en cluster, puis vous pouvez estimer que la sauvegarde de base de données devez environ 100 Go.<br /><br />Si vous prévoyez d’avoir plusieurs copies de sauvegardes de base de données sur le serveur de sauvegarde, vous devez prendre en compte pour eux.<br /><br />Par exemple : Si vous envisagez de sauvegarder 10 bases de données qui contiennent chacun des 5 To de données non compressées, les bases de données ont une taille combinée de 50 To. Si vous envisagez de sauvegarder ces tous les jours de 10 bases de données pendant 5 jours dans une ligne, la taille totale est de 250 To. Factorisation dans un taux de compression de 7:1, vous devez 250 / 7 = 35,7 To de stockage sur votre serveur de sauvegarde. Nous vous recommandons d’être classique et la mise en route sur 30 % de capacité supplémentaire pour prendre en compte les écarts et de croissance.  Dans cet exemple, to 46,6 serait préférable.|  
|Réseau|Type de connexion réseau.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Déterminer le meilleur type de connexion réseau capable de répondre aux exigences de votre taux de charge.<br /><br />Par exemple : InfiniBand ou 10 Gbit offre Ethernet fournit le meilleur du chargement de taux. Ethernet de 1 Gbit limite charges à 360 Go par heure ou moins.|  
|E/S|Octets par heure pour les écritures.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Pour écrire des sauvegardes sur disque, de 4 To par heure vitesses d’écriture sont optimales.<br /><br />Par exemple : pour les lecteurs qui peuvent écrire à 50 Mo par seconde, vous devez au moins 24 disques, ainsi que pour la mise en miroir ou de parité.<br /><br />Pour la capacité d’e/s, prendre en compte toutes les e/s qui se passe sur le serveur lors du chargement. Si le serveur de chargement possède le reste du trafic d’e/s en plus des chargements de données, par exemple recevoir des fichiers de données à partir d’un serveur ETL, les exigences d’e/s augmentera.|  
|Unité centrale|Nombre de sockets.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Recevoir et stocker des fichiers de sauvegarde ne sont pas une application gourmande en ressources processeur.  Au minimum, nous vous recommandons d’à l’aide d’un serveur socket 2 récemment fabriqués.|  
|RAM|Go de mémoire qui permet à Windows en cache les fichiers au cours de charge.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Recevoir et stocker des fichiers de sauvegarde nécessitent très peu de mémoire RAM sur le serveur lors du chargement.<br /><br />Pour déterminer la mémoire RAM requise, reportez-vous à votre installation de Windows Server et des exigences d’application tiers 3e. Nous vous recommandons d’un minimum de 32 Go si vous n’avez pas de configuration requise à partir d’autres sources.|  
  
Lorsque vous avez terminé de détermination de vos besoins en capacité, revenez à la [acquérir et de configurer un serveur de chargement](acquire-and-configure-loading-server.md) rubrique pour planifier votre achat.  
  
## <a name="see-also"></a>Voir aussi  
[Sauvegarde et chargement de matériel](backup-and-loading-hardware.md)  
  
