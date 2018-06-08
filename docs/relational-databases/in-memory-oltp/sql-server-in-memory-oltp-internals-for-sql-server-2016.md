---
title: Mécanismes internes d’OLTP en mémoire de SQL Server pour SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 09/14/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b14da361-a6b8-4d85-b196-7f2f13650f44
caps.latest.revision: 2
author: jodebrui
ms.author: jodebrui
manager: craigg
ms.openlocfilehash: a1e2c9d3da182dae8b3b5831b009d410a44906a3
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-in-memory-oltp-internals-for-sql-server-2016"></a>Mécanismes internes d’OLTP en mémoire de SQL Server pour SQL Server 2016
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

**Résumé :** OLTP en mémoire, fréquemment appelé par son nom de code « Hekaton », a été introduit dans SQL Server 2014.
Cette technologie puissante vous permet de tirer parti de grandes quantités de mémoire et de plusieurs dizaines de cœurs pour augmenter les performances des opérations OLTP d’un facteur allant jusqu’à 30 à 40 ! SQL Server 2016 poursuit l’investissement réalisé dans OLTP en mémoire en supprimant la plupart des restrictions de SQL Server 2014 et en optimisant les algorithmes de traitement interne, de façon à améliorer encore davantage OLTP en mémoire. Ce document décrit l’implémentation de la technologie OLTP en mémoire de SQL Server 2016 dans la version RTM de SQL Server 2016. Avec OLTP en mémoire, les tables peuvent être déclarées comme « optimisées en mémoire » pour activer les fonctionnalités d’OLTP en mémoire. Les tables optimisées en mémoire sont entièrement transactionnelles et sont accessibles via Transact-SQL. Les procédures stockées, les déclencheurs et les fonctions scalaires définies par l’utilisateur de Transact-SQL peuvent être compilés en code machine pour améliorer les performances sur les tables optimisées en mémoire. Le moteur est conçu pour la haute concurrence, sans aucun blocage.    
  
**Auteur :** Kalen Delaney  
  
**Réviseurs techniques :** Sunil Agarwal et Jos de Bruijn  
  
**Date de publication :** juin 2016  
  
**S’applique à :** SQL Server 2016  
  
Pour consulter le document, téléchargez le document [SQL Server In-Memory OLTP Internals for SQL Server 2016](http://download.microsoft.com/download/8/3/6/8360731A-A27C-4684-BC88-FC7B5849A133/SQL_Server_2016_In_Memory_OLTP_White_Paper.pdf) .   
