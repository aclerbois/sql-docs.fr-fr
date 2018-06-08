---
title: Configuration du stockage des tables optimisées en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 10/25/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6e005de0-3a77-4b91-b497-14cc0f9f6605
caps.latest.revision: 7
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 801fa05e3bf50fbf3fdc4abf4481bc8db793ae81
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="configuring-storage-for-memory-optimized-tables"></a>Configuration du stockage des tables optimisées en mémoire
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous devez configurer des opérations d'entrée/sortie par seconde (IOPS) et la capacité de stockage.  
  
## <a name="storage-capacity"></a>Capacité de stockage  
 Utilisez les informations contenues dans [Estimer les besoins en mémoire des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) pour estimer la taille en mémoire des tables durables optimisées en mémoire de la base de données. Les index n'étant pas conservés pour les tables mémoire optimisées, n'incluez pas la taille des index. Une fois que vous avez déterminé la taille, vous devez fournir l'espace disque correspondant à quatre fois la taille des tables durables en mémoire.  
  
## <a name="storage-iops"></a>E/S par seconde dédiées au stockage  
 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] peut augmenter considérablement le débit de la charge de travail. Par conséquent, il est important de s'assurer que les E/S ne forment pas un goulet d'étranglement.  
  
-   Lorsque vous migrez des tables sur disque vers des tables mémoire optimisées, vérifiez que le journal des transactions se trouve sur un support de stockage capable de prendre en charge le surcroît d'activité du journal des transactions. Par exemple, si votre support de stockage prend en charge des opérations du journal des transactions à 100 Mo/s et que les tables mémoire optimisées offrent des performances cinq fois supérieures, le support de stockage du journal des transactions doit être en mesure de prendre en charge une amélioration des performances cinq fois supérieure, pour empêcher l'activité du journal des transactions de devenir un goulot d'étranglement des performances.  
  
-   Les tables à mémoire optimisée sont conservées dans des fichiers de point de contrôle, qui sont distribués sur un ou plusieurs conteneurs. Chaque conteneur doit généralement être mappé à son propre dispositif de stockage et est utilisé pour augmenter la capacité de stockage et améliorer les performances d’E/S par seconde (IOPS). Vous devez vous assurer que les IOPS séquentielles du support de stockage peuvent prendre en charge jusqu’à 3 fois le débit soutenu du journal des transactions. Les opérations d’écriture dans les fichiers de point de contrôle représentent 256 Ko pour les fichiers de données et 4 Ko pour les fichiers delta.
  
     - Par exemple, si les tables à mémoire optimisée génèrent 500 Mo/s d’activité soutenue dans le journal des transactions, le stockage pour ces tables doit prendre en charge 1,5 Go/s d’IOPS. La nécessité de prendre en charge le triple du débit soutenu du journal des transactions s’explique par le fait que les paires de fichiers de données et delta sont d’abord écrites avec les données initiales, puis doivent être lues/réécrites dans le cadre d’une opération de fusion.  
  
- Le temps de récupération des tables optimisées en mémoire est un autre facteur entrant en compte dans l’estimation des IOPS pour le stockage. Les données des tables durables doivent être lues en mémoire avant qu'une base de données puisse être mise à la disposition des applications. Généralement, le chargement de données dans les tables mémoire optimisées peut être effectué à la vitesse des opérations d’E/S par seconde (IOPS). Si le stockage total des tables durables, mémoire optimisées, s'élève à 60 Go et que vous souhaitez pouvoir charger ces données en une minute, les IOPS du stockage doivent être définies à 1 Go/sec.  
  
-   Les fichiers de point de contrôle sont généralement distribués uniformément entre tous les conteneurs, si l’espace disponible le permet. Avec SQL Server 2014, vous devez provisionner un nombre impair de conteneurs pour obtenir une distribution uniforme ; à partir de la version 2016, les nombres de conteneurs impairs comme pairs assurent une distribution uniforme.
  
## <a name="encryption"></a>Chiffrement  
 Dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], le stockage des tables optimisées en mémoire est chiffré dans le cadre de l’activation de TDE (Transparent Data Encryption) sur la base de données. Pour plus d’informations, consultez [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md). Dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], les fichiers de point de contrôle ne sont pas chiffrés, même si TDE est activé sur la base de données.
  
## <a name="see-also"></a> Voir aussi  
 [Création et gestion du stockage des objets mémoire optimisés](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
