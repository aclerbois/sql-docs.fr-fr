---
title: EXPLAIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.component: t-sql|queries
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4846a576-57ea-4068-959c-81e69e39ddc1
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: f5dfb29ff0e824ebb596a3d7c2a483e7445d2a42
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="explain-transact-sql"></a>EXPLAIN (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Retourne le plan de requête pour une instruction [!INCLUDE[ssDW](../../includes/ssdw-md.md)] [!INCLUDE[DWsql](../../includes/dwsql-md.md)] sans exécuter l’instruction. Utilisez **EXPLAIN** pour afficher un aperçu des opérations qui nécessiteront un déplacement de données et afficher les coûts estimés des opérations de requête.  
  
 Pour plus d’informations sur les plans de requête, consultez « Understanding Query Plans » dans la [!INCLUDE[pdw-product-documentation_md](../../includes/pdw-product-documentation-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  

```  
EXPLAIN SQL_statement  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *SQL_statement*  
 Instruction [!INCLUDE[DWsql](../../includes/dwsql-md.md)] sur laquelle **EXPLAIN** s’exécutera. *SQL_statement* peut être l’une de ces commandes : **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **CREATE TABLE AS SELECT**, **CREATE REMOTE TABLE**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **SHOWPLAN** et l’autorisation d’exécuter *SQL_statement*. Consultez [Autorisations : GRANT, DENY, REVOKE &#40;Azure SQL Data Warehouse, Parallel Data Warehouse&#41;](../../t-sql/statements/permissions-grant-deny-revoke-azure-sql-data-warehouse-parallel-data-warehouse.md).  
  
## <a name="return-value"></a>Valeur retournée  
 La valeur de retour de la commande **EXPLAIN** est un document XML ayant la structure illustrée ci-dessous. Ce document XML répertorie toutes les opérations dans le plan de requête pour la requête en question. Chacune opération est délimitée par la balise `<dsql_operation>`. La valeur de retour est de type **nvarchar(max)**.  
  
 Le plan de requête retourné décrit les instructions SQL séquentielles. L’exécution de la requête peut impliquer des opérations en parallèle, obligeant certaines instructions séquentielles indiquées à s’exécuter simultanément.  
  
```  
\<?xml version="1.0" encoding="utf-8"?>  
<dsql_query>  
  <sql>. . .</sql>  
  <params />  
  <dsql_operations>  
    <dsql_operation>  
     . . .      
    </dsql_operation>  
    [ . . . n ]  
  <dsql_operations>  
</dsql_query>  
```  
  
 Les balises XML contiennent les informations suivantes :  
  
|Balise XML|Récapitulatif, attributs et contenu|  
|-------------|--------------------------------------|  
|\<dsql_query>|Élément de niveau supérieur ou de document.|
|\<sql>|Répercute *SQL_statement*.|  
|\<params>|Cette balise n’est pas utilisée pour l’instant.|  
|\<dsql_operations>|Récapitule et contient les étapes de la requête, et inclut des informations sur le coût de la requête. Contient également tous les blocs `<dsql_operation>`. Cette balise contient des informations d’inventaire pour l’intégralité de la requête :<br /><br /> `<dsql_operations total_cost=total_cost total_number_operations=total_number_operations>`<br /><br /> *total_cost* est la durée totale estimée de l’exécution de la requête, en millisecondes.<br /><br /> *total_number_operations* est le nombre total d’opérations de la requête. Une opération qui va être exécutée en parallèle sur plusieurs nœuds est comptée comme une seule opération.|  
|\<dsql_operation>|Décrit une opération unique dans le plan de requête. La balise \<dsql_operation> spécifie le type d’opération comme attribut :<br /><br /> `<dsql_operation operation_type=operation_type>`<br /><br /> *operation_type* est une des valeurs répertoriées dans [Querying Data (SQL Server PDW)](http://msdn.microsoft.com/en-us/3f4f5643-012a-4c36-b5ec-691c4bbe668c).<br /><br /> Le contenu du bloc `\<dsql_operation>` varie en fonction du type d’opération.<br /><br /> Consultez le tableau ci-dessous.|  
  
|Type d’opération|Contenu| Exemple|  
|--------------------|-------------|-------------|  
|BROADCAST_MOVE, DISTRIBUTE_REPLICATED_TABLE_MOVE, MASTER_TABLE_MOVE, PARTITION_MOVE, SHUFFLE_MOVE et TRIM_MOVE|Élément `<operation_cost>` avec ces attributs. Les valeurs reflètent uniquement l’opération locale :<br /><br /> -   *cost* est le coût de l’opérateur local et affiche la durée estimée de l’exécution de l’opération, en millisecondes.<br />-   *accumulative_cost* est la somme de toutes les opérations indiquées dans le plan, y compris les valeurs additionnées pour les opérations en parallèle, en millisecondes.<br />-   *average_rowsize* est la taille de ligne moyenne estimée (en octets) des lignes récupérées et passées durant l’opération.<br />-   *output_rows* est la cardinalité de sortie (nœud) et affiche le nombre de lignes de sortie.<br /><br /> `<location>` : nœuds ou distributions où l’opération va s’exécuter. Les options sont : « Control », « ComputeNode », « AllComputeNodes », « AllDistributions », « SubsetDistributions », « Distribution » et « SubsetNodes ».<br /><br /> `<source_statement>` : données sources pour le déplacement aléatoire.<br /><br /> `<destination_table>` : table temporaire interne dans laquelle les données seront déplacées.<br /><br /> `<shuffle_columns>` : (applicable uniquement aux opérations SHUFFLE_MOVE). La ou les colonnes à utiliser comme colonnes de distribution pour la table temporaire.|`<operation_cost cost="40" accumulative_cost="40" average_rowsize = "50" output_rows="100"/>`<br /><br /> `<location distribution="AllDistributions" />`<br /><br /> `<source_statement type="statement">SELECT [TableAlias_3b77ee1d8ccf4a94ba644118b355db9d].[dist_date] FROM [qatest].[dbo].[flyers] [TableAlias_3b77ee1d8ccf4a94ba644118b355db9d]       </source_statement>`<br /><br /> `<destination_table>Q_[TEMP_ID_259]_[PARTITION_ID]</destination_table>`<br /><br /> `<shuffle_columns>dist_date;</shuffle_columns>`|  
|CopyOperation|`<operation_cost>` : voir `<operation_cost>` ci-dessus.<br /><br /> `<DestinationCatalog>` : le nœud ou les nœuds de destination.<br /><br /> `<DestinationSchema>` : schéma de destination dans DestinationCatalog.<br /><br /> `<DestinationTableName>` : nom de la table de destination ou « TableName ».<br /><br /> `<DestinationDatasource>` : nom ou informations de connexion pour la base de données de destination.<br /><br /> `<Username>` et `<Password>` : ces champs indiquent qu’un nom d’utilisateur et un mot de passe pour la destination peuvent être nécessaires.<br /><br /> `<BatchSize>` : taille de lot pour l’opération de copie.<br /><br /> `<SelectStatement>` : instruction select utilisée pour effectuer la copie.<br /><br /> `<distribution>` : distribution sur laquelle la copie est effectuée.|`<operation_cost cost="0" accumulative_cost="0" average_rowsize="4" output_rows="1" />`<br /><br /> `<DestinationCatalog>master</DestinationCatalog>`<br /><br /> `<DestinationSchema>dbo</DestinationSchema>`<br /><br /> `<DestinationTableName>[TableName]</DestinationTableName>`<br /><br /> `<DestinationDatasource>localhost, 8080</DestinationDatasource>`<br /><br /> `<Username>...</Username>`<br /><br /> `<Password>...</Password>`<br /><br /> `<BatchSize>6000</BatchSize>`<br /><br /> `<SelectStatement>SELECT T1_1.c1 AS c1 FROM [qatest].[dbo].[gigs] AS T1_1</SelectStatement>`<br /><br /> `<distribution>ControlNode</distribution>`|  
|MetaDataCreate_Operation|`<source_table>` : table source pour l’opération.<br /><br /> `<destionation_table>` : table de destination pour l’opération.|`<source_table>databases</source_table>`<br /><br /> `<destination_table>MetaDataCreateLandingTempTable</destination_table>`|  
|ON|`<location>` : voir `<location>` ci-dessus.<br /><br /> `<sql_operation>` : identifie la commande SQL à exécuter sur un nœud.|`<location permanent="false" distribution="AllDistributions">Compute</location>`<br /><br /> `<sql_operation type="statement">CREATE TABLE [tempdb].[dbo]. [Q_[TEMP_ID_259]]_ [PARTITION_ID]]]([dist_date] DATE) WITH (DISTRIBUTION = HASH([dist_date]),) </sql_operation>`|  
|RemoteOnOperation|`<DestinationCatalog>` : catalogue de destination.<br /><br /> `<DestinationSchema>` : schéma de destination dans DestinationCatalog.<br /><br /> `<DestinationTableName>` : nom de la table de destination ou « TableName ».<br /><br /> `<DestinationDatasource>` : nom de la source de données de destination.<br /><br /> `<Username>` et `<Password>` : ces champs indiquent qu’un nom d’utilisateur et un mot de passe pour la destination peuvent être nécessaires.<br /><br /> `<CreateStatement>` : instruction de création de table pour la base de données de destination.|`<DestinationCatalog>master</DestinationCatalog>`<br /><br /> `<DestinationSchema>dbo</DestinationSchema>`<br /><br /> `<DestinationTableName>TableName</DestinationTableName>`<br /><br /> `<DestinationDatasource>DestDataSource</DestinationDatasource>`<br /><br /> `<Username>...</Username>`<br /><br /> `<Password>...</Password>`<br /><br /> `<CreateStatement>CREATE TABLE [master].[dbo].[TableName] ([col1] BIGINT) ON [PRIMARY] WITH(DATA_COMPRESSION=PAGE);</CreateStatement>`|  
|RETURN|`<resultset>` : identificateur du jeu de résultats.|`<resultset>RS_19</resultset>`|  
|RND_ID|`<identifier>` : identificateur de l’objet créé.|`<identifier>TEMP_ID_260</identifier>`|  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 **EXPLAIN** peut être appliqué uniquement à des requêtes *optimisables*, c’est-à-dire des requêtes qui peuvent être améliorées ou modifiées en fonction des résultats d’une commande **EXPLAIN**. Les commandes **EXPLAIN** prises en charge sont répertoriées ci-dessus. Toute tentative d’utilisation d’une commande **EXPLAIN** avec un type de requête non pris en charge retourne une erreur ou répercute la requête.  
  
 **EXPLAIN** n’est pas pris en charge dans une transaction utilisateur.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre une commande **EXPLAIN** exécutée sur une instruction **SELECT**, ainsi que le résultat XML retourné.  
  
 **Envoi d’une instruction EXPLAIN**  
  
 La commande envoyée dans cet exemple est la suivante :  
  
```  
-- Uses AdventureWorks  
  
EXPLAIN   
    SELECT CAST (AVG(YearlyIncome) AS int) AS AverageIncome,   
        CAST(AVG(FIS.SalesAmount) AS int) AS AverageSales,   
        G.StateProvinceName, T.SalesTerritoryGroup  
    FROM dbo.DimGeography AS G  
    JOIN dbo.DimSalesTerritory AS T  
        ON G.SalesTerritoryKey = T.SalesTerritoryKey  
    JOIN dbo.DimCustomer AS C  
        ON G.GeographyKey = C.GeographyKey  
    JOIN dbo.FactInternetSales AS FIS  
        ON C.CustomerKey = FIS.CustomerKey  
    WHERE T.SalesTerritoryGroup IN ('North America', 'Pacific')  
        AND Gender = 'F'  
    GROUP BY G.StateProvinceName, T.SalesTerritoryGroup  
    ORDER BY AVG(YearlyIncome) DESC;  
GO  
```  
  
 Après l’exécution de l’instruction avec l’option **EXPLAIN**, l’onglet message présente une seule ligne intitulée **explain** et commençant par le texte XML `\<?xml version="1.0" encoding="utf-8"?>` Cliquez sur le code XML pour afficher l’intégralité du texte dans une fenêtre XML. Pour faciliter la compréhension des commentaires suivants, activez l’affichage des numéros de ligne dans SSDT.  
  
#### <a name="to-turn-on-line-numbers"></a>Pour activer les numéros de ligne  
  
1.  Quand la sortie s’affiche dans l’onglet **explain** de SSDT, dans le menu **OUTILS**, sélectionnez **Options**.  
  
2.  Développez la section de **l’éditeur de texte**, développez **XML**, puis cliquez sur **Général**.  
  
3.  Dans la zone **Affichage**, cochez **Numéros de ligne**.  
  
4.  Cliquez sur **OK**.  
  
 **Exemple de sortie EXPLAIN**  
  
 Résultat XML de la commande **EXPLAIN** avec les numéros de ligne activés :  
  
```  
1  \<?xml version="1.0" encoding="utf-8"?>  
2  <dsql_query>  
3    <sql>SELECT CAST (AVG(YearlyIncome) AS int) AS AverageIncome,   
4          CAST(AVG(FIS.SalesAmount) AS int) AS AverageSales,   
5          G.StateProvinceName, T.SalesTerritoryGroup  
6      FROM dbo.DimGeography AS G  
7      JOIN dbo.DimSalesTerritory AS T  
8          ON G.SalesTerritoryKey = T.SalesTerritoryKey  
9      JOIN dbo.DimCustomer AS C  
10          ON G.GeographyKey = C.GeographyKey  
11      JOIN dbo.FactInternetSales AS FIS  
12          ON C.CustomerKey = FIS.CustomerKey  
13      WHERE T.SalesTerritoryGroup IN ('North America', 'Pacific')  
14          AND Gender = 'F'  
15      GROUP BY G.StateProvinceName, T.SalesTerritoryGroup  
16      ORDER BY AVG(YearlyIncome) DESC</sql>  
17    <dsql_operations total_cost="0.926237696" total_number_operations="9">  
18      <dsql_operation operation_type="RND_ID">  
19        <identifier>TEMP_ID_16893</identifier>  
20      </dsql_operation>  
21      <dsql_operation operation_type="ON">  
22        <location permanent="false" distribution="AllComputeNodes" />  
23        <sql_operations>  
24          <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_16893] ([CustomerKey] INT NOT NULL, [GeographyKey] INT, [YearlyIncome] MONEY ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>  
25        </sql_operations>  
26      </dsql_operation>  
27      <dsql_operation operation_type="BROADCAST_MOVE">  
28        <operation_cost cost="0.121431552" accumulative_cost="0.121431552" average_rowsize="16" output_rows="31.6228" />  
29        <source_statement>SELECT [T1_1].[CustomerKey] AS [CustomerKey],  
30         [T1_1].[GeographyKey] AS [GeographyKey],  
31         [T1_1].[YearlyIncome] AS [YearlyIncome]  
32  FROM   (SELECT [T2_1].[CustomerKey] AS [CustomerKey],  
33                 [T2_1].[GeographyKey] AS [GeographyKey],  
34                 [T2_1].[YearlyIncome] AS [YearlyIncome]  
35          FROM   [AdventureWorksPDW2012].[dbo].[DimCustomer] AS T2_1  
36          WHERE  ([T2_1].[Gender] = CAST (N'F' COLLATE Latin1_General_100_CI_AS_KS_WS AS NVARCHAR (1)) COLLATE Latin1_General_100_CI_AS_KS_WS)) AS T1_1</source_statement>  
37        <destination_table>[TEMP_ID_16893]</destination_table>  
38      </dsql_operation>  
39      <dsql_operation operation_type="RND_ID">  
40        <identifier>TEMP_ID_16894</identifier>  
41      </dsql_operation>  
42      <dsql_operation operation_type="ON">  
43        <location permanent="false" distribution="AllDistributions" />  
44        <sql_operations>  
45          <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_16894] ([StateProvinceName] NVARCHAR(50) COLLATE Latin1_General_100_CI_AS_KS_WS, [SalesTerritoryGroup] NVARCHAR(50) COLLATE Latin1_General_100_CI_AS_KS_WS NOT NULL, [col] BIGINT, [col1] MONEY NOT NULL, [col2] BIGINT, [col3] MONEY NOT NULL ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>  
46        </sql_operations>  
47      </dsql_operation>  
48      <dsql_operation operation_type="SHUFFLE_MOVE">  
49        <operation_cost cost="0.804806144" accumulative_cost="0.926237696" average_rowsize="232" output_rows="108.406" />  
50        <source_statement>SELECT [T1_1].[StateProvinceName] AS [StateProvinceName],  
51         [T1_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
52         [T1_1].[col2] AS [col],  
53         [T1_1].[col] AS [col1],  
54         [T1_1].[col3] AS [col2],  
55         [T1_1].[col1] AS [col3]  
56  FROM   (SELECT ISNULL([T2_1].[col1], CONVERT (MONEY, 0.00, 0)) AS [col],  
57                 ISNULL([T2_1].[col3], CONVERT (MONEY, 0.00, 0)) AS [col1],  
58                 [T2_1].[StateProvinceName] AS [StateProvinceName],  
59                 [T2_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
60                 [T2_1].[col] AS [col2],  
61                 [T2_1].[col2] AS [col3]  
62          FROM   (SELECT   COUNT_BIG([T3_2].[YearlyIncome]) AS [col],  
63                           SUM([T3_2].[YearlyIncome]) AS [col1],  
64                           COUNT_BIG(CAST ((0) AS INT)) AS [col2],  
65                           SUM([T3_2].[SalesAmount]) AS [col3],  
66                           [T3_2].[StateProvinceName] AS [StateProvinceName],  
67                           [T3_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
68                  FROM     (SELECT [T4_1].[SalesTerritoryKey] AS [SalesTerritoryKey],  
69                                   [T4_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
70                            FROM   [AdventureWorksPDW2012].[dbo].[DimSalesTerritory] AS T4_1  
71                            WHERE  (([T4_1].[SalesTerritoryGroup] = CAST (N'North America' COLLATE Latin1_General_100_CI_AS_KS_WS AS NVARCHAR (13)) COLLATE Latin1_General_100_CI_AS_KS_WS)  
72                                    OR ([T4_1].[SalesTerritoryGroup] = CAST (N'Pacific' COLLATE Latin1_General_100_CI_AS_KS_WS AS NVARCHAR (7)) COLLATE Latin1_General_100_CI_AS_KS_WS))) AS T3_1  
73                           INNER JOIN  
74                           (SELECT [T4_1].[SalesTerritoryKey] AS [SalesTerritoryKey],  
75                                   [T4_2].[YearlyIncome] AS [YearlyIncome],  
76                                   [T4_2].[SalesAmount] AS [SalesAmount],  
77                                   [T4_1].[StateProvinceName] AS [StateProvinceName]  
78                            FROM   [AdventureWorksPDW2012].[dbo].[DimGeography] AS T4_1  
79                                   INNER JOIN  
80                                   (SELECT [T5_2].[GeographyKey] AS [GeographyKey],  
81                                           [T5_2].[YearlyIncome] AS [YearlyIncome],  
82                                           [T5_1].[SalesAmount] AS [SalesAmount]  
83                                    FROM   [AdventureWorksPDW2012].[dbo].[FactInternetSales] AS T5_1  
84                                           INNER JOIN  
85                                           [tempdb].[dbo].[TEMP_ID_16893] AS T5_2  
86                                           ON ([T5_1].[CustomerKey] = [T5_2].[CustomerKey])) AS T4_2  
87                                   ON ([T4_2].[GeographyKey] = [T4_1].[GeographyKey])) AS T3_2  
88                           ON ([T3_1].[SalesTerritoryKey] = [T3_2].[SalesTerritoryKey])  
89                  GROUP BY [T3_2].[StateProvinceName], [T3_1].[SalesTerritoryGroup]) AS T2_1) AS T1_1</source_statement>  
90        <destination_table>[TEMP_ID_16894]</destination_table>  
91        <shuffle_columns>StateProvinceName;</shuffle_columns>  
92      </dsql_operation>  
93      <dsql_operation operation_type="ON">  
94        <location permanent="false" distribution="AllComputeNodes" />  
95        <sql_operations>  
96          <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_16893]</sql_operation>  
97        </sql_operations>  
98      </dsql_operation>  
99      <dsql_operation operation_type="RETURN">  
100        <location distribution="AllDistributions" />  
101        <select>SELECT   [T1_1].[col] AS [col],  
102           [T1_1].[col1] AS [col1],  
103           [T1_1].[StateProvinceName] AS [StateProvinceName],  
104           [T1_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
105           [T1_1].[col2] AS [col2]  
106  FROM     (SELECT CONVERT (INT, [T2_1].[col], 0) AS [col],  
107                   CONVERT (INT, [T2_1].[col1], 0) AS [col1],  
108                   [T2_1].[StateProvinceName] AS [StateProvinceName],  
109                   [T2_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup],  
110                   [T2_1].[col] AS [col2]  
111            FROM   (SELECT CASE  
112                            WHEN ([T3_1].[col] = CAST ((0) AS BIGINT)) THEN CAST (NULL AS MONEY)  
113                            ELSE ([T3_1].[col1] / CONVERT (MONEY, [T3_1].[col], 0))  
114                           END AS [col],  
115                           CASE  
116                            WHEN ([T3_1].[col2] = CAST ((0) AS BIGINT)) THEN CAST (NULL AS MONEY)  
117                            ELSE ([T3_1].[col3] / CONVERT (MONEY, [T3_1].[col2], 0))  
118                           END AS [col1],  
119                           [T3_1].[StateProvinceName] AS [StateProvinceName],  
120                           [T3_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
121                    FROM   (SELECT ISNULL([T4_1].[col], CONVERT (BIGINT, 0, 0)) AS [col],  
122                                   ISNULL([T4_1].[col1], CONVERT (MONEY, 0.00, 0)) AS [col1],  
123                                   ISNULL([T4_1].[col2], CONVERT (BIGINT, 0, 0)) AS [col2],  
124                                   ISNULL([T4_1].[col3], CONVERT (MONEY, 0.00, 0)) AS [col3],  
125                                   [T4_1].[StateProvinceName] AS [StateProvinceName],  
126                                   [T4_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
127                            FROM   (SELECT   SUM([T5_1].[col]) AS [col],  
128                                             SUM([T5_1].[col1]) AS [col1],  
129                                             SUM([T5_1].[col2]) AS [col2],  
130                                             SUM([T5_1].[col3]) AS [col3],  
131                                             [T5_1].[StateProvinceName] AS [StateProvinceName],  
132                                             [T5_1].[SalesTerritoryGroup] AS [SalesTerritoryGroup]  
133                                    FROM     [tempdb].[dbo].[TEMP_ID_16894] AS T5_1  
134                                    GROUP BY [T5_1].[StateProvinceName], [T5_1].[SalesTerritoryGroup]) AS T4_1) AS T3_1) AS T2_1) AS T1_1  
135  ORDER BY [T1_1].[col2] DESC</select>  
136      </dsql_operation>  
137      <dsql_operation operation_type="ON">  
138        <location permanent="false" distribution="AllDistributions" />  
139        <sql_operations>  
140          <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_16894]</sql_operation>  
141        </sql_operations>  
142      </dsql_operation>  
143    </dsql_operations>  
144  </dsql_query>  
  
```  
  
 **Explication de la sortie EXPLAIN**  
  
 La sortie ci-dessus contient 144 lignes numérotées. Il est possible que votre sortie de la même requête soit un peu différente. La liste suivante décrit les sections principales.  
  
-   Les lignes 3 à 16 décrivent la requête analysée.  
  
-   La ligne 17 indique qu’il y a 9 opérations au total. Le début de chaque opération est signalé par les mots **dsql_operation**.  
  
-   La ligne 18 démarre l’opération 1. Les lignes 18 et 19 indiquent qu’une opération **RND_ID** va créer un numéro d’identification aléatoire utilisé pour décrire l’objet. L’objet décrit dans la sortie ci-dessus est **TEMP_ID_16893**. Votre numéro sera différent.  
  
-   La ligne 20 démarre l’opération 2. Lignes 21 à 25 : l’opération crée une table temporaire nommée **TEMP_ID_16893** sur tous les nœuds de calcul.  
  
-   La ligne 26 démarre l’opération 3. Lignes 27 à 37 : l’opération déplace les données vers **TEMP_ID_16893** en effectuant un déplacement par diffusion. La requête envoyée à chaque nœud de calcul est fournie. La ligne 37 spécifie que la table de destination est **TEMP_ID_16893**.  
  
-   La ligne 38 démarre l’opération 4. Lignes 39 à 40 : l’opération crée un ID aléatoire pour une table. **TEMP_ID_16894** est le numéro d’identification utilisé dans l’exemple ci-dessus. Votre numéro sera différent.  
  
-   La ligne 41 démarre l’opération 5. Lignes 42 à 46 : l’opération crée une table temporaire nommée **TEMP_ID_16894** sur tous les nœuds.  
  
-   La ligne 47 démarre l’opération 6. Lignes 48 à 91 : l’opération déplace les données de plusieurs tables (dont **TEMP_ID_16893**) vers la table **TEMP_ID_16894**, en effectuant un déplacement aléatoire. La requête envoyée à chaque nœud de calcul est fournie. La ligne 90 spécifie que la table de destination est **TEMP_ID_16894**. La ligne 91 spécifie les colonnes.  
  
-   La ligne 92 démarre l’opération 7. Lignes 93 à 97 : l’opération supprime la table temporaire **TEMP_ID_16893** sur tous les nœuds de calcul.  
  
-   La ligne 98 démarre l’opération 8. Lignes 99 à 135 : l’opération retourne les résultats au client. Utilise la requête fournie pour obtenir les résultats.  
  
-   La ligne 136 démarre l’opération 9. Lignes 137 à 140 : l’opération supprime la table temporaire **TEMP_ID_16894** sur tous les nœuds.  
  
  

