---
title: Guide pratique pour préparer SQL Server pour CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a327fa18-58f4-4e69-bb87-44faf47e20ef
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 60ee6bd81452554e5c1e5896380b10eef7dcafac
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-prepare-sql-server-for-cdc"></a>Procédure : préparer SQL Server pour la capture de données modifiées
  Le service de capture de données modifiées Oracle requiert que toutes les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cibles contiennent la base de données MSXDBCDC. Vous créez cette base de données à l'aide de l'action Préparer SQL Server dans la console de configuration du service de capture de données modifiées. Cette tâche n'est effectuée qu'une seule fois pour chaque instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible.  
  
 La section suivante décrit comment préparer une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la capture de données modifiées Oracle à l'aide de la console de configuration du service de capture de données modifiées. Ce processus crée la base de données MSXDBCDC et définit les tables requises, les procédures stockées et autres artefacts nécessaires.  
  
 La préparation de SQL Server pour la capture de données modifiées Oracle est effectuée par l'administrateur de service de capture de données modifiées Oracle. Pour plus d'informations sur le rôle Administrateur de service de capture de données modifiées, consultez [User Roles](../../integration-services/change-data-capture/user-roles.md).  
  
### <a name="to-enable-sql-server-for-cdc"></a>Pour activer SQL Server pour la capture de données modifiées  
  
1.  Dans le menu **Démarrer** , sélectionnez **Configuration du service de capture de données modifiées pour Oracle**.  
  
2.  Dans le volet gauche, sélectionnez **Service sde capture de données modifiées locaux** , puis dans le volet **Actions** , cliquez sur **Préparer SQL Server**.  
  
     Vous pouvez également cliquer avec le bouton droit sur **Services de capture de données modifiées locaux** et sélectionner **Préparer SQL Server**.  
  
3.  Entrez les informations nécessaires dans la boîte de dialogue Préparation d'une instance SQL Server pour la capture de données modifiées Oracle. Pour plus d'informations sur la façon d'entrer les informations nécessaires dans cette boîte de dialogue, consultez [Prepare SQL Server for CDC](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md).  
  
     Pour préparer l'instance SQL Server pour la capture de données modifiées Oracle, la connexion doit avoir l'autorisation d'écriture dans la base de données MSXDBCDC. Entrez les informations d'identification pour une connexion qui a l'autorisation d'écriture dans la base de données MSXDBCDC, telle qu'un membre du rôle `sysasmin` .  
  
 **Remarque**: Vous pouvez cliquer sur **Afficher le script** pour afficher une version en lecture seule du script d’installation. Un administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut copier ce script dans la console de gestion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en vue de le modifier et de l'exécuter, si nécessaire.  
  
## <a name="see-also"></a> Voir aussi  
 [Préparer SQL Server pour la capture de données modifiées](../../integration-services/change-data-capture/prepare-sql-server-for-cdc.md)  
  
  
