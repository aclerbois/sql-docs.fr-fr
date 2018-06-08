---
title: srv_describe (API de procédure stockée étendue) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
- srv_describe
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_describe
ms.assetid: 2115600e-5ce7-4be0-9cd3-a1dd1fab0729
caps.latest.revision: 33
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 432a53228022c2f332758252520f980c3faa5436
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="srvdescribe-extended-stored-procedure-api"></a>srv_describe (API de procédure stockée étendue)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Définit le nom de colonne et les types de données sources et de destination pour une colonne spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_describe (  
SRV_PROC *  
srvproc  
,  
int  
colnumber  
,  
DBCHAR *  
column_name  
,  
int  
namelen  
,  
DBINT  
desttype  
,  
DBINT  
destlen  
,  
DBINT  
srctype  
,  
DBINT  
srclen  
,  
void *  
srcdata  
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui est le handle d'une connexion cliente particulière (dans ce cas, le client qui envoie la ligne). La structure contient toutes les informations que la bibliothèque d'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client.  
  
 *colnumber*  
 N’est pas pris en charge. Les colonnes doivent être décrites dans l'ordre. Toutes les colonnes doivent être décrites avant que **srv_sendrow** ne soit appelé.  
  
 *column_name*  
 Spécifie le nom de la colonne à laquelle les données appartiennent. Ce paramètre peut être NULL car il n'est pas obligatoire qu'une colonne ait un nom.  
  
 *namelen*  
 Spécifie la longueur, en octets, de *column_name*. Si *namelen* est SRV_NULLTERM, *column_name* doit se terminer par null.  
  
 *desttype*  
 Spécifie le type de données de la colonne de ligne de destination. Il s'agit du type de données envoyé au client. Le type de données doit être spécifié même si les données sont NULL. Pour plus d’informations, consultez [Types de données &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/data-types-extended-stored-procedure-api.md).  
  
 *destlen*  
 Spécifie la longueur, en octets, des données à envoyer au client. Pour les types de données de longueur fixe qui n’autorisent pas de valeurs NULL, *destlen* est ignoré. Pour les types de données de longueur variable et les types de données de longueur fixe qui autorisent les valeurs NULL, *destlen* spécifie la longueur maximale des données de destination.  
  
 *srctype*  
 Spécifie le type de données des données sources.  
  
 *srclen*  
 Spécifie la longueur, en octets, des données sources. Cette valeur est ignorée pour les types de données de longueur fixe.  
  
 *srcdata*  
 Fournit l'adresse des données sources pour une colonne particulière. Quand **srv_sendrow** est appelé, il recherche les données pour *colnumber* à *srcdata*. Ainsi, il ne doit pas être libéré avant un appel à **srv_sendrow**. Vous pouvez changer l’adresse des données sources entre des appels à **srv_sendrow** à l’aide de **srv_setcoldata**. La mémoire allouée pour *srcdata* ne doit pas être libérée tant que les données de la colonne ne sont pas remplacées par un autre appel à **srv_setcoldata** ou tant que **srv_senddone** n’est pas appelé.  
  
 Si *desttype* est SRVDECIMAL ou SRVNUMERIC, le paramètre *srcdata* doit être un pointeur vers une structure DBNUMERIC ou DBDECIMAL avec les champs de précision et d’échelle de la structure déjà définis aux valeurs que vous souhaitez. Vous pouvez utiliser DEFAULTPRECISION pour spécifier une précision par défaut et DEFAULTSCALE pour spécifier une échelle par défaut.  
  
## <a name="returns"></a>Valeur renvoyée  
 Numéro de la colonne décrite. La première colonne est la colonne 1. Si une erreur se produit, retourne 0.  
  
## <a name="remarks"></a>Notes   
 La fonction **srv_describe** doit être appelée une fois pour chaque colonne dans la ligne avant le premier appel à **srv_sendrow**. Les colonnes d'une ligne peuvent être décrites dans n'importe quel ordre.  
  
 Pour changer l’emplacement et la longueur des données sources dans les lignes de colonnes avant d’envoyer le jeu de résultats complet, utilisez **srv_setcoldata** et **srv_setcollen**, respectivement.  
  
 Pour une description des types de données et des conversions de types de données d’API de procédure stockée étendue, consultez [Types de données &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/data-types-extended-stored-procedure-api.md).  
  
 Si le nom de colonne dans votre application est en Unicode, vous devez le convertir en page de codes multioctets du serveur avant d’appeler **srv_describe**. Pour plus d’informations, consultez [Données Unicode et pages de codes du serveur](../../relational-databases/extended-stored-procedures-programming/unicode-data-and-server-code-pages.md).  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](http://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409http://msdn.microsoft.com/security/).  
  
## <a name="see-also"></a> Voir aussi  
 [srv_sendrow &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-sendrow-extended-stored-procedure-api.md)   
 [srv_setutype &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-setutype-extended-stored-procedure-api.md)   
 [srv_setcoldata &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-setcoldata-extended-stored-procedure-api.md)  
  
  
