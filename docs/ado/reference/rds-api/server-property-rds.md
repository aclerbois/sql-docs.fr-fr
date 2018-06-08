---
title: Propriété du serveur (RDS) | Documents Microsoft
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
f1_keywords:
- RDS::IBindMgr21::Server
helpviewer_keywords:
- Server property [RDS]
ms.assetid: d2727ce7-da9f-4271-ae3c-9334ef477c14
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6015f19a003148dbe12d6489b23a33848aa0ca29
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-property-rds"></a>Propriété du serveur (RDS)
Indique le protocole de nom et la communication Internet Information Services (IIS).  
  
 Vous pouvez définir le **Server** propriété au moment du design dans les balises d’objet de la[RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) de l’objet ou au moment de l’exécution dans le code de script.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
 **HTTP**  
  
 Syntaxe d’au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="http:  
//awebsrvr:port  
">  
  
```  
  
 Syntaxe de l’exécution  
  
```  
  
DataControl  
.Server="http://  
awebsrvr:port  
"  
  
```  
  
 **HTTPS**  
  
 Syntaxe d’au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="https://awebsrvr:port">  
```  
  
 Syntaxe de l’exécution  
  
```  
  
DataControl.Server="https://awebsrvr:port"  
```  
  
 **DCOM**  
  
 Syntaxe d’au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="  
computername  
">  
  
```  
  
 Syntaxe de l’exécution  
  
```  
  
DataControl.Server="computername"  
```  
  
 **Dans le processus**  
  
 Syntaxe d’au moment du design  
  
```  
  
<PARAM NAME="Server" VALUE="">  
  
```  
  
 Syntaxe de l’exécution  
  
```  
  
DataControl.Server=""  
```  
  
## <a name="parameters"></a>Paramètres  
 *awebsrvr*ou *Nom_Ordinateur*  
 A **chaîne** valeur contenant un Internet ou le chemin d’accès de l’intranet ou le nom de l’ordinateur, si le serveur se trouve sur un ordinateur distant ; ou une chaîne vide si le serveur se trouve sur l’ordinateur local.  
  
 *port*  
 Ce paramètre est facultatif. Un port qui est utilisé pour se connecter à un serveur qui exécute IIS. Le numéro de port est défini dans Internet Explorer (sur le **vue** menu, cliquez sur **Options**, puis sélectionnez le **connexion** onglet) ou dans IIS.  
  
 *DataControl*  
 Une variable objet qui représente un **RDS. DataControl** objet.  
  
## <a name="remarks"></a>Notes  
 Le serveur est l’emplacement où le **RDS. DataControl** demande (autrement dit, une requête ou une mise à jour) est traitée. Par défaut, toutes les demandes sont traitées par le [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet [MSDFMAP. Gestionnaire](../../../ado/guide/remote-data-service/datafactory-customization.md) composant, et [MSDFMAP. INI](../../../ado/guide/remote-data-service/understanding-the-customization-file.md) fichier sur le serveur spécifié. N’oubliez pas que lorsque vous modifiez des serveurs pour comparer les paramètres des anciennes et nouvelles **MSDFMAP. INI** fichiers. Incompatibilités peuvent entraîner des requêtes qui aboutissent sur un serveur sur un autre. Si la propriété de serveur est définie à la chaîne vide « », ces objets seront utilisés sur l’ordinateur local.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété de serveur (VBScript)](../../../ado/reference/rds-api/server-property-example-vbscript.md)   
 [Se connecter, propriété (RDS)](../../../ado/reference/rds-api/connect-property-rds.md)   
 [Propriété SQL](../../../ado/reference/rds-api/sql-property.md)   
 [SubmitChanges, méthode (RDS)](../../../ado/reference/rds-api/submitchanges-method-rds.md)


