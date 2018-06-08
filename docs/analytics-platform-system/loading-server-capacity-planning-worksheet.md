---
title: Le chargement de la capacité du serveur planification - système de plateforme Analytique | Documents Microsoft
description: Cette feuille de calcul de la planification de capacité vous aide à déterminer la configuration requise pour un serveur de chargement pour charger les données à Analytique plateforme système Parallel Data Warehouse. »
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 2f0efd7e0688496d5af7887431ca00ae683c874f
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="loading-server-capacity-planning-worksheet-for-analytics-platform-system"></a>Le chargement de la feuille de planification de capacité de serveur pour le système de plateforme Analytique
Cette feuille de calcul de la planification de capacité vous aide à déterminer la configuration requise pour un serveur de chargement pour charger des données dans SQL Server PDW. Cela permet de créer votre plan pour l’achat ou de configuration existant du chargement des serveurs.  
  
## <a name="worksheet-notes"></a>Notes de la feuille de calcul
  
1.  Cette feuille de calcul s’applique aux serveurs que le chargement des données avec le **dwloader** outil de chargement de ligne de commande.  
  
2.  Pour charger les données avec Integration Services ou un outil de chargement de tiers, la configuration requise peut varier selon les différences dans le processus de chargement.  
  
3.  La plupart des exigences s’appliquent au chargement des fichiers de données compressé ou non compressé ; les différences de configuration requise sont indiquées en gras.  
  
## <a name="clipboardmediaclipboard-iconpng-clipboard-capacity-planning-worksheet"></a>![Presse-papiers](media/clipboard-icon.png "Presse-papiers") feuille de calcul Planification des capacités  
  
Imprimer cette feuille de calcul et le remplir avec vos propres exigences.  
  
|Composant|Condition requise|Remplissez cette colonne avec vos propres exigences|Recommandations|  
|-------------|---------------|--------------------------------------------------|-------------------|  
|Stockage|Nombre maximal d’octets que vous envisagez de stocker sur le serveur lors du chargement à une période donnée.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Pour déterminer les besoins de stockage, déterminez la quantité de données vous envisagez de stocker sur le serveur lors du chargement à une période donnée.  La capacité requise est pour charger les fichiers uniquement. le système d’exploitation et les fichiers de charge doivent être sur baies de disques différents.<br /><br />Par exemple : Si vous envisagez de charger de 100 Go de données à partir du disque 3 fois par jour, mais pas supprimer les fichiers de données jusqu'à la fin de la semaine, vous avez besoin d’un minimum 2,1 To pour stocker les fichiers de données. Nous vous recommandons d’être classique et la mise en route sur stockage de plus de 30 % pour prendre en compte les écarts et de croissance.  Pour cet exemple, vous préférerez certainement de 2,73 to d’espace de stockage.|  
|Taux de charge|Nombre maximal d’octets par heure de données à charger dans PDW.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Il s’agit d’une estimation. Lors du calcul de cette exigence, supposent que les fichiers sont déjà sur le serveur lors du chargement, et que les autres conditions de charge font aussi bonne qualité que possibles.<br /><br />Par exemple : inutile à factoriser compressibilité des données depuis dwloader envoie toujours les données non compressées à l’empaquetage. Aucun besoin de tenir compte des conversions de types de données et la taille de la table de destination.|  
|Réseau|Type de connexion réseau.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Déterminer le meilleur type de connexion réseau pour vos exigences de taux de charge.<br /><br />Par exemple : InfiniBand ou 10 GB Ethernet fournira les taux de chargement optimal. 1 Gb Ethernet limite charges à 360 Go par heure ou moins.|  
|E/S|Octets par heure pour les lectures et écritures.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Pour charger des données, dwloader doit lire toutes les données à partir du disque avant de l’envoyer à PDW.<br /><br />Chaque serveur de chargement ne peut pas charger les données plus rapidement que l’application peut recevoir des données à partir de toutes les sources de chargement. Pour économiser de l’argent, planifiez les e/s de lecture capacité pour charger afin qu’il ne dépasse pas la capacité de charge de l’application.<br /><br />Par exemple :<br />PDW reçoit et charge les données dans un dispositif de rack de 1 à un taux maximum de 1,8 To par heure. Pour une application avec un rayon de 2 ou plus, le taux de charge maximale est 3,6 To par heure.<br /><br />Si vous envisagez de charger à partir de plusieurs serveurs de chargement en même temps, les exigences d’e/s pour chaque serveur de chargement sera inférieure à un seul serveur effectue lorsque tout le chargement.<br /><br />Par exemple : un serveur de chargement peut charger un maximum de 1,8 To par heure pour un dispositif de rack de 1. Deux serveurs de chargement impossible chacun simultanément charger 900 Go par heure dans un dispositif de rack de 1. Les niveaux supérieurs de l’accès concurrentiel peuvent réduire l’efficacité et le débit maximal.<br /><br />Pour la capacité d’e/s, prendre en compte toutes les e/s qui se passe sur le serveur lors du chargement. Si le serveur de chargement possède le reste du trafic d’e/s en plus des chargements de données, par exemple recevoir des fichiers de données à partir d’un serveur ETL, les exigences d’e/s augmentera.<br /><br />Pour les données compressées, les exigences d’e/s dépendent de la vitesse de la compression de données. dwloader lit les données compressées et il décompresse ensuite avant de l’envoyer à PDW. Plus le taux de compression, les moins de données le chargement du serveur devez lire à partir du disque.<br /><br />Par exemple : si le taux de charge requis est 1,8 To par heure et les données sont stockées sur le serveur de chargement avec compression 2:1, le chargement du serveur ne doit lire 900 Go par heure à partir du disque au lieu de 1,8 To. Un taux de compression 3:1 signifie que le chargement du serveur doit lire 600 Go par heure à partir du disque.|  
|Unité centrale|Nombre de sockets.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|Pour le chargement des données non compressées, dwloader n’est pas une application gourmande en ressources processeur.  Au minimum, nous vous recommandons d’à l’aide d’un serveur socket 2 récemment fabriqués.<br /><br />Pour le chargement des données compressées, vous devez alimenter l’UC pour décompresser les données avant de l’envoyer à PDW. dwloader exécuter 10 threads actifs simultanément. Si vous envisagez de charger simultanément les 10 fichiers compressés, nous vous recommandons du que serveur a au moins un processeur cœur 10 ou deux processeurs 6 cœurs.|  
|RAM|Go de mémoire qui permet à Windows en cache les fichiers au cours de charge.|![Icône de crayon](media/pencil-icon.png "icône de crayon")|dwloader utilise très peu de mémoire RAM sur le serveur lors du chargement. Pour des performances, Windows utilise la mémoire pour mettre en cache les fichiers de chargement après les avoir lues à partir du disque.<br /><br />Pour déterminer la mémoire RAM requise, reportez-vous à votre installation de Windows Server et des exigences d’application tiers 3e. Nous vous recommandons d’un minimum de 32 Go si vous n’avez pas de configuration requise à partir d’autres sources.<br /><br />Pour les données compressées, RAM plus rapide est utile, car elle permet d’accélérer la décompression.|  
  
## <a name="see-also"></a>Voir aussi  
[Obtenir et configurer un serveur de chargement](acquire-and-configure-loading-server.md)  
[Chargeur de ligne de commande de dwloader](dwloader.md)  
  
