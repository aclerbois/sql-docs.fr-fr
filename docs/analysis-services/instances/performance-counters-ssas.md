---
title: Les compteurs de performance (SSAS) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 79b4ecc40d69e8f40a5a1612985477d8ee6f166a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="performance-counters-ssas"></a>Compteurs de performance (SSAS)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'Analyseur de performances vous permet d'analyser les performances d'une instance Microsoft SQL Server Analysis Services (SSAS) à l'aide de compteurs de performance.  
  
 L'Analyseur de performances est un composant logiciel enfichable MMC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console) qui assure le suivi de l'utilisation des ressources. Pour démarrer ce composant logiciel enfichable, tapez **PerfMon** à l’invite de commandes ou dans le Panneau de configuration, cliquez sur **Outils d’administration**, puis sur **Analyseur de performances**. L'Analyseur de performances vous permet d'assurer le suivi des performances et de l'activité du serveur et du traitement à l'aide d'objets et de compteurs prédéfinis et de surveiller les événements à l'aide de compteurs définis par l'utilisateur. L'Analyseur de performances collecte des comptes, et non des données, relatifs aux événements (par exemple, sur l'utilisation de la mémoire, le nombre de transactions actives ou l'activité de l'UC.) Vous pouvez également définir des seuils pour des compteurs spécifiques de manière à générer des alertes pour avertir les opérateurs.  
  
 L'Analyseur de performances analyse les instances locales et distantes d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Utilisation de l'Analyseur de performances](http://technet.microsoft.com/library/cc749115.aspx).  
  
 Pour afficher la description de l'un des compteurs pouvant être utilisés avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], dans l'Analyseur de performances, ouvrez la boîte de dialogue **Ajouter des compteurs** , sélectionnez un objet de performance, puis cliquez sur **Afficher la description**. Les compteurs les plus importants sont les compteurs relatifs à l'utilisation de l'UC, à l'utilisation de la mémoire et à l'activité des disques. Nous vous recommandons de commencer par ces compteurs importants et de passer à des compteurs plus détaillés une fois que vous aurez une meilleure idée de ce qui peut être amélioré par le biais de l'analyse. Pour plus d'informations sur les compteurs à inclure, consultez le [guide des opérations de SQL Server 2008 R2](http://go.microsoft.com/fwlink/?LinkID=225539).  
  
 Les compteurs sont regroupés afin que vous puissiez plus facilement rechercher les compteurs associés.  
  
## <a name="counters-by-groups"></a>Compteurs par groupes  
  
|Grouper|Description|  
|-----------|-----------------|  
|[Cache](#bkmk_Cache)|Statistiques relatives au cache d'agrégation Analysis Services.|  
|[Connexion](#bkmk_Connection)|Statistiques relatives aux connexions Microsoft Analysis Services.|  
|[Prédiction de l'exploration de données](#bkmk_DataMiningPrediction)|Statistiques relatives au traitement de modèles d'exploration de données.|  
|[Traitement du modèle d'exploration de données](#bkmk_DataMiningModelProcessing)|Statistiques relatives à la création de prédictions à partir de modèles d'exploration de données.|  
|[Verrous](#bkmk_Locks)|Statistiques relatives aux verrous internes du serveur Microsoft Analysis Services.|  
|[MDX](#bkmk_MDX)|Statistiques relatives aux calculs MDX Microsoft Analysis Services.|  
|[Mémoire](#bkmk_Memory)|Statistiques relatives à la mémoire interne du serveur Microsoft Analysis Services.|  
|[Mise en cache proactive](#bkmk_ProactiveCaching)|Statistiques relatives à la mise en cache proactive Microsoft Analysis Services.|  
|[Traitement des agrégations](#bkmk_ProcAggregations)|Statistiques relatives au traitement des agrégations dans des fichiers de données MOLAP.|  
|[Traitement des index](#bkmk_ProcIndexes)|Statistiques relatives au traitement des index pour les fichiers de données MOLAP.|  
|[Traitement](#bkmk_Processing)|Statistiques relatives au traitement des données.|  
|[Requête du moteur de stockage](#bkmk_StorageEngineQuery)|Statistiques relatives aux requêtes du moteur de stockage Microsoft Analysis Services.|  
|[Threads](#bkmk_Threads)|Statistiques relatives aux threads Microsoft Analysis Services.|  
  
###  <a name="bkmk_Cache"></a> Cache  
 Statistiques relatives au cache d'agrégation Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Ko actuel|Mémoire actuelle utilisée par le cache d'agrégation, en Ko.|  
|Ko ajoutés/s|Taux de la mémoire ajoutée au cache, en Ko/s.|  
|Entrées actives|Nombre actuel d'entrées du cache.|  
|Insertions/s|Taux d'insertions dans le cache.  Le taux est suivi par partition par cube par base de données.|  
|Évictions/s|Taux d'évictions du cache.  Le taux est suivi par partition par cube par base de données.  Les évictions sont généralement dues au nettoyeur d'arrière-plan.|  
|Total insertions|Insertions dans le cache.  Le taux est suivi par partition par cube par base de données.|  
|Total évictions|Évictions du cache.  Les évictions sont suivies par partition par cube par base de données.  Les évictions sont généralement dues au nettoyeur d'arrière-plan.|  
|Accès directs/s|Taux d'accès directs au cache. Un accès au cache indique que les réponses aux requêtes ont été obtenues à partir d'une entrée du cache existante.|  
|Absences/s|Taux d'absences dans le cache.|  
|Recherches/s|Taux de recherches dans le cache.|  
|Total accès directs|Nombre total d'accès directs au cache.  Un accès direct au cache indique que les réponses aux requêtes ont été obtenues à partir d'entrées du cache existantes.|  
|Total absences|Nombre total d'absences dans le cache.|  
|Total recherches|Nombre total de recherches dans le cache.|  
|Taux d'accès directs|Taux d'accès directs aux recherches dans le cache, pour la période entre l'obtention des valeurs de compteur.|  
|Nombre total d'accès au cache de l'itérateur filtré|Nombre total d'accès au cache qui ont retourné un itérateur indexé sur les résultats filtrés.|  
|Nombre total de pertes dans le cache de l'itérateur filtré|Nombre total d'accès au cache qui n'ont pas été en mesure de générer un itérateur indexé sur les résultats filtrés et ont dû construire un nouveau cache avec les résultats filtrés.|  
  
###  <a name="bkmk_Connection"></a> Connexion  
 Statistiques relatives aux connexions Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Connexions actives|Nombre actuel de connexions client établies.|  
|Demandes/s|Taux de demandes de connexion.  Il s'agit d'arrivées.|  
|Total demandes|Nombre total de demandes de connexion.  Il s'agit d'arrivées.|  
|Réussites/s|Taux de fins de connexion réussies.|  
|Total réussites|Nombre total de connexions réussies.|  
|Échecs/s|Taux d'échecs de connexion.|  
|Total échecs|Nombre total de tentatives de connexion qui ont échoué.|  
|Sessions utilisateur actives|Nombre actuel de sessions utilisateur établies.|  
  
###  <a name="bkmk_DataMiningModelProcessing"></a> Traitement du modèle d'exploration de données  
 Statistiques relatives au traitement du modèle d'exploration de données Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Cas/s|Fréquence à laquelle les cas sont traités.|  
|Modèles actuels en traitement|Nombre actuel de modèles en cours de traitement.|  
  
###  <a name="bkmk_DataMiningPrediction"></a> Prédiction de l'exploration de données  
 Statistiques relatives à la prédiction de l'exploration de données Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Requêtes d'exploration de données simultanées|Nombre actuel de requêtes d'exploration de données en cours d'exécution.|  
|Prédictions/s|Nombre de prédictions générées dans les requêtes d'exploration de données|  
|Lignes/s|Nombre de lignes traitées pendant une requête de prédiction d'exploration de données.|  
|Requêtes/s|Nombre de requêtes d'exploration de données gérées.|  
|Total requêtes|Nombre total de requêtes d'exploration de données reçues par le serveur.|  
|Lignes de totaux|Nombre total de lignes retournées par les requêtes d'exploration de données.|  
|Total prédictions|Nombre total de requêtes de prédiction d'exploration de données reçues par le serveur.|  
  
###  <a name="bkmk_Locks"></a> Verrous  
 Statistiques relatives aux verrous internes du serveur Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Attentes de verrous en cours|Nombre actuel de threads en attente d'un verrou.  Il s'agit des requêtes de verrous qui n'ont pas pu être satisfaites immédiatement et qui sont en attente.|  
|Attentes de verrous internes/s|Taux de demandes de verrous ne pouvant être satisfaites immédiatement.|  
|Verrous actifs|Nombre actuel d'objets verrouillés.|  
|Attentes de verrous en cours|Nombre actuel de clients en attente d'un verrou.|  
|Requêtes de verrous/s|Nombre de requêtes de verrous par seconde.|  
|Octrois de verrous/s|Nombre d'octrois de verrous par seconde.|  
|Attentes de verrous/s|Nombre d'attentes de verrous par seconde.  Il s'agit des requêtes de verrous qui ne peuvent pas être satisfaites immédiatement et qui sont mises en attente.|  
|Refus de verrous/s|Taux de refus de verrou.|  
|Requêtes de déverrouillage/s|Nombre de requêtes de déverouillage par seconde.|  
|Blocages totaux détectés|Nombre total de blocages détectés.|  
  
###  <a name="bkmk_MDX"></a> MDX  
 Statistiques relatives aux calculs MDX Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Nombre de couvertures de calcul|Nombre total de nœuds d'évaluation générés par les plans d'exécution MDX comprenant les actifs et les mis en cache.|  
|Nombre actuel de nœuds d'évaluation|Nombre actuel (approximatif) de nœuds d'évaluation générés par les plans d'exécution MDX comprenant les actifs et les mis en cache.|  
|Nombre de nœuds d'évaluation du moteur de stockage|Nombre total de nœuds d'évaluation du moteur de stockage générés par les plans d'exécution MDX.|  
|Nombre de nœuds d'évaluation cellule-par-cellule|Nombre total de nœuds d'évaluation cellule par cellule générés par les plans d'exécution MDX.|  
|Nombre de nœuds d'évaluation en mode bloc|Nombre total de nœuds d'évaluation en mode bloc générés par les plans d'exécution MDX.|  
|Nombre de nœuds d'évaluation qui ont couvert une seule cellule|Nombre total de nœuds d'évaluation générés par les plans d'exécution MDX qui ont couvert une seule cellule.|  
|Nombre de nœuds d'évaluation avec des calculs de la même granularité|Nombre total de nœuds d'évaluation générés par les plans d'exécution MDX pour lesquels les calculs étaient de la même granularité que le nœud d'évaluation.|  
|Nombre actuel de nœuds d'évaluation mis en cache|Nombre actuel (approximatif) de nœuds d'évaluation mis en cache générés par les plans d'exécution MDX.|  
|Nombre de nœuds d'évaluation du moteur de stockage mis en cache|Nombre total de nœuds d'évaluation du moteur de stockage mis en cache générés par les plans d'exécution MDX.|  
|Nombre de nœuds d'évaluation en mode bloc mis en cache|Nombre total de nœuds d'évaluation en mode bloc mis en cache générés par les plans d'exécution MDX.|  
|Nombre de nœuds d'évaluation « autres » mis en cache|Nombre total de nœuds d'évaluation mis en cache générés par les plans d'exécution MDX qui ne sont ni du moteur de stockage ni en mode bloc.|  
|Nombre de suppressions de nœuds d'évaluation|Nombre total de suppressions dans le cache des nœuds d'évaluation en raison de collisions.|  
|Nombre d'accès à l'index de hachage dans le cache des nœuds d'évaluation|Nombre total d'accès dans le cache des nœuds d'évaluation satisfaits par l'index de hachage.|  
|Nombre d'accès cellule par cellule dans le cache des nœuds d'évaluation|Nombre total d'accès cellule par cellule dans le cache des nœuds d'évaluation.|  
|Nombre de pertes cellule par cellule dans le cache des nœuds d'évaluation|Nombre total de pertes cellule par cellule dans le cache des nœuds d'évaluation.|  
|Nombre d'accès au sous-cube dans le cache des nœuds d'évaluation|Nombre total d'accès au sous-cube dans le cache des nœuds d'évaluation.|  
|Nombre de pertes du sous-cube dans le cache des nœuds d'évaluation|Nombre total de pertes du sous-cube dans le cache des nœuds d'évaluation.|  
|Total sous-cubes Sonar|Nombre total de sous-cubes générés par l'optimiseur de requête.|  
|Total cellules calculées|Nombre total de propriétés de cellule calculées.|  
|Total recalculs|Nombre total de cellules recalculées en raison d'une erreur.|  
|Total insertions cache plat|Nombre total de valeurs de cellule insérées dans le cache de calcul plat.|  
|Total caches de calcul inscrits|Nombre total de caches de calcul inscrits.|  
|Total NON EMPTY|Nombre total de fois où l'algorithme NON EMPTY est utilisé.|  
|Total NON EMPTY non optimisé|Nombre total de fois où l'algorithme NON EMPTY non optimisé est utilisé.|  
|Total NON EMPTY pour membres calculés|Nombre total d'exécutions en boucle de l'algorithme NON EMPTY dans les membres calculés.|  
|Total Autoexist|Nombre total d'exécutions d'Autoexist.|  
|Total EXISTING|Nombre total d'exécutions de l'opérateur défini EXISTING.|  
  
###  <a name="bkmk_Memory"></a> Mémoire  
 Statistiques relatives à la mémoire interne du serveur Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Pool de pages mémoire d'allocation 64 (Ko)|Mémoire empruntée au système, en Ko.  Cette mémoire est distribuée aux autres parties du serveur.|  
|Pool de pages mémoire disponible 64 (Ko)|Mémoire actuelle (en Ko) dans la liste de disponibilité 64 Ko  (pages de mémoire prêtes à être utilisées).|  
|Pool de pages mémoire d'allocation 8 (Ko)|Mémoire empruntée (en Ko) au pool de pages 64 Ko.  Cette mémoire est distribuée aux autres parties du serveur.|  
|Pool de pages mémoire disponible 8 (Ko)|Mémoire actuelle (en Ko) dans la liste de disponibilité 8 Ko  (pages de mémoire prêtes à être utilisées).|  
|Pool de pages mémoire d'allocation 1 (Ko)|Mémoire empruntée (en Ko) au pool de pages 64 Ko.  Cette mémoire est distribuée aux autres parties du serveur.|  
|Pool de pages mémoire disponible 1 (Ko)|Mémoire actuelle (en Ko) dans la liste de disponibilité 8 Ko  (pages de mémoire prêtes à être utilisées).|  
|Prix actuel de la mémoire nettoyage|Prix actuel de la mémoire, $/octet/temps, normalisé à 1000.|  
|Équilibre de la mémoire nettoyage/s|Taux d'opérations d'équilibre+réduction.|  
|Mémoire nettoyage réduite Ko/s|Taux de réduction, en Ko/s|  
|Mémoire nettoyage réductible (Ko)|Quantité de mémoire (en Ko) qui doit être vidée par le nettoyeur d'arrière-plan.|  
|Mémoire nettoyage non réductible (Ko)|Quantité de mémoire (en Ko) qui ne doit pas être vidée par le nettoyeur d'arrière-plan.|  
|Mémoire nettoyage (Ko)|Quantité de mémoire (en Ko) identifiée par le nettoyeur d'arrière-plan.  (Mémoire nettoyage réductible + Mémoire nettoyage non réductible.)|  
|Utilisation de la mémoire en Ko|Utilisation de la mémoire du processus serveur intervenant dans le calcul du coût de la mémoire nettoyage.  Équivaut au compteur Process\\PrivateBytes, plus la taille des données mappées en mémoire, moins la mémoire mappée ou allouée par le moteur d'analyse en mémoire xVelocity (VertiPaq) dépassant la limite de mémoire du moteur xVelocity.|  
|Limite basse de la mémoire en Ko|Limite de mémoire inférieure dans le fichier de configuration.|  
|Limite haute de la mémoire en Ko|Limite de mémoire supérieure dans le fichier de configuration.|  
|AggCacheKB|Mémoire actuelle (en Ko) allouée au cache d'agrégation.|  
|Quota en Ko|Quota de mémoire actuelle, en Ko.  Le quota de mémoire est également une allocation ou une réserve de mémoire.|  
|Quota bloqué|Nombre actuel de requêtes de quota qui sont bloquées en attendant la libération d'autres quotas de mémoire.|  
|Ko dans le cache de fichiers|Mémoire actuelle (en Ko) allouée au cache de fichiers.|  
|Défauts de page du cache de fichiers/s|Taux de défaut de page dans le cache de fichiers.|  
|Lectures dans le cache de fichiers/s|Lectures de pages dans le cache de fichiers/s|  
|Lectures (en Ko) dans le cache de fichiers/s|Lectures (en Ko) dans le cache de fichiers/s|  
|Écritures dans le cache de fichiers/s|Pages écrites dans le cache de fichiers/s. Les écritures sont asynchrones.|  
|Écritures en Ko dans le cache de fichiers/s|Écritures en Ko dans le cache de fichiers/s. Les écritures sont asynchrones.|  
|E/S du cache de fichiers Erreurs/s|E/S du cache de fichiers Taux d'erreurs.|  
|Erreurs d'E/S dans le cache de fichiers|Total des erreurs d'E/S dans le cache de fichiers.|  
|Pages d'horloge examinées dans le cache de fichiers/s|Taux auquel le nettoyeur d'arrière-plan examine les pages dans le cadre d'une suppression.|  
|Pages d'horloge HaveRef dans le cache de fichiers/s|Taux auquel le nettoyeur d'arrière-plan examine les pages qui ont un nombre actuel de références (qui sont en cours d'utilisation).|  
|Pages d'horloge Valid dans le cache de fichiers/s|Taux auquel le nettoyeur d'arrière-plan examine les pages qui constituent des candidats valides pour une suppression.|  
|Ko épinglés dans la mémoire du cache de fichiers|Ko épinglés dans la mémoire du cache de fichiers actif.|  
|Ko en mémoire du fichier de propriétés de la dimension|Taille actuelle en mémoire du fichier de propriétés de la dimension, en Ko.|  
|Ko/s du fichier de propriétés de la dimension en mémoire|Taux d'écritures dans le fichier de propriétés de la dimension, en Ko.|  
|Ko du fichier de propriétés de la dimension potentielle en mémoire|Taille potentielle du fichier de propriétés de la dimension en mémoire, en Ko.|  
|Fichiers de propriétés de dimension|Nombre de fichiers de propriétés de dimension.|  
|Ko du fichier (hachage) d'index de la dimension en mémoire|Taille actuelle du fichier (hachage) d'index de la dimension en mémoire, en Ko.|  
|Ko/s du fichier (hachage) d'index de la dimension en mémoire|Taux d'écritures dans le fichier (hachage) d'index de la dimension en mémoire, en Ko.|  
|Ko du fichier (hachage) d'index de la dimension potentielle en mémoire|Taille potentielle du fichier (hachage) d'index de la dimension en mémoire, en Ko.|  
|Fichiers d'index (hachage) de la dimension|Nombre de fichiers d'index (hachage) de la dimension.|  
|Ko en mémoire du fichier de chaîne de la dimension|Taille actuelle en mémoire du fichier de chaîne de la dimension, en Ko.|  
|Ko/s en mémoire du fichier de chaîne de la dimension|Taux d'écritures dans le fichier de chaîne de la dimension, en Ko.|  
|Ko du fichier de chaîne de la dimension potentielle en mémoire|Taille potentielle en mémoire du fichier de chaîne de la dimension, en Ko.|  
|Fichiers de chaîne de dimension|Nombre de fichiers de chaîne de dimension.|  
|Ko du fichier de mappage en mémoire|Taille actuelle du fichier de mappage en mémoire, en Ko.|  
|Ko/s du fichier de mappage en mémoire|Taux d'écritures dans le fichier de mappage en mémoire, en Ko.|  
|Ko du fichier de mappage potentiel en mémoire|Taille potentielle du fichier de mappage en mémoire, en Ko.|  
|Fichiers de mappage|Nombre de fichiers de mappage.|  
|Ko du fichier de mappage d'agrégation en mémoire|Taille actuelle du fichier de mappage d'agrégation en mémoire, en Ko.|  
|Ko/s du fichier de mappage d'agrégation en mémoire|Taux d'écritures dans le fichier de mappage d'agrégation en mémoire, en Ko.|  
|Ko du fichier de mappage d'agrégation potentiel en mémoire|Taille potentielle du fichier de mappage d'agrégation en mémoire, en Ko.|  
|Fichiers de mappage d'agrégation|Nombre de fichiers de mappage d'agrégation.|  
|Ko du fichier de données de faits en mémoire|Taille actuelle du fichier de données de faits en mémoire, en Ko.|  
|Ko/s du fichier de données de faits en mémoire|Taux d'écritures dans le fichier de données de faits en mémoire, en Ko.|  
|Ko du fichier de données de faits potentiel en mémoire|Taille potentielle du fichier de données de faits en mémoire, en Ko.|  
|Fichiers de données de faits|Nombre de fichiers de données de faits.|  
|Ko du fichier de chaîne de faits en mémoire|Taille actuelle du fichier de chaîne de faits en mémoire, en Ko.|  
|Ko/s du fichier de chaîne de faits en mémoire|Taux d'écritures dans le fichier de chaîne de faits, en Ko.|  
|Ko du fichier de chaîne de faits potentiel en mémoire|Taille potentielle du fichier de chaîne de faits en mémoire, en Ko.|  
|Fichiers de chaîne de faits|Nombre de fichiers de chaîne de faits.|  
|Ko du fichier d'agrégation de faits en mémoire|Taille actuelle du fichier d'agrégation de faits en mémoire, en Ko.|  
|Ko/s du fichier d'agrégation de faits en mémoire|Taux d'écritures dans le fichier d'agrégation de faits en mémoire, en Ko.|  
|Ko du fichier d'agrégation de faits potentiel en mémoire|Taille potentielle du fichier d'agrégation de faits potentiel en mémoire, en Ko.|  
|Fichiers d'agrégation de faits|Nombre de fichiers d'agrégation de faits.|  
|Ko d'un autre fichier en mémoire|Taille actuelle d'un autre fichier en mémoire, en Ko.|  
|Ko/s d'un autre fichier en mémoire|Taux d'écritures dans un autre fichier en mémoire, en Ko.|  
|Ko d'un autre fichier potentiel en mémoire|Taille d'un autre fichier potentiel en mémoire, en Ko.|  
|Autres fichiers|Nombre d'autres fichiers.|  
|Mémoire paginée VertiPaq en Ko|Kilo-octets de mémoire paginée utilisée pour les données en mémoire.|  
|Mémoire non paginée VertiPaq en Ko|Kilo-octets de mémoire verrouillée dans la plage de travail à utiliser par le moteur en mémoire.|  
|Mappage mémoire VertiPaq en Ko|Kilo-octets de mémoire paginable utilisée pour les données en mémoire.|  
|Limite inconditionnelle de la mémoire en Ko|Limite de mémoire inconditionnelle dans le fichier de configuration.|  
|Limite de la mémoire VertiPaq en Ko|Limite en mémoire dans le fichier de configuration.|  
  
###  <a name="bkmk_ProactiveCaching"></a> Mise en cache proactive  
 Statistiques relatives à la mise en cache proactive Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Notifications/s|Vitesse des notifications à partir de la base de données relationnelle.|  
|Annulations de traitement/s|Taux des annulations de traitement provoquées par les notifications.|  
|Début de la mise en cache proactive/s|Vitesse de début de la mise en cache proactive.|  
|Fin de la mise en cache proactive/s|Vitesse d'achèvement de la mise en cache proactive.|  
  
###  <a name="bkmk_ProcAggregations"></a> Traitement des agrégations  
 Statistiques relatives au traitement Microsoft Analysis Services des agrégations dans les fichiers de données MOLAP.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Partitions actuelles|Nombre actuel de partitions en cours de traitement.|  
|Total partitions|Nombre total de partitions traitées (avec succès ou autre).|  
|Taille de la mémoire en lignes|Taille des agrégations actuelles en mémoire.  Ce nombre est une estimation.|  
|Taille de la mémoire en octets|Taille des agrégations actuelles en mémoire.  Ce nombre est une estimation.|  
|Lignes fusionnées/s|Taux de lignes fusionnées ou insérées dans une agrégation.|  
|Lignes créées/s|Taux de lignes d'agrégation créées.|  
|Lignes de fichiers temporaires écrites/s|Taux d'écritures de lignes dans un fichier temporaire.  L'écriture dans des fichiers temporaires a lieu lorsque les agrégations dépassent les limites de la mémoire.|  
|Octets écrits dans fichier temporaire/s|Taux d'écritures d'octets dans un fichier temporaire.  L'écriture dans des fichiers temporaires a lieu lorsque les agrégations dépassent les limites de la mémoire.|  
  
###  <a name="bkmk_ProcIndexes"></a> Traitement des index  
 Statistiques relatives au traitement Microsoft Analysis Services des index dans les fichiers de données MOLAP.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Partitions actuelles|Nombre actuel de partitions en cours de traitement.|  
|Total partitions|Nombre total de partitions traitées (avec succès ou autre).|  
|Lignes/s|Taux de lignes issues de fichiers MOLAP utilisées pour la création d'index.|  
|Lignes de totaux|Nombre total de lignes issues de fichiers MOLAP utilisées pour la création d'index.|  
  
###  <a name="bkmk_Processing"></a> Traitement  
 Statistiques relatives au traitement des données par Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Lignes lues/s|Taux de lignes lues à partir de toutes les bases de données relationnelles.|  
|Total lignes lues|Nombre de lignes lues à partir de toutes les bases de données relationnelles.|  
|Lignes converties/s|Taux de lignes converties lors du traitement.|  
|Total lignes converties|Nombre de lignes converties lors du traitement.|  
|Lignes écrites/s|Taux de lignes écrites lors du traitement.|  
|Total lignes écrites|Nombre de lignes écrites lors du traitement.|  
  
###  <a name="bkmk_StorageEngineQuery"></a> Requête du moteur de stockage  
 Statistiques relatives aux requêtes du moteur de stockage Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Requêtes actuelles de groupes de mesures|Nombre actuel de requêtes de groupes de mesures en cours de traitement.|  
|Requêtes de groupes de mesures/s|Fréquence des requêtes de groupes de mesures|  
|Total requêtes de groupes de mesures|Nombre total de requêtes dans les groupes de mesures.|  
|Requêtes de dimension actuelles|Nombre actuel de requêtes de dimension utilisées activement.|  
|Requêtes de dimension/s|Fréquence des requêtes de dimension|  
|Total requêtes de dimension|Nombre total de requêtes de dimension.|  
|Requêtes ayant obtenu une réponse/s|Taux de requêtes ayant obtenu une réponse.|  
|Total requêtes avec réponse|Nombre total de requêtes avec réponse.|  
|Octets envoyés/s|Taux d'octets envoyés par le serveur aux clients, en réponse aux requêtes.|  
|Total octets envoyés|Nombre total d'octets envoyés par le serveur aux clients, en réponse aux requêtes.|  
|Lignes envoyées/s|Taux de lignes envoyées par le serveur aux clients.|  
|Total lignes envoyées|Nombre total de lignes envoyées par le serveur aux clients.|  
|Requêtes directes à partir du cache/s|Taux de requêtes dont la réponse est obtenue directement à partir du cache.|  
|Requêtes filtrées à partir du cache/s|Taux de requêtes dont la réponse est obtenue par filtrage de l'entrée existante du cache.|  
|Requêtes à partir d'un fichier/s|Taux de requêtes ayant obtenu une réponse à partir de fichiers.|  
|Total requêtes directes à partir du cache|Nombre total de requêtes dérivées directement du cache.  Notez que ce nombre est indiqué par partition.|  
|Total requêtes filtrées à partir du cache|Nombre total de requêtes ayant obtenu une réponse par filtrage des entrées existantes du cache.|  
|Total requêtes à partir d'un fichier|Nombre total de requêtes ayant obtenu une réponse à partir de fichiers.|  
|Lectures de mappage/s|Nombre d'opérations de lecture logique utilisant le fichier de mappage.|  
|Octets de mappage/s|Octets lus à partir du fichier de mappage.|  
|Lectures de données/s|Nombre d'opérations de lecture logique utilisant le fichier de données.|  
|Octets de données/s|Octets lus à partir du fichier de données.|  
|Temps moyen/requête|Temps moyen par requête, en millisecondes.  Temps de réponse basé sur les requêtes ayant obtenu une réponse depuis la dernière mesure du compteur.|  
|Boucles réseau/s|Taux de boucles réseau.  Inclut toutes les communications client/serveur.|  
|Total boucles réseau|Nombre total de boucles réseau.  Inclut toutes les communications client/serveur.|  
|Recherches dans le cache plat/s|Taux de recherches dans le cache plat.  Cela comprend les caches plats d'étendue de la requête, de session et global.|  
|Présences dans le cache plat/s|Taux de présences dans le cache plat.  Cela comprend les caches plats d'étendue de la requête, de session et global.|  
|Recherches dans le cache de calcul/s|Taux de recherches dans le cache de calcul.  Cela comprend les caches de calcul d'étendue de la requête, de session et global.|  
|Présences dans le cache de calcul/s|Taux de présences dans le cache de calcul.  Cela comprend les caches de calcul d'étendue de la requête, de session et global.|  
|Recherches dans le cache persistant/s|Taux de recherches dans le cache persistant.  Les caches persistants sont créés par l'instruction CACHE du script MDX.|  
|Présences dans le cache persistant/s|Taux de présences dans le cache persistant.  Les caches persistants sont créés par l'instruction CACHE du script MDX.|  
|Recherches dans le cache de dimension/s|Taux de recherches dans le cache de dimension.|  
|Présences dans le cache de dimension/s|Taux de présences dans le cache de dimension.|  
|Recherches dans le cache de groupe de mesures/s|Taux de recherches dans le cache de groupe de mesures.|  
|Présences dans le cache de groupe de mesures/s|Taux de présences dans le cache de groupe de mesures.|  
|Recherches d'agrégation/s|Taux de recherches d'agrégation.|  
|Présences d'agrégation/s|Taux de présences d'agrégation.|  
  
###  <a name="bkmk_Threads"></a> Threads  
 Statistiques relatives aux threads Microsoft Analysis Services.  
  
|Compteur|Description|  
|-------------|-----------------|  
|Threads inactifs d'analyse courte|Nombre de threads inactifs dans le pool de threads d'analyse courte.|  
|Threads occupés d'analyse courte|Nombre de threads occupés dans le pool de threads d'analyse courte.|  
|Longueur de la file d'attente des travaux d'analyse courte|Nombre de travaux dans la file d'attente du pool de threads d'analyse courte.|  
|Taux de travaux d'analyse courte|Taux de travaux via le pool de threads d'analyse courte.|  
|Threads inactifs d'analyse longue|Nombre de threads inactifs dans le pool de threads d'analyse longue.|  
|Threads occupés d'analyse longue|Nombre de threads occupés dans le pool de threads d'analyse longue.|  
|Longueur de la file d'attente des travaux d'analyse longue|Nombre de travaux dans la file d'attente du pool de threads d'analyse longue.|  
|Taux de travaux d'analyse longue|Taux de travaux via le pool de threads d'analyse longue.|  
|Threads inactifs du pool de requêtes|Nombre de threads inactifs dans le pool de threads de requêtes.|  
|Threads occupés du pool de requêtes|Nombre de threads occupés dans le pool de threads de requêtes.|  
|Longueur de la file d'attente des travaux du pool de requêtes|Nombre de travaux dans la file d'attente du pool de threads de requêtes.|  
|Taux de travaux du pool de requêtes|Taux de travaux via le pool de threads de requêtes.|  
|Threads de non-E/S inactifs du pool de traitement|Nombre de threads inactifs dans le pool de threads de traitement dédiés aux travaux de non-E/S.|  
|Threads de non-E/S occupés du pool de traitement|Nombre de threads exécutant des travaux de non-E/S dans le pool de threads de traitement.|  
|Longueur de la file d'attente des travaux du pool de traitement|Nombre de travaux de non-E/S dans la file d'attente du pool de threads de traitement.|  
|Taux de travaux du pool de traitement|Taux de travaux de non-E/S via le pool de threads de traitement.|  
|Threads de travaux d'E/S inactifs du pool de traitement|Nombre de threads inactifs pour les travaux d'E/S dans le pool de threads de traitement.|  
|Threads de travaux d'E/S occupés du pool de traitement|Nombre de threads exécutant des travaux d'E/S dans le pool de threads de traitement.|  
|Longueur de la file d'attente des travaux d'E/S du pool de traitement|Nombre de travaux d'E/S dans la file d'attente du pool de threads de traitement.|  
|Taux d'achèvement des travaux d'E/S du pool de traitement|Taux de travaux d'E/S via le pool de threads de traitement.|  
  
  
