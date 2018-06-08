---
title: Modifier le nom de propriété dynamique (ADO) | Documents Microsoft
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
- Reshape Name property [ADO]
ms.assetid: 690229d1-46cc-42e6-a57d-4438251fe248
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4901e09d0df933c6aa35e8c1ebed3e62ef2012f8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reshape-name-property-dynamic-ado"></a>Modifier le nom de propriété dynamique (ADO)
Spécifie un nom pour le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un **chaîne** valeur qui est le nom de la **Recordset**.  
  
## <a name="remarks"></a>Notes  
 Les noms persistent pendant la durée de la connexion ou jusqu'à ce que le **Recordset** est fermé.  
  
 Le **nom de remodeler** propriété est principalement utilisée pour une utilisation avec la fonctionnalité de remise en forme de la [Service de mise en forme des données Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) fournisseur de services. Les noms doivent être uniques pour participer à la remise en forme.  
  
 Cette propriété est en lecture seule, mais peut être définie lorsque indirectement un **Recordset** est créé. Par exemple, si une clause d’une commande Shape crée un **Recordset** et lui donne un nom d’alias à l’aide de la **AS** (mot clé), l’alias est affecté à la **remodeler les nom** propriété. Si aucun alias n’est déclaré, le **nom de remodeler** est affectée à un nom unique généré par le service de mise en forme des données. Si le nom d’alias est le même que le nom d’un objet **Recordset**, ni **Recordset** peut être redessinée jusqu'à ce qu’un d’eux est libéré. Le comportement par défaut peut être modifié en définissant un nom unique dans le [nom de remodeler](../../../ado/reference/ado-api/reshape-name-property-dynamic-ado.md) propriété sur la connexion ADO **True**. Définition de cette propriété fournit les données d’autorisation de service pour modifier le nom d’utilisateur attribué, si nécessaire, pour garantir l’unicité de la mise en forme. Pour plus d’informations sur la mise en forme, consultez [Service de mise en forme des données Microsoft pour OLE DB (fournisseur de services ADO)](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md).  
  
 Utilisez le **remodeler le nom** propriété lorsque vous souhaitez faire référence à un **Recordset** dans une commande de la forme, ou lorsque vous ne connaissez pas le nom, car il a été généré par le Service de mise en forme des données. Dans ce cas, générez une commande SHAPE en concaténant la commande autour de la chaîne retournée par la **nom de remodeler** propriété.  
  
 **Remodeler nom** est une propriété dynamique ajoutée à la **Recordset** l’objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection lorsque le [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) est définie sur **adUseClient**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Données de Microsoft Service pour OLE DB (fournisseur de services ADO) de mise en forme](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)   
 [En général, les commandes de forme](../../../ado/guide/data/shape-commands-in-general.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
