---
title: Réduire l’utilisation de l’espace de fichier journal | Documents Microsoft
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
- log file space in RDS [ADO]
ms.assetid: 669662a0-e20f-483e-ab28-53f66c524c98
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7b0a0b3ba5bdaba5f334bce70acf87e52656c13b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="minimizing-log-file-space-usage"></a>Réduire l’utilisation de l’espace de fichier journal
Un fichier journal peut se remplir rapidement (provoquant ainsi l’abandon du serveur) s’il existe un grand volume d’activité sur une base de données SQL Server. Vous pouvez définir le fichier journal **vidage au point de contrôle** pour allonger de manière significative la durée de vie du fichier journal pour une base de données.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
### <a name="to-enable-truncate-on-checkpoint-in-microsoft-sql-server-65"></a>Pour activer le vidage au point de contrôle dans Microsoft SQL Server 6.5  
  
1.  Démarrez Microsoft SQL Server Enterprise Manager, ouvrez l’arborescence du serveur, puis ouvrez l’arborescence de la base de données des appareils.  
  
2.  Double-cliquez sur le nom de la base de données sur lequel cette fonctionnalité sera activée.  
  
3.  À partir de la **base de données** onglet, sélectionnez **Truncate**.  
  
4.  À partir de la **Options** onglet, sélectionnez **tronquer le journal sur le point de contrôle**, puis cliquez sur **OK**.  
  
### <a name="to-enable-truncate-on-checkpoint-in-microsoft-sql-server-70"></a>Pour activer le vidage au point de contrôle dans Microsoft SQL Server 7.0  
  
1.  Démarrez Microsoft SQL Server Enterprise Manager, ouvrez l’arborescence du serveur, puis ouvrez l’arborescence de bases de données.  
  
2.  Cliquez sur le nom de la base de données sur lequel cette fonctionnalité est activée et choisissez **propriétés**.  
  
3.  À partir de la **Options** onglet, sélectionnez **tronquer le journal sur le point de contrôle**, puis cliquez sur **OK**.  
  
 Pour plus d’informations sur la **vidage au point de contrôle** de la fonctionnalité, consultez la documentation de Microsoft SQL Server.  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)


