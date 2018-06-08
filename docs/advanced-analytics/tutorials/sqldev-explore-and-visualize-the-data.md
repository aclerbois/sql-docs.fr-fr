---
title: Leçon 3 Explorer et visualiser les données | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 4be76ebbb8f082e84a00bfe93b36c9bd8c2c0a81
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="lesson-3-explore-and-visualize-the-data"></a>Leçon 3 : Explorer et visualiser les données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel pour les développeurs SQL sur la façon d’utiliser R dans SQL Server.

Dans cette leçon, vous allez passez en revue les exemples de données et puis générer des graphiques à l’aide des fonctions R. Ces fonctions R sont déjà incluses dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Vous pouvez appeler les fonctions R [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="review-the-data"></a>Passez en revue les données

Le développement d’une solution de science des données comprend généralement l’exploration et la visualisation des données. Tout d’abord prendre une minute pour examiner les données d’exemple, si vous n’avez pas déjà.

Dans le dataset d’origine, les identificateurs de taxis et les enregistrements de trajets ont été fournis dans des fichiers distincts. Toutefois, pour faciliter les exemples de données à utiliser, les deux jeux de données d’origine ont été jointes sur les colonnes _medallion_, _hack\_licence_, et _collecte\_datetime_.  Les enregistrements ont aussi été échantillonnés pour obtenir seulement 1 % du nombre d’enregistrements d’origine. Le dataset échantillonné obtenu compte 1 703 957 lignes et 23 colonnes.

**Identificateurs de taxis**
  
-   La colonne _medallion_ représente le numéro d’ID unique du taxi.
  
-   Le _hack\_licence_ colonne contient le numéro de licence du pilote taxi (anonyme).
  
**Enregistrements de trajets et de prix**
  
-   Chaque enregistrement de trajet comprend les lieux de prise en charge et de dépose, ainsi que la durée et la distance du trajet.
  
-   Chaque enregistrement de prix inclut des informations telles que le type de paiement, le montant total du paiement et le montant du pourboire.
  
-   Les trois dernières colonnes peuvent être utilisées pour différentes tâches d’apprentissage automatique.  Le _Conseil\_quantité_ colonne contient des valeurs numériques continues et peut être utilisé comme le **étiquette** colonne pour l’analyse de régression. La colonne _tipped_ contient seulement des valeurs oui/non. Elle sert à la classification binaire. Le _Conseil\_classe_ colonne possède plusieurs **classe étiquettes** et peuvent donc être utilisés en tant que l’étiquette pour les tâches de classification multiclasse.
  
    Cette procédure pas à pas ne montre que la tâche de classification binaire. Si vous le souhaitez, vous pouvez essayer de créer des modèles pour les autres deux tâches d’apprentissage automatique, la régression et la classification multiclasse.
  
-   Les valeurs utilisées pour les colonnes d’étiquette sont toutes basées sur le _Conseil\_quantité_ colonne, à l’aide de ces règles d’entreprise :
  
    |Nom de la colonne dérivée|Règle|
    |-|-|
     |tipped|Si tip_amount > 0, tipped = 1, sinon tipped = 0|
    |tip_class|Classe 0 : tip_amount = 0 $<br /><br />Classe 1 : tip_amount > 0 $ et tip_amount <= 5 $<br /><br />Classe 2 : tip_amount > 5 $ et tip_amount <= 10 $<br /><br />Classe 3 : tip_amount > 10 $ et tip_amount <= 20 $<br /><br />Classe 4 : tip_amount > 20 $|

## <a name="create-plots-using-r-in-t-sql"></a>Créer des graphiques à l’aide de R dans T-SQL

La visualisation étant un outil tellement puissant pour comprendre la distribution des données et des valeurs hors norme, R fournit de nombreux packages de visualisation de données. La distribution open source standard de R inclut également de nombreuses fonctions pour créer des histogrammes, des nuages de points, des diagrammes à surface et d’autres graphiques d’exploration de données.

R crée généralement des images à l’aide d’un périphérique R pour la sortie graphique. Vous pouvez capturer la sortie de ce périphérique et stocker l’image dans un type de données **varbinary** pour le rendu dans une application, ou vous pouvez enregistrer les images dans l’un des formats de fichiers pris en charge (.JPG, .PDF, etc.).

Dans cette section, vous allez apprendre à travailler avec chaque type de sortie à l’aide de procédures stockées. Le processus global est la suivante :

- Créer une procédure stockée pour générer un traçage R en tant que données varbinary

- Générer le tracé et l’enregistrer dans un fichier image

- Utiliser une procédure stockée pour convertir les données binaires de traçage dans un fichier JPG ou PDF

### <a name="create-the-stored-procedure-plothistogram"></a>Créez la procédure stockée PlotHistogram

1. Pour créer le tracé, utilisez `rxHistogram`, l’une des fonctions R améliorées fournies dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], pour tracer un histogramme basé sur des données d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] requête. Pour faciliter l’appel de la fonction R, vous allez l’encapsuler dans une procédure stockée, _PlotHistogram_.

    Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez une nouvelle **requête** fenêtre.

2. Dans la base de données qui contient les données du didacticiel, créez la procédure à l’aide de cette instruction :

    ```SQL
    CREATE PROCEDURE [dbo].[PlotHistogram]
    AS
    BEGIN
      SET NOCOUNT ON;
      DECLARE @query nvarchar(max) =  
      N'SELECT tipped FROM nyctaxi_sample'  
      EXECUTE sp_execute_external_script @language = N'R',  
                                         @script = N'  
       image_file = tempfile();  
       jpeg(filename = image_file);  
       #Plot histogram  
       rxHistogram(~tipped, data=InputDataSet, col=''lightgreen'',   
       title = ''Tip Histogram'', xlab =''Tipped or not'', ylab =''Counts'');  
       dev.off();  
       OutputDataSet <- data.frame(data=readBin(file(image_file, "rb"), what=raw(), n=1e6));  
       ',  
       @input_data_1 = @query  
       WITH RESULT SETS ((plot varbinary(max)));  
    END
    GO
    ```

    N’oubliez pas de modifier le code pour utiliser le nom de table approprié, si nécessaire.
  
    -   La variable `@query` définit le texte de requête (`'SELECT tipped FROM nyctaxi_sample'`), qui est transmis au script R comme argument de la variable d’entrée du script, `@input_data_1`.
  
    -   Le script R est assez simple : une variable R (`image_file`) est définie pour stocker l’image, puis la fonction `rxHistogram` est appelée pour générer le tracé.
  
    -   Le périphérique R est défini sur **off**.
  
        En R, quand vous émettez une commande principale de traçage, R ouvre une fenêtre graphique appelée *device*(périphérique). Vous pouvez modifier la taille, les couleurs et d’autres aspects de la fenêtre, ou vous pouvez désactiver le périphérique si vous écrivez dans un fichier ou si vous gérez la sortie d’une autre manière.
  
    -   L’objet graphique R est sérialisé en data.frame R pour la sortie.

### <a name="generate-the-graphics-data-and-save-to-file"></a>Générer les données de graphique et l’enregistrer dans un fichier

La procédure stockée retourne l’image sous forme de flux de données varbinary qui, évidemment, ne peut pas être affiché directement. Toutefois, vous pouvez utiliser l’utilitaire **bcp** pour obtenir les données varbinary et les enregistrer en tant que fichier image sur un ordinateur client.
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], exécutez la commande suivante :
  
    ```SQL
    EXEC [dbo].[PlotHistogram]
    ```
  
    **Résultats**
    
    *traçage*
    *0xFFD8FFE000104A4649...*
  
2.  Ouvrez une invite de commandes PowerShell et exécutez la commande suivante, en fournissant le nom d’instance, le nom de base de données, le nom d’utilisateur et les informations d’identification appropriés en tant qu’arguments :
  
     ```
     bcp "exec PlotHistogram" queryout "plot.jpg" -S <SQL Server instance name> -d  <database name>  -U <user name> -P <password>
     ```

    > [!NOTE]
    > Commutateurs de commande pour bcp respectent la casse.
  
3.  Si la connexion réussit, vous serez invité à entrer davantage d’informations sur le format du fichier graphique. Appuyez sur Entrée à chaque invite pour accepter les valeurs par défaut, à l’exception de ces modifications :
  
    -   Pour **prefix-length of field plot**, tapez 0.
  
    -   Type **Y** si vous souhaitez enregistrer les paramètres de sortie pour une réutilisation ultérieure.
  
    ```
    Enter the file storage type of field plot [varbinary(max)]:
    Enter prefix-length of field plot [8]: 0
    Enter length of field plot [0]:
    Enter field terminator [none]:
  
    Do you want to save this format information in a file? [Y/n]
    Host filename [bcp.fmt]:
    ```
  
    **Résultats**
    
    ```
    Starting copy...
    1 rows copied.
    Network packet size (bytes): 4096
    Clock Time (ms.) Total     : 3922   Average : (0.25 rows per sec.)
    ```

    > [!TIP]
    > Si vous enregistrez les informations de format dans un fichier (bcp.fmt), l’utilitaire **bcp** génère une définition de format que vous pouvez appliquer ultérieurement à des commandes similaires sans avoir à entrer d’options de format de fichier graphique. Pour utiliser le fichier de format, ajoutez `-f bcp.fmt` à la fin d’une ligne de commande, après l’argument de mot de passe.
  
4.  Le fichier de sortie est créé dans le même répertoire que celui où vous avez exécuté la commande PowerShell. Pour afficher le tracé, il suffit d’ouvrir le fichier plot.jpg.
  
    ![courses de taxi avec et sans pourboires](media/rsql-devtut-tippedornot.jpg "courses de taxi avec et sans pourboires")  
  
### <a name="export-the-plot-data-to-a-viewable-file"></a>Exporter les données de traçage vers un fichier visible

Sortie d’un tracé de R sur les données binaires type peut s’avérer pratique pour la consommation par les applications, mais il n’est pas très utile pour un chercheur de données qui a besoin le tracé rendu pendant la phase d’exploration de données. En général, le spécialiste des données génère plusieurs visualisations de données, pour obtenir des informations sur les données de différentes perspectives.

Pour générer des graphiques pour les utilisateurs, vous pouvez utiliser une procédure stockée qui crée la sortie de R dans des formats courants tels que. JPG. PDF, et. PNG. Une fois que la procédure stockée a créé le graphique, il vous suffit d’ouvrir le fichier pour visualiser le tracé.

1. Créez une procédure stockée, _PlotInOutputFiles_, qui montre comment écrire des histogrammes et des graphiques R à scatterplots. JPG et. Format PDF.

    Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez une nouvelle **requête** fenêtre et la coller dans le code suivant [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.
  
    ```SQL
    CREATE PROCEDURE [dbo].[PlotInOutputFiles]  
    AS  
    BEGIN  
      SET NOCOUNT ON;  
      DECLARE @query nvarchar(max) =  
      N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'  
      EXECUTE sp_execute_external_script @language = N'R',  
      @script = N'  
       # Set output directory for files and check for existing files with same names   
        mainDir <- ''C:\\temp\\plots''  
        dir.create(mainDir, recursive = TRUE, showWarnings = FALSE)  
        setwd(mainDir);  
        print("Creating output plot files:", quote=FALSE)
        
        # Open a jpeg file and output histogram of tipped variable in that file.  
        dest_filename = tempfile(pattern = ''rHistogram_Tipped_'', tmpdir = mainDir)  
        dest_filename = paste(dest_filename, ''.jpg'',sep="")  
        print(dest_filename, quote=FALSE);  
        jpeg(filename=dest_filename);  
        hist(InputDataSet$tipped, col = ''lightgreen'', xlab=''Tipped'',   
            ylab = ''Counts'', main = ''Histogram, Tipped'');  
         dev.off();  

        # Open a pdf file and output histograms of tip amount and fare amount.   
        # Outputs two plots in one row  
        dest_filename = tempfile(pattern = ''rHistograms_Tip_and_Fare_Amount_'', tmpdir = mainDir)  
        dest_filename = paste(dest_filename, ''.pdf'',sep="")  
        print(dest_filename, quote=FALSE);  
        pdf(file=dest_filename, height=4, width=7);  
        par(mfrow=c(1,2));  
        hist(InputDataSet$tip_amount, col = ''lightgreen'',   
            xlab=''Tip amount ($)'',   
            ylab = ''Counts'',   
            main = ''Histogram, Tip amount'', xlim = c(0,40), 100);  
        hist(InputDataSet$fare_amount, col = ''lightgreen'',   
            xlab=''Fare amount ($)'',   
            ylab = ''Counts'',   
            main = ''Histogram,   
            Fare amount'',   
            xlim = c(0,100), 100);  
       dev.off();  
  
        # Open a pdf file and output an xyplot of tip amount vs. fare amount using lattice;  
        # Only 10,000 sampled observations are plotted here, otherwise file is large.  
        dest_filename = tempfile(pattern = ''rXYPlots_Tip_vs_Fare_Amount_'', tmpdir = mainDir)  
        dest_filename = paste(dest_filename, ''.pdf'',sep="")  
        print(dest_filename, quote=FALSE);  
        pdf(file=dest_filename, height=4, width=4);  
        plot(tip_amount ~ fare_amount,   
            data = InputDataSet[sample(nrow(InputDataSet), 10000), ],   
            ylim = c(0,50),   
            xlim = c(0,150),   
            cex=.5,   
            pch=19,   
            col=''darkgreen'',    
            main = ''Tip amount by Fare amount'',   
            xlab=''Fare Amount ($)'',   
            ylab = ''Tip Amount ($)'');   
        dev.off();',  
     @input_data_1 = @query  
     END
    ```
  
    -   Le résultat de la requête SELECT dans la procédure stockée est stocké dans la trame de données R par défaut, `InputDataSet`. Vous pouvez ensuite appeler différentes fonctions de traçage R pour générer les fichiers graphiques proprement dits.
  
        La plupart du script R incorporé représente des options pour ces fonctions graphiques, telles que `plot` ou `hist`.
  
    -   Tous les fichiers sont enregistrés dans le dossier local _C:\temp\Plots\\_. Le dossier de destination est défini par les arguments fournis au script R dans le cadre de la procédure stockée.  Vous pouvez modifier le dossier de destination en modifiant la valeur de la variable `mainDir`.
  
2.  Exécutez l’instruction pour créer la procédure stockée.

    ```SQL
    EXEC PlotInOutputFiles
    ```

    **Résultats**
    
    ```
    STDOUT message(s) from external script:
    [1] Creating output plot files:[1] C:\temp\plots\rHistogram_Tipped_18887f6265d4.jpg[1] 
    
    C:\temp\plots\rHistograms_Tip_and_Fare_Amount_1888441e542c.pdf[1]
    
    C:\temp\plots\rXYPlots_Tip_vs_Fare_Amount_18887c9d517b.pdf
    ```

    Les nombres dans les noms de fichiers sont générés de façon aléatoire pour vous assurer que vous n’obtenez une erreur lorsque vous tentez d’écrire dans un fichier existant.

3. Pour afficher le tracé, ouvrez le dossier de destination et examinez les fichiers qui ont été créés par le code R dans la procédure stockée.

    + Le fichier `rHistogram_Tipped.jpg` indique le nombre d’allers-retours qui a obtenu une info-bulle et les boucles qui a reçu aucune info-bulle. (Cet histogramme est comme celui que vous avez créé à l’étape précédente.)

    + Le fichier `rHistograms_Tip_and_Fare_Amount.pdf` montre la répartition des montants de Conseil, tracée sur les montants de prix.
    
    ![Histogramme affichant tip_amount et fare_amount](media/rsql-devtut-tipamtfareamt.PNG "Histogramme affichant tip_amount et fare_amount")

    + Le fichier `rXYPlots_Tip_vs_Fare_Amount.pdf` contient un scatterplot avec le montant de frais sur l’axe x et la quantité de Conseil sur l’axe des ordonnées.

    ![quantité d’info-bulle tracée sur le montant de frais](media/rsql-devtut-tipamtbyfareamt.PNG "quantité d’info-bulle tracée sur le montant de frais")

2.  Pour exporter les fichiers vers un autre dossier, modifiez la valeur de la variable `mainDir` dans le script R incorporé dans la procédure stockée. Vous pouvez également modifier le script pour générer des formats différents, plusieurs fichiers, et ainsi de suite.

## <a name="next-lesson"></a>Leçon suivante

[Leçon 4 : Créer des fonctionnalités de données à l’aide de T-SQL](../tutorials/sqldev-create-data-features-using-t-sql.md)

## <a name="previous-lesson"></a>Leçon précédente

[Leçon 2 : Importer des données vers SQL Server à l’aide de PowerShell](../r/sqldev-import-data-to-sql-server-using-powershell.md)
