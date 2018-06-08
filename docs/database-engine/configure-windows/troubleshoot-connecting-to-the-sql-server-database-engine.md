---
title: Résoudre les problèmes de connexion au moteur de base de données SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 02/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting, connecting to Database Engine
- connecting to Database Engine, troubleshooting
ms.assetid: 474c365b-c451-4b07-b636-1653439f4b1f
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: fda5188298c2cae3b56bdb4119ae1bbc96679a2f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-connecting-to-the-sql-server-database-engine"></a>Résoudre les problèmes de connexion au moteur de base de données SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Il s’agit d’une liste exhaustive de techniques de dépannage à utiliser quand vous ne pouvez pas vous connecter au moteur de base de données SQL Server. Ces étapes ne sont pas dans l’ordre des problèmes les plus probables que vous connaissez peut-être déjà. Ces étapes sont classées dans l’ordre des problèmes les plus simples aux plus complexes. Ces étapes supposent que vous vous connectez à SQL Server à partir d’un autre ordinateur à l’aide du protocole TCP/IP, ce qui est le cas le plus courant. Ces étapes concernent SQL Server 2016 avec les applications clientes et SQL Server exécutant Windows 10 ; cependant les étapes s’appliquent généralement à d’autres versions de SQL Server et d’autres systèmes d’exploitation avec seulement quelques modifications.

Ces instructions sont particulièrement utiles lors de la résolution de l’erreur de «**connexion au serveur**», qui peut être le numéro d’erreur 11001 (ou 53), gravité : 20, état : 0, et des messages d’erreur tels que :

*   « Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server. Le serveur est introuvable ou n'est pas accessible. Vérifiez que le nom de l'instance est correct et que SQL Server est configuré pour autoriser les connexions distantes. " 

*   « (fournisseur : Fournisseur de canaux nommés, erreur : 40 - Impossible d’ouvrir une connexion à SQL Server) (Microsoft SQL Server, erreur : 53) » ou « (fournisseur : Fournisseur TCP, erreur : 0 - Hôte inconnu.) (Microsoft SQL Server, erreur : 11001) » 

Cette erreur signifie généralement que l’ordinateur SQL Server est introuvable ou que le numéro de port TCP est inconnu, n’est pas correct ou est bloqué par un pare-feu.

>  [!TIP]
>  Une page de résolution interactive est disponible à partir des services de support technique de [!INCLUDE[msCoName_md](../../includes/msconame-md.md)] sur [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server).

### <a name="not-included"></a>Non inclus

* Cette rubrique n’inclut pas d’informations sur les erreurs SSPI. Pour plus d’informations sur les erreurs SSPI, consultez [Comment faire pour résoudre le message d’erreur « Impossible de générer le contexte SSPI »](http://support.microsoft.com/kb/811889).  
* Cette rubrique n’inclut pas d’informations sur les erreurs Kerberos. Pour obtenir de l’aide, consultez [Microsoft Kerberos Configuration Manager pour SQL Server](http://www.microsoft.com/download/details.aspx?id=39046).
* Cette rubrique n’inclut pas d’informations sur la connectivité SQL Azure. Pour obtenir de l’aide, consultez [Dépannage des problèmes de connectivité avec Microsoft Azure SQL Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database). 

## <a name="gathering-information-about-the-instance-of-sql-server"></a>Collecte des informations sur l’instance de SQL Server

Tout d’abord, vous devez collecter des informations de base sur le moteur de base de données.

1. Vérifiez que l’instance du moteur de base de données SQL Server est installée et en cours d’exécution.
    1.  Ouvrez une session sur l’ordinateur qui héberge l’instance de SQL Server.
    2.  Démarrez le Gestionnaire de configuration SQL Server. (Le Gestionnaire de configuration est automatiquement installé sur l’ordinateur quand SQL Server est installé. Les instructions sur le démarrage du Gestionnaire de configuration varient légèrement selon la version de SQL Server et Windows. Pour obtenir de l’aide sur le démarrage du Gestionnaire de configuration, consultez [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).)
    3.  En utilisant le Gestionnaire de configuration, dans le volet gauche, sélectionnez **Services SQL Server**. Dans le volet droit, vérifiez que l’instance du moteur de base de données est présente et en cours d’exécution. Une instance nommée **SQL Server (MSSQLSERVER)** est une instance par défaut (sans nom). Il ne peut exister qu’une seule instance par défaut. Les autres instances (nommées) auront leurs noms répertoriés entre parenthèses. SQL Server Express utilise le nom **SQL Server (SQLEXPRESS)** comme nom d’instance, sauf si un utilisateur a choisi un autre nom pendant l’installation. Notez le nom de l’instance à laquelle vous essayez de vous connecter. En outre, vérifiez que l’instance est en cours d’exécution, en recherchant la flèche verte. Si l’instance présente un carré rouge, cliquez avec le bouton droit sur l’instance, puis cliquez sur **Démarrer**. La couleur doit être verte.
     4. Si vous essayez de vous connecter à une instance nommée, vérifiez que le service SQL Server Browser est en cours d’exécution.
 

2.  Obtenez l’adresse IP de l’ordinateur.
    1. Dans le menu Démarrer, cliquez sur **Exécuter**. Dans la fenêtre **Exécuter** , tapez **cmd**, puis cliquez sur **OK**.
    2.  Dans la fenêtre d’invite de commandes, tapez **ipconfig** et appuyez sur Entrée. Notez l’adresse **IPv4** et l’adresse **IPv6** . (SQL Server peut se connecter à l’aide de l’ancien protocole IP version 4 ou du plus récent protocole IP version 6. Votre réseau peut autoriser l’un des deux, ou les deux. La plupart des personnes commencent par la résolution des problèmes de l’adresse **IPv4** . Elle est plus courte et plus simple à taper.)


3.  Obtenez le numéro de port TCP utilisé par SQL Server. Dans la plupart des cas, vous vous connectez au moteur de base de données à partir d’un autre ordinateur en utilisant le protocole TCP.
    1.  En utilisant SQL Server Management Studio sur l’ordinateur exécutant SQL Server, connectez-vous à l’instance de SQL Server. Dans l’Explorateur d’objets, développez **Gestion**, **Journaux SQL Server**, puis double-cliquez sur le journal actuel.
    2.  Dans la visionneuse du journal, cliquez sur le bouton **Filtrer** dans la barre d’outils. Dans la zone **Le message contient du texte** , tapez **Le serveur écoute sur**, cliquez sur **Appliquer le filtre**, puis sur **OK**.
    3.  Un message similaire à **Le serveur écoute sur [ ’tout’ \<ipv4> 1433]** doit s’afficher. Ce message indique que cette instance de SQL Server écoute sur toutes les adresses IP de cet ordinateur (pour le protocole IP version 4) et écoute le port TCP 1433. (Le port TCP 1433 est généralement le port utilisé par le moteur de base de données. Une seule instance de SQL Server pouvant utiliser un port, certaines instances doivent utiliser d’autres numéros de port si plusieurs instances de SQL Server sont installées.) Notez le numéro de port utilisé par l’instance de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] auquel vous essayez de vous connecter. 

    >    [!NOTE] 
    >    L’adresse IP 127.0.0.1 est probablement répertoriée. Elle est appelée adresse de carte de bouclage et ne peut être connectée qu’à des processus sur le même ordinateur. Elle peut être utile pour la résolution des problèmes, mais vous ne pouvez pas l’utiliser pour vous connecter à partir d’un autre ordinateur.

## <a name="enable-protocols"></a>Activer les protocoles

Dans certaines installations de SQL Server, la connexion au moteur de base de données à partir d’un autre ordinateur est désactivée, sauf si un administrateur utilise le Gestionnaire de configuration pour l’activer. Pour activer des connexions à partir d’un autre ordinateur :

1.  Ouvrez le Gestionnaire de configuration SQL Server, comme décrit précédemment. 
2.  En utilisant le Gestionnaire de configuration, dans le volet gauche, développez **Configuration du réseau SQL Server**, puis sélectionnez l’instance de SQL Server à laquelle vous voulez vous connecter. Le volet droit répertorie les protocoles de connexion disponibles. La mémoire partagée est normalement activée. Comme elle peut uniquement être utilisée à partir du même ordinateur, la plupart des installations laissent la mémoire partagée activée. Pour vous connecter à SQL Server à partir d’un autre ordinateur, vous allez normalement utiliser TCP/IP. Si TCP/IP n’est pas activé, cliquez avec le bouton droit sur **TCP/IP**, puis cliquez sur **Activer**. 
3.  Si vous avez modifié le paramètre activé pour n’importe quel protocole, vous devez redémarrer le moteur de base de données. Dans le volet gauche, sélectionnez **Services SQL Server**. Dans le volet droit, cliquez avec le bouton droit sur l’instance du moteur de base de données, puis cliquez sur **Redémarrer**. 

## <a name="testing-tcpip-connectivity"></a>Test de la connectivité TCP/IP

La connexion à SQL Server à l’aide de TCP/IP exige que Windows puisse établir la connexion. Utilisez l’outil `ping` pour tester TCP.
1.  Dans le menu Démarrer, cliquez sur **Exécuter**. Dans la fenêtre **Exécuter** , tapez **cmd**, puis cliquez sur **OK**. 
2.  Dans la fenêtre d’invite de commandes, tapez `ping` , puis l’adresse IP de l’ordinateur qui exécute SQL Server. Par exemple, `ping 192.168.1.101` avec une adresse IPv4 ou `ping fe80::d51d:5ab5:6f09:8f48%11` avec une adresse IPv6. (Vous devez remplacer les nombres après ping par les adresses IP sur votre ordinateur que vous avez collectées précédemment.) 
3.  Si votre réseau est correctement configuré, vous recevrez une réponse comme **Réponse de \<adresse IP>** suivie d’autres informations. Si vous recevez une erreur telle que **Impossible de joindre l’hôte de destination.** ou **Délai d’attente de la demande dépassé.**, cela signifie que TCP/IP n’est pas configuré correctement. (Vérifiez que l’adresse IP était exacte et correctement tapée.) Des erreurs à ce stade peuvent indiquer un problème avec l’ordinateur client, l’ordinateur serveur, ou relatif au réseau (par exemple, un routeur). Internet a de nombreuses ressources pour la résolution des problèmes TCP/IP. Il peut être judicieux de commencer par cet article de 2006, [Résolution des problèmes liés au TCP/IP](http://support.microsoft.com/kb/169790).
4.  Ensuite, si le test ping a réussi à l’aide de l’adresse IP, vérifiez que le nom de l’ordinateur peut être résolu en adresse TCP/IP. Sur l’ordinateur client, dans la fenêtre d’invite de commandes, tapez `ping` , puis le nom de l’ordinateur qui exécute SQL Server. Par exemple, `ping newofficepc` 
5.  Si vous pouvez tester l’adresse IP, mais que vous recevez à présent une erreur telle que **Impossible de joindre l’hôte de destination.** ou **Délai d’attente de la demande dépassé.**, vous avez peut-être des informations de résolution de noms anciennes (obsolètes) mises en cache sur l’ordinateur client. Tapez `ipconfig /flushdns` pour effacer le cache DNS (résolution de noms dynamique). Réexécutez ensuite une commande ping sur l’ordinateur selon le nom. Comme le cache DNS est vide, l’ordinateur client recherche les informations les plus récentes sur l’adresse IP pour l’ordinateur serveur. 
6.  Si votre réseau est correctement configuré, vous recevrez une réponse comme **Réponse de \<adresse IP>** suivie d’autres informations. Si vous pouvez correctement exécuter une commande ping sur l’ordinateur serveur selon l’adresse IP, mais recevez une erreur telle que **Impossible de joindre l’hôte de destination.** ou **Délai d’attente de la demande dépassé.** en exécutant la commande ping selon le nom de l’ordinateur, la résolution de noms n’est pas correctement configurée. (Pour plus d’informations, consultez l’article de 2006 précédemment mentionné, [Résolution des problèmes liés au TCP/IP](http://support.microsoft.com/kb/169790).) Une résolution correcte de noms n’est pas obligatoire pour se connecter à SQL Server mais, si le nom d’ordinateur ne peut pas résolu en adresse IP, les connexions doivent être établies en spécifiant l’adresse IP. Ce n’est pas idéal, mais le problème de résolution de noms peut être corrigé plus tard.
  
  
## <a name="testing-a-local-connection"></a>Test d’une connexion locale

Avant de résoudre un problème de connexion à partir d’un autre ordinateur, commencez par tester votre capacité à vous connecter à partir d’une application cliente installée sur l’ordinateur qui exécute SQL Server. (Cela permet d’éviter les problèmes liés au pare-feu.) Cette procédure utilise SQL Server Management Studio. Si Management Studio n’est pas installé, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md). (Si vous n’êtes pas en mesure d’installer Management Studio, vous pouvez tester la connexion en utilisant l’utilitaire `sqlcmd.exe` qui est installé avec le moteur de base de données. Pour plus d’informations sur `sqlcmd.exe`, consultez [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md).)
1.  Ouvrez une session sur l’ordinateur où SQL Server est installé à l’aide d’une connexion qui a l’autorisation d’accéder à SQL Server. (Lors de l’installation, SQL Server nécessite au moins une connexion à spécifier en tant qu’administrateur SQL Server. Si vous ne connaissez pas d’administrateur, consultez [Se connecter à SQL Server lorsque les administrateurs système n’y ont plus accès](http://msdn.microsoft.com/library/dd207004.aspx).)
2.   Dans la page de démarrage, tapez **SQL Server Management Studio**ou, sur des versions plus anciennes de Windows, dans le menu Démarrer, pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, puis cliquez sur **SQL Server Management Studio**.
3.  Dans la zone **Type de serveur** de la boîte de dialogue **Se connecter au serveur** , sélectionnez **Moteur de base de données**. Dans la zone **Authentification** , sélectionnez **Authentification Windows**. Dans la zone **Nom du serveur** , tapez l’un des éléments suivants :

|Connexion à :|Type :|Exemple :|
|-----------------|---------------|-----------------|
|Instance par défaut|Nom de l’ordinateur|ACCNT27|
|Instance nommée|Nom de l’ordinateur\nom de l’instance|ACCNT27\PAYROLL|

>  [!NOTE] 
>  Lorsque vous vous connectez à un serveur SQL Server à partir d’une application cliente sur le même ordinateur, le protocole de mémoire partagée est utilisé. La mémoire partagée étant un type de canal nommé local, des erreurs relatives aux canaux se produisent parfois.

Si vous recevez une erreur à ce stade, vous devez la résoudre avant de continuer. Il existe de nombreuses sources de problèmes possibles. Votre connexion peut être refusée. Votre base de données par défaut peut-être absente.

>    [!NOTE] 
>    Certains messages d’erreur transmis au client intentionnellement ne donnent pas suffisamment d’informations pour résoudre le problème. Il s’agit d’une fonctionnalité de sécurité pour éviter de fournir à un attaquant des informations sur SQL Server. Pour afficher les informations complètes sur l’erreur, consultez le journal des erreurs SQL Server. Les détails y sont fournis. Si vous recevez l’erreur **18456 Échec de la connexion pour l’utilisateur**, la rubrique de la documentation en ligne [MSSQLSERVER_18456](http://msdn.microsoft.com/library/cc645917) contient des informations supplémentaires sur les codes d’erreur. En outre, le blog d’Aaron Bertrand comprend une liste très complète des codes d’erreur à la page [Troubleshooting Error 18456](http://www2.sqlblog.com/blogs/aaron_bertrand/archive/2011/01/14/sql-server-v-next-denali-additional-states-for-error-18456.aspx). Vous pouvez afficher le journal des erreurs avec SSMS (si vous pouvez vous connecter), dans la section Gestion de l’Explorateur d’objets. Sinon, vous pouvez afficher le journal des erreurs avec le programme Bloc-notes de Windows. L’emplacement par défaut varie en fonction de votre version et peut être modifié pendant l’installation. L’emplacement par défaut pour [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] est `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log\ERRORLOG`.  

4.   Si vous pouvez vous connecter à l’aide de la mémoire partagée, testez la connexion à l’aide de TCP. Vous pouvez forcer une connexion TCP en spécifiant **tcp:** avant le nom. Exemple :

|Connexion à :|Type :|Exemple :|
|-----------------|---------------|-----------------|
|Instance par défaut|tcp:Nom de l’ordinateur|tcp:ACCNT27|
|Instance nommée|tcp: Nom de l’ordinateur\nom de l’instance|tcp:ACCNT27\PAYROLL|
  
Si vous pouvez vous connecter avec la mémoire partagée mais pas avec TCP, vous devez corriger le problème TCP. La cause la plus probable est que TCP n’est pas activé. Pour activer TCP, consultez la procédure **Activer les protocoles** ci-dessus.

5.   Si votre objectif est de vous connecter avec un compte autre qu’un compte d’administrateur, une fois que vous pouvez vous connecter en tant qu’administrateur, essayez à nouveau la connexion d’authentification Windows ou la connexion d’authentification SQL Server qui sera utilisée par l’application cliente.

## <a name="opening-a-port-in-the-firewall"></a>Ouverture d’un port dans le pare-feu

Depuis de nombreuses années, avec Windows XP Service Pack 2, le Pare-feu Windows est activé et bloque les connexions à partir d’un autre ordinateur. Pour vous connecter à l’aide de TCP/IP à partir d’un autre ordinateur, vous devez configurer le pare-feu sur l’ordinateur SQL Server pour autoriser les connexions au port TCP utilisé par le moteur de base de données. Comme mentionné précédemment, l’instance par défaut est généralement à l’écoute du port TCP 1433. Si vous avez des instances nommées, ou si vous avez modifié la valeur par défaut, le port TCP [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] peut être à l’écoute sur un autre port. Consultez la section du début portant sur la collecte d’informations pour déterminer votre port.  
Si vous vous connectez à une instance nommée ou un port autre que le port TCP 1433, vous devez également ouvrir le port UDP 1434 pour le service SQL Server Browser. Pour obtenir des instructions détaillées sur l’ouverture d’un port dans le Pare-feu Windows, consultez [Configurer un pare-feu Windows pour accéder au moteur de base de données](https://msdn.microsoft.com/library/ms175043).

## <a name="testing-the-connection"></a>Test de la connexion

Une fois que vous pouvez vous connecter à l’aide de TCP sur le même ordinateur, il est temps de tester la connexion à partir de l’ordinateur client. Vous pouvez théoriquement utiliser toute application cliente, mais pour éviter une complexité supplémentaire, installez les outils d’administration SQL Server sur le client et tentez l’opération à l’aide de SQL Server Management Studio.

1. Sur l’ordinateur client, avec SQL Server Management Studio, essayez de vous connecter à l’aide de l’adresse IP et du numéro de port TCP au format adresse IP virgule numéro de port. Par exemple, **192.168.1.101,1433** . Si cela ne fonctionne pas, vous êtes probablement confronté à l’un des problèmes suivants :

    * La commande**Ping** sur l’adresse IP ne fonctionne pas, ce qui indique un problème de configuration TCP d’ordre général. Revenez à la section **Test de la connectivité TCP/IP**. 
    *   SQL Server n’écoute pas sur le protocole TCP. Revenez à la section **Activer les protocoles**. 
    *   SQL Server est à l’écoute sur un port autre que le port spécifié. Revenez à la section **Collecte des informations sur l’instance de SQL Server**. 
    *   Le port TCP SQL Server est bloqué par le pare-feu. Revenez à la section **Ouverture d’un port dans le pare-feu**.


2. Une fois que vous pouvez vous connecter à l’aide de l’adresse IP et du numéro de port, essayez de vous connecter à l’aide de l’adresse IP sans numéro de port. Pour une instance par défaut, utilisez simplement l’adresse IP. Pour une instance nommée, utilisez l’adresse IP et le nom de l’instance au format adresse IP barre oblique inverse nom de l’instance, par exemple **192.168.1.101\PAYROLL** . Si cela ne fonctionne pas, vous êtes probablement confronté à l’un des problèmes suivants :

    *   Si vous vous connectez à l’instance par défaut, elle peut être à l’écoute sur un port autre que le port TCP 1433 et le client ne tente pas de se connecter au numéro de port correct.
    *   Si vous vous connectez à une instance nommée, le numéro de port n’est pas retourné au client.  

Ces deux problèmes sont liés au service SQL Server Browser, qui fournit le numéro de port au client. Les solutions sont les suivantes :

  * Démarrez le service SQL Server Browser. Revenez à la section **Collecte des informations sur l’instance de SQL Server**, section 1.d.
  * Le service SQL Server Browser est bloqué par le pare-feu. Ouvrez le port UDP 1434 dans le pare-feu. Revenez à la section **Ouverture d’un port dans le pare-feu**. (Assurez-vous ouvrez un port UDP, et non un port TCP. Ces derniers sont différents.)
  * Les informations du port UDP 1434 sont bloquées par un routeur. Les communications UDP (protocole UDP) ne sont pas conçues pour passer par des routeurs. Cela empêche le réseau d’être rempli par un trafic de faible priorité. Vous pouvez peut-être configurer votre routeur pour transférer le trafic UDP, ou vous pouvez choisir de toujours fournir le numéro de port quand vous vous connectez.
  * Si l’ordinateur client utilise Windows 7 ou Windows Server 2008 (ou un système d’exploitation plus récent), le trafic UDP peut être supprimé par le système d’exploitation client, car la réponse du serveur est retournée à partir d’une adresse IP différente de celle de la requête. Il s’agit d’une fonctionnalité de sécurité qui bloque le « mappage de source libre ». Pour plus d’informations, consultez la section **Adresses IP de serveurs multiples** de la rubrique de la documentation en ligne [Dépannage : expiration du délai d’attente](http://msdn.microsoft.com/library/ms190181.aspx). Il s’agit d’un article de SQL Server 2008 R2, mais les principes s’appliquent toujours. Vous pouvez peut-être configurer le client pour utiliser l’adresse IP correcte, ou vous pouvez choisir de toujours fournir le numéro de port quand vous vous connectez.
     
3. Une fois que vous pouvez vous connecter à l’aide de l’adresse IP (ou de l’adresse IP et du nom de l’instance pour une instance nommée), essayez de vous connecter en utilisant le nom de l’ordinateur (ou le nom de l’ordinateur et le nom de l’instance pour une instance nommée). Placez `tcp:` devant le nom de l’ordinateur pour forcer une connexion TCP/IP. Par exemple, pour l’instance par défaut sur un ordinateur nommé `ACCNT27`utilisez `tcp:ACCNT27` . Pour une instance nommée appelée `PAYROLL`sur cet ordinateur, utilisez `tcp:ACCNT27\PAYROLL` . Si vous pouvez vous connecter à l’aide de l’adresse IP, mais pas avec le nom de l’ordinateur, vous avez un problème de résolution de noms. Revenez à la section **Test de la connectivité TCP/IP**, section 4.

4. Une fois que vous pouvez vous connecter en utilisant le nom de l’ordinateur et en forçant une connexion TCP, tentez de vous connecter en utilisant le nom de l’ordinateur, mais sans forcer la connexion TCP. Par exemple, pour une instance par défaut, utilisez simplement le nom de l’ordinateur comme `CCNT27` . Pour une instance nommée, utilisez le nom de l’ordinateur et le nom de l’instance comme `ACCNT27\PAYROLL` . Si vous pouvez vous connecter en forçant une connexion TCP, mais pas sans forcer la connexion TCP, le client utilise probablement un autre protocole (par exemple, des canaux nommés).

    1. Sur l’ordinateur client, à l’aide du Gestionnaire de configuration SQL Server, dans le volet gauche développez **Configuration** *version* **Configuration**, and then select **Client Protocols**.
    2. Dans le volet droit, vérifiez que TCP/IP est activé. Si TCP/IP est désactivé, cliquez avec le bouton droit sur **TCP/IP** , puis cliquez sur **Activer**.
    3. Vérifiez que l’ordre des protocoles pour TCP/IP est un nombre inférieur aux protocoles de canaux nommés (ou VIA sur des versions plus anciennes). En règle générale, vous devez laisser la mémoire partagée en 1 et TCP/IP en 2. La mémoire partagée est utilisée uniquement quand le client et SQL Server sont en cours d’exécution sur le même ordinateur. Tous les protocoles activés sont essayés dans l’ordre jusqu’à ce que l’un d’eux réussisse, sauf que la mémoire partagée est ignorée quand la connexion n’est pas sur le même ordinateur. 

