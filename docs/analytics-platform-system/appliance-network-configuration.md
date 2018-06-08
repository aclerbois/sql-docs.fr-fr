---
title: Configuration réseau - système de plateforme Analytique | Documents Microsoft
description: Le matériel de système de plateforme Analytique (APS) créé et configuré avec un ensemble de correction des adresses IP dans l’ensemble de tous les serveurs et appareils applicables à partir de l’usine du fabricant de matériel. Lors de la livraison de l’application, l’adresse IP externe (Ethernet) traité doit être reconfiguré pour répondre aux besoins de centre de données du client spécifique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 2db040c63d3c31f93cd0b72e48422e806aef01e0
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="appliance-network-configuration-for-analytics-platform-system"></a>Configuration réseau pour le système de plateforme Analytique
Le matériel de système de plateforme Analytique (APS) créé et configuré avec un ensemble de correction des adresses IP dans l’ensemble de tous les serveurs et appareils applicables à partir de l’usine du fabricant de matériel. Lors de la livraison de l’application, l’adresse IP externe (Ethernet) traité doit être reconfiguré pour répondre aux besoins de centre de données du client spécifique.  
  
> [!NOTE]  
> PDW V1 requis 8 IP externe (*face au client*) des adresses pour fournir une connectivité externe à chaque contrôle rack de nœuds. PDW 2012 (V2) améliorée des communications réseau en exposant chaque composant de l’application en externe via les adresses IP. Cette approche fournit une conception plus robuste, ce qui réduit les coûts et augmente la flexibilité et améliore le déplacement des données, le chargement de données et l’intégration de Hadoop. Le nombre d’adresses IP nécessaire varie selon le nombre de nœuds dans l’application et la présence de fonctionnalités, telles que HDInsight. Pour prendre en compte ce plus grand bloc d’adresses IP, le client doit configurer un sous-réseau distinct pour PDW. Dans ce sous-réseau, il y a un espace d’adressage IP (jusqu'à 250 adresses) pour prendre en charge les composants de jusqu'à 5 racks PDW.  
  
Le **Configuration réseau** page permet de vous permet d’afficher les paramètres de réseau externe pour les nœuds sur votre appliance Analytique plateforme système. Cette page est en lecture seule.  
  
![Réseau des appliances DWConfig](./media/appliance-network-configuration/SQL_Server_PDW_DWConfig_ApplTopNetwork.png "SQL_Server_PDW_DWConfig_ApplTopNetwork")  
  
## <a name="to-update-the-network-configuration-on-your-appliance"></a>Pour mettre à jour la configuration du réseau sur votre appliance  
Modifier les adresses IP de l’ensemble fibre optique domaine, domaine de la charge de travail et HDInsight domaines en modifiant le **AplianceInfo.xml** fichier, puis vous exécutez le programme d’installation. Il s’agit d’une opération hors connexion. HDInsight (le cas échéant) et PDW régions seront automatiquement arrêtées pendant la modification d’adresse IP.  
  
> [!NOTE]  
> Les noms de domaine sont fournis lors de l’installation et sont spécifiés comme jusqu'à 6 caractères alphanumériques, commençant par une lettre. Un système d’affectation de noms fréquent crée un domaine de l’ensemble fibre optique en commençant par F, un domaine de la charge de travail PDW commençant par P et un domaine HDInsight commençant par H. Ce format est présumé tout au long de l’aide du fichier, mais n’est pas obligatoire. <!-- MISSING LINKS For more information about the domain structure, see [PDW Domain Security &#40;SQL Server PDW&#41;](../sqlpdw/pdw-domain-security-sql-server-pdw.md) and [Understanding the Security Model of the HDInsight Region &#40;Analytics Platform System&#41;](../hdinsight/understanding-the-security-model-of-the-hdinsight-region.md)  -->  
  
#### <a name="to-change-the-ip-addresses-of-the-analytics-platform-system"></a>Pour modifier les adresses IP du système de plateforme Analytique  
  
1.  À l’aide de la **Bureau à distance** application, se connecter à **HST01** à l’aide du compte administrateur de domaine de la charge de travail.  
  
2.  Sur le nœud HST01, ouvrez le fichier info appliance **c:\pdwinst\media\AplianceInfo.xml**.  
  
    > [!NOTE]  
    > Si le fichier n’est pas présent, un nouveau fichier peut doivent être créés.  
  
3.  Mettre à jour les valeurs Ethernet IP en fonction des besoins et enregistrez le fichier.  
  
4.  Dans une fenêtre d’invite de commandes, exécutez la commande suivante pour mettre à jour les adresses IP de la région PDW, en utilisant les noms de domaine P/F/H et les mots de passe d’administrateur.  
  
    ```  
    c:\pdwinst\media\setup.exe /action="ConfigureEthernet" /DomainAdminPassword="<password>" /ApplianceInfoFile="C:\PDWINST\media\ApplianceInfo.xml"  
    ```  
  
## <a name="manufacturer-references"></a>Références du fabricant  
Pour plus d’informations sur les équipements Dell, consultez :  
  
-   Instructions de commutateur PowerConnect [Guide de référence du système de la série Dell PowerConnect 6200 CLI](http://downloads.dell.com/Manuals/all-products/esuprt_ser_stor_net/esuprt_powerconnect/powerconnect-6224f_Reference%20Guide_en-us.pdf)  
  
-   iDRAC/BMC [Guide de l’utilisateur de la Version 1.30.30 intégré Dell distant accès contrôleur 7 (iDRAC7)](http://downloads.dell.com/Manuals/all-products/esuprt_electronics/esuprt_software/esuprt_remote_ent_sys_mgmt/integrated-dell-remote-access-cntrllr-7-v1.30.30_User%27s%20Guide_en-us.pdf?c=us&l=en&cs=555&s=biz)  
  
-   PDU **Dell limitées Rack PDU**`ftp://ftp.dell.com/Manuals/all-products/esuprt_ser_stor_net/esuprt_rack_infrastructure/dell-metered-pdu-led_User's%20Guide_en-us.pdf`  
  
## <a name="see-also"></a>Voir aussi  
[Lancez le Gestionnaire de Configuration &#40;Analytique plate-forme système&#41;](launch-the-configuration-manager.md)  
  
