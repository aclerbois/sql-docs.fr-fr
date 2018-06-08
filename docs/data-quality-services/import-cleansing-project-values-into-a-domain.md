---
title: Importer des valeurs de projet de nettoyage dans un domaine | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dqs.kb.importprojectvalues.f1
ms.assetid: f23e38e2-39e0-42d7-abd5-34d8fcca5d2a
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 90333ec5e27254ee54c42ac02be355c94c6bb289
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-cleansing-project-values-into-a-domain"></a>Importer des valeurs de projet de nettoyage dans un domaine

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS), vous pouvez importer les connaissances de qualité des données collectées pendant le processus de nettoyage dans un projet de nettoyage de qualité des données ou un package Integration Services qui contient le composant de nettoyage DQS dans un domaine. Cela garantit que des connaissances approuvées ne sont pas perdues, et que la base de connaissances est améliorée en permanence.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Pour importer les valeurs d'un projet de nettoyage dans un domaine, le domaine doit avoir été utilisé dans le projet de nettoyage dans le client de qualité des données ou dans le package Integration Services qui contient un composant de nettoyage DQS.  
  
-   Le projet de nettoyage dans le client de qualité des données ou le package Integration Services qui contient le composant de nettoyage DQS doit s'être correctement terminé.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour importer les connaissances de qualité des données collectées pendant le processus de nettoyage vers un domaine.  
  
##  <a name="Import"></a> Importer des valeurs de projet de nettoyage  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Sur l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez une base de connaissances dans l'activité Gestion de l'arborescence du domaine.  
  
3.  Si vous ajoutez des valeurs à un domaine existant, sélectionnez le domaine dans la liste des domaines.  
  
4.  Cliquez sur l'onglet **Valeurs du domaine** , cliquez sur l'icône **Importer des valeurs** dans la barre d'icônes, puis cliquez sur **Importer des valeurs de projet**. La boîte de dialogue **Importer des valeurs de projet** apparaît avec la liste des projets de qualité des données et des packages Integration Services qui ont été nettoyés à l'aide du domaine.  
  
    > [!NOTE]  
    >  Si aucun projet utilisant le domaine ou l'un de ses domaines liés n'a été créé, ou si le projet n'est pas terminé, l'option **Importer des valeurs de projet** n'est pas disponible.  
  
5.  Dans la boîte de dialogue **Importer des valeurs de projet** :  
  
    -   Sélectionnez **Tout** dans la liste déroulante **Importés** pour afficher tous les projets, ou sur **Non** pour afficher uniquement les projets dont les valeurs n'ont pas encore été importées.  
  
    -   Sélectionnez le projet à partir duquel vous voulez importer des valeurs.  
  
    -   Sélectionnez **Ajouter des valeurs à partir de l'onglet Nouveau** pour importer des valeurs du nouvel onglet, en plus des valeurs des onglets **Correct** et **Corrigés** .  
  
    -   Cliquez sur **OK**.  
  
6.  Vous revenez à l'onglet **Valeurs du domaine** et un message s'affiche indiquant l'importation réussie des valeurs. Les valeurs qui ont été importées, et qui sont donc nouvelles dans le domaine, s'affichent dans la table **Valeurs** .  
  
7.  Désélectionnez **Afficher seulement les nouvelles valeurs** pour afficher toutes les valeurs qui sont dans le domaine.  
  
8.  Sélectionnez **Correct**, **Erreur**ou **Non valides** pour afficher uniquement les valeurs du type sélectionné.  
  
9. Pour rechercher une chaîne spécifique, entrez la chaîne dans la zone de texte **Rechercher** . Cliquez sur la flèche haut ou bas pour parcourir les valeurs qui répondent aux critères de recherche. Elles sont mises en surbrillance en jaune.  
  
10. Cliquez sur **Terminer**.  
  
    > [!NOTE]  
    >  Pour plus d'informations sur l'utilisation des valeurs dans l'onglet **Valeurs du domaine** , consultez [Change Domain Values](../data-quality-services/change-domain-values.md).  
  
##  <a name="FollowUp"></a> Suivi : Après l'importation de valeurs de projet de nettoyage dans un domaine  
 Après avoir importé les connaissances de qualité des données collectées pendant le processus de nettoyage dans un domaine, vous pouvez effectuer d'autres tâches de gestion de domaine sur le domaine et les valeurs. Pour plus d’informations, consultez [Gestion d’un domaine](../data-quality-services/managing-a-domain.md).  
  
##  <a name="Values"></a> Valeurs qui seront importées  
 Les valeurs suivantes seront importées à partir d'un projet vers un domaine :  
  
-   Seules les valeurs de chaîne sont importées vers le domaine.  
  
-   Seules les valeurs des onglets **Correct**, **Corrigés**, et **Nouveau** disponibles dans la page **Gérer et afficher les résultats** de l'activité **Nettoyage** seront importées. Les valeurs de l'onglet **Nouveau** seront importées uniquement si la case à cocher **Ajouter des valeurs à partir de l'onglet Nouveau** de la boîte de dialogue **Importer des valeurs de projet** a été activée.  
  
-   Les valeurs seront importées comme correctes ou en tant qu'erreur avec sa correction. Seule une valeur d'erreur avec une valeur de correction sera importée.  
  
-   La valeur de correction sera soit une nouvelle valeur qui n'existe pas dans la base de connaissances, soit une valeur correcte existante.  
  
-   Seules les corrections effectuées au niveau des valeurs, et non pas au niveau des enregistrements, seront importées dans la base de connaissances.  
  
-   Les valeurs non valides seront créées si la valeur importées contredit une règle de domaine.  
  
-   Si vous importez des valeurs à partir de plusieurs projets à la fois, les valeurs sont importées en ordre séquentiel.  
  
-   Une correction effectuée suite à une relation à base de termes dans un domaine est importée en tant que valeur correcte (et non en tant qu'erreur).  
  
##  <a name="ValuesNot"></a> Valeurs qui ne seront pas importées  
 Les valeurs suivantes ne seront pas importées à partir d'un projet vers un domaine :  
  
-   Les valeurs des onglets **Suggérés** et **Non valides** disponibles dans la page **Gérer et afficher les résultats** de l'activité **Nettoyage** ne seront pas importées.  
  
-   Si une valeur trouvée dans le projet de nettoyage contredit une valeur existante dans le domaine, la valeur trouvée dans le projet est ignorée. Cela inclut les conflits entre les valeurs de nettoyage et celles de la base de connaissances.  
  
-   Les corrections effectuées au niveau des enregistrements ne seront pas importées dans la base de connaissances.  
  
-   Aucune valeur ne sera importée vers un domaine si la valeur qu'elle remplacerait a été corrigée ou approuvée comme correcte par un service de données de référence.  
  
-   Si une valeur de correction apparaît dans la base de connaissances comme valeur non valide ou comme valeur d'erreur, ni l'erreur ni la valeur de correction ne seront importées.  
  
-   Si le domaine fait partie d'un domaine composite, et que le nettoyage a été effectué sur le domaine composite, aucune valeur ne sera importée.  
  
-   Vous pouvez importer des valeurs d'un projet uniquement lorsque la base de connaissances a l'état En cours et que la base de connaissances est verrouillée par l'utilisateur qui effectue l'importation.  
  
## <a name="see-also"></a> Voir aussi  
 [Nettoyage des données](../data-quality-services/data-cleansing.md)   
 [Transformation de nettoyage DQS](../integration-services/data-flow/transformations/dqs-cleansing-transformation.md)  
  
  
