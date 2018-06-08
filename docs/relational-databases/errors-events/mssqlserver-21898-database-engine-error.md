---
title: MSSQLSERVER_21898 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 21898 (Database Engine error)
ms.assetid: 02405b21-3d4e-4c2d-b4b3-d7b1ec05edb4
caps.latest.revision: 6
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 381f7cd9cdaede9b8048f46c76253b02161ce404
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver21898"></a>MSSQLSERVER_21898
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|21898|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum21898|  
|Texte du message|Le serveur de publication '%s' utilise la base de données de distribution '%s' et non '%s', requise pour héberger la base de données de publication '%s'. Exécutez **sp_changedistpublisher** sur le serveur de distribution '%s' pour remplacer la base de données du serveur de distribution utilisée par le serveur de publication par '%s'.|  
  
## <a name="explanation"></a>Explication  
**sp_validate_redirected_publisher** interroge msdb.dbo.MSdistpublishers sur le serveur de distribution local pour vérifier que la base de données de distribution utilisée par le nouveau serveur de publication est la même que la base de données de distribution utilisée par le serveur de publication d’origine. Cette erreur est retournée lorsque ces bases de données diffèrent, faisant du serveur de publication un hôte incorrect pour la base de données du serveur de publication.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez la procédure stockée **sp_changedistpublisher** pour remplacer la base de données de distribution du nouveau serveur de publication et utiliser celle utilisée par le serveur de publication d’origine.  
  
> [!NOTE]  
> L’exécution de **sp_changedistpublisher** résout le problème si la base de données de distribution incorrecte a été configurée quand **sp_adddistpublisher** a été exécuté sur le serveur de distribution pour le serveur de publication. Toutefois, si le serveur de publication distant possède des publications existantes provenant d'une autre base de données de publication qui utilisent la base de données de distribution identifiée, cette modification n'est pas appropriée. La réplication utilisant la base de données de distribution nommée doit être systématiquement supprimée puis rétablie à l'aide de la base de données de distribution d'origine du serveur de publication pour que le nouveau serveur de publication fonctionne comme un hôte approprié.  
  
