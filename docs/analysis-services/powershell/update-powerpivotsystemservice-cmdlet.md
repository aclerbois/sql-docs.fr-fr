---
title: Applet de commande Update-PowerPivotSystemService | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3f09c45944b15f474bf9454303924c759ac6f6e9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="update-powerpivotsystemservice-cmdlet"></a>Applet de commande Update-PowerPivotSystemService
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Met à niveau l’objet parent du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Update-PowerPivotSystemService [-Confirm <switch>] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande **Update-PowerPivotSystemService** exécute une série d’actions de mise à niveau sur l’objet parent du service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , les instances et les applications de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie. Tous les services et applications de niveau intermédiaire d’un déploiement [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint doivent s’exécuter au même niveau fonctionnel. Cette applet de commande effectue les actions de mise à niveau sur tous ces objets.  
  
 Exécutez cette applet de commande après avoir exécuté le programme d’installation de SQL Server pour installer une version plus récente de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint ou si vous avez appliqué une mise à jour cumulative sur le serveur. Pour vérifier si une mise à niveau est requise, exécutez `Get-PowerPivotSystemService` pour examiner la propriété **NeedsUpgrade** . Si la propriété **NeedsUpgrade** a la valeur true, vous devez exécuter l’applet de commande pour mettre à niveau les objets [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de niveau intermédiaire dans la batterie.  
  
 Étant donné qu’un déploiement [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint inclut des services de données de niveaux intermédiaires et principaux, vous devez exécuter **Update-PowerPivotEngineService** chaque fois que vous exécutez **Update-PowerPivotSystemService** pour garantir que les deux niveaux utilisent la même version dans toute la batterie.  
  
 La mise à niveau ne peut pas être restaurée à la version précédente. Si vous devez rétablir une version antérieure, supprimez [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de votre batterie SharePoint et réinstallez le logiciel. Pour vous assurer de la réussite de l'opération de mise à niveau, exécutez `Get-PowerPivotSystemService` pour passer en revue les informations de version dans les propriétés globales et pour vérifier que **NeedsUpgrade** n'a plus la valeur true.  
  
## <a name="parameters"></a>Paramètres  
  
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
 Cette applet de commande prend en charge les paramètres suivants :  
  
-   Commentaires  
  
-   Débogage  
  
-   ErrorAction  
  
-   ErrorVariable  
  
-   WarningAction  
  
-   WarningVariable  
  
-   OutBuffer  
  
-   OutVariable  
  
 Pour plus d’informations, consultez [About_CommonParameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|Aucun.|  
|Sorties|Aucun.|  
  
  
