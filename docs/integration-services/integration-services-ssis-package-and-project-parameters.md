---
title: Paramètres de projet et de package Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.parameter.f1
- sql13.dts.designer.paramterwindow.f1
ms.assetid: 9ed9ca8e-8b1e-48d9-907d-285516d6562b
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 44b3ace5dca858f1216d1e1d5298be8c26a444b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-package-and-project-parameters"></a>Paramètres de projet et de package Integration Services (SSIS)
  Les paramètres[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] (SSIS) vous permettent d'affecter des valeurs aux propriétés dans des packages au moment de l'exécution du package. Vous pouvez créer des *paramètres de projet* au niveau du projet et des *paramètres de package* au niveau du package. Les paramètres du projet sont utilisés pour fournir une entrée externe que le projet reçoit à un ou plusieurs packages du projet. L'utilisation de paramètres de package vous permet de modifier l'exécution du package sans avoir à modifier et à redéployer le package.  
  
 Dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] vous créez, modifiez ou supprimez les paramètres d'un projet à l'aide de la fenêtre **Project.params** . Vous créez, modifiez et supprimez les paramètres d'un package à l'aide de l'onglet **Paramètres** dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] . Vous associez un nouveau paramètre ou un paramètre existant à une propriété de tâche à l'aide de la boîte de dialogue **Paramétrer** . Pour plus d'informations sur l'utilisation de la fenêtre **Project.params** et l'onglet **Paramètres** , consultez [Create Parameters](http://msdn.microsoft.com/library/cd5d675b-dd5d-49cc-8b1f-dc717a973f99). Pour plus d'informations sur la boîte de dialogue **Paramétrer** , consultez [Parameterize Dialog Box](http://msdn.microsoft.com/library/fac02b6d-d247-447a-8940-e8700c7ac350).  
  
## <a name="parameters-and-package-deployment-model"></a>Paramètres et modèle de déploiement de package  
 En général, si vous déployez un package à l'aide du modèle de déploiement de package, vous devez utiliser les configurations au lieu des paramètres.  
  
 Lorsque vous déployez un package qui contient des paramètres à l'aide du modèle de déploiement de package, puis exécutez le package, les paramètres ne sont pas appelés au moment de l'exécution. Si le package contient des paramètres de package et des expressions dans le package utilisent les paramètres, les valeurs résultantes sont appliquées au moment de l'exécution. Si le package contient des paramètres de projet, l'exécution du package peut échouer.  
  
## <a name="parameters-and-project-deployment-model"></a>Paramètres et modèle de déploiement de projet  
 Quand vous déployez un projet sur le serveur SQL Server Integration Services (SSIS), vous utilisez des vues, des procédures stockées et l’interface utilisateur [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] pour gérer les paramètres du projet et du package. Pour plus d'informations, consultez les rubriques ci-dessous.  
  
-   [Vues &#40;catalogue Integration Services&#41;](../integration-services/system-views/views-integration-services-catalog.md)  
  
-   [Procédures stockées &#40;catalogue Integration Services&#41;](../integration-services/system-stored-procedures/stored-procedures-integration-services-catalog.md)  
  
-   [Boîte de dialogue Configurer](../integration-services/service/configure-dialog-box.md)  
  
-   [Execute Package Dialog Box](../integration-services/packages/run-integration-services-ssis-packages.md#execute_package_dialog)  
  
### <a name="parameter-values"></a>Valeurs des paramètres  
 Attribuez au plus trois types différents de valeurs à un paramètre. Lorsque l'exécution d'un package démarre, une valeur unique est utilisée pour le paramètre, et le paramètre est résolu à sa valeur littérale finale.  
  
 Le tableau qui suit énumère les types de valeurs.  
  
|Nom de la valeur|Description|Type de la valeur|  
|----------------|-----------------|-------------------|  
|Valeur d'exécution|Valeur affectée à une instance spécifique de l'exécution du package. Cette affectation remplace toutes les autres valeurs, mais s'applique uniquement à une instance unique de l'exécution du package.|Littéral|  
|Valeur de serveur|Valeur affectée au paramètre dans l’étendue du projet après que le projet est déployé sur le serveur Integration Services. Cette valeur remplace la valeur de conception par défaut.|Littéral ou Référence de variable d'environnement|  
|Valeur de création|Valeur affectée au paramètre lorsque le projet est créé ou modifié dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]. Cette valeur est conservée avec le projet.|Littéral|  
  
 Utilisez un paramètre unique pour affecter une valeur à plusieurs propriétés des packages. Il n'est possible d'affecter une valeur à une propriété de package unique qu'à partir d'un paramètre unique.  
  
###  <a name="executions"></a> Exécutions et valeurs de paramètres  
 L' *exécution* est un objet qui représente une instance unique de l'exécution du package. Lorsque vous créez une exécution, vous spécifiez tous les détails nécessaires pour exécuter un package, tel que les valeurs de paramètres d'exécution. Vous pouvez également modifier les valeurs de paramètres pour les exécutions existantes.  
  
 Lorsque vous définissez de manière explicite une valeur de paramètre d'exécution, cette valeur s'applique uniquement à cette instance d'exécution particulière. La valeur d'exécution est utilisée à la place d'une valeur de serveur ou d'une valeur de conception. Si vous ne définissez pas de valeur d'exécution de manière explicite, et qu'une valeur de serveur a été spécifiée, la valeur de serveur est utilisée.  
  
 Lorsqu'un paramètre est marqué comme requis, une valeur de serveur ou une valeur d'exécution doit être spécifiée pour ce paramètre. Sinon, le package correspondant ne s'exécute pas. Bien que le paramètre dispose d'une valeur par défaut au moment de la conception, elle ne sera jamais utilisée une fois le projet déployé.  
  
#### <a name="environment-variables"></a>Variables d'environnement  
 Si un paramètre référence une variable d'environnement, la valeur littérale de cette variable est résolue par l'intermédiaire de la référence environnementale spécifiée et est appliquée au paramètre. La valeur de paramètre littérale finale utilisée pour l'exécution du package est appelée « valeur de paramètre d'exécution ». Vous spécifiez la référence environnementale pour une exécution à l'aide de la boîte de dialogue **Exécuter** .  
  
 Si un paramètre de projet référence une variable d'environnement et la valeur littérale de la variable n'est pas résolue au moment de l'exécution, la valeur de conception est utilisée. La valeur de serveur n'est pas utilisée.  
  
 Pour afficher les variables d'environnement affectées aux valeurs de paramètre, interrogez l'affichage catalog.object_parameters. Pour plus d’informations, consultez [catalog.object_parameters &#40;base de données SSISDB&#41;](../integration-services/system-views/catalog-object-parameters-ssisdb-database.md).  
  
#### <a name="determining-execution-parameter-values"></a>Détermination des valeurs de paramètre d'exécution  
 La procédure stockée et les vues Transact-SQL suivantes peuvent être utilisées pour afficher et définir des valeurs de paramètre.  
  
 [catalog.execution_parameter_values &#40;base de données SSISDB&#41;](../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md) (vue)  
 Affiche les valeurs de paramètre effectives qui seront utilisées par une exécution spécifique  
  
 [catalog.get_parameter_values &#40;base de données SSISDB&#41;](../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md) (procédure stockée)  
 Résout et affiche les valeurs effectives du package et de la référence environnementale spécifiés  
  
 [catalog.object_parameters &#40;base de données SSISDB&#41;](../integration-services/system-views/catalog-object-parameters-ssisdb-database.md) (vue)  
 Affiche les paramètres et les propriétés de tous les packages et les projets dans le catalogue [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , y compris les valeurs de conception par défaut et les valeurs de serveur par défaut.  
  
 [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
 Définit la valeur d'un paramètre pour une instance d'exécution dans le catalogue [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
 Vous pouvez également utiliser la boîte de dialogue **Exécuter le package** dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] pour modifiez la valeur de paramètre. Pour plus d'informations, consultez [Execute Package Dialog Box](../integration-services/packages/run-integration-services-ssis-packages.md#execute_package_dialog).  
  
 Vous pouvez également utiliser l'option **/Parameter** dtexec pour modifier une valeur de paramètre. Pour plus d'informations, consultez [dtexec Utility](../integration-services/packages/dtexec-utility.md).  
  
### <a name="parameter-validation"></a>Validation des paramètres  
 Si les valeurs de paramètre ne peuvent pas être résolues, l'exécution du package correspondante échoue. Pour éviter les échecs, vous pouvez valider les projets et les packages à l'aide de la boîte de dialogue **Valider** dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. La validation vous permet de vérifier que tous les paramètres disposent des valeurs nécessaires ou qu'ils peuvent résoudre les valeurs requises avec des références environnementales spécifiques. La validation vérifie également d'autres problèmes courants liés aux packages.  
  
 Pour plus d'informations, consultez [Validate Dialog Box](../integration-services/service/validate-dialog-box.md).  
  
### <a name="parameter-example"></a>Exemples de paramètre  
 Cet exemple illustre un paramètre nommé **pkgOptions** utilisé pour spécifier les options du package dans lequel il réside.  
  
 Au moment de la conception, lorsque le paramètre a été créé dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], la valeur par défaut 1 a été affectée au paramètre. Cette valeur par défaut est appelée « valeur de conception par défaut ». Si le projet a été déployé dans le catalogue SSISDB et qu'aucune autre valeur n'a été affectée à ce paramètre, la propriété de package correspondant au paramètre **pkgOptions** se verrait affecter la valeur 1 pendant l'exécution du package. La valeur de conception par défaut est conservée avec le projet pendant tout son cycle de vie.  
  
 Lors de la préparation une instance spécifique de l'exécution du package, la valeur 5 est affectée au paramètre **pkgOptions** . Cette valeur est appelée « valeur d'exécution » car elle s'applique au paramètre uniquement pour cette instance d'exécution particulière. Lorsque l'exécution démarre, la propriété de package correspondant au paramètre **pkgOptions** se voit affecter la valeur 5.  
  
## <a name="create-parameters"></a>Create Parameters
Vous pouvez utiliser [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] pour créer les paramètres de projet et de package. Les procédures suivantes fournissent des instructions pas-à-pas pour créer les paramètres de package/projet.  
  
> **REMARQUE :** si vous convertissez un projet que vous avez créé à l’aide d’une version antérieure de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] en modèle de déploiement de projet, vous pouvez utiliser **l’Assistant Conversion de projet Integration Services** pour créer les paramètres en fonction des configurations. Pour plus d’informations, consultez [Déployer des projets et des packages Integration Services (SSIS)](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
### <a name="create-package-parameters"></a>Créer les paramètres de package  
  
1.  Ouvrez le package dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], puis cliquez sur l'onglet **Paramètres** dans le Concepteur SSIS.  
  
     ![Onglet des paramètres de package](../integration-services/media/denali-package-parameters.gif "Onglet des paramètres de package")  
  
2.  Cliquez sur le bouton **Ajouter un paramètre** de la barre d'outils.  
  
     ![Bouton de la barre d’outils](../integration-services/media/denali-parameter-add.gif "Ajouter un bouton à la barre d’outils")  
  
3.  Entrez les valeurs des propriétés **Nom**, **Type de données**, **Valeur**, **Sensible**et **Requis** dans la liste elle-même ou dans la fenêtre **Propriétés** . Le tableau suivant décrit ces propriétés.  
  
    |Propriété|Description|  
    |--------------|-----------------|  
    |Nom   |Nom du paramètre.|  
    |Type de données|Type de données du paramètre.|  
    |Valeur par défaut|Valeur par défaut du paramètre affecté au moment de la conception. Cette valeur est aussi appelée « valeur de conception par défaut ».|  
    |Sensible|Les valeurs de paramètre sensibles sont chiffrées dans le catalogue et apparaissent sous la forme d'une valeur Null lorsqu'elles sont affichées avec Transact-SQL ou SQL Server Management Studio.|  
    |Requis|Nécessite qu'une valeur, autre que la valeur de conception par défaut, soit spécifiée pour que le package puisse s'exécuter.|  
    |Description|Pour faciliter la maintenance, description du paramètre. Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], définissez la description des paramètres dans la fenêtre Propriétés de Visual Studio lorsque le paramètre est sélectionné dans la fenêtre des paramètres applicables.|  
  
    > **REMARQUE :** lorsque vous déployez un projet dans le catalogue, plusieurs autres propriétés sont associées au projet. Pour afficher toutes les propriétés pour tous les paramètres dans le catalogue, utilisez la vue [catalog.object_parameters &#40;base de données SSISDB&#41;](../integration-services/system-views/catalog-object-parameters-ssisdb-database.md).  
  
4.  Enregistrez le projet pour sauvegarder les modifications apportées aux paramètres. Les valeurs de paramètre sont stockées dans le fichier du projet.  
  
    > **AVERTISSEMENT** Vous pouvez effectuer sur place des modifications dans la liste ou utiliser la fenêtre **Propriétés** pour modifier les valeurs des propriétés de paramètre. Vous pouvez supprimer un paramètre à l'aide du bouton **Supprimer (X)** de la barre d'outils. À l'aide du dernier bouton de la barre d'outils, vous pouvez spécifier une valeur de paramètre qui n'est utilisée que lorsque vous exécutez le package dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)].  
  
    > **REMARQUE :** si vous rouvrez le fichier de package sans ouvrir le projet dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], l’onglet **Paramètres** est vide et désactivé.  
  
### <a name="create-project-parameters"></a>Créer les paramètres de projet  
  
1.  Ouvrez le projet dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)].  
  
2.  Cliquez avec le bouton droit sur **Project.params** dans l'Explorateur de solutions, puis cliquez sur **Ouvrir** ou double-cliquez sur **Project.params** pour l'ouvrir.  
  
     ![Fenêtre des paramètres du projet](../integration-services/media/denali-project-parameters.gif "Fenêtre des paramètres du projet")  
  
3.  Cliquez sur le bouton **Ajouter un paramètre** de la barre d'outils.  
  
     ![Bouton de la barre d’outils](../integration-services/media/denali-parameter-add.gif "Ajouter un bouton à la barre d’outils")  
  
4.  Entrez les valeurs des propriétés **Nom**, **Type de données**, **Valeur**, **Sensible**et **Requis** .  
  
    |Propriété|Description|  
    |--------------|-----------------|  
    |Nom   |Nom du paramètre.|  
    |Type de données|Type de données du paramètre.|  
    |Valeur par défaut|Valeur par défaut du paramètre affecté au moment de la conception. Cette valeur est aussi appelée « valeur de conception par défaut ».|  
    |Sensible|Les valeurs de paramètre sensibles sont chiffrées dans le catalogue et apparaissent sous la forme d'une valeur Null lorsqu'elles sont affichées avec Transact-SQL ou SQL Server Management Studio.|  
    |Requis|Nécessite qu'une valeur, autre que la valeur de conception par défaut, soit spécifiée pour que le package puisse s'exécuter.|  
    |Description|Pour faciliter la maintenance, description du paramètre. Dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], définissez la description des paramètres dans la fenêtre Propriétés de Visual Studio lorsque le paramètre est sélectionné dans la fenêtre des paramètres applicables.|  
  
5.  Enregistrez le projet pour sauvegarder les modifications apportées aux paramètres. Les valeurs de paramètres sont stockées dans des configurations au sein du fichier projet. Enregistrez le fichier projet pour valider sur disque toutes les modifications apportées aux valeurs de paramètres.  
  
    > **AVERTISSEMENT** Vous pouvez effectuer sur place des modifications dans la liste ou utiliser la fenêtre **Propriétés** pour modifier les valeurs des propriétés de paramètre. Vous pouvez supprimer un paramètre à l'aide du bouton **Supprimer (X)** de la barre d'outils. En cliquant sur le dernier bouton de la barre d'outils pour ouvrir la boîte de dialogue **Gérer les valeurs de paramètre** , vous pouvez spécifier une valeur pour un paramètre utilisé uniquement lors de l'exécution du package dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)].  
    
## <a name="parameterize-dialog-box"></a>Parameterize Dialog Box
La boîte de dialogue **Paramétrer** vous permet d’associer un paramètre nouveau ou existant à la propriété d’une tâche. Vous ouvrez la boîte de dialogue en cliquant avec le bouton droit sur une tâche ou en affichant l'onglet Flux de contrôle dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , puis en cliquant sur **Paramétrer**. La liste suivante décrit les éléments d'interface utilisateur de la boîte de dialogue. Pour plus d’informations sur les paramètres, consultez [Paramètres Integration Services (SSIS)](https://msdn.microsoft.com/library/hh213214.aspx).
  
### <a name="options"></a>Options  
 **Propriété**  
 Sélectionnez la propriété de la tâche que vous souhaitez associer à un paramètre. Cette liste est remplie avec toutes les propriétés qui sont paramétrables.  
  
 **Utiliser un paramètre existant**  
 Sélectionnez cette option pour associer la propriété de la tâche à un paramètre existant, puis sélectionnez le paramètre dans la liste déroulante.  
  
 **Ne pas utiliser de paramètre**  
 Sélectionnez cette option pour supprimer une référence à un paramètre. Le paramètre n'est pas supprimé.  
  
 **Créer un paramètre**  
 Sélectionnez cette option pour créer un nouveau paramètre que vous souhaitez associer à la propriété de la tâche.  
  
 **Nom**  
 Spécifiez le nom du paramètre à créer.  
  
 **Description**  
 Spécifiez la description du paramètre.  
  
 **Value**  
 Spécifiez la valeur par défaut du paramètre. Cette opération est aussi appelée « valeur par défaut de conception », qui peut être remplacée ultérieurement au moment du déploiement.  
  
 **Portée**  
 Spécifiez l'étendue du paramètre en sélectionnant l'option **Projet** ou **Package** . Les paramètres du projet sont utilisés pour fournir une entrée externe que le projet reçoit à un ou plusieurs packages du projet. L'utilisation de paramètres de package vous permet de modifier l'exécution du package sans avoir à modifier et à redéployer le package.  
  
 **Sensible**  
 Spécifiez si le paramètre contient une valeur sensible en activant ou en désactivant la case à cocher. Les valeurs de paramètre sensibles sont chiffrées dans le catalogue et apparaissent sous la forme d'une valeur Null lorsqu'elles sont affichées avec Transact-SQL ou SQL Server Management Studio.  
  
 **Obligatoire**  
 Spécifiez si le paramètre nécessite qu'une valeur, autre que la valeur de conception par défaut, soit spécifiée pour que le package puisse s'exécuter.  
 
## <a name="set-parameter-values-after-the-project-is-deployed"></a>Définir des valeurs de paramètres après le déploiement du projet
L'Assistant Déploiement vous permet de définir des valeurs de paramètre par défaut du serveur lorsque vous déployez votre projet dans le catalogue. Une fois votre projet dans le catalogue, vous pouvez utiliser l'Explorateur d'objets SQL Server Management Studio (SSMS) ou Transact-SQL pour définir les valeurs par défaut du serveur.  
  
### <a name="set-server-defaults-with-ssms-object-explorer"></a>Définir les valeurs par défaut du serveur avec l’Explorateur d’objets SSMS  
  
1.  Sélectionnez le projet sous le nœud **Integration Services**, puis cliquez dessus avec le bouton droit.  
  
2.  Cliquez sur **Propriétés** pour ouvrir la fenêtre de dialogue **Propriétés du projet** .  
  
3.  Ouvrez la page des paramètres en cliquant sur **Paramètres** sous **Sélectionner une page**.  
  
4.  Sélectionnez le paramètre souhaité dans la liste **Paramètres** . Remarque : la colonne **Conteneur** permet de faire la distinction entre les paramètres du projet et les paramètres du package.  
  
5.  Dans la colonne **Valeur** , spécifiez la valeur de paramètre du serveur par défaut souhaitée.  

### <a name="set-server-defaults-with-transact-sql"></a>Définir les paramètres par défaut du serveur avec Transact-SQL  
 Pour définir les paramètres par défaut du serveur avec Transact-SQL, utilisez la procédure stockée [catalog.set_object_parameter_value &#40;base de données SSISDB&#41;](../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md). Pour afficher les valeurs par défaut actuelles du serveur, interrogez la vue [catalog.object_parameters &#40;base de données SSISDB&#41;](../integration-services/system-views/catalog-object-parameters-ssisdb-database.md). Pour effacer une valeur par défaut du serveur, utilisez la procédure stockée [catalog.clear_object_parameter_value &#40;base de données SSISDB&#41;](../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md).  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [Astuce rapide SSIS : Paramètres requis](http://go.microsoft.com/fwlink/?LinkId=239781), sur le site mattmasson.com.  
  
  
