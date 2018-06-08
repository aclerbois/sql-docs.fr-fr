---
title: Autres manières d’obtenir une connexion de données (Générateur de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: aebc5f3d-97d5-4d54-b525-753fed073a9a
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 8d84cb434338e8174fe74f7466be768bc344e2fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alternative-ways-to-get-a-data-connection-report-builder"></a>Autres manières d'obtenir une connexion de données (Générateur de rapports)
Une connexion de données contient les informations nécessaires pour se connecter à une source de données externe telle qu'une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . En règle générale, vous obtenez les informations de connexion et le type d'informations d'identification à utiliser auprès du propriétaire de la source de données.  
  
Pour spécifier une connexion de données, vous pouvez utiliser une source de données partagée sur le serveur de rapports ou créer une source de données incorporée utilisée uniquement dans un rapport spécifique.  
  
Dans la plupart des didacticiels, vous utilisez des sources de données incorporées, mais si vous avez accès à des sources de données partagées, vous pouvez les utiliser à la place.  
  
## <a name="getting-a-data-connection-from-a-shared-data-source"></a>Obtention d'une connexion de données à partir d'une source de données partagée  
Si le serveur de rapports a des sources de données partagées que vous avez l’autorisation d’utiliser, vous pouvez les utiliser à la place d’une source de données incorporée. Les procédures suivantes montrent comment localiser les sources de données partagées et fournir les informations d'identification nécessaires à leur utilisation.  
  
Pour utiliser une source de données partagée, accédez à un serveur de rapports et sélectionnez-la. En règle générale, vous obtenez l'URL du serveur de rapports auprès de l'administrateur du serveur de rapports.  
  
### <a name="to-specify-a-data-connection-from-a-list-of-shared-data-sources"></a>Pour spécifier une connexion de données à partir d'une liste de sources de données partagées  
  
1.  Dans l’Assistant Nouveau tableau ou matrice, ou Nouveau graphique, dans la page **Choisir un dataset** , sélectionnez **Créer un dataset**, puis cliquez sur **Suivant**. La page **Choisir une connexion à une source de données** s’ouvre.  
  
2.  Dans la liste des sources de données, sélectionnez une source de données pour laquelle vous disposez d'autorisations d'accès.  
  
3.  Pour vous assurer que vous pouvez vous connecter à la source de données, cliquez sur **Tester la connexion**. Le message « La connexion a été correctement créée » s'affiche. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
4.  Cliquez sur **Suivant**.  
  
    Si nécessaire, entrez vos informations d'identification. Pour enregistrer les informations d’identification localement, sélectionnez **Enregistrer le mot de passe avec la connexion**. Si vous ne sélectionnez pas cette option, vous êtes invité à indiquer vos informations d’identification chaque fois que vous exécutez le rapport  
  
5.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-specify-a-data-connection-by-browsing-to-a-shared-data-source-on-a-report-server"></a>Pour spécifier une connexion de données en accédant à une source de données partagée sur un serveur de rapports  
  
1.  Dans l’Assistant Nouveau tableau ou matrice, ou Nouveau graphique, dans la page **Choisir un dataset** , sélectionnez **Créer un dataset**, puis cliquez sur **Suivant**. La page **Choisir une connexion à une source de données** s’ouvre.  
  
2.  Cliquez sur **Parcourir**. La boîte de dialogue **Sélectionner une source de données** s’ouvre.  
  
3.  Dans la liste déroulante **Regarder dans** , sélectionnez **Sites et serveurs récents**. Dans le volet de source de données, cliquez sur l’URL de votre serveur, puis sur **Ouvrir**.  
  
    La liste des sources de données ou modèles s'affiche.  
  
4.  Vous pouvez aussi taper l’URL du serveur de rapports dans la zone **Nom**. Cliquez sur **Ouvrir**.  
  
    Le Générateur de rapports se connecte au serveur de rapports, puis charge les sources de données disponibles au niveau du dossier racine.  
  
5.  Naviguez jusqu’à un dossier qui contient une source de données pour laquelle vous disposez d’autorisations suffisantes pour vous y connecter, sélectionnez la source de données, puis cliquez sur **Ouvrir**.  
  
    Vous revenez à la page **Choisir une connexion à une source de données** .  
  
6.  Pour vous assurer que vous pouvez vous connecter à la source de données, cliquez sur **Tester la connexion**.  
  
    Le message « La connexion a été correctement créée » s'affiche. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  Cliquez sur **Suivant**.  
  
8.  Si vous êtes invité à entrer un nom d'utilisateur et un mot de passe, entrez vos informations d'identification. Pour enregistrer les informations d’identification localement, sélectionnez **Enregistrer le mot de passe avec la connexion**.  
  
9. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
[Datasets de rapport &#40;SSRS&#41;](../reporting-services/report-data/report-datasets-ssrs.md)  
[Didacticiels du Générateur de rapports](../reporting-services/report-builder-tutorials.md) 
  

