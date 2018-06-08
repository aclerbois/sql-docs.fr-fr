---
title: Fournisseur d’accès à distance Microsoft OLE DB (fournisseur de services ADO) | Documents Microsoft
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
helpviewer_keywords:
- OLE DB remoting provider [ADO]
- providers [ADO], OLE DB remoting provider
- remoting provider [ADO]
ms.assetid: a4360ed4-b70f-4734-9041-4025d033346b
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b69c6f64de019aadf71476958c26f99a46dabac2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-remoting-provider-overview"></a>Vue d’ensemble du fournisseur Microsoft OLE DB la communication à distance
Le fournisseur Microsoft OLE DB d’accès à distance permet à un utilisateur local sur un ordinateur client appeler des fournisseurs de données sur un ordinateur distant. Spécifiez les paramètres du fournisseur de données pour l’ordinateur distant comme vous le feriez si vous étiez un utilisateur local sur l’ordinateur distant. Ensuite, spécifiez les paramètres utilisés par le fournisseur de la communication à distance pour accéder à l’ordinateur distant. Vous pouvez ensuite accéder l’ordinateur distant comme si vous étiez un utilisateur local.

> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).

## <a name="provider-keyword"></a>Mot clé de fournisseur
 Pour appeler le fournisseur OLE DB la communication à distance, spécifiez le mot clé et la valeur suivante dans la chaîne de connexion. (Notez l’espace vide dans le nom du fournisseur).

```
"Provider=MS Remote"
```

## <a name="additional-keywords"></a>Mots clés supplémentaires
 Lorsque ce fournisseur de services est appelé, les mots clés supplémentaires suivants sont pertinents.

|Mot clé| Description|
|-------------|-----------------|
|**Source de données**|Spécifie le nom de la source de données distante. Il est passé pour le fournisseur OLE DB la communication à distance pour le traitement.<br /><br /> Ce mot clé est équivalent à la [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) l’objet [Connect](../../../ado/reference/rds-api/connect-property-rds.md) propriété.|

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Lorsque ce fournisseur de services est appelé, les propriétés dynamiques suivantes sont ajoutées à la [connexion](../../../ado/reference/ado-api/connection-object-ado.md)l’objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection.

|Nom de la propriété dynamique| Description|
|---------------------------|-----------------|
|**DFMode**|Indique le Mode DataFactory. Chaîne qui spécifie la version souhaitée de la [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet sur le serveur. Définissez cette propriété avant d’ouvrir une connexion pour demander une version particulière de la **DataFactory**. Si la version demandée n’est pas disponible, une tentative est effectuée pour utiliser la version précédente. S’il n’existe aucune version précédente, une erreur se produit. Si **DFMode** est inférieure à la version disponible, une erreur se produit. Cette propriété est en lecture seule après qu’une connexion est établie.<br /><br /> Peut être une des valeurs de chaîne valides suivantes :<br /><br /> -« 25 », version 2.5 (valeur par défaut)<br />-« 21 », version 2.1<br />-« 20 », version 2.0<br />-« 15 », version 1.5|
|**Propriétés de la commande**|Indique les valeurs qui seront ajoutés à la chaîne de propriétés de commande (ensemble de lignes) envoyées au serveur par le fournisseur MS Remote. La valeur par défaut pour cette chaîne est vt_empty.|
|**DFMode actuel**|Indique le numéro de version réelle de la **DataFactory** sur le serveur. Consultez cette propriété pour vérifier si la version demandée dans le **DFMode** propriété a été respectée.<br /><br /> Peut prendre l’une des valeurs d’entier Long valides suivantes :<br /><br /> -25 : version 2.5 (valeur par défaut)<br />-21 : version 2.1<br />-20-version 2.0<br />-15 : version 1.5<br /><br /> Ajout de « DFMode = 20 ; » à votre chaîne de connexion lorsque vous utilisez la **MSRemote, vous** fournisseur peut améliorer les performances de votre serveur lors de la mise à jour des données. Avec ce paramètre, le **RDSServer.DataFactory** objet sur le serveur utilise un mode moins gourmandes en ressources. Toutefois, les fonctionnalités suivantes ne sont pas disponibles dans cette configuration :<br /><br /> -Utiliser des requêtes paramétrables.<br />-L’obtention des informations de paramètre ou la colonne avant d’appeler le **Execute** (méthode).<br />-La définition de **Transact mises à jour** à **True**.<br />-Obtenir l’état de la ligne.<br />-Appelant le **Resync** (méthode).<br />-Actualisation (explicite ou automatique) via la **Update Resync** propriété.<br />-La définition de **commande** ou **Recordset** propriétés.<br />-À l’aide de **adCmdTableDirect**.|
|**Gestionnaire**|Indique le nom d’un programme de personnalisation côté serveur (gestionnaire) qui étend les fonctionnalités de la [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)et tous les paramètres utilisés par le Gestionnaire de *,* séparés par des virgules ( ","). A **chaîne** valeur.|
|**Délai d’attente Internet**|Indique le nombre maximal de millisecondes à attendre une demande à destination et à partir du serveur. (La valeur par défaut est 5 minutes.)|
|**Fournisseur distant**|Indique le nom du fournisseur de données à utiliser sur le serveur distant.|
|**Serveur distant**|Indique le protocole de communication et le nom de serveur à être utilisée par cette connexion. Cette propriété est équivalente à la [RDS. DataContro](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet [Server](../../../ado/reference/rds-api/server-property-rds.md) propriété.|
|**Transact mises à jour**|Lorsque la valeur **True**, cette valeur indique que lorsque [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) est effectuée sur le serveur, elle sera effectuée à l’intérieur d’une transaction. La valeur par défaut de cette propriété dynamique booléenne **False**.|

 Vous pouvez également définir des propriétés dynamiques en écriture en spécifiant leurs noms en tant que mots clés dans la chaîne de connexion. Par exemple, définissez la **Internet Timeout** propriété dynamique à cinq secondes en spécifiant :

```
Dim cn as New ADODB.Connection
cn.Open "Provider=MS Remote;Internet Timeout=5000"
```

 Vous pouvez également définir ou extraire une propriété dynamique en spécifiant son nom comme index de la **propriétés** propriété. L’exemple suivant montre comment obtenir et imprimer la valeur actuelle de la **Internet Timeout** propriété dynamique et définissez une nouvelle valeur :

```
Debug.Print cn.Properties("Internet Timeout")
cn.Properties("Internet Timeout") = 5000
```

## <a name="remarks"></a>Notes
 Dans ADO 2.0, le fournisseur OLE DB la communication à distance peut uniquement être spécifié dans le *ActiveConnection* paramètre de la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet **ouvrir** (méthode). À partir de ADO 2.1, le fournisseur peut également être spécifié dans le *ConnectionString* paramètre de la [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet **ouvrir** (méthode).

 L’équivalent de la **RDS. DataControl** objet [SQL](../../../ado/reference/rds-api/sql-property.md) propriété n’est pas disponible. Le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet **ouvrir** méthode *Source* argument est utilisé à la place.

 **Remarque** en spécifiant «... » ; Fournisseur distant = MS Remote ;... » créerait un scénario à quatre niveaux. Scénarios, au-delà de trois niveaux n’ont pas été testés et ne doivent pas être nécessaire.

## <a name="example"></a>Exemple
 Cet exemple exécute une requête sur le **auteurs** table de la **Pubs** base de données sur un serveur nommé *YourServer*. Les noms de la source de données distante et le serveur distant sont fournis dans le [ouvrir](../../../ado/reference/ado-api/open-method-ado-connection.md) méthode de la[connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet et la requête SQL est spécifiée dans le[ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode de la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet. A **Recordset** objet est retourné, modifié et utilisé pour mettre à jour la source de données.

```
Dim rs as New ADODB.Recordset
Dim cn as New ADODB.Connection
cn.Open  "Provider=MS Remote;Data Source=pubs;" & _
         "Remote Server=http://YourServer"
rs.Open "SELECT * FROM authors", cn
...                'Edit the recordset
rs.UpdateBatch     'Equivalent of RDS SubmitChanges
...
```

## <a name="see-also"></a>Voir aussi
 [Vue d’ensemble du fournisseur OLE DB la communication à distance](http://msdn.microsoft.com/en-us/4083b72f-68c4-4252-b366-abb70db5ca2b)
