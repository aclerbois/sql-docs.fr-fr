---
title: Erreurs de script R dans SQL Server Machine Learning et R Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/31/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 0b695111849b234f6ca38fc89c538e905f187fb6
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34709100"
---
# <a name="r-scripting-errors-in-sql-server"></a>Erreurs de script R dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit plusieurs i.inittabDans gerrors lors de l’exécution de code R dans SQL Server. La liste n’est pas exhaustive. Il existe de nombreux packages et les erreurs peuvent varier entre les versions du même package. Nous vous recommandons d’erreurs de script de validation sur le [Forum sur la Machine Learning Server](https://social.msdn.microsoft.com/Forums/home?category=MicrosoftR), qui prend en charge les composants utilisés dans R Services (de-de base de données), le Client Microsoft R et Microsoft R Server d’apprentissage automatique.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services


## <a name="valid-script-fails-in-t-sql-or-in-stored-procedures"></a>Échec de script valide dans T-SQL ou dans les procédures stockées

Avant d’encapsuler votre code R dans une procédure stockée, il est judicieux d’exécuter votre code R dans un IDE externe, ou dans un des outils tels que RTerm ou RGui R. À l’aide de ces méthodes, vous pouvez tester et déboguer le code en utilisant les messages d’erreur détaillés sont retournés par R.

Toutefois, parfois le code qui fonctionne parfaitement dans un IDE ou l’utilitaire externe peut échouer à s’exécuter dans une procédure stockée ou le contexte de calcul dans un serveur SQL Server. Dans ce cas, il existe de nombreux problèmes pour rechercher des avant que vous pouvez supposer que le package ne fonctionne pas dans SQL Server.

1. Vérifiez si le Launchpad est en cours d’exécution.

2. Passez en revue les messages pour voir si les données d’entrée ou les données de sortie contient des colonnes avec des types de données incompatibles ou non pris en charge. Par exemple, les requêtes sur une base de données SQL retournent souvent GUID ou ROWGUID, qui sont tous deux non pris en charge. Pour plus d’informations, consultez [R bibliothèques et types de données](r/r-libraries-and-data-types.md).

3. Passez en revue les pages d’aide pour les fonctions R individuelles déterminer si tous les paramètres sont pris en charge pour le contexte de calcul de SQL Server. Pour obtenir l’aide ScaleR, utilisez les commandes d’aide inline R, ou consultez [une référence au Package](https://docs.microsoft.com/r-server/r-reference/revoscaler/revoscaler).

Si le runtime R fonctionne, mais votre script retourne des erreurs, nous vous recommandons d’essayer de déboguer le script dans un environnement de développement R dédié, telles que les outils R pour Visual Studio.

Nous vous recommandons également que vous passez en revue et légèrement Réécrivez le script pour corriger les problèmes liés aux types de données qui peuvent se produire lorsque vous déplacez des données entre R et le moteur de base de données. Pour plus d’informations, consultez [R bibliothèques et types de données](r/r-libraries-and-data-types.md).

En outre, vous pouvez utiliser le package sqlrutils pour regrouper votre script R dans un format qui est plus simple qu’une procédure stockée. Pour plus d'informations, consultez :
* [Générer une procédure stockée pour le code R à l’aide du package sqlrutils](r/generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package.md)
* [Créer une procédure stockée à l’aide de sqlrutils](r/how-to-create-a-stored-procedure-using-sqlrutils.md)

## <a name="script-returns-inconsistent-results"></a>Script retourne des résultats incohérents

Des scripts R peuvent retourner des valeurs différentes dans un contexte de SQL Server, pour plusieurs raisons :

- Conversion de type implicite est automatiquement effectuée sur certains types de données lorsque les données sont transmises entre SQL Server et R. Pour plus d’informations, consultez [R bibliothèques et types de données](r/r-libraries-and-data-types.md).

- Déterminez si le nombre de bits est un facteur. Par exemple, il existe souvent des différences dans les résultats des opérations mathématiques pour les bibliothèques de point flottant 32 bits et 64 bits.

- Déterminer si les valeurs NaN ont été générés pour les opérations. Cela peut invalider les résultats.

- Petites différences peuvent être amplifiées lorsque vous effectuez un réciproque d’un nombre proche de zéro.

- Les erreurs d’arrondi cumulées peuvent entraîner des éléments tels que les valeurs qui sont inférieures à zéro au lieu de zéro.

## <a name="implied-authentication-for-remote-execution-via-odbc"></a>Authentification implicite pour l’exécution à distance via ODBC

Si vous vous connectez à la [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] commandes de l’ordinateur pour exécuter R à l’aide de la **RevoScaleR** fonctions, vous pouvez obtenir une erreur lorsque vous utilisez des appels ODBC qui écrivent des données sur le serveur. Cette erreur se produit uniquement lorsque vous utilisez l’authentification Windows.

La raison est que les comptes de travail qui sont créés pour R Services n’ont pas d’autorisation de se connecter au serveur. Par conséquent, les appels ODBC ne peut pas être exécutées en votre nom. Le problème ne se produit pas avec les connexions SQL, car, avec les connexions SQL, les informations d’identification sont transmises explicitement à partir du client de R à l’instance de SQL Server, puis à ODBC. Toutefois, à l’aide de connexions SQL est également moins sécurisée que l’authentification Windows.

Pour activer vos informations d’identification Windows à passer en toute sécurité à partir d’un script qui a lancé à distance, SQL Server doit émuler vos informations d’identification. Ce processus est appelé _l’authentification implicite_. Pour ce faire, les comptes de travail qui exécutent des scripts R ou Python sur l’ordinateur SQL Server doivent avoir les autorisations appropriées.

1. Ouvrez [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] en tant qu’administrateur sur l’instance où vous souhaitez exécuter le code R.

2. Exécutez le script suivant. Veillez à modifier le nom du groupe d’utilisateurs, si vous avez modifié la valeur par défaut et les noms d’ordinateur et d’instance.

    ```SQL
    USE [master]
    GO
    
    CREATE LOGIN [computername\\SQLRUserGroup] FROM WINDOWS WITH
    DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ```

## <a name="avoid-clearing-the-workspace-while-youre-running-r-in-a-sql-compute-context"></a>Évitez de désactiver l’espace de travail en cours d’exécution R dans un contexte de calcul SQL

Bien qu’il soit courant d’effacement de l’espace de travail lorsque vous travaillez dans la console R, il peut avoir des conséquences inattendues dans SQL contexte de calcul.

`revoScriptConnection` est un objet dans l’espace de travail R qui contient des informations sur une session R appelée à partir de SQL Server. Toutefois, si votre code R inclut une commande pour effacer l’espace de travail (tels que `rm(list=ls())`), toutes les informations sur la session et d’autres objets dans l’espace de travail R sont également désactivées.

Pour résoudre ce problème, évitez d’effacement manque de discernement dans des variables et autres objets en cours d’exécution R dans SQL Server. Vous pouvez supprimer des variables spécifiques à l’aide de la **supprimer** fonction :

```R
remove('name1', 'name2', ...)
```

S’il existe plusieurs variables à supprimer, nous vous suggérons d’enregistrer les noms des variables temporaires à une liste et ensuite effectuer périodiques garbage collection sur la liste.



## <a name="next-steps"></a>Étapes suivantes

[Problèmes connus et dépannage de machine Learning Services](machine-learning-troubleshooting-faq.md)

[Collecte de données pour la résolution des problèmes d’apprentissage](data-collection-ml-troubleshooting-process.md)

[FAQ sur la mise à niveau et l’installation](r/upgrade-and-installation-faq-sql-server-r-services.md)

[Résoudre les problèmes de connexions du moteur de base de données](../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)
