---
title: DataSpace, objet (RDS) | Documents Microsoft
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
- DataSpace object [RDS]
ms.assetid: 9194bffa-5bdf-4dff-af86-f7158c23bfa7
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4624cf678df777a33e39a2d5438cbdc613ff7e02
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dataspace-object-rds"></a>DataSpace, objet (RDS)
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Crée un proxy côté client à des objets métier personnalisés situés sur la couche intermédiaire.  
  
 Remote Data Service nécessite des proxys d’objets métier afin que les composants côté client peuvent communiquer avec les objets métiers sur la couche intermédiaire. Les proxys permettent l’empaquetage, 8369Les et de transport (marshaling) de l’application [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) données au-delà des limites de processus ou d’ordinateur.  
  
 Service de données distant utilise le **RDS. DataSpace** l’objet [CreateObject](../../../ado/reference/rds-api/createobject-method-rds.md) méthode pour créer des proxys d’objets métier. Proxy d’objet métier est créé dynamiquement chaque fois qu’une instance de son équivalent d’objet métier de couche intermédiaire est créée. Service de données distant prend en charge les protocoles suivants : HTTP, HTTPS (HTTP Secure Sockets), DCOM et in-process (composants du client et l’objet métier résident sur le même ordinateur).  
  
> [!NOTE]
>  Services Bureau à distance se comporte de manière « sans état » lorsque le **RDS. DataSpace** objet utilise les protocoles HTTP ou HTTPS. Autrement dit, toutes les informations internes sur une demande du client sont ignorées une fois que le serveur retourne une réponse.  
  
> [!NOTE]
>  Bien que l’objet métier semble exister pour la durée de vie de proxy d’objet métier, l’objet métier existe réellement uniquement jusqu'à ce qu’une réponse est envoyée à une demande. Lorsqu’une demande est émise (autrement dit, une méthode est appelée sur l’objet métier), le proxy ouvre une nouvelle connexion au serveur et le serveur crée une nouvelle instance de l’objet métier. Une fois que l’objet métier répond à la demande, le serveur détruit l’objet métier et ferme la connexion.  
  
> [!NOTE]
>  Ce comportement signifie que vous ne peut pas passer des données à partir d’une demande à un autre à l’aide d’une propriété de l’objet métier ou une variable. Vous devez utiliser un autre mécanisme, tel qu’un fichier ou un argument de méthode, pour rendre persistantes les données d’état.  
  
 L’ID de classe pour le **RDS. DataSpace** objet est 0-983A-00C04FC29E36 BD96C556 65A3 - 11 D.  
  
 Le **DataSpace** objet est sécurisé pour le script.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet DataSpace (RDS)](../../../ado/reference/rds-api/dataspace-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [DataSpace (exemple d’objet) et CreateObject (exemple de méthode) (VBScript)](../../../ado/reference/rds-api/dataspace-object-and-createobject-method-example-vbscript.md)


