---
title: Résoudre les problèmes de SQL Server sur Linux | Documents Microsoft
description: Fournit des conseils de dépannage pour l’utilisation de SQL Server 2017 sur Linux.
author: annashres
ms.author: anshrest
manager: craigg
ms.date: 04/30/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 966e2e389bbefeafcb381ddaecff7b7303ba489d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="troubleshoot-sql-server-on-linux"></a>Résoudre les problèmes de SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Ce document décrit comment résoudre les problèmes de Microsoft SQL Server s'exécutant sur Linux ou dans un conteneur Docker. Lors du dépannage de SQL Server sur Linux, n’oubliez pas de consulter les fonctionnalités prises en charge et les limitations connues dans les [notes de publication de SQL Server sur Linux](sql-server-linux-release-notes.md).

> [!TIP]
> Pour obtenir des réponses aux questions fréquemment posées, consultez le [SQL Server sur le Forum aux questions sur Linux](sql-server-linux-faq.md).

## <a id="connection"></a> Résoudre les échecs de connexion
Si vous rencontrez des difficultés pour vous connecter à votre serveur SQL Server sous Linux, il existe quelques éléments à vérifier. 

- Vérifiez que le nom du serveur ou l’adresse IP est accessible à partir de votre ordinateur client.

   > [!TIP]
   > Pour rechercher l’adresse IP de votre machine Ubuntu, vous pouvez exécuter la commande ifconfig comme dans l’exemple suivant :
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > Pour Red Hat, vous pouvez utiliser l’adresse ip, comme dans l’exemple suivant :
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > Une exception à cette technique concerne les machines virtuelles Azure. Pour les machines virtuelles Azure, [rechercher l’adresse IP publique de la machine virtuelle dans le portail Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect).

- Le cas échéant, vérifiez que vous avez ouvert le port SQL Server (1433 par défaut) sur le pare-feu.

- Pour les machines virtuelles Azure, vérifiez que vous avez une [règle de groupe de sécurité réseau pour le port de SQL Server par défaut](https://docs.microsoft.com/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote).

- Vérifiez que le nom d’utilisateur et le mot de passe ne contiennent pas de fautes de frappe, ni d'espaces, ni une casse incorrecte.

- Essayez de définir explicitement le numéro de port et de protocole avec le nom du serveur à l’exemple suivant : **tcp:servername, 1433**.

- Des problèmes de connectivité réseau peuvent également entraîner des délais d’attente et des erreurs de connexion. Après avoir vérifié vos informations de connexion et la connectivité réseau, recommencez l’opération.

## <a name="manage-the-sql-server-service"></a>Gérer le service SQL Server

Les sections suivantes montrent comment démarrer, arrêter, redémarrer et vérifier l’état du service SQL Server. 

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>Gérer le service mssql-server sous Red Hat Enterprise Linux (RHEL) et Ubuntu 

Vérifiez l’état du service SQL Server à l’aide de cette commande :

   ```bash
   sudo systemctl status mssql-server
   ```

Vous pouvez arrêter, démarrer ou redémarrer le service SQL Server en fonction des besoins à l’aide des commandes suivantes :

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>Gérer l’exécution du conteneur Docker mssql

Vous pouvez obtenir l’ID de conteneur et d’état du dernier conteneur Docker de serveur SQL créé en exécutant la commande suivante (l’ID est sous le **ID de conteneur** colonne) :

   ```bash
   sudo docker ps -l
   ```
   
Vous pouvez arrêter ou redémarrer le service SQL Server en fonction des besoins à l’aide des commandes suivantes :
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> Pour obtenir des conseils de dépannage supplémentaires pour Docker, consultez [conteneurs dépannage de SQL Server Docker](sql-server-linux-configure-docker.md#troubleshooting).

## <a name="access-the-log-files"></a>Accéder aux fichiers journaux
   
SQL Server stocke ses journaux dans le fichier /var/opt/mssql/log/errorlog dans les installations sous Linux et Docker.
 Vous devez être en mode de 'super utilisateur' pour parcourir ce répertoire.

Le programme d’installation enregistre ici : /var/opt/mssql/setup-< horodatage qui représente l'heure d’installation> Vous pouvez parcourir les fichiers journaux des erreurs avec n’importe quel outil compatible UTF-16 tel que « vim » ou « cat » comme suit : 

   ```bash
   sudo cat errorlog
   ```

Si vous préférez, vous pouvez également convertir les fichiers vers UTF-8 pour les lire avec « more » ou « less » avec la commande suivante :
   
   ```bash
   sudo iconv –f UTF-16LE –t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>Événements étendus

Les événements étendus peuvent être interrogées via une commande SQL.  Plus d’informations sur les événements étendus accessibles [ici](https://technet.microsoft.com/en-us/library/bb630282.aspx):

## <a name="crash-dumps"></a>Vidages sur incident 

Recherchez les vidages dans le répertoire de journaux dans Linux. Recherchez dans le répertoire /var/opt/mssql/log les vidages Linux Core (extension .tar.gz2) ou les mini-vidages SQL (extension .mdmp)

Pour les vidages 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

Pour les sauvegardes SQL 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>Démarrage de SQL Server dans une Configuration minimale ou en Mode mono-utilisateur

### <a name="start-sql-server-in-minimal-configuration-mode"></a>Démarrage de SQL Server en Mode Configuration minimale
Cette option est utile lorsqu'une valeur de configuration définie (espace mémoire insuffisant, par exemple) a empêché le serveur de démarrer.
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>Démarrage de SQL Server en Mode mono-utilisateur
Dans certaines circonstances, vous devrez peut-être démarrer une instance de SQL Server en mode mono-utilisateur à l’aide de l’option de démarrage -m. Vous pouvez par exemple vouloir modifier les options de configuration du serveur ou rétablir une base de données maître ou une autre base de données système endommagée. Par exemple, vous pouvez vouloir modifier les options de configuration du serveur ou récupérer une base de données maître endommagée ou une autre base de données système   

Démarrage de SQL Server en Mode mono-utilisateur
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

Démarrage de SQL Server en Mode mono-utilisateur avec SQLCMD
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  Démarrez SQL Server sous Linux avec l’utilisateur « mssql » afin d’éviter les problèmes de démarrage à l'avenir. Exemple « sudo -u mssql /opt/mssql/bin/sqlservr [OPTIONS DE DÉMARRAGE] » 

Si vous avez démarré par inadvertance SQL Server avec un autre utilisateur, vous devez changer la propriété des fichiers de base de données SQL Server pour l'attribuer à l’utilisateur 'mssql' avant de démarrer SQL Server avec systemd. Par exemple, exécutez la commande suivante pour modifier la propriété de tous les fichiers de base de données sous /var/opt/mssql sur l’utilisateur « mssql »,

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>Reconstruire des bases de données système
En dernier recours, vous pouvez choisir de recréer le fichier principal et bases de données de modèle dans les versions par défaut.

> [!WARNING]
> Ces étapes seront **supprimer toutes les données du système SQL Server** que vous avez configuré ! Cela inclut des informations sur les bases de données (mais pas les bases de données utilisateur eux-mêmes). Autres informations stockées dans les bases de données système, y compris les éléments suivants seront également supprimés : informations de clé principale, les certificats chargés dans master, le mot de passe de connexion SA, les informations liées à la tâche à partir de msdb, les informations de messagerie de base de données msdb, et que les options de sp_configure. Utiliser uniquement si vous comprenez les implications !

1. Arrêt de SQL Server.

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Exécutez **sqlservr** avec la **le programme d’installation-force** paramètre. 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > Consultez l’avertissement précédent ! En outre, vous devez exécuter en tant que le **mssql** utilisateur comme indiqué ici.

1. Une fois que vous voyez le message « Récupération est terminée », appuyez sur CTRL + C. SQL Server s’arrête

1. Reconfigurer le mot de passe SA.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. Démarrage de SQL Server et reconfigurer le serveur. Cela inclut la restauration ou attachement de nouveau toutes les bases de données utilisateur.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="common-issues"></a>Problèmes courants

1. Impossible de se connecter à votre instance de SQL Server à distance.

   Consultez la section Dépannage de l’article, [se connecter à SQL Server sur Linux](#connection).

2. Erreur : Le nom d’hôte doit compter 15 caractères maximum.

   Il s’agit d’un problème connu qui se produit chaque fois que le nom de l’ordinateur sur lequel on tente d’installer le package Debian SQL Server est supérieur à 15 caractères. Il n’existe actuellement aucune solution de contournement autre que la modification du nom de l’ordinateur. Une façon d’effectuer cette opération est de modifier le fichier de nom d’hôte et de redémarrer l’ordinateur. Le [guide du site Web](http://www.cyberciti.biz/faq/ubuntu-change-hostname-command/) suivant explique cela en détail.

3. La réinitialisation de mot de passe système (SA) d’administration.

   Si vous avez oublié le mot de passe d’administrateur système ou que vous devez le réinitialiser pour une raison quelconque, procédez comme suit.

   > [!NOTE]
   > Les étapes suivantes arrêter le service SQL Server temporairement.

   Connectez vous au terminal de l’ordinateur hôte, exécutez les commandes suivantes, suivez les invites pour réinitialiser le mot de passe SA :

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. Utiliser des caractères spéciaux dans le mot de passe.

   Si vous utilisez des caractères dans le mot de passe du compte de connexion SQL Server, vous devrez peut-être caractère d’échappement avec une barre oblique inverse lorsque vous les utilisez dans une commande de Linux dans le terminal. Par exemple, vous devez isoler le symbole dollar ($) chaque fois que vous l’utilisez dans un script de shell de commande/Terminal Server :

   Ne fonctionne pas :

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   Fonctionne :

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   Ressources : [des caractères spéciaux](http://tldp.org/LDP/abs/html/special-chars.html)
   [Escaping](http://tldp.org/LDP/abs/html/escapingsection.html)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]
