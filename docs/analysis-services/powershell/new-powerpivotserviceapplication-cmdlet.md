---
title: Applet de commande New-PowerPivotServiceApplication | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5be9914dfb9361af2067cbef469f2ef6737b4fb7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="new-powerpivotserviceapplication-cmdlet"></a>Applet de commande New-PowerPivotServiceApplication
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Crée une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
New-PowerPivotServiceApplication [-ServiceApplicationName] <string> [-DatabaseServerName] <string> [-DatabaseName] <string> [-AddToDefaultProxyGroup <switch>] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande New-PowerPivotServiceApplication permet de créer une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie de serveurs. Vous devez définir au moins une application de service et celle-ci doit être membre du groupe de service de proxy par défaut. Éventuellement, vous pouvez créer des applications de service supplémentaires si vous devez utiliser plusieurs propriétés ou paramètres de configuration différents. Les applications de service supplémentaires doivent être membres des groupes de connexion de service personnalisés. Une seule application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] peut être membre du groupe de proxys par défaut.  
  
 Une nouvelle application de service est créée à l'aide d'une configuration par défaut. Pour personnaliser les propriétés de configuration, utilisez l'applet de commande Set-PowerPivotServiceApplication.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-serviceapplicationname-string"></a>-ServiceApplicationName \<chaîne >  
 Définit le nom d'affichage de l'application de service.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-databaseservername-string"></a>-DatabaseServerName \<chaîne >  
 Spécifie une instance du moteur de base de données relationnelle SQL Server qui héberge la base de données d'application. Par défaut, vous pouvez utiliser le serveur de base de données de la batterie de serveurs, ou vous pouvez choisir un autre serveur de base de données pour lequel vous disposez des droits requis pour créer des bases de données.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-databasename-string"></a>-DatabaseName \<chaîne >  
 Spécifie le nom de la base de données relationnelle SQL Server qui stocke les données d'application. Veillez à spécifier un nom qui correspond à l'application afin que vous puissiez plus facilement identifier sa fonction. Vous pouvez créer une base de données ou spécifier une base de données d’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] existante pour l’application que vous créez.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|2|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-addtodefaultproxygroup-switch"></a>-AddToDefaultProxyGroup \<commutateur >  
 Crée une connexion de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans le groupe de connexions de service par défaut. Les associations entre les applications Web et les applications de service sont déterminées par l'appartenance à ce groupe. Toutes les applications web abonnées au groupe de connexions de service par défaut peuvent utiliser l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] que vous ajoutez au groupe. Bien que vous puissiez disposer de plusieurs applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans une batterie de serveurs, une seule application de service peut être membre du groupe de connexions de service par défaut.  
  
 Si vous disposez déjà d’une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] membre du groupe de proxys par défaut, vous devez définir AddToDefautlProxyGroup:$false pour l’application que vous créez. Vous devez ajouter la nouvelle application de service à un groupe de connexions de service personnalisé.  Vous pouvez utiliser les applets de commande SharePoint intégrées à cet effet.  Get-SPServiceApplicationProxyGroup retourne la liste des groupes de connexions de service définis dans la batterie.  
  
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
C:\PS>New-PowerPivotServiceApplication -ServiceApplicationName "PowerPivot Service Application" -DatabaseServerName "AdvWorks-SRV01\PowerPivot" -DatabaseName "PowerPivotServiceApp1" -AddtoDefaultProxyGroup:$true  
```  
  
 Cet exemple crée une application de service. La base de données d’application de service est créée sur un serveur de base de données nommé AdvWorks-SRV01 qui a été installé en tant qu’instance nommée [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , configuration commune à de nombreuses installations [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Vous devez disposer d'autorisations dbcreator sur l'instance de SQL Server pour créer la base de données. Vous devez être db_owner sur la base de données de configuration SharePoint. Étant donné qu’il s’agit de la première application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de la batterie de serveurs, celle-ci doit être membre du groupe de proxys par défaut.  
  
  
