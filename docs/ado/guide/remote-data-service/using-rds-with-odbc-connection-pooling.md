---
title: À l’aide des services Bureau à distance avec une connexion ODBC mise en pool | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connection pooling in RDS [ADO]
ms.assetid: e8b912c1-da5b-4e85-a000-1e6648a94237
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d38a0d41ae5cdf0c1f40db21420fd39edca72237
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-rds-with-odbc-connection-pooling"></a>À l’aide des services Bureau à distance avec une connexion ODBC mise en pool
Si vous utilisez une source de données ODBC, vous pouvez utiliser l’option dans Internet Information Services (IIS) de regroupement de connexions pour bénéficier d’une gestion de la charge client hautes performances. Le regroupement de connexions est un gestionnaire de ressources pour les connexions, en conservant l’état ouvert sur les connexions fréquemment utilisées.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Pour activer le regroupement de connexions, reportez-vous à la documentation d’Internet Information Services.  
  
 Notez que ce qui permet le regroupement de connexions peut imposer le serveur Web d’autres restrictions, comme indiqué dans la documentation de Microsoft Internet Information Services.  
  
 Pour vous assurer que le regroupement de connexions est stable et offre des gains de performances supplémentaires, vous devez configurer Microsoft SQL Server pour utiliser la bibliothèque réseau de TCP/IP Socket.  
  
 Pour ce faire, vous devez :  
  
-   Configurez l’ordinateur SQL Server pour utiliser des Sockets TCP/IP.  
  
-   Configurer le serveur Web pour utiliser des Sockets TCP/IP.  
  
## <a name="configuring-the-sql-server-computer-to-use-tcpip-sockets"></a>Configuration de l’ordinateur SQL Server pour utiliser des Sockets TCP/IP  
 Sur l’ordinateur SQL Server, exécutez le programme d’installation de SQL Server afin que les interactions avec la source de données utilisent la bibliothèque réseau de sockets TCP/IP.  
  
### <a name="to-specify-the-tcpip-socket-network-library-on-the-sql-server-computer"></a>Pour spécifier la bibliothèque réseau de sockets TCP/IP sur l’ordinateur SQL Server  
  
### <a name="in-microsoft-sql-server-65"></a>Dans Microsoft SQL Server 6.5 :  
  
1.  Dans le menu Démarrer, pointez sur Programmes, pointez sur Microsoft SQL Server 6.5, puis cliquez sur le programme d’installation de SQL.  
  
2.  Cliquez deux fois sur Continuer.  
  
3.  Dans Microsoft SQL Server, boîte de dialogue Options, sélectionnez la prise en charge du réseau de modification, puis cliquez sur Continuer.  
  
4.  Assurez-vous que la case à cocher Sockets TCP/IP est sélectionné, puis cliquez sur OK.  
  
5.  Cliquez sur Continuer pour terminer et quitter le programme d’installation.  
  
### <a name="in-microsoft-sql-server-70"></a>Dans Microsoft SQL Server 7.0 :  
  
1.  Dans le menu Démarrer, pointez sur Programmes, pointez sur Microsoft SQL Server 7.0, puis cliquez sur l’utilitaire réseau serveur.  
  
2.  Sous l’onglet Général de la boîte de dialogue, cliquez sur Ajouter.  
  
3.  Dans la boîte de dialogue Ajouter une Configuration de bibliothèque réseau, cliquez sur TCP/IP.  
  
4.  Dans les zones d’adresse de Proxy et un numéro de Port, entrez l’adresse du proxy et le numéro de port fourni par votre administrateur réseau.  
  
5.  Cliquez sur OK pour terminer et quitter le programme d’installation.  
  
## <a name="configuring-the-web-server-to-use-tcpip-sockets"></a>Configuration du serveur Web pour utiliser des Sockets TCP/IP  
 Il existe deux options pour la configuration du serveur Web pour utiliser des Sockets TCP/IP. Opérations dépend de si tous les serveurs SQL sont accessibles à partir du serveur Web ou un serveur spécifique SQL Server est accessible à partir du serveur Web.  
  
 Si tous les serveurs SQL sont accessibles à partir du serveur Web, vous devez exécuter l’utilitaire de Configuration SQL Server Client sur l’ordinateur serveur Web. La procédure suivante modifie la bibliothèque réseau par défaut pour toutes les connexions SQL Server sont effectuées à partir de ce serveur Web IIS pour utiliser la bibliothèque réseau de Sockets TCP/IP.  
  
### <a name="to-configure-the-web-server-all-sql-servers"></a>Pour configurer le serveur Web (tous les serveurs SQL)  
  
### <a name="for-microsoft-sql-server-65"></a>Pour Microsoft SQL Server 6.5 :  
  
1.  Dans le menu Démarrer, pointez sur Programmes, pointez sur Microsoft SQL Server 6.5, puis cliquez sur utilitaire de Configuration du Client SQL.  
  
2.  Cliquez sur l’onglet de la Net-Library.  
  
3.  Dans la zone réseau par défaut, sélectionnez Sockets TCP/IP.  
  
4.  Cliquez sur Terminer pour enregistrer les modifications et quittez l’utilitaire.  
  
### <a name="for-microsoft-sql-server-70"></a>Pour Microsoft SQL Server 7.0 :  
  
1.  Dans le menu Démarrer, pointez sur Programmes, pointez sur Microsoft SQL Server 7.0, puis cliquez sur utilitaire réseau du Client.  
  
2.  Cliquez sur l’onglet Général.  
  
3.  Dans la bibliothèque réseau par défaut, cliquez sur TCP/IP.  
  
4.  Cliquez sur OK pour enregistrer les modifications et quittez l’utilitaire.  
  
 Si un serveur SQL spécifique est accessible à partir d’un serveur Web, vous devez exécuter l’utilitaire de Configuration SQL Server Client sur l’ordinateur serveur Web. Pour modifier la bibliothèque réseau pour une connexion SQL Server spécifique, vous devez configurer le logiciel Client de SQL Server sur l’ordinateur serveur Web comme suit.  
  
### <a name="to-configure-the-web-server-a-specific-sql-server"></a>Pour configurer le serveur Web (SQL Server spécifique)  
  
### <a name="for-microsoft-sql-server-65"></a>Pour Microsoft SQL Server 6.5 :  
  
1.  Dans le menu Démarrer, pointez sur Programmes, pointez sur Microsoft SQL Server 6.5, puis cliquez sur utilitaire de Configuration du Client SQL.  
  
2.  Cliquez sur l’onglet Avancé.  
  
3.  Dans la zone serveur, tapez le nom du serveur pour se connecter à l’aide de Sockets TCP/IP.  
  
4.  Dans la zone Nom de la DLL, sélectionnez Sockets TCP/IP.  
  
5.  Cliquez sur Ajouter/modifier. Toutes les sources de données pointant vers ce serveur utiliseront désormais les Sockets TCP/IP.  
  
6.  Cliquez sur terminé.  
  
### <a name="for-microsoft-sql-server-70"></a>Pour Microsoft SQL Server 7.0 :  
  
1.  Dans le menu Démarrer, pointez sur Programmes, pointez sur Microsoft SQL Server 7.0, puis cliquez sur utilitaire de Configuration du Client.  
  
2.  Cliquez sur l’onglet Général.  
  
3.  Cliquez sur Ajouter.  
  
4.  Entrez l’alias du serveur dans la zone alias du serveur. Dans la zone de bibliothèques réseau, cliquez sur TCP/IP. Dans la zone Nom d’ordinateur, entrez le nom de l’ordinateur qui écoute les clients de Sockets TCP/IP. Dans la zone numéro de Port, entrez le port sur lequel écoute le serveur SQL Server.  
  
5.  Cliquez sur OK, puis OK à nouveau pour quitter l’utilitaire.  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)






















