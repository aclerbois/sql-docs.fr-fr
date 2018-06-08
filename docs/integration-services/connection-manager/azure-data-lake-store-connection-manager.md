---
title: Gestionnaire de connexions Azure Data Lake Store | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSCM.F1
- sql14.dts.designer.afpadlscm.f1
ms.assetid: f4c44553-0f08-4731-ac47-7534990b8c8d
caps.latest.revision: 7
author: Lingxi-Li
ms.author: lingxl
manager: craigg
ms.openlocfilehash: 44cb070aacc75e93509cc2e30addfa1917c62ae1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="azure-data-lake-store-connection-manager"></a>Gestionnaire de connexions Azure Data Lake Store
Un package SQL Server Integration Services (SSIS) peut utiliser le gestionnaire de connexions Azure Data Lake Store pour se connecter à un service Azure Data Lake Store avec l’un des deux types d’authentification suivants :
-   Identité de l’utilisateur Azure AD
-   Identité du service Azure AD 

Le gestionnaire de connexions Azure Data Lake Store est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

>   [!NOTE]
> Pour que le gestionnaire de connexions Azure Data Lake Store et les composants qui l’utilisent, notamment la source Azure Data Lake Store et la destination Azure Data Lake Store, puissent se connecter aux services, veillez à télécharger la dernière version du Feature Pack Azure [ici](https://www.microsoft.com/download/details.aspx?id=49492). 
 
## <a name="configure-the-azure-data-lake-store-connection-manager"></a>Configurer le gestionnaire de connexions Azure Data Lake Store

1.  Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS**, sélectionnez **AzureDataLake**, puis **Ajouter**. La boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Store** s’affiche.
  
2.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Store**, dans le champ **Hôte ADLS**, indiquez l’URL de l’hôte Azure Data Lake Store. Par exemple, `https://test.azuredatalakestore.net` ou `test.azuredatalakestore.net`.
  
3.  Dans le champ **Authentification**, choisissez le type d’authentification approprié pour accéder aux données dans Azure Data Lake Store.

    1.  Si vous sélectionnez l’option d’authentification **Identité de l’utilisateur Azure AD**, effectuez les étapes suivantes :
        1. Renseignez les champs **Nom d’utilisateur** et **Mot de passe**. 
    
        2. Sélectionnez **Tester la connexion** pour tester la connexion. Si l’administrateur client ou vous-même n’avez pas préalablement autorisé SSIS à accéder à vos données Azure Data Lake Store, sélectionnez **Accepter** lorsque vous y êtes invité. Pour plus d’informations sur cette procédure de consentement, consultez [Intégration d’applications dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications#updating-an-application).
    
        >   [!NOTE] 
        > Lorsque vous sélectionnez l’option d’authentification **Identité de l’utilisateur Azure AD**, l’authentification multifacteur et l’authentification de compte Microsoft ne sont pas prises en charge.
    
    2. Si vous sélectionnez l’option d’authentification **Identité du service Azure AD**, effectuez les étapes suivantes :
        1. Créez une application et un principal du service Azure Active Directory (AAD) pour accéder aux données Azure Data Lake.
    
        2. Affectez les autorisations appropriées pour permettre à cette application AAD d’accéder à vos ressources Azure Data Lake. Pour plus d’informations sur cette option d’authentification, consultez [Utiliser le portail pour créer une application Active Directory et un principal du service qui peuvent accéder aux ressources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).
    
        3. Renseignez les champs **ID client**, **Clé secrète** et **Nom du locataire**.
    
        4. Sélectionnez **Tester la connexion** pour tester la connexion.  
  
6.  Sélectionnez **OK** pour fermer la boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Store**.  

## <a name="view-the-properties-of-the-connection-manager"></a>Afficher les propriétés du gestionnaire de connexions
Les propriétés du gestionnaire de connexions que vous avez créées apparaissent dans la fenêtre **Propriétés** .  
  
  
