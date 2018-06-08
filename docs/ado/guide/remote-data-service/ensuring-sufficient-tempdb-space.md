---
title: Espace TempDB suffisant | Documents Microsoft
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
- TempDB space in RDS [ADO]
ms.assetid: 09130db1-6248-4234-a1e5-a9c8e1622c06
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c86196fdf0320b5f3cb5028cb7d5db484c4da846
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ensuring-sufficient-tempdb-space"></a>Espace TempDB suffisant
Si des erreurs surviennent lors de la gestion des [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objets nécessitant un espace de traitement dans Microsoft SQL Server 6.5, vous devrez peut-être augmenter la taille de TempDB. (Certaines requêtes nécessitent un espace de traitement temporaire ; par exemple, une requête avec une clause ORDER BY exige le tri de la **Recordset**, ce qui nécessite de l’espace temporaire.)  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
> [!IMPORTANT]
>  Lisez cette procédure avant d’effectuer les actions, car il n’est pas aussi facile de réduire un appareil une fois qu’il est développé.  
  
> [!NOTE]
>  Par défaut inMicrosoft SQL Server 7.0 et versions ultérieur, TempDB est définie à croître automatiquement en fonction des besoins. Par conséquent, cette procédure peut uniquement être nécessaire sur les serveurs exécutant des versions antérieures à la version 7.0.  
  
### <a name="to-increase-the-tempdb-space-on-sql-server-65"></a>Pour augmenter l’espace de TempDB dans SQL Server 6.5  
  
1.  Démarrez Microsoft SQL Server Enterprise Manager, ouvrez l’arborescence du serveur, puis ouvrez l’arborescence de la base de données des appareils.  
  
2.  Sélectionnez une unité (physique) pour développer, telles que Master et double-cliquez sur le périphérique pour ouvrir la **modifier une unité de base de données** boîte de dialogue.  
  
     Cette boîte de dialogue affiche la quantité d’espace à l’aide de bases de données actuelles.  
  
3.  Dans le **taille** zone, augmentez l’appareil à la quantité souhaitée (par exemple, 50 mégaoctets (Mo) d’espace disque).  
  
4.  Cliquez sur **modifier maintenant** pour augmenter la quantité d’espace dans lequel la base de données (logique) tempdb.  
  
5.  Ouvrez l’arborescence de bases de données sur le serveur, puis double-cliquez sur **TempDB** pour ouvrir le **modifier la base de données** boîte de dialogue. Le **base de données** onglet répertorie la quantité d’espace actuellement alloué à TempDB (**taille des données**). Par défaut, il s’agit de 2 Mo.  
  
6.  Sous le **taille** , cliquez sur **Expand**. Les graphiques montrent l’espace disponible et alloué sur chacun des périphériques physiques. Les barres de couleur marron représentent l’espace disponible.  
  
7.  Sélectionnez un **journal**, telles que Master, pour afficher la taille disponible dans le **taille (Mo)** boîte.  
  
8.  Cliquez sur **développer maintenant** pour allouer cet espace à la base de données TempDB.  
  
     Le **modifier la base de données** boîte de dialogue affiche la nouvelle taille allouée à TempDB.  
  
 Pour plus d’informations sur cette rubrique, recherchez le fichier d’aide de Microsoft SQL Server Enterprise Manager pour « Boîte de dialogue de développement de base de données ».  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)


