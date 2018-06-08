---
title: Colonnes de données d’événements notification | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bf7695ec7dbbe7361641fb939718724f611e6c1e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="notification-events-data-columns"></a>Colonnes de données d'événements de notification
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les événements de notification sont des événements qui ne sont pas directement générés par les utilisateurs de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Par exemple, des notifications se déclenchent suite à la mise à jour par les utilisateurs des tables sous-jacentes pour la mise en cache proactive.  
  
 La catégorie d'événement Événements de notification a la classe d'événements suivante :  
  
|**ID d'événement**|**Nom d'événement**|**Description de l'événement**|  
|------------------|--------------------|---------------------------|  
|39|Notification|Événement de notification.|  
|40|Définis par l'utilisateur|Événement défini par l'utilisateur.|  
  
 Le tableau suivant répertorie les colonnes de données de la classe d'événements.  
  
## <a name="notification"></a>Notification  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements.<br /><br /> Les paires **ID de sous-classe**:<br />                      **Nom de sous-classe** suivantes sont définies :<br /><br /> 0 : début de la mise en cache proactive<br /><br /> 1 : fin de la mise en cache proactive<br /><br /> 2 : boîte noire démarrée<br /><br /> 3 : boîte noire arrêtée<br /><br /> 4 : propriétés de configuration mises à jour<br /><br /> 5 : trace SQL<br /><br /> 6 : objet créé<br /><br /> 7 : objet supprimé<br /><br /> 8 : objet modifié<br /><br /> 9 : début de l’interrogation de la mise en cache proactive<br /><br /> 10 : fin de l’interrogation de la mise en cache proactive<br /><br /> 11 : début de l’instantané de la boîte noire<br /><br /> 12 : fin de l’instantané de la boîte noire<br /><br /> 13 : mise en cache proactive : objet notifiable mis à jour<br /><br /> 14 : traitement différé : début du traitement<br /><br /> 15 : traitement différé : traitement terminé<br /><br /> 16 : début de l’événement SessionOpened<br /><br /> 17 : fin de l’événement SessionOpened<br /><br /> 18 : début de l’événement SessionClosing<br /><br /> 19 : fin de l’événement SessionClosing<br /><br /> 20 : début de l’événement CubeOpened<br /><br /> 21 : fin de l’événement CubeOpened<br /><br /> 22 : début de l’événement CubeClosing<br /><br /> 23 : fin de l’événement CubeClosing<br /><br /> 24 : arrêt de transaction demandé|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement de notification, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Contient l'heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Contient le temps (en millisecondes) utilisé par l'événement.|  
|IntegerData|10|1|Contient les données entières associées à l'événement de notification. Lorsque la colonne EventSubclass contient la valeur 8, les valeurs sont :<br /><br /> 1 = Créé<br /><br /> 2 = Supprimé<br /><br /> 3 = Propriétés de l'objet modifié<br /><br /> 4 = Propriétés modifiées de l'enfant de l'objet<br /><br /> 6 = Enfant ajouté<br /><br /> 7 = Enfant supprimé<br /><br /> 8 = Objet complètement traité<br /><br /> 9 = Objet partiellement traité<br /><br /> 10 = Objet non traité<br /><br /> 11 = Objet complètement optimisé<br /><br /> 12 = Objet partiellement optimisé<br /><br /> 13 = Objet non optimisé|  
|ObjectID|11|8|Contient l'ID d'objet pour lequel cette notification est émise ; il s'agit d'une valeur de chaîne.|  
|ObjectType|12|1|Contient le type d'objet associé à l'événement de notification.|  
|ObjectName|13|8|Contient le nom d'objet associé à l'événement de notification.|  
|ObjectPath|14|8|Contient le chemin d'accès à l'objet associé à l'événement de notification. Le chemin d'accès est retourné sous la forme d'une liste de parents séparés par une virgule commençant par le parent de l'objet.|  
|ObjectReference|15|8|Contient la référence d'objet de l'événement de fin du rapport de progression. La référence d'objet est encodée en langage XML en fonction de tous les parents en utilisant des balises pour décrire l'objet.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement de notification.|  
|DatabaseName|28|8|Contient le nom de la base de données dans laquelle l'événement de notification s'est produit.|  
|NTUserName|32|8|Contient le nom d'utilisateur Windows associé à l'événement de notification.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement de notification.|  
|NTCanonicalUserName|40|8|Contient le nom d'utilisateur Windows associé à l'événement de notification. Le nom d'utilisateur a la forme canonique. engineering.microsoft.com/software/user, par exemple.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID, Server Process ID) qui identifie de manière unique la session utilisateur associée à l'événement de notification. Le SPID correspond directement au GUID de session utilisé par XMLA.|  
|TextData|42|9|Contient les données texte associées à l'événement de notification.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement de notification s'est produit.|  
|RequestProperties|45|9|Contient les propriétés de la demande XMLA.|  
  
## <a name="user-defined"></a>Définis par l'utilisateur  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|Sous-classe d'événement spécifique de l'utilisateur qui fournit des informations supplémentaires sur chaque classe d'événements.|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement de notification, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|IntegerData|10|1|Informations d'événement définies par l'utilisateur.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement de notification.|  
|DatabaseName|28|8|Contient le nom de la base de données dans laquelle l'événement de notification s'est produit.|  
|NTUserName|32|8|Contient le nom d'utilisateur Windows associé à l'événement de notification.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement de notification.|  
|NTCanonicalUserName|40|8|Contient le nom d'utilisateur Windows associé à l'événement de notification. Le nom d'utilisateur a la forme canonique. engineering.microsoft.com/software/user, par exemple.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID, Server Process ID) qui identifie de manière unique la session utilisateur associée à l'événement de notification. Le SPID correspond directement au GUID de session utilisé par XMLA.|  
|TextData|42|9|Contient les données texte associées à l'événement de notification.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement de notification s'est produit.|  
  
## <a name="see-also"></a>Voir aussi  
 [Catégorie d’événement notification Events](../../analysis-services/trace-events/notification-events-event-category.md)  
  
  
