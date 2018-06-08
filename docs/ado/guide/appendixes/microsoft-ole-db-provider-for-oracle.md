---
title: Fournisseur Microsoft OLE DB pour Oracle | Documents Microsoft
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
- providers [ADO], OLE DB provider for Oracle
- OLE DB provider for Oracle [ADO]
- Oracle provider [ADO]
ms.assetid: 44fae9dd-5585-4cd6-8bbd-3248a78931b4
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: de312ff17a7d66bf58a5b8f1fb7a6c33aa27acec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-provider-for-oracle-overview"></a>Fournisseur Microsoft OLE DB pour Oracle présentation
> [!IMPORTANT]
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez plutôt le fournisseur OLE DB d’Oracle.

 Le fournisseur Microsoft OLE DB pour Oracle permet à ADO accéder aux bases de données Oracle.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez la *fournisseur* argument de la [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) propriété :

```
MSDAORA
```

 La lecture de la [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) propriété retournera également cette chaîne.

 Si une requête de jointure avec un curseur keyset ou dynamic est exécutée dans une base de données Oracle, une erreur se produit. Oracle prend uniquement en charge un curseur statique en lecture seule.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion par défaut pour ce fournisseur est :

```
"Provider=MSDAORA;Data Source=serverName;User ID=MyUserID; Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé| Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur OLE DB pour Oracle.|
|**Source de données**|Spécifie le nom d’un serveur.|
|**ID d'utilisateur**|Spécifie le nom d’utilisateur.|
|**Mot de passe**|Spécifie le mot de passe.|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = yes** ou **Integrated Security = SSPI** au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.

## <a name="provider-specific-connection-parameters"></a>Paramètres de connexion spécifique au fournisseur
 Le fournisseur prend en charge plusieurs paramètres de connexion spécifique au fournisseur en plus de ceux définis par ADO. Comme avec les propriétés de connexion ADO, ces propriétés spécifiques au fournisseur peuvent être définies la [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection d’un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) ou dans le cadre de la **ConnectionString**.

 Ces paramètres sont entièrement décrits dans le [de référence du programmeur OLE DB](http://msdn.microsoft.com/en-us/3c5e2dd5-35e5-4a93-ac3a-3818bb43bbf8). Le [Index des propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-property-index.md) fournit une référence croisée entre ces noms de paramètres et les propriétés OLE DB correspondantes.

|Paramètre| Description|
|---------------|-----------------|
|**Handle de fenêtre**|Indique le handle de fenêtre à utiliser pour demander des informations supplémentaires.|
|**Identificateur de paramètres régionaux**|Indique un nombre 32 bits unique (par exemple, 1033) qui spécifie les préférences liées à la langue de l’utilisateur. Ces préférences indiquent la façon dont les dates et heures sont formatées, les éléments sont triés par ordre alphabétique, les chaînes sont comparées et ainsi de suite.|
|**Services OLE DB**|Indique le masque de bits qui spécifie les services OLE DB pour activer ou désactiver.|
|**invite de commandes**|Indique s’il faut inviter l’utilisateur pendant une connexion est établie.|
|**Propriétés étendues**|Chaîne contenant les informations de connexion étendues spécifiques au fournisseur. Utilisez cette propriété uniquement pour les informations de connexion spécifique au fournisseur qui ne peut pas être décrites par le biais du mécanisme de propriété.|

## <a name="see-also"></a>Voir aussi
 [ConnectionString, propriété (ADO)](../../../ado/reference/ado-api/connectionstring-property-ado.md) [fournisseur, propriété (ADO)](../../../ado/reference/ado-api/provider-property-ado.md) [l’objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
