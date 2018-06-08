---
title: Fonction de SQLPoolConnect | Documents Microsoft
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
- SQLPoolConnect function [ODBC]
ms.assetid: 41322737-890d-4a81-aed2-06cc3d546962
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 141ba45aca6a6bcc25503ef73d575793f1cf5b15
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlpoolconnect-function"></a>SQLPoolConnect (fonction)
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 3.8 ODBC : ODBC  
  
 **Résumé**  
 **SQLPoolConnect** est utilisé pour créer une nouvelle connexion si aucune connexion dans le pool ne peut être réutilisée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SQLRETURN  SQLPoolConnect(  
                SQLHDBC              hDbc,  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              wszOutConnectString,  
                SQLSMALLINT          cchConnectStringBuffer,  
                SQLSMALLINT *        cchConnectStringLen );  
```  
  
## <a name="arguments"></a>Arguments  
 *Pas*  
 [Entrée] Le handle de connexion.  
  
 *hDbcInfoToken*  
 [Entrée] Le handle de jeton pour la nouvelle requête de connexion d’application.  
  
 *wszOutConnectString*  
 [Sortie] Pointeur vers une mémoire tampon pour la chaîne de connexion terminée. Lors de la connexion réussie à la source de données cible, cette mémoire tampon contient la chaîne de connexion terminée. Applications devraient allouer cette mémoire tampon au moins 1 024 caractères.  
  
 Si *wszOutConnectString* est NULL, *cchConnectStringLen* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *wszOutConnectString*.  
  
 *cchConnectStringBuffer*  
 [Entrée] Longueur de la **wszOutConnectString* mémoire tampon, en caractères.  
  
 *cchConnectStringLen*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total de caractères (sans compter le caractère de fin de la valeur null) disponibles à renvoyer dans \* *wszOutConnectString*. Si le nombre de caractères à retourner est supérieur ou égal à *cchConnectStringBuffer*, la fin de chaîne de connexion dans \* *wszOutConnectString* est tronqué à *cchConnectStringBuffer* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Semblable à [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) pour tout d’entrée d’une erreur de validation, à ceci près que le Gestionnaire de pilotes utilise un **HandleType** de SQL_HANDLE_DBC_INFO_TOKEN et un **gérer** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Notes  
 Le Gestionnaire de pilotes garantit que le parent HENV gérer de *pas* et *hDbcInfoToken* sont les mêmes.  
  
 Contrairement aux [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md), il est sans *DriverCompletion* argument pour inviter les utilisateurs à entrer des informations de connexion. Une boîte de dialogue invite n’est pas autorisée dans le scénario de regroupement.  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Chaque fois qu’un pilote retourne SQL_ERROR ou SQL_INVALID_HANDLE, le Gestionnaire de pilotes retourne l’erreur à l’application (dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) ou [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Chaque fois qu’un pilote retourne SQL_SUCCESS_WITH_INFO, le Gestionnaire de pilotes obtiendra les informations de diagnostic à partir de *hDbcInfoToken*et retourne SQL_SUCCESS_WITH_INFO, à l’application dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) et [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Lorsqu’une application utilise [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md), *wszOutConnectString* sera un tampon NULL (les trois derniers paramètres sont toutes être définies sur NULL, 0, NULL). Dans le cas contraire, le pilote doit renvoyer la chaîne de connexion de sortie, qui s’affichera à l’application [fonction SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) appeler.  
  
 Inclure sqlspi.h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Le regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
