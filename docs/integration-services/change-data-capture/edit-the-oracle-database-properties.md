---
title: Modifier les propriétés d’une base de données Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- oraProp
ms.assetid: 58dc99f1-ee6b-4508-bb66-2bc589611ff7
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9cb0b4b48446ef113ceffac80c306e6b8098b5f4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="edit-the-oracle-database-properties"></a>Modifier les propriétés d'une base de données Oracle
  Utilisez l'onglet Oracle de l'éditeur de propriétés pour apporter des modifications à la description que vous avez fournie dans la page Créer une base de données CDC de l'Assistant Nouvelle instance et pour apporter des modifications aux informations de connexion à la base de données d'exploration de données de journaux Oracle.  
  
 La section suivante décrit les informations présentes dans l'onglet **Oracle** .  
  
 **Nom**  
 Nom de l'instance de capture de données modifiées entré dans la page Créer une base de données CDC de l'Assistant Nouvelle instance. Ce champ est en lecture seule et vous ne pouvez pas modifier ces informations.  
  
 **Description**  
 Vous pouvez modifier la description de la nouvelle instance ou en ajouter une si vous ne l'avez pas fait lors de la création de l'instance de capture de données modifiées.  
  
 **Chaîne de connexion Oracle**  
 Chaîne de connexion Oracle à l'ordinateur sur lequel est installée la base de données Oracle que vous utilisez. Ce champ est en lecture seule et vous ne pouvez pas modifier ces informations. Cela est dû au fait que certaines modifications apportées à la chaîne de connexion peuvent faire pointer l’instance Oracle CDC vers une autre base de données Oracle, ce qui peut altérer l’état de l’instance de capture de données modifiées stocké dans la table **cdc.xdbcdc_config** . Si des modifications mineures sont nécessaires, vous pouvez modifier la chaîne de connexion directement dans la table de configuration à l'aide de SQL Server Management Studio.  
  
 **Authentification pour l'exploration de données de journaux Oracle**  
 Pour entrer les informations d’identification pour la base de données Oracle qui contient l’outil Log Miner, sélectionnez une des options suivantes sous **Authentification**:  
  
-   **Authentification Windows**: sélectionnez cette option pour utiliser les informations d'identification actuelles de domaine Windows. Vous ne pouvez utiliser cette option que si la base de données Oracle est configurée pour utiliser l'authentification Windows.  
  
-   **Authentification Oracle**: si vous sélectionnez cette option, vous devez taper le **Nom d'utilisateur** et le **Mot de passe** de l'utilisateur dans la base de données Oracle à laquelle vous êtes connecté.  
  
 Vous pouvez afficher les propriétés de base de données Oracle dans la visionneuse. Lorsque vous utilisez la visionneuse, les informations sont en lecture seule. La visionneuse inclut également la liste des colonnes capturées dans la table. Pour plus d'informations sur l'accès à la visionneuse, consultez [How to Manage a CDC Instance](../../integration-services/change-data-capture/how-to-manage-a-cdc-instance.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Procédure : gérer un service de capture de données modifiées à partir de la console du concepteur CDC](../../integration-services/change-data-capture/how-to-manage-a-cdc-service-from-the-cdc-designer-console.md)   
 [Se connecter à une base de données source Oracle](../../integration-services/change-data-capture/connect-to-an-oracle-source-database.md)   
 [Se connecter à Oracle](../../integration-services/change-data-capture/connect-to-oracle.md)  
  
  
