---
title: ODBCCONF. EXE | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- odbcconf.exe
ms.assetid: 3bf2be83-61f9-4183-836b-85204ac7116a
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8f7a6e48f6a7a17c8ea3decd79d765fb176080e6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbcconfexe"></a>ODBCCONF. EXE
ODBCCONF.exe est un outil de ligne de commande qui vous permet de configurer des noms de source de données et des pilotes ODBC.  
  
> [!NOTE]  
>  ODBCCONF.exe sera supprimée dans une future version de Windows Data Access Components. Évitez d’utiliser cette fonctionnalité et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Vous pouvez utiliser des commandes PowerShell pour gérer les pilotes et les sources de données. Pour plus d’informations sur ces commandes PowerShell, consultez [applets de commande Windows Data Access Components](https://technet.microsoft.com/library/hh771019.aspx).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ODBCCONF [switches] action  
```  
  
## <a name="arguments"></a>Arguments  
 *Commutateurs*  
 Zéro ou plusieurs options de commutateur. Pour obtenir la liste des commutateurs disponibles, consultez la section Notes, plus loin dans cette rubrique.  
  
 *action*  
 Une action à effectuer. Pour obtenir la liste des options disponibles, consultez la section Notes.  
  
## <a name="remarks"></a>Notes  
 Les commutateurs suivants sont disponibles :  
  
|Commutateur| Description|  
|------------|-----------------|  
|/A {*action*}|Spécifier une action.<br /><br /> /A est facultative si seule une action est spécifiée.|  
|/?|Affiche l’utilisation de ODBCCONF. EXE.|  
|/C|Le traitement se poursuit si une action échoue.|  
|/E|Supprimer le fichier réponse spécifié avec /F lorsque le traitement est terminé.|  
|/F|Utiliser un fichier de réponse, tel que `odbcconf /F my.rsp`.<br /><br /> My.rsp peut ressembler à ceci : `REGSVR c:\my.dll`<br /><br /> /A n’est pas utilisé dans un fichier réponse.|  
|/H|Affiche l’utilisation (aide). Ce commutateur est identique à / ?.|  
|/ L [*mode*] *nom de fichier*|Envoyer la sortie du programme dans un fichier dans un des trois modes : normal (n), verbose (v) et débogage (d). Les DLL qui sont chargés par odbcconf.exe les enregistrements en mode de débogage.<br /><br /> Si vous spécifiez/l sans mode, le fichier journal sera vide.<br /><br /> Par exemple, **/Lv log.txt**.|  
|/R|L’action sera effectuée après un redémarrage.|  
|/S|Mode silencieux. Ne pas afficher les messages d’erreur.|  
  
 Les actions suivantes sont disponibles :  
  
|Action| Description|  
|------------|-----------------|  
|CONFIGDRIVER *nom_pilote ** spécifiques au pilote configuration params*|Charge la DLL d’installation du pilote approprié et appelle le **ConfigDriver** (fonction).<br /><br /> Équivalent à la [SQLConfigDriver fonction](../odbc/reference/syntax/sqlconfigdriver-function.md).<br /><br /> Par exemple :<br /><br /> /A {CONFIGDRIVER « Nom de pilote » « CPTimeout = 60 »}<br /><br /> /A {CONFIGDRIVER « Nom de pilote » « DriverODBCVer = 03.80 »}|  
|CONFIGDSN *nom_pilote* DSN =*nom* &#124; *attributs*|Ajoute ou modifie une source de données système.<br /><br /> Équivalent à la [fonction SQLConfigDataSource](../odbc/reference/syntax/sqlconfigdatasource-function.md).<br /><br /> Par exemple :<br /><br /> /A {CONFIGDSN « SQL Server » « DSN = nom &#124; Server = srv »}|  
|CONFIGSYSDSN *nom_pilote* DSN =*nom* &#124; *attributs*|Ajoute ou modifie une source de données système.<br /><br /> Équivalent à la [fonction SQLConfigDataSource](../odbc/reference/syntax/sqlconfigdatasource-function.md).<br /><br /> Par exemple :<br /><br /> /A {CONFIGSYSDSN « SQL Server » « DSN = nom &#124; Server = srv »}|  
|INSTALLDRIVER|Équivalent à [SQLInstallDriverEx fonction](../odbc/reference/syntax/sqlinstalldriverex-function.md).<br /><br /> Pour plus d’informations sur la syntaxe de paires mot clé-valeur passée à INSTALLDRIVER, consultez [pilote spécification sous-clés](../odbc/reference/install/driver-specification-subkeys.md).<br /><br /> Par exemple :<br /><br /> /A {INSTALLDRIVER « votre pilote &#124; Driver=c:\your.dll &#124; Setup=c:\your.dll &#124; APILevel = 2 &#124; ConnectFunctions = YYY &#124; DriverODBCVer = 03.50 &#124; FileUsage = 0 &#124; SQLLevel = 1 »}|  
|INSTALLTRANSLATOR *configuration du traducteur ** chemin d’accès du pilote*|Ajoute des informations sur un convertisseur pour la **HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST. INI\ODBC traducteurs** clé de Registre.<br /><br /> Équivalent à [SQLInstallTranslatorEx fonction](../odbc/reference/syntax/sqlinstalltranslatorex-function.md).<br /><br /> Pour plus d’informations sur la syntaxe de paires mot clé-valeur passée à INSTALLDRIVER, consultez [traducteur spécification sous-clés](../odbc/reference/install/translator-specification-subkeys.md).<br /><br /> Par exemple :<br /><br /> /A {INSTALLTRANSLATOR « mon traducteur &#124; Translator=c:\my.dll &#124; Setup=c:\my.dll »}|  
|REGSVR *dll*|Inscrit une DLL.<br /><br /> Équivalent à regsvr32.exe.<br /><br /> Par exemple :<br /><br /> /A {REGSVR c:\my.dll}|  
|SETFILEDSNDIR|Lorsque HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC. INI\ODBC fichier DSN\DefaultDSNDir n’existe pas, l’action SETFILEDSNDIR crée et lui assigne la valeur à HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\CommonFilesDir, ajouté avec \ODBC\Data Sources.<br /><br /> Valeur à HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC. INI\ODBC fichier DSN\DefaultDSNDir Spécifie l’emplacement par défaut utilisé par l’administrateur de Source de données ODBC lors de la création d’une source de données de fichiers.<br /><br /> Par exemple :<br /><br /> /A {SETFILEDSNDIR}|  
  
## <a name="see-also"></a>Voir aussi  
 [Microsoft ODBC (Open Database Connectivity)](../odbc/microsoft-open-database-connectivity-odbc.md)
