---
title: srv_sendmsg (API de procédure stockée étendue) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
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
- srv_sendmsg
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_sendmsg
ms.assetid: efcb50b9-f8ff-4121-bf67-05830171b928
caps.latest.revision: 30
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 31c558a3deb0080c11b8ba4f055f7341fd1a4443
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="srvsendmsg-extended-stored-procedure-api"></a>srv_sendmsg (API de procédure stockée étendue)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Envoie un message au client.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_sendmsg (  
SRV_PROC *  
srvproc  
,  
int  
msgtype  
,  
DBINT  
msgnum  
,  
DBTINYINT  
class  
,   
DBTINYINT  
state  
,  
DBCHAR *  
rpcname  
,  
int   
rpcnamelen  
,  
DBUSMALLINT  
linenum  
,  
DBCHAR *  
message  
,  
int  
msglen   
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui est le handle d'une connexion cliente particulière (dans ce cas, le handle qui a reçu la demande de langue). La structure contient des informations que la bibliothèque d'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client.  
  
 *msgtype*  
 SRV_MSG_INFO ou SRV_MSG_ERROR, selon que le serveur envoie un message d'information ou un message d'erreur.  
  
 *msgnum*  
 Numéro de message à 4 octets.  
  
 *class*  
 Spécifie la gravité de l'erreur. Une gravité inférieure ou égale à 10 est considérée comme un message d'information.  
  
 *state*  
 Fournit le numéro d'état de l'erreur pour le message actuel. Le numéro d'état de l'erreur fournit des informations sur le contexte de l'erreur. Les numéros d'état valides sont compris entre 0 et 255.  
  
 *rpcname*  
 N’est pas pris en charge.  
  
 *rpcnamelen*  
 N’est pas pris en charge.  
  
 *linenum*  
 Numéro de ligne dans le lot de commandes du langage auquel le message s'applique. La numérotation des lignes débute à 1. Si *linenum* ne s’applique pas au message, définissez cette valeur sur 0.  
  
 *message*  
 Pointeur vers la chaîne de caractères à envoyer au client.  
  
 *msglen*  
 Spécifie la longueur, en octets, de *message*. Si *message* se termine par le caractère NULL, définissez *msglen* sur SRV_NULLTERM.  
  
## <a name="returns"></a>Valeur renvoyée  
 SUCCEED ou FAIL  
  
## <a name="remarks"></a>Notes   
 Cette fonction envoie des messages d'erreur ou d'information au client. Elle est appelée une fois pour chaque message à envoyer.  
  
 Les messages peuvent être envoyés au client avec **srv_sendmsg** dans n’importe quel ordre avant ou après que toutes les lignes (le cas échéant) ont été envoyées avec **srv_sendrow**. Tous les messages, s’il en existe, doivent être envoyés au client avant que l’état d’achèvement soit envoyé avec **srv_senddone**.  
  
 Pour envoyer des messages en Unicode, utilisez **srv_wsendmsg** à la place de **srv_sendmsg**.  
  
 Pour plus d’informations, consultez [Données Unicode et pages de codes du serveur](../../relational-databases/extended-stored-procedures-programming/unicode-data-and-server-code-pages.md).  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](http://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409http://msdn.microsoft.com/security/).  
  
  
