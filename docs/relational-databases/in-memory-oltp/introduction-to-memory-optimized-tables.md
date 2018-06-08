---
title: Introduction aux tables optimisées en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 12/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ef1cc7de-63be-4fa3-a622-6d93b440e3ac
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e3edda9ebc4f356302c1e6a01c87d026bdb8f5e9
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="introduction-to-memory-optimized-tables"></a>Introduction aux tables optimisées en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Les tables à mémoire optimisée sont créées à l’aide de [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 Les tables à mémoire optimisée sont intégralement durables par défaut, et, tout comme les transactions sur des tables sur disque (traditionnelles), les transactions sur les tables à mémoire optimisée sont ACID (atomicité, cohérence, isolation, durabilité). Les tables à mémoire optimisée et les procédures stockées compilées en mode natif ne prennent en charge qu’un sous-ensemble de fonctionnalités [!INCLUDE[tsql](../../includes/tsql-md.md)].
 
À compter de SQL Server 2016 et dans Azure SQL Database, il n’existe aucune limite relative aux [classements ni pages de codes](../../relational-databases/collations/collation-and-unicode-support.md) qui sont spécifiques à OLTP en mémoire.
  
 Le stockage principal des tables à mémoire optimisée est la mémoire principale. Les lignes de la table sont lues et écrites dans la mémoire. Une deuxième copie des données de la table est conservée sur le disque, mais uniquement pour la durabilité. Consultez [Création et gestion du stockage des objets mémoire optimisés](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md) Pour plus d’informations sur les tables durables. Les données des tables à mémoire optimisée ne sont lues sur le disque que lors de la récupération de la base de données (par exemple, après le redémarrage d’un serveur).  
  
 Pour des gains de performance encore accrus, l'OLTP en mémoire prend en charge les tables durables avec des transactions à durabilité retardée. Les transactions durables retardées sont enregistrées sur le disque peu après qu’elles ont été validées et que le contrôle a été retourné au client. En revanche, les transactions validées qui n'ont pas été enregistrées sur le disque sont perdues en cas de défaillance ou de basculement du serveur.  
  
 Outre les tables optimisées en mémoire durables par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend également en charge les tables optimisées en mémoire non durables, qui ne sont pas enregistrées et dont les données ne sont pas rendues persistantes sur disque. Cela signifie que les transactions sur ces tables ne nécessitent aucune E/S disque, mais que les données ne sont pas récupérées suite à un incident ou à une défaillance du serveur.  
  
 L'OLTP en mémoire est intégré à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour offrir une expérience transparente dans tous les domaines, tels que le développement, le déploiement, la gestion et la prise en charge. Une base de données peut contenir des objets en mémoire et sur disque.  
  
 Les lignes des tables optimisées en mémoire ont un contrôle de version. Cela signifie que chaque ligne de la table a potentiellement plusieurs versions. Toutes les versions de ligne sont conservées dans la même structure de données de la table. Le contrôle de version de ligne est utilisé pour autoriser les lectures et les écritures simultanées sur la même ligne. Pour plus d’informations sur les lectures et les écritures simultanées sur la même ligne, consultez [Transactions with Memory-Optimized Tables](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)(Transactions avec des tables optimisées en mémoire).  
  
 La figure ci-dessous illustre les contrôles de version multiples. Elle représente une table de trois lignes ayant chacune plusieurs versions.  
  
![Multi-versioning.](../../relational-databases/in-memory-oltp/media/hekaton-tables-1.gif "Multi-versioning.")  
  
 La table a trois lignes : r1, r2 et r3. r1 a trois versions, r2 a deux versions, et r3 a quatre versions. Notez que les différentes versions de la même ligne n'occupent pas nécessairement des emplacements de mémoire consécutifs. Les différentes versions de ligne peuvent être dispersées dans l'ensemble de la structure de données de la table.  
  
 La structure de données des tables optimisées en mémoire peut être considérée comme une collection de versions de ligne. Les lignes d'une table sur disque sont organisées dans des pages et des extensions, et les lignes individuelles sont traitées sur la base du numéro et du décalage de la page, les versions de ligne dans les tables optimisées en mémoire sont traitées à l'aide de pointeurs de mémoire à huit octets.  
  
 Les données des tables optimisées en mémoire sont accessibles de deux manières :  
  
-   Via des procédures stockées compilées en mode natif.  
  
-   Via le [!INCLUDE[tsql](../../includes/tsql-md.md)]interprété, en dehors d'une procédure stockée compilée en mode natif. Ces instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent être dans des procédures stockées interprétées ou des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc.  
  
## <a name="accessing-data-in-memory-optimized-tables"></a>Accès aux données des tables optimisées en mémoire  

Les tables optimisées en mémoire sont plus efficacement accessibles à partir des procédures stockées compilées en mode natif ([Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)). Elles peuvent également être accédées via du [!INCLUDE[tsql](../../includes/tsql-md.md)]traditionnel et interprète. Le terme [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété fait référence à l'accès aux tables optimisées en mémoire sans procédure stockée compilée en mode natif. Parmi les exemples d'accès en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété figure l'accès à une table optimisée en mémoire à partir d'un déclencheur DML ou d'un traitement, d'une vue ou d'une fonction table [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc.  
  
 Le tableau suivant résume l'accès natif et l'accès en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété pour différents objets.  
  
|Fonctionnalité|Accès à l'aide d'une procédure stockée compilée en mode natif|Accès en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété|Accès CLR|  
|-------------|-------------------------------------------------------|-------------------------------------------|----------------|  
|Table mémoire optimisée|Oui|Oui|Non*|  
|Type de table mémoire optimisée|Oui|Oui|non|  
|Procédure stockée compilée en mode natif|L’imbrication de procédures stockées compilées en mode natif est désormais prise en charge. Vous pouvez utiliser la syntaxe EXECUTE à l’intérieur des procédures stockées, à condition que la procédure référencée soit également compilée en mode natif.|Oui|Non*|  
  
 *Vous ne pouvez pas accéder à une table optimisée en mémoire ou à une procédure stockée compilée en mode natif à partir de la connexion contextuelle (connexion à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au moment de l’exécution d’un module CLR). Cependant, vous pouvez créer et ouvrir une autre connexion à partir de laquelle vous accédez aux tables optimisées en mémoire et aux procédures stockées compilées en mode natif.  
  
## <a name="performance-and-scalability"></a>Performances et extensibilité  

Les facteurs suivants affectent les gains de performance pouvant être obtenus avec l'OLTP en mémoire :  
  
*Communication :* Une application comportant de nombreux appels à des procédures stockées courtes a un moindre gain de performances par rapport à une application comportant moins d’appels et plus de fonctionnalités implémentées dans chaque procédure stockée.  
  
*[!INCLUDE[tsql](../../includes/tsql-md.md)] Exécution :* L’OLTP en mémoire offre des performances optimales si vous utilisez des procédures stockées compilées en mode natif plutôt que des procédures stockées interprétées ou l’exécution de requêtes. Il peut être avantageux d’accéder aux tables optimisées en mémoire à partir de ces procédures stockées.  
  
*Comparaison entre l’analyse de plage et la recherche de points :* Les index non cluster optimisés en mémoire prennent en charge les analyses de plage et les analyses triées. Pour les recherches de points, les index de hachage optimisés en mémoire offrent de meilleures performances que les index non cluster optimisés en mémoire. Les index non cluster optimisés en mémoire offrent de meilleures performances que les index sur disque.

- À partir de SQL Server 2016, le plan de requête pour une table optimisée en mémoire peut analyser la table en parallèle. Les requêtes analytiques sont donc plus performantes.
  - Les index de hachage peuvent également être analysés en parallèle à partir de SQL Server 2016.
  - Les index non cluster peuvent également être analysés en parallèle à partir de SQL Server 2016.
  - Les index columnstore peuvent être analysés en parallèle depuis leur création dans SQL Server 2014.
  
*Opérations d’index :* Les opérations d’index ne sont pas stockées et existent uniquement en mémoire.  
  
*Accès simultané :* Pour les applications dont les performances sont affectées par l’accès simultané au niveau du moteur, tel que la contention de verrous internes ou le blocage, les performances s’améliorent de façon significative quand l’application passe à l’OLTP en mémoire.  
  
Le tableau suivant répertorie les problèmes de performance et d'extensibilité couramment rencontrés dans les bases de données relationnelles et indique comment l'OLTP peut améliorer les performances.  
  
|Problème|Impact sur l'OLTP en mémoire|  
|-----------|----------------------------|  
|Performances<br /><br /> Utilisation importante des ressources (UC, E/S, réseau ou mémoire).|Unité centrale<br /> Les procédures stockées compilées en mode natif peuvent réduire de façon significative l'utilisation de l'UC, car elles nécessitent moins d'instructions pour exécuter une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] que les procédures stockées interprétées.<br /><br /> L'OLTP en mémoire peut vous aider à réduire l'investissement en matériel dans des charges de travail avec montée en puissance parallèle, car un serveur peut assurer à lui seul le débit de cinq à dix serveurs.<br /><br /> E/S<br /> Si vous observez un goulot d'étranglement des E/S à partir du traitement des données ou des pages d'index, l'OLTP en mémoire peut l'atténuer. En outre, les points de contrôle des objets OLTP en mémoire sont continus et n'entraînent pas une hausse soudaine des opérations d'E/S. Toutefois, si la plage de travail des tables critiques pour les performances ne tient pas en mémoire, l'OLTP en mémoire n'améliore pas les performances, car il nécessite que les données résident en mémoire. Si vous observez un goulot d'étranglement des E/S lors de la journalisation, l'OLTP en mémoire l'atténue, car il nécessite moins d'opérations de journalisation. Si une ou plusieurs tables optimisées en mémoire sont configurées en tant que tables non durables, supprimez la journalisation des données.<br /><br /> Mémoire<br /> L'OLTP en mémoire ne procure aucun gain de performance. Il peut en effet solliciter davantage la mémoire, car les objets doivent résider en mémoire.<br /><br /> Réseau<br /> L'OLTP en mémoire ne procure aucun gain de performance. Les données doivent être communiquées de la couche Données à la couche Application.|  
|Extensibilité<br /><br /> La plupart des problèmes de mise à l'échelle dans les applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont dus à des problèmes d'accès concurrentiel tels que la contention dans les verrous, les verrous internes et les verrouillages tournants.|Contention de verrous internes<br /> Un scénario typique est la contention dans la dernière page d'un index lors de l'insertion simultanée de lignes dans l'ordre des clés. Étant donné que l'OLTP en mémoire ne prend pas de verrou interne lors de l'accès aux données, il n'y a plus de problèmes d'extensibilité liés à la contention de verrous internes.<br /><br /> Contention de verrouillage tournant<br /> Étant donné que l'OLTP en mémoire ne prend pas de verrou interne lors de l'accès aux données, il n'y a plus de problèmes d'extensibilité liés à la contention de verrouillage tournant.<br /><br /> Contention liée au verrouillage<br /> Si votre application de base de données rencontre des problèmes de blocage entre les opérations de lecture et d'écriture, l'OLTP en mémoire les supprime, car il utilise une nouvelle forme de contrôle d'accès concurrentiel optimiste pour implémenter tous les niveaux d'isolation des transactions. L'OLTP en mémoire n'utilise pas TempDB pour stocker les versions de ligne.<br /><br /> Si le problème de mise à l'échelle est provoqué par un conflit entre deux opérations d'écriture, telles que deux transactions simultanées tentant de mettre à jour la même ligne, l'OLTP en mémoire laisse une transaction aboutir et fait échouer l'autre. La transaction en échec doit être retentée par un nouvel envoi explicite ou implicite. Dans les deux cas, vous devez apporter des modifications à l'application.<br /><br /> Si votre application crée des conflits fréquents entre deux opérations d'écriture, la valeur du verrouillage optimiste est diminuée. L'application ne convient pas pour l'OLTP en mémoire. La plupart des applications OLTP ne connaissent pas de conflits d'écriture à moins que le conflit ne soit induit par une escalade de verrous.|  
  
##  <a name="rls"></a> Sécurité de niveau ligne dans les tables optimisées en mémoire  

[Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md) La sécurité au niveau des lignes est prise en charge pour les tables optimisées en mémoire. La procédure d’application de stratégies de sécurité de niveau ligne à des tables optimisées en mémoire est globalement identique à celle qui concerne les tables sur disque, à une exception près : les Fonctions table incluses qui sont utilisées comme prédicats de sécurité doivent être compilées en mode natif (créées avec l’option WITH NATIVE_COMPILATION). Pour plus d’informations, consultez la section [Compatibilité entre fonctionnalités](../../relational-databases/security/row-level-security.md#Limitations) dans la rubrique [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md) .  
  
 Diverses fonctions de sécurité intégrées essentielles à la sécurité au niveau des lignes ont été activées pour les tables en mémoire. Pour plus d’informations, consultez [Fonctions intégrées dans les modules compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md#bfncsp).  
  
 **EXECUTE AS CALLER** : désormais, tous les modules natifs prennent en charge et utilisent EXECUTE AS CALLER par défaut, même si l’indicateur n’est pas spécifié. En effet, toutes les fonctions de prédicat de sécurité au niveau des lignes sont censées utiliser EXECUTE AS CALLER, de sorte que la fonction (et toutes les fonctions intégrées utilisées au sein de cette dernière) seront évaluées dans le contexte de l’utilisateur appelant.   
EXECUTE AS CALLER n’entraîne qu’une faible baisse de performances (de l’ordre de 10 %) du fait des vérifications des autorisations effectuées sur l’appelant. Si le module spécifie explicitement EXECUTE AS OWNER ou EXECUTE AS SELF, ces vérifications d’autorisations et les coûts de performances qui en découlent seront évités. Toutefois, l’utilisation de l’une de ces options avec les fonctions intégrées ci-dessus aura un impact nettement plus sensible sur les performances en raison du basculement de contexte nécessaire.  
  
## <a name="scenarios"></a>Scénarios

Pour découvrir une brève description des scénarios types dans lesquels [!INCLUDE[hek_1](../../includes/hek-1-md.md)] peut contribuer à améliorer les performances, voir [OLTP en mémoire](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a> Voir aussi

[OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
