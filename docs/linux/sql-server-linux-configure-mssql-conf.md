---
title: Configurer les paramètres de SQL Server sur Linux | Documents Microsoft
description: Cet article décrit comment utiliser l’outil mssql-conf pour configurer les paramètres de SQL Server 2017 sur Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 02/20/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 06798dff-65c7-43e0-9ab3-ffb23374b322
ms.openlocfilehash: 6369c3144a9ce641765358621027729ce235f69d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configure-sql-server-on-linux-with-the-mssql-conf-tool"></a>Configurer SQL Server sur Linux avec l’outil mssql-conf

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

**MSSQL-conf** est un script de configuration qui est installé avec SQL Server 2017 pour Red Hat Enterprise Linux, SUSE Linux Enterprise Server et Ubuntu. Vous pouvez utiliser cet utilitaire pour définir les paramètres suivants :

|||
|---|---|
| [Agent](#agent) | Activer l’Agent SQL Server |
| [Classement](#collation) | Définir un nouveau classement pour SQL Server sur Linux. |
| [Commentaires client](#customerfeedback) | Choisissez ou non de SQL Server envoie des commentaires à Microsoft. |
| [Profil de messagerie de base de données](#dbmail) | Définir le profil de messagerie de base de données par défaut pour SQL Server sur Linux |
| [Répertoire de données par défaut](#datadir) | Remplacez le répertoire par défaut pour les nouveaux fichiers de données de base de données SQL Server (.mdf). |
| [Répertoire de journal par défaut](#datadir) | Change le répertoire par défaut pour les nouveaux fichiers de journal (.ldf) de base de données SQL Server. |
| [Répertoire par défaut du fichier de base de données master](#masterdatabasedir) | Change le répertoire par défaut pour les fichiers de base de données master sur l’installation existante de SQL.|
| [Nom de fichier par défaut de la base de données master](#masterdatabasename) | Modifie le nom des fichiers de base de données master. |
| [Répertoire de vidage par défaut](#dumpdir) | Remplacez le répertoire par défaut pour les nouvelles images de mémoire et d’autres fichiers de résolution des problèmes. |
| [Répertoire de journal d’erreur par défaut](#errorlogdir) | Change le répertoire par défaut pour les nouveaux fichiers de journal des erreurs de SQL Server, suivi du Générateur de profils par défaut, XE de Session de contrôle d’intégrité système et XE de Session Hekaton. |
| [Répertoire de sauvegarde par défaut](#backupdir) | Remplacez le répertoire par défaut pour les nouveaux fichiers de sauvegarde. |
| [Type de vidage](#coredump) | Choisissez le type de fichier de vidage de mémoire de vidage à collecter. |
| [Haute disponibilité](#hadr) | Activer les groupes de disponibilité. |
| [Répertoire d’Audit local](#localaudit) | Définir un répertoire à ajouter les fichiers d’Audit Local. |
| [Paramètres régionaux](#lcid) | Définir les paramètres régionaux pour SQL Server à utiliser. |
| [Limite de mémoire](#memorylimit) | Définir la limite de mémoire pour SQL Server. |
| [Port TCP](#tcpport) | Modifiez le port sur lequel SQL Server écoute les connexions. |
| [TLS](#tls) | Configurer la sécurité de niveau Transport. |
| [TraceFlags](#traceflags) | Définissez les traceflags qui va utiliser le service. |

> [!TIP]
> Certains de ces paramètres peuvent également être configuré avec les variables d’environnement. Pour plus d’informations, consultez [des paramètres de configuration de SQL Server avec les variables d’environnement](sql-server-linux-configure-environment-variables.md).

## <a name="usage-tips"></a>Conseils d’utilisation

* Pour les groupes de disponibilité AlwaysOn et les clusters de disque partagé, vous devez toujours apporter les mêmes modifications de configuration sur chaque nœud.

* Pour le scénario de cluster de disque partagé, n’essayez pas de redémarrer la **mssql-serveur** service pour appliquer les modifications. SQL Server est en cours d’exécution en tant qu’application. Au lieu de cela, faites passer la ressource puis en ligne et hors connexion.

* Ces exemples, exécutez mssql-conf par spécifient le chemin d’accès complet : **/opt/mssql/bin/mssql-conf**. Si vous choisissez accéder à ce chemin d’accès à la place, exécutez mssql-conf dans le contexte du répertoire actif : **. / mssql-conf**.

## <a id="agent"></a> Activer l’Agent SQL Server

Le **sqlagent.enabled** paramètre active [l’Agent SQL Server](sql-server-linux-run-sql-server-agent-job.md). Par défaut, l’Agent SQL Server est désactivé. Si **sqlagent.enabled** n’est pas présent dans le fichier de paramètres mssql.conf, puis SQL Server en interne part du principe que l’Agent SQL Server est activé.

Pour modifier ces paramètres, procédez comme suit :

1. Activer l’Agent SQL Server :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a id="collation"></a> Modifier le classement de SQL Server

Le **set-classement** option modifie la valeur de classement à un des classements pris en charge.

1. Première [sauvegarde toutes les bases de données utilisateur](sql-server-linux-backup-and-restore-database.md) sur votre serveur.

1. Utilisez ensuite le [sp_detach_db](../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) une procédure stockée à détacher les bases de données utilisateur.

1. Exécutez le **set-classement** option et suivez les invites :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-collation
   ```

1. L’utilitaire mssql-conf va tenter de modifier la valeur de classement spécifié et redémarrez le service. S’il existe des erreurs, il restaure le classement à la valeur précédente.

1. Restaurer vos sauvegardes de base de données utilisateur.

Pour obtenir la liste des classements pris en charge, exécutez le [sys.fn_helpcollations](../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) fonction : `SELECT Name from sys.fn_helpcollations()`.

## <a id="customerfeedback"></a> Configurer les commentaires des clients

Le **telemetry.customerfeedback** modifications apportées au paramètre si SQL Server envoie des commentaires à Microsoft ou non. Par défaut, cette valeur est définie **true**. Pour modifier la valeur, exécutez les commandes suivantes :

1. Exécutez le script mssql-conf en tant que racine avec le **définir** commande **telemetry.customerfeedback**. L’exemple suivant désactive les commentaires des clients en spécifiant **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

Pour plus d’informations, consultez [Feedback client pour SQL Server sur Linux](sql-server-linux-customer-feedback.md).

## <a id="datadir"></a> Modifier l’emplacement de répertoire par défaut des données ou de journal

Le **filelocation.defaultdatadir** et **filelocation.defaultlogdir** modifier les paramètres de l’emplacement où les nouvelle base de données et les fichiers journaux sont créés. Par défaut, cet emplacement est /var/opt/mssql/data. Pour modifier ces paramètres, procédez comme suit :

1. Créer le répertoire cible pour la nouvelle base de données des fichiers journaux et de données. L’exemple suivant crée un nouveau **/tmp/données** active :

   ```bash
   sudo mkdir /tmp/data
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/data
   sudo chgrp mssql /tmp/data
   ```

1. Mssql-conf permet de modifier le répertoire de données par défaut avec le **définir** commande :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdatadir /tmp/data
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

1. Tous les fichiers de base de données pour les nouvelles bases de données seront désormais être stockées dans ce nouvel emplacement. Si vous souhaitez modifier l’emplacement des fichiers journaux (.ldf) de nouvelles bases de données, vous pouvez utiliser la commande « set » suivant :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultlogdir /tmp/log
   ```

1. Cette commande suppose également qu’un répertoire de journal/tmp/existe et qu’il se trouve sous l’utilisateur et le groupe **mssql**.


## <a id="masterdatabasedir"></a> Modifier l’emplacement de répertoire de fichiers de base de données master par défaut

Le **filelocation.masterdatafile** et **filelocation.masterlogfile** changements de paramètre de l’emplacement où le moteur SQL Server recherche les fichiers de base de données master. Par défaut, cet emplacement est /var/opt/mssql/data. 

Pour modifier ces paramètres, procédez comme suit :

1. Créer le répertoire cible pour les nouveaux fichiers journaux des erreurs. L’exemple suivant crée un nouveau **masterdatabasedir/tmp/** active :

   ```bash
   sudo mkdir /tmp/masterdatabasedir
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
   ```

1. Mssql-conf permet de modifier le répertoire de base de données master par défaut pour les fichiers journaux et de données master avec le **définir** commande :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
   ```

1. Arrêtez le service SQL Server :

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Déplacez le master.mdf et masterlog.ldf : 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf 
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
   ```

1. Démarrez le service SQL Server :

   ```bash
   sudo systemctl start mssql-server
   ```
   
> [!NOTE]
> Si SQL Server ne peut pas trouver les fichiers master.mdf et mastlog.ldf dans le répertoire spécifié, une copie basée sur un modèle de bases de données système sera automatiquement créée dans le répertoire spécifié, et SQL Server démarre avec succès. Toutefois, les métadonnées, telles que les bases de données utilisateur, les connexions de serveur, des certificats de serveur, les clés de chiffrement, les travaux de l’agent SQL ou ancien mot de passe de connexion SA ne seront pas modifiée dans la nouvelle base de données master. Vous devez arrêter SQL Server et de déplacer votre ancien master.mdf mastlog.ldf vers le nouvel emplacement spécifié et de démarrage de SQL Server pour continuer à utiliser les métadonnées existantes. 


## <a id="masterdatabasename"></a> Modifier le nom des fichiers de base de données master.

Le **filelocation.masterdatafile** et **filelocation.masterlogfile** changements de paramètre de l’emplacement où le moteur SQL Server recherche les fichiers de base de données master. Par défaut, cet emplacement est /var/opt/mssql/data. Pour modifier ces paramètres, procédez comme suit :

1. Arrêtez le service SQL Server :

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Mssql-conf permet de modifier les noms de base de données master attendue pour les fichiers journaux et de données master avec le **définir** commande :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /var/opt/mssql/data/masternew.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.mastlogfile /var/opt/mssql/data /mastlognew.ldf
   ```

1. Modifier le nom des base de données master données et fichiers journaux 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /var/opt/mssql/data/masternew.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /var/opt/mssql/data/mastlognew.ldf
   ```

1. Démarrez le service SQL Server :

   ```bash
   sudo systemctl start mssql-server
   ```



## <a id="dumpdir"></a> Modifier l’emplacement par défaut du répertoire de vidage

Le **filelocation.defaultdumpdir** l’emplacement par défaut où la mémoire et les sauvegardes SQL sont générés chaque fois qu’un incident de changements de paramètre. Par défaut, ces fichiers sont générés dans /var/opt/mssql/log.

Pour configurer ce nouvel emplacement, utilisez les commandes suivantes :

1. Créer le répertoire cible pour les nouveaux fichiers de vidage. L’exemple suivant crée un nouveau **/tmp/dump** active :

   ```bash
   sudo mkdir /tmp/dump
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/dump
   sudo chgrp mssql /tmp/dump
   ```

1. Mssql-conf permet de modifier le répertoire de données par défaut avec le **définir** commande :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdumpdir /tmp/dump
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a id="errorlogdir"></a> Modifier l’emplacement de répertoire de fichier par défaut erreur journal

Le **filelocation.errorlogfile** changements de paramètre de l’emplacement où le nouveau journal des erreurs, trace du Générateur de profils par défaut, session de contrôle d’intégrité système XE et les fichiers de session XE Hekaton sont créés. Par défaut, cet emplacement est /var/opt/mssql/log. Le répertoire dans lequel le fichier journal des erreurs SQL est défini est le répertoire de journal par défaut pour les autres journaux.

Pour modifier ces paramètres :

1. Créer le répertoire cible pour les nouveaux fichiers journaux des erreurs. L’exemple suivant crée un nouveau **/tmp/logs** active :

   ```bash
   sudo mkdir /tmp/logs
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/logs
   sudo chgrp mssql /tmp/logs
   ```

1. Mssql-conf permet de modifier le nom de fichier du journal des erreurs par défaut avec le **définir** commande :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.errorlogfile /tmp/logs/errorlog
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```


## <a id="backupdir"></a> Modifier l’emplacement du répertoire de sauvegarde par défaut

Le **filelocation.defaultbackupdir** changements de paramètre de l’emplacement par défaut où les fichiers de sauvegarde sont générés. Par défaut, ces fichiers sont générés dans /var/opt/mssql/data.

Pour configurer ce nouvel emplacement, utilisez les commandes suivantes :

1. Créer le répertoire cible pour les nouveaux fichiers de sauvegarde. L’exemple suivant crée un nouveau **/tmp/de sauvegarde** active :

   ```bash
   sudo mkdir /tmp/backup
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/backup
   sudo chgrp mssql /tmp/backup
   ```

1. Mssql-conf permet de modifier le répertoire de sauvegarde par défaut avec la commande « set » :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultbackupdir /tmp/backup
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a id="coredump"></a> Spécifier les paramètres de vidage

Si une exception se produit dans un processus SQL Server, SQL Server crée une image mémoire.

Il existe deux options pour contrôler le type de mémoire vide que SQL Server collecte : **coredump.coredumptype** et **coredump.captureminiandfull**. Ils concernent les deux phases de capture de vidage. 

La première capture phase est contrôlée par le **coredump.coredumptype** paramètre qui détermine le type de fichier de vidage généré au cours d’une exception. La deuxième phase est activé lorsque la **coredump.captureminiandfull** paramètre. Si **coredump.captureminiandfull** est définie sur true, le vidage de fichier spécifié par **coredump.coredumptype** est généré et un deuxième vidage minimal est également généré. Paramètre **coredump.captureminiandfull** false désactive la capture de deuxième tentative.

1. Décidez s’il faut capturer les vidages minimaux et complètes avec le **coredump.captureminiandfull** paramètre.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.captureminiandfull <true or false>
    ```

    Valeur par défaut : **false**

1. Spécifiez le type de fichier dump avec le **coredump.coredumptype** paramètre.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.coredumptype <dump_type>
    ```

    Valeur par défaut : **miniplus**

    Le tableau suivant répertorie les possible **coredump.coredumptype** valeurs.

    | Type |  Description |
    |-----|-----|
    | **Mini** | Mini est le plus petit type de fichier de vidage. Il utilise les informations de système de Linux pour déterminer les threads et les modules dans le processus. L’image mémoire contient uniquement les modules et les piles de threads d’environnement hôte. Il ne contient pas de références de mémoire indirect ou des variables globales. |
    | **miniplus** | MiniPlus est similaire à mini, mais il inclut la mémoire supplémentaire. Il prend en charge les mécanismes internes de SQLPAL et de l’environnement hôte, en ajoutant les régions de mémoire suivantes pour le vidage :</br></br> -Globals divers</br> -Toute la mémoire au-dessus de 64 To</br> -All nommé trouvées dans les régions   **/proc / $pid/mappages**</br> -Mémoire indirecte des threads et des piles</br> -Informations sur le thread</br> -Associé de Teb et de Peb</br> -Informations module</br> -Arborescence VMM et VAD |
    | **Filtré** | Conception utilise filtré en fonction des soustraction où toute la mémoire dans le processus est incluse, sauf si spécifiquement exclus. La conception comprend les mécanismes internes de SQLPAL et de l’environnement hôte, à l’exception de certaines régions à partir de l’image mémoire.
    | **Complète** | Complète un vidage de processus complète qui inclut toutes les régions se trouve dans **/proc / $pid/mappages**. Cela n’est pas contrôlé par **coredump.captureminiandfull** paramètre. |

## <a id="dbmail"></a> Définir le profil de messagerie de base de données par défaut pour SQL Server sur Linux

Le **sqlpagent.databasemailprofile** vous permet de définir le profil de messagerie de base de données par défaut pour les alertes par courrier électronique.

```bash
sudo /opt/mssq/bin/mssql-conf set sqlagent.databasemailprofile <profile_name>
```
## <a id="hadr"></a> Haute disponibilité

Le **hadr.hadrenabled** option active les groupes de disponibilité sur votre instance de SQL Server. La commande suivante active les groupes de disponibilité en définissant **hadr.hadrenabled** à 1. Vous devez redémarrer SQL Server pour le paramètre prenne effet.

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
sudo systemctl restart mssql-server
```

Pour plus d’informations comment il est utilisé avec les groupes de disponibilité, consultez les deux rubriques suivantes.

- [Configurez toujours sur le groupe de disponibilité pour SQL Server sur Linux](sql-server-linux-availability-group-configure-ha.md)
- [Configurer le groupe de disponibilité de la lecture à l’échelle pour SQL Server sur Linux](sql-server-linux-availability-group-configure-rs.md)

## <a id="localaudit"></a> Définir un répertoire local d’audit

Le **telemetry.userrequestedlocalauditdirectory** paramètre active d’Audit Local et vous permet de définir le répertoire où les Local journaux d’Audit est créés.

1. Créez un répertoire cible pour les nouveaux journaux d’Audit Local. L’exemple suivant crée un nouveau **/tmp/audit** active :

   ```bash
   sudo mkdir /tmp/audit
   ```

1. Modifier le propriétaire et le groupe du répertoire à la **mssql** utilisateur :

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. Exécutez le script mssql-conf en tant que racine avec le **définir** commande **telemetry.userrequestedlocalauditdirectory**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

Pour plus d’informations, consultez [Feedback client pour SQL Server sur Linux](sql-server-linux-customer-feedback.md).

## <a id="lcid"></a> Modifier les paramètres régionaux de SQL Server

Le **language.lcid** définition modifie les paramètres régionaux de SQL Server à un identificateur de langue prise en charge (LCID). 

1. L’exemple suivant modifie les paramètres régionaux à Français (1036) :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set language.lcid 1036
   ```

1. Redémarrez le service SQL Server pour appliquer les modifications :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a id="memorylimit"></a> Définir la limite de mémoire

Le **memory.memorylimitmb** définition des contrôles la quantité mémoire physique (en Mo) disponible pour SQL Server. La valeur par défaut est 80 % de la mémoire physique.

1. Exécutez le script mssql-conf en tant que racine avec le **définir** commande **memory.memorylimitmb**. L’exemple suivant modifie la mémoire disponible pour SQL Server à 3,25 Go (3328 Mo).

   ```bash
   sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
   ```

1. Redémarrez le service SQL Server pour appliquer les modifications :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a id="tcpport"></a> Modifier le port TCP

Le **network.tcpport** modifications apportées au paramètre du port TCP sur lequel SQL Server écoute les connexions. Par défaut, ce port est défini sur 1433. Pour modifier le port, exécutez les commandes suivantes :

1. Exécutez le script de mssql-conf en tant que racine avec la commande « set » pour « network.tcpport » :

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.tcpport <new_tcp_port>
   ```

1. Redémarrez le service SQL Server :

   ```bash
   sudo systemctl restart mssql-server
   ```

1. Lors de la connexion à SQL Server maintenant, vous devez spécifier le port personnalisé avec une virgule (,) après le nom d’hôte ou l’adresse IP. Par exemple, pour vous connecter avec SQLCMD, vous utiliseriez la commande suivante :

   ```bash
   sqlcmd -S localhost,<new_tcp_port> -U test -P test
   ```

## <a id="tls"></a> Spécifier les paramètres de protocole TLS

Les options suivantes configurent TLS pour une instance de SQL Server est en cours d’exécution sur Linux.

|Option | Description |
|--- |--- |
|**Network.ForceEncryption** |La valeur 1, puis [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] force toutes les connexions à chiffrer. Par défaut, cette option est 0. |
|**network.tlscert** |Le chemin d’accès absolu pour le certificat du fichier qui [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise pour TLS. Exemple : `/etc/ssl/certs/mssql.pem` le fichier de certificat doit être accessible par le compte mssql. Microsoft vous recommande de restreindre l’accès au fichier à l’aide de `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlskey** |Le chemin d’accès absolu à la clé privée du fichier qui [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise pour TLS. Exemple : `/etc/ssl/private/mssql.key` le fichier de certificat doit être accessible par le compte mssql. Microsoft vous recommande de restreindre l’accès au fichier à l’aide de `chown mssql:mssql <file>; chmod 400 <file>`. |
|**Network.tlsprotocols** |Une liste séparée par des virgules de quels TLS protocoles sont autorisés par SQL Server. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tente toujours de négocier le protocole autorisé les plus fortes. Si un client ne prend pas en charge n’importe quel protocole autorisé, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] rejette la tentative de connexion.  Pour la compatibilité, tous les protocoles pris en charge sont autorisés par défaut (1.2, 1.1, 1.0).  Si vos clients prennent en charge TLS 1.2, Microsoft vous recommande d’autoriser uniquement TLS 1.2. |
|**network.tlsciphers** |Spécifie les chiffrements autorisés par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour TLS. Cette chaîne doit être mise en forme par [format de liste de chiffrement d’OpenSSL](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). En règle générale, vous ne devez pas modifier cette option. <br /> Par défaut, les chiffrements suivants sont autorisés : <br /> `ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA` |
| **network.kerberoskeytabfile** |Chemin d’accès au fichier keytab Kerberos |

Pour obtenir un exemple de l’aide des paramètres TLS, consultez [chiffrement des connexions à SQL Server sur Linux](sql-server-linux-encrypted-connections.md).

## <a id="traceflags"></a> Activer/désactiver traceflags

Cela **traceflag** option active ou désactive les traceflags pour le démarrage du service SQL Server. Pour activer/désactiver un indicateur de trace. Utilisez les commandes suivantes :

1. Activer un indicateur de trace à l’aide de la commande suivante. Par exemple, pour l’indicateur de trace 1234 :

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 on
   ```

1. Vous pouvez activer plusieurs traceflags en les spécifiant séparément :

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 2345 3456 on
   ```

1. De la même façon, vous pouvez désactiver une ou plusieurs traceflags activés en spécifiant et en ajoutant le **hors** paramètre :

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 2345 3456 off
   ```

1. Redémarrez le service SQL Server pour appliquer les modifications :

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="remove-a-setting"></a>Supprimer un paramètre

Pour annuler tout paramètre effectuées avec `mssql-conf set`, appelez **mssql-conf** avec la `unset` option et le nom du paramètre. Cela efface le paramètre retourner efficacement à sa valeur par défaut.

1. L’exemple suivant efface le **network.tcpport** option.

   ```bash
   sudo /opt/mssql/bin/mssql-conf unset network.tcpport
   ```

1. Redémarrez le service SQL Server.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="view-current-settings"></a>Afficher les paramètres actuels

Pour afficher les paramètres configurés, exécutez la commande suivante pour afficher le contenu de la **mssql.conf** fichier :

```bash
sudo cat /var/opt/mssql/mssql.conf
```

Notez que tous les paramètres ne pas indiqués dans ce fichier sont à l’aide de leurs valeurs par défaut. La section suivante fournit un exemple **mssql.conf** fichier.

## <a name="mssqlconf-format"></a>format de MSSQL.conf

Les éléments suivants **/var/opt/mssql/mssql.conf** fichier fournit un exemple de chaque paramètre. Vous pouvez utiliser ce format pour apporter manuellement des modifications à la **mssql.conf** de fichiers en fonction des besoins. Si vous modifier manuellement le fichier, vous devez redémarrer SQL Server avant d’appliquent les modifications. Pour utiliser le **mssql.conf** fichier avec Docker, vous devez disposer de Docker [conserver vos données](sql-server-linux-configure-docker.md). Tout d’abord ajouter un **mssql.conf** de fichiers à votre répertoire de l’hôte, puis exécutez le conteneur. Il existe un exemple dans [commentaires](sql-server-linux-customer-feedback.md).

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

## <a name="next-steps"></a>Étapes suivantes

Pour utiliser à la place des variables d’environnement pour effectuer certaines de ces modifications de configuration, consultez [des paramètres de configuration de SQL Server avec les variables d’environnement](sql-server-linux-configure-environment-variables.md).

Pour d’autres outils de gestion et les scénarios, consultez [gérer SQL Server sur Linux](sql-server-linux-management-overview.md).
