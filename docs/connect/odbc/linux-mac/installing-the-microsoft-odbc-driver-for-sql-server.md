---
title: L’installation de Microsoft ODBC Driver for SQL Server sur Linux et macOS | Documents Microsoft
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
caps.latest.revision: 69
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8b601c52db4dae0a7d103cee09ca231fc4d058d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-the-microsoft-odbc-driver-for-sql-server-on-linux-and-macos"></a>Installation de Microsoft ODBC Driver for SQL Server sur Linux et macOS
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cet article explique comment installer le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux et macOS, ainsi que les outils de ligne de commande facultatifs pour SQL Server (`bcp` et `sqlcmd`) et le développement en-têtes unixODBC.

## <a name="microsoft-odbc-driver-17-for-sql-server"></a>Pilote ODBC Microsoft 17 pour SQL Server 

> [!IMPORTANT]
> Si vous avez installé le v17 `msodbcsql` package qui a été brièvement disponible, vous devez le supprimer avant d’installer le `msodbcsql17` package. Cela permet d’éviter les conflits. Le `msodbcsql17` package peut être installé côte à côte avec la `msodbcsql` v13 package.

### <a name="debian-8-and-9"></a>Debian 8 et 9
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6-and-7"></a>RedHat Enterprise Server 6 et 7
```
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11sp4-and-12"></a>SUSE Linux Enterprise Server 11SP4 et 12

```
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
``` 

### <a name="ubuntu-1404-1604-and-1710"></a>Ubuntu 14.04, 16.04 et 17.10
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 14.04
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 17.10
curl https://packages.microsoft.com/config/ubuntu/17.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="os-x-1011-el-capitan-macos-1012-sierra-and-macos-1013-high-sierra"></a>OS X 10.11 (El Capitan), macOS 10.12 (Sierra) et macOS 10.13 (High Sierra)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install --no-sandbox msodbcsql17 mssql-tools
```

## <a name="microsoft-odbc-driver-131-for-sql-server"></a>Microsoft ODBC Driver 13.1 for SQL Server 

### <a name="debian-8"></a>Debian 8
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6
```
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7
```
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
``` 

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
``` 

### <a name="ubuntu-1510"></a>Ubuntu 15.10
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="os-x-1011-el-capitan-and-macos-1012-sierra"></a>OS X 10.11 (El Capitan) et macOS 10.12 (Sierra)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install --no-sandbox msodbcsql@13.1.9.2 mssql-tools@14.0.6.0
```

## <a name="microsoft-odbc-driver-13-for-sql-server"></a>Microsoft ODBC Driver 13 for SQL Server

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6
```
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7
```
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>Installation hors connexion
Si vous préférez/nécessitent la [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 pour être installé sur un ordinateur disposant d’aucune connexion internet, vous devrez résoudre manuellement les dépendances du package. La [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 présente les dépendances de directs suivantes :
- Ubuntu : libc6 (> = 2.21), libstdc ++ 6 (> = 4.9), 3-libkrb5, libcurl3, openssl, debconf (> = 0,5), unixodbc (> = 2.3.1-1)
- Red Hat : ```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SuSE : ```glibc, libuuid1, krb5, openssl, unixODBC```

Chacun de ces packages à son tour a leurs dépendances, ce qui peut ou peut ne pas exister sur le système. Pour une solution générale à ce problème, reportez-vous à la documentation du Gestionnaire de distribution de votre package : [Redhat](https://wiki.centos.org/HowTos/CreateLocalRepos), [Ubuntu](http://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian), et [SUSE](https://en.opensuse.org/Portal:Zypper)

Il est également courant de télécharger manuellement tous les packages dépendants et placez-les ensemble sur l’ordinateur d’installation, puis installez manuellement chaque package à son tour, fin avec la [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 package.

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7
  - Téléchargez la dernière version `msodbcsql` `.rpm` à partir d’ici : http://packages.microsoft.com/rhel/7/prod/
  - Les dépendances d’installation et le pilote
  
```
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04
- Téléchargez la dernière version `msodbcsql` `.deb` à partir d’ici : http://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/ 
- Les dépendances d’installation et le pilote 

```
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12
- Téléchargez la dernière version `msodbcsql` `.rpm` à partir d’ici : http://packages.microsoft.com/sles/12/prod/
- Installer les dépendances et le pilote

```
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

Une fois que vous avez terminé l’installation du package, vous pouvez vérifier que le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 pour trouver toutes ses dépendances, ldd en cours d’exécution, en inspectant sa sortie pour les bibliothèques manquants :
```
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```
  
## <a name="microsoft-odbc-driver-11-for-sql-server-on-linux"></a>Microsoft ODBC Driver 11 for SQL Server sur Linux

Avant de pouvoir utiliser le pilote, installez le Gestionnaire de pilotes unixODBC. Pour plus d’informations, consultez [Installing the Driver Manager](../../../connect/odbc/linux-mac/installing-the-driver-manager.md).

**Étapes d’installation**  

> [!IMPORTANT]  
> Ces instructions font référence à `msodbcsql-11.0.2270.0.tar.gz`, qui est le fichier d’installation pour Red Hat Linux. Si vous installez la version préliminaire pour SUSE Linux, le nom de fichier est `msodbcsql-11.0.2260.0.tar.gz`.  
  
Pour installer le pilote :

1.  Assurez-vous de disposer d’une autorisation d’accès à la racine.  

2.  Accédez au répertoire où le téléchargement a placé le fichier `msodbcsql-11.0.2270.0.tar.gz`. Assurez-vous de disposer du fichier \*.tar.gz correspondant à votre version de Linux. Pour extraire les fichiers, exécutez la commande suivante : `tar xvzf msodbcsql-11.0.2270.0.tar.gz`.  
  
3.  Remplacez par le `msodbcsql-11.0.2270.0` active et vous devez voir un fichier appelé **install.sh**.  
  
4.  Pour afficher la liste des options d’installation disponibles, exécutez la commande suivante : **./install.sh**.  
  
5.  Effectuez une sauvegarde de **odbcinst.ini**. L’installation du pilote met à jour **odbcinst.ini**. odbcinst.ini contient la liste des pilotes enregistrés auprès du Gestionnaire de pilotes unixODBC. Pour découvrir l’emplacement du fichier odbcinst.ini sur votre ordinateur, exécutez la commande suivante : ```odbc_config --odbcinstini```.  
  
6.  Avant d’installer le pilote, exécutez la commande suivante : `./install.sh verify`. La sortie de `./install.sh verify` signale si votre ordinateur dispose des logiciels requis pour prendre en charge le pilote ODBC sur Linux.  
  
7.  Lorsque vous êtes prêt à installer le pilote ODBC sur Linux, exécutez la commande : `./install.sh install`. Si vous devez spécifier une commande d’installation (`bin-dir` ou `lib-dir`), spécifiez la commande après le **installer** option.  
  
8.  Après avoir examiné le contrat de licence, tapez **YES** pour poursuivre l’installation.  
  
L’installation place le pilote dans `/opt/microsoft/msodbcsql/11.0.2270.0`. Le pilote et ses fichiers de prise en charge doivent être dans `/opt/microsoft/msodbcsql/11.0.2270.0`.  
  
Pour vérifier que le pilote Microsoft ODBC pour Linux a été inscrit correctement, exécutez la commande suivante : ```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```.  
  
La page de blog[Use Existing MSDN C++ ODBC Samples for the ODBC Driver on Linux](http://blogs.msdn.com/b/sqlblog/archive/2012/01/26/use-existing-msdn-c-odbc-samples-for-microsoft-linux-odbc-driver.aspx) présente un exemple de code qui se connecte à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] à l’aide du pilote ODBC sur Linux.  
  
**Désinstallation**  
  
Vous pouvez désinstaller le pilote ODBC 11 sur Linux en exécutant les commandes suivantes :  
  
1.  `rm -f /usr/bin/sqlcmd`
  
2.  `rm -f /usr/bin/bcp`  
  
3.  `rm -rf /opt/microsoft/msodbcsql`  
  
4.  `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="troubleshooting-connection-problems"></a>Résolution des problèmes de connexion  
Si vous ne parvenez pas à établir une connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] à l’aide du pilote ODBC, utilisez les informations suivantes pour identifier le problème.  
  
Le problème de connexion le plus courant est dû à l’installation de deux copies du Gestionnaire de pilotes UnixODBC. Recherchez /usr for libodbc\*.so\*. Si plusieurs versions du fichier apparaissent, vous avez (probablement) plusieurs gestionnaires de pilotes installés. Votre application risque alors d’utiliser la mauvaise version.
  
Activer le journal de connexion en modifiant votre `/etc/odbcinst.ini` fichier pour contenir la section suivante avec ces éléments :

```
[ODBC]
Trace = Yes
TraceFile = (path to log file, or /dev/stdout to output directly to the terminal)
```  
  
Si vous obtenez un autre échec de connexion et que vous ne voyez pas de fichier journal, il existe (peut-être) deux copies du Gestionnaire de pilotes sur votre ordinateur. Sinon, la sortie du journal doit ressembler à ce qui suit :  
  
```  
[ODBC][28783][1321576347.077780][SQLDriverConnectW.c][290]  
        Entry:  
            Connection = 0x17c858e0  
            Window Hdl = (nil)  
            Str In = [DRIVER={ODBC Driver 13 for SQL Server};SERVER={contoso.com};Trusted_Connection={YES};WSID={mydb.contoso.com};AP...][length = 139 (SQL_NTS)]  
            Str Out = (nil)  
            Str Out Max = 0  
            Str Out Ptr = (nil)  
            Completion = 0  
        UNICODE Using encoding ASCII 'UTF8' and UNICODE 'UTF16LE'  
```  
  
Si l’encodage de caractères ASCII n’est pas UTF-8, par exemple : 
  
```  
UNICODE Using encoding ASCII 'ISO8859-1' and UNICODE 'UCS-2LE'  
```  
  
Il existe plusieurs gestionnaires de pilotes installés et votre application est à l’aide du mauvais un, ou le Gestionnaire de pilotes n’a pas créé correctement.  
  
Pour plus d’informations sur la résolution des échecs de connexion, consultez :  
  
-   [Procédure de résolution des problèmes de connectivité SQL](http://blogs.msdn.com/b/sql_protocols/archive/2008/04/30/steps-to-troubleshoot-connectivity-issues.aspx)  
  
-   [Résolution des problèmes de connectivité SQL Server 2005 - 1re partie](http://blogs.msdn.com/b/sql_protocols/archive/2005/10/22/sql-server-2005-connectivity-issue-troubleshoot-part-i.aspx)  
  
-   [Résolution des problèmes de connectivité dans SQL Server 2008 avec le tampon en anneau de connectivité](http://blogs.msdn.com/b/sql_protocols/archive/2008/05/20/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer.aspx)  
  
-   [Résolution des problèmes d’authentification SQL Server](http://blogs.msdn.com/b/sqlsecurity/archive/2010/03/29/sql-server-authentication-troubleshooter.aspx)  
  
-   [Détails d’erreur)http://www.microsoft.com/products/ee/transform.aspx?ProdName=Microsoft+SQL+Server&EvtSrc=MSSQLServer&EvtID=11001)](http://www.microsoft.com/products/ee/transform.aspx?ProdName=Microsoft+SQL+Server&EvtSrc=MSSQLServer&EvtID=001)  
  
    Le numéro d’erreur spécifié dans l’URL (11001) doit être modifié pour correspondre à l’erreur que vous voyez.  
  
## <a name="driver-files"></a>Fichiers de pilote
Le pilote ODBC sur Linux et MacOS comprend les composants suivants :

### <a name="linux"></a>Linux

|Composant| Description|  
|---------------|-----------------|  
|libmsodbcsql-17. X.so.X.X ou libmsodbcsql-13. X.so.X.X|L’objet partagé (`so`) fichier de bibliothèque dynamique qui contient toutes les fonctionnalités du pilote. Ce fichier est installé dans `/opt/microsoft/msodbcsql17/lib64/` pour la version de pilote 17 et dans `/opt/microsoft/msodbcsql/lib64/` pour Driver 13.|  
|`msodbcsqlr17.rll`ou`msodbcsqlr13.rll`|Fichier de ressources qui accompagne la bibliothèque du pilote. Ce fichier est installé dans `[driver .so directory]../share/resources/en_US/`| 
|msodbcsql.h|Le fichier d’en-tête qui contient toutes les nouvelles définitions nécessaires à l’utilisation du pilote.<br /><br /> **Remarque :**  vous ne pouvez pas référencer msodbcsql.h et odbcss.h dans le même programme.<br /><br /> msodbcsql.h est installé dans `/opt/microsoft/msodbcsql17/include/` de 17 de pilote et de `/opt/microsoft/msodbcsql/include/` pour Driver 13. |
|Fichier LICENSE.txt|Le fichier texte qui contient les termes du contrat de licence utilisateur final. Ce fichier est placé dans `/usr/share/doc/msodbcsql17/` de 17 de pilote et de `/usr/share/doc/msodbcsql/` pour Driver 13.|
|RELEASE_NOTES|Le fichier texte qui contient les notes de publication. Ce fichier est placé dans `/usr/share/doc/msodbcsql17/` de 17 de pilote et de `/usr/share/doc/msodbcsql/` pour Driver 13.|


### <a name="macos"></a>MacOS

|Composant| Description|  
|---------------|-----------------|  
|libmsodbcsql.17.dylib ou libmsodbcsql.13.dylib|La bibliothèque dynamique (`dylib`) le fichier qui contient toutes les fonctionnalités du pilote. Ce fichier est installé dans `/usr/local/lib/`.|  
|`msodbcsqlr17.rll`ou`msodbcsqlr13.rll`|Fichier de ressources qui accompagne la bibliothèque du pilote. Ce fichier est installé dans `[driver .dylib directory]../share/msodbcsql17/resources/en_US/` de 17 de pilote et de `[driver .dylib directory]../share/msodbcsql/resources/en_US/` pour Driver 13. | 
|msodbcsql.h|Le fichier d’en-tête qui contient toutes les nouvelles définitions nécessaires à l’utilisation du pilote.<br /><br /> **Remarque :**  vous ne pouvez pas référencer msodbcsql.h et odbcss.h dans le même programme.<br /><br /> msodbcsql.h est installé dans `/usr/local/include/msodbcsql17/` de 17 de pilote et de `/usr/local/include/msodbcsql/` pour Driver 13. |
|Fichier LICENSE.txt|Le fichier texte qui contient les termes du contrat de licence utilisateur final. Ce fichier est placé dans `/usr/local/share/doc/msodbcsql17/` de 17 de pilote et de `/usr/local/share/doc/msodbcsql/` pour Driver 13. |
|RELEASE_NOTES|Le fichier texte qui contient les notes de publication. Ce fichier est placé dans `/usr/local/share/doc/msodbcsql17/` de 17 de pilote et de `/usr/local/share/doc/msodbcsql/` pour Driver 13. |

## <a name="resource-file-loading"></a>Chargement du fichier de ressources

Le pilote doit charger le fichier de ressources pour pouvoir fonctionner. Ce fichier est appelé `msodbcsqlr17.rll` ou `msodbcsqlr13.rll` selon la version du pilote. L’emplacement de la `.rll` fichier est relatif à l’emplacement du pilote lui-même (`so` ou `dylib`), comme indiqué dans le tableau ci-dessus. À compter de la version 17.1 le pilote tente également de charger le `.rll` à partir de la valeur par défaut active si le chargement à partir du chemin d’accès relatif échoue. Les chemins d’accès du fichier de ressources par défaut sont :

Linux : `/opt/microsoft/msodbcsql17/share/resources/en_US/`

macOS : `/usr/local/share/msodbcsql17/resources/en_US/`


  
## <a name="see-also"></a>Voir aussi

[Installation du Gestionnaire de pilotes](../../../connect/odbc/linux-mac/installing-the-driver-manager.md)

[Notes de publication](../../../connect/odbc/linux-mac/release-notes.md)

[Configuration système requise](../../../connect/odbc/linux-mac/system-requirements.md)
