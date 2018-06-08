---
title: Spécifications de Type défini par l’utilisateur | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- UDTs [CLR integration], requirements
- serialization
- Native serialization format [CLR integration]
- attributes [CLR integration]
- XML serialization [CLR integration]
- user-defined types [CLR integration], requirements
- user-defined serialization [CLR integration]
- user-defined types [CLR integration], Native serialization
- UDTs [CLR integration], Native serialization
ms.assetid: bedc3372-50eb-40f2-bcf2-d6db6a63b7e6
caps.latest.revision: 31
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 6d4bccc8ae83d25f848011421d057ad8cb0a7016
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-user-defined-types---requirements"></a>Création de Types définis par l’utilisateur - configuration requise
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous devez prendre plusieurs décisions de conception importantes lors de la création d’un type défini par l’utilisateur (UDT) doit être installé dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour la plupart des types définis par l'utilisateur, il est recommandé de créer un type défini par l'utilisateur sous forme de structure mais il est également possible de le créer sous forme de classe. La définition de l'UDT doit être conforme aux spécifications de création d'UDT afin de pouvoir l'enregistrer avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="requirements-for-implementing-udts"></a>Configuration requise pour l'implémentation des types définis par l'utilisateur  
 Pour s'exécuter dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], votre UDT doit implémenter les éléments requis suivants dans sa définition :  
  
 L’UDT doit spécifier le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute**. L’utilisation de la **System.SerializableAttribute** est facultative mais recommandée.  
  
-   L’UDT doit implémenter la **System.Data.SqlTypes.INullable** interface dans la classe ou structure en créant un public **statique** (**Shared** dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic) **Null** (méthode). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est par défaut compatible avec la valeur NULL. Ceci est nécessaire pour que le code exécuté dans l'UDT puisse être en mesure de reconnaître une valeur NULL.  
  
-   L’UDT doit contenir un public **statique** (ou **Shared**) **analyser** méthode qui prend en charge de l’analyse à partir d’et publique **ToString** procédé de conversion en une représentation sous forme de chaîne de l’objet.  
  
-   Un UDT avec un format de sérialisation définie par l’utilisateur doit implémenter la **System.Data.IBinarySerialize** interface et fournir un **en lecture** et un **écrire** (méthode).  
  
-   L’UDT doit implémenter **System.Xml.Serialization.IXmlSerializable**, ou tous les champs publics et les propriétés doivent être des types XML serializable ou muni de la **XmlIgnore** attribut si la substitution de la sérialisation standard est requise.  
  
-   Chaque objet UDT doit être soumis à une seule sérialisation. La validation échoue si les routines de sérialisation ou désérialisation reconnaissent plusieurs représentations d'un objet en particulier.  
  
-   **SqlUserDefinedTypeAttribute.IsByteOrdered** doit être **true** pour comparer des données dans l’ordre d’octet. Si l’interface IComparable n’est pas implémentée et **SqlUserDefinedTypeAttribute.IsByteOrdered** est **false**, les comparaisons d’ordre octets échouera.  
  
-   Un UDT défini dans une classe doit disposer d'un constructeur public qui n'accepte aucun argument. Vous pouvez éventuellement créer des constructeurs de classe surchargés supplémentaires.  
  
-   L'UDT doit dévoiler les éléments de données sous forme de champs publics ou de procédures de propriété.  
  
-   Noms publics ne peut pas dépasser 128 caractères et doit être conforme à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] règles de dénomination des identificateurs comme défini dans [des identificateurs de base de données](../../relational-databases/databases/database-identifiers.md).  
  
-   **sql_variant** colonnes ne peut pas contenir des instances d’un type UDT.  
  
-   Les membres hérités ne sont pas accessibles depuis [!INCLUDE[tsql](../../includes/tsql-md.md)] puisque le système de type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas compatible avec la hiérarchie d'héritage entre UDT. Toutefois, vous pouvez recourir à la fonction d'héritage au moment de structurer vos classes et pouvez appeler ces méthodes dans l'implémentation du code managé du type.  
  
-   Hormis le constructeur de classe, les membres ne peuvent pas être surchargés. Si vous créez une méthode surchargée, aucune erreur ne se produit au moment d'inscrire l'assembly ou de créer le type dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La détection de la méthode surchargée a lieu au moment de l'exécution et non lors de la création du type. Les méthodes surchargées peuvent exister dans la classe tant qu'elles ne sont jamais appelées. Une erreur est générée dès que vous appelez la méthode surchargée.  
  
-   N’importe quel **statique** (ou **Shared**) les membres doivent être déclarés en tant que constantes ou en lecture seule. Les membres statiques ne peuvent pas être mutables.  
  
-   Si le **SqlUserDefinedTypeAttribute.MaxByteSize** champ a la valeur -1, l’UDT sérialisé peut être aussi volumineux que la limite de taille LOB (large object) (actuellement 2 Go). La taille de l’UDT ne peut pas dépasser la valeur spécifiée dans le **MaxByteSized** champ.  
  
> [!NOTE]  
>  Bien qu’il n’est pas utilisé par le serveur pour effectuer des comparaisons, vous pouvez implémenter la **System.IComparable** interface, qui expose une méthode unique, **CompareTo**. Elle intervient côté client dans des situations où il est préférable de comparer ou de classer avec précision des valeurs de type défini par l'utilisateur (UDT).  
  
## <a name="native-serialization"></a>Sérialisation native  
 Le choix du bon attribut de sérialisation pour votre UDT dépend du type d'UDT que vous essayez de créer. Le **natif** format de sérialisation utilise une structure très simple qui permet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker une représentation native efficace de l’UDT sur le disque. Le **natif** format est recommandé si l’UDT est simple et contient uniquement des champs des types suivants :  
  
 **bool**, **byte**, **sbyte**, **short**, **ushort**, **int**, **uint**, **long**, **ulong**, **float**, **double**, **SqlByte**, **SqlInt16**, **SqlInt32**, **SqlInt64**, **SqlDateTime**, **SqlSingle**, **SqlDouble**, **SqlMoney**, **SqlBoolean**  
  
 Les types qui sont composés de champs des types ci-dessus sont de bons candidats à la valeur **natif** mettre en forme, telles que **structs** en Visual c# (ou **Structures** qu’elles sont connues en Visual Basic). Par exemple, un UDT spécifié avec le **natif** format de sérialisation peut contenir un champ d’un autre UDT également spécifié avec le **natif** format. Si la définition de l’UDT est plus complexe et contient des types de données pas dans la liste ci-dessus, vous devez spécifier le **UserDefined** à la place du format de sérialisation.  
  
 Le **natif** format exige les éléments suivants :  
  
-   Le type ne doit pas spécifier une valeur pour **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute.MaxByteSize**.  
  
-   Tous les champs doivent être sérialisables.  
  
-   Le **System.Runtime.InteropServices.StructLayoutAttribute qui a** doit être spécifié en tant que **StructLayout.LayoutKindSequential** si l’UDT est défini dans une classe et non dans une structure. Cet attribut contrôle la disposition physique des champs de données et est utilisé pour contraindre les membres à se placer dans l'ordre dans lequel ils apparaissent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise cet attribut pour déterminer l'ordre des champs des UDT au moyen de plusieurs valeurs.  
  
 Pour obtenir un exemple d’UDT défini avec **natif** sérialisation, consultez l’UDT Point dans [Coding User-Defined Types](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
## <a name="userdefined-serialization"></a>Sérialisation UserDefined  
 Le **UserDefined** paramètre pour mettre en forme le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** donne attribut le développeur complète de contrôler le format binaire. Lorsque vous spécifiez la **Format** de la propriété d’attribut **UserDefined**, vous devez procédez comme suit dans votre code :  
  
-   Spécifiez le paramètre facultatif **IsByteOrdered** propriété d’attribut. La valeur par défaut est **false**.  
  
-   Spécifiez le **MaxByteSize** propriété de la **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute**.  
  
-   Écrire du code pour implémenter **en lecture** et **écrire** méthodes pour que l’UDT en implémentant la **System.Data.Sql.IBinarySerialize** interface.  
  
 Pour obtenir un exemple d’UDT défini avec **UserDefined** sérialisation, consultez l’UDT Currency dans [Coding User-Defined Types](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
> [!NOTE]  
>  Les champs UDT doivent utiliser la sérialisation native ou être persistants pour pouvoir être indexés.  
  
## <a name="serialization-attributes"></a>Attributs de sérialisation  
 Les attributs déterminent la façon dont la sérialisation est utilisée pour construire la représentation de stockage des types définis par l'utilisateur et pour transmettre des types définis par l'utilisateur par valeur au client. Vous devez spécifier le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** lors de la création de l’UDT. Le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** attribut indique que la classe est un UDT et précise le stockage de l’UDT. Vous pouvez éventuellement spécifier le **Serializable** d’attribut, bien que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’a pas cette exigence.  
  
 Le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute** a les propriétés suivantes.  
  
 **Format**  
 Spécifie le format de sérialisation, qui peut être **natif** ou **UserDefined**, selon les types de données de l’UDT.  
  
 **IsByteOrdered**  
 A **booléenne** valeur qui détermine comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue des comparaisons binaires sur l’UDT.  
  
 **IsFixedLength**  
 Indique si toutes les instances de cet UDT ont la même longueur.  
  
 **MaxByteSize**  
 Taille maximale de l'instance, en octets. Vous devez spécifier **MaxByteSize** avec la **UserDefined** format de sérialisation. Pour un type UDT avec la sérialisation définie par l’utilisateur spécifié, **MaxByteSize** fait référence à la taille totale de l’UDT dans sa forme sérialisée comme défini par l’utilisateur. La valeur de **MaxByteSize** doit être dans la plage de 1 à 8 000, ou la valeur -1 pour indiquer que l’UDT est supérieure à 8 000 octets (la taille totale ne peut pas dépasser la taille maximale de LOB). Considérez un UDT doté d’une propriété d’une chaîne de 10 caractères (**System.Char**). Lorsque l'UDT est sérialisé à l'aide de BinaryWriter, la taille totale de la chaîne sérialisée est de 22 octets : 2 octets par caractère Unicode UTF-16, multipliés par le nombre maximal de caractères, plus 2 octets de contrôle de la charge mémoire générée par la sérialisation d'un flux binaire. Par conséquent, lors de la détermination de la valeur de **MaxByteSize**, la taille totale de l’UDT sérialisé doit être prises en compte : la taille des données sérialisées sous forme binaire, plus la charge induite par la sérialisation.  
  
 **ValidationMethodName**  
 Nom de la méthode utilisée pour valider des instances du type défini par l'utilisateur.  
  
### <a name="setting-isbyteordered"></a>Définition de la propriété IsByteOrdered  
 Lorsque le **Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute.IsByteOrdered** est définie sur **true**, vous êtes certain que les données binaires sérialisées peuvent être utilisées pour le classement sémantique des informations. Par conséquent, chaque instance d'un objet UDT ordonné par octet peut avoir une seule représentation sérialisée. Lorsque vous soumettez les octets sérialisés à une comparaison dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les résultats doivent être les mêmes que si vous réalisiez cette opération de comparaison dans le code managé. Les fonctionnalités suivantes sont également prises en charge quand **IsByteOrdered** a la valeur **true**:  
  
-   Possibilité de créer des index dans les colonnes de ce type.  
  
-   Possibilité de créer des clés primaires et étrangères ainsi que des contraintes CHECK et UNIQUE dans les colonnes de ce type.  
  
-   Possibilité d'utiliser les clauses ORDER BY, GROUP BY et PARTITION BY de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Dans ces cas, la représentation binaire du type est utilisée pour déterminer l'ordre.  
  
-   Possibilité d'utiliser des opérateurs de comparaison dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Possibilité de rendre persistantes des colonnes calculées de ce type.  
  
 Notez qu’à la fois le **natif** et **UserDefined** formats de sérialisation prend en charge les opérateurs de comparaison suivants lorsque **IsByteOrdered** a la valeur **true**:  
  
-   Égal à (=)  
  
-   Différent de (!=)  
  
-   Supérieur à (>)  
  
-   Inférieur à (\<)  
  
-   Supérieur ou égal à (>=)  
  
-   Inférieur ou égal à (<=)  
  
### <a name="implementing-nullability"></a>Implémentation de la possibilité de valeur NULL  
 En plus de spécifier correctement les attributs de vos assemblys, votre classe doit également prendre en charge la possibilité de valeur NULL. Les UDT chargés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont null prenant en charge, mais pour l’UDT puisse reconnaître une valeur null, la classe doit implémenter la **INullable** interface. Pour plus d’informations et obtenir un exemple montrant comment implémenter la possibilité de valeur NULL dans un UDT, consultez [Coding User-Defined Types](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
### <a name="string-conversions"></a>Conversions de chaînes  
 Pour prendre en charge la conversion de chaînes vers et depuis l’UDT, vous devez fournir un **analyser** (méthode) et un **ToString** méthode dans votre classe. Le **analyser** méthode autorise une chaîne à convertir en un type UDT. Il doit être déclaré en tant que **statique** (ou **Shared** en Visual Basic) et accepter un paramètre de type **System.Data.SqlTypes.SqlString**. Pour plus d’informations et obtenir un exemple montrant comment implémenter le **analyser** et **ToString** méthodes, consultez [Coding User-Defined Types](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
## <a name="xml-serialization"></a>Sérialisation XML  
 Les UDT doivent prendre en charge conversion vers et depuis le **xml** type de données par conforme au contrat de sérialisation XML. Le **System.Xml.Serialization** espace de noms contient des classes servant à sérialiser des objets en documents au format XML ou en flux. Vous pouvez choisir d’implémenter **xml** sérialisation à l’aide de la **IXmlSerializable** interface, qui fournit la mise en forme personnalisée pour la sérialisation XML et la désérialisation.  
  
 En plus d’effectuer des conversions explicites de l’UDT **xml**, la sérialisation XML vous permet de :  
  
-   Utilisez **Xquery** sur les valeurs d’instances d’UDT après conversion vers le **xml** type de données.  
  
-   Utiliser des UDT dans des requêtes paramétrables et des méthodes Web à l'aide des services Web XML natifs dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Utiliser des UDT pour recevoir un chargement en masse de données XML.  
  
-   Sérialiser des datasets contenant des tables avec des colonnes UDT.  
  
 Les UDT ne sont pas sérialisés dans les requêtes FOR XML. Pour exécuter une requête FOR XML qui affiche la sérialisation XML des UDT, convertissez explicitement chaque colonne UDT le **xml** type de données dans l’instruction SELECT. Vous pouvez également convertir explicitement les colonnes à **varbinary**, **varchar**, ou **nvarchar**.  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’un Type défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md)  
  
  
