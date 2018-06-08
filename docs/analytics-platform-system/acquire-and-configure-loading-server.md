---
title: Obtenir et configurer un serveur de chargement - Parallel Data Warehouse | Documents Microsoft
description: Cet article décrit comment acquérir et configurez un serveur de chargement comme un système de Windows non appliance pour l’envoi des chargements de données à Parallel Data Warehouse (PDW).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: a796616ad76ba62ea4174cf22c1517c489305055
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="acquire-and-configure-a-loading-server-for-parallel-data-warehouse"></a>Obtenir et configurer un serveur de chargement pour Parallel Data Warehouse
Cet article décrit comment acquérir et configurez un serveur de chargement comme un système de Windows non appliance pour l’envoi des chargements de données à Parallel Data Warehouse (PDW).  
  
## <a name="Basics"></a>Principes de base  
Le chargement du serveur :  
  
-   Pas nécessairement être un serveur unique. Vous pouvez charger en même temps que plusieurs serveurs de chargement.  
  
-   Est fournie et géré par votre équipe informatique. Vous disposez déjà d’un serveur ou les serveurs qui peuvent être utilisés pour le chargement des données dans PDW.  
  
-   Se trouve dans un rack de matériel non et ne peut pas être placé dans le matériel de système de plateforme Analytique.  
  
-   Est connecté à l’application via le réseau InfiniBand de matériel ou sur Ethernet. Pour des performances, nous vous recommandons d’utiliser InfiniBand.  
  
-   Se trouve dans votre propre domaine client, pas le domaine d’application. Il n’existe aucune relation d’approbation entre votre client et le domaine d’application.  
  
## <a name="Step1"></a>Étape 1 : Déterminer la capacité requise  
Le système de chargement peut être conçu comme un ou plusieurs serveurs de chargement qui effectuent des chargements simultanés. Chaque serveur de chargement ne dispose pas d’être dédié uniquement le chargement, tant qu’il gère les exigences de stockage et les performances de votre charge de travail.  
  
La configuration requise pour un serveur de chargement dépend presque entièrement votre propre charge de travail. Utilisez le [le chargement des feuille de planification de capacité de serveur](loading-server-capacity-planning-worksheet.md) pour aider à déterminer vos besoins en capacité.  
  
## <a name="Step2"></a>Étape 2 : Obtenir le Sserveur  
Maintenant que vous comprenez mieux vos besoins en capacité, vous pouvez planifier les serveurs et les composants réseau dont vous aurez besoin pour l’achat ou de configurer. Incorporer la liste suivante des exigences de votre plan d’achat, puis achetez votre serveur ou configurer un serveur existant.  
  
### <a name="R"></a>Configuration logicielle requise  
Systèmes d’exploitation pris en charge :  
  
-   Windows Server 2012 ou Windows Server 2012 R2. Ces systèmes d’exploitation nécessitent la carte réseau de Verification.  
  
-   Windows Server 2008 R2. Ce système d’exploitation requiert la carte réseau DDR.  
  
Le serveur doit utiliser les paramètres régionaux EN-US pour pouvoir utiliser l’outil de ligne de commande du chargement de dwloader. dwloader ne prend pas en charge les autres paramètres régionaux.  
  
### <a name="networking-requirements-for-windows-server-2012-and-windows-server-2012-r2"></a>Configuration réseau requise pour Windows Server 2012 et Windows Server 2012 R2  
Mais non obligatoire pour le chargement, InfiniBand est le type de connexion recommandé pour le chargement des serveurs. Pour de meilleures performances, utilisez Windows Server 2012 ou Windows Server 2012 R2 et la carte réseau InfiniBand de Verification pour connecter le chargement du serveur au réseau InfiniBand appliance.  
  
Pour préparer une connexion de Windows Server 2012 ou Windows Server 2012 R2 InfiniBand :  
  
1.  Envisagez le serveur en rack suffisamment proches pour l’application afin que vous pouvez vous connecter à l’appliance InfiniBand bascule. Pour plus d’informations à partir de Mellanox Technologies InfiniBand, voir le livre blanc, [présentation InfiniBand](http://www.mellanox.com/pdf/whitepapers/IB_Intro_WP_190.pdf).  
  
2.  Acheter une carte simple ou double port Mellanox ConnectX-3 Verification InfiniBand. Nous vous recommandons d’achat de la carte réseau avec deux ports pour la tolérance de panne pendant la transmission de données. Une carte réseau de deux ports est requise pour la haute disponibilité.  
  
3.  Acheter 2 câbles Verification InfiniBand pour une carte de deux ports ou 1 câble Verification InfiniBand pour une carte de port unique. Les câbles Verification InfiniBand seront connecte le chargement du serveur au réseau InfiniBand appliance. La longueur du câble dépend de la distance entre le serveur de chargement et les commutateurs InfiniBand du matériel, selon votre environnement.  
  
## <a name="Step3"></a>Étape 3 : Relier le serveur aux réseaux InfiniBand  
Utilisez ces étapes pour connecter le chargement du serveur au réseau InfiniBand. Si le serveur n’utilise pas le réseau InfiniBand, ignorez cette étape.  
  
1.  Rack serveur suffisamment proches pour l’application afin que vous pouvez vous connecter au réseau InfiniBand appliance.  
  
2.  Installer la carte réseau InfiniBand Mellanox ConnectX-3 Verification InfiniBand sur le serveur de chargement.  
  
3.  Utilisez les câbles de Verification pour se connecter la carte réseau InfiniBand à un des deux commutateurs InfiniBand dans le premier rack de matériel.  
  
4.  Installez et configurez le pilote Windows approprié pour la carte réseau InfiniBand.  
  
    -   Pilotes InfiniBand pour Windows sont développées par Alliance OpenFabrics, un consortium industriel InfiniBand fournisseurs.  Le pilote peut ont été distribué avec votre carte de réseau InfiniBand. Si ce n’est pas le cas, le pilote peut être téléchargé à partir de www.openfabrics.org.  
  
5.  Configurez les paramètres DNS et InfiniBand pour les cartes réseau. Pour obtenir des instructions de configuration, consultez [cartes réseau InfiniBand de configurer](configure-infiniband-network-adapters.md).  
  
## <a name="Step4"></a>Étape 4 : Installer les outils de chargement  
Les outils clients sont disponibles en téléchargement à partir du Microsoft Download Center. 

Pour installer dwloader, exécutez l’installation de dwloader dans les outils clients.
  
Si vous envisagez d’utiliser les Services d’intégration pour le chargement, vous devrez installer Integration Services et les adaptateurs de destination Integration Services. Les adaptateurs sont disponibles dans les outils clients.

<!-- To install the des[Install Integration Services Destination Adapters](install-integration-services-destination-adapters.md). 
--> 
  
## <a name="Step5"></a>Étape 5 : Démarrer le chargement  
Vous êtes maintenant prêt à commencer le chargement des données. Pour plus d'informations, consultez :  
  
1.  [Outil de ligne de commande du chargement de dwloader](dwloader.md)  
  
2.  [Vue d’ensemble de la charge](load-overview.md)  
  
## <a name="performance"></a>Performance  
Pour optimiser les performances sur Windows Server 2012 et au-delà de chargement, activer l’initialisation instantanée de fichiers afin que lorsque les données sont remplacées, le système d’exploitation n’écrase pas les données existantes par des zéros. S’il s’agit d’un risque de sécurité, car il existe encore des données précédentes sur les disques, veillez à désactiver l’initialisation instantanée des fichiers.  
  
## <a name="Security"></a>Avis de sécurité  
Étant donné que les données à charger ne sont pas stockées sur l’appareil, votre équipe informatique est chargé de gérer tous les aspects de la sécurité de vos données à charger. Par exemple, cela inclut la gestion de la sécurité des données à charger, la sécurité du serveur utilisé pour stocker les charges et la sécurité de l’infrastructure réseau qui connecte le serveur de chargement à l’appliance SQL Server PDW.  
  
> [!IMPORTANT]  
> Il est particulièrement important de sécuriser chaque serveur de chargement qui utilise l’outil de ligne de commande du chargement de dwloader. Lorsque dwloader charge des données, il authentifie tout d’abord avec le nœud de contrôle et puis après une authentification réussie il déplace les données à partir du serveur de chargement directement aux nœuds de calcul sur les canaux de données. Validation du certificat ne se produire pas pendant les tremblements entre chaque serveur de chargement et de chaque nœud de calcul. Cela laisse les nœuds de calcul exposés à des attaques potentielles man in the middle sur chaque canal de données lors du chargement. Ces attaques peut entraîner des données falsifiées et/ou la divulgation d’informations.  
  
Pour réduire les risques de sécurité à vos données, il est conseillé des éléments suivants :  
  
-   Désignez un compte Windows qui est utilisé uniquement dans le but de chargement de données dans PDW. Autoriser ce compte disposent des autorisations à l’emplacement de chargement et nulle part ailleurs.  
  
-   Désigner un utilisateur PDW qui dispose des autorisations pour charger des données. Selon vos besoins de sécurité, vous pourriez avoir un utilisateur spécifique par base de données.  
  
-   Opérations sur le serveur de chargement peuvent accepter un chemin d’accès UNC à partir de laquelle pour extraire des données à partir de l’extérieur du réseau interne approuvé. Et une personne malveillante sur le réseau ou avec la possibilité pour influencer la résolution de noms peut intercepter ou modifier les données envoyées à l’ordinateur SQL Server PDW. Cela pose un risque de divulgation de falsification et informations. Falsification doit être atténuée à exiger la signature sur la connexion. Pour atténuer ce risque, définissez l’option de stratégie de groupe suivante **de sécurité\Stratégies Locales\options** sur le serveur lors du chargement : **client réseau Microsoft : communications signées numériquement (toujours) : Activé**  
  
-   Désactivez l’initialisation instantanée des fichiers sur Windows Server 2012 et au-delà. Il s’agit d’un compromis entre les performances et la sécurité, comme indiqué dans la section performances. Vous devez décider ce qui convient le mieux en fonction de vos exigences de sécurité.  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble de sauvegarde et de restauration](backup-and-restore-overview.md)  
  
