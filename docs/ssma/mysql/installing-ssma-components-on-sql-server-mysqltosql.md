---
title: Installation des composants SSMA sur SQL Server (MySQLToSql) | Documents Microsoft
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
- SSMA extension pack, Installation
ms.assetid: 6772d0c5-258f-4d7b-afb0-b5f810e71af1
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: c9bdf41c278fe5660c645eab72498cbbee540508
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-ssma-components-on-sql-server-mysqltosql"></a>Installation des composants SSMA sur SQL Server (MySQLToSql)
Outre l’installation SSMA, vous devez également installer les composants sur l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Ces composants incluent le Pack d’extension SSMA, qui prend en charge la migration des données et les fournisseurs de MySQL pour permettre la connectivité du serveur à serveur.  
  
## <a name="ssma-for-mysql-extension-pack"></a>SSMA pour le Pack d’Extension MySQL  
Le Pack d’extension SSMA ajoute une base de données, **sysdb**, à l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Cette base de données contient les tables et les procédures stockées qui sont requises pour migrer des données.  
  
En outre, lorsque vous migrez des données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], SSMA crée [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des travaux de l’Agent, lorsque le moteur de migration de données côté serveur est utilisé pour migrer les données.  
  
### <a name="prerequisites"></a>Configuration requise  
Avant d’installer SSMA pour les composants de serveur MySQL sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], assurez-vous que l’ordinateur répond aux exigences suivantes :  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 ou une version ultérieure.  
  
-   Le fournisseur de Client de MySQL et la connectivité à la base de données MySQL que vous souhaitez migrer. Vous pouvez installer des fournisseurs à partir du support produit de MySQL ou d’un site Web de MySQL.  
  
-   Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] service Browser doit être en cours d’exécution pendant l’installation. Cela permet de remplir une liste des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans l’Assistant installation. Vous pouvez désactiver la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] service Browser après l’installation.  
  
    > [!NOTE]  
    > Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] service Browser est en cours d’exécution, mais vous ne voyez toujours pas une liste d’instances dans le programme d’installation, vous devez débloquer le port UDP 1434. Vous pouvez utiliser le pare-feu Windows pour temporairement débloquer le port, ou vous pouvez désactiver temporairement le pare-feu Windows. Vous devrez peut-être également désactiver temporairement le logiciel antiviru. Veillez à activer le pare-feu et le logiciel antivirus après l’installation.  
  
### <a name="installing-the-extension-pack"></a>L’installation du Pack d’Extension  
Vous pouvez installer le Pack d’extension à tout moment avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
> [!IMPORTANT]  
> Pour installer le Pack d’extension, vous devez être un membre de la **sysadmin** sur l’instance de rôle de serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
**Pour installer le Pack d’extension**  
  
1.  Copiez SSMA pour le Pack d’Extension MySQL. *n*. Install.exe, où *n* est le numéro de build, à l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
2.  Double-cliquez sur SSMA pour le Pack d’Extension MySQL. *n*. Install.exe.  
  
3.  Dans la boîte de dialogue de bienvenue, cliquez sur **suivant**.  
  
4.  Dans la boîte de dialogue contrat de licence utilisateur final, lisez le contrat de licence. Si vous acceptez, cochez la **J’accepte les termes du contrat de licence** case à cocher, puis cliquez sur **suivant**.  
  
5.  Dans la boîte de dialogue Choisir un Type d’installation, cliquez sur **standard**.  
  
6.  Dans prêt pour l’installation, cliquez sur **installer**.  
  
7.  Dans le terminé, la boîte de dialogue de première étape de l’Installation, cliquez sur **suivant**.  
  
    Une boîte de dialogue s’affiche, dans laquelle vous sélectionnez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour l’installation de pack d’extension.  
  
8.  Sélectionnez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] où vous sera être migration des schémas de MySQL, puis cliquez sur **suivant**.  
  
    L’instance par défaut a le même nom que l’ordinateur. Instances nommées seront suivis par une barre oblique inverse et le nom d’instance.  
  
9. Dans la boîte de dialogue de connexion, sélectionnez la méthode d’authentification, puis sur **suivant**.  
  
    L’authentification Windows permet de tenter de se connecter à l’instance de vos informations d’identification Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Si vous sélectionnez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, vous devez entrer un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] nom d’utilisateur et mot de passe.  
  
10. Dans la boîte de dialogue suivante, sélectionnez **installer d’utilitaires de base de données** *n*, où *n* est le numéro de version, puis cliquez sur **suivant**.  
  
    Le **sysdb** base de données est créée avec les tables et procédures stockées requises pour la migration de données (à l’aide du moteur de migration de données côté serveur) sont créés dans cette base de données.  
  
11. Pour installer les utilitaires vers une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], sélectionnez **Oui**, puis cliquez sur **suivant**. Ou, pour quitter l’Assistant, cliquez sur **non**.  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour MySQL Client &#40;MySQLToSQL&#41;](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
