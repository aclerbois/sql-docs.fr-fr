---
title: Applet de commande Get-PowerPivotSystemServiceInstance | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cb07d3ee84f8a43e15732f3aafc1a4c7cef83fbf
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="get-powerpivotsystemserviceinstance-cmdlet"></a>Applet de commande Get-PowerPivotSystemServiceInstance
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Retourne une ou plusieurs instances du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] s’exécutant sur des serveurs d’applications de la batterie.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Get-PowerPivotSystemServiceInstance [-Identity <PowerPivotMidTierServiceInstancePipeBind>] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande Get-PowerPivotSystemServiceInstance retourne les propriétés d’une ou plusieurs instances du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] s’exécutant dans la batterie. L'applet de commande indique le type de l'application, son état (en ligne ou hors connexion), et son identité. Pour afficher les propriétés supplémentaires d'une instance spécifique, ajoutez le paramètre Identity et l'option format-list à l'applet de commande.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-identity-powerpivotmidtierserviceinstancepipebind"></a>-Identité \<PowerPivotMidTierServiceInstancePipeBind >  
 Spécifie l'instance de service à obtenir. La valeur doit être un GUID valide qui identifie l'objet de manière unique dans la batterie.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 Cette applet de commande prend en charge les paramètres communs : Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer et OutVariable. Pour plus d’informations, consultez [About_CommonParameters](http://go.microsoft.com/fwlink/?linkID=227825)  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|Aucun.|  
|Sorties|Aucun.|  
  
## <a name="example-1"></a>Exemple 1  
  
```  
C:\PS>Get-PowerPivotSystemServiceInstance -Identity 1234567-890a-bcde-fghijklm | format-list| format-list  
```  
  
 Cet exemple retourne les propriétés supplémentaires de l'instance spécifiée, notamment le nom du serveur, la version et l'état de la mise à niveau.  
  
  
