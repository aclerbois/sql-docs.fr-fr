---
title: Refresh, méthode (ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Collection::Refresh
- Parameters::Refresh
- Properties::Refresh
helpviewer_keywords:
- Refresh method [ADO]
ms.assetid: 089b7ca7-684f-4259-8032-5bd1ecc54426
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: af7917817022b83ae5a8da4955ba3c08d05b0819
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="refresh-method-ado"></a>Refresh, méthode (ADO)
Met à jour les objets dans une collection afin de refléter les objets disponibles à partir d’et spécifiques au fournisseur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
collection.Refresh  
```  
  
## <a name="remarks"></a>Notes  
 Le **Actualiser** méthode exécute différentes tâches en fonction de la collection à partir de laquelle vous l’appelez.  
  
### <a name="parameters"></a>Paramètres  
 À l’aide de la **Actualiser** méthode sur le [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) collection d’un [commande](../../../ado/reference/ado-api/command-object-ado.md) objet récupère les informations de paramètres côté fournisseur pour la procédure stockée ou requête paramétrée spécifiée dans le **commande** objet. La collection est vide pour les fournisseurs qui ne prennent pas en charge les appels de procédures stockées ou des requêtes paramétrables.  
  
 Vous devez définir le [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) propriété de la **commande** objet [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet, le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) propriété pour une commande valide et la [CommandType](../../../ado/reference/ado-api/commandtype-property-ado.md) propriété **valeur adCmdStoredProc** avant d’appeler le **Actualiser** (méthode).  
  
 Si vous accédez à la **paramètres** collection avant d’appeler le **Actualiser** méthode, ADO d’appeler la méthode automatiquement et remplir la collection.  
  
> [!NOTE]
>  Si vous utilisez la **Actualiser** méthode pour obtenir des informations sur les paramètres du fournisseur d’et il renvoie au moins un type de données de longueur variable [paramètre](../../../ado/reference/ado-api/parameter-object.md) ADO peut allouer de mémoire pour les paramètres en fonction des objets de leur taille maximale potentielle, ce qui provoque une erreur lors de l’exécution. Vous devez définir explicitement la [taille](../../../ado/reference/ado-api/size-property-ado-parameter.md) propriété de ces paramètres avant d’appeler le [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md) méthode pour éviter les erreurs.  
  
### <a name="fields"></a>Champs  
 À l’aide de la **Actualiser** méthode sur le [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection n’a aucun effet visible. Pour récupérer les modifications de la structure de base de données sous-jacente, vous devez utiliser soit le [Requery](../../../ado/reference/ado-api/requery-method.md) (méthode) ou, si le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet ne prend pas en charge les signets, la [MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)(méthode).  
  
### <a name="properties"></a>Propriétés  
 À l’aide de la **Actualiser** méthode sur un **propriétés** collection de certains objets remplit la collection avec les propriétés dynamiques exposées par le fournisseur. Ces propriétés fournissent des informations sur les fonctionnalités spécifiques au fournisseur, au-delà des propriétés intégrées ADO prend en charge.  
  
## <a name="applies-to"></a>S'applique à  
  
||||  
|-|-|-|  
|[Collection axes](../../../ado/reference/ado-md-api/axes-collection-ado-md.md)|[Collection de colonnes](../../../ado/reference/adox-api/columns-collection-adox.md)|[Collection CubeDefs](../../../ado/reference/ado-md-api/cubedefs-collection-ado-md.md)|  
|[Collection de dimensions](../../../ado/reference/ado-md-api/dimensions-collection-ado-md.md)|[Collection d’erreurs](../../../ado/reference/ado-api/errors-collection-ado.md)|[Collection de champs](../../../ado/reference/ado-api/fields-collection-ado.md)|  
|[Collection de groupes](../../../ado/reference/adox-api/groups-collection-adox.md)|[Collection de hiérarchies](../../../ado/reference/ado-md-api/hierarchies-collection-ado-md.md)|[Collection d’index](../../../ado/reference/adox-api/indexes-collection-adox.md)|  
|[Collection de clés](../../../ado/reference/adox-api/keys-collection-adox.md)|[Collection de niveaux](../../../ado/reference/ado-md-api/levels-collection-ado-md.md)|[Collection de membres](../../../ado/reference/ado-md-api/members-collection-ado-md.md)|  
|[Collection de paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md)|[Collection de positions](../../../ado/reference/ado-md-api/positions-collection-ado-md.md)|[Collection de procédures](../../../ado/reference/adox-api/procedures-collection-adox.md)|  
|[Collection de propriétés](../../../ado/reference/ado-api/properties-collection-ado.md)|[Collection de tables](../../../ado/reference/adox-api/tables-collection-adox.md)|[Collection d’utilisateurs](../../../ado/reference/adox-api/users-collection-adox.md)|  
|[Collection de vues](../../../ado/reference/adox-api/views-collection-adox.md)|||  
  
## <a name="see-also"></a>Voir aussi  
 [Actualiser l’exemple de méthode (VB)](../../../ado/reference/ado-api/refresh-method-example-vb.md)   
 [Actualiser l’exemple de méthode (VC ++)](../../../ado/reference/ado-api/refresh-method-example-vc.md)   
 [Count, propriété (ADO)](../../../ado/reference/ado-api/count-property-ado.md)   
 [Refresh, méthode (RDS)](../../../ado/reference/rds-api/refresh-method-rds.md)
