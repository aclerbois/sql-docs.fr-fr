---
title: Fonction SQLGetDiagRec | Documents Microsoft
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
- SQLGetDiagRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagRec
helpviewer_keywords:
- SQLGetDiagRec function [ODBC]
ms.assetid: ebdbac93-3d68-438f-8416-ef1f08e04269
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a926e8de729fe5d654f880128371bfc48dd39fdd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetdiagrec-function"></a>SQLGetDiagRec (fonction)
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetDiagRec** retourne les valeurs actuelles de plusieurs champs d’un enregistrement de diagnostic qui contient des informations d’erreur, d’avertissement et d’état. Contrairement aux **SQLGetDiagField**, qui retourne un champ de diagnostic par appel, **SQLGetDiagRec** retourne plusieurs des champs d’un enregistrement de diagnostic, y compris la valeur SQLSTATE, le code d’erreur natif et le texte du message de diagnostic communément utilisés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetDiagRec(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLCHAR *       SQLState,  
     SQLINTEGER *    NativeErrorPtr,  
     SQLCHAR *       MessageText,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   TextLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 [Entrée] Identificateur de type handle qui décrit le type de handle pour lequel les tests de diagnostic sont requis. Doit prendre l'une des valeurs suivantes :  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 Handle SQL_HANDLE_DBC_INFO_TOKEN est utilisé uniquement par le Gestionnaire de pilotes et le pilote. Applications ne doivent pas utiliser ce type de handle. Pour plus d’informations sur SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement reconnaissance du Pool de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *Handle*  
 [Entrée] Un handle pour la structure de données de diagnostic, du type indiqué par *HandleType*. Si *HandleType* est SQL_HANDLE_ENV, *gérer* peut être partagé ou un handle d’environnement non partagé.  
  
 *RecNumber*  
 [Entrée] Indique que l’enregistrement de l’état à partir de laquelle l’application recherche des informations. Enregistrements d’état sont numérotées de 1.  
  
 *SQLState*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner un code SQLSTATE à cinq caractères (et le caractère NULL de fin) pour l’enregistrement de diagnostic *RecNumber*. Les deux premiers caractères indiquent la classe ; les trois suivants indiquent la sous-classe. Ces informations sont contenues dans le champ de diagnostic SQL_DIAG_SQLSTATE. Pour plus d’informations, consultez [SQLSTATE](../../../odbc/reference/develop-app/sqlstates.md).  
  
 *NativeErrorPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le code d’erreur natif spécifique à la source de données. Ces informations sont contenues dans le champ de diagnostic SQL_DIAG_NATIVE.  
  
 *MessageText*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la chaîne de texte de message de diagnostic. Ces informations sont contenues dans le champ de diagnostic SQL_DIAG_MESSAGE_TEXT. Pour le format de la chaîne, consultez [des Messages de Diagnostic](../../../odbc/reference/develop-app/diagnostic-messages.md).  
  
 Si *MessageText* est NULL, *TextLengthPtr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *MessageText*.  
  
 *BufferLength*  
 [Entrée] Longueur de la **MessageText* mémoire tampon en caractères. Il n’existe aucune longueur maximale du texte du message de diagnostic.  
  
 *TextLengthPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total de caractères (sans compter le nombre de caractères requis pour le caractère de fin de la valeur null) disponibles à renvoyer dans  *\*MessageText*. Si le nombre de caractères à retourner est supérieur à *BufferLength*, le texte du message de diagnostic dans  *\*MessageText* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 **SQLGetDiagRec** ne valide pas les enregistrements de diagnostic pour lui-même. Il utilise les valeurs de retournés suivantes pour signaler le résultat de l’exécution de son propre :  
  
-   SQL_SUCCESS : La fonction retournée avec succès les informations de diagnostic.  
  
-   SQL_SUCCESS_WITH_INFO : Le \* *MessageText* tampon est trop petite pour contenir le message de diagnostic demandé. Aucun enregistrement de diagnostic n’ont été générés. Pour déterminer qu’une troncation s’est produite, l’application doit comparer *BufferLength* au nombre réel d’octets disponible, ce qui est écrite dans **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE : Le descripteur indiqué par *HandleType* et *gérer* n’était pas un handle valide.  
  
-   SQL_ERROR : Un des éléments suivants s’est produit :  
  
    -   *RecNumber* était négatif ou égal à 0.  
  
    -   *BufferLength* était inférieur à zéro.  
  
    -   Lorsque vous utilisez la notification asynchrone, l’opération asynchrone sur le handle n’était pas terminée.  
  
-   SQL_NO_DATA : *RecNumber* est supérieur au nombre d’enregistrements de diagnostic qui existaient pour le handle spécifié dans *gérer.* La fonction retourne également SQL_NO_DATA pour n’importe quel nombre positif *RecNumber* si aucun enregistrement de diagnostic pour *gérer*.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle généralement **SQLGetDiagRec** lorsqu’un appel précédent à une fonction ODBC a renvoyé SQL_ERROR ou SQL_SUCCESS_WITH_INFO. Toutefois, étant donné que n’importe quelle fonction ODBC peut valider zéro ou plusieurs enregistrements de diagnostic à chaque fois qu’elle est appelée, une application peut appeler **SQLGetDiagRec** après un appel de fonction ODBC. Une application peut appeler **SQLGetDiagRec** plusieurs fois afin de retourner tout ou partie des enregistrements dans la structure de données de diagnostic. ODBC n’impose aucune limite au nombre d’enregistrements de diagnostic qui peuvent être stockés à tout moment.  
  
 **SQLGetDiagRec** ne peut pas être utilisée pour retourner les champs de l’en-tête de la structure de données de diagnostic. (Le *RecNumber* argument doit être supérieure à 0.) L’application doit appeler **SQLGetDiagField** à cet effet.  
  
 **SQLGetDiagRec** récupère uniquement les informations de diagnostic plus récemment associées au handle spécifié dans le *gérer* argument. Si l’application appelle une autre fonction ODBC, sauf **SQLGetDiagRec**, **SQLGetDiagField**, ou **SQLError**, toutes les informations de diagnostic à partir des appels précédentes sur le même handle sont perdues.  
  
 Une application peut analyser tous les enregistrements de diagnostic en effectuant une boucle, incrémentant *RecNumber*, à condition que **SQLGetDiagRec** retourne SQL_SUCCESS. Les appels à **SQLGetDiagRec** sont non destructrice pour les champs d’en-tête et un enregistrement. L’application peut appeler **SQLGetDiagRec** ultérieurement pour récupérer un champ d’un enregistrement en tant qu’aucune autre fonction, à l’exception de **SQLGetDiagRec**, **SQLGetDiagField**, ou **SQLError**, a été appelée dans l’intervalle. L’application peut également récupérer un nombre au nombre total d’enregistrements de diagnostic en appelant **SQLGetDiagField** pour récupérer la valeur du champ SQL_DIAG_NUMBER et appeler ensuite **SQLGetDiagRec** autant de fois.  
  
 Pour obtenir une description des champs de la structure de données de diagnostic, consultez [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md). Pour plus d’informations, consultez [à l’aide de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) et [mise en œuvre de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 Appel d’une API que celle qui est exécutée de manière asynchrone génère HY010 « Erreur de séquence de fonction ». Toutefois, l’enregistrement d’erreur ne peut pas être récupérée avant la fin de l’opération asynchrone.  
  
## <a name="handletype-argument"></a>HandleType Argument  
 Chaque type de handle peut avoir des informations de diagnostic associées. Le *HandleType* argument indique le type de handle de la *gérer* argument.  
  
 Certains champs d’en-tête et d’enregistrement ne peut pas retournés pour l’environnement, connexion, l’instruction et descripteur de handles. Ces poignées pour lesquels un champ n’est pas applicable sont indiquées dans les sections « Champs d’en-tête » et « Champs de l’enregistrement » de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
 Un appel à **SQLGetDiagRec** retourne SQL_INVALID_HANDLE si *HandleType* est SQL_HANDLE_SENV, ce qui indique un handle d’environnement partagé. Toutefois, si *HandleType* est SQL_HANDLE_ENV, *gérer* peut être partagé ou un handle d’environnement non partagé.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention d’un champ d’un enregistrement de diagnostic ou d’un champ de l’en-tête de diagnostic|[SQLGetDiagField, fonction](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md)
