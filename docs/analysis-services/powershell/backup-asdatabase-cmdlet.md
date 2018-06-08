---
title: Applet de commande Backup-ASDatabase | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ea4e93828aded76fd45ffe6bd279cba8397d8483
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="backup-asdatabase-cmdlet"></a>Applet de commande Backup-ASDatabase
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]

  Sauvegarde d'une base de données multidimensionnelle ou tabulaire Analysis Services dans un fichier de sauvegarde Analysis Services (.abf).  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
## <a name="syntax"></a>Syntaxe  
 `Backup-ASDatabase [-BackupFile] <string> [-Name] <string> [-AllowOverwrite <SwitchParameter>] [-BackupRemotePartitions <SwitchParameter>] [-ApplyCompression <SwitchParameter>] [-FilePassword <SecureString>] [-Locations <Microsoft.AnalysisServices.BackupLocation[]>] [-Server <string>] [-Credential <PSCredential>] [<CommonParameters>]`  
  
 `Backup-ASDatabase –Database <Microsoft.AnalysisServices.Database> [-AllowOverwrite <SwitchParameter>] [-BackupRemotePartitions <SwitchParameter>] [-ApplyCompression <SwitchParameter>] [-FilePassword <SecureString>] [-Locations <Microsoft.AnalysisServices.BackupLocation[]>] [-Server <string>] [-Credential <PSCredential>] [<CommonParameters>]`  
  
## <a name="description"></a> Description  
 Permet à un administrateur système Analysis Services de sauvegarder une base de données multidimensionnelle ou tabulaire dans un fichier de sauvegarde. Si vous ne spécifiez pas un emplacement, l'emplacement de sauvegarde par défaut spécifié pendant l'installation est utilisé.  
  
 Les fichiers que vous sauvegardez peuvent être chiffrés. Utilisez –FilePassword pour chiffrer le fichier. Lorsque vous restaurez le fichier ultérieurement, vous devez fournir le même mot de passe que celui que vous avez spécifié pour le chiffrer.  
  
 Cette applet de commande prend en charge le paramètre –Credential, qui peut être utilisé si vous avez configuré l'instance Analysis Services pour l'accès HTTP. Le paramètre –Credential accepte un objet PSCredential qui fournit une identité d'utilisateur Windows. IIS emprunte l'identité de cet utilisateur lors de la connexion à Analysis Services. L'identité doit avoir des autorisations d'administrateur système sur l'instance d'Analysis Services pour effectuer la sauvegarde.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-backupfile-string"></a>-BackupFile \<chaîne >  
 Spécifie le chemin d'accès et le nom du fichier de sauvegarde. Si vous spécifiez simplement un nom de fichier sans un chemin d'accès, l'emplacement de sauvegarde par défaut est utilisé. Ce paramètre est utilisé uniquement avec le paramètre –Name.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-name-string"></a>-Name \<chaîne >  
 Spécifie la base de données Analysis Services à sauvegarder. Vous pouvez spécifier une base de données en utilisant le paramètre –Database ou –Name si vous souhaitez transmettre le nom en tant que chaîne.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-allowoverwrite-switchparameter"></a>-AllowOverwrite \<Paramètre_booléen >  
 Remplace un fichier de sauvegarde du même nom.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-backupremotepartitions-switchparameter"></a>-BackupRemotePartitions \<Paramètre_booléen >  
 Spécifie si les partitions distantes seront incluses dans la sauvegarde.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-applycompressionswitchparameter"></a>-ApplyCompression\<Paramètre_booléen >  
 Spécifie s'il convient de compresser le fichier de sauvegarde.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-filepassword-securestring"></a>-FilePassword \<SecureString >  
 Indique un mot de passe à utiliser avec le chiffrement du fichier de sauvegarde.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-locations-microsoftanalysisservicesbackuplocation"></a>-Emplacements \<[de Microsoft.AnalysisServices.BackupLocation] >  
 Spécifie l'emplacement où le fichier de sauvegarde sera stocké.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-server-string"></a>-Serveur \<chaîne >  
 Spécifie l'instance Analysis Services à laquelle l'applet de commande se connectera et qu'il exécutera. Si aucun nom de serveur n'est fourni, une connexion sera établie à localhost. Pour les instances par défaut, spécifiez simplement le nom du serveur. Pour les instances nommées, utilisez le format nom_serveur\nom_instance. Pour les connexions HTTP, utilisez le format http[s]://serveur[:port]/répertoirevirtuel/msmdpump.dll.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|localhost|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-credential-pscredential"></a>-Credential \<PSCredential >  
 Spécifie un objet PSCredential qui fournit un nom et un mot de passe d'utilisateur Windows. Spécifiez ce paramètre uniquement si l'instance Analysis Services est configurée pour l'accès HTTP, à l'aide de l'authentification de base. Pour les connexions natives utilisant la sécurité intégrée, ce paramètre est ignoré.  
  
 Si ce paramètre est présent, les informations d'identification qu'il fournit sont ajoutées à la chaîne de connexion. IIS emprunte l'identité de cet utilisateur lors de la connexion à Analysis Services. Si aucune information d'identification n'est indiquée, le compte Windows par défaut de l'utilisateur qui exécute l'outil sera utilisé.  
  
 Pour utiliser ce paramètre, créez d’abord un objet PSCredential à l’aide de Get-Credential pour spécifier le nom d’utilisateur et le mot de passe, par exemple `$Cred=Get-Credential “adventure-works\admin”`). Vous pouvez ensuite canaliser cet objet vers le paramètre –Credential `(-Credential:$Cred`).  
  
Pour plus d’informations sur l’accès HTTP, consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|True (ByValue)|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-database-microsoftanalysisservicesdatabase"></a>-De base de données \<[de Microsoft.AnalysisServices.Database] >  
 Spécifie un objet de base de données Analysis Services à sauvegarder. Vous pouvez spécifier une base de données en utilisant le paramètre –Database ou –Name. Utilisez –Database si vous souhaitez canaliser le nom de la base de données.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 La commande cmdlet prend en charge les paramètres communs : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d’informations, consultez [About_CommonParameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|Microsoft.AnalysisServices.Database<br /><br /> Vous pouvez canaliser plusieurs bases de données à sauvegarder, par exemple toutes les bases de données d'une instance spécifique.|  
|Sorties|Aucun.|  
  
## <a name="example-1"></a>Exemple 1  
  
```  
PS SQLSERVER:\SQLAS\Localhost\default >backup-asdatabase awdb-20110930.abf “Adventure Works” -AllowOverwrite -ApplyCompression  
```  
  
 Cette commande sauvegarde la base de données Adventure Works dans un fichier .abf à l'emplacement de sauvegarde par défaut. Si un fichier du même nom existe déjà à l'emplacement, il est remplacé.  
  
## <a name="example-2"></a>Exemple 2  
  
```  
PS SQLSERVER:\SQLAS\Localhost\default >$AWDB = get-item “databases\Adventure Works”   
PS SQLSERVER:\SQLAS\Localhost\default >Backup-asdatabase –database:$AWDB –AllowOverwrite  
```  
  
 Cette commande utilise –Database au lieu de -Backupfile et -Name. Utilisez le paramètre –Database lorsque vous souhaitez canaliser le nom de la base de données vers l'applet de commande.  
  
## <a name="example-3"></a>Exemple 3  
  
```  
PS SQLSERVER:\SQLAS\Localhost\default\databases >dir * | backup-asdatabase  
```  
  
 Cette commande sauvegarde toutes les bases de données sur le serveur local.  
  
## <a name="example-4"></a>Exemple 4  
  
```  
PS SQLSERVER:\SQLAS\Localhost\default > $pwd = read-host –AsSecureString –Prompt “Password”   
PS SQLSERVER:\SQLAS\Localhost\default > $pwd -is [System.IDisposable]   
PS SQLSERVER:\SQLAS\Localhost\default > backup-asdatabase –backupfile test.abf –name contoso_retail –filepassword:$pwd server myremoteserver  
PS SQLSERVER:\SQLAS\Localhost\default >$pwd.Dispose()  
PS SQLSERVER:\SQLAS\Localhost\default >Remove-Variable –Name pwd  
```  
  
 Les lignes 1 et 2 sont utilisées de manière à demander un mot de passe qui sera utilisé pour chiffrer le fichier.  
  
 La ligne 3 sauvegarde l'exemple de base de données Contoso_Retail sur un serveur Analysis Services distant dans un fichier de sauvegarde Analysis Services nommé test.abf, également situé sur le serveur distant. Le fichier est enregistré dans le dossier de sauvegarde par défaut de l'instance par défaut.  
  
 Les lignes 4 et 5 suppriment le mot de passe.  
  
  
  
