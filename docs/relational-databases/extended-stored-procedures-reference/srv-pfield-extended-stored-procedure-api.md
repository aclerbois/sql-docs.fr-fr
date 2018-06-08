---
title: srv_pfield (API de procédure stockée étendue) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: extended-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- srv_pfield
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_pfield
ms.assetid: a61e4c1f-e65b-48ea-a7d1-3e1544af389d
caps.latest.revision: 32
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: b99afab30584db1a13444f2d259ffafa0459c967
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="srvpfield-extended-stored-procedure-api"></a>srv_pfield (API de procédure stockée étendue)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Retourne des informations sur une connexion de base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DBCHAR * srv_pfield (  
SRV_PROC *  
srvproc  
,  
int   
field  
,  
int *  
len  
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur identifiant une connexion de base de données.  
  
 *field*  
 Spécifie des données sur la connexion à retourner.  
  
|Valeur|Valeur renvoyée|  
|-----------|-------------|  
|SRV_APPLNAME|Nom d'application fourni par le client au moment d'établir la connexion.|  
|SRV_BCPFLAG|Indicateur possédant la valeur TRUE si le client prépare une opération de copie en bloc ou la valeur FALSE dans le cas contraire.|  
|SRV_CLIB|Nom de la bibliothèque permettant au client de communiquer avec un serveur.|  
|SRV_CPID|ID de processus client sur l'ordinateur source client.|  
|SRV_HOST|Nom de l'ordinateur client fourni par le client au moment d'établir la connexion.|  
|SRV_LIBVERS|Version de la bibliothèque cliente.|  
|SRV_LSECURE|Indicateur. Possède la valeur TRUE si la connexion a été établie au moyen de la sécurité intégrée.|  
|SRV_NETWORK_MODULE|Nom de la DLL Net-Library utilisée par la connexion.|  
|SRV_NETWORK_VERSION|Version de la DLL Net-Library utilisée par la connexion.|  
|SRV_NETWORK_CONNECTION|Chaîne de connexion passée à la DLL Net-library employée pour la connexion *srvproc* actuelle.|  
|SRV_PIPEHANDLE|Chaîne contenant le handle de canal d'un client connecté ou la valeur NULL si le client est connecté sur un réseau qui n'utilise pas de canaux nommés. Pour utiliser ce handle comme handle de canal valide avec [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, convertissez cette chaîne en entier.|  
|SRV_RMTSERVER|Serveur à partir duquel le processus client est connecté. Si la connexion provient d'un client, cette valeur est une chaîne vide.|  
|SRV_ROWSENT|Nombre de lignes déjà transmises par *srvproc* pour le jeu de résultats actuel.|  
|SRV_SPID|ID de thread sur le serveur de *srvproc*. Pour les procédures stockées étendues, cette valeur est la même que celle de la colonne **kpid** de **sys.sysprocesses** et peut changer par la suite.|  
|SRV_SPROC_CODEPAGE|Page de codes employée par le serveur pour interpréter des données multioctets.|  
|SRV_STATUS|État actuel de *srvproc* : en cours d’exécution ou fermé.|  
|SRV_TYPE|Type de connexion de *srvproc*. Si le serveur est retourné, *srvproc* provient d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le client est retourné, *srvproc* provient d’une bibliothèque de bases de données ou d’un client ODBC.|  
|SRV_USER|Nom de l'utilisateur de la connexion.|  
|||  
  
 *len*  
 Pointeur vers une variable **int** contenant la longueur de la valeur *field* retournée. Si *len* a la valeur NULL; la longueur de la chaîne n’est pas retournée.  
  
## <a name="returns"></a>Valeur renvoyée  
 Pointeur vers une chaîne terminée par le caractère NULL contenant la valeur actuelle du champ spécifié dans la structure SRV_PROC. Si le champ est vide, un pointeur valide vers une chaîne vide est retourné et *len* contient la valeur 0. Si le champ est inconnu, la valeur NULL est retournée et *len* contient la valeur -1.  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d’informations sur cet examen et sur les tests de sécurité, consultez le site [TechCenter sur la sécurité](http://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409http://msdn.microsoft.com/security/).  
  
  
