---
title: Comparaison du stockage des tables sur disque et du stockage des tables optimisées en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: eacf443c-001a-445f-ad1c-5f5a45eca6f4
caps.latest.revision: 5
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: bb1251cc7127e93f116a1a2fb697b3f6f41d831f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="comparing-disk-based-table-storage-to-memory-optimized-table-storage"></a>Comparaison du stockage des tables sur disque et du stockage des tables optimisées en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  
  
|Catégories|Table sur disque|Table mémoire optimisée durable|  
|----------------|-----------------------|-------------------------------------|  
|DDL|Les informations de métadonnées sont stockées dans les tables système dans le groupe de fichiers primaire de la base de données, et sont accessibles via les affichages catalogue.|Les informations de métadonnées sont stockées dans les tables système dans le groupe de fichiers primaire de la base de données, et sont accessibles via les affichages catalogue.|  
|Structure|Les lignes sont stockées dans des pages de 8 Ko. Une page stocke uniquement les lignes de la même table.|Les lignes sont stockées en tant que lignes individuelles. Il n'y a aucune structure de page. Deux lignes consécutives dans un fichier de données peuvent appartenir à des tables mémoire optimisées différentes.|  
|Index|Les index sont stockés dans une structure de page semblable aux lignes de données.|Seule la définition d'index est conservée (pas les lignes d'index). Les index sont conservés en mémoire et sont régénérés lorsque la table mémoire optimisée est chargée lors du redémarrage d'une base de données. Étant donné que les lignes d'index ne sont pas conservées, aucune journalisation n'a lieu pour les modifications d'index.|  
|Opération DML|La première étape est la recherche de la page, puis son chargement dans le pool de mémoires tampons.<br /><br /> Insert<br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] insère la ligne dans la page selon le classement des lignes s'il s'agit d'un index cluster.<br /><br /> DELETE<br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche la ligne à supprimer dans la page et la marque comme supprimée.<br /><br /> Update<br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche la ligne dans la page. La mise à jour est effectuée sur place pour les colonnes non-clés. La mise à jour de la colonne clé est effectuée par une opération de suppression et d'insertion.<br /><br /> Une fois l'opération DML terminée, les pages concernées sont vidées sur le disque dans le cadre de la stratégie du pool de mémoires tampons, du point de contrôle ou de la validation des transactions pour les opérations avec une journalisation minime. Les opérations de lecture/écriture sur les pages entraînent des E/S superflues.|Pour les tables mémoire optimisées, les opérations DML sont effectuées directement en mémoire puisque les données résident dans la mémoire. Un thread d'arrière-plan lit les enregistrements du journal des tables mémoire optimisées et les rend persistants dans les fichiers de données et delta. Une mise à jour génère une nouvelle version de ligne. Mais une mise à jour est journalisée comme une suppression suivie d'une insertion.|  
|Fragmentation des données|La manipulation des données fragmente les données et aboutit à des pages partiellement remplies et à des pages logiquement consécutives qui ne sont pas contiguës sur le disque. Ceci dégrade les performances d'accès aux données et vous oblige à défragmenter des données.|Les données mémoire optimisées ne sont pas stockées dans des pages, par conséquent, il n'y a pas de fragmentation de données. Toutefois, à mesure que des lignes sont mises à jour/supprimées, les fichiers de données et delta doivent être compactés. Cette opération est effectuée par un thread d'arrière-plan MERGE basé sur une stratégie de fusion.|  
  
## <a name="see-also"></a> Voir aussi  
 [Création et gestion du stockage des objets mémoire optimisés](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
