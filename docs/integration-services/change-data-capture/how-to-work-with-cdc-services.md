---
title: Guide pratique pour utiliser des services CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: db5c718a-6e7f-48ec-82a3-9d5b131716e5
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0a8bf44c84b25c1633914fb2516446e4136b7133
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32873144"
---
# <a name="how-to-work-with-cdc-services"></a>Procédure : utiliser des services de capture de données modifiées
  Cette procédure décrit comment utiliser la console de configuration du service de capture de données modifiées pour préparer une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en vue de l'utilisation des services de capture de données modifiées Oracle et de la création d'un service de capture de données modifiées.  
  
### <a name="to-work-with-cdc-services"></a>Pour utiliser des services de capture de données modifiées  
  
1.  Dans le menu **Démarrer** , sélectionnez **Configuration du service de capture de données modifiées pour Oracle**.  
  
2.  Dans le volet gauche, sélectionnez **Services de capture de données modifiées locaux** (au niveau de la racine).  
  
3.  Vous effectuez l'une des deux tâches suivantes ou les deux :  
  
    -   **Préparer SQL Server**  
  
         Sélectionnez cette option dans le volet **Actions** à droite de la console de configuration du service de capture de données modifiées.  
  
         Vous pouvez également cliquer avec le bouton droit sur **Services de capture de données modifiées locaux** et sélectionner **Préparer SQL Server**.  
  
         La boîte de dialogue Préparation d'une l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la capture de données modifiées Oracle s'ouvre.  
  
         Pour préparer l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les services de capture de données modifiées Oracle, la connexion doit avoir une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le rôle serveur fixe `dbcreator` . Cette connexion est utilisée pour créer la base de données MSXDBCDC requise pour ajouter des services de capture de données modifiées Oracle et, ultérieurement, des instances Oracle CDC.  
  
         Pour plus d'informations sur l'utilisation de cette boîte de dialogue, consultez [Prepare SQL Server for CDC](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md). Pour plus d’informations sur l’activation d’une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la capture de données modifiées, consultez [Procédure : préparer SQL Server pour la capture de données modifiées](../../integration-services/change-data-capture/how-to-prepare-sql-server-for-cdc.md).  
  
    -   **Créer un service de capture de données modifiées**  
  
         Cliquez sur **Nouveau service** dans le volet **Actions** à droite de la console de configuration du service de capture de données modifiées.  
  
         Vous pouvez également cliquer avec le bouton droit sur **Services de capture de données modifiées locaux** et sélectionner **Nouveau service**.  
  
         La boîte de dialogue Nouveau service de capture de données modifiées Oracle s'ouvre.  
  
         Pour plus d'informations sur l'utilisation de cette boîte de dialogue, consultez [Créer et modifier un service de capture de données modifiées Oracle](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md). Pour plus d'informations sur la façon de créer ou modifier un service de capture de données modifiées, consultez [Procédure : créer et modifier un service de capture de données modifiées](../../integration-services/change-data-capture/how-to-create-and-edit-a-cdc-service.md).  
  
         La connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée par le service de capture de données modifiées Oracle doit être membre du rôle serveur fixe `public` ; aucun autre privilège n'est nécessaire. Toutefois, pour créer le service de capture de données modifiées Oracle, la connexion doit avoir l’autorisation d’écriture dans la base de données MSXDBCDC, par exemple le rôle de base de données **db_owner** doit être assigné à la connexion. Lorsqu'une connexion sans autorisation d'écriture sur la base de données MSXDBDCDC tente de créer une instance Oracle CDC un message d'erreur s'affiche. Cliquez sur **OK** dans cette boîte de dialogue pour afficher la boîte de dialogue Connexion à SQL Server.  
  
         Pour plus d’informations sur la façon d’entrer des informations d’identification pour une connexion ayant l’autorisation d’écriture dans la base de données MSXDBCDC, tel le rôle de base de données **db_owner** , consultez [Créer et modifier un service de capture de données modifiées Oracle](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md) et [Connexion à SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser des services de capture de données modifiées](../../integration-services/change-data-capture/work-with-cdc-services.md)  
  
  
