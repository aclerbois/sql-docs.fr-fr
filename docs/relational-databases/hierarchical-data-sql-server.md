---
title: Données hiérarchiques (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: relational-databases-misc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [SQL Server], tables to support
- hierarchyid [Database Engine], concepts
- hierarchical tables [Database Engine]
- SqlHierarchyId
- hierarchyid [Database Engine]
- hierarchical queries [SQL Server], using hierarchyid data type
ms.assetid: 19aefa9a-fbc2-4b22-92cf-67b8bb01671c
caps.latest.revision: 40
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c6c53ca0e99485f38d62c833cd3b0a3d3f11e4be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hierarchical-data-sql-server"></a>Données hiérarchiques (SQL Server)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Le type de données **hierarchyid** intégré simplifie le stockage et l’interrogation de données hiérarchiques. **hierarchyid** est optimisé pour représenter les arborescences, qui sont le type le plus courant de données hiérarchiques.  
  
 Les données hiérarchiques sont définies comme un jeu d'éléments de données liés entre eux par des relations hiérarchiques. Des relations hiérarchiques existent dans lesquelles un élément de données est le parent d'un autre élément. Voici quelques exemples de données hiérarchiques communément stockées dans les bases de données :  
  
-   Structure d'organisation  
  
-   Système de fichiers  
  
-   Ensemble de tâches dans un projet  
  
-   Taxonomie de termes langagiers  
  
-   Graphique de liens entre pages Web  
  
 Le type de données [hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md) permet de créer des tables avec une structure hiérarchique ou de décrire la structure hiérarchique de données stockées à un autre emplacement. Utilisez les [fonctions hierarchyid](http://msdn.microsoft.com/library/01a050f5-7580-4d5f-807c-7f11423cbb06) de [!INCLUDE[tsql](../includes/tsql-md.md)] pour interroger et gérer les données hiérarchiques.  
  
##  <a name="keyprops"></a> Propriétés principales de hierarchyid  
 Une valeur du type de données **hierarchyid** représente une position dans une hiérarchie d'arborescence. Les valeurs de **hierarchyid** ont les propriétés suivantes :  
  
-   Extrêmement compact  
  
     Le nombre moyen de bits nécessaires pour représenter un nœud dans une arborescence avec *n* nœuds dépend de la sortance moyenne (nombre moyen d’enfants d’un nœud). Pour les petites sortances (de 0 à 7), la taille est d’environ 6\*logA*n* bits, où A est la sortance moyenne. Un nœud dans une hiérarchie d'organisation de 100 000 personnes avec une sortance moyenne de 6 niveaux prend approximativement 38 bits. Ce chiffre est arrondi à 40 bits, ou 5 octets, pour le stockage.  
  
-   La comparaison est effectuée dans l'ordre à profondeur prioritaire  
  
     Étant donné deux valeurs **hierarchyid** **a** et **b**, **a<b** signifie que a se situe avant b dans un parcours à profondeur prioritaire de l’arborescence. Les index sur les types de données **hierarchyid** sont dans l’ordre à profondeur prioritaire, et les nœuds proches les uns des autres dans un parcours à profondeur prioritaire sont stockés les uns à côté des autres. Par exemple, les enfants d'un enregistrement sont stockés à côté de cet enregistrement.  
  
-   Prise en charge des insertions et suppressions arbitraires  
  
     En utilisant la méthode [GetDescendant](../t-sql/data-types/getdescendant-database-engine.md) , il est toujours possible de générer un frère à droite d'un nœud donné, à gauche d'un nœud donné, ou entre les deux frères. La propriété de comparaison est maintenue lorsqu'un nombre arbitraire de nœuds est inséré ou supprimé dans la hiérarchie. La plupart des insertions et suppressions préservent la propriété de compacité. Toutefois, les insertions entre deux nœuds produiront des valeurs hierarchyid avec une représentation légèrement moins compacte.  
  
  
##  <a name="limits"></a> Limites de hierarchyid  
 Les limites du type de données **hierarchyid** sont les suivantes :  
  
-   Une colonne de type **hierarchyid** ne représente pas automatiquement une arborescence. Il appartient à l'application de générer et d'assigner des valeurs **hierarchyid** de telle façon que la relation voulue entre les lignes soit reflétée dans les valeurs. Certaines applications peuvent avoir une colonne de type **hierarchyid** qui indique l'emplacement dans une hiérarchie définie dans une autre table.  
  
-   Il appartient à l'application de gérer la concurrence en générant et en affectant des valeurs **hierarchyid** Rien ne garantit que les valeurs **hierarchyid** d'une colonne sont uniques, à moins que l'application utilise une contrainte de clé unique ou applique elle-même l'unicité selon sa propre logique.  
  
-   Les relations hiérarchiques représentées par les valeurs **hierarchyid** ne sont pas appliquées de la même manière qu'une relation de clé étrangère. Dans une relation hiérarchique, il est possible et parfois nécessaire que A ait un enfant B, puis que A soit supprimé, laissant B avec une relation à un enregistrement inexistant. Si ce comportement n'est pas acceptable, l'application doit rechercher des descendants avant de supprimer des parents.  
  
  
##  <a name="alternatives"></a> Quand utiliser des alternatives à hierarchyid  
 Les deux alternatives à **hierarchyid** pour représenter des données hiérarchiques sont les suivantes :  
  
-   Parent/enfant  
  
-   XML  
  
 **hierarchyid** est généralement supérieur à ces alternatives. Toutefois, il existe certaines situations spécifiques, détaillées ci-dessous, pour lesquelles ces alternatives peuvent s'avérer supérieures.  
  
### <a name="parentchild"></a>Parent/enfant  
 Lors de l'utilisation de l'approche de parent/enfant, chaque ligne contient une référence au parent. La table suivante définit une table classique qui est utilisée pour contenir les lignes parent et enfant dans une relation parent/enfant :  
  
```  
USE AdventureWorks2012 ;  
GO  
  
CREATE TABLE ParentChildOrg  
   (  
    BusinessEntityID int PRIMARY KEY,  
    ManagerId int REFERENCES ParentChildOrg(BusinessEntityID),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
```  
  
 Comparaison de parent/enfant et **hierarchyid** pour les opérations courantes  
  
-   Les requêtes de sous-arborescence sont beaucoup plus rapides avec **hierarchyid**  
  
-   Les requêtes de descendants directs sont légèrement plus lentes avec **hierarchyid**  
  
-   Le déplacement de nœuds non terminaux est beaucoup plus lent avec **hierarchyid**.  
  
-   L’insertion de nœuds non terminaux et l’insertion ou le déplacement de nœuds terminaux présentent la même complexité avec **hierarchyid**.  
  
 Il se peut que la relation parent/enfant soit supérieure si les conditions suivantes sont réunies :  
  
-   La taille de la clé est essentielle. Pour le même nombre de nœuds, une valeur **hierarchyid** est égale ou supérieure à une valeur de famille d’entiers (**smallint**, **int**, **bigint**). Cela ne constitue une raison d’utiliser la relation parent/enfant que dans de rares cas, car la localité d’E/S et la complexité de l’UC de **hierarchyid** sont meilleures que celles des expressions de table communes requises quand vous utilisez une structure parent/enfant.  
  
-   Les requêtes portent rarement sur plusieurs sections de la hiérarchie. En d'autres termes, les requêtes portent habituellement sur un seul point de la hiérarchie. Dans ces cas, la co-location n'est pas importante. Par exemple, parent/enfant est supérieur lorsque la table d'organisation est utilisée uniquement pour le traitement des salaires d'employés individuels.  
  
-   Les sous-arborescences qui ne sont pas au niveau du nœud terminal sont fréquemment déplacées et les performances sont importantes. Dans une représentation parent/enfant, la modification de l'emplacement d'une ligne dans une hiérarchie affecte une seule ligne. La modification de l’emplacement d’une ligne dans une utilisation de **hierarchyid** affecte *n* lignes, où *n* est le nombre de nœuds dans la sous-arborescence déplacée.  
  
     Si les sous-arborescences qui ne sont pas au niveau du nœud terminal sont fréquemment déplacées et que les performances sont importantes, mais que la plupart des déplacements se font à un niveau bien défini de la hiérarchie, envisagez le fractionnement des niveaux supérieurs et inférieurs en deux hiérarchies. Tous les déplacements se font ainsi dans les niveaux du nœud terminal de la hiérarchie supérieure. Prenons par exemple une hiérarchie de sites Web hébergés par un service. Les sites contiennent de nombreuses pages organisées de façon hiérarchique. Les sites hébergés peuvent être déplacés vers d'autres emplacements dans la hiérarchie de site, mais les pages subordonnées sont rarement réorganisées. Cela peut être représenté de la manière suivante :  
  
    ```  
    CREATE TABLE HostedSites   
       (  
        SiteId hierarchyid, PageId hierarchyid  
       ) ;  
    GO  
    ```  
  
  
### <a name="xml"></a>XML  
 Un document XML est une arborescence. Par conséquent, une instance de type de données XML unique peut représenter la totalité d'une hiérarchie. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] when an XML dedex is created, **hierarchyid** values are used deternally to represent the position de the hierarchy.  
  
 Il peut être préférable d'utiliser le type de données XML lorsque les conditions suivantes sont réunies :  
  
-   La hiérarchie est toujours stockée et extraite dans sa totalité.  
  
-   Les données sont consommées au format XML par l'application.  
  
-   Les recherches de prédicat sont extrêmement limitées et non critiques pour les performances.  
  
 Par exemple, si une application effectue le suivi de plusieurs organisations, stocke et extrait toujours la totalité de la hiérarchie d'organisation et que ses requêtes ne portent pas sur une organisation unique, une table se présentant au format suivant peut être appropriée :  
  
```  
CREATE TABLE XMLOrg   
    (  
    Orgid int,  
    Orgdata xml  
    ) ;  
GO  
```  
  
  
##  <a name="indexing"></a> Stratégies d'indexation des données hiérarchiques  
 Il existe deux stratégies d'indexation des données hiérarchiques :  
  
-   **À profondeur prioritaire**  
  
     Un index à profondeur prioritaire stocke les lignes dans une sous-arborescence à proximité les unes des autres. Par exemple, tous les employés ayant un supérieur sont stockés à proximité de l'enregistrement de celui-ci.  
  
     Dans un index à profondeur prioritaire, tous les nœuds dans la sous-arborescence d'un nœud sont colocalisés. Les index à profondeur prioritaire sont par conséquent efficaces pour répondre aux requêtes sur les sous-arborescences, comme « Rechercher tous les fichiers dans ce dossier et ses sous-dossiers ».  
  
-   **À largeur prioritaire**  
  
     Un index à largeur prioritaire stocke les lignes en regroupant les niveaux de la hiérarchie. Par exemple, les enregistrements des employés ayant le même supérieur direct sont stockés à proximité les uns des autres.  
  
     Dans un index à largeur prioritaire, tous les enfants directs d'un nœud sont colocalisés. Les index à largeur prioritaire sont par conséquent efficaces pour répondre aux requêtes sur les enfants immédiats, telle que « Rechercher tous les employés dont ce responsable est le supérieur direct ».  
  
 Le choix entre profondeur prioritaire, largeur prioritaire ou les deux, et la sélection de l'un d'eux comme clé de clustering (si nécessaire) dépend de l'importance relative des types de requêtes ci-dessus et de l'importance relative de SELECT par rapport aux opérations DML. Pour un exemple détaillé de stratégies d'indexation, consultez [Tutorial: Using the hierarchyid Data Type](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md).  
  
  
### <a name="creating-indexes"></a>Création des index  
 Vous pouvez utiliser la méthode GetLevel() pour créer un classement à largeur prioritaire. Dans l'exemple suivant, des index à largeur prioritaire et des index à profondeur prioritaire sont créés :  
  
```wmimof  
USE AdventureWorks2012 ;   
GO  
  
CREATE TABLE Organization  
   (  
    BusinessEntityID hierarchyid,  
    OrgLevel as BusinessEntityID.GetLevel(),   
    EmployeeName nvarchar(50) NOT NULL  
   ) ;  
GO  
  
CREATE CLUSTERED INDEX Org_Breadth_First   
ON Organization(OrgLevel,BusinessEntityID) ;  
GO  
  
CREATE UNIQUE INDEX Org_Depth_First   
ON Organization(BusinessEntityID) ;  
GO  
```  
  
  
## <a name="examples"></a>Exemples  
  
### <a name="simple-example"></a>Exemple simple  
 L'exemple suivant est intentionnellement très simplifié pour vous aider à démarrer. Créez une table pour contenir des données de type geography.  
  
```  
CREATE TABLE SimpleDemo  
(Level hierarchyid NOT NULL,  
Location nvarchar(30) NOT NULL,  
LocationType nvarchar(9) NULL);  
```  
  
 Insérez maintenant les données de certains continents, pays, états et villes.  
  
```  
INSERT SimpleDemo  
VALUES   
('/1/', 'Europe', 'Continent'),  
('/2/', 'South America', 'Continent'),  
('/1/1/', 'France', 'Country'),  
('/1/1/1/', 'Paris', 'City'),  
('/1/2/1/', 'Madrid', 'City'),  
('/1/2/', 'Spain', 'Country'),  
('/3/', 'Antarctica', 'Continent'),  
('/2/1/', 'Brazil', 'Country'),  
('/2/1/1/', 'Brasilia', 'City'),  
('/2/1/2/', 'Bahia', 'State'),  
('/2/1/2/1/', 'Salvador', 'City'),  
('/3/1/', 'McMurdo Station', 'City');  
```  
  
 Sélectionnez les données, en ajoutant une colonne qui convertit les données de niveau en une valeur texte facile à comprendre. Cette requête trie également les résultats par le type de données **hierarchyid** .  
  
```  
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], *   
FROM SimpleDemo ORDER BY Level;  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
Converted Level  Level     Location         LocationType  
/1/              0x58      Europe           Continent  
/1/1/            0x5AC0    France           Country  
/1/1/1/          0x5AD6    Paris            City  
/1/2/            0x5B40    Spain            Country  
/1/2/1/          0x5B56    Madrid           City  
/2/              0x68      South America    Continent  
/2/1/            0x6AC0    Brazil           Country  
/2/1/1/          0x6AD6    Brasilia         City  
/2/1/2/          0x6ADA    Bahia            State  
/2/1/2/1/        0x6ADAB0  Salvador         City  
/3/              0x78      Antarctica       Continent  
/3/1/            0x7AC0    McMurdo Station  City  
```  
  
 Notez que la hiérarchie a une structure valide, même si elle n’est pas cohérente en interne. Bahia est le seul État. Il s'affiche dans la hiérarchie en tant qu'homologue de la ville Brasilia. De même, McMurdo Station n'a pas de pays parent. Les utilisateurs doivent décider si ce type de hiérarchie convient à leur utilisation.  
  
 Ajoutez une autre ligne et sélectionnez les résultats.  
  
```  
INSERT SimpleDemo  
VALUES ('/1/3/1/', 'Kyoto', 'City'), ('/1/3/1/', 'London', 'City');  
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], * FROM SimpleDemo ORDER BY Level;  
```  
  
 Cela indique d'autres problèmes possibles. Kyoto peut être inséré en tant que niveau `/1/3/1/` bien qu'il n'existe pas de niveau parent `/1/3/`. Et Londres et Kyoto ont la même valeur de **hierarchyid**. Là encore, les utilisateurs doivent décider si ce type de hiérarchie convient à leur utilisation, et bloquer les valeurs qui ne sont pas valides pour leur utilisation.  
  
 En outre, la table n'utilise pas le haut de la hiérarchie `'/'`. Il a été omis, car il n'y a aucun parent commun à tous les continents. Pour en ajouter un, ajoutez la planète entière.  
  
```  
INSERT SimpleDemo  
VALUES ('/', 'Earth', 'Planet');  
```  
  
##  <a name="tasks"></a> Tâches associées  
  
###  <a name="migrating"></a> Migration de parent/enfant vers hierarchyid  
 La plupart des arborescences sont représentées à l'aide de parent/enfant. La méthode la plus simple pour effectuer une migration d’une structure parent/enfant vers une table à l’aide de **hierarchyid** consiste à utiliser une colonne ou une table temporaire pour conserver une trace du nombre de nœuds à chaque niveau de la hiérarchie. Pour voir un exemple de migration de table parent/enfant, consultez la leçon 1 de [Didacticiel : utilisation du type de données hierarchyid](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md).  
  
  
###  <a name="BKMK_ManagingTrees"></a> Gestion d'une arborescence à l'aide de hierarchyid  
 Bien qu'une colonne **hierarchyid** ne représente pas nécessairement une arborescence, une application peut facilement faire en sorte que ce soit le cas.  
  
-   Lorsque vous générez de nouvelles valeurs, effectuez l'une des opérations suivantes :  
  
    -   Effectuez le suivi du dernier numéro enfant dans la ligne parent.  
  
    -   Calculez le dernier enfant. Cette opération nécessite un index à largeur prioritaire pour être efficace.  
  
-   Appliquez l'unicité en créant un index unique sur la colonne, éventuellement en tant que partie d'une clé de clustering. Pour vous assurer que des valeurs uniques sont insérées, effectuez l'une des opérations suivantes :  
  
    -   détection des échecs de violation de clés uniques et nouvelle tentative.  
  
    -   Détermination de l'unicité de chaque nouveau nœud enfant et insertion dans une transaction sérialisable.  
  
  
#### <a name="example-using-error-detection"></a>Exemple utilisant la détection d'erreurs  
 Dans l’exemple suivant, le code calcule la nouvelle valeur enfant **EmployeeId** , puis détecte toute violation de clé et retourne au marqueur **INS_EMP** pour recalculer la valeur **EmployeeId** de la nouvelle ligne :  
  
```  
USE AdventureWorks ;  
GO  
  
CREATE TABLE Org_T1  
   (  
    EmployeeId hierarchyid PRIMARY KEY,  
    OrgLevel AS EmployeeId.GetLevel(),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
  
CREATE INDEX Org_BreadthFirst ON Org_T1(OrgLevel, EmployeeId)  
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50) )   
AS  
BEGIN  
    DECLARE @last_child hierarchyid  
INS_EMP:   
    SELECT @last_child = MAX(EmployeeId) FROM Org_T1   
    WHERE EmployeeId.GetAncestor(1) = @mgrid  
INSERT Org_T1 (EmployeeId, EmployeeName)  
SELECT @mgrid.GetDescendant(@last_child, NULL), @EmpName   
-- On error, return to INS_EMP to recompute @last_child  
IF @@error <> 0 GOTO INS_EMP   
END ;  
GO  
```  
  
  
#### <a name="example-using-a-serializable-transaction"></a>Exemple utilisant une transaction sérialisable  
 Les limites du type de données **Org_BreadthFirst** garantit que la détermination de **@last_child** utilise une recherche de plage. En plus des autres cas d’erreur qu’une application peut être amenée à vérifier, une violation de clé en double après l’insertion indique une tentative d’ajouter plusieurs employés ayant le même ID. Par conséquent, **@last_child** doit être recalculé. Le code suivant utilise une transaction sérialisable et un index à largeur prioritaire pour calculer la nouvelle valeur de nœud :  
  
```  
CREATE TABLE Org_T2  
    (  
    EmployeeId hierarchyid PRIMARY KEY,  
    LastChild hierarchyid,   
    EmployeeName nvarchar(50)   
    ) ;  
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50))   
AS  
BEGIN  
DECLARE @last_child hierarchyid  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION   
  
UPDATE Org_T2   
SET @last_child = LastChild = EmployeeId.GetDescendant(LastChild,NULL)  
WHERE EmployeeId = @mgrid  
INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(@last_child, @EmpName)  
COMMIT  
END ;  
```  
  
 Le code suivant remplit la table avec trois lignes et retourne les résultats :  
  
```  
INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(hierarchyid::GetRoot(), 'David') ;  
GO  
AddEmp 0x , 'Sariya'  
GO  
AddEmp 0x58 , 'Mary'  
GO  
SELECT * FROM Org_T2  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
EmployeeId LastChild EmployeeName  
---------- --------- ------------  
0x        0x58       David  
0x58      0x5AC0     Sariya  
0x5AC0    NULL       Mary  
```  
  
  
###  <a name="BKMK_EnforcingTrees"></a> Application d'une arborescence  
 Les exemples ci-dessus illustrent la manière dont une application peut garantir la conservation d'une arborescence. Pour appliquer une arborescence à l'aide de contraintes, une colonne calculée qui définit le parent de chaque nœud peut être créée avec une contrainte de clé étrangère vers l'ID de clé primaire.  
  
```  
CREATE TABLE Org_T3  
(  
   EmployeeId hierarchyid PRIMARY KEY,  
   ParentId AS EmployeeId.GetAncestor(1) PERSISTED    
      REFERENCES Org_T3(EmployeeId),  
   LastChild hierarchyid,   
   EmployeeName nvarchar(50)  
)  
GO  
```  
  
 Cette méthode d'application d'une relation est préférable lorsqu'un code non fiable pour maintenir l'arborescence hiérarchique dispose d'un accès DML direct à la table. Toutefois, cette méthode est susceptible de réduire les performances car la contrainte doit être vérifiée à chaque opération DML.  
  
  
###  <a name="findclr"></a> Recherche d'ancêtres à l'aide du CLR  
 La recherche de l'ancêtre commun le plus bas est une opération courante impliquant deux nœuds dans une hiérarchie. Cela peut être écrit dans [!INCLUDE[tsql](../includes/tsql-md.md)] ou CLR, car le type **hierarchyid** est disponible pour les deux. L'utilisation de CLR est recommandée car les performances seront plus rapides.  
  
 Utilisez le code CLR suivant pour répertorier les ancêtres et trouver l'ancêtre commun le plus bas :  
  
```  
using System;  
using System.Collections;  
using System.Text;  
using Microsoft.SqlServer.Server;  
using Microsoft.SqlServer.Types;  
  
public partial class HierarchyId_Operations  
{  
    [SqlFunction(FillRowMethodName = "FillRow_ListAncestors")]  
    public static IEnumerable ListAncestors(SqlHierarchyId h)  
    {  
        while (!h.IsNull)  
        {  
            yield return (h);  
            h = h.GetAncestor(1);  
        }  
    }  
    public static void FillRow_ListAncestors(Object obj, out SqlHierarchyId ancestor)  
    {  
        ancestor = (SqlHierarchyId)obj;  
    }  
  
    public static HierarchyId CommonAncestor(SqlHierarchyId h1, HierarchyId h2)  
    {  
        while (!h1.IsDescendant(h2))  
            h1 = h1.GetAncestor(1);  
  
        return h1;  
    }  
}  
```  
  
 Pour utiliser les méthodes **ListAncestor** et **CommonAncestor** dans les exemples [!INCLUDE[tsql](../includes/tsql-md.md)] suivants, générez la DLL et créez l’assembly **HierarchyId_Operations** dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en exécutant du code semblable à celui-ci :  
  
```  
CREATE ASSEMBLY HierarchyId_Operations   
FROM '<path to DLL>\ListAncestors.dll'  
GO  
```  
  
  
###  <a name="ancestors"></a> Répertorier les ancêtres  
 La création d'une liste d'ancêtres d'un nœud est une opération courante, par exemple pour afficher la position au sein d'une organisation. Pour cela, vous pouvez par exemple utiliser une fonction table à l’aide de la classe **HierarchyId_Operations** définie ci-dessus :  
  
 Utilisation de [!INCLUDE[tsql](../includes/tsql-md.md)]:  
  
```  
CREATE FUNCTION ListAncestors (@node hierarchyid)  
RETURNS TABLE (node hierarchyid)  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.ListAncestors  
GO  
```  
  
 Exemple d'utilisation :  
  
```  
DECLARE @h hierarchyid  
SELECT @h = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0' -- /1/1/5/2/  
  
SELECT LoginID, OrgNode.ToString() AS LogicalNode  
FROM HumanResources.EmployeeDemo AS ED  
JOIN ListAncestors(@h) AS A   
   ON ED.OrgNode = A.Node  
GO  
```  
  
  
###  <a name="lowestcommon"></a> Recherche de l'ancêtre commun le plus bas  
 À l’aide de la classe **HierarchyId_Operations** définie ci-dessus, créez la fonction [!INCLUDE[tsql](../includes/tsql-md.md)] suivante pour rechercher l’ancêtre commun le plus bas impliquant deux nœuds dans une hiérarchie :  
  
```  
CREATE FUNCTION CommonAncestor (@node1 hierarchyid, @node2 hierarchyid)  
RETURNS hierarchyid  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.CommonAncestor  
GO  
```  
  
 Exemple d'utilisation :  
  
```  
DECLARE @h1 hierarchyid, @h2 hierarchyid  
  
SELECT @h1 = OrgNode   
FROM  HumanResources.EmployeeDemo   
WHERE LoginID = 'adventure-works\jossef0' -- Node is /1/1/3/  
  
SELECT @h2 = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0' -- Node is /1/1/5/2/  
  
SELECT OrgNode.ToString() AS LogicalNode, LoginID   
FROM HumanResources.EmployeeDemo    
WHERE OrgNode = dbo.CommonAncestor(@h1, @h2) ;  
```  
  
 Le nœud résultant est /1/1/  
  
  
###  <a name="BKMK_MovingSubtrees"></a> Déplacement de sous-arborescences  
 Une autre opération courante concerne le déplacement de sous-arborescences. La procédure suivante prend la sous-arborescence de **@oldMgr** pour en faire une sous-arborescence de **@oldMgr**(en y incluant **@newMgr**.  
  
```  
CREATE PROCEDURE MoveOrg(@oldMgr nvarchar(256), @newMgr nvarchar(256) )  
AS  
BEGIN  
DECLARE @nold hierarchyid, @nnew hierarchyid  
SELECT @nold = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @oldMgr ;  
  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION  
SELECT @nnew = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @newMgr ;  
  
SELECT @nnew = @nnew.GetDescendant(max(OrgNode), NULL)   
FROM HumanResources.EmployeeDemo WHERE OrgNode.GetAncestor(1)=@nnew ;  
  
UPDATE HumanResources.EmployeeDemo    
SET OrgNode = OrgNode.GetReparentedValue(@nold, @nnew)  
WHERE OrgNode.IsDescendantOf(@nold) = 1 ;  
  
COMMIT TRANSACTION  
END ;  
GO  
```  
  
  
## <a name="see-also"></a> Voir aussi  
 [Référence de méthodes de type de données hierarchyid](http://msdn.microsoft.com/library/01a050f5-7580-4d5f-807c-7f11423cbb06)   
 [Tutorial: Using the hierarchyid Data Type](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)   
 [hierarchyid &#40;Transact-SQL&#41;](../t-sql/data-types/hierarchyid-data-type-method-reference.md)  
  
  
