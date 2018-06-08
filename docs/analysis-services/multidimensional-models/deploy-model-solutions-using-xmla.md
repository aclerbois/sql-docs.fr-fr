---
title: Déployer des Solutions de modèle à l’aide de XMLA | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5ce0f50d158d715ddbbc689c47a012a4e92ba7c4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-model-solutions-using-xmla"></a>Déployer des solutions de modèle à l'aide de XMLA
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l’option **CREATE To** de la commande **Générer un script de la base de données en tant que** crée un script XML d’une base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] entière ou de l’un de ses objets. Le script résultant peut être exécuté sur un autre ordinateur pour recréer le schéma (métadonnées) de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Le script génère l'ensemble de la base de données, et il n'existe aucun mécanisme pour mettre à jour de manière incrémentielle les objets déjà déployés lors de l'utilisation du script. Après l'exécution du script et le déploiement de la base de données, la nouvelle base de données doit être traitée pour que les utilisateurs puissent l'utiliser.  
  
 Pour plus d’informations sur la commande **Générer un script de la base de données en tant que** , consultez [Documenter et générer des scripts pour une base de données Analysis Services](../../analysis-services/multidimensional-models/document-and-script-an-analysis-services-database.md).  
  
## <a name="modifying-object-properties-in-the-xml-script"></a>Modification des propriétés des objets dans le script XML  
 Quand vous utilisez la commande **Générer un script de la base de données en tant que** , vous ne pouvez pas modifier des propriétés spécifiques (telles que le nom de la base de données, les chaînes de connexion à la source de données et les paramètres de sécurité) des objets de la base de données. Ces propriétés doivent être modifiées manuellement dans le script après sa génération, ou dans la base de données déployée après l'exécution du script.  
  
> [!IMPORTANT]  
>  Le script XML ne contient pas le mot de passe si celui-ci est spécifié dans la chaîne de connexion d'une source de données ou à des fins d'emprunt d'identité. Dans la mesure où le mot de passe est nécessaire à des fins de traitement dans ce scénario, vous devez l'ajouter manuellement au script XML avant son exécution ou après l'exécution du script XML.  
  
## <a name="see-also"></a>Voir aussi  
 [Déployer des Solutions de modèle à l’aide de l’Assistant de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md)   
 [Synchroniser les bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)  
  
  
