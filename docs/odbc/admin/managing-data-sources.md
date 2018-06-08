---
title: Gestion des Sources de données | Documents Microsoft
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
- deleting data sources [ODBC], ODBC data source administrator
- data sources [ODBC], ODBC data source administrator
- adding data sources [ODBC], ODBC data source administrator
- removing data sources [ODBC], ODBC data source administrator
- ODBC data source administrator [ODBC], data source management
ms.assetid: 67cc4945-4850-4eb4-8da6-b835ddaeca4c
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6be8fba37a93e2fda66e96d1334ee4ddcb15d9e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="managing-data-sources"></a>Gestion des Sources de données
Après avoir installé un pilote ODBC programme du pilote d’installation, vous pouvez définir une ou plusieurs sources de données pour celui-ci. Le nom de source de données (DSN) doit fournir une description unique des données ; par exemple, *paie* ou *paies*. Les sources de données utilisateur et système qui sont définies pour tous les pilotes actuellement installés sont répertoriés dans le **DSN utilisateur** ou **DSN système** onglets de la **administrateur de sources de données ODBC** boîte de dialogue. Les sources de données de fichier dans un répertoire donné sont répertoriées dans le **DSN de fichier** onglet ; le répertoire à afficher est entré dans le **Regarder dans** zone le **DSN de fichier** onglet.  
  
> [!NOTE]  
>  Pour gérer une source de données qui se connecte à un pilote 32 bits sous la plateforme 64 bits, utilisez c:\windows\sysWOW64\odbcad32.exe. Pour gérer une source de données qui se connecte à un pilote 64 bits, utilisez c:\windows\system32\odbcad32.exe. Dans **outils d’administration** sur un système d’exploitation de Windows 8 64 bits, il existe des icônes pour les 32 bits et 64 bits **administrateur de sources de données ODBC** boîte de dialogue.  
  
 Si vous utilisez le odbcad32.exe 64 bits pour configurer ou supprimer une source de données qui se connecte à un pilote 32 bits, par exemple, **Driver do Microsoft Access (\*.mdb)**, vous recevez le message d’erreur suivant :  
  
```  
The specified DSN contains an architecture mismatch between the Driver and Application  
```  
  
 Pour résoudre cette erreur, utilisez le odbcad32.exe 32 bits pour configurer ou supprimer la source de données.  
  
 Une source de données associe les données que vous souhaitez accéder via le pilote à un pilote ODBC spécifique. Par exemple, vous pouvez créer une source de données pour utiliser le pilote dBASE ODBC pour accéder à un ou plusieurs fichiers dBASE dans un répertoire spécifique sur votre disque dur ou un lecteur réseau. À l’aide de l’administrateur de Source de données ODBC, vous pouvez ajouter, modifier et supprimer des sources de données, comme indiqué dans le tableau suivant.  
  
|Action| Description|  
|------------|-----------------|  
|Ajout de sources de données|Il est possible d’ajouter plusieurs sources de données, chacune d’elles association d’un pilote avec des données que vous souhaitez accéder à l’aide de ce pilote. Donnez un nom qui identifie de façon unique cette source de données à chaque source de données. Par exemple, si vous créez une source de données pour un ensemble de fichiers dBASE qui contiennent des informations sur le client, vous pouvez nommer la source de données « Customers ». Applications affichent généralement aux utilisateurs de choisir à partir des noms de sources de données.<br /><br /> Ajout d’une source de données de fichier est légèrement différente de l’ajout d’utilisateur ou des sources de données système. Pour plus d’informations, consultez l’administrateur de Source de données ODBC à l’aide de fichiers.|  
|Modification de sources de données|Selon vos besoins, vous pouvez s’avérer nécessaire de reconfigurer les sources de données. Vous pouvez réinitialiser les options en cliquant sur **configurer** dans toute boîte de dialogue de configuration de pilote.|  
|Suppression de sources de données|Cliquez sur **supprimer** après avoir sélectionné une source de données.|  
  
 Pour plus d’informations sur les sources de données de fichier, consultez [la connexion à l’aide de Sources de données](../../odbc/reference/develop-app/connecting-using-file-data-sources.md) ou [fonction SQLDriverConnect](../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Administrateur de sources de données ODBC](../../odbc/admin/odbc-data-source-administrator.md)
