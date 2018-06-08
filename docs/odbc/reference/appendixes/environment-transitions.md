---
title: Transitions d’environnement | Documents Microsoft
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
- environment transitions [ODBC]
- transitioning states [ODBC], environment
- state transitions [ODBC], environment
ms.assetid: 9d11b1ab-f4c8-48ca-9812-8c04303f939d
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c6152a2ee5f21d20a6bf0ddea568e807443dfd00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="environment-transitions"></a>Transitions d’environnement
Les environnements de ODBC ont trois états suivants.  
  
|État| Description|  
|-----------|-----------------|  
|E0|Environnement non alloué|  
|E1|Environnement alloué, non alloué de connexion|  
|E2|Allouée environnement, allouée de connexion|  
  
 Les tableaux suivants indiquent comment chaque fonction ODBC affecte l’état de l’environnement.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|E1 [1]|--[4]|--[4]|  
|(INCLUENT) [2]|E2 [5]<br />(HY010) [6]|--[4]|  
|(INCLUENT) [3]|(INCLUENT)|--[4]|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3] de cette ligne affiche les transitions lorsque *HandleType* était SQL_HANDLE_STMT ou SQL_HANDLE_DESC.  
  
 [4] appel de **SQLAllocHandle** avec *OutputHandlePtr* pointant vers un handle valide remplace ce handle. Cela peut être une erreur de programmation d’application.  
  
 [5] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [6] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources et SQLDrivers  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT)|--[1]<br />(HY010) [2]|--[1]<br />(HY010) [2]|  
  
 [1] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [2] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT) [1]|--[3]<br />(HY010) [4]|--[3]<br />(HY010) [4]|  
|(INCLUENT) [2]|(INCLUENT)|--|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [4] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT) [1]|E0|(HY010)|  
|(INCLUENT) [2]|(INCLUENT)|--[4]<br />E1 [5]|  
|(INCLUENT) [3]|(INCLUENT)|--|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_DBC.  
  
 [3] de cette ligne affiche les transitions lorsque *HandleType* était SQL_HANDLE_STMT ou SQL_HANDLE_DESC.  
  
 [4] ont été autres handles de connexion alloué.  
  
 [5], le handle de connexion spécifié dans *gérer* a été le handle de connexion alloué uniquement.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField et SQLGetDiagRec  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT) [1]|--|--|  
|(INCLUENT) [2]|(INCLUENT)|--|  
  
 [1] cette ligne affiche les transitions lorsque *HandleType* a été SQL_HANDLE_ENV.  
  
 [2] de cette ligne affiche les transitions lorsque *HandleType* était SQL_HANDLE_DBC, SQL_HANDLE_STMT ou SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>Cas  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT)|--[1]<br />(HY010) [2]|--|  
  
 [1] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [2] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT)|--[1]<br />(HY010) [2]|(HY011)|  
  
 [1] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [2] le *attribut* argument n’a pas été SQL_ATTR_ODBC_VERSION et l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="all-other-odbc-functions"></a>Toutes les autres fonctions ODBC  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Alloué|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|(INCLUENT)|(INCLUENT)|--|
