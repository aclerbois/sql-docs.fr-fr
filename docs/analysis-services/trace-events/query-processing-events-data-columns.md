---
title: Colonnes de données des événements de traitement des requêtes | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8c0222c19d1f21e130f342f689ebb72cc8c0fda7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="query-processing-events-data-columns"></a>Colonnes de données des événements de traitement des requêtes
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La catégorie d'événement Événements de traitement de requête contient les classes d'événements suivantes :  
  
|**ID d'événement**|**Nom d'événement**|**Description de l'événement**|  
|------------------|--------------------|---------------------------|  
|70|Query Cube Begin|Interroger le cube Début.|  
|71|Query Cube End|Interroger le cube Fin.|  
|72|Calculate Non Empty Begin|Calculer non vide Début.|  
|73|Calculate Non Empty Current|Calculer non vide En cours.|  
|74|Calculate Non Empty End|Calculer non vide Fin.|  
|75|Serialize Results Begin|Sérialiser résultats Début.|  
|76|Serialize Results Current|Sérialiser résultats En cours.|  
|77|Serialize Results End|Sérialiser résultats Fin.|  
|78|Execute MDX Script Begin|Exécuter script MDX Début.|  
|79|Execute MDX Script Current|Exécuter script MDX En cours. Déconseillé.|  
|80|Execute MDX Script End|Exécuter script MDX Fin.|  
|81|Query Dimension|Dimension de requête.|  
|11|Query Subcube|Requête sur un sous-cube, pour une optimisation basée sur l'utilisation.|  
|12|Query Subcube Verbose|Requête sur un sous-cube avec des informations détaillées. Cet événement peut avoir un impact négatif sur la performance en cas d'activation.|  
|60|Get Data From Aggregation|Répondre à la requête en obtenant des données à partir de l'agrégation. Cet événement peut avoir un impact négatif sur la performance en cas d'activation.|  
|61|Get Data From Cache|Répondre à la requête en obtenant des données à partir de l'un des caches. Cet événement peut avoir un impact négatif sur la performance en cas d'activation.|  
|82|VertiPaq SE Query Begin|Requête VertiPaq SE|  
|83|VertiPaq SE Query End|Requête VertiPaq SE|  
|84|Utilisation des ressources|Lectures de rapports, écritures, utilisation de l'UC après la fin des commandes et des requêtes.|  
|85 %|VertiPaq SE Query Cache Match|Utilisation du cache de requêtes VertiPaq SE|  
|98|Direct Query Begin|Début de requête directe.|  
|99|Direct Query End|Fin de requête directe.|  
  
 Les tableaux suivants répertorient les colonnes de données de chacune de ces classes d'événements.  
  
## <a name="query-cube-begin"></a>Query Cube Begin  
  
|||||  
|-|-|-|-|  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="query-cube-end"></a>Query Cube End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="calculate-non-empty-begin"></a>Calculate Non Empty Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="calculate-non-empty-current"></a>Calculate Non Empty Current  
  
|||||  
|-|-|-|-|  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : Get Data<br /><br /> 2 : Process Calculated Members<br /><br /> 3 : Post Order|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="calculate-non-empty-end"></a>Calculate Non Empty End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="serialize-results-begin"></a>Serialize Results Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="serialize-results-current"></a>Serialize Results Current  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements.<br /><br /> 1 : Serialize Axes<br /><br /> 2 : Serialize Cells<br /><br /> 3 : Serialize SQL Rowset<br /><br /> 4 : Serialize Flattened Rowset|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="serialize-results-end"></a>Serialize Results End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="execute-mdx-script-begin"></a>Execute MDX Script Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : MDX Script<br /><br /> 2 : MDX Script Command|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="execute-mdx-script-current"></a>Execute MDX Script Current  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="execute-mdx-script-end"></a>Execute MDX Script End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : MDX Script<br /><br /> 2 : MDX Script Command|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="query-dimension"></a>Query Dimension  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements.<br /><br /> 1 : Données du cache<br /><br /> 2 : Données non mises en cache|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="query-subcube"></a>Query Subcube  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : Données du cache<br /><br /> 2 : Données non mises en cache<br /><br /> 3 : Données internes<br /><br /> 4 : Données SQL<br /><br /> 11 : Modification structurelle du groupe de mesures<br /><br /> 12 : Suppression du groupe de mesures|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="query-subcube-verbose"></a>Query Subcube Verbose  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 21 : Données du cache<br /><br /> 22 : Données non mises en cache<br /><br /> 23 : Données internes<br /><br /> 24 : Données SQL|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="get-data-from-aggregation"></a>Get Data From Aggregation  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="get-data-from-cache"></a>Get Data From Cache  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements.<br /><br /> 1: Obtenir les données depuis le cache du groupe de mesures<br /><br /> 2 : Obtenir les données depuis le cache plat<br /><br /> 3 : Obtenir les données depuis le cache de calcul<br /><br /> 4 : Obtenir les données depuis le cache persistant|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="vertipaq-se-query-begin"></a>VertiPaq SE Query Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 0 : Analyse VertiPaq<br /><br /> 1 : Requête tabulaire<br /><br /> 2 : Requête de traitement de la hiérarchie utilisateur<br /><br /> 10 : Analyse VertiPaq interne<br /><br /> 11 : Requête tabulaire interne<br /><br /> 12 : Requête de traitement de la hiérarchie utilisateur interne|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ObjectReference|15|8|Référence de l'objet. Encodée au format XML pour tous les parents, en utilisant des balises pour décrire l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="vertipaq-se-query-end"></a>VertiPaq SE Query End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d'événements fournit des informations supplémentaires sur chaque classe d'événements.<br /><br /> 0 : Analyse VertiPaq<br /><br /> 1 : Requête tabulaire<br /><br /> 10 : Analyse VertiPaq interne<br /><br /> 11 : Requête tabulaire interne|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ProgressTotal|9|1|Total de progression.|  
|IntegerData|10|1|Données de type Integer (entier).|  
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
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="resource-usage"></a>Utilisation des ressources  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|ClientProcessID|36|1|ID de processus de l'application cliente.|  
|ApplicationName|37|8|Nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="vertipaq-se-query-cache-match"></a>VertiPaq SE Query Cache Match  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 0 :Correspondance exacte du cache VertiPaq|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|JobID|7|1|ID du travail pour la progression.|  
|SessionType|8|8|Type de session (entité qui a provoqué l'opération).|  
|ObjectID|11|8|ID d'objet (notez il s'agit d'une chaîne).|  
|ObjectType|12|1|Type d'objet.|  
|ObjectName|13|8|Nom de l'objet.|  
|ObjectPath|14|8|Chemin d'accès de l'objet. Liste de parents séparés par une virgule, commençant par le parent de l'objet.|  
|ObjectReference|15|8|Référence de l'objet. Encodée au format XML pour tous les parents, en utilisant des balises pour décrire l'objet.|  
|ConnectionID|25|1|ID de connexion unique.|  
|DatabaseName|28|8|Nom de la base de données dans laquelle l'instruction de l'utilisateur s'exécute.|  
|NTUserName|32|8|Nom d'utilisateur Windows.|  
|NTDomainName|33|8|Domaine Windows auquel appartient l'utilisateur.|  
|SessionID|39|8|GUID de session.|  
|NTCanonicalUserName|40|8|Nom d'utilisateur sous forme canonique. Par exemple, engineering.microsoft.com/software/someone.|  
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="direct-query-begin"></a>Direct Query Begin  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
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
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="direct-query-end"></a>Direct Query End  
  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|---------------------|-------------------|---------------------|----------------------------|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|CurrentTime|2|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Heure à laquelle a débuté l'événement, si disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Durée (en millisecondes) de l'événement.|  
|CPUTime|6|2|Temps processeur (en millisecondes) utilisé par l'événement.|  
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
|SPID|41|1|ID de processus serveur. Cela identifie de façon unique une session utilisateur. Le SPID correspond directement au GUID de session utilisé par XML/A.|  
|TextData|42|9|Données texte associées à l'événement.|  
|ServerName|43|8|Nom du serveur produisant l'événement.|  
  
## <a name="see-also"></a>Voir aussi  
 [Catégorie d’événements de traitement de requête](../../analysis-services/trace-events/query-processing-events-category.md)  
  
  
