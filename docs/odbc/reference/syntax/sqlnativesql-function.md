---
title: Fonction SQLNativeSql | Documents Microsoft
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
- SQLNativeSql
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLNativeSql
helpviewer_keywords:
- SQLNativeSql function [ODBC]
ms.assetid: b8efc247-27ab-4a00-92b6-1400785783fe
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c19a18aad5268be9aa46f3f2674ed39e02ab0640
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlnativesql-function"></a>Fonction SQLNativeSql
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 1.0 ODBC : ODBC  
  
 **Résumé**  
 **SQLNativeSql** renvoie la chaîne SQL modifiée par le pilote. **SQLNativeSql** ne s’exécute pas l’instruction SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLNativeSql(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      InStatementText,  
     SQLINTEGER     TextLength1,  
     SQLCHAR *      OutStatementText,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   TextLength2Ptr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Handle de connexion*  
 [Entrée] Handle de connexion.  
  
 *InStatementText*  
 [Entrée] Chaîne de texte SQL doivent être converties.  
  
 *TextLength1*  
 [Entrée] Longueur en caractères de **InStatementText* chaîne de texte.  
  
 *OutStatementText*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la chaîne traduite SQL.  
  
 Si *OutStatementText* est NULL, *TextLength2Ptr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *OutStatementText*.  
  
 *BufferLength*  
 [Entrée] Nombre de caractères dans le \* *OutStatementText* mémoire tampon. Si la valeur retournée dans  *\*InStatementText* est une chaîne Unicode (lors de l’appel **SQLNativeSqlW**), la *BufferLength* l’argument doit être un nombre pair.  
  
 *TextLength2Ptr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total de caractères (à l’exclusion des arrêts de null) disponibles à renvoyer dans \* *OutStatementText*. Si le nombre de caractères à retourner est supérieur ou égal à *BufferLength*, traduit la chaîne SQL dans \* *OutStatementText* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLNativeSql** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_DBC et un *gérer* de *handle de connexion*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLNativeSql** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|La mémoire tampon \* *OutStatementText* n’est pas suffisamment grande pour retourner la chaîne SQL entière, donc la chaîne SQL a été tronquée. La longueur de la chaîne SQL non tronquée est retournée dans **TextLength2Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08003|Connexion non ouverte|Le *handle de connexion* n’était pas dans un état connecté.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|22007|Format datetime non valide|**InStatementText* contenu d’une clause d’échappement avec une valeur de date, heure ou timestamp non valide.|  
|24000|État de curseur non valide|Le curseur dans l’instruction a été positionné avant le début du jeu de résultats ou après la fin du jeu de résultats. Cette erreur ne peut pas être retournée par un pilote ayant une implémentation de curseur SGBD native.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) **InStatementText* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le *handle de connexion* et toujours en cours d’exécution lorsque cette fonction a été appelée.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) l’argument *TextLength1* était inférieur à 0, mais pas égale à SQL_NTS.|  
|||(DM) l’argument *BufferLength* était inférieure à 0 et l’argument *OutStatementText* n’était pas un pointeur null.|  
|HY109|Position du curseur non valide|La ligne actuelle du curseur a été supprimée ou n’a pas été extraites. Cette erreur ne peut pas être retournée par un pilote ayant une implémentation de curseur SGBD native.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *handle de connexion* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Voici des exemples de ce que **SQLNativeSql** peut retourner pour la chaîne SQL d’entrée suivante contenant la fonction scalaire CONVERT. Supposons que la colonne empid est de type entier dans la source de données :  
  
```  
SELECT { fn CONVERT (empid, SQL_SMALLINT) } FROM employee  
```  
  
 Un pilote pour Microsoft SQL Server peut renvoyer la chaîne SQL traduite suivante :  
  
```  
SELECT convert (smallint, empid) FROM employee  
```  
  
 Un pilote pour le serveur ORACLE peut retourner la chaîne SQL traduite suivante :  
  
```  
SELECT to_number (empid) FROM employee  
```  
  
 Un pilote pour Ingres peut retourner la chaîne SQL traduite suivante :  
  
```  
SELECT int2 (empid) FROM employee  
```  
  
 Pour plus d’informations, consultez [l’exécution directe](../../../odbc/reference/develop-app/direct-execution-odbc.md) et [exécution préparée](../../../odbc/reference/develop-app/prepared-execution-odbc.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
 Aucun.  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
