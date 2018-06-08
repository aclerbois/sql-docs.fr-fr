---
title: Se connecter à des sources de données et des partages de fichiers avec l’authentification Windows | Microsoft Docs
ms.date: 02/05/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.component: lift-shift
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0a0f1b6936644f2cae9cee469cb763696786a628
ms.sourcegitcommit: 0cc2cb281e467a13a76174e0d9afbdcf4ccddc29
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2018
---
# <a name="connect-to-on-premises-data-sources-and-azure-file-shares-with-windows-authentication-in-ssis"></a>Se connecter à des sources de données locales et à des partages de fichiers Azure avec l’authentification Windows dans SSIS
Cet article explique comment configurer le catalogue SSIS sur Azure SQL Database pour exécuter des packages qui utilisent l’authentification Windows afin de se connecter à des sources de données locales et des partages de fichiers Azure. Vous pouvez utiliser l’authentification Windows pour vous connecter à des sources de données dans le même réseau virtuel qu’Azure SSIS Integration Runtime, aussi bien localement que sur des machines virtuelles Azure et dans Azure Files.

> [!WARNING]
> Si vous ne fournissez pas d’informations d’identification de domaine valides pour l’authentification Windows en exécutant `catalog`.`set_execution_credential`, comme décrit dans cet article, les packages qui dépendent de l’authentification Windows ne peuvent pas se connecter aux sources de données et échouent au moment de l’exécution.

## <a name="you-can-only-use-one-set-of-credentials"></a>Vous ne pouvez utiliser qu’un seul jeu d’informations d’identification

À l’heure actuelle, vous ne pouvez utiliser qu’un seul jeu d’informations d’identification dans un package. Les informations d’identification de domaine que vous fournissez quand vous suivez les étapes de cet article s’appliquent à toutes les exécutions de package, interactives ou planifiées, sur l’instance SQL Database, et ce jusqu’à ce que vous changiez ou supprimiez les informations d’identification. Si votre package doit se connecter à plusieurs sources de données avec différents jeux d’informations d’identification, vous devrez peut-être séparer le package en plusieurs packages.

Si l’une de vos sources de données est Azure Files, vous pouvez contourner cette limitation en montant le partage de fichiers Azure au moment de l’exécution du package avec `net use` ou l’équivalent dans une tâche Exécuter le processus. Pour plus d’informations, consultez [Montage d’un partage de fichiers Azure et accès au partage dans Windows](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows).

## <a name="provide-domain-credentials-for-windows-authentication"></a>Fournir des informations d’identification de domaine pour l’authentification Windows
Pour fournir des informations d’identification de domaine qui permettent aux packages d’utiliser l’authentification Windows afin de se connecter à des sources de données locales, effectuez les actions suivantes :

1.  Avec SQL Server Management Studio (SSMS) ou un autre outil, connectez-vous à l’instance SQL Database qui héberge la base de données de catalogues SSIS (SSISDB). Pour plus d’informations, consultez [Se connecter à la base de données de catalogues SSISDB sur Azure](ssis-azure-connect-to-catalog-database.md).

2.  La base de données SSISDB étant la base de données active, ouvrez une fenêtre de requête.

3.  Exécutez la procédure stockée suivante et fournissez les informations d’identification de domaine appropriées :

    ```sql
    catalog.set_execution_credential @user='<your user name>', @domain='<your domain name>', @password='<your password>'
    ```

4.  Exécutez vos packages SSIS. Les packages utilisent les informations d’identification que vous avez fournies pour se connecter aux sources de données locales via l’authentification Windows.

### <a name="view-domain-credentials"></a>Afficher les informations d’identification de domaine
Pour afficher les informations d’identification de domaine actives, effectuez les actions suivantes :

1.  Avec SQL Server Management Studio (SSMS) ou un autre outil, connectez-vous à l’instance SQL Database qui héberge la base de données de catalogues SSIS (SSISDB).

2.  La base de données SSISDB étant la base de données active, ouvrez une fenêtre de requête.

3.  Exécutez la procédure stockée suivante et vérifiez la sortie :

    ```sql
    SELECT * 
    FROM catalog.master_properties
    WHERE property_name = 'EXECUTION_DOMAIN' OR property_name = 'EXECUTION_USER'
    ```

### <a name="clear-domain-credentials"></a>Effacer les informations d’identification de domaine
Pour effacer et supprimer les informations d’identification que vous avez fournies, comme indiqué dans cet article, effectuez les actions suivantes :

1.  Avec SQL Server Management Studio (SSMS) ou un autre outil, connectez-vous à l’instance SQL Database qui héberge la base de données de catalogues SSIS (SSISDB).

2.  La base de données SSISDB étant la base de données active, ouvrez une fenêtre de requête.

3.  Exécutez la procédure stockée suivante :

    ```sql
    catalog.set_execution_credential @user='', @domain='', @password=''
    ```

## <a name="connect-to-an-on-premises-sql-server"></a>Se connecter à un serveur SQL Server local
Pour vérifier si vous pouvez vous connecter à une instance SQL Server locale, effectuez les actions suivantes :

1.  Pour exécuter ce test, recherchez un ordinateur non membre d’un domaine.

2.  Sur l’ordinateur non membre d’un domaine, exécutez la commande suivante pour démarrer SQL Server Management Studio (SSMS) avec les informations d’identification de domaine à utiliser :

    ```cmd
    runas.exe /netonly /user:<domain>\<username> SSMS.exe
    ```

3.  À partir de SSMS, vérifiez si vous pouvez vous connecter à l’instance SQL Server locale à utiliser.

### <a name="prerequisites"></a>Conditions préalables requises
Pour vous connecter à un serveur SQL Server local à partir d’un package s’exécutant sur Azure, vous devez satisfaire aux prérequis suivants :

1.  Dans le Gestionnaire de Configuration SQL Server, activez le protocole TCP/IP.
2.  Autorisez l’accès à travers le pare-feu Windows. Pour plus d’informations, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](https://docs.microsoft.com/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access).
3.  Pour vous connecter avec l’authentification Windows, vérifiez qu’Azure SSIS Integration Runtime appartient à un réseau virtuel qui inclut également le serveur SQL Server local.  Pour plus d’informations, consultez [Joindre un runtime d’intégration Azure SSIS à un réseau virtuel](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Utilisez ensuite `catalog.set_execution_credential` pour fournir des informations d’identification comme décrit dans cet article.

## <a name="connect-to-an-on-premises-file-share"></a>Se connecter à un partage de fichiers local
Pour vérifier si vous pouvez vous connecter à un partage de fichiers local, effectuez les actions suivantes :

1.  Pour exécuter ce test, recherchez un ordinateur non membre d’un domaine.

2.  Sur l’ordinateur non membre d’un domaine, exécutez la commande suivante. Cette commande ouvre une fenêtre d’invite de commandes avec les informations d’identification de domaine à utiliser. Elle teste ensuite la connectivité au partage de fichiers en obtenant une liste de répertoires.

    ```cmd
    runas.exe /netonly /user:<domain>\<username> cmd.exe
    dir \\fileshare
    ```

3.  Vérifiez si la liste de répertoires est retournée pour le partage de fichiers local à utiliser.

## <a name="connect-to-a-file-share-on-an-azure-vm"></a>Se connecter à un partage de fichiers sur une machine virtuelle Azure
Pour vous connecter à un partage de fichiers sur une machine virtuelle Azure, procédez comme suit :

1.  Avec SQL Server Management Studio (SSMS) ou un autre outil, connectez-vous à l’instance SQL Database qui héberge la base de données de catalogues SSIS (SSISDB).

2.  La base de données SSISDB étant la base de données active, ouvrez une fenêtre de requête.

3.  Exécutez la procédure stockée `catalog.set_execution_credential` comme indiqué dans les options suivantes :

    ```sql
    catalog.set_execution_credential @domain = N'.', @user = N'username of local account on Azure virtual machine', @password = N'password'
    ```

## <a name="connect-to-a-file-share-in-azure-files"></a>Se connecter à un partage de fichiers dans Azure Files
Pour plus d’informations sur Azure Files, consultez [Azure Files](https://azure.microsoft.com/services/storage/files/).

Pour vous connecter à un partage de fichiers sur un partage de fichiers Azure, procédez comme suit :

1.  Avec SQL Server Management Studio (SSMS) ou un autre outil, connectez-vous à l’instance SQL Database qui héberge la base de données de catalogues SSIS (SSISDB).

2.  La base de données SSISDB étant la base de données active, ouvrez une fenêtre de requête.

3.  Exécutez la procédure stockée `catalog.set_execution_credential` comme indiqué dans les options suivantes :

    ```sql
    catalog.set_execution_credential @domain = N'Azure', @user = N'<storage-account-name>', @password = N'<storage-account-key>'
    ```

## <a name="next-steps"></a>Étapes suivantes
- Déployez un package. Pour plus d’informations, consultez [Déployer un projet SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-deploy-ssms.md).
- Exécutez un package. Pour plus d’informations, consultez [Exécuter un package SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).
- Planifiez un package. Pour plus d’informations, consultez [Planifier l’exécution d’un package SSIS sur Azure](ssis-azure-schedule-packages.md).
