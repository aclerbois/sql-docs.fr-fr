---
title: 'Étape 4 : Ajout d’une destination de fichier plat | Microsoft Docs'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: f4088de3-16d8-419c-96a1-a2cd005d0a5b
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d50777afbba77d87e6988338835f3cec023ee4d5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-4-4---adding-a-flat-file-destination"></a>Leçon 4-4 : Ajout d’une destination de fichier plat
La sortie d'erreur de la transformation Lookup Currency Key réachemine vers la transformation Script toutes les lignes de données ayant échoué au cours de l'opération de recherche. Pour améliorer les informations recueillies sur les erreurs survenues, la transformation Script exécute un script chargé d'obtenir la description des erreurs.  
  
Au cours de cette tâche, vous allez enregistrer toutes ces informations sur les lignes échouées dans un fichier délimité pour les traiter ultérieurement. Pour enregistrer les lignes ayant échoué, vous devez ajouter et configurer un gestionnaire de connexions de fichiers plats pour le fichier texte censé contenir les données sur les erreurs, ainsi qu'une destination de fichier plat. En définissant les propriétés du gestionnaire de connexions de fichier plat utilisé par la destination de fichier plat, vous pouvez spécifier la façon dont elle met en forme et écrit le fichier texte. Pour plus d'informations, consultez [Flat File Connection Manager](../integration-services/connection-manager/flat-file-connection-manager.md) et [Flat File Destination](../integration-services/data-flow/flat-file-destination.md).  
  
### <a name="to-add-and-configure-a-flat-file-destination"></a>Pour ajouter et configurer une destination de fichier plat  
  
1.  Cliquez sur l'onglet **Flux de données** .  
  
2.  Dans la **Boîte à outils SSIS**, développez **Autre**, puis faites glisser **Destination du fichier plat** sur l'aire de conception Flux de données. Placez la **Destination de fichier plat** directement sous la transformation **Get Error Description** .  
  
3.  Cliquez sur la transformation **Obtenir la description de l’erreur** , puis faites glisser la flèche verte vers la nouvelle **Destination de fichier plat**.  
  
4.  Sur l'aire de conception **Flux de données** , cliquez sur **Destination de fichier plat** dans la transformation de **Destination de fichier plat** nouvellement ajoutée, puis remplacez le nom par **Failed Rows**.  
  
5.  Cliquez avec le bouton droit sur la transformation **Failed Rows** , cliquez sur **Modifier**, puis dans **l’Éditeur de destination de fichier plat**, cliquez sur **Nouveau**.  
  
6.  Dans la boîte de dialogue **Format de fichier plat** , assurez-vous que **Délimité** est sélectionné, puis cliquez sur **OK**.  
  
7.  Dans l' **Éditeur du gestionnaire de connexions de fichiers plats**, dans la zone **Nom du gestionnaire de connexions** , tapez **Données d'erreur**.  
  
8.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , cliquez sur **Parcourir**, puis recherchez le dossier dans lequel le fichier est à stocker.  
  
9. Dans la boîte de dialogue **Ouvrir** , dans la zone **Nom de fichier**, tapez **SortieErreur.txt**, puis cliquez sur **Ouvrir**.  
  
10. Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , vérifiez que la zone **Paramètres régionaux** contient la valeur Anglais (États-Unis) et la zone **Page de codes** la valeur 1252 (ANSI - Latin I).  
  
11. Dans le volet Options, cliquez sur **Colonnes**.  
  
    Notez qu'en plus des colonnes du fichier de données source, trois nouvelles colonnes sont présentes : ErrorCode, ErrorColumn et ErrorDescription. Ces colonnes sont générées par la sortie d'erreur de la transformation Lookup Currency Key et par le script de la transformation Get Error Description. Vous pouvez les utiliser pour résoudre le problème à l'origine de l'échec de la ligne.  
  
12. Cliquez sur **OK**.  
  
13. Dans l' **Éditeur de destination de fichier plat**, désactivez la case à cocher **Remplacer les données du fichier** .  
  
    La désactivation de cette case à cocher conserve les erreurs sur plusieurs exécutions de packages.  
  
14. Dans l' **Éditeur de destination de fichier plat**, cliquez sur **Mappages** pour vérifier que toutes les colonnes sont correctes. Vous pouvez également renommer les colonnes dans la destination.  
  
15. Cliquez sur **OK**.  
  
## <a name="next-steps"></a>Next Steps  
[Étape 5 : Test de la leçon 4 du Package du didacticiel](../integration-services/lesson-4-5-testing-the-lesson-4-tutorial-package.md)  
  
  
  
