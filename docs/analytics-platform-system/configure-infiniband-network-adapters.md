---
title: Configurer InfiniBand - système de plateforme Analytique | Documents Microsoft
description: Décrit comment configurer des cartes réseau InfiniBand sur un serveur autre que l’appliance client pour se connecter au nœud de contrôle sur Parallel Data Warehouse (PDW). Utilisez ces instructions pour la connectivité de base et pour la haute disponibilité, afin que le chargement, le processus de sauvegarde et d’autres se connectent automatiquement au réseau InfiniBand actif.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 8e67d63e7bb4bded0bd19e5db4a0b7faddb80977
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-infiniband-network-adapters-for-analytics-platform-system"></a>Configurer des cartes réseau InfiniBand pour système de plateforme Analytique
Décrit comment configurer des cartes réseau InfiniBand sur un serveur autre que l’appliance client pour se connecter au nœud de contrôle sur Parallel Data Warehouse (PDW). Utilisez ces instructions pour la connectivité de base et pour la haute disponibilité, afin que le chargement, le processus de sauvegarde et d’autres se connectent automatiquement au réseau InfiniBand actif.  
  
## <a name="Basics"></a>Description  
Ces instructions vous indiquent comment rechercher, puis définissez l’adresse IP correcte InfiniBand adresses et des masques de sous-réseau sur votre serveur connecté InfiniBand. Elles expliquent également comment configurer votre serveur pour utiliser l’application de points d’accès DNS afin que votre connexion correspond au réseau InfiniBand actif.  
  
Pour la haute disponibilité, points d’accès possède deux réseaux InfiniBand, un actif et un passif. Chaque réseau InfiniBand a une adresse IP différente pour le nœud de contrôle. Si le réseau InfiniBand actif tombe en panne, le réseau InfiniBand passif devient le réseau actif. Dans ce cas un script ou un processus se connecte automatiquement au réseau InfiniBand actif sans modifier les paramètres de script.  
  
Plus précisément, dans cet article vous :  
  
1.  Trouver les adresses IP de InfiniBand de APS DNS des serveurs (appliance_domain-AD01 et appliance_domain *-AD02). Pour ce faire, vous ouvrez une session en tant que les serveurs AD01 et AD02 et obtenez les adresses IP pour chaque réseau InfiniBand. Les adresses IP de InfiniBand sur le nœud Active Directory sont les adresses IP DNS.  
  
2.  Configurez chaque carte réseau pour utiliser une adresse IP disponible sur les réseaux InfiniBand de points d’accès.  
  
    1.  Si vous avez deux cartes réseau de InfiniBand, vous configurez une carte avec une adresse IP disponible dans le réseau InfiniBand première qui est appelé TeamIB1 et l’autre avec une adresse IP disponible dans le deuxième réseau InfiniBand appelé TeamIB2. Utiliser le TeamIB1 appliance_domain-AD01 une adresse IP en tant que le serveur DNS préféré et appliance_domain-AD02 TeamIB1 adresse IP que le serveur DNS auxiliaire pour la carte réseau de TeamIB1. Utiliser le TeamIB2 appliance_domain-AD01 une adresse IP en tant que le serveur DNS préféré et appliance_domain-AD02 TeamIB2 adresse IP que le serveur DNS auxiliaire pour la carte réseau de TeamIB2.  
  
    2.  Si vous n'avez qu’une carte de réseau InfiniBand, vous configurez la carte avec une adresse IP disponible à partir d’un des réseaux InfiniBand. Puis vous configurez les Favoris et les serveurs DNS secondaires sur cette carte, en utilisant soit appliance_domain-AD01 TeamIB1 et appliance_domain-AD02 TeamIB1 ou appliance_domain-AD01 TeamIB2 et appliance_domain-AD02 TeamIB2 selon ce qui se trouve sur le même réseau respectivement en tant que l’adaptateur configuré en tant que les Favoris et les serveurs DNS secondaires.  
  
3.  Configurez votre carte de réseau InfiniBand pour utiliser des serveurs DNS de APS pour résoudre votre connexion au réseau InfiniBand actif.  
  
    1.  Cela vous permet de configurer les paramètres TCP/IP avancés pour ajouter le suffixe DNS du domaine appliance au début de la liste des suffixes DNS sur votre serveur client. Cette opération ne doit être configurée sur une des cartes réseau ; le paramètre s’applique aux deux cartes.  
  
Après avoir configuré les cartes réseau InfiniBand, processus client peuvent se connecter au nœud de contrôle sur le réseau InfiniBand, à l’aide de `PDW_region-SQLCTL01` pour l’adresse du serveur. Votre serveur ajoute le suffixe DNS de système de plateforme Analytique, ou vous pouvez entrer l’adresse complète qui est `PDW_region-SQLCTL01.appliance_domain.pdw.local`.  
  
Par exemple, si votre nom de la région PDW est MyPDW et le nom du matériel est MyAPS, la spécification du serveur dwloader pour le chargement des données est une des opérations suivantes :  
  
-   `dwloader –S MYPDW-SQLCTL01.MyAPS.pdw.local`  
  
-   `dwloader –S MYPDW-SQLCTL01`  
  
## <a name="BeforeBegin"></a>Avant de commencer  
  
### <a name="requirements"></a>Spécifications  
Vous avez besoin d’un compte de domaine du matériel APS pour vous connecter au nœud AD01. Par exemple, F12345 * \Administrator.  
  
Vous avez besoin d’un compte Windows sur le serveur client qui a l’autorisation de configurer les cartes réseau.  
  
### <a name="prerequisites"></a>Configuration requise  
Ces instructions supposent que le serveur client est déjà mis en rack et connecté au réseau InfiniBand appliance. Pour le montage en rack et le câblage des instructions, consultez [acquérir et de configurer un serveur de chargement](acquire-and-configure-loading-server.md).  
  
### <a name="general-remarks"></a>Remarques d'ordre général  
À l’aide de SQLCTL01, le système de plateforme Analytique DNS se connecte votre serveur client au nœud de contrôle en utilisant le réseau InfiniBand actif. Cela s’applique uniquement à la connexion ; Si le réseau InfiniBand tombe en panne lors d’une sauvegarde ou de charge, vous devez redémarrer le processus.  
  
Pour répondre aux besoins de votre entreprise, vous pouvez également rejoindre le serveur client pour votre propre groupe de travail autre que l’appliance ou d’un domaine Windows.  
  
## <a name="Sec1"></a>Étape 1 : Obtenir l’application des paramètres de réseau InfiniBand  
*Pour obtenir l’application des paramètres de réseau InfiniBand*  
  
1.  Connectez-vous à l’appliance AD01 nœud en utilisant le compte appliance_domain\Administrator.  
  
2.  Sur le nœud de AD01 appliance, ouvrez le panneau, sélectionnez le réseau et Internet, sélectionnez réseau et partage Center *, puis sélectionnez Modifier les paramètres de carte.  
  
3.  Dans la fenêtre Connexions réseau, avec le bouton droit sur IB1 d’équipe, puis sélectionnez Propriétés.  
  
    ![Connexions InfiniBand sur le nœud de gestion](media/network-teamib.png "connexions InfiniBand sur le nœud de gestion")  
  
4.  Dans la fenêtre Propriétés Internet Protocol Version 4 (TCP/IPv4), notez les valeurs pour le **adresse IP** et **masque de sous-réseau**.  L’adresse IP de la ***appliance_domain *-AD01** nœud est l’adresse IP du serveur DNS de système de plateforme Analytique.  
  
5.  Répétez les étapes 1 à 5 ci-dessus pour la carte TeamIB1 sur ***appliance_domain *-AD02** server.  
  
    ![Propriétés du nœud InfiniBand 1 PDW gestion](media/network-ip1-properties.png "propriétés d’un nœud InfiniBand 1 administration de PDW")  
  
6.  Cliquez sur Annuler pour fermer la fenêtre.  
  
7.  Rechercher une adresse IP inutilisée sur le réseau TeamIB1 et écrivez.  
  
    Pour rechercher une adresse IP inutilisée, ouvrez une fenêtre de commande et effectuez un test ping des adresses IP dans la plage d’adresses de votre application. Dans cet exemple, l’adresse IP du réseau TeamIB1 est 172.16.14.30. Rechercher une adresse IP qui commence par 172.16.14 n’est pas utilisé. Par exemple, à partir de la ligne de commande entrez « ping 172.16.14.254 ». Si la requête ping échoue, l’adresse IP est disponible.  
  
8.  Faire la même chose de TeamIB2. Dans la * fenêtre Connexions réseau, cliquez sur l’équipe IB2 et sélectionnez Propriétés.  
  
9. Dans la fenêtre Propriétés Internet Protocol Version 4 (TCP/IPv4), notez les valeurs de l’adresse IP et le masque de sous-réseau de TeamIB2.  
  
10. Répétez les étapes 8 à 9 ci-dessus pour la carte TeamIB2 appliance_domain-AD02 serveur.  
  
    ![Propriétés de TeamIB2](media/network-ip2-properties.png "propriétés de TeamIB2")  
  
11. Rechercher une adresse IP inutilisée sur le **TeamIB2** réseau et de les écrire vers le bas.  
  
    Pour rechercher une adresse IP inutilisée, ouvrez une fenêtre de commande et effectuez un test ping des adresses IP dans la plage d’adresses de votre application. Dans cet exemple, l’adresse IP du réseau de TeamIB2 est 172.16.18.30. Rechercher une adresse IP qui commence par 172.16.18 n’est pas utilisé. Par exemple, à partir de la ligne de commande entrez « ping 172.16.18.254 ». Si la requête ping échoue, l’adresse IP est disponible.  
  
## <a name="Sec2"></a>Étape 2 : Configurer les paramètres de carte réseau InfiniBand sur votre serveur Client  

### <a name="notes"></a>Remarques  
  
-   Ces étapes vous montrent comment inscrire votre serveur avec les serveurs DNS de APS.  
  
-   Pour répondre aux besoins de votre propre réseau, vous pouvez également joindre le serveur client votre groupe de travail autre que l’appliance ou d’un domaine Windows.  
  
-   Les instructions pas à pas dans la configuration de deux cartes réseau sur chaque serveur.  Si vous n’avez qu’une seule carte réseau, choisissez une des réseaux à configurer sur la carte réseau, puis ajoutez la deuxième adresse IP DNS comme serveur DNS auxiliaire.  
  
### <a name="to-configure-the-infiniband-network-adapter-settings-on-your-client-server"></a>Pour configurer les paramètres de carte réseau InfiniBand sur votre serveur client  
  
1.  Connectez-vous en tant qu’administrateur Windows pour votre chargement, la sauvegarde ou autre serveur du client sur le matériel réseau InfiniBand.  
  
2.  Ouvrez le volet contrôle *, sélectionnez Centre réseau et partage, puis modifier les paramètres de carte.  
  
### <a name="to-configure-the-first-network-adapter"></a>Pour configurer la première carte réseau  
  
1.  Dans la fenêtre Connexions réseau, avec le bouton droit sur l’un des emplacements réseau non identifié pour l’adaptateur Mellanox, puis sélectionnez Propriétés.  
  
    ![Sélectionnez les réseaux InfiniBand](media/network-connections.png "sélectionner les réseaux InfiniBand")  
  
2.  Dans la fenêtre Propriétés  
  
    1.  Sous l’onglet Général, définissez l’adresse IP à l’adresse IP que vous vérifié comme étant libre dans le test ping pour TeamIB1. Pour les valeurs de l’exemple utilisés dans cet article, vous devez entrer 172.16.14.254.  
  
    2.  Définir le masque de sous-réseau pour le masque de sous-réseau que vous avez notées pour TeamIB1.  
  
    3.  Définir le serveur DNS préféré à l’adresse IP de TeamIB1 dont vous avez noté précédemment à partir d’appliance_domain *-AD01 nœud.  
  
    4.  Définir le serveur DNS auxiliaire à l’adresse IP de TeamIB1 dont vous avez noté précédemment à partir d’appliance_domain *-AD02 nœud.  
  
        ![Propriétés de la carte réseau 1 InfiniBand](media/network-ib1-properties.png "SQL_Server_PDW_Network_IB1_properties")  
  
    5.  Cliquez sur OK pour appliquer les modifications.  
  
### <a name="to-configure-the-second-network-adapter"></a>Pour configurer la seconde carte réseau  
  
1.  Ignorez cette section si vous disposez d’une carte réseau.  
  
2.  Dans la fenêtre Connexions réseau, avec le bouton droit sur le deuxième emplacement réseau non identifié pour l’adaptateur Mellanox, puis sélectionnez Propriétés.  
  
    ![Sélectionnez les réseaux InfiniBand](media/network-connections.png "sélectionner les réseaux InfiniBand")  
  
3.  Dans la fenêtre Propriétés  
  
    1.  Sous l’onglet Général, définissez l’adresse IP à l’adresse IP que vous vérifié comme étant libre dans le test ping de TeamIB2. Pour les valeurs de l’exemple utilisés dans cet article, vous devez entrer 172.16.18.254.  
  
    2.  Définir le masque de sous-réseau pour le masque de sous-réseau que vous avez notées de TeamIB2.  
  
    3.  Définir le serveur DNS préféré à l’adresse IP de TeamIB2 dont vous avez noté précédemment à partir d’appliance_domain *-AD01 nœud.  
  
    4.  Définir le serveur DNS auxiliaire à l’adresse IP de TeamIB2 dont vous avez noté précédemment à partir d’appliance_domain *-AD02 nœud.  
  
        > [!NOTE]  
        > Si vous avez une carte réseau, configurez les Favoris et les serveurs DNS secondaires à l’aide de l’appliance AD01 TeamIB1 et un dispositif AD02 TeamIB1 comme les Favoris et les serveurs DNS secondaires respectivement ou utiliser l’appliance AD01 TeamIB2 et équipement AD02 TeamIB2 comme les Favoris et les serveurs DNS secondaires selon que l’ordinateur Active Directory a basculé.  
  
        ![Propriétés de la carte réseau 1 InfiniBand](media/network-ib1-properties.png "propriétés de la carte réseau 1 InfiniBand")  
  
    5.  Cliquez sur OK pour appliquer les modifications.  
  
### <a name="to-configure-the-dns-suffix"></a>Pour configurer le suffixe DNS  
  
1.  Dans la fenêtre Connexions réseau, avec le bouton droit sur l’un des emplacements réseau pour la carte Mellanox, puis sélectionnez Propriétés.  
  
2.  Cliquez sur avancées... .  
  
3.  Dans la fenêtre Paramètres TCP/IP avancés, la si ajouter ces Suffixes DNS (dans l’ordre) option n'est pas grisée, vérification de la zone appelée ajouter ces suffixes DNS (dans l’ordre) :, sélectionnez le suffixe de domaine d’application et cliquez sur Ajouter... Le suffixe de domaine d’application est `appliance_domain.local`  
  
4.  Si l’opération d’ajout ces suffixes DNS (dans l’ordre) : option est grisée, vous pouvez ajouter le domaine de points d’accès à ce serveur en modifiant la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\DNSClient.  
  
    ![Les paramètres TCP/IP](media/network-tcpip.png "les paramètres TCP/IP")  
  
5.  Pour la résolution d’adresse plus rapide, nous vous recommandons de déplacer le suffixe de l’application vers le haut de la liste.  
  
6.  Cliquez sur OK.  
  
7.  Maintenant, vous pouvez vous connecter au réseau Infiniband appliance à l’aide de `PDW_region-SQLCTL01.appliance_domain.local`, ou simplement `appliance_domain-SQLCTL01`. La connexion peut être établie plus rapidement si vous vous connectez avec le nom complet et le suffixe DNS.  
  
    Exemples pour une application nommée MyAPS avec une région MyPDW PDW :  
  
    -   MyPDW-SQLCTL01.MyAPS.local  
  
    -   MyPDW-SQLCTL01  
  
## <a name="see-also"></a>Voir aussi  
[Obtenir et configurer un serveur de chargement ](acquire-and-configure-loading-server.md)  
  
