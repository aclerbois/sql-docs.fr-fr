---
title: Spécifier des indicateurs de fin de champ et de fin de ligne (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], terminators
- field terminators [SQL Server]
- data formats [SQL Server], terminators
- row terminators [SQL Server]
- terminators [SQL Server]
ms.assetid: f68b6782-f386-4947-93c4-e89110800704
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 9d0890d79f2277b5f1ea1676bed9f4c9b20e6590
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-field-and-row-terminators-sql-server"></a>Spécifier des indicateurs de fin de champ et de fin de ligne (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Dans le cas de champs de données de type caractère, des caractères facultatifs de fin vous permettent d'indiquer la fin de chaque champ inclus dans un fichier de données par un *indicateur de fin de champ* et la fin de chaque ligne par un *indicateur de fin de ligne*. Les caractères de fin constituent un moyen d'indiquer à des programmes en cours de lecture du fichier de données la fin d'un champ ou d'une ligne et le début du suivant.  
  
> [!IMPORTANT]  
>  Lorsque vous utilisez le format natif ou natif Unicode, préférez les préfixes de longueur aux indicateurs de fin de champ. Les données au format natif peuvent en effet entrer en conflit avec les indicateurs de fin, car les fichiers de données sont stockés au format de données binaire interne de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="characters-supported-as-terminators"></a>Caractères pris en charge en tant qu'indicateurs de fin  
 La commande **bcp** , l’instruction BULK INSERT et le fournisseur d’ensembles de lignes OPENROWSET prennent en charge un choix complet de caractères pouvant être utilisés en tant qu’indicateurs de fin de champ ou de fin de ligne et recherchent systématiquement la première occurrence de chacun de ces indicateurs. Le tableau ci-dessous répertorie les caractères pris en charge pour les terminateurs.  
  
|Caractère de terminaison|Indiqué par|  
|---------------------------|------------------|  
|Onglet|\t<br /><br /> Il s'agit de l'indicateur de fin de champ par défaut.|  
|Caractère de saut de ligne|\n<br /><br /> Il s'agit de l'indicateur de fin de ligne par défaut.|  
|Retour chariot/saut de ligne|\r|  
|Barre oblique inverse*|\\\|  
|Indicateur de fin NULL (indicateur de fin non visible)**|\0|  
|Tout caractère imprimable (les caractères de contrôle ne le sont pas, sauf les valeurs NULL, tabulation, saut de ligne et retour chariot)|(*, A, t, l, etc.)|  
|Une chaîne de 10 caractères imprimables maximum, y compris certains ou tous les indicateurs de fin énumérés précédemment|(**\t\*\*, fin, !!!!!!!!!!, \t—\n, etc.)|  
  
 *Seuls les caractères t, n, r, 0, et '\0' fonctionnent avec le caractère d’échappement Barre oblique inverse pour générer un caractère de contrôle.  
  
 **Bien que le caractère de contrôle null (\0) ne soit pas visible à l’impression, il constitue bel et bien un caractère à part dans le fichier de données. En d'autres termes, utiliser ce caractère en tant qu'indicateur de fin de champ ou de fin de ligne ne revient pas à simplement omettre cet indicateur de fin.  
  
> [!IMPORTANT]  
>  Si un caractère correspond à un caractère indicateur de fin dans la continuité des données, il est alors interprété en tant que tel et non en tant que donnée ; les données suivant ce caractère sont donc considérées comme faisant partie du champ ou de l'enregistrement suivant. Par conséquent, choisissez vos indicateurs consciencieusement afin qu'ils n'apparaissent jamais au sein de vos données. Par exemple, il n'est pas judicieux de choisir un indicateur de fin de champ de substitut faible pour un indicateur de fin de champ si les données contiennent ce substitut faible.  
  
## <a name="using-row-terminators"></a>Manipulation des indicateurs de fin de ligne  
 L'indicateur de fin de ligne peut être le même que celui employé en tant qu'indicateur du dernier champ. Cependant, un indicateur de fin de ligne distinct s'avère généralement plus utile. Par exemple, afin d'obtenir en sortie des tabulations, marquez la fin du dernier champ de chaque ligne par le caractère de saut de ligne (\n) et la fin de tous les autres champs par le caractère de tabulation (\t). Pour placer chaque enregistrement de données dans une ligne séparée dans le fichier de données, attribuez la combinaison de caractères \r\n à l'indicateur de fin de ligne.  
  
> [!NOTE]  
>  Quand vous utilisez la commande **bcp** de manière interactive et que vous spécifiez le caractère de saut de ligne \n en tant qu’indicateur de fin de ligne, **bcp** fait précéder ce caractère du retour chariot \r automatiquement, modifiant ainsi l’indicateur de fin de ligne en \r\n.  
  
## <a name="specifying-terminators-for-bulk-export"></a>Spécification d'indicateurs pour l'exportation en bloc  
 Si vous procédez à une exportation en bloc des données de type **char** ou **nchar** , mais que vous voulez utiliser un indicateur de fin autre que celui défini par défaut, vous devez indiquer ce dernier à la commande **bcp** . Vous pouvez le spécifier de l'une des façons suivantes :  
  
-   Grâce à un fichier de format précisant l'indicateur de fin pour chaque champ.  
  
    > [!NOTE]  
    >  Pour plus d’informations sur l’utilisation des fichiers de format, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
-   Sans avoir recours à un fichier de format, les options suivantes sont aussi à votre disposition :  
  
    -   Utilisation du commutateur **-t** pour préciser l’indicateur de fin de champ pour tous les champs à l’exception du dernier champ de la ligne et utilisation du commutateur **-r** pour préciser un indicateur de fin de ligne.  
  
    -   Utilisation d’un commutateur de format caractère (**-c** ou **-w**) sans le commutateur **-t** , qui définit l’indicateur de fin de champ sur la tabulation, \t. Cette option est équivalente à la spécification de **-t**\t.  
  
        > [!NOTE]  
        >  Si vous ajoutez le commutateur **-n** (correspondant aux données natives) ou **-N** (format natif Unicode), les indicateurs de fin ne sont pas insérés.  
  
    -   Si une commande **bcp** interactive contient l’option **in** ou **out** sans le commutateur du fichier de format (**-f**) ou celui du fichier de données (**-n**, **-c**, **-w**ou **-N**) mais que vous n’avez pas non plus précisé la longueur du préfixe et la longueur de champ, la commande vous invite à saisir l’indicateur de fin de champ de chaque champ, tout en vous proposant par défaut de ne pas en spécifier :  
  
         `Enter field terminator [none]:`  
  
         généralement, cette valeur par défaut convient. Dans le cas des champs de données de type **char** ou **nchar** , consultez cependant la sous-section « Instructions pour l'utilisation d'indicateurs de fin ». Pour obtenir un exemple illustrant cette invite en contexte, consultez [Spécifier des formats de données pour la compatibilité lors de l’utilisation de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
        > [!NOTE]  
        >  Après avoir indiqué de façon interactive tous les champs d’une commande **bcp**, cette dernière vous demande de sauvegarder vos réponses dans un fichier de format autre que XML pour chacun des champs fournis. Pour plus d’informations sur les fichiers de format non-XML, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
### <a name="guidelines-for-using-terminators"></a>Instructions pour l'utilisation d'indicateurs de fin  
 Dans certains cas, un indicateur de fin s'avère utile pour les champs de données de type **char** ou **nchar** . Exemple :  
  
-   pour une colonne de données qui contient une valeur Null dans un fichier de données qui sera importé dans un programme qui ne comprend pas les informations de longueur de préfixe ;  
  
     Toute colonne de données qui contient une valeur Null est considérée de longueur variable. En l'absence de longueurs de préfixe, un indicateur de fin est nécessaire pour identifier la fin d'un champ Null et garantir que les données sont correctement interprétées.  
  
-   pour une longue colonne de longueur fixe dont l'espace n'est utilisé que partiellement par de nombreuses lignes.  
  
     Dans cette situation, la spécification d'un indicateur de fin réduit l'espace de stockage et permet au champ d'être traité comme un champ de longueur variable.  
  
### <a name="examples"></a>Exemples  
 Cette commande d’exportation en bloc exporte les données de la table `AdventureWorks.HumanResources.Department` vers le fichier de données `Department-c-t.txt` au format de caractères, utilisant la virgule comme indicateur de fin de champ et le saut de ligne (\n) comme indicateur de fin de ligne.  
  
 La commande **bcp** contient les commutateurs suivants.  
  
|Commutateur|Description|  
|------------|-----------------|  
|**-c**|Chargement des champs de données en tant que données sous forme de caractères.|  
|**-t** `,`|Virgule (,) servant d'indicateur de fin de champ.|  
|**-r** \n|Indicateur de fin de ligne en tant que caractère de saut de ligne. Il s'agit de l'indicateur par défaut ; le préciser est donc facultatif.|  
|**-T**|Spécifie que l'utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec une connexion approuvée qui utilise la sécurité intégrée. Si **-T** n’est pas spécifié, vous devez indiquer **-U** et **-P** pour vous connecter.|  
  
 Pour plus d’informations, consultez [bcp Utility](../../tools/bcp-utility.md).  
  
 À l'invite de commandes Windows [!INCLUDE[msCoName](../../includes/msconame-md.md)] , entrez :  
  
```cmd
bcp AdventureWorks.HumanResources.Department out C:\myDepartment-c-t.txt -c -t, -r \n -T  
```  
  
 Ceci entraîne la création de `Department-c-t.txt`contenant 16 enregistrements de 4 champs chacun. Les champs sont séparés par une virgule.  
  
## <a name="specifying-terminators-for-bulk-import"></a>Spécification des terminateurs pour l'importation en bloc  
 Quand vous procédez à une importation en bloc de données de type **char** ou **nchar** , la commande prévue à cet effet doit reconnaître les indicateurs de fin utilisés dans le fichier de données. Voici comment les indicateurs de fin doivent être précisés selon la commande d'importation en bloc :  
  
-   **bcp**  
  
     Spécifier des terminateurs pour une opération d'importation revient à utiliser la même syntaxe que pour une opération d'exportation. Pour plus d'informations, consultez la section « Spécification des terminateurs pour l'exportation en bloc », plus haut dans cette rubrique.  
  
-   BULK INSERT  
  
     Les terminateurs peuvent être spécifiés pour chaque champ d'un fichier de format ou pour le fichier de données tout entier grâce aux qualificateurs répertoriés dans le tableau suivant.  
  
    |Qualificateur|Description|  
    |---------------|-----------------|  
    |FIELDTERMINATOR **='***field_terminator***'**|Spécifie l'indicateur de fin de champ à utiliser pour les fichiers de données de type caractère et de type caractère Unicode.<br /><br /> Par défaut, c'est le caractère de tabulation (\t).|  
    |ROWTERMINATOR **='***row_terminator***'**|Spécifie l'indicateur de fin de ligne à utiliser pour les fichiers de données de type caractère et de type caractère Unicode.<br /><br /> Par défaut, il s'agit du caractère de saut de ligne (\n).|  
  
     Pour plus d’informations, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
-   INSERT ... SELECT * FROM OPENROWSET(BULK...)  
  
     Dans le cas du fournisseur de l'ensemble de lignes en bloc OPENROWSET, les indicateurs de fin ne peuvent être précisés que dans le fichier de format (requis sauf dans le cas de types de données incluses dans un objet volumineux). Si un fichier de données de type caractère utilise un indicateur de fin qui ne soit pas un indicateur par défaut, ce dernier doit alors être défini dans le fichier de format. Pour plus d’informations, consultez [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md) et [Utiliser un fichier de format pour importer des données en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  
  
     Pour plus d’informations sur la clause OPENROWSET BULK, consultez [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
### <a name="examples"></a>Exemples  
 Les exemples de cette section importent en bloc des données de type caractère du fichier de données `Department-c-t.txt` créé dans l'exemple précédent dans la table `myDepartment` se trouvant dans l'exemple de base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Avant de commencer, vous devez créer cette base de données. Pour créer cette table, sous le schéma **dbo** , dans l'Éditeur de requêtes [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , exécutez le code suivant :  
  
```sql  
USE AdventureWorks;  
GO  
DROP TABLE myDepartment;  
CREATE TABLE myDepartment   
(DepartmentID smallint,  
Name nvarchar(50),  
GroupName nvarchar(50) NULL,  
ModifiedDate datetime not NULL CONSTRAINT DF_AddressType_ModifiedDate DEFAULT (GETDATE())  
);  
GO 
```  
  
#### <a name="a-using-bcp-to-interactively-specify-terminators"></a>A. Utilisation de bcp pour spécifier de façon interactive les terminateurs  
 L'exemple suivant importe en bloc le fichier de données `Department-c-t.txt` par le biais de la commande `bcp` . Les commutateurs de cette commande sont identiques à ceux de la commande d'exportation en bloc. Pour plus d'informations, consultez la section « Spécification des terminateurs pour l'exportation en bloc », plus haut dans cette rubrique.  
  
 À l'invite de commandes Windows, entrez :  
  
```cmd
bcp AdventureWorks..myDepartment in C:\myDepartment-c-t.txt -c -t , -r \n -T  
```  
  
#### <a name="b-using-bulk-insert-to-interactively-specify-terminators"></a>B. Utilisation de BULK INSERT pour spécifier de façon interactive les terminateurs  
 L'exemple suivant importe en bloc le fichier de données `Department-c-t.txt` par le biais de l'instruction `BULK INSERT` en utilisant les qualificateurs répertoriés dans le tableau suivant.  
  
|Option|Attribute|  
|------------|---------------|  
|DATAFILETYPE **='** char **'**|Chargement des champs de données en tant que données sous forme de caractères.|  
|FIELDTERMINATOR **='**`,`**'**|Virgule (`,`) servant d'indicateur de fin de champ.|  
|ROWTERMINATOR **='**`\n`**'**|Indicateur de fin de ligne en tant que caractère de saut de ligne.|  
  
 Dans l'Éditeur de requête [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , exécutez le code suivant :  
  
```sql  
USE AdventureWorks;  
GO  
BULK INSERT myDepartment FROM 'C:\myDepartment-c-t.txt'  
   WITH (  
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ',',  
      ROWTERMINATOR = '\n'  
);  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Spécifier la longueur des champs au moyen de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)   
 [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
  
