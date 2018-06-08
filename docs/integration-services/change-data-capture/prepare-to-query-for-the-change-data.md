---
title: Préparer la recherche des données modifiées | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],preparing query
ms.assetid: 9ea2db7a-3dca-4bbf-9903-cccd2d494b5f
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 57d4e31b133ed4b7f55ee2a89089b956aedc9930
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepare-to-query-for-the-change-data"></a>Préparer la recherche des données modifiées
  Dans le flux de contrôle d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui effectue un chargement incrémentiel des données modifiées, la troisième et dernière tâche consiste à préparer la recherche des données modifiées et à ajouter une tâche de flux de données.  
  
> [!NOTE]  
>  La deuxième tâche pour le flux de contrôle est de garantir que les données modifiées pour l'intervalle sélectionné sont prêtes. Pour plus d’informations, consultez [Déterminer si les données modifiées sont prêtes](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md). Pour obtenir une description du processus d’ensemble de la conception du flux de contrôle, consultez [Capture de données modifiées &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="design-considerations"></a>Remarques sur la conception  
 Pour récupérer les données modifiées, vous appellerez une fonction table Transact-SQL qui accepte les points de terminaison de l'intervalle comme paramètres d'entrée et qui retourne les données modifiées pour l'intervalle spécifié. Un composant source dans le flux de données appelle cette fonction. Pour plus d’informations sur ce composant source, consultez [Récupérer et comprendre les données modifiées](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
 Les composants sources [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] les plus fréquemment utilisés, notamment la source OLE DB, la source ADO et la source ADO NET, ne peuvent pas dériver d’informations de paramètre pour une fonction table. Ainsi, la plupart des sources ne peuvent pas appeler directement une fonction paramétrable.  
  
 Deux options de conception s'offrent à vous pour passer les paramètres d'entrée à la fonction :  
  
-   **Assembler la requête paramétrable en tant que chaîne**. Vous pouvez utiliser une tâche de script ou une tâche d'exécution SQL pour assembler une chaîne SQL dynamique avec les valeurs de paramètre codées en dur dans la chaîne. Vous pouvez ensuite stocker cette chaîne dans une variable de package et l'utiliser pour définir la propriété SqlCommand d'un composant source. Cette approche aboutit car le composant source n'a plus besoin des informations de paramètre.  
  
    > [!NOTE]  
    >  Un script précompilé nécessite un temps de traitement inférieur à une tâche d'exécution SQL.  
  
-   **Utiliser un wrapper paramétrable**. Vous pouvez également créer une procédure stockée paramétrable en tant que wrapper qui appelle la fonction table paramétrable. Cette approche part du principe qu'un composant source peut correctement dériver des informations de paramètre pour une procédure stockée.  
  
 Cette rubrique utilise la première option de conception et assemble une requête paramétrable en tant que chaîne.  
  
## <a name="preparing-the-query"></a>Préparation de la requête  
 Avant de pouvoir concaténer les valeurs des paramètres d'entrée dans une chaîne de requête unique, vous devez installer les variables de package dont la requête a besoin.  
  
#### <a name="to-set-up-package-variables"></a>Pour configurer des variables de package  
  
-   Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], dans la fenêtre **Variables** , créez une variable du type de données string pour contenir la chaîne de requête retournée par la tâche d’exécution SQL.  
  
     Cet exemple utilise le nom de variable SqlDataQuery.  
  
 Une fois la variable de package créée, vous pouvez utiliser une tâche de script ou une tâche d'exécution SQL pour concaténer les valeurs des paramètres d'entrée. Les deux procédures suivantes décrivent comment configurer ces composants.  
  
#### <a name="to-use-a-script-task-to-concatenate-the-query-string"></a>Pour utiliser une tâche de script pour concaténer la chaîne de requête  
  
1.  Sous l’onglet **Flux de contrôle** , ajoutez une tâche de script au package après le conteneur de boucles For et connectez ce dernier à la tâche.  
  
    > [!NOTE]  
    >  Cette procédure suppose que le package effectue un chargement incrémentiel à partir d'une table unique. Si le package effectue le chargement à partir de plusieurs tables et qu'il possède un package parent avec plusieurs packages enfants, cette tâche est ajoutée en tant que premier composant à chaque package enfant. Pour plus d’informations, consultez [Exécuter un chargement incrémentiel de plusieurs tables](../../integration-services/change-data-capture/perform-an-incremental-load-of-multiple-tables.md).  
  
2.  Dans **l’Éditeur de tâche de script**, dans la page **Script** , sélectionnez les options suivantes :  
  
    1.  Pour **ReadOnlyVariables**, sélectionnez **User::DataReady**, **User::ExtractStartTime**et **User::ExtractEndTime** dans la liste.  
  
    2.  Pour **ReadWriteVariables**, sélectionnez User::SqlDataQuery dans la liste.  
  
3.  Dans **l’Éditeur de tâche de script**, dans la page **Script** , cliquez sur **Modifier le script** pour ouvrir l’environnement de développement de script.  
  
4.  Dans la procédure Main, entrez l'un des segments de code suivants :  
  
    -   Si vous programmez en C#, entrez les lignes de code suivantes :  
  
        ```csharp 
        int dataReady;  
        System.DateTime extractStartTime;  
        System.DateTime extractEndTime;  
        string sqlDataQuery;  
  
        dataReady = (int)Dts.Variables["DataReady"].Value;  
        extractStartTime = (System.DateTime)Dts.Variables["ExtractStartTime"].Value;  
        extractEndTime = (System.DateTime)Dts.Variables["ExtractEndTime"].Value;  
  
        if (dataReady == 2)  
          {  
          sqlDataQuery = "SELECT * FROM CDCSample.uf_Customer('" + string.Format("{0:yyyy-MM-dd hh:mm:ss}", extractStartTime) + "', '" + string.Format("{0:yyyy-MM-dd hh:mm:ss}", extractEndTime) + "')";  
          }  
        else  
          {  
          sqlDataQuery = "SELECT * FROM CDCSample.uf_Customer(null" + ", '" + string.Format("{0:yyyy-MM-dd hh:mm:ss}", extractEndTime) + "')";  
          }  
  
        Dts.Variables["SqlDataQuery"].Value = sqlDataQuery;  
        ```  
  
         \- ou -  
  
    -   Si vous programmez en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], entrez les lignes de code suivantes :  
  
        ```vb  
        Dim dataReady As Integer  
        Dim extractStartTime As Date  
        Dim extractEndTime As Date  
        Dim sqlDataQuery As String  
  
        dataReady = CType(Dts.Variables("DataReady").Value, Integer)  
        extractStartTime = CType(Dts.Variables("ExtractStartTime").Value, Date)  
        extractEndTime = CType(Dts.Variables("ExtractEndTime").Value, Date)  
  
        If dataReady = 2 Then  
          sqlDataQuery = "SELECT * FROM CDCSample.uf_Customer('" & _  
              String.Format("{0:yyyy-MM-dd hh:mm:ss}", extractStartTime) & _  
              "', '" & _  
              String.Format("{0:yyyy-MM-dd hh:mm:ss}", extractEndTime) & _  
              "')"  
        Else  
          sqlDataQuery = "SELECT * FROM CDCSample.uf_Customer(null" & _  
              ", '" & _  
              String.Format("{0:yyyy-MM-dd hh:mm:ss}", extractEndTime) & _  
              "')"  
        End If  
  
        Dts.Variables("SqlDataQuery").Value = sqlDataQuery  
  
        ```  
  
5.  Laissez la ligne de code par défaut qui retourne **DtsExecResult.Success** suite à l’exécution du script.  
  
6.  Fermez l’environnement de développement de script et **l’Éditeur de tâche de script**.  
  
#### <a name="to-use-an-execute-sql-task-to-concatenate-the-query-string"></a>Pour utiliser une tâche d'exécution SQL pour concaténer la chaîne de requête  
  
1.  Sous l’onglet **Flux de contrôle** , ajoutez une tâche d’exécution SQL au package après le conteneur de boucles For et connectez ce dernier à cette tâche.  
  
    > [!NOTE]  
    >  Cette procédure suppose que le package effectue un chargement incrémentiel à partir d'une table unique. Si le package effectue le chargement à partir de plusieurs tables et qu'il possède un package parent avec plusieurs packages enfants, cette tâche est ajoutée en tant que premier composant à chaque package enfant. Pour plus d’informations, consultez [Exécuter un chargement incrémentiel de plusieurs tables](../../integration-services/change-data-capture/perform-an-incremental-load-of-multiple-tables.md).  
  
2.  Dans **l’Éditeur de tâche d’exécution de requêtes SQL**, dans la page **Général** , sélectionnez les options suivantes :  
  
    1.  Pour **ResultSet**, sélectionnez **Ligne unique**.  
  
    2.  Configurez une connexion valide à la base de données source.  
  
    3.  Pour **SQLSourceType**, sélectionnez **Entrée directe**.  
  
    4.  Pour **SQLStatement**, entrez l’instruction SQL suivante :  
  
        ```sql
        declare @ExtractStartTime datetime,  
        @ExtractEndTime datetime,   
        @DataReady int  
  
        select @DataReady = ?,   
        @ExtractStartTime = ?,   
        @ExtractEndTime = ?  
  
        if @DataReady = 2  
        select N'select * from CDCSample.uf_Customer'  
        + N'('''+ convert(nvarchar(30),@ExtractStartTime,120)  
        + ''', '''  
        + convert(nvarchar(30),@ExtractEndTime,120) + ''') '   
        as SqlDataQuery  
        else  
        select N'select * from CDCSample.uf_Customer'  
        + N'(null, '''  
        + convert(nvarchar(30),@ExtractEndTime,120)  
        + ''') '  
        as SqlDataQuery  
  
        ```  
  
        > [!NOTE]  
        >  La clause **else** dans cet exemple génère une requête pour le chargement initial des données modifiées en passant une valeur Null pour la date et l’heure de début. Cet exemple ne s'applique pas au scénario selon lequel des modifications qui ont été apportées avant l'activation de la capture de données modifiées doivent aussi être téléchargées dans l'entrepôt de données.  
  
3.  Dans la page **Mappage de paramètre** de **l’Éditeur de tâche d’exécution de requêtes SQL**, effectuez le mappage suivant :  
  
    1.  Mappez la variable DataReady au paramètre 0.  
  
    2.  Mappez la variable ExtractStartTime au paramètre 1.  
  
    3.  Mappez la variable ExtractEndTime au paramètre 2.  
  
4.  Dans la page **Ensemble de résultats** de **l’Éditeur de tâche d’exécution de requêtes SQL**, mappez le Nom de résultat à la variable SqlDataQuery.  
  
     Le Nom de résultat est le nom de la colonne unique retournée, SqlDataQuery.  
  
 Les procédures précédentes configurent une tâche qui prépare une chaîne de requête avec des valeurs de chaîne codées en dur pour les paramètres d'entrée. Le code suivant est un exemple d'une telle chaîne de requête :  
  
 `select * from CDCSample. uf_Customer('2007-06-11 14:21:58', '2007-06-12 14:21:58')`  
  
## <a name="adding-a-data-flow-task"></a>Ajout d’une tâche de flux de données  
 La dernière étape de la conception du flux de contrôle pour le package consiste à ajouter une tâche de flux de données.  
  
#### <a name="to-add-a-data-flow-task-and-complete-the-control-flow"></a>Pour ajouter une tâche de flux de données et terminer le flux de contrôle  
  
-   Sous l’onglet **Flux de contrôle** , ajoutez une tâche de flux et connectez la tâche ayant concaténé la chaîne de requête.  
  
## <a name="next-step"></a>Étape suivante  
 Une fois que vous avez préparé la chaîne de requête et configuré la tâche de flux de données, l'étape suivante consiste à créer la fonction table qui récupèrera les données modifiées de la base de données.  
  
 **Rubrique suivante :** [Créer la fonction de récupération des données modifiées](../../integration-services/change-data-capture/create-the-function-to-retrieve-the-change-data.md)  
  
  
