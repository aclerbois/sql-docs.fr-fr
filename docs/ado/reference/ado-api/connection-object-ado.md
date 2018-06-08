---
title: Objet de connexion (ADO) | Documents Microsoft
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
- Connection
helpviewer_keywords:
- Connection object [ADO]
ms.assetid: ef6b1824-5b12-43db-89d7-8f3d13896d4d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ba405d7993c5f9c327d5789cfa3bf3c9ccfba2b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-object-ado"></a>Objet de connexion (ADO)
Représente une connexion ouverte à une source de données.  
  
## <a name="remarks"></a>Notes  
 A **connexion** objet représente une session unique avec une source de données. Dans un système de base de données client/serveur, il peut être équivalente à une connexion réseau réelle au serveur. Selon les fonctionnalités prises en charge par le fournisseur, certaines collections, des méthodes ou propriétés d’un **connexion** objet n’est peut-être pas disponible.  
  
 Avec les collections, les méthodes et les propriétés d’un **connexion** de l’objet, vous pouvez procédez comme suit :  
  
-   Configurer la connexion avant de l’ouvrir avec le [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md), [ConnectionTimeout](../../../ado/reference/ado-api/connectiontimeout-property-ado.md), et [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriétés. **ConnectionString** est la propriété par défaut de la **connexion** objet.  
  
-   Définir le [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété au client pour appeler le [le Service de curseur Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md), qui prend en charge les mises à jour du lot.  
  
-   Définir la base de données par défaut pour la connexion avec le [DefaultDatabase](../../../ado/reference/ado-api/defaultdatabase-property.md) propriété.  
  
-   Ensemble, le niveau d’isolation des transactions ouvertes sur la connexion avec le [IsolationLevel](../../../ado/reference/ado-api/isolationlevel-property.md) propriété.  
  
-   Spécifiez un fournisseur OLE DB avec la [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) propriété.  
  
-   Établir et ultérieurement rompre la connexion physique à la source de données avec le [ouvrir](../../../ado/reference/ado-api/open-method-ado-connection.md) et [fermer](../../../ado/reference/ado-api/close-method-ado.md) méthodes.  
  
-   Exécuter une commande sur la connexion avec le [Execute](../../../ado/reference/ado-api/execute-method-ado-connection.md) (méthode) et configurer l’exécution avec le [CommandTimeout](../../../ado/reference/ado-api/commandtimeout-property-ado.md) propriété.  
  
    > [!NOTE]
    >  Pour exécuter une requête sans l’aide d’un objet de commande, passez une chaîne de requête à la **Execute** méthode d’un **connexion** objet. Toutefois, un [commande](../../../ado/reference/ado-api/command-object-ado.md) objet est requis lorsque vous souhaitez conserver le texte de commande et exécuter de nouveau ou utiliser les paramètres de requête.  
  
-   Gérer des transactions sur une connexion ouverte, notamment des transactions imbriquées si le fournisseur prend en charge, avec le [BeginTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md), [CommitTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md), et [RollbackTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) méthodes et les [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) propriété.  
  
-   Examinez les erreurs retournées par la source de données avec le [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection.  
  
-   Lire la version de l’implémentation ADO utilisée avec la [Version](../../../ado/reference/ado-api/version-property-ado.md) propriété.  
  
-   Obtenir des informations de schéma sur votre base de données avec le [OpenSchema](../../../ado/reference/ado-api/openschema-method.md) (méthode).  
  
 Vous pouvez créer **connexion** objets indépendamment de tout autre objet défini précédemment.  
  
 Vous pouvez exécuter des commandes nommées ou des procédures stockées comme si elles étaient des méthodes natives sur une **connexion** de l’objet, comme indiqué dans la section suivante. Lorsqu’une commande nommée possède le même nom que celui d’une procédure stockée, méthode d’appel la « natif » sur un **connexion** objet toujours exécuter la commande nommée au lieu de la procédure stockée.  
  
> [!NOTE]
>  N’utilisez pas cette fonctionnalité (appelant une procédure stockée ou une commande nommée comme s’il s’agissait d’une méthode native sur le **connexion** objet) dans une application Microsoft® .NET Framework, car l’implémentation sous-jacente de conflits de fonctionnalité avec la façon dont le .NET Framework interagit avec COM.  
  
## <a name="execute-a-command-as-a-native-method-of-a-connection-object"></a>Exécuter une commande comme une méthode native d’un objet de connexion  
 Pour exécuter une commande, donnez-lui un nom à l’aide de la **commande** objet [nom](../../../ado/reference/ado-api/name-property-ado.md) propriété. Définir le **ActiveConnection** propriété de la **commande** objet à la connexion. Ensuite vous émettez une instruction où le nom de commande est utilisé comme s’il s’agissait d’une méthode sur le **connexion** objet, suivie de tous les paramètres et un **Recordset** de l’objet si toutes les lignes sont retournées. Définir le **Recordset** propriétés pour personnaliser résultant **Recordset**. Par exemple :  
  
```  
Dim cnn As New ADODB.Connection  
Dim cmd As New ADODB.Command  
Dim rst As New ADODB.Recordset  
...  
cnn.Open "..."  
cmd.Name = "yourCommandName"  
cmd.ActiveConnection = cnn  
...  
'Your command name, any parameters, and an optional Recordset.  
cnn. "parameter", rst  
```  
  
## <a name="execute-a-stored-procedure-as-a-native-method-of-a-connection-object"></a>Exécuter une procédure stockée comme une méthode native d’un objet de connexion  
 Pour exécuter une procédure stockée, émettez une instruction où le nom de la procédure stockée est utilisé comme s’il s’agissait d’une méthode sur le **connexion** objet, suivie de tous les paramètres. ADO effectuera une « estimation » des types de paramètres. Par exemple :  
  
```  
Dim cnn As New ADODB.Connection  
...  
'Your stored procedure name and any parameters.  
cnn. "parameter"  
```  
  
 Le **connexion** objet est sécurisé pour le script.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés d’objet de connexion, méthodes et événements](../../../ado/reference/ado-api/connection-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet de commande (ADO)](../../../ado/reference/ado-api/command-object-ado.md)   
 [Collection d’erreurs (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)   
 [Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Annexe A : Fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md)
