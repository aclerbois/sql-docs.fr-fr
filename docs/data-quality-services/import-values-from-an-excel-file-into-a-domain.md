---
title: Importer les valeurs d’un fichier Excel dans un domaine | Microsoft Docs
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
- sql13.dqs.kb.importfailing.f1
- sql13.dqs.kb.importselect.f1
- sql13.dqs.kb.failingvalues.f1
ms.assetid: 04cde693-2043-477f-8417-fcc463ca7195
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a6af86f6aad6272dafff5f9974f933174d3797cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-values-from-an-excel-file-into-a-domain"></a>Importer les valeurs d'un fichier Excel dans un domaine

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment importer des valeurs à partir d'un fichier Excel vers un champ de [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). L'utilisation d'un fichier Excel pour importer les valeurs de champ dans l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] simplifie le processus de génération de connaissance, et permet d'économiser aussi bien le temps que les efforts. Elle permet aux personnes qui ont une liste de valeurs de données valides dans un fichier Excel ou un fichier texte d'importer ces valeurs dans un domaine. À partir d'un fichier Excel, vous pouvez importer les valeurs de domaine dans un domaine ou des domaines d'une base de connaissances. (Consultez [Importer les domaines d’un fichier Excel dans la découverte des connaissances](../data-quality-services/import-domains-from-an-excel-file-in-knowledge-discovery.md) pour plus d’informations sur l’importation de domaines dans une base de connaissances.) L'exportation vers un fichier Excel n'est pas prise en charge.  
  
 Vous pouvez importer les valeurs de données de deux façons :  
  
-   Créez un nouveau domaine, puis importez les valeurs à partir d'un fichier Excel, auquel cas toutes les valeurs sont ajoutées au domaine.  
  
-   Importez les valeurs dans un domaine existant et renseigné, auquel cas seules les nouvelles valeurs sont importées. Toutes les valeurs qui existent déjà ne seront pas importées.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour importer les champs d'un fichier Excel, Excel doit être installé sur l'ordinateur où l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] est installée pour pouvoir importer les valeurs de domaine ou un domaine complet ; vous devez avoir créé un fichier Excel avec des valeurs de domaine (consultez [How the import works](#How)) et devez avoir créé et ouvert une base de connaissances dans laquelle importer le domaine.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour importer les valeurs de champs d'un fichier Excel.  
  
##  <a name="Import"></a> Importer les valeurs d'un fichier Excel dans un domaine  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Sur l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez une base de connaissances dans l'activité Gestion de l'arborescence du domaine.  
  
3.  Si vous ajoutez des valeurs à un nouveau domaine, créez un nouveau domaine à l'aide de l'icône **Créer un domaine** , puis sélectionnez le nouveau domaine dans la liste des domaines.  
  
4.  Si vous ajoutez des valeurs à un domaine existant, sélectionnez le domaine dans la liste des domaines.  
  
5.  Cliquez sur l'onglet **Valeurs du domaine** , cliquez sur l'icône d' **Importer des valeurs** dans la barre d'icônes, puis cliquez **Importer des valeurs valides d'Excel**.  
  
6.  Dans la boîte de dialogue **Importer les valeurs du domaine** , cliquez sur **Parcourir**.  
  
7.  Dans la boîte de dialogue **Sélectionner un fichier** , accédez au dossier contenant le fichier Excel dont vous souhaitez importer les valeurs de domaine, sélectionnez le fichier (extension .xlsx, .xls ou .csv), puis cliquez **Ouvrir**. Le fichier doit être sur le client à partir duquel vous exécutez DQS ou sur un partage de fichiers auquel l'utilisateur a accès.  
  
8.  Dans la liste déroulante **Feuille de calcul** , sélectionnez la feuille à partir de laquelle vous importez.  
  
9. Sélectionnez **Utiliser la première ligne comme en-tête** si la première ligne de la feuille de calcul représente le nom du domaine, et toutes les autres lignes représentent des valeurs valides de domaine.  
  
10. Cliquez sur **OK**. Une barre de progression s'affiche, avec indication sur le nombre de valeurs importées avec succès, le nombre de valeurs non importées et le nombre total de valeurs. Cliquez sur le bouton **Annuler** pour annuler le processus.  
  
11. Vérifiez que « Importation terminée » s'affiche dans la boîte de dialogue **Importer les valeurs du domaine** . Consultez les valeurs importées avec succès et celles qui ne l'ont pas été. Sont indiqués le nom du fichier et le chemin d'accès du fichier, l'état d'achèvement de l'opération, le nombre de valeurs importées avec succès, le nombre de valeurs non importées et le nombre total de valeurs traitées.  
  
12. Pour les valeurs qui n'ont pas été importées avec succès, cliquez sur **Journal** pour afficher la boîte de dialogue **Importer les valeurs du domaine – Valeurs erronées** pour voir pourquoi l'opération d'importation a échoué. La colonne **Valeur erronée** montre les valeurs qui n'ont pas pu être importées à partir d'un fichier Excel dans un domaine et la colonne **Raison** explique pourquoi l'importation a échoué. Cliquez sur **Copier dans le Presse-papiers** pour copier la table **Valeur erronée** dans le presse-papiers, à partir duquel vous pouvez le copier dans un autre programme, tel qu'une feuille de calcul Excel ou un fichier du Bloc-notes. Cliquez sur **OK** pour fermer la boîte de dialogue **Valeurs erronées** .  
  
13. Cliquez sur **OK** pour terminer l'opération d'importation et fermer la boîte de dialogue. Lorsque l'importation s'est terminé avec succès, la liste des valeurs du domaine de la page **Valeurs du domaine** est actualisée et inclut les nouvelles valeurs importées. Le filtre est modifié en **Toutes les valeurs** et **Afficher seulement les nouvelles valeurs** est sélectionné. Lorsque **Afficher seulement les nouvelles valeurs** est sélectionné après l'opération d'importation, seules les valeurs importées à partir du fichier Excel s'affichent.  
  
14. Cliquez **Terminer** pour ajouter les valeurs à la base de connaissances.  
  
##  <a name="FollowUp"></a> Suivi : Après l'importation des valeurs d'un fichier Excel dans un domaine  
 Après avoir importé des valeurs dans un domaine, vous pouvez effectuer d'autres tâches de gestion de domaine sur le domaine, vous pouvez exécuter la découverte de connaissances pour ajouter des connaissances au domaine ou vous pouvez ajouter une stratégie correspondante au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="Synonyms"></a> Importation des synonymes  
 Les synonymes sont importés comme suit :  
  
-   D'abord, toutes les valeurs sont importées, puis la connexion du synonyme est établie.  
  
-   S'il est impossible de connecter des valeurs de synonyme, une erreur s'affiche sur l'écran du journal. Il est possible que les principales valeurs et les synonymes du fichier soient importés vers le domaine, mais ne soient pas définis comme synonymes.  
  
 Les points suivants s'appliquent au processus de définition des connexions de synonyme :  
  
-   Si la valeur de départ du fichier Excel existe déjà dans le domaine comme synonyme d'une autre valeur, vous devrez définir les synonymes manuellement (par exemple, dans le fichier Excel nous souhaitons que la valeur A soit la valeur de départ de la valeur B, mais dans le domaine la valeur A apparaît comme synonyme de la valeur C). Outre la définition manuelle des synonymes une fois l'importation terminée, vous pouvez également dissocier les valeurs qui sont actuellement synonymes (par exemple, dissocier les valeurs A et C ci-dessus), puis importer le fichier.  
  
-   Si le synonyme est déjà connecté à une autre valeur de départ, vous devez définir les synonymes manuellement.  
  
-   Si les valeurs ne peuvent pas être connectées manuellement dans l'application pour une raison quelconque, elles ne pourront pas s'appliquer à l'opération d'importation.  
  
##  <a name="How"></a> How the import works  
 Les valeurs suivantes sont importées par cette opération :  
  
 Dans l'opération d'importation, DQS importe à partir d'un fichier Excel comme suit :  
  
-   Les valeurs correctes et les nouvelles valeurs sont importées. S'il existe une ou plusieurs valeurs importées de domaine, les valeurs ne sont pas importées.  
  
-   Une valeur qui contredit une règle de domaine est importée comme valeur non valide.  
  
-   Une valeur ne sera pas importée à partir du fichier si la valeur n'est pas du type de données du domaine ou est NULL.  
  
-   Les valeurs sont importées dans l'ordre où elles apparaissent dans le fichier.  
  
-   Chaque ligne représente une valeur de domaine.  
  
-   La première ligne représente les noms de domaine ou la première valeur ou premier enregistrement de données, selon la valeur de la case à cocher **Utiliser la première ligne comme en-tête** . Si vous sélectionnez **Utiliser la première ligne comme en-tête** lors de l'utilisation d'un fichier .xslx ou .xls, tous les noms de colonnes qui sont null seront automatiquement convertis en F*n*et un numéro sera ajouté à toutes les colonnes en doublon.  
  
-   Si vous annulez l'opération d'importation avant qu'elle soit terminée, l'opération est annulée et aucune donnée n'est importée.  
  
-   Les valeurs de la première colonne sont importées dans le domaine. Si outre la première colonne, une ou plusieurs colonnes supplémentaires sont remplies, les valeurs de ces colonnes seront ajoutées en tant que synonymes (consultez [Importation des synonymes](#Synonyms)).  
  
    -   Le format attendu est que la première colonne correspond aux valeurs de départ et la deuxième colonne et les colonnes suivantes aux synonymes.  
  
    -   Vous pouvez importer plusieurs synonymes dans la même ligne ou dans des lignes différentes. Par exemple, si vous souhaitez importer « NYC » et « New York » City en tant que synonymes de « New York », vous pouvez importer une seule ligne avec « New York » dans la colonne 1, « NYC » dans la colonne 2, et « New York City » dans la colonne 3 ; ou vous pouvez importer une ligne avec « New York » dans la colonne 1 et « NYC » dans la colonne 2, et une autre ligne avec « New York » dans la colonne 1 et « New York City » dans la colonne 2. Notez que si la valeur « New York » existe déjà dans le domaine, seuls les synonymes seront ajoutés et l'utilisateur ne recevra pas d'erreur pendant l'importation lui indiquant que la valeur existe déjà. Si la première valeur n'existe pas déjà, elle est ajoutée au domaine.  
  
 Les règles suivantes s'appliquent au fichier Excel utilisé pour l'importation :  
  
-   Le fichier Excel peut avoir une extension .xlsx, .xls ou .csv. Microsoft Excel doit être installé sur l'ordinateur où l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] est installée en fonction pour pouvoir importer les valeurs de domaine ou un domaine complet. Les versions Excel 2003 et versions ultérieures sont prises en charge. Si la version 64 bits d'Excel est utilisée, seuls les fichiers Excel 2003 sont pris en charge ; les fichiers Excel 2007 ou 2010 ne sont pas pris en charge.  
  
-   Les fichiers Excel du type .xlsx ne sont pas pris en charge pour une installation Excel 64 bits. Si vous utilisez Excel 64 bits, enregistrez le fichier comme fichier .xls ou fichier .csv, ou installez une installation Excel 32 bits à la place.  
  
-   Dans les fichiers .xlsx et .xls, le type de données de la colonne est déterminé par les huit premières lignes. Si le type de données de colonne des huit premières lignes est mixte, la colonne sera de type « string ». Si une cellule de la ligne 9 ou ligne supérieure n'est pas conforme à ce type de données, elle recevra une valeur NULL.  
  
-   Dans les fichiers .csv, le type de données est déterminé par le type de données prédominant des huit premières lignes.  
  
-   Si le fichier Excel n'est pas dans le bon format ou est endommagé, l'opération d'importation génère une erreur.  
  
  
