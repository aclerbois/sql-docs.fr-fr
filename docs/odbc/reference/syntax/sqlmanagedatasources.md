---
title: SQLManageDataSources | Documents Microsoft
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
- SQLManageDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLManageDataSources
helpviewer_keywords:
- SQLManageDataSources [ODBC]
ms.assetid: ac6d186f-b394-406c-94c4-c6331d1ca468
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 649d01dbea741c29d1e7d0ec6b63dfeb436caf3b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlmanagedatasources"></a>SQLManageDataSources
**Mise en conformité**  
 Version introduite : ODBC version 2.0  
  
 **Résumé**  
 **SQLManageDataSources** affiche une boîte de dialogue avec laquelle les utilisateurs peuvent configurer, ajouter et supprimer des sources de données dans les informations système.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLManageDataSources(  
     HWND     hwnd);  
```  
  
## <a name="arguments"></a>Arguments  
 *HWND*  
 [Entrée] Handle de fenêtre parente.  
  
## <a name="returns"></a>Valeur renvoyée  
 **SQLManageDataSources** retourne FALSE si *hwnd* n’est pas un handle de fenêtre valide. Sinon, elle retourne la valeur TRUE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLManageDataSources** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur peut être obtenue en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Erreur du programme d’installation générales|Une erreur s’est produite pour lequel aucune erreur d’installation spécifique s’est produite.|  
|ODBC_ERROR_REQUEST_FAILED|*Demande* a échoué|L’appel à **ConfigDSN** a échoué.|  
|ODBC_ERROR_INVALID__HWND|Handle de fenêtre non valide|Le *hwnd* argument était non valide ou NULL.|  
|ODBC_ERROR_OUT_OF_MEM|Mémoire insuffisante|Le programme d’installation n’a pas pu effectuer la fonction en raison d’un manque de mémoire.|  
  
## <a name="managing-data-sources"></a>Gestion des Sources de données  
 **SQLManageDataSources** affiche initialement les **administrateur de sources de données ODBC** boîte de dialogue, comme indiqué dans l’illustration suivante.  
  
 ![Boîte de dialogue Administrateur de sources de données ODBC](../../../odbc/reference/syntax/media/ch23e.gif "CH23E")  
  
 La boîte de dialogue affiche les sources de données répertoriées dans les informations système sous trois onglets : **DSN utilisateur**, **DSN système**, et **DSN de fichier**. Si l’utilisateur double-clique sur une source de données ou qu’il sélectionne une source de données et qu’il clique sur **configurer**, **SQLManageDataSources** appelle **ConfigDSN** dans la DLL d’installation avec l’option ODBC_CONFIG_DSN.  
  
 Si l’utilisateur clique sur **ajouter**, **SQLManageDataSources** affiche le **créer une nouvelle Source de données** boîte de dialogue, indiqué dans l’illustration suivante.  
  
 ![Créer la boîte de dialogue Nouvelle Source de données](../../../odbc/reference/syntax/media/ch23f.gif "CH23F")  
  
 La boîte de dialogue affiche une liste des pilotes installés. Si l’utilisateur double-clique sur un pilote ou sélectionne un pilote et clique sur **OK**, **SQLManageDataSources** appelle **ConfigDSN** dans la DLL d’installation et le passe à l’option ODBC_ADD_DSN.  
  
 Si l’utilisateur sélectionne une source de données et clique sur **supprimer**, **SQLManageDataSources** vous demande si l’utilisateur souhaite supprimer la source de données. Si l’utilisateur clique sur **Oui**, **SQLManageDataSources** appelle **ConfigDSN** dans la DLL d’installation avec l’option ODBC_REMOVE_DSN.  
  
 Le **créer une nouvelle Source de données** boîte de dialogue est utilisée pour ajouter ou supprimer une source de données utilisateur, d’une source de données système ou d’une source de données de fichier.  
  
## <a name="user-dsns"></a>Sources de données utilisateur  
 Sources de données, pour les distinguer des sources de données système, les sources de données créés pour les utilisateurs individuels seront appelées. Sources de données utilisateur sont enregistrés comme suit dans les informations système :  
  
 `HKEY_CURRENT_USERS`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbc.ini`  
  
## <a name="system-dsns"></a>Sources de données système  
 Le **créer une nouvelle Source de données** boîte de dialogue vous permet de pour ajouter une source de données système à votre ordinateur local ou le supprimer une, ou pour définir la configuration pour une source de données système.  
  
 Une source de données configurée avec un nom de source de données système (DSN) peut être utilisée par plusieurs utilisateurs sur le même ordinateur. Il peut également être utilisé par un service du système, qui peut alors accéder à la source de données même si aucun utilisateur n’est connecté à l’ordinateur.  
  
 Une source de données système est enregistré dans l’entrée HKEY_LOCAL_MACHINE dans les informations système plutôt que dans l’entrée HKEY_CURRENT_USER. Il n’est pas lié à un utilisateur qui se connecte avec son nom d’utilisateur et mot de passe, mais peut être utilisé par n’importe quel utilisateur de cet ordinateur ou par un service du système automatique. La source de données système est, toutefois, liée à un seul ordinateur. Il ne prend pas en charge la possibilité d’utiliser des sources de données à distance entre des ordinateurs. Sources de données système sont enregistrés comme suit dans les informations système :  
  
 HKEY_LOCAL_MACHINE SOFTWARE ODBC Odbc.ini  
  
## <a name="file-dsns"></a>Sources de données fichier  
 Une source de données de fichier n’a pas de nom de source de données, que les sources de données machine et n’est pas inscrit à n’importe quel ordinateur ou d’un utilisateur. Les informations de connexion pour cette source de données sont contenues dans un fichier .dsn qui peut être copié vers n’importe quel ordinateur. Une source de données de fichier peut être partageable, auquel cas le fichier .dsn réside sur un réseau et peut être utilisée simultanément par plusieurs utilisateurs sur le réseau tant que l’utilisateur a le pilote approprié installé. Une source de données de fichier peut également être partageable, auquel cas il peut être utilisé uniquement sur un ordinateur unique.  
  
 Pour plus d’informations sur les sources de données de fichier, consultez [la connexion à l’aide de Sources de données](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md), ou consultez [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="managing-drivers"></a>Gestion des pilotes  
 Si l’utilisateur clique sur le **pilotes** onglet dans le **administrateur de sources de données ODBC** boîte de dialogue, **SQLManageDataSources** affiche la liste des pilotes ODBC installés sur le système, ainsi que des informations sur les pilotes. La date affichée est la date de création du pilote, comme indiqué dans l’illustration suivante.  
  
 ![Onglet pilotes de l’administrateur de sources de données ODBC](../../../odbc/reference/syntax/media/ch23g.gif "ch23g")  
  
## <a name="tracing-options"></a>Options de suivi  
 Si l’utilisateur clique sur le **suivi** onglet dans le **administrateur de sources de données ODBC** boîte de dialogue, **SQLManageDataSources** affiche les options de suivi, comme indiqué dans l’illustration suivante.  
  
 ![Onglet traçage de l’administrateur de sources de données ODBC](../../../odbc/reference/syntax/media/ch23h.gif "Ch23h")  
  
 Si l’utilisateur clique sur **démarrer le traçage maintenant** , puis clique sur **OK**, **SQLManageDataSources** Active le suivi manuellement pour toutes les applications en cours d’exécution sur l’ordinateur.  
  
 Si l’utilisateur spécifie le nom d’un fichier de trace dans le **chemin d’accès du journal** zone de texte, puis clique sur **OK**, **SQLManageDataSources** définit le **TraceFile** mot clé dans la section [ODBC] des informations système pour le nom spécifié.  
  
> [!IMPORTANT]  
>  Prise en charge de Visual Studio Analyzer a été supprimée à compter de Windows 8 (Visual Studio Analyzer a été inclus uniquement dans les versions antérieures de Visual Studio.). Pour un autre mécanisme de résolution des problèmes, utilisez le suivi de l’offre.  
  
 Si l’utilisateur clique sur **démarrer Visual Studio Analyzer** , puis clique sur **OK**, Visual Studio Analyzer est activé. Il reste activée jusqu'à ce que **arrêter Visual Studio Analyzer** est activé.  
  
 Pour plus d’informations sur le suivi, consultez [suivi](../../../odbc/reference/develop-app/tracing.md). Pour plus d’informations sur la **Trace** et **TraceFile** mots clés, consultez [ODBC sous-clé](../../../odbc/reference/install/odbc-subkey.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Création de sources de données|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|
