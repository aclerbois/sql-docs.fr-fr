---
title: Commande de chemin d’accès de SET | Documents Microsoft
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
- SET PATH command [ODBC]
ms.assetid: db488d1e-0963-4f45-8c76-a23b9bde9e9d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 058f9f36aa3e762b27e2548fb1a1ddd4767399e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-path-command"></a>Commande de chemin d’accès de SET
Spécifie un chemin d’accès pour les recherches de fichier. Pour plus d’informations spécifiques au pilote, consultez la section Notes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET PATH TO [Path]  
```  
  
## <a name="arguments"></a>Arguments  
 POUR [ *chemin d’accès*]  
 Spécifie les répertoires que vous souhaitez que Visual FoxPro à rechercher. Utilisez des virgules ou des points-virgules pour séparer les répertoires.  
  
## <a name="remarks"></a>Notes  
 Permet de définir le chemin d’accès vous permet de spécifier les chemins de recherche pour d’autres programmes Visual FoxPro qui peuvent être appelées dans des procédures stockées. DÉFINIR le chemin d’accès ne modifiera pas le chemin d’accès de la source de données que vous avez spécifié pour la connexion.  
  
 Émettre définir les chemin d’accès à sans *chemin d’accès* pour restaurer le chemin d’accès au répertoire par défaut ou au dossier.  
  
## <a name="driver-remarks"></a>Section Notes de pilote  
 Si vous exécutez définir le chemin d’accès dans une procédure stockée, il sera ignoré par les commandes et les fonctions suivantes :  
  
-   Fonctions de catalogue comme [SQLTables](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md) et [SQLColumns](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md) ignorera le nouveau chemin d’accès et continuera à référencer le chemin d’accès spécifié par la source de données dans [SQLPrepare](../../odbc/microsoft/sqlprepare-visual-foxpro-odbc-driver.md) ou [SQLExecDirect](../../odbc/microsoft/sqlexecdirect-visual-foxpro-odbc-driver.md).  
  
-   Commandes telles que SELECT, INSERT, UPDATE, DELETE et CREATE TABLE ignorera le nouveau chemin d’accès et continuera à référencer le chemin d’accès spécifié par la source de données dans **SQLPrepare** ou **SQLExecDirect**.  
  
 Si vous émettez définir le chemin d’accès dans une procédure stockée et que vous ne définissez pas par la suite le chemin d’accès à son état d’origine, autres connexions à la base de données utilise le nouveau chemin d’accès (étant donné que définir le chemin d’accès n’est pas étendue aux sessions de données).  
  
 Si vous souhaitez créer, sélectionner ou mettre à jour les tables dans un répertoire autre que celui spécifié par la source de données, spécifiez le chemin d’accès complet du fichier avec votre commande.  
  
## <a name="see-also"></a>Voir aussi  
 [Boîte de dialogue du programme d’installation ODBC Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)   
 [SQLColumns (le pilote ODBC Visual FoxPro)](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md)   
 [SQLDriverConnect (le pilote ODBC Visual FoxPro)](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md)   
 [SQLTables (pilote ODBC Visual FoxPro)](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md)
