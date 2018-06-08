---
title: Utilisez l’Assistant génération de schéma (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a9f60b324a706d28145fb9e843957b20da5ed90a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="use-the-schema-generation-wizard-analysis-services"></a>Utiliser l'Assistant Génération de schéma (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'Assistant Génération de schéma requiert un volume d'informations restreint durant la phase de génération. La plupart des informations dont l’Assistant Génération de schéma a besoin pour générer des schémas relationnels sont extraites depuis les cubes et dimensions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que vous avez déjà créés dans le projet. En outre, vous pouvez personnaliser la façon dont le schéma de la base de données de zone de sujet est généré et comment les objets du schéma sont nommés.  
  
## <a name="start-the-wizard"></a>Démarrer l'Assistant  
 Vous pouvez ouvrir l'Assistant Génération de schéma à partir de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] de plusieurs façons :  
  
-   Cliquez avec le bouton droit sur l’objet de projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis sélectionnez **Générer le schéma relationnel** dans le menu contextuel.  
  
-   Cliquez sur l’objet de projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis sur **Générer le schéma relationnel** dans le menu **Base de données** .  
  
-   Démarrez l’Assistant depuis l’Assistant Dimension en cliquant sur la case à cocher **Créer le schéma maintenant** dans la dernière page de l’Assistant.  
  
## <a name="step-1-specify-targets"></a>Étape 1 : spécifier des cibles  
 Vous devez spécifier la vue de source de données (DSV) dans laquelle l'Assistant Génération de schéma génère le schéma de la base de données de la zone de sujet. Bien que vous puissiez sélectionner une vue DSV existante, en général vous créez une nouvelle vue basée sur une source de données. Vous pouvez créer la source de données à partir d'une connexion existante ou d'une nouvelle connexion, ou bien à partir d'un autre objet. L'Assistant Génération de schéma génère le schéma pour la base de données de la zone de sujet dans la base de données référencée par la source de données, ainsi que dans la vue de la source de données. L'Assistant Génération de schéma ne crée pas la base de données de la zone de sujet lui-même ; il crée le schéma relationnel pour prendre en charge les cubes et les dimensions dans la base de données existante que vous spécifiez.  
  
 Quand l’Assistant Génération de schéma génère les objets sous-jacents, il lie les dimensions et les cubes [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] aux tables et colonnes générées à l’aide des liaisons de style d’affichage de source de données.  
  
> [!NOTE]  
>  Pour séparer les cubes et les dimensions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] des objets préalablement générés, supprimez la vue de source de données à laquelle les cubes et les dimensions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sont liés, puis définissez une nouvelle vue de source de données pour les cubes et les dimensions à l'aide de l'Assistant Génération de schéma.  
  
## <a name="step-3-specify-schema-options-for-the-subject-area-database"></a>Étape 3 : spécifier des options de schéma pour la base de données de la zone de sujet  
 L'Assistant Génération de schéma fournit des options pour définir le schéma qui est généré pour la base de données de la zone de sujet. Vous pouvez spécifier ces options dans la page **Options du schéma de la BdD de la zone de sujet** de l’Assistant.  
  
### <a name="specifying-the-schema-owner"></a>Spécification du propriétaire du schéma  
 Vous pouvez spécifier le propriétaire du schéma en affectant une valeur de chaîne valide à **Schéma propriétaire** . Le propriétaire du schéma par défaut est le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , mais vous pouvez spécifier le propriétaire de schéma de votre choix.  
  
### <a name="specifying-primary-keys-indexes-and-constraints"></a>Spécification des clés primaires, des index et des contraintes  
 L'Assistant Génération de schéma par défaut crée une contrainte de clé primaire dans chaque table de dimension de la base de données de la zone de sujet. La clé primaire correspond à l’attribut qui est désigné comme attribut clé dans la dimension [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] correspondante. Cette contrainte améliore les performances de traitement dans la plupart des environnements, à un coût minime. Les clés primaires logiques sont toujours créées dans la vue de source de données, même si vous choisissez de ne pas créer la clé primaire dans la base de données de la zone de sujet. Pour définir des contraintes de clé primaire sur des tables de dimension, sélectionnez **Créer des clés primaires sur les tables de la dimension**.  
  
 L'assistant par défaut crée également des index sur les colonnes clés étrangères dans chaque table de faits. Ces index améliorent les performances de traitement de la plupart des environnements. Les performances sont généralement renforcées car les requêtes de traitement que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère pour récupérer de nouvelles données dans la base de données de la zone de sujet incluent généralement un grand nombre d'instructions de jointure entre la table de faits et les tables de dimension. Pour définir les index sur les colonnes clés étrangères de chaque table de faits, sélectionnez **Créer des index**.  
  
 Enfin, l'assistant par défaut applique l'intégrité référentielle entre la table de faits et chacune des tables de dimension. Si vous choisissez de ne pas appliquer l'intégrité référentielle, l'Assistant Génération de schéma crée tout de même ces relations dans la base de données et la vue de la source de données. Pour appliquer l’intégrité référentielle, sélectionnez **Appliquer l’intégrité référentielle**.  
  
### <a name="preserving-data-for-incremental-generation"></a>Préservation de données pour la génération incrémentielle  
 L'Assistant Génération de schéma par défaut tente de préserver les données lorsque le schéma de la base de données est régénéré. Si l'Assistant Génération de schéma doit supprimer des lignes en raison d'une modification du schéma, vous recevez un avertissement avant que les lignes ne soient supprimées. Par exemple, des lignes doivent parfois être supprimées pour résoudre des problèmes d'intégrité référentielle, parce que vous avez supprimé une dimension ou qu'un type de données a changé lorsque vous avez modifié un attribut de dimension. Pour préserver les données pendant la régénération d’un schéma de base de données, sélectionnez **Préserver les données lors de la régénération**.  
  
## <a name="step-4-specify-naming-conventions"></a>Étape 4 : spécifier des conventions de nom  
 Vous pouvez définir les conventions de nom que l’Assistant Génération de schéma utilise quand vous générez certains objets dans la base de données de la zone de sujet dans la page **Spécifier les conventions de nom** de l’assistant. Pour plus d’informations sur les options disponibles dans la page **Spécifier les conventions de nom**, consultez [Spécifier les conventions de nom &#40;Assistant Génération de schéma&#41; &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/02d830ea-5b1f-4485-9f94-d64b8bea592b).  
  
  
