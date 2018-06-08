---
title: Applet de commande Remove-PowerPivotSystemServiceInstance | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c4f6feb69b3375b5ed5d0d69407ef5a033991f96
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="remove-powerpivotsystemserviceinstance-cmdlet"></a>Applet de commande Remove-PowerPivotSystemServiceInstance
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Supprime une instance du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie de serveurs.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Remove-PowerPivotSystemServiceInstance [-Confirm <switch>] [-DeleteLocal <switch>] [-Identity <PowerPivotMidTierServiceInstancePipeBind>] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande Remove-PowerPivotSystemServiceInstance supprime les informations d’instance relatives au service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie de serveurs. Elle ne supprime pas les fichiers programme. Pour supprimer définitivement les fichiers programme, vous devez les désinstaller.  
  
 Si vous supprimez le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , veillez à exécuter également Remove-PowerPivotEngineServiceInstance pour supprimer l’instance Analysis Services associée, puis Remove-PowerPivotServiceApplication pour supprimer toutes les applications de service PowerPivot. Les applications de service ne s'exécuteront plus une fois les services supprimés.  
  
 Pour annuler cette modification, vous pouvez exécuter New-PowerPivotSystemServiceInstance pour réactiver les informations d'instance.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-identity-powerpivotmidtierserviceinstancepipebind"></a>-Identité \<PowerPivotMidTierServiceInstancePipeBind >  
 Spécifie le GUID de l’instance du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] que vous souhaitez supprimer. Il existe une instance de service sur chaque serveur d’applications comportant une installation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-deletelocal-switch"></a>-DeleteLocal \<commutateur >  
 Supprime l’instance du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] installée sur l’ordinateur local, ce qui vous permet de supprimer l’instance sans avoir à spécifier d’identité d’objet.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
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
C:\PS>Remove-PowerPivotSystemServiceInstance -deletelocal  
```  
  
 Cet exemple indique comment supprimer l’instance du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui s’exécute sur le serveur d’applications local.  
  
## <a name="example-2"></a>Exemple 2  
  
```  
C:\PS>Remove-PowerPivotSystemServiceInstance -identity 1234567-890a-bcde-fghijklmn  
```  
  
 Cet exemple indique comment supprimer un service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] spécifique en fonction de son identité.  
  
  
