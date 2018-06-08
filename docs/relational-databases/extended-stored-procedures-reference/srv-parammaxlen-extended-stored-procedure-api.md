---
title: srv_parammaxlen (API de procédure stockée étendue) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- srv_parammaxlen
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_parammaxlen
ms.assetid: 49bfc29d-f76a-4963-b0e6-b8532dfda850
caps.latest.revision: 32
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 6a59c20ed8447e5c930e88b7a259bc63c95a3c8b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="srvparammaxlen-extended-stored-procedure-api"></a>srv_parammaxlen (API de procédure stockée étendue)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Retourne la longueur de données maximale d'un paramètre d'appel de procédure stockée distante. Cette fonction a été remplacée par la fonction **srv_paraminfo**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_parammaxlen (  
SRV_PROC *  
srvproc  
,  
int  
n   
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui correspond au handle d'une connexion cliente particulière (dans ce cas, le handle qui a reçu l'appel de procédure stockée distante). La structure contient des informations que la bibliothèque d'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client.  
  
 *n*  
 Indique le numéro du paramètre. Le premier paramètre est 1.  
  
## <a name="returns"></a>Valeur renvoyée  
 Longueur maximale, en octets, des données du paramètre. En l’absence du *n*ième paramètre ou d’une procédure stockée distante, la valeur -1 est retournée.  
  
 Cette fonction retourne les valeurs suivantes, si le paramètre est l’un des types de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivants.  
  
|Nouveaux types de données|Longueur de données d'entrée|  
|--------------------|-----------------------|  
|**BITN**|**NULL :** 1<br /><br /> **ZERO :** 1<br /><br /> **>=255 :** N/A<br /><br /> **<255 :** N/A|  
|**BIGVARCHAR**|**NULL :** 255<br /><br /> **ZERO :** 255<br /><br /> **>=255 :** 255<br /><br /> **<255 :** 255|  
|**BIGCHAR**|**NULL :** 255<br /><br /> **ZERO :** 255<br /><br /> **>=255 :** 255<br /><br /> **<255 :** 255|  
|**BIGBINARY**|**NULL :** 255<br /><br /> **ZERO :** 255<br /><br /> **>=255 :** 255<br /><br /> **<255 :** 255|  
|**BIGVARBINARY**|**NULL :** 255<br /><br /> **ZERO :** 255<br /><br /> **>=255 :** 255<br /><br /> **<255 :** 255|  
|**NCHAR**|**NULL :** 255<br /><br /> **ZERO :** 255<br /><br /> **>=255 :** 255<br /><br /> **<255 :** 255|  
|**NVARCHAR**|**NULL :** 255<br /><br /> **ZERO :** 255<br /><br /> **>=255 :** 255<br /><br /> **<255 :** 255|  
|**NTEXT**|**NULL :** -1<br /><br /> **ZERO :** -1<br /><br /> **>=255:** -1<br /><br /> **\<255:** -1|  
  
## <a name="remarks"></a>Notes   
 Chaque paramètre de procédure stockée distante possède une longueur de données réelle et une longueur de données maximale. Pour les types de données de longueur fixe standard qui n'autorisent pas les valeurs NULL, les longueurs réelle et maximale sont les mêmes. Pour les types de données de longueur variable, les longueurs peuvent varier. Par exemple, un paramètre déclaré en tant que **varchar(30)** peut posséder des données longues de 10 octets seulement. La longueur réelle du paramètre est 10 et sa longueur maximale est 30. La fonction **srv_parammaxlen** obtient la longueur de données maximale d’une procédure stockée distante. Pour obtenir la longueur réelle d’un paramètre, utilisez **srv_paramlen**.  
  
 Quand un appel de procédure stockée distante est effectué avec des paramètres, ceux-ci peuvent être passés par nom ou par position (sans nom). Si l'appel de procédure stockée distante est effectué avec certains paramètres passés par nom et certains passés par position, une erreur se produit. Le gestionnaire SRV_RPC est tout de même appelé, mais il apparaît comme s’il n’y avait aucun paramètre et **srv_rpcparams** retourne 0.  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](http://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409http://msdn.microsoft.com/security/).  
  
## <a name="see-also"></a> Voir aussi  
 [srv_paraminfo &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-paraminfo-extended-stored-procedure-api.md)   
 [srv_rpcparams &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-rpcparams-extended-stored-procedure-api.md)  
  
  
