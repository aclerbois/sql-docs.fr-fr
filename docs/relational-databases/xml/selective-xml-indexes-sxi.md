---
title: Index XML sélectifs (SXI) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 598ecdcd-084b-4032-81b2-eed6ae9f5d44
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1bc3e5c4f5476fe97a096a2e1a1e4ca9748f709e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="selective-xml-indexes-sxi"></a>Index XML sélectifs (SXI)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les index XML sélectifs sont un autre type d'index XML disponible en plus des index XML ordinaires. Les objectifs de la fonctionnalité d'index XML sélectif sont les suivants :  
  
-   Améliorer les performances des requêtes sur les données XML stockées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Prendre en charge une indexation plus rapide des charges de travail importantes de données XML.  
  
-   Améliorer l'évolutivité en réduisant les coûts de stockage des index XML.  
  
 La principale limitation avec des index XML ordinaires est qu'ils indexent le document XML entier. Cela entraîne plusieurs inconvénients significatifs, tels que la réduction des performances des requêtes et la hausee du coût de maintenance de l'index, principalement liés aux coûts de stockage de l'index.  
  
 La fonctionnalité d'index XML sélectif vous permet de promouvoir uniquement certains chemins d'accès des documents XML à indexer. Lors de la création d'index, les chemins d'accès sont évalués et les nœuds vers lesquels ils pointent sont fragmentés et stockés dans une table relationnelle dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette fonctionnalité utilise un algorithme efficace de mappage développé par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Research en collaboration avec l'équipe de produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cet algorithme associe les nœuds XML à une table relationnelle, et permet d'obtenir des performances exceptionnelles tout en exigeant qu'un espace de stockage modeste.  
  
 La fonctionnalité d'index XML sélectif prend également en charge les index XML secondaires sélectifs sur les nœuds qui ont été indexés par un index XML sélectif. Ces index sélectifs secondaires sont efficaces et améliorent encore les performances des requêtes.  
  
##  <a name="benefits"></a> Avantages des index XML sélectifs  
 Les index XML sélectif présentent les avantages suivants :  
  
1.  Les performances des requêtes sont considérablement améliorées sur le type de données XML pour les charges de requête classique.  
  
2.  Capacité de stockage réduite comparée aux index XML ordinaires.  
  
3.  Coûts réduits de maintenance des index par rapport aux index XML ordinaires.  
  
4.  Aucun besoin de mettre à jour des applications pour tirer parti des index XML sélectifs.  
  
  
##  <a name="compare"></a> Index XML sélectifs et index XML primaires sélectifs  
  
> [!IMPORTANT]  
>  Créez un index XML sélectif au lieu d'un index XML ordinaire dans la plupart des cas pour obtenir de meilleures performances et un stockage plus efficace.  
  
 Toutefois, un index XML sélectif n'est pas recommandé lorsque l'une des conditions suivantes est remplie :  
  
-   Vous mappez un grand nombre de chemins d'accès de nœud.  
  
-   Vous devez prendre en charge des requêtes d'éléments inconnus ou des éléments d'un emplacement inconnu dans la structure du document.  
  
  
##  <a name="example"></a> Exemple simple d'un index XML sélectif  
 Examinons le fragment de code XML suivant sous la forme d'un document XML dans une table d'environ 500 000 lignes :  
  
```xml  
<book>  
    <created>2004-03-01</created>   
    <authors>Various</authors>  
    <subjects>  
        <subject>English wit and humor -- Periodicals</subject>  
        <subject>AP</subject>   
    </subjects>  
    <title>Punch, or the London Charivari, Volume 156, April 2, 1919</title>  
    <id>etext11617</id>  
</book>  
```  
  
 La création d'un index XML primaire sur un aussi grand nombre de lignes de ce schéma simple prend beaucoup de temps. L'interrogation de ces données souffre également du fait qu'un index XML primaire ne prend pas en charge l'indexation sélective.  
  
 Si vous devez uniquement interroger ces données sur le chemin d'accès `/book/title` et le chemin d'accès `/book/subjects` , créez l'index XML sélectif suivant :  
  
```sql  
CREATE SELECTIVE XML INDEX SXI_index  
ON Tbl(xmlcol)  
FOR   
(  
    pathTitle = '/book/title/text()' AS XQUERY 'xs:string',   
    pathAuthors = '/book/authors' AS XQUERY 'node()',  
    pathId = '/book/id' AS SQL NVARCHAR(100)  
)  
```  
  
 L'instruction précédente est un bon exemple de syntaxe CREATE que vous utilisez lorsque vous créez un index XML sélectif. Dans l'instruction CREATE, vous spécifiez d'abord un nom pour l'index et vous identifiez la table et la colonne XML à indexer. Vous spécifiez ensuite les chemins d'accès à indexer. Un chemin d'accès possède trois parties :  
  
1.  Un nom qui identifie de façon unique le chemin d'accès.  
  
2.  Une expression XQuery qui décrit le chemin d'accès.  
  
3.  Des indicateurs d'optimisation facultatifs.  
  
 Pour plus d'informations sur ces éléments, consultez [Tâches associées](#reltasks).  
  
  
## <a name="supported-features-prerequisites-and-limitations"></a>Fonctionnalités prises en charge, configuration requise et limitations  
  
###  <a name="features"></a> Fonctionnalités XML prises en charge  
 Les index XML sélectifs prennent en charge la syntaxe XQuery prise en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans les méthodes exist(), value() et nodes().  
  
-   Pour les méthodes exist(), value() et nodes(), les index XML sélectifs contiennent suffisamment d'informations pour convertir l'expression entière.  
  
-   Pour les méthodes query() et modify(), les index XML sélectifs ne peuvent être utilisés que pour le filtrage de nœud.  
  
-   Pour la méthode query(), les index XML sélectifs ne sont pas utilisés pour récupérer les résultats.  
  
-   Pour la méthode modify(), les index XML sélectifs ne sont pas utilisés pour mettre à jour les documents XML.  
  
  
###  <a name="unsupported"></a> Fonctionnalités XML non prises en charge  
 Les index XML sélectifs ne prennent pas en charge les fonctionnalités suivantes qui sont prises en charge dans l'implémentation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du XML :  
  
-   Indexation des nœuds avec des types complexes XS : types d'unions, types de séquences et types de listes.  
  
-   Indexation des nœuds avec des types de données binaires XS : par exemple, base64Binary et hexBinary.  
  
-   Spécification des nœuds à indexer avec des expressions Xpath qui contiennent le caractère générique `*` à la fin : par exemple,  `/a/b/c/*`, `/a//b/*`ou `/a/b/*:c`.  
  
-   Indexation d'un axe autre que l'enfant, l'attribut ou le descendant. `//<step>` est autorisé comme un cas particulier.  
  
-   Indexation d'instructions de traitement XML et de commentaires.  
  
-   Spécification et récupération de l'identificateur d'un nœud en utilisant la fonction id().  
  
  
###  <a name="prereq"></a> Configuration requise  
 Les conditions suivantes doivent être réunies avant de pouvoir créer un index XML sélectif sur une colonne XML dans une table utilisateur :  
  
-   Un index cluster doit exister dans la clé primaire de la table utilisateur.  
  
-   La clé primaire de la table d'utilisateur est limitée à une taille de 128 octets si elle est utilisée avec des index XML sélectifs.  
  
-   La clé de clustering de la table utilisateur est limitée à 15 colonnes si elle est utilisée avec des index XML sélectifs.  
  
  
###  <a name="limits"></a> Limitations  
 **Limitations et exigences générales**  
  
-   Un index XML sélectif ne peut être créé que sur une colonne XML unique.  
  
-   Vous ne pouvez pas créer un index XML sélectif sur une colonne autre qu'une colonne XML.  
  
-   Chaque colonne XML d'une table peut présenter un seul index XML sélectif.  
  
-   Chaque table peut comporter jusqu'à 249 index XML sélectifs.  
  
 **Limitations des objets pris en charge**  
  
 Vous ne pouvez pas créer d'index XML sélectifs sur les objets suivants :  
  
1.  Colonnes XML dans une vue.  
  
2.  Variable table avec des colonnes XML.  
  
3.  Variables de type XML.  
  
4.  Colonnes XML calculées.  
  
5.  Colonnes XML avec une profondeur de plus de 128 nœuds imbriqués.  
  
 **Limitations relatives au stockage**  
  
 Il existe une limite finie sur le nombre de nœuds du document XML pouvant être ajoutés à l'index. Un index XML sélectif mappe des documents XML à une table relationnelle. Par conséquent, il ne peut pas y avoir plus de 1 024 colonnes non Null dans une ligne donnée de la table. En outre, la plupart des limitations des colonnes éparses s'appliquent également aux index XML sélectifs, car les index utilisent des colonnes éparses pour le stockage.  
  
 Le nombre maximal de colonnes non Null prises en charge dans toute ligne donnée dépend de la taille des données dans les colonnes :  
  
-   Dans le meilleur des cas, 1024 colonnes non NULL sont prises en charge quand toutes les colonnes sont de type **bit**.  
  
-   Dans le pire des cas, seules 236 colonnes non NULL sont prises en charge quand toutes les colonnes sont de grands objets de type **varchar**.  
  
 Les index XML sélectifs utilisent une à quatre colonnes en interne pour chaque chemin d'accès du nœud qui est indexé. Le nombre total de nœuds qui peuvent être indexés s'étend de 60 à plusieurs centaines de nœuds, selon la taille réelle des données dans les chemins d'accès indexés.  
  
-   Dans le pire des cas, si certains ou tous les nœuds sont mappés en utilisant `//` dans la définition du chemin d'accès du nœud, le nombre maximal de nœuds indexés est 60.  
  
-   Dans le meilleur des cas, si des nœuds sont mappés sans utiliser `//` dans la définition du chemin d'accès du nœud, le nombre maximal de nœuds indexés est 200.  
  
 **Les index XML sélectifs sont régénérés lorsque vous créez (CREATE) ou modifiez (ALTER) l'index.**  
  
 Lorsque vous créez ou modifiez un index XML sélectif, il est reconstruit en mode hors connexion monothread. L'utilisation fréquente d'instructions ALTER a un impact négatif sur les performances des requêtes exécutées sur des documents XML indexés.  
  
 **Autres limitations**  
  
-   Les index XML sélectifs ne sont pas pris en charge dans les indicateurs de requête.  
  
-   Les index XML sélectifs et les index XML secondaires sélectifs ne sont pas pris en charge par l'Assistant Paramétrage de base de données.  
  
  
##  <a name="reltasks"></a> Tâches associées  
  
|||  
|-|-|  
|**Tâche**|**Rubrique**|  
|Spécifiez les chemins d'accès du nœud que vous souhaitez indexer et les indicateurs facultatifs d'optimisation lorsque vous créez ou modifiez un index XML sélectif.|[Spécifier les chemins d'accès et les indicateurs d'optimisation des index XML sélectifs](../../relational-databases/xml/specify-paths-and-optimization-hints-for-selective-xml-indexes.md)|  
|Créez, modifiez ou supprimez un index XML sélectif.|[Créer, modifier ou supprimer des index XML sélectifs](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)|  
|Créez, modifiez ou supprimez un index XML secondaire sélectif.|[Créer, modifier ou supprimer des index XML secondaires sélectifs](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)|  
  
  
  
