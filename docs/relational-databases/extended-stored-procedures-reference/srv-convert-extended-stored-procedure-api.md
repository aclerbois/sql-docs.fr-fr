---
title: srv_convert (API de procédure stockée étendue) | Microsoft Docs
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
- srv_convert
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_convert
ms.assetid: 216b4a31-786e-4361-8a33-e5f6e9790f90
caps.latest.revision: 33
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 4812174e3a736a05475ceaf901bc86bf7949436d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="srvconvert-extended-stored-procedure-api"></a>srv_convert (API de procédure stockée étendue)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Modifie des données d'un type de données en un autre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
int srv_convert (  
SRV_PROC *  
srvproc  
,  
int  
srctype  
,  
void *  
src  
,  
DBINT  
srclen  
,  
int  
desttype  
,  
void *  
  dest  
,  
DBINT  
destlen  
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srvproc*  
 Pointeur vers la structure SRV_PROC qui est le handle pour une connexion cliente particulière. La structure contient toutes les informations de contrôle que l'API de procédure stockée étendue utilise pour gérer les communications et les données entre l'application et le client. Si le handle *srvproc* est fourni, il est passé à la fonction du gestionnaire d’erreurs d’API de procédure stockée étendue quand une erreur se produit.  
  
 *srctype*  
 Spécifie le type des données à convertir. Ce paramètre peut être n'importe lequel des types de données d'API de procédure stockée étendue.  
  
 *src*  
 Pointeur vers les données à convertir. Ce paramètre peut être n'importe lequel des types de données d'API de procédure stockée étendue.  
  
 *srclen*  
 Spécifie la longueur, en octets, des données à convertir. Si *srclen* est 0, **srv_convert** place une valeur NULL dans la variable de destination. À moins d'être égal à 0, ce paramètre est ignoré pour les types de données de longueur fixe, auquel cas les données sources sont supposées être NULL. Pour les données du type de données SRVCHAR, une longueur de -1 indique que la chaîne se termine par NULL.  
  
 *desttype*  
 Spécifie le type de données vers lequel convertir la source. Ce paramètre peut être n'importe lequel des types de données d'API de procédure stockée étendue.  
  
 *dest*  
 Pointeur vers la variable de destination qui reçoit les données converties. Si ce pointeur est NULL, **srv_convert** appelle le gestionnaire d’erreurs fourni par l’utilisateur, le cas échéant, et retourne -1.  
  
 Si *desttype* est SRVDECIMAL ou SRVNUMERIC, le paramètre *dest* doit être un pointeur vers une structure DBNUMERIC ou DBDECIMAL avec les champs de précision et d’échelle de la structure déjà définis aux valeurs souhaitées. Vous pouvez utiliser DEFAULTPRECISION pour spécifier une précision par défaut et DEFAULTSCALE pour spécifier une échelle par défaut.  
  
 *destlen*  
 Spécifie la longueur, en octets, de la variable de destination. Ce paramètre est ignoré pour les types de données de longueur fixe. Pour une variable de destination de type SRVCHAR, la valeur de *destlen* doit être la longueur totale de l’espace de mémoire tampon de destination. Une longueur de -1 pour une variable de destination de type SRVCHAR ou SRVBINARY indique que l'espace disponible est suffisant. Pour une variable de destination de type *srvchar*, une longueur de -1 fait en sorte que la chaîne de caractères se termine par NULL.  
  
## <a name="returns"></a>Valeur renvoyée  
 Longueur des données converties, en octets, si la conversion de type de données réussit. Quand **srv_convert** rencontre une demande de conversion qu’il ne prend pas en charge, il appelle le gestionnaire d’erreurs fourni par le développeur, le cas échéant, définit un numéro d’erreur global et retourne -1.  
  
## <a name="remarks"></a>Notes   
 La fonction **srv_willconvert** détermine si une conversion particulière est autorisée.  
  
 La conversion vers le type de données numérique approximatif SRVFLT4 ou SRVFLT8 peut provoquer une certaine perte de précision. La conversion à partir du type de données numérique approximatif SRVFLT4 ou SRVFLT8 vers SRVCHAR ou SRVTEXT peut également provoquer une certaine perte de précision.  
  
 La conversion vers SRVFLT*x*, SRVINT*x*, SRVMONEY, SRVMONEY4, SRVDECIMAL ou SRVNUMERIC peut provoquer un dépassement de capacité si le nombre est supérieur à la valeur maximale de la destination ou un dépassement de précision si le nombre est inférieur à la valeur minimale de la destination. Si le dépassement de capacité se produit lors de la conversion vers SRVCHAR ou SRVTEXT, le premier caractère de la valeur résultante contient un astérisque (*) pour indiquer l'erreur.  
  
 Pendant la conversion de SRVCHAR en SRVBINARY, **srv_convert** interprète SRVCHAR comme hexadécimal, que la chaîne contienne ou non un 0 de début. Pendant la conversion de SRVBINARY en SRVCHAR, **srv_convert** crée une chaîne hexadécimale sans 0 de début. Dans tous les autres cas, une conversion vers ou à partir du type de données SRVBINARY est une copie binaire simple.  
  
 Dans certains cas, il peut être utile de convertir un type de données en lui-même. Par exemple, la conversion de SRVCHAR en SRVCHAR avec un *destlen* de -1 ajoute un terminateur NULL à une chaîne.  
  
 Pour une description des types de données et des conversions de types de données d’API de procédure stockée étendue, consultez [Types de données &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/data-types-extended-stored-procedure-api.md).  
  
 La fonction **srv_convert** peut échouer pour plusieurs raisons :  
  
-   La conversion demandée n'est pas disponible.  
  
-   La conversion a provoqué une troncation, un dépassement de capacité ou une perte de précision dans la variable de destination.  
  
-   Une erreur de syntaxe s'est produite lors de la conversion d'une chaîne de caractères en un type de données numérique.  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](http://go.microsoft.com/fwlink/?LinkID=54761&amp;clcid=0x409http://msdn.microsoft.com/security/).  
  
## <a name="see-also"></a> Voir aussi  
 [srv_setutype &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-setutype-extended-stored-procedure-api.md)   
 [srv_willconvert &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-willconvert-extended-stored-procedure-api.md)  
  
  
