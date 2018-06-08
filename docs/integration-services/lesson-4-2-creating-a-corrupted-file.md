---
title: 'Étape 2 : Création d’un fichier endommagé | Microsoft Docs'
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
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: cd0b18dc-66c3-4d88-86ef-8e40cb660fae
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4f2088f0c99586609b5a95c5cd5a30bbf6a75bcf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-4-2---creating-a-corrupted-file"></a>Leçon 4-2 : Création d’un fichier endommagé
Afin de démontrer l'utilisation des fonctions de configuration et de gestion des erreurs de transformation, vous allez devoir créer un exemple de fichier plat qui, lors de son traitement, entraîne l'échec d'un composant.  
  
Au cours de cette tâche, vous allez créer une copie d'un fichier plat existant. Vous ouvrirez ensuite ce fichier dans le Bloc-notes et modifierez la colonne **CurrencyID** pour vous assurer qu'aucune correspondance ne peut être établie au cours de la recherche de transformations. Lors du traitement du nouveau fichier, l'échec de la recherche provoquera à son tour l'échec de la transformation Lookup Currency Key et, par conséquent, celui du reste du package. Une fois le fichier exemple corrompu créé, vous exécuterez le package pour examiner son échec.  
  
### <a name="to-create-a-corrupted-sample-flat-file"></a>Pour créer un fichier plat exemple corrompu  
  
1.  Dans le Bloc-notes ou un autre éditeur de texte, ouvrez le fichier Currency_VEB.txt.  
  
    Les exemples de données sont inclus dans les packages de leçons SSIS. Pour télécharger ces exemples de données et les packages de leçons, procédez comme suit.  
  
    1.  Accédez à [Exemples de produits Integration Services](http://go.microsoft.com/fwlink/?LinkID=267527).  
  
    2.  Cliquez sur l'onglet **DOWNLOADS** (Téléchargements).  
  
    3.  Cliquez sur le fichier SQL2012.Integration_Services.Create_Simple_ETL_Tutorial.Sample.zip.  
  
2.  Utilisez la fonction Rechercher/Remplacer de l'éditeur de texte pour retrouver toutes les instances de **VEB** et les remplacer par **BAD**.  
  
3.  Dans le dossier qui contient les autres exemples de fichiers de données, enregistrez le fichier modifié en tant que **Currency_BAD.txt**.  
  
    > [!IMPORTANT]  
    > Vérifiez que **Currency_BAD.txt** est enregistré dans le même dossier que les autres exemples de fichiers de données.  
  
4.  Fermez l'éditeur de texte.  
  
### <a name="to-verify-that-an-error-will-occur-during-run-time"></a>Pour vérifier si une erreur se produit au moment de l'exécution  
  
1.  Dans le menu **Déboguer** , cliquez sur **Démarrer le débogage**.  
  
    À la troisième itération du flux de données, la transformation Lookup Currency Key tente de traiter le fichier Currency_BAD.txt et la transformation échoue. L'échec de la transformation entraîne l'échec de tout le package.  
  
2.  Dans le menu **Déboguer** , cliquez sur **Arrêter le débogage**.  
  
3.  Dans la zone de conception, cliquez sur l'onglet **Résultats d'exécution** .  
  
4.  Parcourez le journal et vérifiez si les erreurs non gérées suivantes se sont produites :  
  
    `[Lookup Currency Key[27]] Error: Row yielded no match during lookup.`  
  
    > [!NOTE]  
    > Le nombre 27 désigne l'ID du composant. Cette valeur est attribuée lors de la création du flux de données ; la valeur définie dans votre package peut être différente.  
  
## <a name="next-steps"></a>Next Steps  
[Étape 3 : Ajout de redirection de flux d'erreurs](../integration-services/lesson-4-3-adding-error-flow-redirection.md)  
  
  
  
