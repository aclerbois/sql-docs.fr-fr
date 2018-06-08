---
title: Section de données | Documents Microsoft
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
- data section [ADO]
ms.assetid: 43dc42a8-7057-48e6-93d6-880d5c5c51a4
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cc7a8e66765d35d4c8a8a7f74f63720dec4d9429
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-section"></a>Section de données
La section de données définit les données de l’ensemble de lignes, ainsi que tout en attente de mises à jour, les insertions ou les suppressions. La section de données peut contenir zéro ou plusieurs lignes. Il ne peut contenir que des données à partir d’un ensemble de lignes où la ligne est définie par le schéma. En outre, comme mentionné précédemment, les colonnes dépourvues de données peuvent être omis. Si un attribut ou un sous-élément est utilisé dans la section de données et que cette construction n’a pas été définie dans la section de schéma, il est ignoré en mode silencieux.  
  
## <a name="string"></a>Chaîne  
 Les caractères XML réservés dans les données texte doivent être remplacées par des entités de caractères approprié. Par exemple, dans le nom de la société « De Jacques Garage », le guillemet simple doit être remplacé par une entité. La ligne se présente comme suit :  
  
```  
<z:row CompanyName="Joe's Garage"/>  
```  
  
 Les caractères suivants sont réservés dans XML et doivent être remplacées par des entités de caractères : {', », &,\<, >}.  
  
## <a name="binary"></a>Binaire  
 Données binaires sont bin.hex codé (autrement dit, un octet correspond à deux caractères, un caractère par quartet).  
  
## <a name="datetime"></a>DateTime  
 Le format VT_DATE variant n’est pas directement pris en charge par les types de données XML-Data. Le format correct pour les dates avec un composant à la fois une date et l’heure est aaaa-mm-jjThh.  
  
 Pour plus d’informations sur les formats de date spécifiés par XML, consultez le [spécification W3C XML-Data](https://go.microsoft.com/fwlink/?LinkId=5692).  
  
 Lorsque la spécification XML-Data définit deux types de données équivalents (par exemple, i4 == int), ADO sera écrire le nom convivial, mais elle est lue dans les deux.  
  
## <a name="managing-pending-changes"></a>La gestion des modifications en attente  
 Un jeu d’enregistrements peut être ouverts dans immédiate ou en mode de mise à jour par lots. Lorsqu’ils sont ouverts en mode de mise à jour par lot avec des curseurs côté client, toutes les modifications apportées à l’objet Recordset sont dans un état d’attente jusqu'à ce que la méthode UpdateBatch est appelée. Modifications en attente sont également conservés lorsque le jeu d’enregistrements est enregistré. Dans XML, ils sont représentés par l’utilisation des éléments « update » défini dans l’urn : schemas-microsoft-rowset. En outre, si un ensemble de lignes peut être mis à jour, la propriété modifiable doit être définie true dans la définition de la ligne. Par exemple, pour définir que la table Shippers contient des modifications en attente, la définition de ligne aurait aspect ressemblent aux suivantes.  
  
```  
<s:ElementType name="row" content="eltOnly" updatable="true">  
  <s:attribute type="ShipperID"/>  
  <s:attribute type="CompanyName"/>  
  <s:attribute type="Phone"/>  
  <s:extends type="rs:rowbase"/>  
</s:ElementType>  
```  
  
 Cela indique la persistance au fournisseur de données afin qu’ADO puisse créer un objet Recordset modifiable.  
  
 Les données d’exemple suivant montrent l’aspect des insertions, des modifications et des suppressions dans le fichier persistant.  
  
```  
<rs:data>  
  <z:row ShipperID="2" CompanyName="United Package"   
    Phone="(503) 555-3199"/>  
<rs:update>  
  <rs:original>  
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>  
  </rs:original>  
  <z:row Phone="(503) 552-7134"/>  
</rs:update>  
<rs:insert>  
  <z:row ShipperID="12" CompanyName="Lightning Shipping"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="13" CompanyName="Thunder Overnight"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="14" CompanyName="Blue Angel Air Delivery"   
    Phone="(505) 111-2222"/>  
</rs:insert>  
<rs:delete>  
  <z:row ShipperID="1" CompanyName="Speedy Express" Phone="(503) 555-9831"/>  
</rs:delete>  
</rs:data>  
```  
  
 Une mise à jour contienne toujours les données de ligne d’origine entier suivies par les données de la ligne modifiée. La ligne modifiée peut contenir toutes les colonnes ou uniquement les colonnes qui ont été modifiées. Dans l’exemple précédent, la ligne relative à Shipper 2 n’est pas modifiée, et que la colonne Phone a modifié les valeurs pour Shipper 3 et est donc la seule colonne incluse dans la ligne modifiée. Les lignes insérées pour Shipper 12, 13 et 14 sont traités par lot balise rs : insert sous un ensemble. Notez que les lignes supprimées peuvent également être regroupées, bien que cela n’est pas présenté dans l’exemple précédent.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
