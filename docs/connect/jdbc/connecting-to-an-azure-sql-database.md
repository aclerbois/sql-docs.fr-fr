---
title: Connexion à une base de données SQL Azure | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 49645b1f-39b1-4757-bda1-c51ebc375c34
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 183cd9c749cac8b1af3d97a9830d4d4288fd464f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-an-azure-sql-database"></a>Connexion à une base de données SQL Azure
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cette rubrique traite des problèmes lors de l’utilisation du [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] pour se connecter à un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]. Pour plus d’informations sur la connexion à un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], consultez :  
  
-   [SQL Azure Database](http://go.microsoft.com/fwlink/?LinkID=202490)  
  
-   [Comment : se connecter à SQL Azure à l’aide de JDBC](http://msdn.microsoft.com/library/gg715284.aspx)  
  
-   [Utilisation de SQL Azure dans Java](http://msdn.microsoft.com/library/windowsazure/hh749029(VS.103).aspx)

-   [Connexion avec l’authentification Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md)  
  
## <a name="details"></a>Détails  
 Lors de la connexion à un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], vous devez vous connecter à la base de données master pour appeler **SQLServerDatabaseMetaData.getCatalogs**.  
 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne prend pas en charge le retour de l’ensemble des catalogues à partir d’une base de données utilisateur. **SQLServerDatabaseMetaData.getCatalogs** utilise la vue sys.databases pour récupérer les catalogues. Reportez-vous à la discussion des autorisations de [sys.databases (base de données SQL Azure)](http://go.microsoft.com/fwlink/?LinkId=217396) pour comprendre **SQLServerDatabaseMetaData.getCatalogs** comportement sur un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)].  
  
 Connexions supprimées  
 Lors de la connexion à un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], connexions inactives peuvent être arrêtées par un composant réseau (tel qu’un pare-feu) après une période d’inactivité. Il existe deux types de connexions inactives dans ce contexte :  
  
-   Les connexions inactives sur la couche TCP, lorsque les connexions peuvent être supprimées par n'importe quel dispositif réseau.  
  
-   Inactives via la passerelle SQL Azure, où TCP **keepalive** rendent peuvent se produire (établissement de la connexion non inactive du point de vue TCP), mais sans requête active dans les 30 minutes. Dans ce scenario, la passerelle détermine que la connexion TDS est inactive à 30 minutes et l'arrête.  
  
 Pour éviter la suppression des connexions inactives par un composant réseau, les paramètres de Registre suivants (ou leurs équivalents non Windows) doivent être définis sur le système d'exploitation dans lequel le pilote est chargé&nbsp;:  
  
|Paramètre de Registre|Valeur recommandée|  
|----------------------|-----------------------|  
|HKEY_LOCAL_MACHINE \ système \ CurrentControlSet \ Services \ Tcpip \ paramètres \ KeepAliveTime|30 000|  
|HKEY_LOCAL_MACHINE \ système \ CurrentControlSet \ Services \ Tcpip \ paramètres \ KeepAliveInterval|1000|  
|HKEY_LOCAL_MACHINE \ système \ CurrentControlSet \ Services \ Tcpip \ paramètres \ TcpMaxDataRetransmissions|10|  
  
 Vous devez redémarrer l'ordinateur pour appliquer les paramètres de clé de Registre.  
  
 Pour effectuer cela lorsque vous exécutez le système dans Windows Azuren, créez une tâche de démarrage pour ajouter les clés de Registre.  Par exemple, ajoutez la tâche de démarrage suivante au fichier de définition du service :  
  
```  
<Startup>  
    <Task commandLine="AddKeepAlive.cmd" executionContext="elevated" taskType="simple">  
    </Task>  
</Startup>  
```  
  
 Puis ajoutez un fichier AddKeepAlive.cmd à votre projet. Définissez le paramètre « Copier dans le répertoire de sortie » sur « Toujours copier ». Voici un exemple de fichier AddKeepAlive.cmd :  
  
```  
if exist keepalive.txt goto done  
time /t > keepalive.txt  
REM Workaround for JDBC keep alive on SQL Azure  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveTime /t REG_DWORD /d 30000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveInterval /t REG_DWORD /d 1000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v TcpMaxDataRetransmissions /t REG_DWORD /d 10 >> keepalive.txt  
shutdown /r /t 1  
:done  
```  
  
 Ajout du nom de serveur à l'ID d'utilisateur dans la chaîne de connexion  
 Avant la version 4.0 de la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], lors de la connexion à une [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], vous deviez ajouter le nom du serveur à l’ID d’utilisateur dans la chaîne de connexion. Par exemple, user@servername. À compter de la version 4.0 de la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], il n’est plus nécessaire d’ajouter @servername à l’ID d’utilisateur dans la chaîne de connexion.  
  
 Utilisation du chiffrement, nécessitant la définition d'un hostNameInCertificate  
 Lors de la connexion à un [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], vous devez spécifier **hostNameInCertificate** si vous spécifiez **chiffrer = true**. (Si le nom du serveur dans la chaîne de connexion est *shortName*. *nom_domaine*, définissez le **hostNameInCertificate** propriété \*. *nom_domaine*.)  
  
 Par exemple :  
  
```  
jdbc:sqlserver://abcd.int.mscds.com;databaseName= myDatabase;user=myName;password=myPassword;encrypt=true;hostNameInCertificate= *.int.mscds.com;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
  
  
