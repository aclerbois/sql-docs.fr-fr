---
title: Publication de l’exécution de procédures stockées dans la réplication transactionnelle | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
- publishing [SQL Server replication], stored procedure execution
- articles [SQL Server replication], stored procedures and
- transactional replication, publishing stored procedure execution
ms.assetid: f4686f6f-c224-4f07-a7cb-92f4dd483158
caps.latest.revision: 40
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ae475c60cbbab9ebbf0f016606dd52d75e270106
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publishing-stored-procedure-execution-in-transactional-replication"></a>Publication de l'exécution de procédures stockées dans la réplication transactionnelle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Si vous avez une ou plusieurs procédures stockées qui s'exécutent sur le serveur de publication et qui affectent des tables publiées, vous pouvez envisager d'inclure ces procédures stockées dans votre publication en tant qu'articles d'exécution de procédure stockée. La définition de la procédure (l'instruction CREATE PROCEDURE) est répliquée vers l'Abonné quand l'abonnement est initialisé ; quand la procédure est exécutée sur le serveur de publication, la réplication exécute la procédure correspondante sur l'Abonné. Cela peut apporter des performances significativement meilleures dans les cas où de grosses opérations de traitement sont effectuées, car seule l'exécution de la procédure est répliquée, ce qui élimine la nécessité de répliquer les modifications individuelles pour chaque ligne. Supposons par exemple que vous créez la procédure stockée suivante dans la base de données de publication :  
  
```  
CREATE PROC give_raise AS  
UPDATE EMPLOYEES SET salary = salary * 1.10  
```  
  
 Cette procédure accorde à chacun des 10 000 employés de la société une augmentation de salaire de 10 %. Lorsque vous exécutez cette procédure stockée sur le serveur de publication, le salaire de chaque employé est mis à jour. Sans la réplication de l'exécution de la procédure stockée, la mise à jour est envoyée aux Abonnés comme une grosse transaction comportant plusieurs étapes :  
  
```  
BEGIN TRAN  
UPDATE EMPLOYEES SET salary = salary * 1.10 WHERE PK = 'emp 1'  
UPDATE EMPLOYEES SET salary = salary * 1.10 WHERE PK = 'emp 2'  
```  
  
 Et ceci se répète pour les 10 000 mises à jour.  
  
 Avec la réplication de l'exécution de procédure stockée, la réplication envoie seulement la commande pour exécuter la procédure stockée sur l'Abonné, au lieu d'écrire toutes les mises à jour dans la base de données de distribution, puis de les envoyer à l'Abonné via le réseau :  
  
```  
EXEC give_raise  
```  
  
> [!IMPORTANT]  
>  La réplication des procédures stockées ne convient pas à toutes les applications. Si un article est filtré horizontalement, de sorte que les ensembles de lignes soient différents sur l'éditeur et sur l'abonné, l'exécution de la même procédure stockée sur les deux serveurs donne des résultats différents. De même, si une mise à jour est basée sur la sous-requête d'une autre table non répliquée, l'exécution de la même procédure stockée  sur le serveur de publication et sur l'Abonné renvoie des résultats différents.  
  
 **Pour publier l'exécution d'une procédure stockée**  
  
-   SQL Server Management Studio : [Publier l’exécution d’une procédure stockée dans une publication transactionnelle &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/publish-execution-of-stored-procedure-in-transactional-publication.md)  
  
-   Programmation Transact-SQL de la réplication : exécutez [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) et spécifiez la valeur 'serializable proc exec' (recommandé) ou 'proc exec' pour le paramètre **@type**. Pour plus d’informations sur la définition d’articles, consultez [Définir un article](../../../relational-databases/replication/publish/define-an-article.md).  
  
## <a name="modifying-the-procedure-at-the-subscriber"></a>Modification de la procédure sur l'Abonné  
 Par défaut, la définition de la procédure stockée sur le serveur de publication est propagée vers chaque Abonné. Cependant, vous pouvez aussi modifier la procédure stockée sur l'Abonné. Ceci est utile si vous souhaitez que des logiques différentes soient exécutées sur le serveur de publication et sur l'Abonné. Considérons par exemple la procédure stockée sur le serveur de publication **sp_big_delete**, qui a deux fonctions : elle supprime 1 000 000 de lignes de la table répliquée **big_table1** et met à jour la table non répliquée **big_table2**. Pour solliciter moins de ressources réseau, vous pouvez transmettre la suppression de ce million de lignes en tant que procédure stockée en publiant **sp_big_delete**. Sur l'Abonné, vous pouvez modifier **sp_big_delete** afin qu'elle supprime le million de lignes sans effectuer ensuite la mise à jour de **big_table2**.  
  
> [!NOTE]  
>  Par défaut, toutes les modifications effectuées à l'aide de ALTER PROCEDURE sur le serveur de publication sont propagées vers l'Abonné. Pour éviter cela, désactivez la propagation des modifications de schéma avant d'exécuter ALTER PROCEDURE. Pour obtenir des informations sur les modifications de schéma, consultez [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
## <a name="types-of-stored-procedure-execution-articles"></a>Types d'articles d'exécution de procédure stockée  
 Il existe deux manières de publier l'exécution d'une procédure stockée : article d'exécution de procédure sérialisable et article d'exécution de procédure.  
  
-   L'option sérialisable est recommandée car elle réplique l'exécution de la procédure seulement si la procédure est exécutée dans le contexte d'une transaction sérialisable. Si la procédure stockée est exécutée en dehors d'une transaction sérialisable, les modifications apportées aux données dans les tables publiées sont répliquées sous la forme d'une série d'instructions DML. Ceci favorise la mise en cohérence des données côté abonné avec celles côté éditeur et s'avère particulièrement utile pour les opérations de traitement, telles que les opérations de nettoyage importantes.  
  
-   Avec l'option d'exécution de procédure, il est possible que l'exécution de la procédure soit répliquée vers tous les abonnés, indépendamment du fait que des instructions individuelles de la procédure stockée aient réussi ou non. En outre, étant donné que les modifications apportées aux données par la procédure stockée peuvent émaner de transactions multiples, il se peut que les données des abonnés ne soient pas identiques à celles du serveur de publication. Pour traiter ces problèmes, il est requis que les abonnés soient en lecture seule et que vous utilisiez un niveau d'isolement supérieur à la lecture non validée. Si vous utilisez une lecture non validée, les modifications apportées aux données dans les tables publiées sont répliquées comme une série d'instructions DML.  
  
 L'exemple suivant illustre l'intérêt de configurer une réplication de procédures en tant qu'articles de procédures sérialisables.  
  
```  
BEGIN TRANSACTION T1  
SELECT @var = max(col1) FROM tableA  
UPDATE tableA SET col2 = <value>   
   WHERE col1 = @var   
  
BEGIN TRANSACTION T2  
INSERT tableA VALUES <values>  
COMMIT TRANSACTION T2  
```  
  
 Dans l'exemple précédent, il est supposé que l'instruction SELECT de la transaction T1 intervient avant l'instruction INSERT de la transaction T2.  
  
 Si la procédure n'est pas exécutée dans une transaction sérialisable (avec le niveau d'isolement SERIALIZABLE), la transaction T2 sera autorisée à insérer une nouvelle ligne dans la plage de l'instruction SELECT de T1 et sa validation interviendra avant celle de T1. Cela signifie également qu'elle sera appliquée sur l'abonné avant T1. Lorsque T1 est appliquée sur l'abonné, l'instruction SELECT peut, le cas échéant, renvoyer une valeur différente de celle issue de l'application sur l'éditeur et aboutir à un résultat différent de celui de l'instruction UPDATE.  
  
 Si la procédure est exécutée dans une transaction sérialisable, la transaction T2 ne sera pas autorisée à opérer des insertions dans la plage couverte par l'instruction SELECT de T2. Elle sera neutralisée jusqu'à la validation de T1, ce qui garantit des résultats similaires sur l'abonné.  
  
 Les verrous sont conservés plus longtemps lorsque vous exécutez la procédure dans une transaction sérialisable et peuvent aboutir à une concurrence d'accès réduite.  
  
## <a name="the-xactabort-setting"></a>Le paramètre XACT_ABORT  
 Lors de la réplication de l'exécution d'une procédure stockée, le paramétrage de la session exécutant la procédure stockée doit spécifier XACT_ABORT ON. Si XACT_ABORT est défini à OFF et qu'une erreur se produit lors de l'exécution de la procédure sur le serveur de publication, la même erreur se produira sur l'Abonné, provoquant l'échec de l'Agent de distribution. Le fait de spécifier XACT_ABORT ON garantit que toute erreur rencontrée lors de l'exécution sur le serveur de publication provoque l'annulation de la totalité de l'exécution, évitant ainsi l'échec de l'Agent de distribution. Pour plus d’informations sur la définition de XACT_ABORT, consultez [SET XACT_ABORT &#40;Transact-SQL&#41;](../../../t-sql/statements/set-xact-abort-transact-sql.md).  
  
 Si vous devez définir le paramètre XACT_ABORT à OFF, spécifiez le paramètre **-SkipErrors** pour l'Agent de distribution. Cela permet à l'agent de continuer l'application des modifications sur l'Abonné même si une erreur est rencontrée.  
  
## <a name="see-also"></a> Voir aussi  
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
