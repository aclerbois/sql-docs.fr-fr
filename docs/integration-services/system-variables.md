---
title: Variables système | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- containers [Integration Services], variables
- tasks [Integration Services], variables
- system variables [Integration Services]
- event handlers [Integration Services], variables
- variables [Integration Services], system
ms.assetid: efecd0d4-1489-4eba-a8fe-275d647058b8
caps.latest.revision: 54
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a154155683bff4521a088552e299d34f72c3632e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="system-variables"></a>Variables système
  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] fournit un ensemble de variables système qui stockent des informations sur le package en cours d'exécution et ses objets. Ces variables peuvent être utilisées dans des expressions et des propriétés d'expressions afin de personnaliser des packages, des conteneurs, des tâches et des gestionnaires d'événements.  
  
 Toutes les variables (système et définies par l'utilisateur) peuvent être utilisées dans les liaisons de paramètres que la tâche d'exécution SQL utilise pour mapper des variables à des paramètres.  
  
## <a name="system-variables-for-packages"></a>Variables système pour les packages  
 Le tableau suivant décrit les variables système fournies par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour les packages.  
  
|Variable système|Type de données|Description|  
|---------------------|---------------|-----------------|  
|**CancelEvent**|Int32|Gestionnaire d'un objet d'événement Windows que la tâche peut signaler pour indiquer que la tâche doit interrompre son exécution.|  
|**ContainerStartTime**|DateTime|Heure de démarrage du conteneur.|  
|**CreationDate**|DateTime|Date de création du package.|  
|**CreatorComputerName**|String|Ordinateur sur lequel le package a été créé.|  
|**CreatorName**|String|Nom de la personne qui a créé le package.|  
|**ExecutionInstanceGUID**|String|Identificateur unique de l'instance exécutée d'un package.|  
|**FailedConfigurations**|String|Noms des configurations de package ayant échoué.|  
|**IgnoreConfigurationsOnLoad**|Booléen|Indique si les configurations de package doivent être ignorées lors du chargement du package.|  
|**InteractiveMode**|Booléen|Indique si le package est exécuté en mode interactif. Si un package s’exécute dans le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cette propriété a la valeur **True**. Si un package s’exécute par le biais de l’utilitaire de ligne de commande **DTExec** , la propriété a la valeur **False**.|  
|**LocaleId**|Int32|Paramètre régional utilisé par le package.|  
|**MachineName**|String|Nom de l'ordinateur sur lequel s'exécute le package.|  
|**OfflineMode**|Booléen|Indique si le package est en mode hors connexion. Le mode hors connexion n'acquiert pas de connexions à des sources de données.|  
|**PackageID**|String|Identificateur unique du package.|  
|**PackageName**|String|Nom du package.|  
|**StartTime**|DateTime|Heure de début d'exécution du package.|  
|**ServerExecutionID**|Int64|ID d'exécution du package exécuté sur le serveur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .<br /><br /> La valeur par défaut est zéro. La valeur est modifiée uniquement si le package est exécuté par ISServerExec sur le serveur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Lorsqu'il existe un package enfant, la valeur est passée du package parent au package enfant.|  
|**UserName**|String|Compte de l'utilisateur qui a démarré le package. Le nom d'utilisateur est qualifié par le nom de domaine.|  
|**VersionBuild**|Int32|Version du package.|  
|**VersionComment**|String|Commentaires sur la version du package.|  
|**VersionGUID**|String|Identificateur unique de la version.|  
|**VersionMajor**|Int32|Version principale du package.|  
|**VersionMinor**|Int32|Version secondaire du package.|  
  
## <a name="system-variables-for-containers"></a>Variables système pour les conteneurs  
 Le tableau suivant décrit les variables système fournies par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour les conteneurs de boucles For, de boucles Foreach et de séquence.  
  
|Variable système|Type de données|Description|Conteneur|  
|---------------------|---------------|-----------------|---------------|  
|**LocaleId**|Int32|Paramètre régional utilisé par le conteneur.|Conteneur de boucles For<br /><br /> Conteneur de boucles Foreach<br /><br /> conteneur de séquences|  
  
## <a name="system-variables-for-tasks"></a>Variables système pour les tâches  
 Le tableau suivant décrit les variables système fournies par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour les tâches.  
  
|Variable système|Type de données|Description|  
|---------------------|---------------|-----------------|  
|**CreationName**|String|Nom de la tâche.|  
|**LocaleId**|Int32|Paramètre régional utilisé par la tâche.|  
|**TaskID**|String|Identificateur unique d'une instance de tâche.|  
|**TaskName**|String|Nom de l'instance de tâche.|  
|**TaskTransactionOption**|Int32|Option de transaction utilisée par la tâche.|  
  
## <a name="system-variables-for-event-handlers"></a>Variables système pour les gestionnaires d'événements  
 Le tableau suivant décrit les variables système fournies par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour les gestionnaires d'événements. Toutes les variables ne sont pas disponibles pour tous les gestionnaires d'événements.  
  
|Variable système|Type de données|Description|Gestionnaire d'événements|  
|---------------------|---------------|-----------------|-------------------|  
|**Annuler**|Booléen|Indique si l'exécution du gestionnaire d'événements s'arrête lorsqu'une erreur, un avertissement ou une annulation de requête se produit.|Gestionnaire d'événements OnError<br /><br /> Gestionnaire d'événements OnWarning<br /><br /> Gestionnaire d'événements OnQueryCancel|  
|**ErrorCode**|Int32|Identificateur de l'erreur.|Gestionnaire d'événements OnError<br /><br /> Gestionnaire d'événements OnInformation<br /><br /> Gestionnaire d'événements OnWarning|  
|**ErrorDescription**|String|Description de l'erreur.|Gestionnaire d'événements OnError<br /><br /> Gestionnaire d'événements OnInformation<br /><br /> Gestionnaire d'événements OnWarning|  
|**ExecutionStatus**|Booléen|État de l'exécution en cours.|Gestionnaire d'événements OnExecStatusChanged|  
|**ExecutionValue**|DBNull|Valeur de l'exécution.|Gestionnaire d'événements OnTaskFailed|  
|**LocaleId**|Int32|Paramètre régional utilisé par le gestionnaire d'événements.|Tous les gestionnaires d'événements|  
|**PercentComplete**|Int32|Pourcentage de travail terminé.|Gestionnaire d'événements OnProgress|  
|**ProgressCountHigh**|Int32|Partie supérieure d'une valeur 64 bits qui indique le nombre total d'opérations traitées par l'événement OnProgress.|Gestionnaire d'événements OnProgress|  
|**ProgressCountLow**|Int32|Partie inférieure d'une valeur 64 bits qui indique le nombre total d'opérations traitées par l'événement OnProgress.|Gestionnaire d'événements OnProgress|  
|**ProgressDescription**|String|Description de la progression.|Gestionnaire d'événements OnProgress|  
|**Propagate**|Booléen|Indique si l'événement est propagé à un gestionnaire d'événements de niveau supérieur.<br /><br /> Remarque : la valeur de la variable **Propagate** est ignorée lors de la validation du package. Si vous affectez la valeur **Propagate** à **False** dans un package enfant, cela n'empêche pas la propagation d'un événement à un package parent.|Tous les gestionnaires d'événements|  
|**SourceDescription**|String|Description de l'exécutable dans le gestionnaire d'événements qui a déclenché l'événement.|Tous les gestionnaires d'événements|  
|**SourceID**|String|Identificateur unique de l'exécutable dans le gestionnaire d'événements qui a déclenché l'événement.|Tous les gestionnaires d'événements|  
|**SourceName**|String|Nom de l'exécutable dans le gestionnaire d'événements qui a déclenché l'événement.|Tous les gestionnaires d'événements|  
|**VariableDescription**|String|Description de la variable.|Gestionnaire d'événements OnVariableValueChanged|  
|**VariableID**|String|Identificateur unique de la variable.|Gestionnaire d'événements OnVariableValueChanged|  
  
## <a name="system-variables-in-parameter-bindings"></a>Variables système dans des liaisons de paramètres  
 Il est souvent pratique d'enregistrer les valeurs de variables système dans des tables lors de l'exécution du package. Par exemple, un package qui crée dynamiquement une table et écrit le GUID de l'instance d'exécution du package qui a créé la table dans une colonne de table.  
  
 Si vous utilisez des variables système pour effectuer un mappage à des paramètres dans l'instruction SQL qu'une tâche d'exécution SQL utilise, il est important d'affecter au type de données de chaque liaison de paramètre le type de données de la variable système. Sinon, les valeurs des variable système pourraient être mal traduites. Par exemple, si la variable système **ExecutionInstanceGUID** , qui présente le type de données String et contient une chaîne représentant le GUID de l’instance d’exécution d’un package, est utilisée dans une liaison de paramètre avec le type de données GUID, le GUID de l’instance du package ne sera pas traduit correctement.  
  
 Cette règle s'applique également aux variables définies par l'utilisateur. Cependant, lorsque les types de données de variables système ne peuvent pas être modifiés et si vous devez personnaliser l'utilisation de ces variables en fonction des types de données, les variables définies par l'utilisateur offrent plus de souplesse. Les variables définies par l'utilisateur qui sont utilisées dans des liaisons de paramètres sont généralement définies avec des types de données compatibles avec les types de données des paramètres auxquels elles sont mappées.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Mapper des paramètres de requête à des variables dans une tâche d'exécution SQL](http://msdn.microsoft.com/library/6a164349-dfcf-4995-80bc-d4e7aee52a83)  
  
  
