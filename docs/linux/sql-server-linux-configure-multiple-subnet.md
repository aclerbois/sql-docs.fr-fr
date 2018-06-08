---
title: Configurer les sous-réseaux de plusieurs groupes de disponibilité AlwaysOn et les instances de cluster de basculement sur Linux | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 12/1/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 612ac9c684fa8e0383981a32a94cf82d8ec68678
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-multiple-subnet-always-on-availability-groups-and-failover-cluster-instances"></a>Configurer les sous-réseaux de plusieurs groupes de disponibilité AlwaysOn et les instances de cluster de basculement

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Lorsqu’une instance de cluster toujours sur disponibilité Group (AG) ou le basculement (FCI) s’étend sur plusieurs sites, chaque site généralement possède sa propre mise en réseau. Souvent, cela signifie que chaque site possède sa propre adresse IP. Par exemple, les adresses du Site A commencent par 192.168.1. *x* et les adresses du Site B commencent par 192.168.2. *x*, où *x* est la partie de l’adresse IP qui est unique pour le serveur. Sans une sorte de routage en place au niveau de la couche réseau, ces serveurs ne sera pas en mesure de communiquer entre eux. Il existe deux façons de gérer ce scénario : la configuration du réseau qui relie les deux sous-réseaux différents, appelés un réseau local virtuel, ou configurer le routage entre les sous-réseaux.

## <a name="vlan-based-solution"></a>Solution basée sur le réseau local virtuel
 
**Configuration requise**: solution de base pour un réseau local virtuel, chaque serveur impliqué dans un groupe de disponibilité ou de la FCI a besoin de deux cartes réseau (NIC) pour une disponibilité (un double port de carte réseau sera un point de défaillance sur un serveur physique unique), afin qu’il puisse être attribué des adresses IP sur son sous-réseau natif ainsi que l’autre sur le réseau local virtuel. Il s’agit en plus des autres besoins réseau, telles qu’iSCSI, qui doit également son propre réseau.

La création d’une adresse IP pour le groupe de disponibilité ou l’ICF est effectuée sur le réseau local virtuel. Dans l’exemple suivant, le réseau local virtuel a un sous-réseau de 192.168.3. *x*, de sorte que l’adresse IP créée pour le groupe de disponibilité ou l’ICF est 192.168.3.104. Aucune opération supplémentaire ne doit être configuré, car il existe une adresse IP unique affectée au groupe de disponibilité ou instance FCI.

![](./media/sql-server-linux-configure-multiple-subnet/image1.png)

## <a name="configuration-with-pacemaker"></a>Configuration avec STIMULATEUR

Dans l’univers Windows, un Cluster de basculement de Windows Server (WSFC) en mode natif prend en charge de plusieurs sous-réseaux et gère plusieurs adresses IP via une dépendance ou de l’adresse IP. Sur Linux, il n’existe aucune dépendance OR, mais il existe un moyen de réaliser un sous-réseau multi approprié en mode natif STIMULATEUR, comme indiqué par le texte suivant. Pour cela, vous ne pouvez pas simplement à l’aide de la ligne de commande STIMULATEUR normale pour modifier une ressource. Vous devez modifier les informations de cluster base (CIM). Le CIM est un fichier XML avec la configuration STIMULATEUR.

![](./media/sql-server-linux-configure-multiple-subnet/image2.png)

### <a name="update-the-cib"></a>Mettre à jour le CIM

1.  Exporter le CIM.

    **Red Hat Enterprise Linux (RHEL) et Ubuntu**

    ```bash
    sudo pcs cluster cib <filename>
    ```

    **SUSE Linux Enterprise Server (SLES)**

    ```bash
    sudo cibadmin -Q > <filename>
    ```

    Où *nom de fichier* est le nom que vous souhaitez appeler le CIM.

2.  Modifiez le fichier qui a été généré. Recherchez la `<resources>` section. Vous verrez les différentes ressources qui ont été créés pour le groupe de disponibilité ou de la FCI. Trouver celui associé à l’adresse IP. Ajouter un `<instance attributes>` section avec les informations de la deuxième adresse IP supérieure ou inférieure à celle existant, mais avant que `<operations>`. Il est similaire à la syntaxe suivante :

    ```xml
    <instance attributes id="<NameForAttribute>" score="<Score>">
        <rule id="<RuleName>" score="INFINITY">
            <expression id="<ExpressionName>" attribute="\#uname" operation="eq" value="<NodeNameInSubnet2>" />
        </rule>
        <nvpair id="<NameForSecondIP>" name="ip" value="<IPAddress>"/>
        <nvpair id="<NameForSecondIPNetmask>" name="cidr\_netmask" value="<Netmask>"/>
    </instance attributes>
    ```
    
    où *NameForAttribute* est le nom unique pour cet attribut, *Score* est le numéro affecté à l’attribut, qui doit être supérieur au sous-réseau principal, *RuleName*est le nom de la règle, *ExpressionName* est le nom de l’expression, *NodeNameInSubnet2* est le nom du nœud dans un autre sous-réseau, *NameForSecondIP* est le nom associé à la deuxième adresse IP, *IPAddress* est l’adresse IP du deuxième sous-réseau *NameForSecondIPNetmask* est le nom associé avec le masque de réseau, et *Masque réseau* est le masque de réseau pour le deuxième sous-réseau.
    
    Voici un exemple.
    
    ```xml
    <instance attributes id="Node3-2nd-IP" score="2">
        <rule id="Subnet2-IP" score="INFINITY">
            <expression id="Subnet2-Node" attribute="\#uname" operation="eq" value="Node3" />
        </rule>
        <nvpair id="IP-In-Subnet-2" name="ip" value="192.168.2.102"/>
        <nvpair id="Netmask-For-IP2" name="cidr\_netmask" value="24" />
    </instance attributes>
    ```

3.  Importer le CIM modifié et reconfigurer STIMULATEUR.

    **RHEL/Ubuntu**
    
    ```bash
    sudo pcs cluster cib-push <filename>
    ```

    **SLES**
    
    ```bash
    sudo cibadmin -R -x <filename>
    ```

    où *nom de fichier* est le nom du fichier CIM avec les informations d’adresse IP modifiées.

### <a name="check-and-verify-failover"></a>Vérifiez et vérifier le basculement

1.  Une fois le CIM est correctement appliquée avec la configuration mise à jour, interrogez le nom DNS associé à la ressource d’adresse IP dans STIMULATEUR. Il doit refléter l’adresse IP associé au sous-réseau qui héberge actuellement le groupe de disponibilité ou de la FCI.
2.  Ne pas le groupe de disponibilité ou FCI autres le sous-réseau.
3.  Une fois le groupe de disponibilité ou l’ICF entièrement en ligne, interrogez le nom DNS associé à l’adresse IP. Il doit refléter l’adresse IP dans le deuxième sous-réseau.
4.  Si vous le souhaitez, restauré le groupe de disponibilité ou ICF sur le sous-réseau d’origine.
