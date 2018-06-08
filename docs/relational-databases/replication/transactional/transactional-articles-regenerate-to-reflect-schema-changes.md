---
title: Regénérer des procédures transactionnelles personnalisées pour refléter des modifications de schéma | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- custom procedures [SQL Server replication]
- transactional replication, replicating schema changes
- schemas [SQL Server replication], replicating changes
ms.assetid: ccf68a13-e748-4455-8168-90e6d2868098
caps.latest.revision: 29
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 56a5d8692355a6dc3b9de0f6f21afc85d61d47c7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transactional-articles---regenerate-to-reflect-schema-changes"></a>Articles transactionnels - Regénérer pour refléter les modifications de schéma
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Par défaut, la réplication transactionnelle effectue toutes les modifications de données sur l'Abonné via des procédures stockées qui sont générées par des procédures internes pour chaque article de table de la publication. Les trois procédures (une pour les insertions, une pour les mises à jour et une pour les suppressions) sont copiées vers l'Abonné et s'exécutent quand une insertion, une mise à jour ou une suppression est répliquée vers l'Abonné. Quand une modification de schéma est apportée à une table sur un serveur de publication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , la réplication régénère automatiquement ces procédures en appelant le même ensemble de procédures de script internes, de façon à ce que les nouvelles procédures correspondent au nouveau schéma (la réplication de modifications de schéma n'est pas prise en charge pour les serveurs de publication Oracle).  
  
 Il est également possible de spécifier des procédures personnalisées pour remplacer une ou plusieurs des procédures par défaut. Les procédures personnalisées doivent être modifiées si la modification de schéma affecte la procédure. Par exemple, si une procédure référence une colonne qui est supprimée dans une modification de schéma, les références à la colonne doivent être supprimées de la procédure. Il y a deux façons pour la réplication de propager une nouvelle procédure personnalisée vers les Abonnés :  
  
-   La première option consiste à utiliser une procédure de script personnalisée pour remplacer les procédures par défaut utilisées par la réplication :  
  
    1.  Lors de l’exécution de [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), assurez-vous que le bit 0x02 de **@schema_option** ait la valeur **true**.  
  
    2.  Exécutez [sp_register_custom_scripting &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-register-custom-scripting-transact-sql.md) et spécifiez la valeur 'insert', 'update' ou 'delete' pour le paramètre **@type** et le nom de la procédure de script personnalisée pour le paramètre **@value**.  
  
     La prochaine fois qu'une modification de schéma est effectuée, la réplication appelle cette procédure stockée pour générer le script de la définition de la nouvelle procédure stockée personnalisée définie par l'utilisateur, puis propage la procédure à chaque Abonné.  
  
-   La deuxième option consiste à utiliser un script qui contient une nouvelle définition de procédure personnalisée :  
  
    1.  Lors de l’exécution de [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), affectez au bit 0x02 de **@schema_option** la valeur **false**, de façon à ce que la réplication ne génère pas automatiquement des procédures personnalisées sur l’Abonné.  
  
    2.  Avant chaque modification de schéma, créez un nouveau fichier de script et inscrivez le script avec la réplication en exécutant [sp_register_custom_scripting &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-register-custom-scripting-transact-sql.md). Spécifiez la valeur 'custom_script' pour le paramètre **@type** et le chemin d'accès au script sur le serveur de publication pour le paramètre **@value**.  
  
     La prochaine fois qu'une modification de schéma pertinente est effectuée, ce script s'exécutera sur chaque Abonné au sein de la même transaction que la commande DDL. Quand la modification de schéma est effectuée, le script est désinscrit. Vous devez réinscrire le script pour qu'il soit exécuté après une modification de schéma ultérieure.  
  
## <a name="see-also"></a> Voir aussi  
 [Spécifier le mode de propagation des modifications des articles transactionnels](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)   
 [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)  
  
  
