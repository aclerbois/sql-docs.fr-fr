---
title: Définition des Options de Migration (AccessToSQL) et de la Conversion | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
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
- conversion, setting options
- migration options
- modes
- options, conversion settings
- project settings
- schemas
ms.assetid: 0a7304df-2f35-4453-96ef-7ac83dea1167
caps.latest.revision: 20
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 61814a4d5f4b62c3f7262c7c249b165dd8a99571
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-conversion-and-migration-options-accesstosql"></a>Conversion de paramètre et les Options de Migration (AccessToSQL)
Pour chaque projet SSMA, vous pouvez définir les options au niveau du projet. Ces options déterminent la façon dont les objets sont convertis, la migration des données et la correspondance entre les types de sources de données et les types de données cible. Avant de convertir des objets à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure ou migrer des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, vérifiez que les options de configuration sont appropriées pour le projet.  
  
## <a name="configuration-options-and-modes"></a>Modes et les Options de configuration  
SSMA a quatre ensembles de paramètres de configuration et les quatre modes de configuration de ces paramètres : par défaut, Optimistic, complète et personnalisée. Le mode par défaut est recommandé pour la plupart des utilisateurs. Utilisez le mode optimisé pour les conversions simples. Utilisez le mode complet si vous souhaitez afficher tous les messages. Dans le mode personnalisé, vous définissez les options.  
  
Les paramètres sont décrits dans la section « Référence de l’Interface utilisateur » de cette documentation. Pour plus d’informations sur les paramètres et la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet (Conversion)](http://msdn.microsoft.com/en-us/bcebc635-c638-4ddb-924c-b9ccfef86388)  
  
-   [Paramètres du projet (Migration)](http://msdn.microsoft.com/en-us/4caebc9c-8680-4b99-a8fa-89c43161c95d)  
  
-   [Paramètres du projet (GUI)](http://msdn.microsoft.com/en-us/cf06baf1-8714-48a3-95dc-781f6ca53693)  
  
-   [Paramètres du projet (Mappage de type)](http://msdn.microsoft.com/en-us/b87b9683-abed-4677-8c50-18bdba704655)  
  
-   [Paramètres du projet (SQL Azure)](http://msdn.microsoft.com/en-us/bbb8a204-d0e4-4f0b-9709-271feb1f136e)  
  
## <a name="setting-project-options"></a>Définition des Options de projet  
Dans SSMA, vous pouvez configurer les paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration de SSMA et appliqués à tout nouveau projet que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**.  
  
2.  Dans le **les paramètres de projet par défaut** boîte de dialogue, effectuez l’une des opérations suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichés / a été remplacée par **Version cible de la Migration** liste déroulante, cliquez sur **général** en bas du volet gauche, puis **SQL Azure ou de la Migration ou de la Conversion**.  
  
        > [!NOTE]  
        > SQL Azure est disponible dans le **général** onglet uniquement si le type de projet créé est SQL Azure.  
  
    -   Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **Optimistic**, ou **complète** dans les **Mode** zone de liste déroulante.  
  
    -   Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans les **Mode** zone, sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou une valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres pour le projet actuel. Ces paramètres sont enregistrés dans le fichier de projet actuel.  
  
**Pour personnaliser les paramètres pour le projet actuel**  
  
1.  Sur le **outils** menu, sélectionnez **les paramètres de projet**.  
  
2.  Dans le **les paramètres de projet** boîte de dialogue, effectuez l’une des opérations suivantes :  
  
    -   Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **Optimistic**, ou **complète** dans les **Mode** zone de liste déroulante.  
  
    -   Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans les **Mode** zone, sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou une valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données source et cible, consultez [Source de mappage et les Types de données cible](http://msdn.microsoft.com/en-us/b362a075-16e7-423f-b63f-e1e9f02844a9)  
  
-   Pour personnaliser le mappage des bases de données source et cible, consultez [Source de mappage et de bases de données cibles](http://msdn.microsoft.com/en-us/69bee937-7b2c-49ee-8866-7518c683fad4)  
  
-   Sinon, vous pouvez convertir les définitions d’objets de base de données Access dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou définitions d’objets SQL Azure. Pour plus d’informations, consultez [convertir des objets de base de données Access](http://msdn.microsoft.com/en-us/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c)  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
  
