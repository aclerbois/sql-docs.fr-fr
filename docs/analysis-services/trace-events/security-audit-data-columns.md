---
title: Colonnes de données d’Audit de sécurité | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 06bb8b1c6906ebbced1ed7cf52d254564c77d084
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="security-audit-data-columns"></a>Colonnes de données Audit de sécurité
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La catégorie d'événement Audit de sécurité contient les classes d'événements suivantes :  
  
||||  
|-|-|-|  
|**ID d'événement**|**Nom d'événement**|**Description de l'événement**|  
|1|Audit Login|Collecte tous les événements de connexion depuis le début de la trace, telle qu'une demande de connexion d'un client à un serveur qui exécute une instance de SQL Server.|  
|2|Audit Logout|Collecte tous les nouveaux événements de déconnexion depuis le début de la trace, telle que l'émission par un client d'une commande de déconnexion.|  
|4|Audit Server Starts And Stops|Enregistre l'arrêt du service, le début et la suspension des activités des services.|  
|18|Audit Object Permission Event|Enregistre les modifications d'autorisations sur les objets.|  
|19|Audit Admin Operations Event|Enregistre la sauvegarde/la restauration/la synchronisation/l'attachement/le détachement/le chargement d'image/l'enregistrement d'image.|  
  
 Les tableaux suivants répertorient les colonnes de données de chacune de ces classes d'événements.  
  
## <a name="audit-login"></a>Audit Login  
  
|||||  
|-|-|-|-|  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|ClientHostName|35|8|Nom de l'ordinateur sur lequel le client est exécuté. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|ApplicationName|37|8|Nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="audit-logout"></a>Audit Logout  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|ConnectionID|25|1|ID de connexion unique.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|ClientHostName|35|8|Nom de l'ordinateur sur lequel le client est exécuté. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|ApplicationName|37|8|Nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="audit-server-starts-and-stops"></a>Audit Server Starts And Stops  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : Arrêt de l’instance<br /><br /> 2 : Instance démarrée<br /><br /> 3 : Instance suspendue<br /><br /> 4 : Poursuite de l’instance|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="audit-object-permission-event"></a>Audit Object Permission Event  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ObjectReference|15|8|Référence de l'objet. Encodée au format XML pour tous les parents, en utilisant des balises pour décrire l'objet.|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|ClientHostName|35|8|Nom de l'ordinateur sur lequel le client est exécuté. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|ApplicationName|37|8|Nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="audit-admin-operations-event"></a>Audit Admin Operations Event  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : **Backup**<br /><br /> 2 : **Restore**<br /><br /> 3 : **Synchronize**<br /><br /> 4 : **Detach**<br /><br /> 5 : **Attach**<br /><br /> 6 : **ImageLoad**<br /><br /> 7 : **ImageSave**|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|ClientHostName|35|8|Nom de l'ordinateur sur lequel le client est exécuté. Cette colonne de données est remplie si le nom de l'hôte est fourni par le client.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|ApplicationName|37|8|Nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Catégorie d'événement Audit de sécurité](../../analysis-services/trace-events/security-audit-event-category.md)  
  
  
