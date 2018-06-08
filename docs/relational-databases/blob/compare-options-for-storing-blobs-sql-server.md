---
title: Comparer les options pour le stockage des objets blob (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: blob
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-blob
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6038697b-36a9-49e8-a02a-2ad9e2e60e5a
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1d31a086ccc3cf60d245db67007a9cb2fe26dae1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="compare-options-for-storing-blobs-sql-server"></a>Comparer les options pour le stockage des objets blob (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Explique et compare les options disponibles pour stocker des fichiers et des documents dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="Expectations"></a> Stockage de fichiers dans la base de données – Avantages et attentes  
 Un pourcentage important des données d'entreprise correspond à des données non structurées par nature et est stocké en général sous la forme de fichiers et de documents dans des systèmes de fichiers. La plupart de ces données sont produites, gérées et consommées par des applications qui accèdent aux fichiers via des API Windows. Les entreprises conservent en général ces données dans le système de fichiers, en stockant les métadonnées connexes des fichiers dans une base de données relationnelle.  
  
 L'intégration de données non structurées à la base de données relationnelle apporte des avantages significatifs. Ces avantages incluent ce qui suit :  
  
-   Fonctions intégrées de stockage et de gestion des données telles que la sauvegarde.  
  
-   Services intégrés tels que la recherche en texte intégral et la recherche sémantique sur les données et métadonnées.  
  
-   Facilité d'administration et gestion de la stratégie sur les données non structurées.  
  
 Dans la plupart des cas, cependant, il n'a pas été possible de stocker aisément de telles données non structurées dans une base de données relationnelle. Il n'était pas possible précédemment d'exécuter des applications basées sur Windows existantes sur des systèmes relationnels. Il n'est pas pratique de réécrire des applications établies (telles que Microsoft Word ou Adobe Reader) pour s'exécuter sur des API de base de données relationnelle. Ces applications attendent simplement que les données soient accessibles via des API Windows. En d'autres termes, les attentes concernent les éléments suivants :  
  
-   Les applications Windows ne sont pas informées des transactions de base de données et ne les requièrent pas.  
  
-   Les applications Windows requièrent la compatibilité avec les API du système de fichiers pour les données de répertoire et de fichier.  
  
##  <a name="Filestream"></a> FILESTREAM  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispose déjà de la fonctionnalité FILESTREAM, qui fournit un stockage efficace, une gestion et une diffusion en continu de données non structurées stockées en tant que fichiers sur le système de fichiers. Toutefois, une solution FILESTREAM nécessite une programmation personnalisée et ne répond pas à la configuration requise en matière de compatibilité complète avec les applications Windows décrite ci-dessus.  
  
##  <a name="FileTables"></a> FileTables  
 La fonctionnalité FileTable s'appuie sur les fonctions FILESTREAM existantes afin de permettre aux clients d'entreprise de stocker des données de fichier non structurées et des hiérarchies de répertoires dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en respectant les configurations requises pour l'accès non transactionnel et la compatibilité d'applications Windows pour les données basées sur des fichiers.  
  
##  <a name="CompareFileTable"></a> Comparaison de FILESTREAM et de FileTable  
  
|Fonctionnalité|Serveur de fichiers et solution de base de données|Solution FILESTREAM|Solution FileTable|  
|-------------|---------------------------------------|-------------------------|------------------------|  
|**Histoire unique pour les tâches de gestion**|non|Oui|**Oui**|  
|**Ensemble unique de services**: recherche, création de rapports, interrogation, etc.|non|Oui|**Oui**|  
|**Modèle de sécurité intégré**|non|Oui|**Oui**|  
|**Mises à jour sur place de données FILESTREAM**|Oui|non|**Oui**|  
|**Hiérarchie de répertoires et de fichiers maintenue dans la base de données**|non|non|**Oui**|  
|**Compatibilité d'applications Windows**|Oui|non|**Oui**|  
|**Accès relationnel aux attributs de fichier**|non|non|**Oui**|  
  
##  <a name="CompareRBS"></a> Comparaison de FILESTREAM et du magasin d'objets blob distants (RBS)  
 Pour obtenir une comparaison de ces deux fonctionnalités, consultez la publication de blog suivante rédigée par l'équipe RBS : [SQL Server Remote BLOB Store and FILESTREAM feature comparison (en anglais)](http://go.microsoft.com/fwlink/?LinkId=210317).  
  
##  <a name="more"></a> Informations supplémentaires  
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)  
 [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md)  
 [Magasin d’objets blob distants &#40;RBS&#41; &#40;SQL Server&#41;](../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
