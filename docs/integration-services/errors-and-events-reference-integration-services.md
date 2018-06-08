---
title: Guide de référence des erreurs et des événements (Integration Services) Microsoft Docs
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
- Integration Services packages, events
- events [Integration Services]
- errors [Integration Services]
- Integration Services, errors
ms.assetid: cf4f0f14-8087-42d7-9b67-e4929228abd6
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6135e7e65c10d375d6a59529bd1beb3369a8d084
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errors-and-events-reference-integration-services"></a>Guide de référence des erreurs et des événements (Integration Services)
  Cette section de la documentation contient des informations sur plusieurs erreurs et événements liés à [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Les informations sur les causes et les solutions sont incluses pour les messages d'erreur.  
  
 Pour plus d’informations sur les messages d’erreur de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , notamment une liste de la plupart des erreurs de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] ainsi que leur description, consultez [Guide de référence des erreurs et des messages propres à Integration Services](../integration-services/integration-services-error-and-message-reference.md). Toutefois, la liste n'inclut actuellement aucune information de dépannage.  
  
> [!IMPORTANT]  
>  De nombreuses erreurs que vous pouvez rencontrer lors de l’utilisation de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] proviennent d’autres composants. Il peut s’agir par exemple de fournisseurs OLE DB, d’autres composants de base de données, comme [!INCLUDE[ssDE](../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , ou d’autres services ou composants, comme le système de fichiers, le serveur SMTP ou Microsoft Message Queueing. Pour trouver des informations sur ces messages d'erreur externes, consultez la documentation spécifique au composant.  
  
## <a name="error-messages"></a>Messages d'erreur  
  
|Nom symbolique de l'erreur|Description|  
|----------------------------|-----------------|  
|DTS_E_CACHELOADEDFROMFILE|Indique que le package ne peut pas s'exécuter en raison de la tentative d'écriture d'une transformation du cache dans le cache en mémoire. Toutefois, un Gestionnaire de connexions du cache a déjà chargé un fichier cache dans le cache en mémoire.|  
|DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER|Indique que le package ne peut pas s'exécuter parce qu'une connexion spécifiée a échoué.|  
|DTS_E_CANNOTCONVERTBETWEENUNICODEANDNONUNICODESTRINGCOLUMN|Indique qu'un composant de flux de données tente de transmettre des données de chaîne Unicode à un autre composant qui attend des données de chaîne non-Unicode dans la colonne correspondante, ou inversement.|  
|DTS_E_CANNOTCONVERTBETWEENUNICODEANDNONUNICODESTRINGCOLUMNS|Indique qu'un composant de flux de données tente de transmettre des données de chaîne Unicode à un autre composant qui attend des données de chaîne non-Unicode dans la colonne correspondante, ou inversement.|  
|DTS_E_CANTINSERTCOLUMNTYPE|Indique qu’il est impossible d’ajouter la colonne à la table de la base de données car la conversion entre le type de données de la colonne [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et le type de données de la colonne de base de données n’est pas prise en charge.|  
|DTS_E_CONNECTIONNOTFOUND|Indique que le package ne peut pas s'exécuter parce que le gestionnaire de connexions spécifié est introuvable.|  
|DTS_E_CONNECTIONREQUIREDFORMETADATA|Indique que le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] doit se connecter à une source de données pour récupérer de nouvelles métadonnées ou des métadonnées mises à jour pour une source ou une destination, et qu’il n’est pas en mesure de se connecter à la source de données.|  
|DTS_E_MULTIPLECACHEWRITES|Indique que le package ne peut pas s'exécuter en raison de la tentative d'écriture d'une transformation du cache dans le cache en mémoire. Toutefois, une autre transformation du cache a déjà écrit dans le cache en mémoire.|  
|DTS_E_PRODUCTLEVELTOLOW|Indique que le package ne peut pas s’exécuter car la version appropriée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] n’est pas installée.|  
|DTS_E_READNOTFILLEDCACHE|Indique qu'une transformation de recherche tente de lire des données dans le cache en mémoire en même temps que la transformation du cache écrit les données dans le cache.|  
|DTS_E_UNPROTECTXMLFAILED|Indique que le système n'a pas déchiffré un nœud XML protégé.|  
|DTS_E_WRITEWHILECACHEINUSE|Indique qu'une transformation de cache tente d'écrire des données dans le cache en mémoire en même temps qu'une transformation de recherche lit les données dans le cache en mémoire.|  
|DTS_W_EXTERNALMETADATACOLUMNSOUTOFSYNC|Indique que les métadonnées de colonne dans la source de données ne correspondent pas aux métadonnées de colonne dans le composant source ou de destination connecté à la source de données.|  
  
## <a name="events-sqlispackage"></a>Événements (SQLISPackage)  
 Pour plus d’informations, consultez [Événements journalisés par un package Integration Services](../integration-services/performance/events-logged-by-an-integration-services-package.md).  
  
|Événement|Description|  
|-----------|-----------------|  
|SQLISPackage_12288|Indique qu'un package a démarré.|  
|SQLISPackage_12289|Indique qu'un package a terminé son exécution avec succès.|  
|SQLISPACKAGE_12291|Indique qu'un package n'a pas pu terminer son exécution et s'est arrêté.|  
|SQLISPackage_12546|Indique qu'une tâche ou tout autre exécutable dans un package a fini son travail.|  
|SQLISPackage_12549|Indique qu'un message d'avertissement a été déclenché dans un package.|  
|SQLISPackage_12550|Indique qu'un message d'erreur a été déclenché dans un package.|  
|SQLISPackage_12551|Indique qu'un package n'a pas fini son travail et s'est interrompu.|  
|SQLISPackage_12557|Indique qu'un package a terminé son exécution.|  
  
## <a name="events-sqlisservice"></a>Événements (SQLISService)  
 Pour plus d’informations, consultez [Événements journalisés par le service Integration Services](../integration-services/service/events-logged-by-the-integration-services-service.md).  
  
|Événement|Description|  
|-----------|-----------------|  
|SQLISService_256|Indique que le service est sur le point de démarrer.|  
|SQLISService_257|Indique que le service a démarré.|  
|SQLISService_258|Indique que le service est sur le point de s'arrêter.|  
|SQLISService_259|Indique que le service s'est arrêté.|  
|SQLISService_260|Indique que le service a essayé de démarrer, en vain.|  
|SQLISService_272|Indique que le fichier de configuration n'existe pas à l'emplacement spécifié.|  
|SQLISService_273|Indique que le fichier de configuration n'a pas pu être lu ou n'est pas valide.|  
|SQLISService_274|Indique que l'entrée de Registre qui contient l'emplacement du fichier de configuration n'existe pas ou est vide.|  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des messages propres à Integration Services](../integration-services/integration-services-error-and-message-reference.md)  
  
  
