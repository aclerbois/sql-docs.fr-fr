---
title: Configurer le stockage iSCSI basculement cluster instance - SQL Server sur Linux | Documents Microsoft
description: ''
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.date: 08/28/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.openlocfilehash: 6876ac9f3aa6641efe4e08e6c434870347315bc6
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-failover-cluster-instance---iscsi---sql-server-on-linux"></a>Configurer l’instance de cluster de basculement - iSCSI - SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article explique comment configurer le stockage iSCSI pour une instance de cluster de basculement (FCI) sur Linux. 

## <a name="configure-iscsi"></a>Configurer l’iSCSI 
iSCSI utilise le réseau pour présenter les disques à partir d’un serveur configuré comme une cible pour les serveurs. Les serveurs qui se connectent à la cible iSCSI nécessitent qu’un initiateur iSCSI soit configuré. Les disques sur le serveur cible disposent d’autorisations explicites afin qu’uniquement les initiateurs qui doivent être en mesure d’y accéder puissent le faire. La cible elle-même doit être hautement disponible et fiable.

### <a name="important-iscsi-target-information"></a>Informations de la cible iSCSI important
Bien que cette section ne décrive pas comment configurer une cible iSCSI, car cette configuration est spécifique du type de source que vous souhaitez utiliser, assurez-vous que la sécurité pour les disques qui seront utilisés par les nœuds de cluster est configurée.  

La cible ne doit jamais être configurée sur un nœud de l’instance de cluster si vous utilisez une cible iSCSI basée sur Linux. Pour les performances et la disponibilité, les réseaux iSCSI doivent être distincts de ceux utilisés par le trafic réseau normal sur le serveur source et le serveur client. Les réseaux utilisés pour iSCSI doivent être rapides. N’oubliez pas que ce réseau consomme la bande passante du processeur, planifiez en conséquence si vous utilisez un serveur normal.
Le plus important sur le serveur cible est de veiller à ce que les disques qui sont créés disposent des autorisations appropriées afin que seuls les serveurs participant à l’instance de cluster de basculement puissent y accéder. Un exemple est illustré ci-dessous à partir de la cible Microsoft iSCSI, où linuxnodes1 est le nom créé. Dans ce cas, les adresses IP des nœuds sont affectées pour que NewFCIDisk1.vhdx soit visible pour eux.

![Initiateur][1]

### <a name="instructions"></a>Instructions

Cette section décrit comment configurer un initiateur iSCSI sur les serveurs qui seront utilisés comme nœuds de l’instance de cluster de basculement. Les instructions doivent fonctionner sur RHEL et Ubuntu.

Pour plus d’informations sur l’initiateur iSCSI pour les distributions prises en charge, consultez les liens suivants :
- [Red Hat](http://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/iscsi-api.html)
- [SUSE](http://www.suse.com/documentation/sles11/stor_admin/data/sec_inst_system_iscsi_initiator.html) 
- [Ubuntu](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html)

1.  Choisissez un des serveurs participant à la configuration de l'instance de cluster de basculement. peu importe lequel. iSCSI devant être sur un réseau dédié, configurez iSCSI pour reconnaître et utiliser ce réseau. Exécutez `sudo iscsiadm -m iface -I <iSCSIIfaceName> -o new` où `<iSCSIIfaceName>` est le nom unique ou convivial du réseau. L’exemple suivant utilise `iSCSINIC`:
   
    ```bash
    sudo iscsiadm -m iface -I iSCSINIC -o new
    ```
    ![7-setiscsinetwork][6]
 
2.  Modifier `/var/lib/iscsi/ifaces/iSCSIIfaceName`. Assurez-vous que les valeurs suivantes sont renseignés :

    - iface.net_ifacename est le nom de la carte réseau comme indiqué dans le système d’exploitation.
    - iface.hwaddress est l’adresse MAC du nom unique qui sera créé pour cette interface ci-dessous.
    - iface.IPAddress
    - iface.subnet_Mask 

    Observez l'exemple suivant :

    ![iSCSITargetSettings][2]

3.  Rechercher la cible iSCSI.

    ```bash
    sudo iscsiadm -m discovery -t sendtargets -I <iSCSINetName> -p <TargetIPAddress>:<TargetPort>
    ```

     \<iSCSINetName > est le nom unique/convivial pour le réseau, \<TargetIPAddress > est l’adresse IP de la cible iSCSI, et \<TargetPort > est le port de la cible iSCSI. 

    ![iSCSITargetResults][3]

 
4.  Connectez-vous à la cible

    ```bash
    sudo iscsiadm -m node -I <iSCSIIfaceName> -p TargetIPAddress -l
    ```

    \<iSCSIIfaceName > est le nom unique/convivial pour le réseau et \<TargetIPAddress > est l’adresse IP de la cible iSCSI.

    ![iSCSITargetLogin][4]

5.  Vérifiez qu’il existe une connexion à la cible iSCSI.

    ```bash
    sudo iscsiadm -m session
    ```

    ![iSCSIVerify][5]

 
6.  Vérifiez les disques iSCSI reliés

    ```bash
    sudo grep “Attached SCSI” /var/log/messages
    ```
    ![30-iSCSIattachedDisks][7]

7.  Créez un volume physique sur le disque iSCSI.

    ```bash
    sudo pvcreate /dev/<devicename>
    ```

    \<DeviceName > est le nom de l’appareil de l’étape précédente. 

 
8.  Créez un groupe de volumes sur le disque iSCSI. Les disques affectés à un seul groupe de volumes sont vus comme un pool ou une collection. 

    ```bash
    sudo vgcreate <VolumeGroupName> /dev/devicename
    ```

    \<VolumeGroupName > est le nom du groupe de volumes et \<devicename > est le nom de l’appareil à partir de l’étape 6. 
 
9.  Créer et vérifier le volume logique pour le disque.

    ```bash
    sudo lvcreate -Lsize -n <LogicalVolumeName> <VolumeGroupName>
    ```
    
    \<taille > est la taille du volume à créer et peut être spécifié avec G (Go), T (téraoctets), etc.,\<LogicalVolumeName > est le nom du volume logique, et \<VolumeGroupName > est le nom du groupe de volumes à partir de la étape précédente. 

    L’exemple ci-dessous crée un volume de 25 Go.
 
    ![Create25GBVol][10]

10. Exécutez `sudo lvs` pour afficher le Gestionnaire de volume logique qui a été créé.
 
11. Formater le volume logique avec un système de fichiers pris en charge. Pour EXT4, utilisez l’exemple suivant :

    ```bash
    sudo mkfs.ext4 /dev/<VolumeGroupName>/<LogicalVolumeName>
    ```

    \<VolumeGroupName > est le nom du groupe de volumes à partir de l’étape précédente. \<LogicalVolumeName > est le nom du volume logique à partir de l’étape précédente.  

12. Pour les bases de données système ou les informations stockées dans l’emplacement de données par défaut, procédez comme suit. Sinon, passez à l’étape 13.

   *    Vérifiez que SQL Server est arrêté sur le serveur que vous utilisez.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

   *    Basculez entièrement en super utilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    sudo -i
    ```

   *    Basculez vers l’utilisateur mssql. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    su mssql
    ```

   *    Créez un répertoire temporaire pour stocker les données de SQL Server et les fichiers journaux. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    mkdir <TempDir>
    ```

    \<TempDir > est le nom du dossier. L’exemple suivant crée un dossier nommé /var/opt/mssql/TempDir.

    ```bash
    mkdir /var/opt/mssql/TempDir
    ```
    
   *    Copiez les fichiers journaux et les données de SQL Server dans le répertoire temporaire. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    cp /var/opt/mssql/data/* <TempDir>
    ```

    \<TempDir > est le nom du dossier à partir de l’étape précédente.
    
   *    Vérifiez que les fichiers se trouvent dans le répertoire.

    ```bash
    ls \<TempDir>
    ```
    \<TempDir > est le nom du dossier à partir de l’étape d.

   *    Supprimez les fichiers à partir du répertoire de données SQL Server existant. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    rm – f /var/opt/mssql/data/*
    ```

   *    Vérifiez que les fichiers ont été supprimés. L’illustration ci-dessous montre un exemple de toute la séquence de c à h.

    ```bash
    ls /var/opt/mssql/data
    ```

    ![45-CopyMove][8]
 
   *    Tapez `exit` pour revenir à l’utilisateur racine.

   *    Montez le volume logique iSCSI dans le dossier de données SQL Server. Vous ne recevrez aucun accusé de réception en cas de réussite.

    ```bash
    mount /dev/<VolumeGroupName>/<LogicalVolumeName> /var/opt/mssql/data
    ``` 

    \<VolumeGroupName > est le nom du groupe de volumes et \<LogicalVolumeName > est le nom du volume logique qui a été créé. L’exemple de syntaxe suivante correspond au groupe de volumes et des volumes logiques à partir de la commande précédente.

    ```bash
    mount /dev/FCIDataVG1/FCIDataLV1 /var/opt/mssql/data
    ``` 

   *    Remplacez le propriétaire du montage par mssql. Vous ne recevrez aucun accusé de réception en cas de réussite.

    ```bash
    chown mssql /var/opt/mssql/data
    ```

   *    Définissez mssql comme propriétaire du groupe de montage. Vous ne recevrez aucun accusé de réception en cas de réussite.

    ```bash
    chgrp mssql /var/opt/mssql/data
    ``` 

   *    Basculez vers l’utilisateur mssql.  Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    su mssql
    ``` 

   *    Copiez les fichiers à partir du répertoire temporaire /var/opt/mssql/data. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    cp /var/opt/mssql/TempDir/* /var/opt/mssql/data
    ``` 

   *    Vérifiez les fichiers.

    ```bash
    ls /var/opt/mssql/data
    ``` 
 
   *    Entrez `exit` pour ne plus être mssql.
    
   *    Entrez `exit` pour ne plus être racine.

   *    Démarrez SQL Server. Si tout a été correctement copié et que la sécurité a été appliquée, SQL Server doit apparaître comme ayant démarré.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ``` 
 
   *    Arrêtez SQL Server et vérifiez qu’il est arrêté.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ``` 

13. Pour des éléments autres que des bases de données système, par exemple des bases de données utilisateur ou des sauvegardes, effectuez les étapes suivantes. Si vous utilisez uniquement l’emplacement par défaut, passez à l’étape 14.

   *    Passez en mode superutilisateur. Vous ne recevrez pas d’accusé de réception en cas de réussite.

    ```bash
    sudo -i
    ```

   *    Créez un dossier qui sera utilisé par SQL Server. 

    ```bash
    mkdir <FolderName>
    ```

    \<Nom_dossier > est le nom du dossier. Le chemin du dossier complet doit être spécifié s'il n'est pas au bon emplacement. L’exemple suivant crée un dossier nommé /var/opt/mssql/userdata.

    ```bash
    mkdir /var/opt/mssql/userdata
    ```

   *    Montez le volume logique iSCSI dans le dossier créé à l’étape précédente. Vous ne recevrez aucun accusé de réception en cas de réussite.
    
    ```bash
    mount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
    ```

    \<VolumeGroupName> est le nom du groupe de volumes, \<LogicalVolumeName> le nom du volume logique créé et \<FolderName> le nom du dossier. Un exemple de syntaxe est illustré ci-dessous. Exemple de syntaxe est illustré ci-dessous.

    ```bash
    mount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
    ```

   *    Définissez mssql comme propriétaire du dossier créé. Vous ne recevrez aucun accusé de réception en cas de réussite.

    ```bash
    chown mssql <FolderName>
    ```

    \<Nom_dossier > est le nom du dossier qui a été créé. comme dans l’exemple suivant :

    ```bash
    chown mssql /var/opt/mssql/userdata
    ```
  
   *    Remplacez le groupe du dossier créé par mssql. Vous ne recevrez aucun accusé de réception en cas de réussite.

    ```bash
    chown mssql <FolderName>
    ```

    \<Nom_dossier > est le nom du dossier qui a été créé. comme dans l’exemple suivant :

    ```bash
    chown mssql /var/opt/mssql/userdata
    ```

   *    Tapez `exit` pour ne plus être superutilisateur.

   *    Pour tester, créez une base de données dans ce dossier. L’exemple ci-dessous utilise sqlcmd pour créer une base de données, basculez vers elle, vérifiez que les fichiers existent au niveau du système d’exploitation, puis supprimez l’emplacement temporaire. Vous pouvez utiliser SSMS.
  
    ![50-ExampleCreateSSMS][9]

   *    Démontez le partage 

    ```bash
    sudo umount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
    ```

    \<VolumeGroupName> est le nom du groupe de volumes, \<LogicalVolumeName> le nom du volume logique créé et \<FolderName> le nom du dossier. Un exemple de syntaxe est illustré ci-dessous. Exemple de syntaxe est illustré ci-dessous.

    ```bash
    sudo umount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
    ```

14. Configurez le serveur de telle sorte que seul Pacemaker puisse activer le groupe de volumes.

    ```bash
    sudo lvmconf --enable-halvm --services –startstopservices
    ```
 
15. Générez une liste des groupes de volumes sur le serveur. Tout élément listé qui n’est pas le disque iSCSI est utilisé par le système, comme pour le disque du système d’exploitation.

    ```bash
    sudo vgs
    ```

16. Modifiez la section de configuration d'activation du fichier /etc/lvm/lvm.conf. Configurez la ligne suivante :

    ```bash
    volume_list = [ <ListOfVGsNotUsedByPacemaker> ]
    ```

    \<ListOfVGsNotUsedByPacemaker > est la liste des groupes de volumes provenant de la sortie de l’étape 20 qui ne seront pas utilisés par l’instance de cluster de basculement. Mettez chaque groupe entre guillemets et séparez-les par des virgules. comme dans l’exemple suivant :

    ![55-ListOfVGs][11]
 
 
17. Au démarrage de Linux, il monte le système de fichiers. Pour vous assurer que seul Pacemaker peut monter le disque iSCSI, régénérez l’image de système du fichiers racine. 

    Exécutez la commande suivante, ce qui peut prendre quelques instants. Vous n’obtenez aucun message de retour en cas de réussite.

    ```bash
    sudo dracut -H -f /boot/initramfs-$(uname -r).img $(uname -r)
    ```

18. Redémarrez le serveur.

19. Sur un autre serveur qui participe à la FCI, effectuez les étapes 1 à 6. Ceci présente la cible iSCSI au serveur SQL Server. 
 
20. Générez une liste des groupes de volumes sur le serveur. Elle doit afficher le groupe de volumes créé précédemment. 

    ```bash
    sudo vgs
    ``` 
23. Démarrez SQL Server et vérifiez qu’il peut être démarré sur ce serveur.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```

24. Arrêtez SQL Server et vérifiez qu’il est arrêté.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```
25. Répétez les étapes 1 à 6 sur les autres serveurs qui participeront à la FCI.

Vous êtes maintenant prêt à configurer l’instance FCI.

|Distribution |Rubrique 
|----- |-----
|**Red Hat Enterprise Linux avec un module complémentaire à haute disponibilité** |[Configurer](sql-server-linux-shared-disk-cluster-configure.md)<br/>[Opérer](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server avec un module complémentaire à haute disponibilité** |[Configurer](sql-server-linux-shared-disk-cluster-sles-configure.md)

## <a name="next-steps"></a>Étapes suivantes

[Configurer l’instance de cluster de basculement - SQL Server sur Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/05-initiator.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/10-iscsiTargetSettings.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/15-iSCSITargetResults.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/20-iSCSITargetLogin.png
[5]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/25-iSCSIVerify.png
[6]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/7-setiscsinetwork.png
[7]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/30-iSCSIattachedDisks.png
[8]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/45-CopyMove.png
[9]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/50-ExampleCreateSSMS.png
[10]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/40-Create25GBVol.png
[11]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/55-ListOfVGs.png
