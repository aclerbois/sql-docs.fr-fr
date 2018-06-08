---
title: Résoudre les problèmes de récupération des données avec des rapports Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7680946a-1660-4b59-a03a-c4d474cd8ed3
caps.latest.revision: 4
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3503952eb791c4423826626b98e8f8feb2bce0c7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-data-retrieval-issues-with-reporting-services-reports"></a>Dépanner des problèmes de récupération des données avec des rapports Reporting Services
La première étape de traitement d'un rapport consiste à récupérer les données du rapport pour chaque dataset en exécutant la requête de dataset. Lorsque vous affichez l'aperçu d'un rapport localement, vos connexions à la source de données et vos informations d'identification doivent avoir des autorisations suffisantes pour récupérer les données sur votre ordinateur. Lorsque vous exécutez un rapport sur le serveur de rapports, les connexions à la source de données et les informations d'identification doivent avoir des autorisations suffisantes pour récupérer les données sur le serveur de rapports. Utilisez cette rubrique pour vous aider à résoudre des problèmes de récupération des données de rapport.   
  
## <a name="i-cannot-create-a-connection-to-a-data-source"></a>Je ne parviens pas à créer une connexion à une source de données  
Lorsque vous créez une source de données, exécutez une requête de dataset ou affichez l'aperçu d'un rapport, vous pouvez obtenir le message suivant : « Impossible de créer une connexion à la source de données `<data source name>`».   
    
### <a name="data-source-is-not-available"></a>La source de données n'est pas disponible.  
La source de données est hors connexion ou non disponible pour une raison quelconque.   
  
Vérifiez que vous avez accès à la source de données et qu'elle est disponible. Par exemple, utilisez SQL Server Management Studio pour vous connecter à la source de données. Pour les bases de données relationnelles et multidimensionnelles, utilisez le bouton **Test** de la boîte de dialogue **Propriétés de connexion** pour vérifier la connexion et les autorisations relatives à la source de données.   
  
### <a name="data-source-credentials-are-not-valid"></a>Les informations d'identification de la source de données ne sont pas valides.  
Les informations d'identification que vous utilisez pour vous connecter à la source de données ont des autorisations insuffisantes pour récupérer les données spécifiées dans la requête.  
  
Vérifiez que vous utilisez bien les informations d'identification appropriées. Par exemple, vous pouvez avoir l'autorisation de récupérer des données d'une table ou d'une vue, mais pas pour une colonne spécifique ; ou vous pouvez ne pas avoir les autorisations suffisantes pour exécuter une procédure stockée qui remplit une vue.   
  
> [!NOTE]  
> Les autorisations que vous utilisez pour récupérer des données pour afficher l'aperçu d'un rapport peuvent être différentes de celles qui sont requises pour récupérer des données après la publication d'un rapport sur un serveur de rapports.   
  
### <a name="not-a-valid-password"></a>Mot de passe non valide  
Pour des sources de données avec des informations d'identification demandées ou spécifiées dans la chaîne de connexion, les caractères du mot de passe sont passés aux pilotes de la source de données sous-jacente. Si le mot de passe ou la chaîne contient des caractères spéciaux tels que des signes de ponctuation, certains pilotes de source de données ne peuvent pas les valider.   
  
Vérifiez que le mot de passe ne contient pas de caractères spéciaux. Si le changement du mot de passe s'avère impossible, demandez à votre administrateur de base de données de stocker les informations d'identification appropriées localement et sur le serveur en tant que nom de source de données (DSN) ODBC. Pour en savoir plus, voir « OdbcConnection.ConnectionString » dans la documentation du Kit de développement logiciel (SDK) sur MSDN.   
  
> [!NOTE]  
>Il est recommandé de ne pas ajouter d'informations de connexion, notamment des mots de passe, à la chaîne de connexion. Le Concepteur de rapports comprend une page **Informations d’identification** dans les boîtes de dialogue [Propriétés de la source de données](~/reporting-services/report-data/enter-data-source-credentials-dialog-box-report-builder.md) ou [Propriétés de la source de données partagée](~/reporting-services/report-data/enter-data-source-credentials-dialog-box-report-builder.md) où vous pouvez entrer des informations d’identification. Ces informations d'identification sont stockées de façon sécurisée sur l'ordinateur de création de rapports.  
  
## <a name="why-do-i-see-no-data-when-i-run-my-query-in-the-query-designer"></a>Pourquoi aucune donnée ne s'affiche lorsque j'exécute ma requête dans le concepteur de requêtes ?  
Lorsque vous créez un dataset, la collection de champs de dataset apparaît dans le volet Données du rapport. Parfois, la collection de champs de dataset n'apparaît pas comme prévu.   
  
### <a name="import-query-does-not-import-calculated-fields"></a>La requête d'importation n'importe pas les champs calculés  
  
Bien que les champs calculés soient enregistrés dans une définition de rapport, ils ne sont pas inclus lorsque vous importez une requête de dataset à partir d'un autre rapport. Seuls les champs spécifiés par la requête de dataset apparaissent dans le volet des données de rapportlorsque vous créez un dataset en important une requête à partir d'un autre rapport.   
  
Pour afficher des champs calculés dans le volet Données du rapport, vous devez les définir pour chaque rapport dans lequel ils sont utilisés.   
  
### <a name="some-data-providers-do-not-support-automatic-population-of-the-dataset-field-collection"></a>Certains fournisseurs de données ne prennent pas en charge le remplissage automatique de la collection de champs de dataset  
Lorsque vous définissez une requête dans la boîte de dialogue Propriétés du dataset, puis fermez la boîte de dialogue, la collection de champs de dataset apparaît habituellement dans le volet Données du rapport. Pour certaines sources de données, la collection de champs de dataset n'est pas remplie automatiquement.   
  
Pour remplir la collection de champs de dataset, procédez comme suit :  
* Assurez-vous que vous bénéficiez des autorisations nécessaires pour récupérer les informations des champs de la base de données. Pour certaines sources de données, vous disposez peut-être d'autorisations vous permettant d'accéder à la source de données, mais pas à la table ou à la colonne. Vous pouvez avoir l'autorisation d'accéder à une vue, mais vous n'êtes peut-être pas autorisé à exécuter les procédures stockées qui créent la vue. Pour valider votre accès à des tables ou à des colonnes spécifiques dans une base de données, vérifiez les résultats de votre requête dans une application séparée telle que SQL Server Management Studio, en tirant parti des mêmes autorisations que celles qui sont utilisées pour le rapport. Si vous ne pouvez pas voir les résultats désirés pour votre requête, faites appel à l'administrateur système pour modifier vos autorisations d'accès aux données.   
* Exécutez la requête dans le volet de requêtes de la boîte de dialogue **Propriétés du dataset** . Pour en savoir plus, voir [Ajouter des données à un rapport (Générateur de rapports et SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md).  
* Ajoutez des champs manuellement. Pour en savoir plus, [Procédure : ajouter, modifier ou actualiser des champs dans le volet des données de rapport (Générateur de rapports version 3.0 et SSRS)](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).   
  
## <a name="see-also"></a> Voir aussi  
[Erreurs et événements (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]



