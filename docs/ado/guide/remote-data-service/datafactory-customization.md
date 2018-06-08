---
title: Personnalisation de DataFactory | Documents Microsoft
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
- DataFactory customization in RDS [ADO]
ms.assetid: 86d77985-a0d0-405a-8587-c85a20540a0e
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 174cc584b0f1ca89627dffcd3f85bb4a5f384229
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datafactory-customization"></a>DataFactory, personnalisation
Service de données à distance (RDS) permet de facilement accéder aux données dans un système à trois niveaux client/serveur. Un contrôle des données client spécifie des paramètres de chaîne de connexion et de commande pour effectuer une requête sur une source de données distante ou une chaîne de connexion et [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) paramètres nécessaires pour effectuer une mise à jour de l’objet.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Les paramètres sont transmis à un programme de serveur qui effectue l’opération d’accès aux données sur la source de données distante. RDS fournit un programme serveur par défaut le [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet. Le **RDSServer.DataFactory** objet renvoie aucune **Recordset** objet produit par une requête au client.  
  
 Toutefois, le **RDSServer.DataFactory** est limitée à l’exécution des requêtes et des mises à jour. Il ne peut effectuer la validation ou le traitement sur les chaînes de connexion ou de commande.  
  
 Avec ADO, vous pouvez spécifier que le **DataFactory** fonctionnent conjointement avec un autre type de programme serveur appelé un *gestionnaire*. Le gestionnaire peut modifier les chaînes de commande et de connexion du client avant d’être utilisées pour accéder à la source de données. En outre, le gestionnaire peut appliquer des droits d’accès qui régissent la capacité du client pour lire et écrire des données dans la source de données.  
  
 Les paramètres utilisés par le gestionnaire pour modifier les paramètres du client et les droits d’accès sont spécifiés dans les sections d’un fichier de personnalisation.  
  
 Les rubriques suivantes fournissent plus d’informations sur la personnalisation de la **DataFactory** objet.  
  
-   [Présentation du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)  
  
-   [Fichier de personnalisation, section connect](../../../ado/guide/remote-data-service/customization-file-connect-section.md)  
  
-   [Fichier de personnalisation, section SQL](../../../ado/guide/remote-data-service/customization-file-sql-section.md)  
  
-   [Fichier de personnalisation, section UserList](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)  
  
-   [Fichier de personnalisation, section Logs](../../../ado/guide/remote-data-service/customization-file-logs-section.md)  
  
-   [Paramètres client obligatoires](../../../ado/guide/remote-data-service/required-client-settings.md)  
  
-   [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)


