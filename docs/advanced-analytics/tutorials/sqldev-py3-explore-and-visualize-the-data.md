---
title: Étape 3 Explorer et visualiser les données | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 2575b1c0d5de8d546d1b1ef1775f3c9e7145adc1
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="step-3-explore-and-visualize-the-data"></a>Étape 3 : Explorer et visualiser les données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel, [analytique Python de la base de données pour les développeurs SQL](sqldev-in-database-python-for-sql-developers.md). 

Dans cette étape, vous explorez les exemples de données et générez des graphiques. Une version ultérieure, vous apprenez à sérialiser des objets graphics dans Python, puis désérialiser ces objets et rendre des graphiques.

## <a name="review-the-data"></a>Passez en revue les données

Tout d’abord, prenez une minute pour parcourir le schéma de données, comme nous avons apporté des modifications pour le rendre plus facile d’utiliser les données NYC Taxi

+ Le jeu de données d’origine utilisé des fichiers distincts pour les identificateurs de taxi et les enregistrements de voyage. Nous avons joint deux jeux de données d’origine sur les colonnes _medallion_, _hack_license_, et _pickup_datetime_.  
+ Le jeu de données d’origine sur lesquelles s’étend de fichiers et a été très volumineux. Nous avons sous-échantillonnée pour obtenir uniquement 1 % du nombre d’enregistrements d’origine. La table de données en cours a 1,703,957 lignes et 23 colonnes.

**Identificateurs de taxis**

Le _medallion_ colonne représente le numéro d’identification unique du taxi.

Le _hack_license_ colonne contient le numéro de licence du pilote taxi (anonyme).

**Enregistrements de trajets et de prix**

Chaque enregistrement de trajet comprend les lieux de prise en charge et de dépose, ainsi que la durée et la distance du trajet.

Chaque enregistrement de prix inclut des informations telles que le type de paiement, le montant total du paiement et le montant du pourboire.

Les trois dernières colonnes peuvent être utilisées pour différentes tâches d’apprentissage automatique.  La colonne _tip_amount_ contient des valeurs numériques continues et peut être utilisée comme colonne **étiquette** pour l’analyse de régression. La colonne _tipped_ contient seulement des valeurs oui/non. Elle sert à la classification binaire. La colonne _tip_class_ a plusieurs **étiquettes de classes** , et peut donc être utilisée comme étiquette pour les tâches de classification multiclasse.

Les valeurs utilisées pour les colonnes d’étiquette sont toutes basées sur la `tip_amount` colonne, à l’aide de ces règles d’entreprise :

+ Colonne d’étiquette `tipped` a des valeurs possibles, 0 et 1

    Si `tip_amount` > 0, `tipped` = 1 ; sinon `tipped` = 0

+ Colonne d’étiquette `tip_class` a des valeurs possibles de classe 0-4

    Classe 0 : `tip_amount` = 0 $

    Classe 1 : `tip_amount` > $0 et `tip_amount` < = 5 $
    
    Classe 2 : `tip_amount` > $5 et `tip_amount` < = $10
    
    Classe 3 : `tip_amount` > $10 et `tip_amount` < = 20
    
    Classe 4 : `tip_amount` 20 >

## <a name="create-plots-using-python-in-t-sql"></a>Créer des graphiques à l’aide de Python dans T-SQL

Le développement d’une solution de science des données comprend généralement l’exploration et la visualisation des données. Étant donné que la visualisation est un outil puissant permettant de comprendre la distribution des données et des valeurs hors norme, Python fournit de nombreux packages de visualisation de données. Le **matplotlib** module est une des bibliothèques de visualisation les plus populaires et inclut de nombreuses fonctions pour la création d’histogrammes, nuages de points, des graphiques de zone et autres graphiques d’exploration de données.

Dans cette section, vous allez apprendre à travailler avec des graphiques à l’aide de procédures stockées. Au lieu d’ouvrir l’image sur le serveur, vous stockez l’objet Python `plot` en tant que **varbinary** données, puis écrire que dans un fichier qui peut être partagé ou affichées ailleurs.

### <a name="create-a-plot-as-varbinary-data"></a>Créer un graphique en tant que données varbinary

Le **revoscalepy** module inclus avec SQL Server 2017 Machine Learning Services prend en charge des fonctionnalités similaires à celles de la **RevoScaleR** package de R.  Cet exemple utilise l’équivalent de Python de `rxHistogram` pour tracer un histogramme basé sur des données d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] requête. 

La procédure stockée retourne une Python sérialisée `figure` objet sous forme de flux de **varbinary** données. Vous ne pouvez pas afficher directement les données binaires, mais vous pouvez utiliser du code Python à désérialiser et afficher les chiffres sur le client et puis enregistrez le fichier image sur un ordinateur client.

1. Créez la procédure stockée _SerializePlots_, si le script PowerShell s’est pas déjà fait.

    - La variable `@query` définit le texte de requête `SELECT tipped FROM nyctaxi_sample`, qui est passé au bloc de code Python comme argument à la variable d’entrée du script, `@input_data_1`.
    - Le script Python est relativement simple : **matplotlib** `figure` objets sont utilisés pour effectuer le traçage de l’histogramme et à nuages de points, et ces objets sont sérialisés puis à l’aide de la `pickle` bibliothèque.
    - L’objet graphics Python est sérialisée vers un **pandas** trame de données pour la sortie.
  
    ```SQL
    CREATE PROCEDURE [dbo].[SerializePlots]
    AS
    BEGIN
      SET NOCOUNT ON;
      DECLARE @query nvarchar(max) =
      N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'
      EXECUTE sp_execute_external_script
      @language = N'Python',
      @script = N'
    import matplotlib
    matplotlib.use("Agg")
    import matplotlib.pyplot as plt
    import pandas as pd
    import pickle

    fig_handle = plt.figure()
    plt.hist(InputDataSet.tipped)
    plt.xlabel("Tipped")
    plt.ylabel("Counts")
    plt.title("Histogram, Tipped")
    plot0 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.tip_amount)
    plt.xlabel("Tip amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Tip amount")
    plot1 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.fare_amount)
    plt.xlabel("Fare amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Fare amount")
    plot2 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.scatter( InputDataSet.fare_amount, InputDataSet.tip_amount)
    plt.xlabel("Fare Amount ($)")
    plt.ylabel("Tip Amount ($)")
    plt.title("Tip amount by Fare amount")
    plot3 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    OutputDataSet = plot0.append(plot1, ignore_index=True).append(plot2, ignore_index=True).append(plot3, ignore_index=True)
    ',
    @input_data_1 = @query
    WITH RESULT SETS ((plot varbinary(max)))
    END
    GO
    ```

2. Maintenant, exécutez la procédure stockée sans arguments pour générer un graphique à partir des données codées en dur en tant que requête d’entrée.

    ```
    EXEC [dbo].[SerializePlots]
    ```

3. Les résultats doivent être quelque chose comme ceci :
  
    ```
    plot
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    ```

  
4. À partir d’un client Python, vous pouvez maintenant vous connecter à l’instance de SQL Server qui a généré les objets binaires de traçage et afficher les graphiques. 

    Pour ce faire, exécutez le code Python suivant, en remplaçant le nom du serveur, nom de la base de données et les informations d’identification comme il convient. Assurez-vous que la version Python est le même sur le client et le serveur. Assurez-vous également que vous utilisez la version identique ou ultérieure par rapport aux bibliothèques sont installées sur le serveur dans les bibliothèques Python sur votre client (par exemple, matplotlib).
  
    **À l’aide de l’authentification SQL Server :**
    
    ```python
    import pyodbc
    import pickle
    import os
    cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER={SERVER_NAME};DATABASE={DB_NAME};UID={USER_NAME};PWD={PASSWORD}')
    cursor = cnxn.cursor()
    cursor.execute("EXECUTE [dbo].[SerializePlots]")
    tables = cursor.fetchall()
    for i in range(0, len(tables)):
        fig = pickle.loads(tables[i][0])
        fig.savefig(str(i)+'.png')
    print("The plots are saved in directory: ",os.getcwd())
    ```

    **À l’aide de l’authentification Windows :**

    ```python
    import pyodbc
    import pickle
    import os
    cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER={SERVER_NAME};DATABASE={DB_NAME};Trusted_Connection=yes;')
    cursor = cnxn.cursor()
    cursor.execute("EXECUTE [dbo].[SerializePlots]")
    tables = cursor.fetchall()
    for i in range(0, len(tables)):
        fig = pickle.loads(tables[i][0])
        fig.savefig(str(i)+'.png')
    print("The plots are saved in directory: ",os.getcwd())
    ```

5.  Si la connexion est établie, vous devez voir un message comme suit :
  
    *Les graphiques sont enregistrés dans le répertoire : xxxx*
  
6.  Le fichier de sortie est créé dans le répertoire de travail Python. Pour afficher le tracé, recherchez le répertoire de travail de Python et ouvrez le fichier. L’illustration suivante montre un graphique enregistré sur l’ordinateur client.
  
    ![Conseil le montant de frais vs](media/sqldev-python-sample-plot.png "Conseil, montant de frais de vs") 

## <a name="next-step"></a>Étape suivante

[Étape 4 : Créer des caractéristiques de données à l’aide de T-SQL](sqldev-py5-train-and-save-a-model-using-t-sql.md)

## <a name="previous-step"></a>Étape précédente

[Étape 2 : Importer des données dans SQL Server à l’aide de PowerShell](sqldev-py2-import-data-to-sql-server-using-powershell.md)

