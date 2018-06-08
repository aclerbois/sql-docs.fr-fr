---
title: Fournisseurs et les composants de service | Documents Microsoft
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
- OLE DB providers [ADO]
- ADO, OLE DB providers
- service providers [ADO]
ms.assetid: 1fd7a374-587b-4ca9-9204-3a4019b67a71
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a49731edea9e0aa59977eec3aa73cac32b372b4a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="service-providers-and-components"></a>Fournisseurs de services et de composants
Fournisseurs de services sont des composants qui étendent les fonctionnalités des fournisseurs de données en implémentant les interfaces étendues qui ne sont pas prise en charge par le magasin de données.  
  
 Universal Data Access offre une *architecture du composant* qui permet des composants individuels spécialisés d’implémenter des ensembles indépendants de fonctionnalités de base de données, ou « services », sur des magasins moins performant. Par conséquent, plutôt que de magasins de données pour fournir sa propre implémentation de fonctionnalités étendues ou forcer des applications génériques pour implémenter les fonctionnalités de base de données en interne, les composants du service fournissent une implémentation commune n’importe quelle application peut Utilisez pour accéder à n’importe quel magasin de données. Le fait que certaines fonctionnalités sont implémentée en mode natif par le magasin de données et certains via les composants génériques est transparent pour l’application.  
  
 Par exemple, un curseur de moteur, tel que [le Service de curseur pour OLE DB](http://msdn.microsoft.com/en-us/57638feb-4ecd-4051-becb-8f828d21cf44), est un composant de service qui peut utiliser des données à partir d’un magasin de données séquentielles et en avant uniquement pour produire des données permettant le défilement. Incluent d’autres fournisseurs de services généralement utilisés par ADO le [Microsoft OLE DB fournisseur de persistance (fournisseur de services ADO)](../../../ado/guide/appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) (pour l’enregistrement de données dans un fichier,) le [Service de mise en forme des données Microsoft pour OLE DB (fournisseur de services ADO) ](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (pour hiérarchique **jeux d’enregistrements**) et le [fournisseur Microsoft OLE DB la communication à distance (fournisseur de services ADO)](../../../ado/guide/appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md) (pour l’appel des fournisseurs de données sur un ordinateur distant).  
  
 Pour plus d’informations sur les fournisseurs de services et de données, consultez [annexe a : fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md).
