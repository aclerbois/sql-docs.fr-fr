---
title: Convertir le code R pour une utilisation dans SQL Server Machine Learning Services | Documents Microsoft »
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: bf3d272bd4b5c1227aa8a1969727ea17f5b65596
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="convert-r-code-for-execution-in-sql-server-in-database-instances"></a>Convertir le code R pour l’exécution dans les instances SQL Server (de-de base de données)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit des instructions détaillées sur la façon de modifier le code R à utiliser dans SQL Server. 

Lorsque vous déplacez le code R à partir de R Studio ou un autre environnement pour SQL Server, plus souvent le code fonctionne sans modification supplémentaire : par exemple, si le code est simple, comme une fonction qui accepte certaines entrées et retourne une valeur. Il est également plus facile pour les solutions de port qui utilisent la **RevoScaleR** ou **MicrosoftML** packages, qui prennent en charge l’exécution dans des contextes d’exécution différents avec des modifications minimes.

Toutefois, votre code peut nécessiter des modifications substantielles si une des conditions suivantes est remplie :

+ Vous utilisez des bibliothèques R qui accèdent au réseau ou qui ne peut pas être installé sur SQL Server.
+ Le code effectue des appels séparés à des sources de données en dehors de SQL Server, tels que des feuilles de calcul Excel, les fichiers sur des partages et les autres bases de données. 
+ Vous souhaitez exécuter le code de la *@script* paramètre de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) et également paramétrer la procédure stockée.
+ Votre solution d’origine inclut plusieurs étapes qui peuvent être plus efficaces dans un environnement de production si exécutée indépendamment, telles que la préparation des données ou d’ingénierie de fonctionnalité et modèle d’apprentissage, de calcul de score ou le rapport.
+ Vous souhaitez améliorer optimiser les performances en modifiant les bibliothèques, à l’aide de l’exécution en parallèle ou le déchargement d’un traitement à SQL Server. 

## <a name="step-1-plan-requirements-and-resources"></a>Étape 1. Planifier les besoins et des ressources

**Packages**

+ Déterminer les packages qui sont nécessaires et vous assurer qu’elles fonctionnent sur SQL Server.
 
+ Installation des packages à l’avance, dans la bibliothèque de package par défaut utilisée par les Services de Machine Learning. Bibliothèques utilisateur ne sont pas pris en charge.

**Sources de données** 

+ Si vous souhaitez incorporer votre code R dans [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), identifiez les sources de données principal et secondaire. 

    + **Principal** des sources de données sont des jeux de données volumineux, tels que les données d’apprentissage du modèle, ou les données d’entrée pour les prévisions. Envisagez de mapper votre plus grand jeu de données pour le paramètre d’entrée de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

    + **Secondaire** des sources de données sont généralement plus petits jeux de données, telles que les listes des facteurs ou des variables de regroupement supplémentaire. 
    
    Actuellement, sp_execute_external_script prend en charge uniquement un seul jeu de données en tant qu’entrée à la procédure stockée. Toutefois, vous pouvez ajouter plusieurs entrées de scalaires ou binaire.

    Précédé d’exécuter des appels de procédure stockée ne peut pas être utilisés en tant qu’entrée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Vous pouvez utiliser des requêtes, des vues ou toute autre instruction SELECT valide.

+ Déterminer les sorties que vous avez besoin. Si vous exécutez le code R à l’aide de sp_execute_external_script, la procédure stockée peut sortie ainsi qu’une trame de données. Toutefois, vous pouvez également produire plusieurs sorties scalaires, y compris des graphiques et des modèles dans un format binaire, ainsi que d’autres valeurs scalaires dérivé des paramètres code R ou SQL.

**Types de données**

+ Dressez une liste des problèmes possibles liés aux types des données.

    Tous les types de données R sont pris en charge par SQL Server machine Learning Services. Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge une plus grande diversité de types de données r. Par conséquent, certaines conversions de types de données implicites sont effectuées lors de l’envoi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] données R et vice versa. Vous devrez peut-être convertir explicitement ou de convertir certaines données. 

    Les valeurs NULL sont prises en charge. Toutefois, R utilise le `na` construction de données pour représenter une valeur manquante, qui est similaire à une valeur null.

+ Envisager la suppression de la dépendance sur les données qui ne peut pas être utilisées par r : par exemple, les ID de ligne et les types de données GUID à partir de SQL Server ne peut pas être consommés par R et génèrent des erreurs.

    Pour plus d’informations, consultez [les Types de données et les bibliothèques R](../r/r-libraries-and-data-types.md).

## <a name="step-2-convert-or-repackage-code"></a>Étape 2. Convertir ou de réorganiser le code

Combien vous modifiez votre code varie selon que vous souhaitez soumettre le code R à partir d’un client distant à exécuter dans le contexte de calcul de SQL Server, ou envisagez de déployer le code dans le cadre d’une procédure stockée, ce qui peut fournir de meilleures performances et la sécurité des données. Encapsulant votre code dans une procédure stockée impose certaines exigences supplémentaires. 

+ Définissez vos données d’entrée principales en tant qu’une requête SQL dans la mesure du possible, afin d’éviter le déplacement des données.

+ Lors de l’exécution R dans une procédure stockée, vous pouvez passer à travers plusieurs **scalaire** entrées. Pour tous les paramètres que vous souhaitez utiliser dans la sortie, ajoutez le **sortie** (mot clé). 

    Par exemple, l’entrée scalaire suivante `@model_name` contient le nom du modèle, qui est également fourni dans sa propre colonne dans les résultats :

    ```SQL
    EXEC sp_execute_external_script @model_name="DefaultModel" OUTPUT, @language=N'R', @script=N'R code here'
    ``` 

+ Toutes les variables que vous passez en tant que paramètres de la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) doivent être mappés à des variables dans le code R. Par défaut, les variables sont mappées par nom.

    Toutes les colonnes dans le jeu de données d’entrée doivent également être mappés à des variables dans le script R.  Par exemple, supposons que votre script R contient une formule comme celle-ci :

    ```R
    formula <- ArrDelay ~ CRSDepTime + DayOfWeek + CRSDepHour:DayOfWeek
    ```
    
    Une erreur est générée si le jeu de données d’entrée ne contient pas de colonnes avec la correspondance des noms ArrDelay, CRSDepTime, DayOfWeek, CRSDepHour et DayOfWeek.

+ Dans certains cas, un schéma de sortie doit être défini au préalable pour les résultats.

    Par exemple, pour insérer les données dans une table, vous devez utiliser le **avec jeu de résultats** clause pour spécifier le schéma.

    Le schéma de sortie est également requis si le script R utilise l’argument `@parallel=1`. Cela s’explique par le fait que plusieurs processus peuvent être créés par SQL Server pour exécuter la requête en parallèle et collecter les résultats à la fin. Par conséquent, le schéma de sortie doit être préparé avant de pouvoir créer les processus parallèles.
    
    Dans d’autres cas, vous pouvez omettre le schéma de résultats à l’aide de l’option **avec RESULT SETS UNDEFINED**. Cette instruction renvoie le jeu de données à partir du script R, sans les colonnes d’affectation de noms ou en spécifiant les types de données SQL.

+ Envisagez de générer des données de minutage ou de suivi à l’aide de T-SQL plutôt que R.

    Par exemple, vous pouvez passer l’heure système ou autres informations utilisées pour l’audit et de stockage en ajoutant un appel de T-SQL qui est transmis aux résultats, plutôt que de générer des données similaires dans le script R. 

**Améliorer les performances et sécurité**

+ Évitez d’écrire des prédictions ou des résultats intermédiaires dans le fichier. Écrire des prédictions dans une table à la place, pour éviter le déplacement des données.

+ Exécutez toutes les requêtes à l’avance et passez en revue les plans de requête SQL Server pour identifier les tâches qui peuvent être effectuées en parallèle.

    Si la requête d’entrée peut être parallélisée, définissez `@parallel=1` dans le cadre de vos arguments à [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). 

    Le traitement parallèle avec cet indicateur est généralement possible quand SQL Server peut utiliser des tables partitionnées ou distribuer une requête entre plusieurs processus et agréger les résultats à la fin. En revanche, il n’est généralement pas possible si vous effectuez l’apprentissage de modèles à l’aide d’algorithmes qui nécessitent la lecture de toutes les données, ou si vous avez besoin de créer des agrégats.

+ Examinez votre code R pour déterminer si certaines étapes peuvent être effectuées séparément, ou de manière plus efficace, à l’aide d’un appel de procédure stockée distinct. Par exemple, vous pouvez obtenir de meilleures performances en effectuant des ingénierie de fonctionnalité ou l’extraction de la fonctionnalité séparément et en enregistrant les valeurs dans une table.

+ Recherchez des façons d’utiliser T-SQL au lieu du code R pour effectuer des calculs reposant sur des ensembles.

    Par exemple, cette solution R montre comment défini par l’utilisateur des fonctions T-SQL et R peut effectuer la même tâche d’ingénierie de fonctionnalité : [procédure pas à pas au bout de science des données](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md).

+ Si possible, remplacez les fonctions R classiques avec **ScaleR** fonctions qui prennent en charge l’exécution distribuée. Pour plus d’informations, consultez [comparaison de Base de R et des fonctions R échelle](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler-compared-to-base-r).

+ Consulter un développeur de base de données pour déterminer les moyens d’améliorer les performances à l’aide des fonctionnalités de SQL Server comme [tables optimisées en mémoire](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables), ou, si vous avez Enterprise Edition, [du gouverneur de ressources](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor)).

    Pour plus d’informations, consultez [SQL Server l’optimisation des astuces pour les Services d’Analytique](https://gallery.cortanaintelligence.com/Tutorial/SQL-Server-Optimization-Tips-and-Tricks-for-Analytics-Services)

### <a name="step-3-prepare-for-deployment"></a>Étape 3. Préparer le déploiement

+ Prévenez l’administrateur afin que vous puissiez installer et tester les packages avant le déploiement de votre code. 

    Dans un environnement de développement, il peut être OK installer des packages dans le cadre de votre code, mais il s’agit d’une mauvaise pratique dans un environnement de production. 

    Bibliothèques utilisateur ne sont pas pris en charge, qu’à l’aide d’une procédure stockée ou du code R en cours d’exécution dans le contexte de calcul de SQL Server.

**Package de votre code R dans une procédure stockée**

+ Si votre code est relativement simple, vous pouvez l’incorporer dans une fonction définie par l’utilisateur de T-SQL sans modification, comme décrit dans ces exemples :

    + [Créer une fonction R qui s’exécute dans rxExec](..\tutorials\deepdive-create-a-simple-simulation.md)
    + [Ingénierie de fonctionnalité à l’aide de T-SQL et R](..\tutorials\sqldev-create-data-features-using-t-sql.md)

+ Si le code est plus complexe, utilisez le package R **sqlrutils** à convertir votre code. Ce package est conçu pour aider les utilisateurs expérimentés de R à écrire du code de procédure stockée correct. 

    La première étape consiste à réécrire votre code R en tant qu’une seule fonction avec clairement des entrées et sorties.

    Ensuite, utilisez le **sqlrutils** package pour générer l’entrée et les sorties dans le format correct. Le **sqlrutils** package génère le code de procédure stockée complète pour vous et peuvent également inscrire la procédure stockée dans la base de données. 

    Pour plus d’informations et d’exemples, consultez [SqlRUtils](../r/generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package.md).

**Intégrer à d’autres flux de travail**

+ Tirer parti des outils de T-SQL et les processus ETL. Effectuer une ingénierie de fonctionnalité, d’extraction de fonctionnalité et de nettoyage des données à l’avance en tant que partie du flux de travail de données.

    Lorsque vous travaillez dans un environnement de développement R dédié comme [!INCLUDE[rsql_rtvs_md](../../includes/rsql-rtvs-md.md)] ou RStudio, vous pourrez extraire des données sur votre ordinateur, analyser les données de manière itérative, puis écrire ou afficher les résultats. 
    
    Toutefois, lorsque le code de R autonome est migré vers SQL Server, une grande partie de ce processus peut être simplifiée ou déléguée à d’autres outils de SQL Server. 

+ Utiliser des stratégies de visualisation sécurisée, asynchrone.

    Fréquence à laquelle les utilisateurs de SQL Server ne peut pas accéder aux fichiers sur le serveur et outils clients SQL ne gèrent en général pas le périphérique d’affichage R. Si vous générez des graphiques ou autres graphiques dans le cadre de la solution, envisagez d’exportation les graphiques en tant que données binaires et de l’enregistrement dans une table ou de l’écriture.

+ Encapsuler la prédiction et les fonctions de score dans les procédures stockées pour l’accès direct par les applications.

### <a name="other-resources"></a>Autres ressources

Pour afficher des exemples de la façon dont une solution R peut être déployée dans SQL Server, consultez les exemples suivants :

+ [Créer un modèle prédictif pour les entreprises de location de ski à l’aide de R et SQL Server](https://microsoft.github.io/sql-ml-tutorials/R/rentalprediction/)

+ [Analytique dans base de données pour le développeur SQL](../tutorials/sqldev-in-database-r-for-sql-developers.md) montre comment vous pouvez effectuer votre code R plus modulaire en l’encapsulant dans les procédures stockées

+ [Solution de science des données de bout en bout](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) inclut une comparaison d’ingénierie de fonctionnalité dans R et T-SQL
