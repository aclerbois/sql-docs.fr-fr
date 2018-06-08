---
title: Suppression de SSMA pour les composants Oracle (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Uninstalling the Extension Pack
ms.assetid: 8b527a56-4e52-487a-9ac9-2320388e6d7d
caps.latest.revision: 6
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: dc119244850dd7e7925b9f858fe8ea5cbf28ecda
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="removing-ssma--for-oracle-components-oracletosql"></a>Suppression de SSMA pour les composants Oracle (OracleToSQL)
Lorsque vous avez terminé bases de données de migration d’Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous souhaiterez peut-être désinstaller les composants SSMA. Vous pouvez désinstaller les composants du client à tout moment. Toutefois, vous ne devez pas désinstaller le pack d’extension à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , sauf si vos bases de données migrés ne plus utilisent les fonctions dans le **ssma_oracle** schéma de la **sysdb** base de données.  
  
## <a name="uninstalling-the-ssma-for-oracle-client"></a>Désinstallation de SSMA pour Client Oracle  
Vous pouvez désinstaller SSMA à l’aide de **Ajout / Suppression de programmes**.  
  
**Pour désinstaller SSMA**  
  
1.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez  **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant pour Oracle**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller SSMA, cliquez sur **Oui**.  
  
## <a name="uninstalling-the-extension-pack"></a>Désinstallation du Pack d’Extension  
Si vous êtes sûr de vos bases de données migrées n’utilisent pas les objets dans le **sysdb.ssma_oracle** schéma, vous pouvez supprimer le Pack d’extension à l’aide de **Ajout / Suppression de programmes**.  
  
**Pour désinstaller le Pack d’extension**  
  
1.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez **Microsoft SQL Server Migration Assistant pour Oracle Extension Pack**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller le Pack d’extension, cliquez sur **Oui**.  
  
4.  Dans les Instances de la page des utilitaires des Scripts de base de données, sélectionnez une instance, puis sur **suivant**.  
  
5.  Dans la page Paramètres de connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.  
  
    L’authentification Windows permet de tenter de se connecter à l’instance de vos informations d’identification Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Si vous sélectionnez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, vous devez entrer un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] nom d’utilisateur et mot de passe.  
  
6.  Dans la page Fin de l’opération, cliquez sur **OK**.  
  
7.  Dans la page Terminer, cliquez sur **Exit**.  
  
Après la désinstallation, vous pouvez vérifier que les objets dans le **sysdb.ssma_oracle** schéma, voire la totalité **sysdb** de base de données, a été supprimé à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)]. Toutefois, si vous utilisez d’autres produits SSMA, ils également utilisent le **sysdb** base de données. Si la base de données existe et que vous êtes sûr qu’aucune autre base de données ne référence des objets dans cette base de données, vous pouvez détacher la base de données.  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour Client Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-client-oracletosql.md)  
[Installation des composants SSMA sur SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)  
  
