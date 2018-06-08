---
title: Applet de commande Remove-PowerPivotServiceApplication | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 30b7826bf0239ed59b0b94ce624ab1eb212a34fc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="remove-powerpivotserviceapplication-cmdlet"></a>Applet de commande Remove-PowerPivotServiceApplication
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Supprime une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Remove-PowerPivotServiceApplication [-Identity <SPGeminiServiceApplicationPipeBind>] [-DeleteAll <switch>] [-RemoveData <switch>] [-Confirm <switch>] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande Remove-PowerPivotServiceApplication supprime une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie. Utilisez DeleteAll pour supprimer toutes les applications de service simultanément, ou utilisez le paramètre Identity pour supprimer une seule instance. Pour obtenir les informations d'instance, exécutez Get-PowerPivotServiceApplication pour retourner toutes les instances de la batterie.  
  
 Utilisez le paramètre RemoveData pour supprimer au besoin les bases de données d'application de service et les fichiers mis en cache. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont conservés dans les bibliothèques de contenu, mais ne sont plus fonctionnels une fois que l’application de service a été supprimée.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-identity-spgeminiserviceapplicationpipebind"></a>-Identité \<SPGeminiServiceApplicationPipeBind >  
 Spécifie le GUID d’une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] unique de la batterie. Vous devez spécifier le GUID si vous souhaitez supprimer une seule application sans toucher aux autres.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-confirm-switch"></a>-Confirmer \<commutateur >  
 Demande une confirmation avant d'exécuter la commande. Cette valeur est activée par défaut. Pour contourner la réponse de confirmation dans une commande, spécifiez Confirm:$false sur la commande.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-deleteall-switch"></a>-DeleteAll \<commutateur >  
 Supprime toutes les applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , mais ne supprime pas la base de données d’application de service ni les objets d’instance du service de la batterie. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et du service de moteur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] restent instanciés, mais sont inutilisables après la suppression des applications de service.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-removedata-switch"></a>-RemoveData \<commutateur >  
 Supprime la base de données d'application de service qui contient les planifications d'actualisation des données, les données d'utilisation des classeurs, les mappages d'instance utilisés pour suivre les bases de données chargées et d'autres données internes.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 Cette applet de commande prend en charge les paramètres communs : Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer et OutVariable. Pour plus d’informations, consultez [About_Commonparameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|Aucun.|  
|Sorties|Aucun.|  
  
## <a name="example-1"></a>Exemple 1  
  
```  
C:\PS>Remove-PowerPivotServiceApplication -identity 12345678-90ab-cdef-ghijklmnop  
```  
  
 Cet exemple supprime une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , mais ne supprime pas sa base de données ni son cache. Si vous ne spécifiez pas d'identité, vous serez invité à en fournir une.  
  
## <a name="example-2"></a>Exemple 2  
  
```  
C:\PS>Remove-PowerPivotServiceApplication -DeleteAll  
```  
  
 Cet exemple supprime toutes les applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie. Les bases de données et le cache ne sont pas supprimés.  
  
## <a name="example-3"></a>Exemple 3  
  
```  
CC:\PS>Remove-PowerPivotServiceApplication -identity 12345678-90ab-cdef-ghijklmnop -RemoveData  
```  
  
 Cet exemple supprime une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , ainsi que sa base de données et ses fichiers cache.  
  
  
