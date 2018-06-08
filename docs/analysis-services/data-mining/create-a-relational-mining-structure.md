---
title: Créer une Structure d’exploration de données relationnelles | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fe3958698c41cc54b6606255ced3a79133c66320
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-relational-mining-structure"></a>Créer une structure d'exploration de données relationnelle
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La plupart des modèles d'exploration de données sont basés sur des sources de données relationnelles. Les avantages de la création d'un modèle d'exploration de données relationnel sont que vous pouvez compiler des données ad hoc et effectuer l'apprentissage et la mise à jour d'un modèle sans entrer dans la complexité d'une création de cube.  
  
 Une structure d'exploration de données relationnelle peut ajouter des données provenant de sources disparates. Les données brutes peuvent être stockées dans des tables, des fichiers ou des systèmes de bases de données relationnelles, à condition que les données puissent être définies dans la vue de source de données. Par exemple, vous devez utiliser une structure d'exploration de données relationnelle si vos données se trouvent dans Excel, un entrepôt de données SQL Server ou une base de données de rapports SQL Server, ou dans des sources externes accessibles via des fournisseurs OLE DB ou ODBC.  
  
 Cette rubrique fournit une vue d'ensemble de l'utilisation de l'Assistant Exploration de données pour créer une structure d'exploration de données relationnelle.  
  
 [Spécifications](#BKMK_Relational_Structure)  
  
 [Processus de création d'une structure d'exploration de données relationnelle](#BKMK_Relational_Structure)  
  
 [Comment choisir des sources de données](#BKMK_ChooseRelData)  
  
 [Comment spécifier le type de contenu et le type de données](#bkmk_ContentDataType)  
  
 [Pourquoi et comment créer un jeu de données d'exclusion](#bkmk_Holdout)  
  
 [Pourquoi et comment activer l'extraction](#BKMK_DrillThru)  
  
## <a name="requirements"></a>Spécifications  
 Tout d'abord, vous devez disposer d'une source de données existante. Vous pouvez utiliser le concepteur de source de données pour configurer une source de données, si aucune n'est disponible. Pour plus d’informations, consultez [Créer une source de données &#40;SSAS Multidimensionnel&#41;](../../analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional.md).  
  
 Ensuite, utilisez l'Assistant Vue de source de données pour compiler les données requises en une vue de source de données. Pour plus d’informations sur la façon dont vous pouvez sélectionner, transformer, filtrer ou gérer des données avec des vues de source de données, consultez [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md).  
  
##  <a name="BKMK_Relational_Structure"></a> Vue d’ensemble du processus  
 Démarrez l’Assistant Exploration de données en cliquant avec le bouton droit sur le nœud **Structures d’exploration de données** dans l’Explorateur de solutions, puis en sélectionnant **Ajouter une nouvelle structure d’exploration de données**. L'Assistant vous guide à travers les étapes suivantes pour créer la structure d'un nouveau modèle d'exploration de données relationnel :  
  
1.  **Sélectionner la méthode de définition**: sélectionnez un type de source de données, puis choisissez **À partir d’une base de données relationnelle ou d’un entrepôt de données qui existent déjà**.  
  
2.  **Créer la structure d'exploration de données**: déterminez si vous allez générer simplement une structure ou une structure avec un modèle d'exploration de données.  
  
     Choisissez également un algorithme approprié pour votre modèle initial. Pour obtenir de l’aide sur l’algorithme le mieux approprié à certaines tâches, consultez [Algorithmes d’exploration de données &#40;Analysis Services – exploration de données&#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md).  
  
3.  **Sélectionner une vue de source de données** choisissez une vue de source de données à utiliser dans l'apprentissage de votre modèle. La vue de source de données peut également contenir des données utilisées pour les test ou des données non liées. Vous devez alors choisir les données réellement utilisées dans la structure et le modèle. Vous pouvez également appliquer ultérieurement des filtres aux données.  
  
4.  **Spécifier les types des tables**: sélectionnez la table qui contient les cas utilisés pour l’analyse. Pour certains jeux de données, notamment ceux utilisés pour générer des modèles de panier d'achat, vous pouvez également inclure une table associée, à utiliser comme table imbriquée.  
  
     Pour chaque table, vous devez spécifier la clé, de sorte que l'algorithme puisse identifier un enregistrement unique, et les enregistrements associés si vous avez ajouté une table imbriquée.  
  
     Pour plus d’informations, consultez [Colonnes de structure d’exploration de données](../../analysis-services/data-mining/mining-structure-columns.md).  
  
5.  **Spécifier les données d’apprentissage**: dans cette page, vous choisissez comme *table de cas*la table qui contient les données les plus importantes pour l’analyse.  
  
     Pour certains jeux de données, notamment ceux utilisés pour générer des modèles de panier d'achat, vous pouvez également inclure une table associée. Les valeurs de cette table imbriquée sont gérées sous forme de plusieurs valeurs, toutes associées à une ligne unique (ou cas) dans la table principale.  
  
6.  **Spécifier le type de contenu et de données des colonnes**: pour chaque colonne que vous utilisez dans la structure, vous devez choisir un *type de données* et un *type de contenu*.  
  
     L'Assistant détecte automatiquement les types de données possibles, mais vous n'êtes pas tenu d'utiliser le type de données recommandé par l'Assistant. Par exemple, même si vos données contiennent des nombres, ils peuvent représenter des données catégorielles. Les colonnes que vous spécifiez en tant que clés se voient automatiquement attribuer le type de données approprié pour ce type de modèle en particulier. Pour plus d’informations, consultez [Colonnes d’un modèle d’exploration de données](../../analysis-services/data-mining/mining-model-columns.md) et [Types de données &#40;Exploration de données&#41;](../../analysis-services/data-mining/data-types-data-mining.md).  
  
     Le *type de contenu* que vous choisissez pour chaque colonne que vous utilisez dans le modèle indique à l'algorithme le mode de traitement des données.  
  
     Par exemple, vous pouvez décider de discrétiser des nombres, au lieu d'utiliser des valeurs continues. Vous pouvez également demander à l'algorithme de détecter automatiquement le type de contenu le plus approprié pour la colonne. Pour plus d’informations, consultez [Types de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
7.  **Créer un jeu de test**: dans cette page, vous pouvez indiquer à l'Assistant combien de données doivent être mises de côté pour être utilisées lors du test du modèle. Si vos données prennent en charge plusieurs modèles, il est judicieux de créer un jeu de données d'exclusion, afin que tous les modèles puissent être testés sur les mêmes données.  
  
     Pour plus d’informations, consultez [Test et validation &#40;exploration de données&#41;](../../analysis-services/data-mining/testing-and-validation-data-mining.md).  
  
8.  **Fin de l'Assistant**: dans cette page, vous donnez un nom à la nouvelle structure d'exploration de données et au modèle d'exploration de données associé, puis vous enregistrez la structure et le modèle.  
  
     Vous pouvez également définir des options importantes, selon le type de modèle. Par exemple, vous pouvez activer l'extraction sur la structure.  
  
     À ce stade, la structure d'exploration de données et son modèle sont simplement des métadonnées ; vous devez les traiter tous deux pour obtenir des résultats.  
  
##  <a name="BKMK_ChooseRelData"></a> Comment choisir des données relationnelles  
 Les structures d’exploration de données relationnelles peuvent être basées sur n’importe quelle donnée disponible par le biais d’une source de données OLE DB. Si les données sources sont contenues dans plusieurs tables, utilisez une vue de source de données pour rassembler les tables et les colonnes dont vous avez besoin en un seul lieu.  
  
 Si les tables comprennent des relations un-à-plusieurs (par exemple, vous avez plusieurs enregistrements d'achats pour chaque client que vous souhaitez analyser) vous pouvez ajouter les deux tables, puis utiliser une table comme la table de cas, en liant les données du côté « plusieurs » de la relation en tant que table imbriquée.  
  
 Les données d'une structure d'exploration de données sont dérivées de tout ce qui se trouve dans la vue de source de données existante. Vous pouvez modifier les données dont vous avez besoin dans la vue de source de données, en ajoutant des relations ou des colonnes dérivées qui ne sont peut-être pas présentes dans les données relationnelles sous-jacentes. Vous pouvez également créer des calculs nommés ou des agrégations dans la vue de source de données. Ces fonctionnalités sont très pratiques si vous ne contrôlez pas l'organisation des données dans la source de données ou si vous souhaitez faire des essais avec différentes agrégations de données pour vos modèles d'exploration de données.  
  
 Vous ne devez pas utiliser la totalité des données disponibles ; vous pouvez choisir les colonnes à inclure dans la structure d'exploration de données. Tous les modèles basés sur cette structure peuvent alors utiliser ces colonnes ou vous pouvez marquer certaines colonnes comme **Ignorer** pour un modèle en particulier. Vous pouvez permettre aux utilisateurs d'un modèle d'exploration de données d'explorer ses résultats pour afficher des colonnes supplémentaires de la structure d'exploration de données qui n'étaient pas incluses dans le modèle d'exploration de données lui-même.  
  
##  <a name="bkmk_ContentDataType"></a> Comment spécifier le type de contenu et le type de données  
 Le type de données est à peu près le même que les types de données que vous spécifiez dans SQL Server ou dans d'autres interfaces d'application : dates et heures, nombres de différentes tailles, valeurs booléennes, texte et autres données discrètes.  
  
 Toutefois, les types de contenu sont importants pour l'exploration de données et affectent les résultats de l'analyse. Le type de contenu indique à l'algorithme ce qu'il doit faire avec les données : les numéros doivent-ils être traités sur une échelle continue, ou placés dans un conteneur ? Combien de valeurs potentielles existe-t-il ? Chaque valeur est-elle distincte ? Si la valeur est une clé, de quel type de clé s'agit-il : indique-t-elle une valeur date/heure, une séquence, ou un autre type de clé ?  
  
 Notez que le choix du type de données peut limiter votre choix de types de contenu. Par exemple, vous ne pouvez pas discrétiser des valeurs non numériques. Si le type de contenu que vous souhaitez n’est pas affiché, vous pouvez cliquer sur **Précédent** pour revenir à la page du type de données et essayer un autre type de données.  
  
 Il n'est pas primordial d'obtenir le type de contenu approprié. Il est très facile de créer un modèle et de modifier le type de contenu dans le modèle, à condition que le nouveau type de contenu soit pris en charge par le type de données défini dans la structure d'exploration de données. Il est également très fréquent de créer plusieurs modèles utilisant différents types de contenu, à des fins d'expérience ou pour répondre aux spécifications d'un algorithme différent.  
  
 Par exemple, si vos données contiennent une colonne revenu, vous pouvez créer deux modèles différents lors de l'utilisation de l'algorithme MDT (Microsoft Decision Trees) et configurer la colonne soit pour des nombres continus soit pour des plages discrètes. Toutefois, si vous avez ajouté un modèle utilisant l'algorithme MNB (Microsoft Naïve Bayes), vous serez forcé de modifier la colonne pour des valeurs discrétisées uniquement, car cet algorithme ne prend pas en charge les nombres continus.  
  
##  <a name="bkmk_Holdout"></a> Pourquoi et comment fractionner les données en jeux d'apprentissage et jeux de test  
 Vers la fin de l'Assistant, vous devez décider de partitionner vos données en jeux d'apprentissage et en jeux de test. La possibilité de configurer une partie des données échantillonnée de manière aléatoire pour le test est très utile car elle garantit qu'un jeu cohérent de données de test est disponible pour une utilisation avec tous les modèles d'exploration de données associés à la nouvelle structure d'exploration de données.  
  
> [!WARNING]  
>  Notez que cette option est disponible pour tous les types de modèle. Par exemple, si vous créez un modèle de prévision, vous ne pourrez pas utiliser l'exclusion, car l'algorithme de série chronologique requiert qu'il n'existe aucun écart dans les données. Pour obtenir une liste des types de modèles qui prennent en charge les jeux de données d’exclusion, consultez [Jeux de données d’apprentissage et de test](../../analysis-services/data-mining/training-and-testing-data-sets.md).  
  
 Pour créer ce jeu de données d'exclusion, spécifiez le pourcentage de données à utiliser pour le test. Toutes les données restantes sont utilisées pour l'apprentissage. Éventuellement, vous pouvez définir un nombre maximal de cas à utiliser pour le test ou définir une valeur initiale à utiliser au démarrage du processus de sélection aléatoire.  
  
 La définition du jeu de données d'exclusion est stockée avec la structure d'exploration de données, de sorte que lorsque vous créez un nouveau modèle basé sur la structure, le jeu de données de test est disponible pour évaluer la précision du modèle. Si vous supprimez le cache de la structure d'exploration de données, les informations identifiant les cas qui ont été utilisés pour l'apprentissage et ceux qui ont été utilisés pour le test sont également supprimées.  
  
##  <a name="BKMK_DrillThru"></a> Pourquoi et comment activer l'extraction  
 Vers la toute fin de l'Assistant, vous avez la possibilité d'activer *l’extraction*. Il est facile d'ignorer cette option, mais c'est une option importante. L'extraction permet de consulter les données source de la structure d'exploration de données en interrogeant le modèle d'exploration de données.  
  
 Pourquoi cette option est-elle utile ? Supposons que vous consultez les résultats d'un modèle de clustering et que vous souhaitez afficher les clients placés dans un cluster spécifique. À l'aide de l'extraction, vous pouvez consulter des détails comme les informations de contact.  
  
> [!WARNING]  
>  Pour utiliser l'extraction, vous devez l'activer lors de la création de la structure d'exploration de données. Vous pouvez activer l'extraction sur les modèles ultérieurement, en définissant une propriété sur le modèle, mais les structures d'exploration de données requièrent que cette option soit définie au début. Pour plus d’informations, consultez [Requêtes d’extraction &#40;exploration de données&#41;](../../analysis-services/data-mining/drillthrough-queries-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepteur d’exploration de données](../../analysis-services/data-mining/data-mining-designer.md)   
 [Assistant exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-wizard-analysis-services-data-mining.md)   
 [Propriétés du modèle d’exploration de données](../../analysis-services/data-mining/mining-model-properties.md)   
 [Propriétés de Structure d’exploration de données et les colonnes de Structure](../../analysis-services/data-mining/properties-for-mining-structure-and-structure-columns.md)   
 [Tâches de la Structure d’exploration de données et procédures](../../analysis-services/data-mining/mining-structure-tasks-and-how-tos.md)  
  
  
