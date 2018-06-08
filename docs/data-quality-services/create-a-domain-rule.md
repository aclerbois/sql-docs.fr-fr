---
title: Créer une règle de domaine | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2011
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
- sql13.dqs.dm.testdomainrule.f1
- sql13.dqs.dm.rules.f1
ms.assetid: 339fa10d-e22c-4468-b366-080c33f1a23f
caps.latest.revision: 28
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 3b1103a3528674c8541aa04569e0d63a17ddcefc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-domain-rule"></a>Créer une règle de domaine

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment créer une règle de domaine dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Une règle de domaine est une condition utilisée pour valider, corriger et normaliser les valeurs de domaine. Une règle de domaine doit avoir la valeur true sur un domaine pour que les valeurs de domaine soient considérées comme exactes et conformes aux besoins de l'entreprise. Les règles de domaine peuvent comprendre les règles de validation qui sont utilisées pour valider les valeurs de domaine, mais ne sont pas utilisées pour corriger les données dans les projets de qualité des données. Les règles incluent également les règles de normalisation qui sont appliquées sur les données valides et sont utilisées dans la correction des données.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour créer une règle de domaine, vous devez avoir une base de connaissances et un domaine ouverts dans l'activité Gestion de l'arborescence du domaine.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer une règle de domaine.  
  
##  <a name="Build"></a> Créer les règles de domaine  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez ou créez une base de connaissances. Sélectionnez **Gestion de l'arborescence du domaine** comme activité, puis cliquez sur **Ouvrir** ou **Créer**. Pour plus d’informations, consultez [Créer une base de connaissances](../data-quality-services/create-a-knowledge-base.md) ou [Ouvrir une base de connaissances](../data-quality-services/open-a-knowledge-base.md).  
  
    > [!NOTE]  
    >  La gestion de domaine est exécutée dans une page du client Data Quality Service qui contient cinq onglets pour les opérations distinctes de gestion de domaine. Ce n'est pas un processus piloté par l'Assistant ; toute opération de gestion peut être exécutée séparément.  
  
3.  Dans **Liste des domaines** de la page **Gestion de l'arborescence du domaine** , sélectionnez le domaine pour lequel vous souhaitez créer une règle de domaine, ou créez un nouveau domaine. Si vous devez créer un domaine, consultez [Créer un domaine](../data-quality-services/create-a-domain.md).  
  
4.  Cliquez sur l'onglet **Règles de domaine** .  
  
5.  Cliquez **Ajouter une nouvelle règle de domaine**, puis entrez un nom unique dans la base de connaissances et une description de la règle.  
  
6.  Sélectionnez **Active** pour spécifier que la règle sera exécutée (valeur par défaut) ou désactivez l'option pour empêcher la règle de s'exécuter.  
  
7.  Dans le volet **Créer une règle** , sélectionnez une condition dans la liste déroulante de la zone de la clause de la règle.  
  
8.  Si la condition requiert une valeur, entrez la valeur dans la zone de texte associée.  
  
9. Cliquez sur **Ajoute une nouvelle condition à la clause sélectionnée** si une autre clause est requise.  
  
10. Sélectionnez **AND** ou **OR** en tant qu'opérateur.  
  
11. Sélectionnez une condition dans la liste déroulante, puis entrez une valeur pour l'opérande, si nécessaire.  
  
12. Pour modifier l'ordre des clauses qui apparaissent dans la liste, sélectionnez une clause, puis cliquez sur la flèche haut ou bas. Cela modifiera l'ordre dans lequel elles sont exécutées, ce qui peut affecter les résultats.  
  
13. Ajoutez des clauses si besoin. Si nécessaire, supprimez une clause en la sélectionnant, puis en cliquant sur **Supprime la clause sélectionnée**.  
  
14. Répétez ces étapes pour ajouter de nouvelles règles, selon les besoins.  
  
15. Pour voir l'impact qu'une règle de validation aurait sur les valeurs si implémentée, cliquez sur l'icône **Analyser l'impact de la règle de domaine sur les valeurs de domaine** .  
  
16. Poursuivez jusqu'à la procédure de test ci-dessous.  
  
##  <a name="Test"></a> Tester les règles de domaine  
  
1.  L'une des règles étant sélectionnée, cliquez sur l'icône d' **Exécuter la règle de domaine sélectionnée sur des données de test** .  
  
2.  Dans la boîte de dialogue Tester une règle de domaine, cliquez sur l'icône **Ajoute un nouveau terme de test pour la règle de domaine** . Entrez une valeur à tester. Entrez d'autres valeurs selon les besoins. Sélectionnez une valeur, puis cliquez sur l'icône de **Supprimer le terme de test sélectionné** si nécessaire.  
  
3.  Cliquez sur l'icône **Tester la règle de domaine sur tous les termes** .  
  
4.  Vérifiez la validité de chaque terme. Une coche signifie « correct », une croix « erreur » et un triangle « non valide ».  
  
5.  Une fois terminé, cliquez sur **Fermer** .  
  
6.  Répétez ces étapes pour les autres règles, selon les besoins.  
  
7.  Poursuivez jusqu'à la procédure d'application ci-dessous.  
  
##  <a name="Apply"></a> Appliquer les règles de domaine  
  
1.  Cliquez sur **Appliquer toutes les règles** pour appliquer les règles aux valeurs du domaine. Quand vous cliquez sur **Appliquer toutes les règles**, un message s'affiche et indique le nombre de valeurs de certains états qui seront affectées par la règle. Cliquez sur **Oui** si vous souhaitez toujours appliquer la règle ou sur **Non** dans le cas contraire. Si vous cliquez sur **Oui**, cliquez sur **OK** pour fermer la fenêtre des résultats.  
  
    > [!NOTE]  
    >  Lorsque vous créez ou modifiez une règle, vous n'avez pas besoin d'enregistrer les modifications. Toutefois, vous devez appliquer la règle pour que les modifications prennent effet.  
  
2.  Cliquez sur **Ignorer toutes les modifications** pour supprimer toutes les modifications que vous avez apportées aux règles de domaine, rétablissant ainsi les règles précédemment appliquées ; les modifications apportées après la dernière application des règles ne s'appliqueront plus. La validité de chaque valeur du domaine sera mise à jour pour être conforme aux règles précédemment appliquées, pas aux modifications ignorées.  
  
3.  Cliquez sur **Terminer** pour terminer l'activité de gestion de l'arborescence du domaine, comme décrit dans [End the Domain Management Activity](http://msdn.microsoft.com/library/ab6505ad-3090-453b-bb01-58435e7fa7c0).  
  
##  <a name="FollowUp"></a> Suivi : Après la création d'une règle de domaine  
 Après avoir créé une règle de domaine, vous pouvez effectuer d'autres tâches de gestion des domaines sur le domaine, effectuer une découverte des connaissances pour ajouter des connaissances au domaine ou ajouter une stratégie de correspondance au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="Conditions"></a> Conditions de règle de domaine  
 La table ci-dessous décrit les conditions qui peuvent être appliquées dans la règle de domaine et propose un exemple pour montrer comment les conditions peuvent être appliquées.  
  
 Lorsqu'une règle de domaine est appliquée et qu'une valeur de domaine ne respecte pas la règle, la valeur est indiquée comme Non valide. Une valeur indiquée comme Non valide sera modifiée en Correcte si la règle qui la rend non valide est supprimée ou désactivée, ou que la règle a été modifiée de telle sorte que la valeur ne respecte plus la règle. Si vous avez désigné manuellement une valeur comme non valide (sous l'onglet Valeurs de domaine de l'activité Gestion de l'arborescence du domaine) et qu'une règle que la valeur ne respecte pas a été supprimée, désactivée ou changée, la valeur toujours sera indiquée comme Non valide, selon la désignation manuelle.  
  
 Une règle de domaine qui a une condition définitive appliquera la logique des règles aux synonymes de la valeur de la ou des conditions, ainsi qu'aux valeurs elles-mêmes. Les conditions définitives sont La valeur est égale à, La valeur n'est pas égale à, La valeur se trouve dans, ou La valeur ne se trouve pas dans. Par exemple, imaginons que vous avez la règle de domaine suivante : « Pour « Ville », la valeur est égale « à Los Angeles » ». Si « Los Angeles » et « LA » sont synonymes, les deux sont correctes. En revanche, si votre règle ne contenait pas de condition définitive, telle que « Pour Ville, la valeur se termine par « s », « Los Angeles » serait une valeur correcte, mais son synonyme « LA » serait une erreur.  
  
 Vous avez différentes alternatives pour créer une règle de domaine. Par exemple, pour valider si les valeurs commencent par la lettre A, B, ou C, vous pouvez créer une règle simple avec une condition complexe (telle qu'une expression régulière avec des barres verticales), ou vous pouvez créer une règle complexe qui contient plusieurs conditions simples. Un exemple de la première règle est « La valeur contient l'expression régulière (^A|^B|^C) ». Un exemple de la deuxième règle est « La valeur commence par un A OR La valeur commence par un B OR La valeur commence par un C ».  
  
|Condition|Description| Exemple|  
|---------------|-----------------|-------------|  
|La longueur est égale à|Seules les valeurs composées du nombre de caractères définis par l'opérande seront valides.|Exemple d'opérande : 3<br /><br /> Valeur valide : BB1<br /><br /> Valeur non valide : AA|  
|La longueur est supérieure ou égale à|Seules les valeurs composées du nombre (ou d'un nombre supérieur) de caractères définis par l'opérande seront valides.|Exemple d'opérande : 3<br /><br /> Valeurs valides : BB1, BBAA<br /><br /> Valeur non valide : AA|  
|La longueur est inférieure ou égale à|Seules les valeurs composées du nombre (ou d'un nombre inférieur) de caractères définis par l'opérande seront valides.|Exemple d'opérande : 3<br /><br /> Valeurs valides : BB1, AA<br /><br /> Valeur non valide : BBAA|  
|La valeur est égale à|Seuls les valeurs identiques à l'opérande sont valides.|Exemple d'opérande : BB1<br /><br /> Valeur valide : BB1<br /><br /> Valeur non valide : BB, BB1#|  
|La valeur n'est pas égale à|Seules les valeurs non identiques à l'opérande sont valides.|Exemple d'opérande : BB1<br /><br /> Valeur valide : BB, BB1#<br /><br /> Valeur non valide : BB1|  
|La valeur contient|Seules les valeurs dont les caractères sont contenus dans l'opérande, dans n'importe quel ordre, sont valides.|Exemple d'opérande : A1<br /><br /> Valeurs valides : A1, AA1<br /><br /> Valeur non valide : 1A, AA|  
|La valeur ne contient pas|Seules les valeurs qui ne sont pas contenues dans l'opérande sont valides.|Exemple d'opérande : A1<br /><br /> Valeurs valides : 1A, AA<br /><br /> Valeurs non valides : A1, AA1|  
|La valeur commence par|Seules les valeurs qui commencent par les caractères de l'opérande sont valides.|Exemple d'opérande : AA<br /><br /> Valeurs valides : AA1<br /><br /> Valeurs non valides : 1AAB|  
|La valeur se termine par|Seules les valeurs qui se terminent par les caractères de l'opérande sont valides.|Exemple d'opérande : AA<br /><br /> Valeurs valides : 1AA<br /><br /> Valeurs non valides : 1AAB|  
|La valeur est numérique|Seules les valeurs ayant un type de données numérique SQL Server sont valides. Cela inclut int, decimal, float, etc.|Exemple d'opérande : N/A<br /><br /> Valeurs valides : 1, 25, 345,1234<br /><br /> Valeurs non valides : 2b, bcdef|  
|La valeur est date/heure|Seules les valeurs ayant un type de données date/heure SQL Server sont valides. Cela inclut datetime, time, date, etc.|Exemple d'opérande : N/A<br /><br /> Valeurs valides : 1916-06-04 ; 1916-06-04 18:24:24 ; 21 mars 2001, 5/18/2011 ; 18:24:24<br /><br /> Valeurs non valides : Mars 213, 2006|  
|La valeur se trouve dans|Seules les valeurs qui sont dans l'ensemble de l'opérande sont valides.<br /><br /> Pour écrire les valeurs de l'ensemble, cliquez dans la zone de texte de l'opérande, entrez la première valeur, appuyez sur ENTRÉE, entrez la seconde valeur, répétez ces étapes pour toutes les valeurs que vous souhaitez entrer, puis cliquez de nouveau dans la zone de texte de l'opérande. DQS ajoute une virgule entre les valeurs de l'ensemble. Si vous entrez une chaîne unique avec des virgules et aucun retour chariot (par exemple, « A1, B1 »), DQS considère cette chaîne comme une valeur unique dans l'ensemble.|Exemple d'opérande : [A1, B1]<br /><br /> Valeurs valides : A1, B1<br /><br /> Valeurs non valides : AA, 11|  
|La valeur ne se trouve pas dans|Seules les valeurs qui ne se trouvent pas dans l'ensemble de l'opérande sont valides.|Exemple d'opérande : [A1, B1]<br /><br /> Valeurs valides : AA, 11<br /><br /> Valeurs non valides : A1, B1|  
|La valeur correspond au modèle|Seules les valeurs qui correspondent au modèle de caractères, chiffres ou caractères spéciaux de l'opérande sont valides.<br /><br /> Toute lettre (A… Z) peut être utilisée comme modèle pour toute lettre ; respecte la casse. Tout chiffre (0… 9) peut être utilisé comme modèle pour tout chiffre. Tout caractère spécial, sauf une lettre ou un chiffre, peut être utilisé comme modèle pour lui-même. Les crochets, [], définissent la correspondance facultative.|Exemple d'opérande : AA:000 (un modèle de deux caractères *quelconques* suivis de deux-points (:), puis suivis par trois chiffres *quelconques* .<br /><br /> Valeurs valides : AB:012, df:257<br /><br /> Valeurs non valides : abc:123, FJ-369<br /><br /> Pour plus d'informations et des exemples sur les règles de modèle dans DQS, consultez [Correspondance de modèle dans les règles de domaine DQS](http://blogs.msdn.com/b/dqs/archive/2012/10/08/pattern-matching-in-dqs-domain-rules.aspx).|  
|La valeur ne correspond pas au modèle|Seules les valeurs qui ne correspondent pas au modèle de caractères, chiffres ou caractères spéciaux de l'opérande sont valides.|Exemple d'opérande : A1 (la valeur ne doit pas correspondre à un modèle d'un caractère *quelconque* , suivi par un chiffre *quelconque* ).<br /><br /> Valeurs valides : AB1, A, A:5<br /><br /> Valeurs non valides : B7, c9|  
|La valeur contient le modèle|Seules les valeurs qui contiennent le modèle de caractères, chiffres ou caractères spéciaux de l'opérande sont valides.|Exemple d'opérande : AA-12 (la valeur contient un modèle de deux caractères *quelconques* suivis d'un tiret (-), qui est encore suivi de deux chiffres *quelconques* ).<br /><br /> Valeurs valides : AAA-01, ab-975<br /><br /> Valeur non valide : A7, AA-6, C-45, AA ; 98|  
|La valeur ne contient pas le modèle|Seules les valeurs qui ne contiennent pas le modèle de caractères de l'opérande sont valides.|Exemple d'opérande : AB-12 (la valeur ne doit pas contenir un modèle de deux caractères *quelconques* suivis d'un tiret (-), qui est encore suivi de deux chiffres *quelconques* ).<br /><br /> Valeurs valides : A7, AA-6, C-45, aa;98<br /><br /> Valeur non valide : AAA-01, ab-975|  
|La valeur correspond à l'expression régulière|Seules les valeurs qui correspondent à l'expression régulière de l'opérande sont considérées comme valides.<br /><br /> N'incluez pas les caractères « ^ » ou « $ » à l'expression régulière, car DQS ajoute automatiquement ces caractères à une clause contenant une valeur correspondant à l'expression régulière. (Ou bien, vous pouvez placer l'expression régulière contenant les caractères « ^ » et « $ » entre parenthèses.) Pour plus d'informations sur les expressions régulières, consultez [Éléments du langage des expressions régulières](http://go.microsoft.com/fwlink/?LinkId=225561).|Exemple d'opérande : [1-5]+ (chaque caractère doit être un chiffre numérique de 1 à 5, présent une ou plusieurs fois)<br /><br /> Valeurs valides : 123, 12345, 14352<br /><br /> Valeurs non valides : 456, ABC|  
|La valeur ne correspond pas à une expression régulière|Seules les valeurs qui ne correspondent pas à l'expression régulière de l'opérande sont considérées comme valides.|Exemple d'opérande : [1-5]+ (la chaîne ne doit pas être composée seulement des chiffres numériques de 1 à 5)<br /><br /> Valeurs valides : 456, ABC<br /><br /> Valeur non valide : 123, 123456, 14352|  
  
  
