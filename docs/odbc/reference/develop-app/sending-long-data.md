---
title: Envoi de données de type Long | Documents Microsoft
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
- long data [ODBC]
- sending long data [ODBC]
ms.assetid: ea989084-a8e6-4737-892e-9ec99dd49caf
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3a15a3fdd69a1578392d34adac08b297269df7cf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sending-long-data"></a>Envoi de données de type Long
Définir des SGBD *données longues* sous forme de n’importe quel caractère ou binaire sur une certaine taille, tels que de 254 caractères. Il se peut qu’il ne soit pas possible de stocker la totalité d’un élément de données de type long dans la mémoire, telles que lorsque l’élément représente un document de texte long ou une image bitmap. Étant donné que ces données ne peut pas être stockées dans une seule mémoire tampon, la source de données l’envoie au pilote dans des parties avec **SQLPutData** lorsque l’instruction est exécutée. Paramètres pour lesquels les données sont envoyées au moment de l’exécution sont appelées *data-at-execution paramètres*.  
  
> [!NOTE]  
>  Une application peut envoyer n’importe quel type de données au moment de l’exécution avec **SQLPutData**, bien que les caractères et les données binaires peuvent être envoyées dans les parties. Toutefois, si les données sont assez petites pour tenir dans un tampon unique, il est généralement inutile d’utiliser **SQLPutData**. Il est beaucoup plus facile de lier la mémoire tampon et de laisser le pilote de récupérer les données à partir de la mémoire tampon.  
  
 Pour envoyer des données au moment de l’exécution, l’application effectue les actions suivantes :  
  
1.  Transmet une valeur 32 bits qui identifie le paramètre dans le *ParameterValuePtr* argument dans **SQLBindParameter** plutôt que de la transmission de l’adresse d’une mémoire tampon. Cette valeur n’est pas analysée par le pilote. Il s’affichera à l’application plus tard, afin qu’il doit ont une signification pour l’application. Par exemple, il peut être le numéro du paramètre ou le handle d’un fichier contenant des données.  
  
2.  Passe l’adresse d’une mémoire tampon de longueur / d’indicateur dans le *StrLen_or_IndPtr* argument de **SQLBindParameter**.  
  
3.  Stocke le SQL_DATA_AT_EXEC ou le résultat de la SQL_LEN_DATA_AT_EXEC (*longueur*) (macro) dans la mémoire tampon de longueur / d’indicateur. Ces deux valeurs indiquent au pilote qui seront envoyées avec les données pour le paramètre **SQLPutData**. SQL_LEN_DATA_AT_EXEC (*longueur*) est utilisé lors de l’envoi des données de type long à une source de données qui a besoin de connaître le nombre d’octets de données de type long sera envoyé afin que pré-alloue espace. Pour déterminer si une source de données requiert cette valeur, l’application appelle **SQLGetInfo** avec l’option SQL_NEED_LONG_DATA_LEN. Tous les pilotes doivent prendre en charge cette macro ; Si la source de données ne requiert pas la longueur d’octet, le pilote peut l’ignorer.  
  
4.  Appels **SQLExecute** ou **SQLExecDirect**. Le pilote détecte qu’une mémoire tampon de longueur / d’indicateur contient la valeur SQL_DATA_AT_EXEC ou le résultat de la SQL_LEN_DATA_AT_EXEC (*longueur*) (macro) et retourne SQL_NEED_DATA comme valeur de retour de la fonction.  
  
5.  Appels **SQLParamData** en réponse à la SQL_NEED_DATA valeur de retour. Si les données de type long doivent être envoyé, **SQLParamData** retourne SQL_NEED_DATA. Dans la mémoire tampon vers laquelle pointée le *ValuePtrPtr* argument, le pilote retourne la valeur qui identifie le paramètre de données en cours d’exécution. S’il existe plusieurs paramètres de données d’exécution, l’application doit utiliser cette valeur pour déterminer quel paramètre pour envoyer des données le pilote n’est pas nécessaire pour demander des données pour les paramètres de data-at-execution dans un ordre particulier.  
  
6.  Appels **SQLPutData** pour envoyer les données du paramètre pour le pilote. Si les données du paramètre ne tient pas dans une mémoire tampon unique, comme c’est souvent le cas avec les données de type long, l’application appelle **SQLPutData** à plusieurs reprises pour envoyer les données de parties ; c’est à la source de données et le pilote pour rassembler les données. Si l’application transmet les données de chaîne terminée par null, la source de données ou de pilote doit supprimer le caractère de fin de la valeur null dans le cadre du processus de réassemblage.  
  
7.  Appels **SQLParamData** pour indiquer qu’il a envoyé à toutes les données pour le paramètre. S’il y a aucun paramètre de data-at-execution pour lequel les données n’ont pas été envoyées, le pilote retourne SQL_NEED_DATA et la valeur qui identifie le paramètre suivant ; l’application retourne à l’étape 6. Si les données ont été envoyées pour tous les paramètres à l’exécution, l’instruction est exécutée. **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO et peut retourne une valeur de retour ou diagnostic qui **SQLExecute** ou **SQLExecDirect** peut retourner.  
  
 Après avoir **SQLExecute** ou **SQLExecDirect** retourne SQL_NEED_DATA et avant l’envoi de données pour le dernier paramètre de data-at-execution a complètement, l’instruction est dans un état besoin des données. Alors qu’une instruction est dans un état besoin des données, l’application peut uniquement appeler **SQLPutData**, **SQLParamData**, **SQLCancel**, **SQLGetDiagField**, ou **SQLGetDiagRec**; toutes les autres fonctions retournent la valeur SQLSTATE HY010 (erreur de séquence de fonction). Appel de **SQLCancel** annule l’exécution de l’instruction et le retourne à son état précédent. Pour plus d’informations, consultez [Tables de Transition d’état annexe b : ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
 Pour obtenir un exemple d’envoi de données au moment de l’exécution, consultez la [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md) description de fonction.
