---
title: Chargement de fichier et enregistrer des colonnes de données | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 33567f7a2e3a1e7f6402fd47994067037e4d3ccf
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="file-load-and-save-data-columns"></a>Colonnes de données File Load and Save
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La catégorie d'événement File Load and Save contient les classes d'événements décrites dans le tableau ci-dessous.  
  
|**ID d'événement**|**Nom d'événement**|**Description de l'événement**|  
|------------------|--------------------|---------------------------|  
|90|File Load Begin|Début du chargement du fichier.|  
|91|File Load End|Fin du chargement du fichier.|  
|92|File Save Begin|Début de l'enregistrement du fichier.|  
|93|File Save End|File Save End|  
|94|PageOut Begin|Début PageOut.|  
|95|PageOut End|PageOut End|  
|96|PageIn Begin|Début PageIn.|  
|97|PageIn End|PageIn End|  
  
 Le tableau suivant répertorie les colonnes de données de cette classe d'événements.  
  
## <a name="file-load-begin"></a>File Load Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="file-load-end"></a>File Load End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="file-save-begin"></a>File Save Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="file-save-end"></a>File Save End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="pageout-begin"></a>PageOut Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="pageout-end"></a>PageOut End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="pagein-begin"></a>PageIn Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="pagein-end"></a>PageIn End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|Severity|22|1|Niveau de gravité d'une exception.|  
|Réussi|23|1|1 = réussite. 0 = échec (1 signifie la réussite de la vérification d'autorisations et 0 l'échec de cette vérification, par exemple).|  
|Erreur|24|1|Numéro d'erreur d'un événement donné.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|SessionID|39|8|GUID de session.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Chargement de fichier et enregistrement de catégorie d’événement](../../analysis-services/trace-events/file-load-and-save-event-category.md)  
  
  
