---
title: Applet de commande New-PowerPivotSystemServiceInstance | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4fbffee39fc5cb77ac70c1caaa2556361c162fea
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="new-powerpivotsystemserviceinstance-cmdlet"></a>Applet de commande New-PowerPivotSystemServiceInstance
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Ajoute une nouvelle instance de service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] à un serveur d’applications.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
New-PowerPivotSystemServiceInstance [[-ParentService] <PowerPivotMidTierServicePipeBind>] [-SystemServiceInstanceName <string>] [-Provision] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande New-PowerPivotSystemServiceInstance configure un nouvel objet PowerPivotSystemService au niveau de la batterie une fois que vous avez utilisé le programme d’installation de SQL Server pour installer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint sur le serveur d’applications local. Vous ne pouvez configurer qu'une seule instance du service sur chaque serveur d'applications.  Si le service est déjà configuré, vous ne pouvez pas exécuter cette applet de commande.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-parentservice-powerpivotmidtierservicepipebind"></a>-ParentService \<PowerPivotMidTierServicePipeBind >  
 Spécifie le GUID de l’objet parent du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie. Dans cette version, un seul objet parent est autorisé. Vous pouvez utiliser l'applet de commande Get-PowerPivotSystemService pour retourner l'objet de service ou son GUID.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-systemserviceinstancename-string"></a>-SystemServiceInstanceName \<string>  
 Spécifie un nom qui identifie cet objet.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="provision-switchparameter"></a>Configurer [\<Paramètre_booléen >]  
 Rend le service accessible sur SharePoint. Les valeurs valides sont $true ou $false.  
  
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
C:\PS>New-PowerPivotSystemServiceInstance -Provision:$true  
```  
  
 Cet exemple montre la forme la plus commune de l'applet de commande. Il inscrit le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur le serveur d’applications local avec la batterie.  
  
## <a name="example-2"></a>Exemple 2  
  
```  
C:\PS>New-PowerPivotSystemServiceInstance -SystemServiceInstanceName "MyPSSInstance" -provision:$false  
```  
  
 Cet exemple nomme l’instance du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , mais sans la configurer. Si vous ne fournissez pas de nom, le nom par défaut, instance de service système SQL Server Analysis Services, est utilisé à la place. La création d'un nom personnalisé pour le service est facultative. Vous pouvez nommer le service pour prendre en charge les scénarios de test, ou si vous disposez d'un outil ou d'un script personnalisé qui configure l'instance à une étape ultérieure.  
  
  
