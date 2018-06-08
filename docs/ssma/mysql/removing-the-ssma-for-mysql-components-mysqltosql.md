---
title: Suppression de SSMA pour les composants de MySQL (MySQLToSql) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- Uninstalling, Extension pack
- Uninstalling, SSMA for MySQL client
ms.assetid: 87cdbd49-a0c9-4b00-8a93-34188b18d11a
caps.latest.revision: 10
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 6cfa6f4638beca9f41f7bbd1646a64a4d294212b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="removing-the-ssma-for-mysql-components-mysqltosql"></a>Suppression de SSMA pour les composants de MySQL (MySQLToSql)
Lorsque vous avez terminé bases de données de migration de MySQL vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous souhaiterez peut-être désinstaller les composants SSMA. Vous pouvez désinstaller les composants du client à tout moment. Toutefois, si vous désinstallez le pack d’extension à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , puis, SSMA n’est plus prise en charge la migration des données de MySQL vers la base de données cible (SQL Server/SQL Azure) en utilisant le moteur de Migration de données côté serveur.  
  
## <a name="uninstalling-the-ssma-for-mysql-client"></a>Désinstallation de SSMA pour MySQL Client  
Vous pouvez désinstaller SSMA à l’aide de **Ajout / Suppression de programmes**.  
  
**Pour désinstaller SSMA**  
  
1.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez **Microsoft SQL Server Migration Assistant pour MySQL**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller SSMA, cliquez sur **Oui**.  
  
## <a name="uninstalling-the-extension-pack"></a>Désinstallation du Pack d’Extension  
Vous pouvez supprimer le Pack d’extension à l’aide de **Ajout / Suppression de programmes**.  
  
**Pour désinstaller le Pack d’extension**  
  
1.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez **Microsoft SQL Server Migration Assistant pour MySQL Extension Pack**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller le Pack d’extension, cliquez sur **Oui**.  
  
4.  Dans les Instances de la page des utilitaires des Scripts de base de données, sélectionnez une instance, puis sur **suivant**.  
  
5.  Dans la page Paramètres de connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.  
  
    L’authentification Windows permet de tenter de se connecter à l’instance de vos informations d’identification Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Si vous sélectionnez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, vous devez entrer un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] nom d’utilisateur et mot de passe.  
  
6.  Dans la page Fin de l’opération, cliquez sur **OK**.  
  
7.  Dans la page Terminer, cliquez sur **Exit**.  
  
Une fois le processus de désinstallation est terminé, vous pouvez vérifier que les objets dans le **sysdb.ssma_MySQL** schéma, voire la totalité **sysdb** de base de données, a été supprimé à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)]. Toutefois, si vous utilisez d’autres produits SSMA, ils également utilisent le **sysdb** base de données. Si la base de données existe et que vous êtes sûr qu’aucune autre base de données font référence aux objets de cette base de données, vous pouvez détacher la base de données.  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour MySQL Client &#40;MySQLToSQL&#41;](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)  
[Installation des composants SSMA sur SQL Server](http://msdn.microsoft.com/en-us/6772d0c5-258f-4d7b-afb0-b5f810e71af1)  
  
