---
title: Utiliser les données modifiées (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: track-changes
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- change data [SQL Server]
- change data capture [SQL Server], query function scenarios
- change data capture [SQL Server], LSN boundaries
- change data capture [SQL Server], query functions
ms.assetid: 5346b852-1af8-4080-b278-12efb9b735eb
caps.latest.revision: 19
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: e179185cd629e8bf36a0e805b7fe54713654d67b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-change-data-sql-server"></a>Utiliser les données modifiées (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les données modifiées sont mises à disposition des consommateurs de capture de données modifiées par le biais de fonctions table. Toutes les requêtes de ces fonctions requièrent deux paramètres afin de définir la plage de numéros séquentiels dans le journal (LSN) éligibles lors du développement du jeu de résultats retourné. Les valeurs supérieures et inférieures de ces numéros qui lient l'intervalle sont considérées comme étant incluses dans l'intervalle.  
  
 Plusieurs fonctions sont fournies pour aider à déterminer les valeurs LSN appropriées utilisables dans l'interrogation d'une fonction table. La fonction [sys.fn_cdc_get_min_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md) retourne la valeur LSN la plus faible associée à un intervalle de validité d’instance de capture. L'intervalle de validité est l'intervalle de temps pendant lequel des données modifiées sont disponibles pour leurs instances de capture. La fonction [sys.fn_cdc_get_max_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md) retourne la valeur LSN la plus élevée dans l’intervalle de validité. Les fonctions [sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) et [sys.fn_cdc_map_lsn_to_time](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md) aident à placer des valeurs LSN sur une chronologie classique. Étant donné que la capture de données modifiées utilise des intervalles de requête fermés, il est quelquefois nécessaire de générer la valeur LSN suivante dans une séquence afin de s'assurer que les modifications ne sont pas dupliquées dans des fenêtres de requête consécutives. Les fonctions [sys.fn_cdc_increment_lsn](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md) et [sys.fn_cdc_decrement_lsn](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md) sont utiles quand un ajustement incrémentiel d’une valeur LSN est nécessaire.  
  
##  <a name="LSN"></a> Validation de limites LSN  
 Nous vous recommandons de procéder à la validation préalable des limites LSN à utiliser dans une requête de fonction table. Les points de terminaison nuls ou ceux qui se trouvent à l'extérieur de l'intervalle de validité pour une instance de capture forcent le retour d'une erreur par une fonction table de capture de données modifiées.  
  
 Ainsi, l'erreur ci-après est retournée dans le cadre d'une recherche portant sur toutes les modifications lorsqu'un paramètre utilisé pour définir l'intervalle de requête n'est pas valide, est hors limites, ou lorsque l'option de filtre de lignes n'est pas valide.  
  
 `Msg 313, Level 16, State 3, Line 1`  
  
 `An insufficient number of arguments were supplied for the procedure or function cdc.fn_cdc_get_all_changes_ ...`  
  
 L’erreur correspondante retournée pour une requête **net changes** est la suivante :  
  
 `Msg 313, Level 16, State 3, Line 1`  
  
 `An insufficient number of arguments were supplied for the procedure or function cdc.fn_cdc_get_net_changes_ ...`  
  
> [!NOTE]  
>  Il est reconnu que le message pour Msg 313 est trompeur et n'indique pas la cause réelle de l'échec. Cette utilisation maladroite provient de l'incapacité à lever une erreur explicite à partir d'une fonction table. Néanmoins, il a été jugé préférable de retourner une erreur reconnaissable (quoiqu'inexacte) plutôt que de simplement retourner un résultat vide. Il aurait été impossible de faire la distinction entre un jeu de résultats vide et une requête valide ne retournant aucune modification.  
  
 Les échecs d'autorisation retournent des erreurs lors de la recherche de toutes les modifications, comme indiqué ci-dessous :  
  
 `Msg 229, Level 14, State 5, Line 1`  
  
 `The SELECT permission was denied on the object 'fn_cdc_get_all_changes_...', database 'MyDB', schema 'cdc'.`  
  
 Ceci est également valable lors de l'interrogation des modifications nettes :  
  
 `Msg 229, Level 14, State 5, Line 1`  
  
 `The SELECT permission was denied on the object fn_cdc_get_net_changes_...', database 'MyDB', schema 'cdc'.`  
  
 Consultez le modèle utilisé pour énumérer des modifications nettes à l'aide de TRY CATCH afin de voir comment intercepter ces erreurs de fonction table connues et retourner des informations plus explicites à propos de l'échec.  
  
> [!NOTE]  
>  Pour rechercher des modèles de capture de données modifiées dans SQL Server Management Studio, dans le menu **Affichage** , cliquez sur **Explorateur de modèles**, développez **Modèles SQL Server** , puis développez le dossier **Capture de données modifiées** .  
  
##  <a name="Functions"></a> Fonctions de requête  
 Selon les caractéristiques de la table source faisant l'objet d'un suivi et la configuration de son instance de capture, une ou deux fonctions table sont générées pour la recherche des données modifiées.  
  
-   La fonction [cdc.fn_cdc_get_all_changes_<instance_de_capture>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) retourne toutes les modifications qui ont été effectuées pendant l’intervalle spécifié. Cette fonction est toujours générée. Les entrées sont toujours retournées triées, tout d'abord par le numéro LSN de validation de transaction de la modification, puis par une valeur qui classe la modification dans sa transaction. Selon l'option de filtre de lignes choisie, la ligne finale est retournée lors de la mise à jour (option de filtre de lignes « all ») ou les valeurs ancienne et nouvelle sont toutes deux retournées lors de la mise à jour (option de filtre de lignes « all update old »).  
  
-   La fonction [cdc.fn_cdc_get_net_changes_<instance_de_capture>](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md) est générée quand le paramètre @supports_net_changes a pour valeur 1 quand la table source est activée.  
  
    > [!NOTE]  
    >  Cette option est prise en charge uniquement si la table source possède une clé primaire définie ou si le paramètre @index_name a été utilisé pour identifier un index unique.  
  
     La fonction **netchanges** retourne une modification par ligne de table source modifiée. Si plusieurs modifications sont enregistrées pour la ligne pendant l'intervalle spécifié, les valeurs de colonne reflètent le contenu final de la ligne. Pour identifier correctement l'opération requise pour la mise à jour de l'environnement cible, la fonction table doit prendre en compte à la fois l'opération initiale sur la ligne pendant l'intervalle et la dernière opération sur la ligne. Quand l’option de filtre de lignes « all » est spécifiée, les opérations retournées par une requête **net changes** sont l’insertion, la suppression ou la mise à jour (nouvelles valeurs). Cette option retourne toujours la valeur Null pour le masque de mise à jour car un coût est associé au calcul d'un masque d'agrégation. Si vous avez besoin d'un masque d'agrégation qui reflète toutes les modifications apportées à une ligne, utilisez l'option « all with mask ». Si la distinction entre les insertions et les mises à jour n'est pas nécessaire pour le traitement en aval, utilisez l'option « all with merge ». Dans ce cas, l'opération prendra uniquement deux valeurs : 1 pour une suppression et 5 pour une insertion ou une mise à jour. Cette option supprime le traitement supplémentaire requis pour déterminer si l'opération dérivée doit être une insertion ou une mise à jour. Cela permet d'améliorer les performances de la requête lorsque cette distinction n'est pas nécessaire.  
  
 Le masque de mise à jour retourné par une fonction d'interrogation est une représentation compacte qui identifie toutes les colonnes ayant changé sur une ligne de données modifiées. En général, ces informations sont nécessaires uniquement pour un petit sous-ensemble des colonnes capturées. Des fonctions permettant d'extraire des informations du masque dans un format directement exploitable par les applications sont disponibles. La fonction [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) retourne la position ordinale d’une colonne nommée pour une instance de capture donnée, alors que la fonction [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) retourne la parité du bit dans le masque fourni, en fonction de la position ordinale transmise dans l’appel de fonction. Ces deux fonctions permettent d'extraire efficacement les informations du masque de mise à jour et de les retourner avec la requête de données modifiées. Consultez le modèle utilisé pour énumérer les modifications nettes à l'aide du modèle « All With Mask » afin de voir comment ces fonctions sont utilisées.  
  
##  <a name="Scenarios"></a> Scénarios de fonction de requête  
 Les sections ci-après décrivent des scénarios courants permettant de rechercher des données de capture des données modifiées en utilisant les fonctions de requête cdc.fn_cdc_get_all_changes_<instance_capture> et cdc.fn_cdc_get_net_changes_<instance_capture>.  
  
### <a name="querying-for-all-changes-within-the-capture-instance-validity-interval"></a>Recherche de toutes les modifications dans l'intervalle de validité d'instance de capture  
 La recherche de données modifiées la plus simple est celle qui retourne toutes les données modifiées actuelles dans l'intervalle de validité d'une instance de capture. Avant d'exécuter cette requête, commencez par déterminer les limites LSN inférieures et supérieures de l'intervalle de validité. Ensuite, utilisez ces valeurs pour identifier les paramètres @from_lsn et @to_lsn transmis à la fonction de requête cdc.fn_cdc_get_all_changes<instance_capture> ou cdc.fn_cdc_get_net_changes_<instance_capture>. Utilisez la fonction [sys.fn_cdc_get_min_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md) pour obtenir la limite inférieure et la fonction [sys.fn_cdc_get_max_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md) pour obtenir la limite supérieure. Consultez le modèle utilisé pour énumérer toutes les modifications pour la plage valide afin d'obtenir un exemple de code dans lequel toutes les modifications valides actuelles sont recherchées à l'aide de la fonction de requête cdc.fn_cdc_get_all_changes_<instance_capture>. Consultez le modèle utilisé pour énumérer les modifications nettes pour la plage valide afin d'obtenir un exemple semblable d'utilisation de la fonction cdc.fn_cdc_get_net_changes_<instance_capture>.  
  
### <a name="querying-for-all-new-changes-since-the-last-set-of-changes"></a>Recherche de toutes les nouvelles modifications depuis le dernier ensemble de modifications  
 Pour les applications conventionnelles, la recherche de données modifiées est un processus continu, qui effectue des demandes périodiques pour toutes les modifications qui se sont produites depuis la dernière demande. Avec de telles requêtes, vous pouvez utiliser la fonction [sys.fn_cdc_increment_lsn](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md) pour obtenir la limite inférieure de la requête actuelle à partir de la limite supérieure de la requête précédente. Cette méthode garantit qu'aucune ligne n'est répétée car l'intervalle de requête est toujours traité comme un intervalle fermé dans lequel les deux points d'arrêt sont inclus. Ensuite, utilisez la fonction [sys.fn_cdc_get_max_lsn](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md) pour obtenir le point d’arrêt supérieur pour le nouvel intervalle de requête. Consultez le modèle utilisé pour énumérer toutes les modifications depuis la dernière demande afin d'obtenir un exemple de code permettant de déplacer systématiquement la fenêtre de requête et d'obtenir toutes les modifications depuis la dernière demande.  
  
### <a name="querying-for-all-new-changes-up-until-now"></a>Recherche de toutes les nouvelles modifications jusqu'à maintenant  
 Une contrainte type placée sur les modifications retournées par une fonction de requête consiste à inclure uniquement les modifications qui se sont produites entre la demande précédente et la date et l'heure du jour. Pour cette requête, appliquez la fonction sys.fn_cdc_increment_lsn à la valeur @from_lsn qui a été utilisée dans la demande précédente, afin de déterminer la limite inférieure. Étant donné que la limite supérieure de l'intervalle de temps est exprimée comme une limite dans le temps spécifique, elle doit être convertie en une valeur LSN avant de pouvoir être utilisée par une fonction de requête. Avant que la valeur datetime puisse être convertie en une valeur LSN correspondante, vous devez vous assurer que le processus de capture a terminé le traitement de toutes les modifications validées jusqu'à la limite supérieure spécifiée. Cette condition est nécessaire pour garantir que toutes les modifications applicables ont été propagées à la table de modifications. L'une des méthodes possibles consiste à structurer une boucle d'attente qui vérifie périodiquement si la valeur LSN de validation maximale actuelle enregistrée pour toute table de modifications de base de données dépasse l'heure de fin souhaitée de l'intervalle de demande.  
  
 Une fois que la boucle d’attente a vérifié que le processus de capture a déjà traité toutes les entrées de journal pertinentes, utilisez la fonction [sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) pour déterminer le nouveau point d’arrêt supérieur exprimé comme valeur LSN. Pour faire en sorte que toutes les entrées validées jusqu'à l'heure spécifiée soient extraites, appelez la fonction sys.fn_cdc_map_time_to_lsn et utilisez l'option « largest less than or equal ».  
  
> [!NOTE]  
>  Dans des périodes d'inactivité, une entrée factice est ajoutée à la table cdc.lsn_time_mapping pour indiquer que le processus de capture a traité les modifications jusqu'à une heure de validation donnée. Cela empêche le processus de capture d'apparaître comme ayant pris du retard alors qu'il n'y a simplement plus aucune modification récente à traiter.  
  
 Le modèle utilisé pour énumérer toutes les modifications jusqu'à maintenant montre comment utiliser la stratégie précédente pour lancer une recherche de données modifiées.  
  
### <a name="adding-a-commit-time-to-an-all-changes-result-set"></a>Ajout d'une heure de validation à un jeu de résultats contenant toutes les modifications  
 L’heure de validation de chaque transaction avec une entrée associée dans une table de modifications de base de données est disponible dans la table [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md). En joignant la valeur __$start_lsn retournée dans une demande de toutes les modifications avec la valeur start_lsn d'une entrée de table cdc.lsn_time_mapping, vous pouvez retourner la valeur tran_end_time avec les données modifiées pour horodater la modification avec l'heure de validation de la transaction à la source. Le modèle utilisé pour ajouter une heure de validation à un jeu de résultats contenant toutes les modifications montre comment effectuer cette jointure.  
  
### <a name="joining-change-data-with-other-data-from-the-same-transaction"></a>Jointure de données modifiées avec d'autres données de la même transaction  
 Il est parfois utile de joindre les données modifiées avec d'autres informations relatives à la transaction collectées lors de la validation de celle-ci à la source. La colonne tran_begin_lsn dans la table cdc.lsn_time_mapping fournit les informations nécessaires pour effectuer une telle jointure. Quand la mise à jour de la source se produit, la valeur de database_transaction_begin_lsn dans la vue dynamique système [sys.dm_tran_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md) doit être enregistrée en même temps que toutes les autres informations à joindre avec les données modifiées. Utilisez la fonction fn_convertnumericlsntobinary pour comparer les valeurs database_transaction_begin_lsn et tran_begin_lsn. Le code permettant de créer cette fonction est disponible dans la fonction de création de modèle fn_convertnumericlsntobinary. Le modèle utilisé pour retourner toutes les modifications avec une valeur tran_begin_lsn donnée montre comment effectuer la jointure.  
  
### <a name="querying-using-datetime-wrapper-functions"></a>Recherche à l'aide de fonctions Wrapper Datetime  
 Un scénario d'application type de recherche de données modifiées consiste à demander périodiquement des données modifiées en utilisant une fenêtre glissante délimitée par des valeurs datetime. Pour cette classe de consommateurs, la capture des données modifiées fournit la procédure stockée [sys.sp_cdc_generate_wrapper_function](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md) qui génère des scripts permettant de créer des fonctions Wrapper personnalisées pour les fonctions de requête de capture des données modifiées. Ces wrappers personnalisés permettent d'exprimer l'intervalle de requête comme une paire de valeurs datetime.  
  
 L'appel d'options pour la procédure stockée permet de générer des wrappers pour toutes les instances de capture auquel l'appelant a accès, ou uniquement pour une instance de capture spécifiée. Les options prises en charge incluent également la capacité de spécifier si le point d'arrêt supérieur de l'intervalle de capture doit être ouvert ou fermé, laquelle des colonnes capturées disponibles doit être incluse dans le jeu de résultats et laquelle des colonnes incluses doit être associée à des indicateurs de mise à jour. La procédure retourne un jeu de résultats avec deux colonnes : le nom de fonction généré, qui peut être dérivé du nom d'instance de capture, et l'instruction de création pour la procédure stockée de wrapper. La fonction permettant d'inclure dans un wrapper la requête de recherche de toutes les modifications est toujours générée. Si le paramètre @supports_net_changes a été défini lors de la création de l’instance de capture, la fonction permettant d’inclure dans un wrapper la fonction de recherche des modifications nettes est également générée.  
  
 Le concepteur d'applications est chargé d'appeler la procédure stockée de génération de script afin de générer les instructions de création pour les procédures stockées de wrapper et d'exécuter les scripts de création résultants pour créer les fonctions. Cela ne se produit pas automatiquement lorsqu'une instance de capture est créée.  
  
 Les wrappers datetime appartiennent à l'utilisateur et ne sont pas créés dans le schéma par défaut de l'appelant. La fonction générée peut être utilisée par la plupart des utilisateurs, sans être modifiée. Toutefois, il est toujours possible d'effectuer des tâches de personnalisation supplémentaires sur le script généré avant de créer la fonction.  
  
 Le nom de la fonction permettant d'inclure dans un wrapper la requête de recherche de toutes les modifications est fn_all_changes_ suivi du nom de l'instance de capture. Le préfixe utilisé pour le wrapper des modifications nettes est fn_net_changes_. Les deux fonctions utilisent trois arguments, tout comme leurs fonctions table de capture de données modifiées associées. Toutefois, l'intervalle de requête pour les wrappers est délimité par deux valeurs datetime au lieu de deux valeurs LSN. Le paramètre @row_filter_option pour les deux ensembles de fonctions est le même.  
  
 Les fonctions wrapper générées prennent en charge la convention suivante pour parcourir systématiquement la chronologie de capture de données modifiées : le paramètre @end_time de l’intervalle précédent est censé être utilisé comme paramètre @start_time de l’intervalle suivant. La fonction wrapper se charge de mapper les valeurs datetime aux valeurs LSN et de garantir qu'aucune donnée n'est perdue ou répétée si cette convention est suivie.  
  
 Les wrappers peuvent être générés pour prendre en charge une limite supérieure fermée ou une limite supérieure ouverte sur la fenêtre de requête spécifiée. Autrement dit, l'appelant peut spécifier si les entrées ayant une heure de validation égale à la limite supérieure de l'intervalle d'extraction doivent être incluses dans l'intervalle. Par défaut, la limite supérieure est incluse.  
  
 Alors que les fonctions table de requête générées échouent si elles ont fourni une valeur Null pour @from_lsn ou @to_lsn, les fonctions wrapper datetime utilisent une valeur Null pour permettre aux wrappers datetime de retourner toutes les modifications actuelles. Autrement dit, si une valeur Null est transmise comme point d'arrêt inférieur de la fenêtre de requête au wrapper datetime, le point d'arrêt inférieur de l'intervalle de validité d'instance de capture est utilisé dans l'instruction SELECT sous-jacente appliquée à la fonction table de requête. De la même façon, si une valeur Null est transmise comme point d'arrêt supérieur de la fenêtre de requête, le point d'arrêt supérieur de l'intervalle de validité d'instance de capture est utilisé lors de la sélection de la fonction table de requête.  
  
 Le jeu de résultats retourné par une fonction wrapper inclut toutes les colonnes demandées suivies d'une colonne d'opération, réécrite sous forme d'un ou de deux caractères pour identifier l'opération associée à la ligne. Si des indicateurs de mise à jour ont été demandés, ils apparaissent sous la forme de colonnes de bits après le code d’opération et dans l’ordre spécifié dans le paramètre @update_flag_list. Pour plus d’informations sur les options d’appel permettant de personnaliser les wrappers datetime générés, consultez [sys.sp_cdc_generate_wrapper_function &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md).  
  
 Le modèle utilisé pour instancier une fonction table wrapper avec un indicateur de mise à jour montre comment personnaliser une fonction wrapper générée afin d'ajouter un indicateur de mise à jour pour une colonne spécifiée au jeu de résultats retourné par une requête de modifications nettes. Le modèle utilisé pour instancier des fonctions table wrapper CDC pour un schéma montre comment instancier les wrappers Datetime pour les fonctions table de requête pour toutes les instances de capture créées pour les tables sources dans un schéma de base de données spécifique.  
  
 Le modèle utilisé pour obtenir des modifications nettes à l'aide d'un wrapper avec des indicateurs de mise à jour contient un exemple qui utilise un wrapper datetime pour rechercher des données modifiées. Ce modèle montre comment lancer une recherche de modifications nettes avec une fonction wrapper lorsque le wrapper est configuré pour retourner des indicateurs de mise à jour. Notez que l'option de filtre de lignes « all with mask » est requise pour que la fonction de requête sous-jacente retourne un masque de mise à jour non Null lors d'une mise à jour. Les valeurs Null sont transmises pour les limites inférieure et supérieure de l'intervalle datetime afin de signaler à la fonction qu'elle doit utiliser le point d'arrêt inférieur et le point d'arrêt supérieur de l'intervalle de validité pour l'instance de capture lorsqu'elle effectue la requête LSN sous-jacente. La requête retourne une ligne pour chaque modification apportée à une ligne source dans la plage valide pour l'instance de capture.  
  
### <a name="using-the-datetime-wrapper-functions-to-transition-between-capture-instances"></a>Utilisation des fonctions wrapper Datetime pour assurer la transition entre des instances de capture  
 La capture de données modifiées prend en charge jusqu'à deux instances de capture pour une seule table source suivie. Cette fonction est principalement utilisée pour permettre une transition entre plusieurs instances de capture lorsque les modifications de langage de définition de données (DDL) effectuées sur la table source étendent le jeu des colonnes disponibles à des fins de suivi. Lors de la transition vers une nouvelle instance de capture, l'une des méthodes permettant de protéger les niveaux d'application supérieurs contre tout changement éventuel de nom des fonctions de requête sous-jacentes consiste à utiliser une fonction wrapper pour inclure dans un wrapper l'appel sous-jacent. Ensuite, vous devez vous assurer que le nom de la fonction wrapper reste le même. Lorsque le changement est sur le point de se produire, l'ancienne fonction wrapper peut être supprimée, et une nouvelle fonction wrapper portant le même nom et faisant référence aux nouvelles fonctions de requête peut être créée. Le fait de commencer par modifier le script généré pour créer une fonction wrapper du même nom vous permet de passer à une nouvelle instance de capture sans affecter les couches d'application supérieures.  
  
## <a name="see-also"></a> Voir aussi  
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Activer et désactiver la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)   
 [Administrer et surveiller la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
