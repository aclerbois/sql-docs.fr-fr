---
title: Installation du Gestionnaire de pilote (ODBC Driver for SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 02/14/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Driver Manager, installing
ms.assetid: 7c4b6fb4-f45a-4973-adb9-a4d83f0a2a7a
caps.latest.revision: 59
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 18827f8e2e001700319d47516ba1694c384b26b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-the-driver-manager"></a>Installation du Gestionnaire de pilotes
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cet article contient des instructions pour installer le Gestionnaire de pilotes unixODBC pour une utilisation avec toutes les versions de Microsoft ODBC Driver for SQL Server sur Linux et macOS.  

> [!IMPORTANT]  
> Supprimez tous les packages de Gestionnaire de pilotes installés sur votre ordinateur avant d’installer le Gestionnaire de pilotes unixODBC. L’installation du Gestionnaire de pilotes unixODBC peut entraîner une défaillance d’un Gestionnaire de pilotes existant.  

## <a name="installing-the-driver-manager-for-microsoft-odbc-driver-13-131-and-17"></a>Installation du Gestionnaire de pilotes pour Microsoft ODBC Driver 13 et 17 13.1
La dépendance de gestionnaire de pilote est automatiquement résolue par le système de gestion de package lors de l’installation de Microsoft ODBC Driver 13, 13.1 ou 17 pour SQL Server sur Linux ou macOS en suivant les instructions de [l’installation du pilote ODBC de Microsoft pour SQL Server sur Linux ou macOS](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

## <a name="installing-the-driver-manager-for-microsoft-odbc-driver-11-for-sql-server"></a>Installation du Gestionnaire de pilotes pour Microsoft ODBC Driver 11 for SQL Server  

(SUSE et Red Hat Linux uniquement.)

**À l’aide du Script d’Installation**  
  
> [!IMPORTANT]  
> Ces instructions font référence à `msodbcsql-11.0.2270.0.tar.gz`, qui est le fichier d’installation pour Red Hat Linux. Si vous installez la version préliminaire pour SUSE Linux, le nom de fichier est `msodbcsql-11.0.2260.0.tar.gz`.  

Pour installer le Gestionnaire de pilotes  
  
1.  Assurez-vous de disposer d’une autorisation d’accès à la racine.  
  
2.  Accédez au répertoire où le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] téléchargement du pilote ODBC a placé le fichier nommé `msodbcsql-11.0.2270.0.tar.gz`. Assurez-vous de disposer du fichier \*.tar.gz correspondant à votre version de Linux. Pour extraire les fichiers, exécutez la commande suivante : **tar xvzf msodbcsql-11.0.2270.0.tar.gz**.  

3.  Remplacez par le `msodbcsql-11.0.2270.0` active et vous devez voir un fichier appelé `build_dm.sh`. Vous pouvez exécuter `build_dm.sh` pour installer le Gestionnaire de pilotes unixODBC.

4.  Pour afficher une liste des options disponibles, exécutez la commande suivante : **./build_dm.sh--aide**.  
  
5.  Lorsque vous êtes prêt à installer, et si votre ordinateur peut accéder à un site externe via FTP, exécutez la commande suivante : **./build_dm.sh**.

Si votre ordinateur ne peut pas accéder à un site externe via FTP, obtenez `unixODBC-2.3.0.tar.gz`. Vous pouvez obtenir `unixODBC-2.3.0.tar.gz` de [ http://www.unixodbc.org ](http://www.unixodbc.org/). Cliquez sur le **télécharger** lien sur le côté gauche de la page pour accéder à la page de téléchargement. Cliquez ensuite sur le lien approprié pour télécharger unixODBC-2.3.0 (et non unixODBC-2.3.1). UnixODBC-2.3.1 n’est pas pris en charge avec cette version de la [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)]. Exécutez la commande suivante pour commencer l’installation du Gestionnaire de pilotes unixODBC : **./build_dm.sh--url de téléchargement = file://unixODBC-2.3.0.tar.gz**.  

6.  Type **Oui** pour poursuivre la décompression des fichiers. Cette partie du processus peut prendre jusqu'à cinq minutes.  

7.  Une fois le script terminé, suivez les instructions à l’écran pour installer le Gestionnaire de pilotes unixODBC.

Vous êtes maintenant prêt à installer le pilote. Pour plus d’informations, consultez [l’installation de Microsoft ODBC Driver for SQL Server sur Linux et macOS](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).  

**Installation manuelle**

Si le script d’installation ne peut pas se terminer, configurez et générez le Gestionnaire de pilotes approprié vous-même.

1.  Supprimez toute version antérieure installée d’unixODBC (par exemple unixODBC 2.2.11). Sur Red Hat Enterprise Linux 5 ou 6, exécutez la commande suivante : **yum supprimer unixODBC**. Sur SUSE Linux Enterprise, **zypper supprimer unixODBC**.  
  
2.  Accédez à [ http://www.unixodbc.org ](http://www.unixodbc.org/). Cliquez sur le **télécharger** lien sur le côté gauche de la page pour accéder à la page de téléchargement. Cliquez ensuite sur le lien approprié pour enregistrer le fichier unixODBC-2.3.0.tar.gz sur votre ordinateur. UnixODBC-2.3.1 n’est pas pris en charge avec cette version de la [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
3.  Sur votre ordinateur Linux, exécutez la commande : **tar xvzf unixODBC-2.3.0.tar.gz**.  
  
4.  Accédez au répertoire unixODBC-2.3.0.  
  
5.  À l’invite de commandes, exécutez la commande : **CPPFLAGS = «-DSIZEOF_LONG_INT = 8 »**.  
  
6.  À l’invite de commandes, exécutez la commande : **exporter CPPFLAGS**.  
  
7.  À l’invite de commandes, exécutez la commande : **». / configurer--préfixe/usr =--libdir = / usr/lib64--sysconfdir =, etc.--enable-gui = no--enable-pilotes = no--enable-iconv--avec-iconv-char-enc = UTF8--avec-iconv-ucode-enc = UTF16LE »**.  
  
8.  À l’invite de commandes (connectée en tant que racine), exécutez la commande : **rendre**.  
  
9. À l’invite de commandes (connectée en tant que racine), exécutez la commande : **installer**.  

Vous êtes maintenant prêt à installer le pilote. Pour plus d’informations, consultez [l’installation de Microsoft ODBC Driver for SQL Server sur Linux et macOS](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi
[L’installation de Microsoft ODBC Driver for SQL Server sur Linux et Mac OS](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)

[Problèmes connus dans cette version du pilote](../../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)

[Notes de publication](../../../connect/odbc/linux-mac/release-notes.md)
