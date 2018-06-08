---
title: Exemple d’espace disque d’un index | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- online index disk space
- disk space [SQL Server], indexes
- index disk space [SQL Server]
- space [SQL Server], indexes
- indexes [SQL Server], disk space requirements
- offline index disk space [SQL Server]
ms.assetid: e5c71f55-0be3-4c93-97e9-7b3455c8f581
caps.latest.revision: 30
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 46809da9211dbb4ec8704a788945b30cf5e6ca65
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="index-disk-space-example"></a>Exemple d'espace disque d'un index
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Chaque fois qu'un index est créé, reconstruit ou supprimé, de l'espace disque est nécessaire tant pour l'ancienne structure (source) que pour la nouvelle (cible) dans leurs fichiers et groupes de fichiers respectifs. L'ancienne structure n'est pas désallouée aussi longtemps que la transaction de création d'index n'est pas validée. De l'espace disque temporaire supplémentaire peut également être requis pour le tri des opérations. Pour plus d’informations, consultez [Disk Space Requirements for Index DDL Operations](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md).  
  
 Cet exemple détermine l'espace disque nécessaire pour la création d'un index cluster.  
  
 Supposons que les conditions suivantes sont remplies avant la création de l'index cluster :  
  
-   La table existante (heap) contient 1 million de lignes. Chaque ligne a une longueur de 200 octets.  
  
-   L'index non-cluster A contient 1 million de lignes. Chaque ligne a une longueur de 50 octets.  
  
-   L'index non-cluster B contient 1 million de lignes. Chaque ligne a une longueur de 80 octets.  
  
-   L'option index create memory a pour valeur 2 Mo.  
  
-   Une valeur de taux de remplissage de 80 est utilisée pour l'ensemble des index (nouveaux et existants). Cela signifie que les pages sont remplies à 80 %.  
  
    > [!NOTE]  
    >  Suite à la création d'un index cluster, les deux index non-cluster doivent être reconstruits pour remplacer l'indicateur de ligne par la nouvelle clé d'index cluster.  
  
## <a name="disk-space-calculations-for-an-offline-index-operation"></a>Calculs de l'espace disque pour une opération d'index hors ligne  
 La procédure suivante permet de calculer l'espace disque temporaire à utiliser pendant l'opération d'index et l'espace disque permanent pour le stockage des nouveaux index. Les calculs indiqués sont approximatifs : les résultats sont arrondis et ne tiennent compte que de la taille du niveau feuille de l'index. Le tilde (~) est utilisé pour indiquer les calculs is approximatifs.  
  
1.  Déterminez la taille des structures sources.  
  
     Segment de mémoire : 1 million * 200 octets ~ 200 Mo  
  
     Index non-cluster A : 1 million * 50 octets / 80 % ~ 63 Mo  
  
     Index non-cluster B : 1 million * 80 octets / 80 % ~ 100 Mo  
  
     Taille totale des structures existantes : 363 Mo  
  
2.  Déterminez la taille des structures d'index cibles. Supposons que la nouvelle clé en cluster a une longueur de 24 octets, indicateur d’unicité compris. L'indicateur de ligne (8 octets de long) des deux index non-cluster sera remplacé par cette clé d'index cluster.  
  
     Index cluster : 1 million * 200 octets / 80 % ~ 250 Mo  
  
     Index non cluster A : 1 million * (50 – 8 + 24) octets / 80 % ~ 83 Mo  
  
     Index non cluster B : 1 million * (80 – 8 + 24) octets / 80 % ~ 120 Mo  
  
     Taille totale des nouvelles structures : 453 Mo  
  
     La quantité totale d'espace disque nécessaire à la prise en charge des structures sources et cibles pendant la durée de l'opération d'index est de 816 Mo (363 + 453). L'espace actuellement alloué aux structures sources sera désalloué une fois l'opération d'index validée.  
  
3.  Déterminez l'espace disque temporaire supplémentaire pour le tri.  
  
     L’espace nécessaire est indiqué pour le tri dans **tempdb** (SORT_IN_TEMPDB a la valeur ON) et pour le tri à l’emplacement cible (SORT_IN_TEMPDB a la valeur OFF).  
  
    1.  Lorsque SORT_IN_TEMPDB a la valeur ON, **tempdb** doit disposer de suffisamment d’espace pour contenir l’index le plus volumineux (1 million * 200 octets ~ 200 Mo). Le facteur de remplissage n'est pas pris en compte dans l'opération de tri.  
  
         L’espace disque supplémentaire (à l’emplacement **tempdb** ) équivaut à la valeur de [configuration de l’option de configuration de serveur Création d’index en mémoire](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md) = 2 Mo.  
  
         Taille totale de l'espace disque temporaire avec SORT_IN_TEMPDB ayant la valeur ON ~ 202 Mo.  
  
    2.  Lorsque SORT_IN_TEMPDB a la valeur OFF (par défaut), les 250 Mo d'espace disque déjà pris en compte pour le nouvel index à l'étape 2 sont utilisés pour le tri.  
  
         L’espace disque supplémentaire (à l’emplacement cible) équivaut à la valeur de [configuration de l’option de configuration de serveur Création d’index en mémoire](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md) = 2 Mo.  
  
         Taille totale de l'espace disque temporaire avec SORT_IN_TEMPDB ayant la valeur OFF = 2 Mo.  
  
 Si vous utilisez **tempdb**, un total de 1018 Mo (816 + 202) est nécessaire pour créer les index cluster et non-cluster. Bien que l’utilisation de **tempdb** augmente la quantité d’espace disque temporaire utilisée pour créer un index, elle peut réduire le temps nécessaire pour créer un index lorsque **tempdb** ne se trouve pas sur le même ensemble de disques que la base de données utilisateur. Pour plus d’informations sur l’utilisation de **tempdb**, consultez [Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 Si vous n’utilisez pas **tempdb**, un total de 818 Mo (816 + 2) est nécessaire pour créer les index cluster et non-cluster.  
  
## <a name="disk-space-calculations-for-an-online-clustered-index-operation"></a>Calculs de l'espace disque pour une opération d'index cluster en ligne  
 Lorsque vous créez, supprimez ou reconstruisez un index cluster en ligne, de l'espace disque supplémentaire est requis pour créer et gérer un index de mappage temporaire. Celui-ci contient un enregistrement de chaque ligne de la table, et son contenu est l'union des anciennes colonnes de signets et des nouvelles.  
  
 Pour calculer l'espace disque nécessaire pour une opération d'index cluster en ligne, conformez-vous à la procédure décrite pour une opération d'index hors ligne et ajoutez ces résultats à ceux de l'étape suivante.  
  
-   Déterminez l'espace de l'index de mappage temporaire.  
  
     Dans cet exemple, l’ancien signet est l’ID de ligne (RID) du segment de mémoire (8 octets) et le nouveau signet est la clé de clustering (24 octets, **indicateur d’unicité**compris). Aucune colonne ne se chevauche entre les anciens et nouveaux signets.  
  
     Taille de l'index de mappage temporaire = 1 million * (8 octets + 24 octets) / 80 % ~ 40 Mo.  
  
     Cet espace disque doit être ajouté à l’espace disque requis à l’emplacement cible si SORT_IN_TEMPDB a la valeur OFF, ou être ajouté à **tempdb** si SORT_IN_TEMPDB a la valeur ON.  
  
 Pour plus d’informations sur l’index de mappage temporaire, consultez [Espace disque nécessaire pour les opérations DDL d’index](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md).  
  
## <a name="disk-space-summary"></a>Récapitulatif au sujet de l'espace disque  
 Le tableau suivant récapitule les résultats des calculs de l'espace disque.  
  
|Opération d'index|Espace disque nécessaire pour les emplacements des structures suivantes|  
|---------------------|---------------------------------------------------------------------------|  
|Opération d'index hors ligne avec SORT_IN_TEMPDB = ON|Espace total lors de l'opération : 1018 Mo<br /><br /> -Table et index existants : 363 Mo\*<br /><br /> -<br />                    **tempdb**: 202 Mo*<br /><br /> -Nouveaux index : 453 Mo<br /><br /> Espace total requis après l'opération : 453 Mo|  
|Opération d'index hors ligne avec SORT_IN_TEMPDB = OFF|Espace total lors de l'opération : 816 Mo<br /><br /> -Table et index existants : 363 Mo*<br /><br /> -Nouveaux index : 453 Mo<br /><br /> Espace total requis après l'opération : 453 Mo|  
|Opération d'index en ligne avec SORT_IN_TEMPDB = ON|Espace total lors de l'opération : 1058 Mo<br /><br /> -Table et index existants : 363 Mo\*<br /><br /> -<br />                    **tempdb** (index de mappage compris) : 242 Mo*<br /><br /> -Nouveaux index : 453 Mo<br /><br /> Espace total requis après l'opération : 453 Mo|  
|Opération d'index en ligne avec SORT_IN_TEMPDB = OFF|Espace total lors de l'opération : 856 Mo<br /><br /> -Table et index existants : 363 Mo*<br /><br /> -Index de mappage temporaire : 40 Mo\*<br /><br /> -Nouveaux index : 453 Mo<br /><br /> Espace total requis après l'opération : 453 Mo|  
  
 *Cet espace est désalloué une fois l'opération d'index validée.  
  
 Cet exemple ne tient pas compte de l’espace disque temporaire supplémentaire éventuellement requis dans **tempdb** pour les enregistrements de versions créés par des opérations de mise à jour utilisateur et de suppression simultanées.  
  
## <a name="related-content"></a>Contenu associé  
 [Disk Space Requirements for Index DDL Operations](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md)  
  
 [Espace disque du journal des transactions pour les opérations d'index](../../relational-databases/indexes/transaction-log-disk-space-for-index-operations.md)  
  
  
