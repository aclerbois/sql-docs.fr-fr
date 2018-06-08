---
title: Grant autorisations Lire la définition des métadonnées d’objet (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a30da6aa162cf729dd5e829b8baa2528ce80a39a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="grant-read-definition-permissions-on-object-metadata-analysis-services"></a>Octroyer des autorisations Lire la définition sur des métadonnées d'objets (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'autorisation de lire une définition d'objets ou des métadonnées sur des objets sélectionnés permet à un administrateur d'accorder l'autorisation d'afficher des informations sur les objets sans accorder l'autoriser de modifier les définitions des objets, modifier la structure des objets ou afficher les données des objets. Vous pouvez accorder des autorisations**Lire la définition** au niveau de la base de données, d’une source de données, d’une dimension, d’une structure d’exploration de données et d’un modèle d’exploration de données. Si vous avez besoin d’autorisations **Lire la définition** pour un cube, vous devez activer **Lire la définition** pour la base de données. N’oubliez pas que les autorisations s’ajoutent les unes aux autres. Par exemple, imaginez qu'un rôle accorde l'autorisation de lire les métadonnées d'un cube tandis qu'un second rôle accorde au même utilisateur l'autorisation de lire les métadonnées d'une dimension. Les autorisations des deux rôles se combinent et permettent à l'utilisateur de lire les métadonnées du cube et celles de la dimension dans cette base de données.  
  
> [!NOTE]  
>  L'autorisation de lire les métadonnées d'une base de données est l'autorisation minimale nécessaire pour se connecter à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en utilisant [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Un utilisateur autorisé à lire les métadonnées peut également utiliser l’ensemble de lignes de schéma DISCOVER_XML_METADATA pour exécuter une requête sur l’objet et afficher ses métadonnées. Pour plus d’informations, consultez [Ensemble de lignes DISCOVER_XML_METADATA](../../analysis-services/schema-rowsets/xml/discover-xml-metadata-rowset.md).  
  
## <a name="set-read-definition-permissions-on-a-database"></a>Définir des autorisations Lire la définition sur une base de données  
 Le fait d'accorder l'autorisation de lire des métadonnées de base de données accorde également l'autorisation de lire les métadonnées de tous les objets dans la base de données.  
  
 Nous vous suggérons d’inclure l’autorisation **Lire la définition** au niveau de la base de données chaque fois que vous définissez des rôles pour le traitement dédié. Le fait de disposer de l’autorisation **Lire la définition** permet aux non-administrateurs d’afficher la hiérarchie d’objets d’un modèle dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et d’accéder à des objets spécifiques pour un traitement ultérieur.  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance d’[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], développez **Rôles** pour la base de données appropriée dans l’Explorateur d’objets, puis cliquez sur un rôle de base de données (ou créez un rôle de base de données).  
  
2.  Sous l’onglet **Général** , sélectionnez l’option **Lire la définition** .  
  
3.  Dans le volet **Appartenance** , entrez les comptes d'utilisateurs et de groupes Windows qui se connectent à Analysis Services à l'aide de ce rôle.  
  
4.  Cliquez sur **OK** pour terminer la création du rôle.  
  
## <a name="set-read-definition-permissions-on-individual-objects"></a>Définir des autorisations Lire la définition sur des objets spécifiques  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ouvrez le dossier **Bases de données** , sélectionnez une base de données, développez **Rôles** pour la base de données appropriée dans l’Explorateur d’objets, puis cliquez sur un rôle de base de données (ou créez-en un).  
  
2.  Dans le volet **Général** , décochez la case d’autorisation de base de données pour **Lire la définition**. Cette étape permet de supprimer l'héritage des autorisations afin que vous puissiez définir des autorisations sur des objets spécifiques.  
  
3.  Sélectionnez l’objet pour lequel vous spécifiez les propriétés **Lire la définition** :  
  
    -   Dans le volet **Sources de données** , cochez la case **Lire la définition** correspondant à cette source de données. Les membres du rôle peuvent afficher la chaîne de connexion à la source de données, y compris le nom du serveur et éventuellement le nom d'utilisateur. Cette autorisation est disponible pour le cas où vous souhaiteriez fournir des informations sur la chaîne de connexion sans accorder également l'autorisation de modifier la chaîne de connexion ou d'afficher les définitions d'autres objets.  
  
    -   Dans le volet **Dimensions** , cochez la case **Lire la définition** correspondant à cette dimension. Les analystes et développeurs expérimentés devront peut-être afficher la définition sans être autorisé à la modifier ou afficher les définitions d'autres objets (tels que d'autres dimensions, objets de cube ou modèles et structures d'exploration de données).  
  
    -   Dans le volet Structures d’exploration de données, cochez la case **Lire la définition** correspondant aux modèles et structures d’exploration de données. L’autorisation **Lire la définition** est nécessaire pour parcourir le modèle de données. Pour plus d’informations, consultez [Octroyer des autorisations sur des modèles et des structures d’exploration de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-permissions-on-data-mining-structures-and-models-analysis-services.md).  
  
4.  Dans le volet **Appartenance** , entrez les comptes d'utilisateurs et de groupes Windows qui se connectent à Analysis Services à l'aide de ce rôle.  
  
5.  Cliquez sur **OK** pour terminer la création du rôle.  
  
## <a name="see-also"></a>Voir aussi  
 [Accorder des autorisations de base de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md)   
 [Accorder des autorisations de processus & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-process-permissions-analysis-services.md)  
  
  
