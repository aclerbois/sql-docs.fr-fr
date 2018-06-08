---
title: Commande de RETRAITER SET | Documents Microsoft
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
- SET REPROCESS command [ODBC]
ms.assetid: b0708757-b1d7-42f3-8988-787f2a806b8b
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f7dbfee063d403605fe2a72efada88ecf0d84e34
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-reprocess-command"></a>Commande de RETRAITER SET
Spécifie le long combien de fois ou sur la façon de verrouiller un fichier ou un enregistrement après une tentative infructueuse de verrouillage.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET REPROCESS TO nAttempts [SECONDS] | TO AUTOMATIC  
```  
  
## <a name="arguments"></a>Arguments  
 POUR *nAttempts*[SECONDS]  
 Spécifie le nombre de fois ou le nombre de secondes pour essayer de verrouiller un fichier ou un enregistrement après une initiale échoue. La valeur par défaut est 0 ; la valeur maximale est de 32 000.  
  
 SECONDES Spécifie que Visual FoxPro tente de verrouiller un fichier ou l’enregistrement de *nAttempts* secondes. Il est disponible uniquement lorsque *nAttempts* est supérieure à zéro.  
  
 Par exemple, si *nAttempts* est 30, Visual FoxPro tente de verrouiller un enregistrement ou de fichiers jusqu'à 30 fois. Si vous incluez également secondes (valeur RETRAITER à 30 secondes), Visual FoxPro tente en permanence un enregistrement ou un fichier de verrouillage pour jusqu'à 30 secondes.  
  
 Si une routine ON ERROR est activée et si les tentatives par une commande de verrouillage de l’enregistrement ou le fichier échouent, la routine ON ERROR est exécutée. Toutefois, si une fonction tente du verrou, une routine ON ERROR n’est pas exécutée et la fonction retourne False (. F.).  
  
 Si une routine ON ERROR n’est pas en vigueur, une commande tente de verrouiller l’enregistrement ou le fichier et que le verrou ne peut pas être placé, une erreur est générée. Si une fonction tente de placer le verrou, l’alerte ne s’affiche pas et la fonction retourne False (. F.).  
  
 Si *nAttempts* est 0 (valeur par défaut) et vous émettez une commande ou une fonction qui tente de verrouiller un enregistrement ou un fichier, Visual FoxPro essaie de verrouiller l’enregistrement ou de fichiers indéfiniment. Si l’enregistrement ou le fichier devient disponible pour le verrouillage pendant que vous patientez, le verrou est placé et le message système est désactivé. Si une fonction a tenté de placer le verrou, la fonction retourne la valeur True (. T.).  
  
 Si une routine ON ERROR est activée et une commande essaie de verrouiller l’enregistrement ou le fichier, la routine ON ERROR est prioritaire sur toute tentative supplémentaire pour l’enregistrement ou le fichier de verrouillage. La routine ON ERROR est exécutée immédiatement. Visual FoxPro n’essaie pas d’un enregistrement supplémentaire ou verrous de fichier et n’affiche pas le message système.  
  
 Si *nAttempts* est 1, Visual FoxPro tente de verrouiller l’enregistrement ou de fichiers indéfiniment et une routine ON ERROR n’est pas exécutée.  
  
 Si un verrou a été placé par un autre utilisateur sur l’enregistrement ou de la tentative de verrouillage de fichier, vous devez attendre jusqu'à ce que l’utilisateur libère le verrou.  
  
 EN MODE AUTOMATIQUE  
 Spécifie que Visual FoxPro tente de verrouiller l’enregistrement ou de fichiers indéfiniment. (Retraitement SET-2 est une commande équivalente).  
  
## <a name="remarks"></a>Notes  
 La première tentative de verrouillage d’un enregistrement ou un fichier n’est pas toujours réussie. Souvent, un enregistrement ou un fichier est verrouillé par un autre utilisateur sur le réseau. DÉFINIR le RETRAITER détermine si Visual FoxPro effectue des tentatives supplémentaires pour l’enregistrement ou le fichier de verrouillage lors de la tentative initiale a échoué. Vous pouvez spécifier la fréquence des tentatives supplémentaires sont effectuées ou pour la durée pendant laquelle les tentatives sont effectuées. Une routine ON ERROR affecte verrou comment échoue tentatives sont gérées.
