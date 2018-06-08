---
title: Se connecter à la base de données de catalogues SSIS dans Azure | Microsoft Docs
ms.date: 09/25/2017
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
ms.openlocfilehash: ef69dc52b041541b89052038cadad94b9fa2f45c
ms.sourcegitcommit: b5ab9f3a55800b0ccd7e16997f4cd6184b4995f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="connect-to-the-ssis-catalog-database-ssisdb-in-azure"></a>Se connecter à la base de données de catalogues SSIS (SSISDB) dans Azure

Obtenez les informations de connexion dont vous avez besoin pour vous connecter à la base de données de catalogues SSISDB hébergée sur un serveur Azure SQL Database. Vous avez besoin des éléments suivants pour vous connecter :
- nom complet du serveur
- nom de la base de données
- informations de connexion 

> [!IMPORTANT]
> Vous ne pouvez pas créer la base de données de catalogues SSISDB sur Azure SQL Database à ce stade indépendamment de la création du runtime d’intégration Azure-SSIS dans Azure Data Factory version 2. C’est le runtime d’intégration Azure-SSIS qui exécute les packages SSIS sur Azure. Pour plus d’informations, consultez [Déployer des packages SSIS sur Azure](https://docs.microsoft.com/azure/data-factory/tutorial-create-azure-ssis-runtime-portal). 

## <a name="prerequisites"></a>Conditions préalables requises
Avant de commencer, veillez à disposer de la version 17.2 ou ultérieure de SQL Server Management Studio (SSMS). Si la base de données de catalogue SSISDB est hébergée sur SQL Database Managed Instance (préversion), assurez-vous d’avoir la version 17.6 ou ultérieure de SSMS. Pour télécharger la dernière version de SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

## <a name="get-the-connection-info-from-the-azure-portal"></a>Obtenir les informations de connexion à partir du portail Azure
1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Dans le portail Azure, sélectionnez **Bases de données SQL** dans le menu de gauche, puis sélectionnez la base de données `SSISDB` dans la page **Bases de données SQL**. 
3. Dans la page **Vue d’ensemble** de la base de données `SSISDB`, vérifiez le nom complet du serveur comme indiqué dans l’image suivante. Pointez sur le nom du serveur pour afficher l’option **Cliquer pour copier**.

    ![Informations de connexion au serveur](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. Si vous avez oublié les informations de connexion du serveur SQL Database, accédez à la page du serveur SQL Database. Vous pourrez y trouver le nom de l’administrateur du serveur et, si nécessaire, réinitialiser le mot de passe.

## <a name="connect-with-ssms"></a>Se connecter à SSMS
1. Ouvrez SQL Server Management Studio.

2. **Connectez-vous au serveur**. Dans la boîte de dialogue **Se connecter au serveur**, entrez les informations suivantes :

   | Paramètre       | Valeur suggérée | Description | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Type de serveur** | Moteur de base de données | Cette valeur est requise. |
   | **Nom du serveur** | Nom complet du serveur | Le nom doit être au format suivant : **mysqldbserver.database.windows.net**. |
   | **Authentification** | Authentification SQL Server | |
   | **Connexion** | Compte Administrateur du serveur | Il s’agit du compte que vous avez spécifié quand vous avez créé le serveur. |
   | **Mot de passe** | Mot de passe du compte Administrateur de votre serveur | Il s’agit du mot de passe que vous avez spécifié quand vous avez créé le serveur. |

    ![Se connecter au serveur avec SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **Connectez-vous à la base de données SSISDB**. Sélectionnez **Options** pour développer la boîte de dialogue **Se connecter au serveur**. Dans la boîte de dialogue **Se connecter au serveur** développée, sélectionnez l’onglet **Propriétés de connexions**. Dans le champ **Se connecter à la base de données**, sélectionnez ou entrez `SSISDB`.

    > [!IMPORTANT]
    > Si vous ne sélectionnez pas `SSISDB` quand vous vous connectez, le catalogue SSIS risque de ne pas s’afficher dans l’Explorateur d’objets.

    ![Sélectionner la base de données SSISDB pour la connexion](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. Sélectionnez ensuite **Se connecter**.

5. Dans l’Explorateur d’objets, développez **Catalogues Integration Services**, puis développez **SSISDB** pour afficher les objets de la base de données de catalogues SSIS.

    ![Rechercher la base de données SSISDB dans l’Explorateur d’objets dans SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>Étapes suivantes
- Déployez un package. Pour plus d’informations, consultez [Déployer un projet SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-deploy-ssms.md).
- Exécutez un package. Pour plus d’informations, consultez [Exécuter un package SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).
- Planifiez un package. Pour plus d’informations, consultez [Planifier l’exécution d’un package SSIS sur Azure](ssis-azure-schedule-packages.md)
