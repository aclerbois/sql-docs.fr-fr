---
title: Fonction de SQLDataSources | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSources
helpviewer_keywords:
- SQLDataSources function [ODBC]
ms.assetid: 3f63b1b4-e70e-44cd-96c6-6878d50d0117
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c2faa7631aaba8192f04270236b3ff9ef4e48240
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldatasources-function"></a>SQLDataSources (fonction)
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLDataSources** renvoie des informations sur une source de données. Cette fonction est implémentée uniquement par le Gestionnaire de pilotes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLDataSources(  
     SQLHENV          EnvironmentHandle,  
     SQLUSMALLINT     Direction,  
     SQLCHAR *        ServerName,  
     SQLSMALLINT      BufferLength1,  
     SQLSMALLINT *    NameLength1Ptr,  
     SQLCHAR *        Description,  
     SQLSMALLINT      BufferLength2,  
     SQLSMALLINT *    NameLength2Ptr);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 [Entrée] Handle d’environnement.  
  
 *Direction*  
 [Entrée] Détermine quelle source de données retourne des informations sur le Gestionnaire de pilotes. Valeurs possibles :  
  
 SQL_FETCH_NEXT (pour extraire le nom de source de données suivant dans la liste), SQL_FETCH_FIRST (pour récupérer à partir du début de la liste), SQL_FETCH_FIRST_USER (à l’utilisateur de l’extraction de la première source de données) ou SQL_FETCH_FIRST_SYSTEM (pour extraire le premier système DSN).  
  
 Lorsque *Direction* a la valeur SQL_FETCH_FIRST, les appels suivants à **SQLDataSources** avec *Direction* SQL_FETCH_NEXT la valeur de retour DSN utilisateur et système. Lorsque *Direction* a la valeur SQL_FETCH_FIRST_USER, tous les appels suivants à **SQLDataSources** avec *Direction* SQL_FETCH_NEXT la valeur de retour uniquement des sources de données utilisateur. Lorsque *Direction* a la valeur SQL_FETCH_FIRST_SYSTEM, tous les appels suivants à **SQLDataSources** avec *Direction* SQL_FETCH_NEXT la valeur de retour uniquement les sources de données système.  
  
 *ServerName*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nom de source de données.  
  
 Si *nom_serveur* est NULL, *NameLength1Ptr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *nom_serveur*.  
  
 *BufferLength1*  
 [Entrée] Longueur de la **nom_serveur* de la mémoire tampon, en caractères ; cette ne doit pas comporter plus de SQL_MAX_DSN_LENGTH ainsi que le caractère de fin de la valeur null.  
  
 *NameLength1Ptr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total de caractères (sans compter le caractère de fin de la valeur null) disponibles à renvoyer dans \* *nom_serveur*. Si le nombre de caractères à retourner est supérieur ou égal à *BufferLength1*, le nom de source de données \* *nom_serveur* est tronqué à *BufferLength1* moins la longueur d’un caractère de fin de la valeur null.  
  
 *Description*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la description du pilote associé à la source de données. Par exemple, dBASE ou SQL Server.  
  
 Si *Description* est NULL, *NameLength2Ptr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *Description*.  
  
 *BufferLength2*  
 [Entrée] Longueur en caractères de la **Description* mémoire tampon.  
  
 *NameLength2Ptr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total de caractères (sans compter le caractère de fin de la valeur null) disponibles à renvoyer dans \* *Description*. Si le nombre de caractères à retourner est supérieur ou égal à *BufferLength2*, la description du pilote dans \* *Description* est tronqué à *BufferLength2* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLDataSources** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_ENV et un *gérer* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLDataSources** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|(DM) message d’information du Gestionnaire de pilotes spécifiques. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|(DM) la mémoire tampon \* *nom_serveur* n’est pas suffisamment grande pour retourner le nom de source de données complet. Par conséquent, le nom a été tronqué. La longueur du nom de source de données entière est retournée dans \* *NameLength1Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).<br /><br /> (DM) la mémoire tampon \* *Description* n’est pas suffisamment grande pour retourner la description complète de pilote. Par conséquent, la description a été tronquée. La longueur de la description de la source de données non tronqué est retournée dans **NameLength2Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|HY000|Erreur générale|(DM) une erreur s’est produite pour lequel il n’a aucun code SQLSTATE spécifique et pour lequel aucune SQLSTATE spécifique à l’implémentation a été défini. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le Gestionnaire de pilotes (DM) n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength1* était inférieure à 0.<br /><br /> (DM) la valeur spécifiée pour l’argument *BufferLength2* était inférieure à 0.|  
|HY103|Code de récupération non valide|(DM) la valeur spécifiée pour l’argument *Direction* n’est pas égale à SQL_FETCH_FIRST, SQL_FETCH_FIRST_USER, SQL_FETCH_FIRST_SYSTEM ou SQL_FETCH_NEXT.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Commentaires  
 Étant donné que **SQLDataSources** est implémentée dans le Gestionnaire de pilotes, il est pris en charge pour tous les pilotes indépendamment de la conformité aux normes d’un pilote spécifique.  
  
 Une application peut appeler **SQLDataSources** plusieurs fois afin de récupérer tous les noms de source de données. Le Gestionnaire de pilotes récupère ces informations à partir des informations système. Lorsqu’il n’y a aucun nom de source de données plus, le Gestionnaire de pilotes retourne SQL_NO_DATA. Si **SQLDataSources** est appelée avec SQL_FETCH_NEXT immédiatement après avoir retourné SQL_NO_DATA, elle retourne le premier nom de source de données. Pour plus d’informations sur la façon dont une application utilise les informations retournées par **SQLDataSources**, consultez [choisir une Source de données ou le pilote](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Si SQL_FETCH_NEXT est transmis à **SQLDataSources** la première fois qu’elle est appelée, elle retourne le premier nom de source de données.  
  
 Le pilote détermine comment les noms de source de données sont mappées à des sources de données réelles.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Détection et la liste des valeurs requises pour se connecter à une source de données|[SQLBrowseConnect, fonction](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connexion à une source de données|[SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Connexion à une source de données à l’aide d’une boîte de dialogue ou la chaîne de connexion|[SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Renvoi de descriptions de pilote et attributs|[SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
