---
title: Ajouter une Source de données (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- data sources [ODBC]
ms.assetid: b4ac6f0e-8e6a-4b1a-9a7e-60e0a69b2180
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3d0fd3ef5ecf15450f8ba231a6cff288c25e32e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuring-the-sql-server-odbc-driver---add-a-data-source"></a>Configurer le pilote ODBC de SQL Server - ajouter une Source de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Avant d'utiliser des applications ODBC avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, vous devez savoir comment mettre à niveau la version des procédures stockées du catalogue sur les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et savoir comment ajouter, supprimer et tester des sources de données.  
  
  Vous pouvez ajouter une source de données à l’aide de l’administrateur ODBC, par programmation (à l’aide de [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md)), ou en créant un fichier.  
  
### <a name="to-add-a-data-source-by-using-odbc-administrator"></a>Pour ajouter une source de données à l'aide de l'Administrateur ODBC  
  
1.  À partir de la **le panneau de configuration**, accès **outils d’administration** , puis sur **Sources de données ODBC (64 bits)** ou **Sources de données ODBC (32 bits)**. Vous pouvez également appeler l'exécutable odbcad32.exe.  
  
2.  Cliquez sur le **DSN utilisateur**, **DSN système**, ou **DSN de fichier** onglet, puis cliquez sur **ajouter**.  
  
3.  Cliquez sur **SQL Server**, puis cliquez sur **Terminer**.  
  
4.  Effectuez les étapes de la **créer une nouvelle Source de données vers SQL Server** Assistant.  
  
### <a name="to-add-a-data-source-programmatically"></a>Pour ajouter une source de données par programme  
  
1.  Appelez [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md) avec le deuxième paramètre défini sur ODBC_ADD_DSN ou sur ODBC_ADD_SYS_DSN.  
  
### <a name="to-add-a-file-data-source"></a>Pour ajouter une source de données de fichier  
  
1.  Appelez [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) avec un SAVEFILE = nom_fichier des paramètres dans la chaîne de connexion. Si la connexion est un succès, le pilote ODBC crée une source de données de fichier avec les paramètres de connexion dans l'emplacement désigné par le paramètre SAVEFILE.  
  
## <a name="see-also"></a>Voir aussi  
[Supprimer une Source de données & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-how-to/configuring-the-sql-server-odbc-driver-delete-a-data-source.md)    
  
  
