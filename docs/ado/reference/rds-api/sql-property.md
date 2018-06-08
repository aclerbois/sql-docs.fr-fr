---
title: Propriété SQL | Documents Microsoft
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: sql
ms.prod_service: drivers
ms.component: reference
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- SQL property [RDS]
ms.assetid: e0dabf23-a159-4fe5-a962-3df544a21f5c
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a5ddbfa6b69f4859f61130e8ec1c9b758c40cf64
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-property"></a>Propriété SQL
Indique la chaîne de requête utilisée pour récupérer le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
 Vous pouvez définir le **SQL** propriété au moment du design dans le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) balises d’objet de l’objet, ou en cours d’exécution dans le code de script.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Design time: <PARAM NAME="SQL" VALUE="QueryString">  
Run time: DataControl.SQL = "QueryString"  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Chaîne de requête*  
 A **chaîne** valeur qui contient une demande de données SQL valide.  
  
 *DataControl*  
 Une variable objet qui représente un **RDS. DataControl** objet.  
  
## <a name="remarks"></a>Notes  
 En règle générale, il s’agit d’une instruction de SQL (à l’aide de dialecte du serveur de base de données), tel que `"Select * from NewTitles"`. Pour vous assurer que les enregistrements sont mis en correspondance et mise à jour, une requête actualisable doit contenir un champ autre qu’un champ Long Binary ou un champ calculé.  
  
 Le **SQL** propriété est facultative si un objet métier côté serveur extrait les données pour le client.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété SQL (VBScript)](../../../ado/reference/rds-api/sql-property-example-vbscript.md)   
 [Se connecter, propriété (RDS)](../../../ado/reference/rds-api/connect-property-rds.md)   
 [Méthode de requête (RDS)](../../../ado/reference/rds-api/query-method-rds.md)   
 [Refresh, méthode (RDS)](../../../ado/reference/rds-api/refresh-method-rds.md)   
 [SubmitChanges, méthode (RDS)](../../../ado/reference/rds-api/submitchanges-method-rds.md)


