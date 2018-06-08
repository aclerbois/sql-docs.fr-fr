---
title: Se connecter, propriété (RDS) | Documents Microsoft
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
helpviewer_keywords:
- Connect property [ADO]
ms.assetid: dbad5e77-b213-4eb8-aecf-d60f203fdb59
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b517d0d6a04d74901c51e43bce4b8c45b61280e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-property-rds"></a>Se connecter, propriété (RDS)
Indique le nom de la base de données à partir de laquelle les opérations de requête et de mise à jour sont exécutées.  
  
 Vous pouvez définir le **Connect** propriété au moment du design dans le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) balises d’objet de l’objet, ou au moment de l’exécution de code (par exemple, VBScript) de script.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Design time: <PARAM NAME="Connect" VALUE="ConnectionString">  
Run time: DataControl.Connect = "ConnectionString"  
```  
  
#### <a name="parameters"></a>Paramètres  
 *connectionString*  
 Une chaîne de connexion valide. Pour plus d’informations sur les chaînes de connexion, consultez le [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) propriété ou la documentation de votre fournisseur.  
  
> [!NOTE]
>  Spécification MS Remote comme fournisseur de le **RDS. DataControl** créerait un scénario à quatre niveaux. Scénarios supérieures à trois niveaux n’ont pas été testés et ne doivent pas être nécessaire.  
  
 *DataControl*  
 Une variable objet qui représente un **RDS. DataControl** objet.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Se connecter, propriété-Exemple (VBScript)](../../../ado/reference/rds-api/connect-property-example-vbscript.md)   
 [Méthode de requête (RDS)](../../../ado/reference/rds-api/query-method-rds.md)   
 [Refresh, méthode (RDS)](../../../ado/reference/rds-api/refresh-method-rds.md)   
 [SubmitChanges, méthode (RDS)](../../../ado/reference/rds-api/submitchanges-method-rds.md)


