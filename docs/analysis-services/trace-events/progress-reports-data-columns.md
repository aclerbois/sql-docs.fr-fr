---
title: Colonnes de données des rapports de progression | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e71e2e2973d633a6524c8d965b1eaa41c1dac55b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="progress-reports-data-columns"></a>Colonnes de données des rapports de progression
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La catégorie d'événement Rapport de progression contient les classes d'événements suivantes :  
  
|**ID d'événement**|**Nom d'événement**|**Description de l'événement**|  
|------------------|--------------------|---------------------------|  
|5|Progress Report Begin|Début du rapport de progression.|  
|6|Progress Report End|Fin du rapport de progression.|  
|7|Progress Report Current|Le rapport de progression est en cours.|  
|8|Progress Report Error|Erreur du rapport de progression.|  
  
 Les tableaux suivants répertorient les colonnes de données de chacune de ces classes d'événements.  
  
## <a name="progress-report-begindata-columns"></a>Progress Report Begin–Colonnes de données  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements. Les éléments suivants sont valides : paires **Sub Class Id**: **Sub Class Name** :<br /><br /> 1 : **Process**<br /><br /> 2 : **Merge**<br /><br /> 3 : **Delete**<br /><br /> 4 : **DeleteOldAggregations**<br /><br /> 5 : **Rebuild**<br /><br /> 6 : **Commit**<br /><br /> 7 : **Rollback**<br /><br /> 8 : **CreateIndexes**<br /><br /> 9 : **CreateTable**<br /><br /> 10 : **InsertInto**<br /><br /> 11 : **Transaction**<br /><br /> 12 : **Initialize**<br /><br /> 13 : **Discretize**<br /><br /> 14 : **Query**<br /><br /> 15 : **CreateView**<br /><br /> 16 : **WriteData**<br /><br /> 17 : **ReadData**<br /><br /> 18 : **GroupData**<br /><br /> 19 : **GroupDataRecord**<br /><br /> 20 : **BuildIndex**<br /><br /> 21 : **Aggregate**<br /><br /> 22 : **BuildDecode**<br /><br /> 23 : **WriteDecode**<br /><br /> 24 : **BuildDMDecode**<br /><br /> 25 : **ExecuteSQL**<br /><br /> 26 : **ExecuteModifiedSQL**<br /><br /> 27 : **Connecting**<br /><br /> 28 : **BuildAggsAndIndexes**<br /><br /> 29 : **MergeAggsOnDisk**<br /><br /> 30 : **BuildIndexForRigidAggs**<br /><br /> 31 : **BuildIndexForFlexibleAggs**<br /><br /> 32 : **WriteAggsAndIndexes**<br /><br /> 33 : **WriteSegment**<br /><br /> 34 : **DataMiningProgress**<br /><br /> 35 : **ReadBufferFullReport**<br /><br /> 36 : **ProactiveCacheConversion**<br /><br /> 37 : **Backup**<br /><br /> 38 : **Restore**<br /><br /> 39 : **Synchronize**<br /><br /> 40 : **Build Processing Schedule**<br /><br /> 41 : **Detach**<br /><br /> 42 : **Attach**<br /><br /> 43 : **Analyze\Encode Data**<br /><br /> 44 : **Compress Segment**<br /><br /> 45 : **Write Table Column**<br /><br /> 46 : **Relationship Build Prepare**<br /><br /> 47 : **Build Relationship Segment**<br /><br /> 48 : **Load**<br /><br /> 49 : **Metadata Load**<br /><br /> 50 : **Data Load**<br /><br /> 51 : **Post Load**<br /><br /> 52 : **Metadata traversal during Backup**<br /><br /> 53 : **VertiPaq**<br /><br /> 54 : **Hierarchy processing**<br /><br /> 55 : **Switching dictionary**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement signalé, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|Contient l'ID de travail associé à l'événement signalé.|  
|SessionType|8|8|Contient le type de session (l'entité ayant déclenché l'événement) associé à l'événement signalé. Pour les événements de traitement, les valeurs sont :<br /><br /> 1= Utilisateur<br /><br /> 2= Mise en cache proactive<br /><br /> 3= Traitement différé|  
|ObjectID|11|8|Contient l'ID d'objet (chaîne) associé à l'événement signalé.|  
|ObjectType|12|1|Contient le type d'objet.|  
|ObjectName|13|8|Contient le nom de l'objet associé à l'événement signalé.|  
|ObjectPath|14|8|Contient le chemin d'accès à l'objet associé à l'événement signalé, sous la forme d'une liste de parents séparés par une virgule commençant par les parents de l'objet.|  
|ObjectReference|15|8|Contient la référence d'objet de l'événement signalé, encodée au format XML pour tous les parents et en utilisant des balises pour décrire l'objet.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement signalé.|  
|DatabaseName|28|8|Contient le nom de la base de données dans laquelle l'événement signalé s'est produit.|  
|NTUserName|32|8|Contient le compte d'utilisateur Windows associé à l'événement signalé.|  
|NTDomainName|33|8|Contient le compte de domaine Windows associé à l'événement signalé.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement signalé.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement signalé. Le SPID correspond directement au GUID de session utilisé par XMLA (XML for Analysis).|  
|TextData|42|9|Contient les données texte associées à l'événement signalé.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement signalé s'est produit.|  
  
## <a name="progress-report-enddata-columns"></a>Progress Report End–Colonnes de données  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements. Les éléments suivants sont valides : paires **Sub Class Id**: **Sub Class Name** :<br /><br /> 1 : **Process**<br /><br /> 2 : **Merge**<br /><br /> 3 : **Delete**<br /><br /> 4 : **DeleteOldAggregations**<br /><br /> 5 : **Rebuild**<br /><br /> 6 : **Commit**<br /><br /> 7 : **Rollback**<br /><br /> 8 : **CreateIndexes**<br /><br /> 9 : **CreateTable**<br /><br /> 10 : **InsertInto**<br /><br /> 11 : **Transaction**<br /><br /> 12 : **Initialize**<br /><br /> 13 : **Discretize**<br /><br /> 14 : **Query**<br /><br /> 15 : **CreateView**<br /><br /> 16 : **WriteData**<br /><br /> 17 : **ReadData**<br /><br /> 18 : **GroupData**<br /><br /> 19 : **GroupDataRecord**<br /><br /> 20 : **BuildIndex**<br /><br /> 21 : **Aggregate**<br /><br /> 22 : **BuildDecode**<br /><br /> 23 : **WriteDecode**<br /><br /> 24 : **BuildDMDecode**<br /><br /> 25 : **ExecuteSQL**<br /><br /> 26 : **ExecuteModifiedSQL**<br /><br /> 27 : **Connecting**<br /><br /> 28 : **BuildAggsAndIndexes**<br /><br /> 29 : **MergeAggsOnDisk**<br /><br /> 30 : **BuildIndexForRigidAggs**<br /><br /> 31 : **BuildIndexForFlexibleAggs**<br /><br /> 32 : **WriteAggsAndIndexes**<br /><br /> 33 : **WriteSegment**<br /><br /> 34 : **DataMiningProgress**<br /><br /> 35 : **ReadBufferFullReport**<br /><br /> 36 : **ProactiveCacheConversion**<br /><br /> 37 : **Backup**<br /><br /> 38 : **Restore**<br /><br /> 39 : **Synchronize**<br /><br /> 40 : **Build Processing Schedule**<br /><br /> 41 : **Detach**<br /><br /> 42 : **Attach**<br /><br /> 43 : **Analyze\Encode Data**<br /><br /> 44 : **Compress Segment**<br /><br /> 45 : **Write Table Column**<br /><br /> 46 : **Relationship Build Prepare**<br /><br /> 47 : **Build Relationship Segment**<br /><br /> 48 : **Load**<br /><br /> 49 : **Metadata Load**<br /><br /> 50 : **Data Load**<br /><br /> 51 : **Post Load**<br /><br /> 52 : **Metadata traversal during Backup**<br /><br /> 53 : **VertiPaq**<br /><br /> 54 : **Hierarchy processing**<br /><br /> 55 : **Switching dictionary**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement signalé, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Contient l'heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Contient le temps (en millisecondes) pris par l’événement.|  
|CPUTime|6|2|Contient le temps processeur (en millisecondes) utilisé par l'événement.|  
|JobID|7|1|Contient l'ID de travail associé à l'événement signalé.|  
|SessionType|8|8|Contient le type de session (l'entité ayant déclenché l'événement) associé à l'événement signalé. Pour les événements de traitement, les valeurs sont :<br /><br /> 1= Utilisateur<br /><br /> 2= Mise en cache proactive<br /><br /> 3= Traitement différé|  
|ProgressTotal|9|1|Contient l'avancement total de l'événement signalé.|  
|IntegerData|10|1|Contient les données entières associées à l'événement signalé, telles que le nombre actuel de lignes traitées pour un événement de traitement.|  
|ObjectID|11|8|Contient l'ID d'objet (chaîne) associé à l'événement signalé.|  
|ObjectType|12|1|Contient le type d'objet.|  
|ObjectName|13|8|Contient le nom de l'objet associé à l'événement signalé.|  
|ObjectPath|14|8|Contient le chemin d'accès à l'objet associé à l'événement signalé, sous la forme d'une liste de parents séparés par une virgule commençant par les parents de l'objet.|  
|ObjectReference|15|8|Contient la référence d'objet de l'événement signalé, encodée au format XML pour tous les parents et en utilisant des balises pour décrire l'objet.|  
|Severity|22|1|Contient le niveau de gravité d'une exception associée à l'événement signalé. Valeurs possibles :<br /><br /> 0 = Réussite<br /><br /> 1 = Informationnelle<br /><br /> 2 = Avertissement<br /><br /> 3 = Erreur|  
|Réussi|23|1|Contient la réussite ou l'échec de l'événement signalé sur le serveur. Valeurs possibles :<br /><br /> 0 = Échec<br /><br /> 1 = Réussite|  
|Erreur|24|1|Contient le numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement signalé.|  
|DatabaseName|28|8|Contient le nom de la base de données dans laquelle l'événement signalé s'est produit.|  
|NTUserName|32|8|Contient le compte d'utilisateur Windows associé à l'événement signalé.|  
|NTDomainName|33|8|Contient le compte de domaine Windows associé à l'événement signalé.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement signalé.|  
|NTCanonicalUserName|40|8|Contient le nom d'utilisateur Windows associé à l'événement signalé. Le nom d'utilisateur a la forme canonique. engineering.microsoft.com/software/user, par exemple.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement signalé. Le SPID correspond directement au GUID de session utilisé par XMLA (XML for Analysis).|  
|TextData|42|9|Contient les données texte associées à l'événement signalé.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement signalé s'est produit.|  
  
## <a name="progress-report-currentdata-columns"></a>Progress Report Current–Colonnes de données  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements. Les éléments suivants sont valides : paires **Sub Class Id**: **Sub Class Name** :<br /><br /> 1 : **Process**<br /><br /> 2 : **Merge**<br /><br /> 3 : **Delete**<br /><br /> 4 : **DeleteOldAggregations**<br /><br /> 5 : **Rebuild**<br /><br /> 6 : **Commit**<br /><br /> 7 : **Rollback**<br /><br /> 8 : **CreateIndexes**<br /><br /> 9 : **CreateTable**<br /><br /> 10 : **InsertInto**<br /><br /> 11 : **Transaction**<br /><br /> 12 : **Initialize**<br /><br /> 13 : **Discretize**<br /><br /> 14 : **Query**<br /><br /> 15 : **CreateView**<br /><br /> 16 : **WriteData**<br /><br /> 17 : **ReadData**<br /><br /> 18 : **GroupData**<br /><br /> 19 : **GroupDataRecord**<br /><br /> 20 : **BuildIndex**<br /><br /> 21 : **Aggregate**<br /><br /> 22 : **BuildDecode**<br /><br /> 23 : **WriteDecode**<br /><br /> 24 : **BuildDMDecode**<br /><br /> 25 : **ExecuteSQL**<br /><br /> 26 : **ExecuteModifiedSQL**<br /><br /> 27 : **Connecting**<br /><br /> 28 : **BuildAggsAndIndexes**<br /><br /> 29 : **MergeAggsOnDisk**<br /><br /> 30 : **BuildIndexForRigidAggs**<br /><br /> 31 : **BuildIndexForFlexibleAggs**<br /><br /> 32 : **WriteAggsAndIndexes**<br /><br /> 33 : **WriteSegment**<br /><br /> 34 : **DataMiningProgress**<br /><br /> 35 : **ReadBufferFullReport**<br /><br /> 36 : **ProactiveCacheConversion**<br /><br /> 37 : **Backup**<br /><br /> 38 : **Restore**<br /><br /> 39 : **Synchronize**<br /><br /> 40 : **Build Processing Schedule**<br /><br /> 41 : **Detach**<br /><br /> 42 : **Attach**<br /><br /> 43 : **Analyze\Encode Data**<br /><br /> 44 : **Compress Segment**<br /><br /> 45 : **Write Table Column**<br /><br /> 46 : **Relationship Build Prepare**<br /><br /> 47 : **Build Relationship Segment**<br /><br /> 48 : **Load**<br /><br /> 49 : **Metadata Load**<br /><br /> 50 : **Data Load**<br /><br /> 51 : **Post Load**<br /><br /> 52 : **Metadata traversal during Backup**<br /><br /> 53 : **VertiPaq**<br /><br /> 54 : **Hierarchy processing**<br /><br /> 55 : **Switching dictionary**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement signalé, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|Contient l'ID de travail associé à l'événement signalé.|  
|SessionType|8|8|Contient le type de session (l'entité ayant déclenché l'événement) associé à l'événement signalé. Pour les événements de traitement, les valeurs sont :<br /><br /> 1= Utilisateur<br /><br /> 2= Mise en cache proactive<br /><br /> 3= Traitement différé|  
|ProgressTotal|9|1|Contient l'avancement total de l'événement signalé.|  
|IntegerData|10|1|Contient les données entières associées à l'événement signalé, telles que le nombre actuel de lignes traitées pour un événement de traitement.|  
|ObjectID|11|8|Contient l'ID d'objet (chaîne) associé à l'événement signalé.|  
|ObjectType|12|1|Contient le type d'objet.|  
|ObjectName|13|8|Contient le nom de l'objet associé à l'événement signalé.|  
|ObjectPath|14|8|Contient le chemin d'accès à l'objet associé à l'événement signalé, sous la forme d'une liste de parents séparés par une virgule commençant par les parents de l'objet.|  
|ObjectReference|15|8|Contient la référence d'objet de l'événement signalé, encodée au format XML pour tous les parents et en utilisant des balises pour décrire l'objet.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement signalé.|  
|DatabaseName|28|8|Contient le nom de la base de données dans laquelle l'événement signalé s'est produit.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement signalé.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement signalé. Le SPID correspond directement au GUID de session utilisé par XMLA (XML for Analysis).|  
|TextData|42|9|Contient les données texte associées à l'événement signalé.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement signalé s'est produit.|  
  
## <a name="progress-report-errordata-columns"></a>Progress Report Error–Colonnes de données  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements. Les éléments suivants sont valides : paires **Sub Class Id**: **Sub Class Name** :<br /><br /> 1 : **Process**<br /><br /> 2 : **Merge**<br /><br /> 3 : **Delete**<br /><br /> 4 : **DeleteOldAggregations**<br /><br /> 5 : **Rebuild**<br /><br /> 6 : **Commit**<br /><br /> 7 : **Rollback**<br /><br /> 8 : **CreateIndexes**<br /><br /> 9 : **CreateTable**<br /><br /> 10 : **InsertInto**<br /><br /> 11 : **Transaction**<br /><br /> 12 : **Initialize**<br /><br /> 13 : **Discretize**<br /><br /> 14 : **Query**<br /><br /> 15 : **CreateView**<br /><br /> 16 : **WriteData**<br /><br /> 17 : **ReadData**<br /><br /> 18 : **GroupData**<br /><br /> 19 : **GroupDataRecord**<br /><br /> 20 : **BuildIndex**<br /><br /> 21 : **Aggregate**<br /><br /> 22 : **BuildDecode**<br /><br /> 23 : **WriteDecode**<br /><br /> 24 : **BuildDMDecode**<br /><br /> 25 : **ExecuteSQL**<br /><br /> 26 : **ExecuteModifiedSQL**<br /><br /> 27 : **Connecting**<br /><br /> 28 : **BuildAggsAndIndexes**<br /><br /> 29 : **MergeAggsOnDisk**<br /><br /> 30 : **BuildIndexForRigidAggs**<br /><br /> 31 : **BuildIndexForFlexibleAggs**<br /><br /> 32 : **WriteAggsAndIndexes**<br /><br /> 33 : **WriteSegment**<br /><br /> 34 : **DataMiningProgress**<br /><br /> 35 : **ReadBufferFullReport**<br /><br /> 36 : **ProactiveCacheConversion**<br /><br /> 37 : **Backup**<br /><br /> 38 : **Restore**<br /><br /> 39 : **Synchronize**<br /><br /> 40 : **Build Processing Schedule**<br /><br /> 41 : **Detach**<br /><br /> 42 : **Attach**<br /><br /> 43 : **Analyze\Encode Data**<br /><br /> 44 : **Compress Segment**<br /><br /> 45 : **Write Table Column**<br /><br /> 46 : **Relationship Build Prepare**<br /><br /> 47 : **Build Relationship Segment**<br /><br /> 48 : **Load**<br /><br /> 49 : **Metadata Load**<br /><br /> 50 : **Data Load**<br /><br /> 51 : **Post Load**<br /><br /> 52 : **Metadata traversal during Backup**<br /><br /> 53 : **VertiPaq**<br /><br /> 54 : **Hierarchy processing**<br /><br /> 55 : **Switching dictionary**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement signalé, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Contient l'heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Contient le temps (en millisecondes) pris par l’événement.|  
|JobID|7|1|Contient l'ID de travail associé à l'événement signalé.|  
|SessionType|8|8|Contient le type de session (l'entité ayant déclenché l'événement) associé à l'événement signalé. Pour les événements de traitement, les valeurs sont :<br /><br /> 1= Utilisateur<br /><br /> 2= Mise en cache proactive<br /><br /> 3= Traitement différé|  
|ProgressTotal|9|1|Contient l'avancement total de l'événement signalé.|  
|IntegerData|10|1|Contient les données entières associées à l'événement signalé, telles que le nombre actuel de lignes traitées pour un événement de traitement.|  
|ObjectID|11|8|Contient l'ID d'objet (chaîne) associé à l'événement signalé.|  
|ObjectType|12|1|Contient le type d'objet.|  
|ObjectName|13|8|Contient le nom de l'objet associé à l'événement signalé.|  
|ObjectPath|14|8|Contient le chemin d'accès à l'objet associé à l'événement signalé, sous la forme d'une liste de parents séparés par une virgule commençant par les parents de l'objet.|  
|ObjectReference|15|8|Contient la référence d'objet de l'événement signalé, encodée au format XML pour tous les parents et en utilisant des balises pour décrire l'objet.|  
|Severity|22|1|Contient le niveau de gravité d'une exception associée à l'événement signalé. Valeurs possibles :<br /><br /> 0 = Réussite<br /><br /> 1 = Informationnelle<br /><br /> 2 = Avertissement<br /><br /> 3 = Erreur|  
|Erreur|24|1|Contient le numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement signalé.|  
|DatabaseName|28|8|Contient le nom de la base de données dans laquelle l'événement signalé s'est produit.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement signalé.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement signalé. Le SPID correspond directement au GUID de session utilisé par XMLA (XML for Analysis).|  
|TextData|42|9|Contient les données texte associées à l'événement signalé.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement signalé s'est produit.|  
  
## <a name="see-also"></a>Voir aussi  
 [Catégorie d’événement de rapports de progression](../../analysis-services/trace-events/progress-reports-event-category.md)  
  
  
