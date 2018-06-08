---
title: Performances de l’intégration du CLR | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], performance
- common language runtime [SQL Server], compilation process
- performance [CLR integration]
ms.assetid: 7ce2dfc0-4b1f-4dcb-a979-2c4f95b4cb15
caps.latest.revision: 43
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: ad749572b54e76c751002db3516fdf46ce31f729
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-integration-architecture----performance"></a>Architecture d’intégration CLR - performances
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique aborde certains choix de conception qui améliorent les performances de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégration avec le [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework common language runtime (CLR).  
  
## <a name="the-compilation-process"></a>Processus de compilation  
 Pendant la compilation d'expressions SQL, en cas de référence à une routine managée, un stub MSIL ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Intermediate Language) est généré. Ce stub inclut le code pour marshaler les paramètres de routine à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers le CLR, appeler la fonction et retourner le résultat. Ce code de type glue est basé sur le type de paramètre et sur la direction du paramètre (in, out ou reference).  
  
 Le code de type glue active les optimisations spécifiques au type et garantit une application efficace de la sémantique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telle que la possibilité de valeur NULL, les facettes de contrainte et la gestion des exceptions par valeur et standard. En générant le code pour les types exacts des arguments, vous évitez le conflit de types ou les coûts de création d'objet de wrapper (ou « conversion boxing ») à travers la limite d'appel.  
  
 Le stub généré est ensuite compilé en code natif et optimisé pour l'architecture matérielle particulière sur laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, à l'aide des services de compilation JIT (juste-à-temps) du CLR. Les services JIT sont appelés au niveau de la méthode et permettent à l'environnement d'hébergement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de créer une seule unité de compilation qui couvre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l'exécution du CLR. Une fois le stub compilé, le pointeur fonction résultant devient l'implémentation à l'exécution de la fonction. Cette solution de génération du code garantit qu'il n'existe pas de coûts d'appel supplémentaires en rapport avec la réflexion ou l'accès aux métadonnées au moment de l'exécution.  
  
### <a name="fast-transitions-between-sql-server-and-clr"></a>Transitions rapides entre SQL Server et CLR  
 Le processus de compilation génère une fonction pointeur qui peut être appelée au moment de l'exécution à partir du code natif. Dans le cas de fonctions scalaires définies par l'utilisateur, cet appel de fonction se produit ligne par ligne. Pour réduire le coût de la transition entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR, les instructions qui contiennent un appel managé possèdent une étape de démarrage pour identifier le domaine d'application cible. Cette étape d'identification réduit le coût de la transition pour chaque ligne.  
  
## <a name="performance-considerations"></a>Considérations relatives aux performances  
 Les éléments suivants résument les considérations sur les performances spécifiques à l'intégration du CLR dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous trouverez des informations plus détaillées dans «[à l’aide de l’intégration du CLR dans SQL Server 2005](http://go.microsoft.com/fwlink/?LinkId=50332)» sur le site Web MSDN. Vous trouverez des informations générales relatives aux performances du code managé dans «[Improving .NET Application Performance et évolutivité](http://go.microsoft.com/fwlink/?LinkId=50333)» sur le site Web MSDN.  
  
### <a name="user-defined-functions"></a>Fonctions définies par l'utilisateur  
 Les fonctions CLR tirent parti d'un chemin d'accès d'appel plus rapide que celui des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] définies par l'utilisateur. En outre, le code managé possède un avantage décisif en termes de performances par rapport à [!INCLUDE[tsql](../../includes/tsql-md.md)] quant au code procédural, au calcul et à la manipulation de chaînes. Les fonctions CLR gourmandes en calculs et n'effectuant pas d'accès aux données sont mieux écrites en code managé. Toutefois, les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutent l'accès aux données plus efficacement que l'intégration du CLR.  
  
### <a name="user-defined-aggregates"></a>Agrégats définis par l'utilisateur  
 Le code managé se révèle considérablement plus performant que l'agrégation à base de curseur. En principe, le code managé s'exécute légèrement moins vite que les fonctions d'agrégation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégrées. S'il existe une fonction d'agrégation intégrée native, il est recommandé de l'utiliser. Dans les cas où l'agrégation nécessaire n'est pas prise en charge en mode natif, choisissez, si possible, un agrégat CLR défini par l'utilisateur de préférence à une implémentation à base de curseur pour des raisons de performance.  
  
### <a name="streaming-table-valued-functions"></a>Fonctions table en continu  
 Les applications doivent souvent retourner une table comme résultat de l'appel d'une fonction. Les exemples incluent la lecture de données tabulaires à partir d'un fichier dans le cadre d'une opération d'importation et la conversion de valeurs séparées par des virgules dans le cas d'une représentation relationnelle. En général, vous pouvez accomplir ces actions en matérialisant la table de résultats et en la remplissant avant qu'elle ne puisse être consommée par l'appelant. L'intégration du CLR dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introduit un nouveau mécanisme d'extensibilité appelé fonction table en continu. Les fonctions table en continu offrent de meilleures performances que les implémentations de procédure stockée étendue comparables.  
  
 Table en continu est des fonctions managées qui retournent un **IEnumerable** interface. **IEnumerable** a des méthodes pour naviguer dans le jeu de résultats retourné par la fonction table en continu. Lorsque la fonction table en continu est appelé, retourné **IEnumerable** est directement connecté au plan de requête. Le plan de requête appelle **IEnumerable** méthodes lorsqu’il a besoin extraire les lignes. Ce modèle d'itération permet que les résultats soient consommés immédiatement après que la première ligne a été créée, au lieu d'attendre que la totalité de la table soit remplie. Il réduit aussi considérablement la mémoire consommée en appelant la fonction.  
  
### <a name="arrays-vs-cursors"></a>Différences entre les tableaux et les Curseurs  
 Lorsque les curseurs [!INCLUDE[tsql](../../includes/tsql-md.md)] doivent parcourir des données qui sont plus aisément exprimées en tableau, le code managé peut être utilisé avec des gains de performance significatifs.  
  
### <a name="string-data"></a>Données de type chaîne  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] données de type caractère tel que **varchar**, peut être du type SqlString ou SqlChars dans les fonctions managées. Les variables SqlString créent une instance de la valeur entière en mémoire. Les variables SqlChars fournissent une interface multidiffusion qui peut être utilisée pour obtenir de meilleures performances et une meilleure évolutivité en ne créant pas d'instance de la totalité de la valeur en mémoire. Ce point est particulièrement important pour les données LOB. En outre, les données XML du serveur est accessible via une interface de diffusion en continu renvoyée par **SqlXml.CreateReader()**.  
  
### <a name="clr-vs-extended-stored-procedures"></a>Différences entre le CLR et les Procédures stockées étendues  
 Les API Microsoft.SqlServer.Server (API) qui permettent aux procédures managées de renvoyer des jeux de résultats au client s'exécutent mieux que les API ODS (Open Data Services) utilisées par les procédures stockées étendues. En outre, les System.Data.SqlServer APIs prise en charge des types de données tels que **xml**, **varchar (max)**, **nvarchar (max)**, et **varbinary (max)**, introduite dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], tandis que les API ODS n’ont pas été étendues pour prendre en charge de nouveaux types de données.  
  
 Avec le code managé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère l'utilisation de ressources telles que la mémoire, les threads et la synchronisation. La raison en est que les API managées qui exposent ces ressources sont implémentées sur le gestionnaire de ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Inversement, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a ni vue ou contrôle sur l'utilisation des ressources de la procédure stockée étendue. Par exemple, si une procédure stockée étendue consomme une quantité trop importante d'UC ou de ressources mémoire, il n'existe aucun moyen de le détecter ou de le contrôler avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Avec le code managé, toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut détecter qu'un thread donné n'a rien produit pendant une longue période de temps, puis forcer la tâche à être abandonnée afin qu'un autre travail puisse être planifié. Par conséquent, l'utilisation du code managé offre une meilleure évolutivité et une meilleure utilisation des ressources système.  
  
 Le code managé peut impliquer une charge mémoire supplémentaire nécessaire pour maintenir l'environnement d'exécution et effectuer les contrôles de sécurité. Par exemple, tel est le cas lors de l'exécution dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et que de nombreuses transitions du code managé vers le code natif sont requises (parce que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit effectuer une maintenance supplémentaire sur les paramètres spécifiques au thread lors du déplacement vers ou depuis le code natif). Par conséquent, les procédures stockées étendues peuvent offrir de bien meilleures performances que l'exécution du code managé à l'intérieur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans les cas où il existe de fréquentes transitions entre le code managé et le code natif.  
  
> [!NOTE]  
>  Il est recommandé de ne pas développer de nouvelles procédures stockées étendues, parce que cette fonctionnalité a été déconseillée.  
  
### <a name="native-serialization-for-user-defined-types"></a>Sérialisation native pour les types définis par l'utilisateur  
 Les types définis par l'utilisateur (UDT) sont conçus comme mécanisme d'extensibilité du système de types scalaires. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implémente un format de sérialisation pour les UDT appelé **Format.Native**. Pendant la compilation, la structure du type est examinée pour générer un langage MSIL personnalisé pour cette définition de type particulière.  
  
 La sérialisation native est l'implémentation par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La sérialisation définie par l'utilisateur appelle une méthode définie par le créateur du type pour effectuer la sérialisation. **Format.Native** la sérialisation doit être utilisée lorsque cela est possible pour de meilleures performances.  
  
### <a name="normalization-of-comparable-udts"></a>Normalisation d'UDT comparables  
 Les opérations relationnelles, telles que le tri et la comparaison d'UDT, fonctionnent directement sur la représentation binaire de la valeur. Cette tâche s'effectue en stockant une représentation normalisée (classement binaire) de l'état de l'UDT sur le disque.  
  
 La normalisation présente deux avantages : elle rend la comparaison considérablement moins onéreuse en évitant la construction de l'instance du type et les charges mémoire liées à l'appel de méthode ; de plus, elle crée un domaine binaire pour l'UDT, en permettant la construction d'histogrammes, d'index et d'histogrammes pour les valeurs du type. Par conséquent, les UDT normalisés offrent un profil de performance très similaire aux types intégrés natifs pour les opérations qui n'impliquent pas d'appel de méthode.  
  
### <a name="scalable-memory-usage"></a>Utilisation de la mémoire évolutive  
 Pour que le garbage collection managé s'effectue et évolue correctement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], évitez une allocation importante et unique. Les allocations d'une taille supérieure à 88 Ko sont placées sur le tas des objets volumineux, ce qui provoque une exécution du garbage collection et une évolution bien moins performantes que nombre d'allocations plus réduites. Par exemple, si vous devez allouer un grand tableau multidimensionnel, il est préférable d'allouer un tableau en escalier.  
  
## <a name="see-also"></a>Voir aussi  
 [Types CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)  
  
  
