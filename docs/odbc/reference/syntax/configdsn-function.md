---
title: Fonction de ConfigDSN | Documents Microsoft
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
- ConfigDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDSN
helpviewer_keywords:
- ConfigDSN [ODBC]
ms.assetid: 01ced74e-c575-4a25-83f5-bd7d918123f8
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7a8b75fb1b87a4f6199999e5d5e33d8cd0083bac
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configdsn-function"></a>ConfigDSN (fonction)
**Mise en conformité**  
 Version introduite : ODBC 1.0  
  
 **Résumé**  
 **ConfigDSN** ajoute, modifie ou supprime des sources de données à partir des informations système. Il peut inviter l’utilisateur pour les informations de connexion. Il peut être dans la DLL du pilote ou une DLL d’installation distinct.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL ConfigDSN(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>Arguments  
 *hwndParent*  
 [Entrée] Handle de fenêtre parente. La fonction n’affichera pas les boîtes de dialogue si le handle est null.  
  
 *fréquents*  
 [Entrée] Type de requête. Le *fréquents* argument doit contenir l’une des valeurs suivantes :  
  
 ODBC_ADD_DSN : Ajouter une nouvelle source de données.  
  
 ODBC_CONFIG_DSN : Configurer (modifier) une source de données existante.  
  
 ODBC_REMOVE_DSN : Supprimer une source de données existante.  
  
 *lpszDriver*  
 [Entrée] Description du pilote (généralement le nom du SGBD associé) présentée à l’utilisateur au lieu du nom de pilote physique.  
  
 *lpszAttributes*  
 [Entrée] Liste d’attributs sous la forme de paires mot clé-valeur doublement se terminant par null. Pour plus d’informations, consultez « Commentaires ».  
  
## <a name="returns"></a>Valeur renvoyée  
 La fonction retourne TRUE si l’opération a réussi, FALSE en cas d’échec.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **ConfigDSN** renvoie la valeur FALSE, associé à un  *\*pfErrorCode* valeur est validée dans la mémoire tampon erreur de programme d’installation par un appel à **SQLPostInstallerError** et peut être obtenu en appelant **SQLInstallerError**. Le tableau suivant répertorie les  *\*pfErrorCode* les valeurs qui peuvent être retournées par **SQLInstallerError** et explique chacune d’elles dans le contexte de cette fonction.  
  
|*\*pfErrorCode*|Erreur| Description|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|Handle de fenêtre non valide|Le *hwndParent* argument n’était pas valide.|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|Paires mot clé-valeur non valide|Le *lpszAttributes* argument contenait une erreur de syntaxe.|  
|ODBC_ERROR_INVALID_NAME|Nom de pilote ou de convertisseur non valide|Le *lpszDriver* argument n’était pas valide. Il est introuvable dans le Registre.|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|Type de demande non valide|Le *fréquents* argument n’est pas une des opérations suivantes :<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN|  
|ODBC_ERROR_REQUEST_FAILED|*Demande* a échoué|Impossible d’effectuer l’opération demandée par le *fréquents* argument.|  
|ODBC_ERROR_DRIVER_SPECIFIC|Erreur spécifique du pilote ou du traducteur|Une erreur spécifique au pilote pour lequel il n’existe aucune erreur de programme d’installation ODBC définie. Le *SzError* argument dans un appel à la **SQLPostInstallerError** la fonction doit contenir le message d’erreur spécifique au pilote.|  
  
## <a name="comments"></a>Commentaires  
 **ConfigDSN** reçoit des informations de connexion à partir de la DLL de programme d’installation en tant que liste d’attributs sous la forme de paires mot clé-valeur. Chaque paire se termine par un octet null, et la liste entière se termine par un octet null. (Autrement dit, deux octets null marquent la fin de la liste.) Les espaces ne sont pas autorisés autour du signe égal dans la paire mot clé-valeur. **ConfigDSN** peut accepter des mots clés qui ne sont pas des mots clés valides de **SQLBrowseConnect** et **SQLDriverConnect**. **ConfigDSN** ne prend en charge n’est pas nécessairement tous les mots clés qui sont des mots clés valides de **SQLBrowseConnect** et **SQLDriverConnect**. (**ConfigDSN** n’accepte pas les **pilote** (mot clé).) Les mots clés utilisés par le **ConfigDSN** fonction doit prendre en charge toutes les options requises pour recréer la source de données à l’aide de la fonctionnalité de configuration automatique du programme d’installation. Lorsque les utilisations de la **ConfigDSN** valeurs et les valeurs de chaîne de connexion sont les mêmes, les mêmes mots clés doivent être utilisés.  
  
 Comme dans **SQLBrowseConnect** et **SQLDriverConnect**, les mots clés et leurs valeurs ne doivent pas contenir le **[]{}(), ? \*= ! @** caractères et la valeur de la **DSN** mot clé ne peut pas se composer uniquement d’espaces. En raison de la grammaire du Registre, les noms de sources de données et les mots clés ne peut pas contenir la barre oblique inverse (\\) caractères.  
  
 **ConfigDSN** doit appeler **SQLValidDSN** pour vérifier la longueur du nom de la source de données et vérifiez qu’aucun caractère non valide n’est inclus dans le nom. Si le nom de source de données est supérieure à SQL_MAX_DSN_LENGTH ou contient des caractères non valides, **SQLValidDSN** renvoie une erreur et **ConfigDSN** renvoie une erreur. La longueur du nom de la source de données est également vérifiée par **SQLWriteDSNToIni**.  
  
 Par exemple, pour configurer une source de données qui nécessite un ID utilisateur, le mot de passe et le nom de la base de données, une application d’installation peut transmettre les paires mot clé-valeur suivantes :  
  
```  
DSN=Personnel Data\0UID=Smith\0PWD=Sesame\0DATABASE=Personnel\0\0  
```  
  
 Pour plus d’informations sur ces mots clés, consultez [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) et une documentation de chaque pilote.  
  
 Pour afficher une boîte de dialogue *hwndParent* ne doit pas être null.  
  
## <a name="adding-a-data-source"></a>Ajout d'une source de données  
 Si un nom de source de données est passé à **ConfigDSN** dans *lpszAttributes*, **ConfigDSN** vérifie que le nom est valide. Si le nom de source de données correspond à un nom de source de données existant et *hwndParent* a la valeur null, **ConfigDSN** remplace le nom existant. Si elle correspond à un nom existant et *hwndParent* n’est pas null, **ConfigDSN** invite l’utilisateur à remplacer le nom existant.  
  
 Si *lpszAttributes* contient suffisamment d’informations pour vous connecter à une source de données, **ConfigDSN** pouvez ajouter les données source ou affichent une boîte de dialogue avec laquelle l’utilisateur peut modifier les informations de connexion. Si *lpszAttributes* ne contient pas suffisamment d’informations pour vous connecter à une source de données, **ConfigDSN** doit déterminer les informations nécessaires ; si *hwndParent* est non null, il affiche une boîte de dialogue pour récupérer les informations de l’utilisateur.  
  
 Si **ConfigDSN** affiche une boîte de dialogue, il doit afficher les informations de connexion qui lui est passées dans *lpszAttributes*. En particulier, si un nom de source de données a été passé, **ConfigDSN** affiche ce nom, mais n’autorise pas l’utilisateur à modifier. **ConfigDSN** peut fournir des valeurs par défaut pour les informations de connexion ne pas passé dans *lpszAttributes*.  
  
 Si **ConfigDSN** ne peut pas obtenir les informations de connexion complète pour une source de données, elle retourne FALSE.  
  
 Si **ConfigDSN** peut obtenir des informations de connexion complète pour une source de données, il appelle **SQLWriteDSNToIni** dans le programme d’installation du fichier DLL pour ajouter la nouvelle spécification de source de données pour le fichier Odbc.ini (ou le Registre). **SQLWriteDSNToIni** ajoute le nom de source de données à la section [ODBC Data Sources], crée la section de spécification de source de données et ajoute les **pilote** mot clé avec la description du pilote en tant que sa valeur. **ConfigDSN** appelle **SQLWritePrivateProfileString** dans le programme d’installation de la DLL pour ajouter des mots clés supplémentaires et les valeurs utilisées par le pilote.  
  
## <a name="modifying-a-data-source"></a>Modification d’une Source de données  
 Pour modifier une source de données, un nom de source de données doit être passé à **ConfigDSN** dans *lpszAttributes*. **ConfigDSN** vérifie que le nom de source de données est dans le fichier Odbc.ini (ou le Registre).  
  
 Si *hwndParent* a la valeur null, **ConfigDSN** utilise les informations contenues dans *lpszAttributes* pour modifier les informations contenues dans le fichier Odbc.ini (ou le Registre). Si *hwndParent* n’est pas null, **ConfigDSN** affiche une boîte de dialogue en utilisant les informations dans *lpszAttributes*; pour plus d’informations ne figurant pas dans *lpszAttributes*, il utilise les informations à partir des informations système. L’utilisateur peut modifier les informations avant de **ConfigDSN** stocke les informations système.  
  
 Si le nom de source de données a été modifié, **ConfigDSN** appelle d’abord **SQLRemoveDSNFromIni** dans le programme d’installation de la DLL pour supprimer les données existantes source spécification dans le fichier Odbc.ini (ou le Registre). Ensuite, il suit les étapes décrites dans la section précédente pour ajouter la nouvelle spécification de source de données. Si le nom de source de données n’a pas été modifié, **ConfigDSN** appelle **SQLWritePrivateProfileString** dans le programme d’installation de la DLL apporter d’autres modifications. **ConfigDSN** ne peut pas supprimer ou modifier la valeur de la **pilote** (mot clé).  
  
## <a name="deleting-a-data-source"></a>Suppression d’une Source de données  
 Pour supprimer une source de données, un nom de source de données doit être passé à **ConfigDSN** dans *lpszAttributes*. **ConfigDSN** vérifie que le nom de source de données est dans le fichier Odbc.ini (ou le Registre). Il appelle ensuite **SQLRemoveDSNFromIni** dans le programme d’installation de la DLL pour supprimer la source de données.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Ajout, modification ou suppression d’une source de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|Obtention d’une valeur à partir du fichier Odbc.ini ou le Registre|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|  
|Suppression de la source de données par défaut|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|  
|Suppression d’un nom de source de données Odbc.ini (ou du Registre)|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|Ajout d’un nom de source de données au fichier Odbc.ini (ou au Registre)|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|  
|Écriture d’une valeur dans le fichier Odbc.ini ou le Registre|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
