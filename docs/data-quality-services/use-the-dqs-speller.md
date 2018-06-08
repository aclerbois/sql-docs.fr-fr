---
title: Utiliser le vérificateur d’orthographe DQS | Microsoft Docs
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
ms.assetid: 65e4e53e-2699-4cae-a9e0-fe78547755b5
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 22c4ca9f1c7c0909734c8433d82f06aeb14e4990
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-the-dqs-speller"></a>Utiliser le vérificateur d'orthographe DQS

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Le vérificateur d'orthographe [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) vérifie la syntaxe, l'orthographe, et la structure de la phrase des valeurs de chaîne dans un domaine. Le vérificateur d'orthographe est une fonctionnalité autonome et côté client qui n'a pas d'intégration avec les moteurs côté serveur et aucune conséquence sur les flux ou les états actuels. Le vérificateur d'orthographe identifie ces valeurs de chaîne qu'il considère comme des erreurs potentielles, puis les marque d'un trait de soulignement rouge dans le même emplacement que celui où vous apportez d'autres modifications manuelles aux valeurs de domaine. Ces emplacements incluent :  
  
-   La page **Gérer les valeurs du domaine** de l'activité **Découverte des connaissances**  
  
-   La page **Valeurs du domaine** ou la page **Relations à base de termes** de l'activité **Gestion de l'arborescence du domaine**  
  
-   La page **Gérer et afficher les résultats** de l'activité **Nettoyage**  
  
 Le vérificateur d'orthographe fonctionne uniquement sur les domaines simples dont le type de données est String. Toutes les valeurs d'un domaine simple qui sont du type de données String sont envoyées au correcteur orthographique pour validation. Le vérificateur d'orthographe ne fonctionne pas pour un domaine composite et ne fonctionne pas pour les domaines de types autres que les chaînes, les valeurs mixtes (telles que lettres et chiffres sans espace), les chiffres romains, les caractères uniques et les valeurs qui se composent seulement de majuscules.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour exécuter le vérificateur d'orthographe, vous devez disposer d'une base de connaissances et d'un domaine ouverts dans l'activité de découverte des connaissances ou de gestion de l'arborescence du domaine ; le vérificateur d'orthographe doit être activé pour le domaine et dans la page où vous allez l'exécuter ; et la propriété de langue doit être spécifiée pour le domaine.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour exécuter le vérificateur d'orthographe.  
  
##  <a name="Enable"></a> Activer le vérificateur d'orthographe  
  
1.  Pour activer le vérificateur d'orthographe dans [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)], ouvrez la base de connaissances dans l'activité **Gestion de l'arborescence du domaine** , sélectionnez le domaine souhaité, puis cliquez sur **Activer le vérificateur d'orthographe** dans la page **Propriétés du domaine** . Dans **Langue**, sélectionnez la langue à utiliser avec le vérificateur d'orthographe.  
  
2.  Lorsque le vérificateur d'orthographe est activé dans les propriétés de domaine, il est activé dans la page **Gérer les valeurs du domaine** , la page **Valeurs du domaine** ou la page **Relations à base de termes** et la page **Gérer et afficher les résultats** . Pour désactiver le vérificateur d'orthographe sur ces pages, cliquez sur l'icône **Activer/désactiver le vérificateur d'orthographe** . Un clic sur l'icône modifie l'état du vérificateur d'orthographe sur la page. De même, si la propriété **Activer le vérificateur d'orthographe** pour le domaine est désactivée, un clic sur l'icône **Activer/désactiver le vérificateur d'orthographe** active le vérificateur d'orthographe sur la page. Si vous quittez la page puis y revenez, l'état du bouton est de nouveau déterminé par la propriété de domaine **Activer le vérificateur d'orthographe** .  
  
##  <a name="Use"></a> Utilisez le vérificateur d'orthographe  
  
1.  Se déplacer vers l'une des pages suivantes :  
  
    -   La page **Gérer les valeurs du domaine** de l'activité **Découverte des connaissances**  
  
    -   La page **Valeurs du domaine** ou la page **Relations à base de termes** de l'activité **Gestion de l'arborescence du domaine**  
  
    -   La page **Gérer et afficher les résultats** de l'activité **Nettoyage**  
  
2.  Affichez les valeurs appropriées de la table **Valeur** par le filtrage ou la recherche.  
  
3.  Analysez les lignes de la table **Valeur** pour déterminer si une valeur des colonnes **Valeur** ou **Corriger vers** est identifiée par un trait de soulignement ondulé rouge.  
  
4.  Cliquez avec le bouton droit sur une valeur marquée par un trait de soulignement rouge. Cliquez sur l'une des valeurs de remplacement répertoriées si elle est préférable à la valeur d'origine.  
  
5.  Si aucune valeur affichée n'est préférable et qu'il existe un bouton **Plus de suggestions** indiquant les valeurs supplémentaires, cliquez dessus. Si l'une des valeurs supplémentaires est préférable à l'originale, cliquez dessus.  
  
6.  Si vous souhaitez ajouter la valeur au dictionnaire, cliquez sur **Ajouter au dictionnaire**. Le trait de soulignement rouge disparaîtra de la valeur.  
  
##  <a name="FollowUp"></a> Suivi : Après utilisation du vérificateur d'orthographe  
 Après avoir exécuté le vérificateur d'orthographe, complétez l'activité dans laquelle le domaine est pour utiliser les corrections suggérées par le vérificateur d'orthographe. Si vous êtes dans l'activité de découverte des connaissances, de gestion des domaines ou de stratégie de correspondance, publiez la base de connaissances afin que les résultats de l'analyse du vérificateur d'orthographe soient utilisables dans la base de connaissances. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="How"></a> Fonctionnement du vérificateur d'orthographe  
 Le vérificateur d'orthographe DQS marque toute erreur potentielle de valeur de type chaîne avec un trait de soulignement rouge affiché pour la valeur entière. Par exemple, si « New York » est mal orthographié en « Neu York », le vérificateur d'orthographe affiche un trait de soulignement rouge sous « Neu York », et pas seulement sous « Neu ». Si vous cliquez avec le bouton droit sur la valeur, les suggestions de corrections s'affichent pour la valeur complète. Vous pouvez également cliquer sur **Plus de suggestions** s'il existe plus de cinq suggestions. Vous pouvez choisir l'une des suggestions ou ajouter une valeur au dictionnaire (à un niveau compte d'utilisateur) à afficher pour la valeur d'origine. Les valeurs ajoutées au dictionnaire s'appliquent à tous les domaines. La correction sera effectuée dans le domaine uniquement si vous indiquez explicitement une suggestion. Lorsque vous sélectionnez une suggestion dans le menu contextuel Vérificateur d'orthographe, le type de valeur devient (ou demeure) une erreur. La suggestion sélectionnée sera ajoutée à la colonne de correction. Notez qu'une valeur peut avoir le **Type** **Correct** mais être marquée comme erreur potentielle par le vérificateur d'orthographe.  
  
 DQS fournit des suggestions pour les valeurs de la colonne **Valeur** et de la colonne **Corriger vers** de la table **Valeur** . Lorsque vous sélectionnez une suggestion dans la colonne **Valeur** , le type de la valeur est défini sur **Erreur**et la suggestion est copiée dans la colonne **Corriger vers** , comme si vous l'aviez insérée manuellement. S'il y avait une correction existante, elle devient une suggestion. Dans la page **Gérer et afficher les résultats** de l'activité **Nettoyage** , lorsque vous sélectionnez une suggestion dans la colonne **Corriger vers** , DQS remplace la valeur actuellement sélectionnée par la sélection, et la valeur actuellement sélectionnée devient une suggestion. Dans la page **Gérer et afficher les résultats** de l'activité **Nettoyage** , aucune suggestion n'est effectuée au niveau enregistrement (grille inférieure).  
  
  
