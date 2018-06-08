---
title: SQL Server, objet Transactions | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Transactions
- Transactions object
ms.assetid: 85240267-78fd-476a-9ef6-010d6cf32dd8
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f28b5a581bf784f67ffdd1d90e84fb12a33c51c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-transactions-object"></a>SQL Server, objet Transactions
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet **Transactions** dans Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des compteurs pour analyser le nombre de transactions actives dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]ainsi que les effets de ces transactions sur les ressources, tels que la banque de versions de lignes avec isolement d'instantané dans **tempdb**. Les transactions sont des unités logiques de travail, c'est-à-dire un ensemble d'opérations qui doivent toutes aboutir ou être toutes supprimées d'une base de données afin de maintenir l'intégrité logique des données. Toutes les modifications de données dans les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont effectuées dans des transactions.  
  
 Lorsqu'une base de données est configurée pour autoriser un niveau d'isolement d'instantané, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit gérer un enregistrement des modifications apportées à chaque ligne de la base de données. Chaque fois qu'une ligne est modifiée, une copie de la ligne telle qu'elle existait avant la modification est enregistrée dans une banque de versions de lignes dans **tempdb**. De nombreux compteurs de l'objet **Transaction** peuvent être utilisés pour analyser la taille et le taux de croissance de la banque de versions de lignes dans **tempdb**.  
  
 Les compteurs de l'objet **Transactions** relèvent toutes les transactions contenues dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Ce tableau décrit les compteurs **SQLServer:Transactions** .  
  
|Compteurs de transactions SQL Server|Description|  
|--------------------------------------|-----------------|  
|**Espace disponible dans tempdb (Ko)**|Quantité d’espace (en kilo-octets) disponible dans **tempdb**. Il faut suffisamment d'espace libre pour contenir le magasin de versions avec niveau d'isolement d'instantané et tous les nouveaux objets temporaires créés dans cette instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|**Délai le plus long d'exécution de transaction**|Durée d'attente (en secondes) depuis le démarrage de la transaction qui a été active plus longtemps que toute autre transaction actuelle. Ce compteur ne montre une activité que lorsque la base de données est exécutée avec le niveau d'isolement d'instantané de lecture validée. Il ne consigne aucune activité si la base de données se trouve dans n'importe quel autre niveau d'isolement.|  
|**Transactions de versions non liées à des instantanés**|Nombre de transactions actives n'utilisant pas le niveau d'isolement d'instantané et ayant apporté des modifications aux données, qui ont généré des versions de lignes dans le magasin de versions **tempdb** .|  
|**Transactions d'instantanés**|Nombre de transactions actives utilisant le niveau d'isolement d'instantané.<br /><br /> Remarque : Le compteur d’objets **Transactions de captures instantanées** répond lors du premier accès aux données et pas lors de l’émission de l’instruction `BEGIN TRANSACTION` .|  
|**Transactions**|Nombre de transactions actives de tous types.|  
|**Proportion de conflits de mise à jour**|Pourcentage des transactions utilisant le niveau d'isolement d'instantané et qui ont rencontré des conflits de mise à jour au cours de la dernière seconde. Un conflit de mise à jour se produit lorsqu'une transaction de niveau d'isolement d'instantané tente de modifier une ligne dont la dernière modification a été effectuée par une autre transaction qui n'était pas validée lors du démarrage de la transaction de niveau d'isolement d'instantané.|  
|**Base de la proportion de conflits de mise à jour**|À usage interne uniquement|
|**Transactions d'instantanés de mise à jour**|Nombre de transactions actives utilisant le niveau d'isolement d'instantané et qui ont modifié des données.|  
|**Taux de nettoyage de version (Ko/s)**|Taux (en kilo-octets par seconde) auquel des versions de lignes sont supprimées de la banque de versions avec isolement d’instantané dans **tempdb**.|  
|**Taux de génération de version (Ko/s)**|Taux (en kilo-octets par seconde) auquel de nouvelles versions de lignes sont ajoutées à la banque de versions avec isolement d’instantané dans **tempdb**.|  
|**Taille du magasin de versions (Ko)**|Quantité d’espace (en kilo-octets) dans **tempdb** utilisée pour stocker des versions de lignes avec niveau d’isolement d’instantané.|  
|**Nombre d'unités dans le magasin de versions**|Nombre d'unités d'allocation actives dans le magasin de versions avec isolement d'instantané dans **tempdb**.|  
|**Création d'unité dans le magasin de versions**|Nombre d'unités d'allocation ayant été créées dans le magasin d'isolement d'instantané depuis le démarrage de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
|**Troncation d'unité dans le magasin de versions**|Nombre d'unités d'allocation ayant été supprimées du magasin d'isolement d'instantané depuis le démarrage de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
