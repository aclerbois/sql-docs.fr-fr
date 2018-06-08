---
title: Importer les domaines d’un fichier Excel dans la découverte des connaissances | Microsoft Docs
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
ms.assetid: 4d3a3940-6c2a-4dc4-90eb-86f26012c165
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b7baab0db19dc5ec9a9b7833c0a03c6f62b3d2ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-domains-from-an-excel-file-in-knowledge-discovery"></a>Importer les domaines d'un fichier Excel dans la découverte des connaissances

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment importer un ou plusieurs domaines d'un fichier Excel dans l'activité de découverte de connaissance [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). L'importation simplifie le processus de génération de la connaissance, en permettant de gagner du temps et d'économiser les efforts. Elle permet aux personnes qui ont des données dans un fichier Excel ou un fichier texte de créer une base de connaissances avec ces données. (Consultez [Importer des valeurs d’un fichier Excel dans un domaine](../data-quality-services/import-values-from-an-excel-file-into-a-domain.md) pour plus d’informations sur l’importation de valeurs dans un domaine d’une base de connaissances existante.) L'exportation vers un fichier Excel n'est pas prise en charge.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour importer les domaines d'un fichier Excel, Excel doit être installé sur l'ordinateur où l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] est installée ; vous devez avoir créé un fichier Excel avec des valeurs de domaine (consultez [How the import works](#How)) et avoir créé et ouvert une base de connaissances dans laquelle importer le domaine.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour importer les domaines d'un fichier Excel.  
  
##  <a name="Import"></a> Importer les domaines d'un fichier Excel vers une base de connaissances  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , effectuez l'une des opérations suivantes :  
  
    -   Créez une nouvelle base de connaissances à importer en cliquant sur **Nouvelle Base de connaissances**, entrez un nom pour la base de connaissances, sélectionnez **Aucun** pour **Créer la Base de connaissances à partir de**, sélectionnez l'activité **Découverte des connaissances** , puis cliquez sur **Créer**.  
  
    -   Ouvrez une base de connaissances existante à importer en cliquant sur **Ouvrir la Base de connaissances**, sélectionnez la base de connaissances, sélectionnez **Découverte des connaissances**, puis cliquez **Suivant**.  
  
3.  Dans la page **Mapper** , sélectionnez **Fichier Excel** pour **Source de données**.  
  
4.  Cliquez sur **Parcourir** sur la ligne **Fichier Excel** .  
  
5.  Dans la boîte de dialogue **Sélectionner un fichier Excel** , accédez au dossier contenant le fichier Excel à partir duquel vous voulez importer, sélectionnez le fichier Excel, puis cliquez sur **Ouvrir**.  
  
6.  Dans la liste déroulante **Feuille de calcul** , sélectionnez la feuille de calcul dans le fichier Excel à partir duquel vous voulez importer.  
  
7.  Sélectionnez **Utiliser la première ligne comme en-tête** si vous souhaitez que la première ligne soit considérée comme en-tête de données, et si vous souhaitez que les valeurs de la première ligne soient utilisées comme noms de colonne. Désélectionnez **Utiliser la première ligne comme en-tête** si vous souhaitez que la première ligne soit considérée comme valeur de données, auquel cas DQS utilise les noms d'en-tête Excel (lettres alphabétiques) pour la colonne.  
  
8.  Sélectionnez une colonne, puis mappez un domaine existant à la colonne, ou créez un nouveau domaine en cliquant sur l'icône **Créer un domaine** , en créant un domaine dans la boîte de dialogue **Créer un domaine** , puis en mappant le domaine à la colonne. Le type de données du domaine doit correspondre au type de données de la colonne. Répétez ces étapes pour toutes les colonnes de la feuille de calcul.  
  
9. Cliquez sur **Suivant**.  
  
10. Dans la page **Découverte** , cliquez sur **Démarrer** pour analyser les données de la feuille de calcul Excel.  
  
    > [!NOTE]  
    >  Si vous quittez la page avant que les données n'aient été téléchargées, le processus de téléchargement des fichiers sera terminé.  
  
11. Vérifiez que l'analyse s'est terminée avec succès, puis cliquez sur **Suivant**.  
  
12. Dans la page **Gérer les valeurs du domaine** , vérifiez que les domaines corrects sont répertoriés dans la liste **Domaines** et que les valeurs sont entrées dans la table des domaines.  
  
13. Cliquez sur **Terminer**, puis cliquez sur **Publier** pour publier la base de connaissances, ou sur **Non** pour ne pas publier.  
  
14. Vérifiez que la base de connaissances a été publiée, puis cliquez **OK**.  
  
##  <a name="FollowUp"></a> Suivi : Après importation des domaines d'un fichier Excel  
 Après avoir importé les domaines d'un fichier Excel, vous pouvez ajouter les connaissances aux domaines ou utiliser les domaines dans un projet de nettoyage ou de correspondance, en fonction du contenu des domaines. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md), [Gestion d’un domaine composite](../data-quality-services/managing-a-composite-domain.md), [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md), [Nettoyage des données](../data-quality-services/data-cleansing.md) ou [Correspondance de données](../data-quality-services/data-matching.md).  
  
##  <a name="How"></a> How the import works  
 Dans l'opération d'importation, DQS interprète un fichier Excel comme suit :  
  
-   Une colonne représente un domaine.  
  
-   Une ligne représente un enregistrement de données.  
  
-   La première ligne représente les noms de domaine ou la première valeur ou premier enregistrement de données, selon la valeur de la case à cocher **Utiliser la première ligne comme en-tête** .  
  
 Les règles suivantes s'appliquent à l'opération d'importation :  
  
-   Cette opération importe les valeurs de domaine dans une base de connaissances. Elle n'importe pas les règles de domaine ou une stratégie de correspondance.  
  
-   Le fichier Excel peut avoir une extension .xlsx, .xls ou .csv. Microsoft Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] pour importer les valeurs de domaine ou un domaine complet. Les versions Excel 2003 et versions ultérieures sont prises en charge. Si la version 64 bits d'Excel est utilisée, seuls les fichiers Excel 2003 sont pris en charge ; les fichiers Excel 2007 ou 2010 ne sont pas pris en charge.  
  
-   Les fichiers Excel du type .xlsx ne sont pas pris en charge pour une installation Excel 64 bits. Si vous utilisez Excel 64 bits, enregistrez le fichier comme fichier .xls.  
  
-   Dans les fichiers .xlsx et .xls, le type de données de la colonne est déterminé par le type de données prédominant des huit premières lignes. Si une cellule n'est pas conforme à ce type de données, elle recevra une valeur NULL.  
  
-   Dans les fichiers .csv, le type de données est déterminé par le type de données prédominant des huit premières lignes.  
  
-   Une valeur d'une feuille de calcul Excel qui n'est pas conforme à une règle de domaine est importée comme valeur non valide.  
  
-   Si le fichier Excel n'est pas dans le bon format ou est endommagé, l'opération d'importation génère une erreur.  
  
  
