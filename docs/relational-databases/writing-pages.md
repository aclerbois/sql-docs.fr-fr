---
title: Écriture de pages | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: relational-databases-misc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- server-general
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- pages
ms.assetid: 409c8753-03c4-436d-839c-6a5879971551
caps.latest.revision: 2
author: pmasl
ms.author: pelopes
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 428a59514b2884341e3adcbe842dff28eeb06d17
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="writing-pages"></a>Écritures de pages
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Les E/S d’une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)] incluent les écritures logiques et physiques. Une écriture logique se produit lorsque les données sont modifiées dans une page du cache des tampons. Une écriture physique se produit lorsque la page est écrite du [cache des tampons](../relational-databases/memory-management-architecture-guide.md) sur le disque.

Lorsqu’une page est modifiée dans le cache des tampons, elle n’est pas réécrite immédiatement sur le disque, mais elle est marquée comme erronée. Cela signifie qu'une page peut avoir plusieurs écritures logiques avant son écriture physique sur le disque. Pour chaque écriture logique, un enregistrement du journal des transactions est inséré dans le cache du journal qui enregistre la modification. L'enregistrement doit être écrit sur le disque avant que la page de modifications associée n'ait été supprimée du cache et écrite sur le disque. SQL Server utilise une technique appelée journal WAL (write-ahead log) qui empêche l’écriture d’une page de modifications avant l’écriture sur le disque de l’enregistrement associé. Il s'agit là d'un aspect essentiel au bon fonctionnement du gestionnaire de récupération. Pour plus d’informations, consultez [Journal des transactions à écriture anticipée](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).

L'illustration ci-dessous montre le processus d'écriture d'une page de données modifiée.

![Writing_Pages](../relational-databases/media/writing-pages.gif)

Si le gestionnaire de mémoires tampons écrit une page, il recherche les pages de modifications adjacentes pouvant être incluses dans une seule opération d'écriture de regroupement. Les pages adjacentes ont des ID de pages consécutifs et sont issues du même fichier ; il n'est pas nécessaire que les pages soient contiguës dans la mémoire. La recherche se continue en amont et en aval jusqu'à ce que l'un des événements suivants se produisent :

 * une page correcte a été trouvée ;
 * 32 pages ont été trouvées ;
 * une page incorrecte a été trouvée et son numéro séquentiel dans le journal n'a pas été encore vidé dans le journal ;
 * une page qui ne peut pas être immédiatement verrouillée a été trouvée.

De cette manière, le jeu complet de pages peut être écrit sur le disque au cours d'une seule opération d'écriture avec regroupement. 

Juste avant l'écriture d'une page, la forme de protection de page spécifiée dans la base de données est ajoutée à la page. En cas d’ajout de la protection de page endommagée, la page doit être verrouillée exclusivement pour les E/S. En effet, la protection de page endommagée modifie la page et la rend illisible pour n'importe quel autre thread. Si la protection de la somme de contrôle est ajoutée, ou que la base de données n'utilise aucune protection de page, la page est verrouillée avec un verrou de mise à jour pour les E/S. Ce verrou empêche quiconque de modifier la page durant l'écriture tout en permettant aux lecteurs de l'utiliser. Pour plus d’informations sur les options de protection de page d’E/S disque, consultez [Gestion des tampons](../relational-databases/memory-management-architecture-guide.md).

Une page de modifications est écrite sur le disque selon trois méthodes : 

* Écriture différée   
 L'écriture différée est un processus système qui assure la disponibilité des tampons libres en supprimant les pages peu fréquemment utilisées du cache des tampons. Les pages de modifications sont écrites en priorité sur le disque. 

* Écriture anticipée   
 Ce processus écrit les pages de données incorrectes associées à des opérations non consignées telles que bulk insert et select into. Ce processus permet la création et l'écriture en parallèle de nouvelles pages. Autrement dit, il n'est pas nécessaire que l'opération d'appel attende la fin de l'opération entière avant d'écrire les pages sur le disque.

* Point de contrôle   
 Le processus de point de contrôle analyse régulièrement le cache à la recherche de tampons contenant des pages issues d'une base de données spécifiée et écrit toutes les pages de modifications sur le disque. Les points de contrôle permettent une récupération ultérieure du système en créant un point où toutes les pages de modifications sont effectivement écrites sur le disque. L’utilisateur peut demander une opération de point de contrôle à l’aide de la commande CHECKPOINT, ou le [!INCLUDE[ssDE](../includes/ssde-md.md)] peut générer des points de contrôle automatiques en fonction de la quantité d’espace de journal utilisée et la durée écoulée depuis le dernier point de contrôle. De plus, un point de contrôle est généré si certaines activités se produisent. Par exemple, si un fichier journal ou de données est ajouté ou supprimé d’une base de données, ou en cas d’arrêt de l’instance de SQL Server. Pour plus d’informations, voir [Points de contrôle et partie active du journal](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).

Les processus de l'écriture différée, de l'écriture anticipée et des points de contrôle n'attendent pas la fin de l'opération d'E/S. Ils utilisent toujours les E/S asynchrones (ou chevauchées) et poursuivent d'autres tâches, et vérifient le bon fonctionnement de l'E/S ultérieurement. De cette manière, SQL Server peut optimiser son utilisation des ressources d’E/S et d’UC pour les tâches appropriées.

## <a name="see-also"></a> Voir aussi
[Guide d’architecture des pages et des étendues](../relational-databases/pages-and-extents-architecture-guide.md)   
 [Lecture de pages](../relational-databases/reading-pages.md)
