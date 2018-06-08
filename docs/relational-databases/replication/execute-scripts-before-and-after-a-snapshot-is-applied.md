---
title: Exécuter des scripts avant et après l’application d’un instantané | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], scripts
- scripts [SQL Server replication], snapshots
- snapshot replication [SQL Server], scripts
ms.assetid: b7bb1e4c-5b48-4bb1-9dc8-47c911f2cc82
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3d69aaefe439feea910c62b1aff36ebe38e402a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-scripts-before-and-after-a-snapshot-is-applied"></a>Exécuter des scripts avant et après l’application d’un instantané
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Spécifiez un script facultatif à exécuter avant ou après l’application de l’instantané à la page **Instantané** de la boîte de dialogue **Propriétés de la publication - \<Publication>**. Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
> [!NOTE]  
>  Ces options ne sont pas disponibles si l'option **Format d'instantané** est définie à **Caractère**.  
  
### <a name="to-execute-a-script-before-or-after-a-snapshot-is-applied"></a>Pour exécuter un script avant ou après l'application d'un instantané  
  
1.  Dans la page **Instantané** de la boîte de dialogue **Propriétés de la publication - \<Publication>**  :  
  
    -   Pour spécifier un script à exécuter avant l'application de l'instantané, cliquez sur **Parcourir** pour rechercher le script, ou entrez un chemin d'accès pour le script dans la zone de texte **Exécuter ce script avant l'application de l'instantané** .  
  
        > [!NOTE]  
        >  Les Agents de distribution et de fusion doivent avoir des autorisations en lecture sur le répertoire spécifié. Si des abonnements par extraction sont utilisés, vous devez spécifier un répertoire partagé sous la forme d’un chemin UNC (Universal Naming Convention), par exemple \\\nom_ordinateur\scripts\mon_script.sql.  
  
    -   Pour spécifier un script à exécuter après l'application de l'instantané, cliquez sur **Parcourir** pour rechercher le script, ou entrez un chemin d'accès conforme à la convention d'affectation de noms (UNC) pour le script dans la zone de texte **Exécuter ce script après l'application de l'instantané** .  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer les propriétés d’instantané &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)   
 [Modifier les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Exécuter des scripts avant et après l’application de l’instantané](../../relational-databases/replication/execute-scripts-before-and-after-the-snapshot-is-applied.md)   
 [Initialiser un abonnement avec un instantané](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
  
  
