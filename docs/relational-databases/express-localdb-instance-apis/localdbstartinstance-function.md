---
title: Fonction LocalDBStartInstance | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: localdb
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- LocalDBStartInstance
apilocation:
- sqluserinstance.dll
apitype: DLLExport
ms.assetid: cb325f5d-10ee-4a56-ba28-db0074ab3926
caps.latest.revision: 17
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 3d28685fa83098d6d5a743d06e99e21ffc8604c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="localdbstartinstance-function"></a>Fonction LocalDBStartInstance
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Démarre l'instance SQL Server Express LocalDB spécifiée.  
  
 **Fichier d'en-tête :** sqlncli.h  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT LocalDBStartInstance(  
           PCWSTR pInstanceName,  
           DWORD dwFlags,   
           LPWSTR wszSqlConnection,   
           LPDWORD lpcchSqlConnection   
);  
```  
  
## <a name="parameters"></a>Paramètres  
 *pInstanceName*  
 [Entrée] Nom de l'instance de LocalDB à démarrer.  
  
 *dwFlags*  
 [Entrée] Réservé à un usage ultérieur. Actuellement doit avoir la valeur 0.  
  
 *wszSqlConnection*  
 [Sortie] Mémoire tampon pour stocker la chaîne de connexion à l'instance de LocalDB.  
  
 *lpcchSqlConnection*  
 [Entrée/sortie] En entrée contient la taille de la mémoire tampon de *wszSqlConnection* en caractères, y compris les zéros de fin. En sortie, si la taille de la mémoire tampon donnée est trop petite, contient la taille de la mémoire tampon requise en caractères, y compris les zéros de fin.  
  
## <a name="returns"></a>Valeur renvoyée  
 Cette méthode signale les erreurs en attribuant à la propriété Nombre de l'objet Err global l'une des valeurs du tableau suivant.  
 La fonction a réussi.  
  
 [LOCALDB_ERROR_NOT_INSTALLED](../../relational-databases/express-localdb-error-messages/localdb-error-not-installed.md)  
 SQL Server Express LocalDB n'est pas installé sur l'ordinateur.  
  
 [LOCALDB_ERROR_INVALID_PARAMETER](../../relational-databases/express-localdb-error-messages/localdb-error-invalid-parameter.md)  
 Un ou plusieurs paramètres d'entrée spécifiés ne sont pas valides.  
  
 [LOCALDB_ERROR_INVALID_INSTANCE_NAME](../../relational-databases/express-localdb-error-messages/localdb-error-invalid-instance-name.md)  
 Le nom d'instance spécifié n'est pas valide.  
  
 [LOCALDB_ERROR_UNKNOWN_INSTANCE](../../relational-databases/express-localdb-error-messages/localdb-error-unknown-instance.md)  
 L'instance n'existe pas.  
  
 [LOCALDB_ERROR_INSUFFICIENT_BUFFER](../../relational-databases/express-localdb-error-messages/localdb-error-insufficient-buffer.md)  
 Le tampon de *wszSqlConnection* spécifié est trop petit.  
  
 [LOCALDB_ERROR_WAIT_TIMEOUT](../../relational-databases/express-localdb-error-messages/localdb-error-wait-timeout.md)  
 Un dépassement de délai s'est produit lors de la tentative d'acquisition des verrous de synchronisation.  
  
 [LOCALDB_ERROR_INSTANCE_FOLDER_PATH_TOO_LONG](../../relational-databases/express-localdb-error-messages/localdb-error-instance-folder-path-too-long.md)  
 Le chemin d'accès où l'instance doit être stockée est plus long que MAX_PATH.  
  
 [LOCALDB_ERROR_CANNOT_GET_USER_PROFILE_FOLDER](../../relational-databases/express-localdb-error-messages/localdb-error-cannot-get-user-profile-folder.md)  
 Impossible de récupérer un dossier du profil utilisateur.  
  
 [LOCALDB_ERROR_CANNOT_ACCESS_INSTANCE_FOLDER](../../relational-databases/express-localdb-error-messages/localdb-error-cannot-access-instance-folder.md)  
 Un dossier d'instance n'est pas accessible.  
  
 [LOCALDB_ERROR_CANNOT_ACCESS_INSTANCE_REGISTRY](../../relational-databases/express-localdb-error-messages/localdb-error-cannot-access-instance-registry.md)  
 Un Registre d'instance n'est pas accessible.  
  
 [LOCALDB_ERROR_CANNOT_MODIFY_INSTANCE_REGISTRY](../../relational-databases/express-localdb-error-messages/localdb-error-cannot-modify-instance-registry.md)  
 Impossible de modifier un Registre d'instance.  
  
 [LOCALDB_ERROR_CANNOT_CREATE_SQL_PROCESS](../../relational-databases/express-localdb-error-messages/localdb-error-cannot-create-sql-process.md)  
 Impossible de créer un processus pour SQL Server.  
  
 [LOCALDB_ERROR_SQL_SERVER_STARTUP_FAILED](../../relational-databases/express-localdb-error-messages/localdb-error-sql-server-startup-failed.md)  
 Un processus SQL Server a été démarré, mais le démarrage de SQL Server a échoué.  
  
 [LOCALDB_ERROR_INSTANCE_CONFIGURATION_CORRUPT](../../relational-databases/express-localdb-error-messages/localdb-error-instance-configuration-corrupt.md)  
 Une configuration d'instance a été endommagée.  
  
 [LOCALDB_ERROR_AUTO_INSTANCE_CREATE_FAILED](../../relational-databases/express-localdb-error-messages/localdb-error-auto-instance-create-failed.md)  
 Impossible de créer une instance automatique. Pour plus d'informations sur l'erreur, consultez le journal des événements des applications Windows.  
  
 [LOCALDB_ERROR_INTERNAL_ERROR](../../relational-databases/express-localdb-error-messages/localdb-error-internal-error.md)  
 Une erreur inattendue s'est produite. Pour plus d'informations, consultez le journal des événements.  
  
## <a name="details"></a>Détails  
 L'argument de mémoire tampon de connexion (*wszSqlConnection*) et l'argument de taille de mémoire tampon de connexion (*lpcchSqlConnection*) sont facultatifs. Le tableau suivant affiche les options d'utilisation de ces arguments et leurs résultats.  
  
|Buffer|buffer_size|Rationale|Action|  
|------------|-----------------|---------------|------------|  
|NULL|NULL|L'utilisateur veut démarrer l'instance et n'a pas besoin d'un nom de canal.|Démarre une instance (aucun retour de canal et aucun retour obligatoire de taille de mémoire tampon).|  
|NULL|Présent|L'utilisateur demande la taille de la mémoire tampon de sortie. (Dans l'appel suivant l'utilisateur demandera probablement un début réel.)|Retourne une taille de mémoire tampon requise (aucun début et aucun retour de canal). Le résultat est S_OK.|  
|Présent|NULL|Non autorisé ; entrée incorrecte.|LOCALDB_ERROR_INVALID_PARAMETER est retourné.|  
|Présent|Présent|L'utilisateur veut démarrer l'instance et a besoin du nom de canal pour s'y connecter après qu'il a été démarré.|Vérifie la taille de la mémoire tampon, démarre l'instance, puis retourne le nom de canal dans la mémoire tampon. <br />L'argument de taille de mémoire tampon retourne la longueur de la chaîne de « server= », sans inclure les zéros de fin.|  
  
 Pour un exemple de code qui utilise l'API LocalDB, consultez [SQL Server Express LocalDB Reference](../../relational-databases/sql-server-express-localdb-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [En-tête et informations de version de la base de données locale SQL Server Express](../../relational-databases/express-localdb-instance-apis/sql-server-express-localdb-header-and-version-information.md)  
  
  
