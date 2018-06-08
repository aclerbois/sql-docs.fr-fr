---
title: Programmation ADO Visual C++ | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO]
ms.assetid: 11233b96-e05c-4221-9aed-5f20944b0f1c
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ce5a300ec2dd17109f888c9023b934c686289504
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="visual-c-ado-programming"></a>Programmation ADO Visual C++
La référence de l’API ADO décrit les fonctionnalités de l’ADO application programming interface (API) à l’aide d’une syntaxe semblable à Microsoft Visual Basic. Bien que le public visé est tous les utilisateurs, les programmeurs ADO utilisent divers langages comme Visual Basic, Visual C++ (avec et sans le **#import** directive) et Visual J ++ (avec le package de classes ADO/WFC).  

> [!NOTE]
> Microsoft s’est terminée la prise en charge de Visual J ++ en 2004.

 Pour répondre à cette diversité, les [ADO pour les index de la syntaxe Visual C++](../../../ado/guide/appendixes/using-ado-with-microsoft-visual-c.md) fournissent la syntaxe spécifique au langage de Visual C++ avec des liens vers des descriptions communes de fonctionnalités, paramètres, comportements exceptionnels et ainsi de suite, dans l’API Référence.  
  
 ADO est implémenté avec les interfaces COM (Component Object Model). Toutefois, il est plus facile pour les programmeurs de travailler avec COM dans certains langages de programmation que d’autres. Par exemple, presque tous les détails de l’utilisation de COM sont gérés implicitement pour les programmeurs Visual Basic, tandis que les programmeurs Visual C++ doivent gérer eux-mêmes ces détails.  
  
 Les sections suivantes résument les détails pour les programmeurs de C et C++ à l’aide d’ADO et le **#import** directive. Il se concentre sur les types de données spécifiques à COM (**Variant**, **BSTR**, et **SafeArray**) et la gestion des erreurs (_com_error).  
  
## <a name="using-the-import-compiler-directive"></a>À l’aide de la Directive de compilateur #import  
 Le **#import** directive du compilateur Visual C++ simplifie l’utilisation avec les méthodes et propriétés ADO. La directive prend le nom d’un fichier contenant une bibliothèque de types, tels que la DLL ADO (Msado15.dll) et génère des fichiers d’en-tête contenant des déclarations typedef, des pointeurs intelligents pour les interfaces et les constantes énumérées. Chaque interface est encapsulé ou encapsulée dans une classe.  
  
 Pour chaque opération dans une classe (autrement dit, un appel de méthode ou propriété), il existe une déclaration d’appeler directement l’opération (autrement dit, la forme « brute » de l’opération) et une déclaration qui appelle l’opération brute et lève une exception COM si l’opération échoue à exécuter succ essfully. Si l’opération est une propriété, il existe généralement une directive de compilateur qui crée une autre syntaxe pour l’opération qui a une syntaxe similaire Visual Basic.  
  
 Opérations de récupération de la valeur d’une propriété ont des noms de l’écran, **obtenir *** propriété*. Les opérations qui définissent la valeur d’une propriété ont un nom de l’écran, **Put *** propriété*. Les opérations qui définissent la valeur d’une propriété avec un pointeur vers un objet ADO ont des noms de l’écran, **PutRef *** propriété*.  
  
 Vous pouvez obtenir ou définir une propriété avec des appels des formes suivantes :  
  
```  
variable = objectPtr->GetProperty(); // get property value   
objectPtr->PutProperty(value);       // set property value  
objectPtr->PutRefProperty(&value);   // set property with object pointer  
```  
  
## <a name="using-property-directives"></a>À l’aide de Directives de propriété  
 Le **__declspec(property...)**  directive de compilateur est une extension du langage C spécifique à Microsoft qui déclare une fonction utilisée comme propriété avec une syntaxe alternative. Par conséquent, vous pouvez définir ou obtenir les valeurs d’une propriété d’une manière similaire à Visual Basic. Par exemple, vous pouvez définir et obtenir une propriété de cette façon :  
  
```  
objectPtr->property = value;        // set property value  
variable = objectPtr->property;     // get property value  
```  
  
 Notez que vous n’avez pas au code :  
  
```  
objectPtr->PutProperty(value);      // set property value  
variable = objectPtr->GetProperty;  // get property value  
```  
  
 Le compilateur générera approprié **Get ***-*, **Put**-, ou **PutRef *** propriété* appel en fonction de la syntaxe alternative est déclaré et que la propriété lus ou écrits.  
  
 Le **__declspec(property...)**  directive de compilateur peut déclarer uniquement **obtenir**, **put**, ou **obtenir** et **put** autre syntaxe pour une fonction. Les opérations en lecture seule ne peuvent avoir un **obtenir** déclaration ; les opérations en écriture seule ont uniquement un **put** déclaration ; les opérations qui sont à la fois en lecture et en écriture posséder à la fois **obtenir** et **put** déclarations.  
  
 Seules deux déclarations sont possibles avec cette directive ; Toutefois, chaque propriété peut avoir trois fonctions de propriété : **obtenir *** propriété*, **Put *** propriété*, et **PutRef *** propriété*. Dans ce cas, seuls deux formes de la propriété possèdent une syntaxe alternative.  
  
 Par exemple, le **commande** objet **ActiveConnection** propriété est déclarée avec une syntaxe alternative pour **obtenir *** ActiveConnection* et **PutRef * ** ActiveConnection*. Le **PutRef**-syntaxe est un bon choix, car dans la pratique, vous pouvez placer open **connexion** objet (autrement dit, un **connexion** pointeur d’objet) dans cette propriété. En revanche, le **Recordset** objet a **obtenir**-, **Put**-, et **PutRef *** ActiveConnection* opérations, mais aucune autre syntaxe.  
  
## <a name="collections-the-getitem-method-and-the-item-property"></a>Collections, méthode GetItem et la propriété d’élément  
 ADO définit plusieurs collections, notamment **champs**, **paramètres**, **propriétés**, et **erreurs**. Dans Visual C++, le **GetItem (***index***)** méthode retourne un membre de la collection. *Index* est un **Variant**, dont la valeur est soit un index numérique du membre dans la collection, soit une chaîne contenant le nom du membre.  
  
 Le **__declspec(property...)**  directive de compilateur déclare le **élément** une autre syntaxe pour chaque collection de la propriété de fondamentale **élémentaire GetItem()** (méthode). La syntaxe alternative utilise des crochets et ressemble à une référence de tableau. En règle générale, les deux formes se présenter comme suit :  
  
```  
  
      collectionPtr->GetItem(index);  
collectionPtr->Item[index];  
```  
  
 Par exemple, affecter une valeur à un champ d’un **Recordset** objet, appelé ***rs***, dérivée de la **auteurs** table de la **pubs** base de données. Utilisez le **Item()** à accéder à la troisième propriété **champ** de la **Recordset** objet **champs** (collections sont indexées à partir de collection égal à zéro ; Supposons que le troisième champ se nomme ***au_fname***). Appelez ensuite la **Value()** méthode sur le **champ** objet pour affecter une valeur de chaîne.  
  
 Cela peut être exprimé en Visual Basic dans les éléments suivants quatre façons (les deux derniers sont propres à Visual Basic ; les autres langages n’ont pas d’équivalents) :  
  
```  
rs.Fields.Item(2).Value = "value"  
rs.Fields.Item("au_fname").Value = "value"  
rs(2) = "value"  
rs!au_fname = "value"  
```  
  
 L’équivalent dans Visual C++ pour les deux premières formes ci-dessus est la suivante :  
  
```  
rs->Fields->GetItem(long(2))->PutValue("value");   
rs->Fields->GetItem("au_fname")->PutValue("value");  
```  
  
 - ou - (la syntaxe alternative de la **valeur** propriété est également indiquée)  
  
```  
rs->Fields->Item[long(2)]->Value = "value";  
rs->Fields->Item["au_fname"]->Value = "value";  
```  
  
 Pour obtenir des exemples d’effectuer une itération au sein d’une collection, consultez la section « Collections ADO » de « Référence ADO ».  
  
## <a name="com-specific-data-types"></a>Types de données spécifiques à COM  
 En règle générale, n’importe quel type de données Visual Basic que vous trouver dans la référence de l’API ADO a un équivalent Visual C++. Ceux-ci incluent des types de données standard, tels que **unsigned char** pour un Visual Basic **octets**, **court** pour **entier**, et  **long** pour **Long**. Rechercher dans la syntaxe Indexesto voir exactement ce qui est obligatoire pour les opérandes d’une méthode ou une propriété.  
  
 Les exceptions à cette règle sont les types de données spécifiques à COM : **Variant**, **BSTR**, et **SafeArray**.  
  
### <a name="variant"></a>Variant  
 A **Variant** est un type de données structurées qui contient un membre de valeur et un membre de type de données. A **Variant** peut contenir une grande variété d’autres types de données, y compris une autre variante, BSTR, Boolean, IDispatch ou IUnknown pointeur, devise, date et ainsi de suite. COM fournit également des méthodes qui facilitent la pour convertissent un type de données à un autre.  
  
 Le **_variant_t** classe encapsule et gère le **Variant** type de données.  
  
 Lorsque la référence de l’API ADO indique qu’une méthode ou l’opérande de la propriété prend une valeur, cela signifie généralement que la valeur est passée dans un **_variant_t**.  
  
 Cette règle est explicitement vrai quand le **paramètres** section dans les rubriques de la référence de l’API ADO indique qu’un opérande est un **variante**. Une exception est lorsque la documentation indique explicitement que l’opérande est un type de données standard, tel que **Long** ou **octets**, ou d’une énumération. Une autre exception est lorsque l’opérande prend un **chaîne**.  
  
### <a name="bstr"></a>BSTR  
 A **BSTR** (**B**asic **STR**effectue une opération) est un type de données structurées qui contient une chaîne de caractères et la longueur de la chaîne. COM fournit des méthodes permettant d’allouer, manipuler et libérer un **BSTR**.  
  
 Le **_bstr_t** classe encapsule et gère le **BSTR** type de données.  
  
 Lorsque la référence de l’API ADO indique une méthode ou propriété accepte un **chaîne** valeur, cela signifie que la valeur est sous la forme d’un **_bstr_t**.  
  
### <a name="casting-variantt-and-bstrt-classes"></a>Effectuer un cast des Classes _variant_t et _bstr_t  
 Fréquence à laquelle il n’est pas nécessaire de code explicitement un **_variant_t** ou **_bstr_t** dans un argument d’une opération. Si le **_variant_t** ou **_bstr_t** classe a un constructeur qui correspond au type de données de l’argument, le compilateur générera approprié **_variant_t** ou **_bstr_t**.  
  
 Toutefois, si l’argument est ambigu, autrement dit, le type de données argument correspond à plusieurs constructeurs, vous devez effectuer un cast de l’argument avec le type de données approprié pour appeler le constructeur approprié.  
  
 Par exemple, la déclaration de la **Recordset::Open** méthode est :  
  
```  
    HRESULT Open (  
        const _variant_t & Source,  
        const _variant_t & ActiveConnection,  
        enum CursorTypeEnum CursorType,  
        enum LockTypeEnum LockType,  
        long Options );  
```  
  
 Le `ActiveConnection` argument prend une référence à un **_variant_t**, que vous pouvez coder comme une chaîne de connexion ou un pointeur vers open **connexion** objet.  
  
 Le bon **_variant_t** sera construite implicitement si vous passez une chaîne telle que «`DSN=pubs;uid=MyUserName;pwd=MyPassword;`», ou un pointeur tel que «`(IDispatch *) pConn`».  
  
> [!NOTE]
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = yes** ou **Integrated Security = SSPI** au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.  
  
 Vous pouvez coder explicitement un **_variant_t** contenant un pointeur tel que «`_variant_t((IDispatch *) pConn, true)`». Le cast, `(IDispatch *)`, résout l’ambiguïté avec un autre constructeur qui prend un pointeur vers une interface IUnknown.  
  
 Il est essentiel, bien que rarement mentionné fait, que ADO est une interface IDispatch. Chaque fois qu’un pointeur vers un objet ADO doit être passé en tant qu’un **Variant**, ce pointeur doit être converti en un pointeur vers une interface IDispatch.  
  
 Le dernier cas codes explicitement le deuxième argument booléen du constructeur avec sa valeur par défaut facultative, `true`. Cet argument provoque le **Variant** constructeur à appeler son **AddRef**(méthode) (), ce qui compense ADO appeler automatiquement le **_variant_t::Release**() (méthode) Quand l’appel de méthode ou la propriété ADO se termine.  
  
### <a name="safearray"></a>SafeArray  
 A **SafeArray** est un type de données structurées qui contient un tableau d’autres types de données. A **SafeArray** est appelé *safe* , car il contient des informations sur les limites de chaque dimension du tableau et limite l’accès aux éléments du tableau dans ces limites.  
  
 Lorsque la référence de l’API ADO indique qu’une méthode ou propriété prend ou retourne un tableau, cela signifie que la méthode ou propriété prend ou retourne un **SafeArray**, pas un tableau C/C++ natif.  
  
 Par exemple, le deuxième paramètre de la **connexion** objet **OpenSchema** méthode requiert un tableau de **Variant** valeurs. Ceux **Variant** valeurs doivent être passées en tant qu’éléments d’un **SafeArray**et qui **SafeArray** doit être défini comme la valeur d’un autre **Variant** . Il s’agit qu’autres **Variant** qui est passé comme deuxième argument de **OpenSchema**.  
  
 Exemples plus précisément, le premier argument de la **trouver** méthode est un **Variant** dont la valeur est une dimension **SafeArray**; chaque premier et deuxième arguments facultatifs de **AddNew** est une dimension **SafeArray**; et la valeur de retour de la **GetRows** méthode est un **Variant** dont valeur est une à deux dimensions **SafeArray**.  
  
## <a name="missing-and-default-parameters"></a>Manquants et par défaut des paramètres  
 Visual Basic autorise les paramètres dans les méthodes manquants. Par exemple, le **Recordset** objet **ouvrir** méthode ayant cinq paramètres, mais vous pouvez ignorer les paramètres intermédiaires et omettre les paramètres de fin. Une valeur par défaut **BSTR** ou **Variant** sera remplacée selon le type de données de l’opérande manquant.  
  
 En C/C++, vous doivent spécifier tous les opérandes. Si vous souhaitez spécifier un paramètre manquant dont le type de données est une chaîne, spécifiez un **_bstr_t** contenant une chaîne null. Si vous souhaitez spécifier un paramètre manquant dont le type de données est un **Variant**, spécifiez un **_variant_t** avec la valeur DISP_E_PARAMNOTFOUND et le type VT_ERROR. Vous pouvez également spécifier l’équivalent **_variant_t** constant, **vtMissing**, qui est fournie par le **#import** directive.  
  
 Trois méthodes sont des exceptions à l’utilisation classique de **vtMissing**. Il s’agit le **Execute** méthodes de la **connexion** et **commande** objets et le **NextRecordset** méthode de la **Recordset** objet. Leurs signatures sont les suivants :  
  
```  
_RecordsetPtr <A HREF="mdmthcnnexecute.htm">Execute</A>( _bstr_t CommandText, VARIANT * RecordsAffected,   
        long Options );  // Connection  
_RecordsetPtr <A HREF="mdmthcmdexecute.htm">Execute</A>( VARIANT * RecordsAffected, VARIANT * Parameters,   
        long Options );  // Command  
_RecordsetPtr <A HREF="mdmthnextrec.htm">NextRecordset</A>( VARIANT * RecordsAffected );  // Recordset  
```  
  
 Les paramètres, *RecordsAffected* et *paramètres*, sont des pointeurs vers un **variante**. *Paramètres* est un paramètre d’entrée qui spécifie l’adresse d’un **Variant** contenant un seul paramètre, ou tableau de paramètres, qui modifiera la commande en cours d’exécution. *RecordsAffected* est un paramètre de sortie qui spécifie l’adresse d’un **Variant**, où le nombre de lignes affectées par la méthode est retourné.  
  
 Dans le **commande** objet **Execute** (méthode), indiquer qu’aucun paramètre n’est spécifié en définissant *paramètres* soit `&vtMissing` (recommandé) ou le pointeur null (c'est-à-dire, **NULL** ou zéro (0)). Si *paramètres* est défini pour le pointeur null, la méthode remplace en interne l’équivalent de **vtMissing**, puis termine l’opération.  
  
 Dans toutes les méthodes, indiquent que le nombre d’enregistrements affectés ne doit pas être retourné en définissant *RecordsAffected* au pointeur null. Dans ce cas, le pointeur null n’est pas bien un paramètre manquant comme une indication que la méthode doit ignorer le nombre d’enregistrements concernés.  
  
 Par conséquent, pour ces trois méthodes, il est valide pour un élément de code tels que :  
  
```  
pConnection->Execute("commandText", NULL, adCmdText);   
pCommand->Execute(NULL, NULL, adCmdText);  
pRecordset->NextRecordset(NULL);  
```  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Dans COM, la plupart des opérations renvoient un code de retour HRESULT qui indique si une fonction a réussi. Le **#import** génère du code wrapper autour de chaque méthode « brut » ou la propriété de la directive et vérifie le HRESULT retourné. Si la valeur HRESULT indique un échec, le code wrapper génère une erreur COM en appelant _com_issue_errorex() avec le code de retour HRESULT en tant qu’argument. Objets d’erreur COM peuvent être interceptées dans un **essayez**-**catch** bloc. (Par souci d’efficacité, interceptez une référence à un **_com_error** objet.)  
  
 N’oubliez pas, il s’agit d’erreurs ADO : elles résultent de l’échec d’une opération ADO. Les erreurs renvoyées par le fournisseur sous-jacent apparaissent en tant que **erreur** des objets dans le **connexion** objet **erreurs** collection.  
  
 Le **#import** directive crée les erreurs que les routines de gestion pour les méthodes et propriétés déclarées dans le fichier .dll ADO. Toutefois, vous pouvez tirer parti de cette erreur même mécanisme de gestion en écrivant votre propre fonction inline ou macro de vérification des erreurs. Consultez la rubrique [Extensions Visual C++](../../../ado/guide/appendixes/visual-c-extensions-for-ado.md), ou le code dans les sections suivantes pour obtenir des exemples.  
  
## <a name="visual-c-equivalents-of-visual-basic-conventions"></a>Équivalents Visual C++ des Conventions Visual Basic  
 Voici un résumé de plusieurs conventions dans la documentation ADO, codées en Visual Basic, ainsi que leurs équivalents dans Visual C++.  
  
### <a name="declaring-an-ado-object"></a>Déclaration d’un objet ADO  
 En Visual Basic, une variable d’objet ADO (dans ce cas pour un **Recordset** objet) est déclarée comme suit :  
  
```  
Dim rst As ADODB.Recordset  
```  
  
 La clause «`ADODB.Recordset`», est le ProgID de le **Recordset** tel que défini dans le Registre de l’objet. Une nouvelle instance d’un **enregistrement** objet est déclaré comme suit :  
  
```  
Dim rst As New ADODB.Recordset  
```  
  
 -ou-  
  
```  
Dim rst As ADODB.Recordset  
Set rst = New ADODB.Recordset  
```  
  
 Dans Visual C++, le **#import** directive génère des déclarations de type pointeur intelligent pour tous les objets ADO. Par exemple, une variable qui pointe vers un **_Recordset** objet est de type **_RecordsetPtr**et est déclarée comme suit :  
  
```  
_RecordsetPtr  rs;  
```  
  
 Une variable qui pointe vers une nouvelle instance d’un **_Recordset** objet est déclaré comme suit :  
  
```  
_RecordsetPtr  rs("ADODB.Recordset");  
```  
  
 -ou-  
  
```  
_RecordsetPtr  rs;  
rs.CreateInstance("ADODB.Recordset");  
```  
  
 -ou-  
  
```  
_RecordsetPtr  rs;  
rs.CreateInstance(__uuidof(_Recordset));  
```  
  
 Après le **CreateInstance** est appelée, la variable peut être utilisée comme suit :  
  
```  
rs->Open(...);  
```  
  
 Notez que dans certains cas, le «`.`« opérateur est utilisé comme si la variable était une instance d’une classe (`rs.CreateInstance`) et dans un autre cas, le «`->`» opérateur est utilisé comme si la variable était un pointeur vers une interface (`rs->Open`).  
  
 Une variable peut être utilisée de deux façons, car le «`->`» est surchargé pour permettre à une instance d’une classe se comporte comme un pointeur vers une interface. Un membre de classe privée de la variable d’instance contient un pointeur vers le **_Recordset** interface ; la «`->`« opérateur retourne ce pointeur ; et le pointeur retourné accède aux membres de le **_Recordset**  objet.  
  
### <a name="coding-a-missing-parameter--string"></a>Codage d’un paramètre manquant : chaîne  
 Lorsque vous devez coder un manquant **chaîne** opérande en Visual Basic, vous simplement l’omettre. Vous devez spécifier l’opérande dans Visual C++. Code une **_bstr_t** qui a une chaîne vide comme valeur.  
  
```  
_bstr_t strMissing(L"");  
```  
  
### <a name="coding-a-missing-parameter--variant"></a>Codage d’un paramètre manquant — Variant  
 Lorsque vous devez coder un manquant **Variant** opérande en Visual Basic, vous simplement l’omettre. Vous devez spécifier tous les opérandes dans Visual C++. Absence de code **Variant** paramètre avec un **_variant_t** définir la valeur spéciale DISP_E_PARAMNOTFOUND et le type VT_ERROR. Vous pouvez également spécifier **vtMissing**, qui est une constante prédéfinie équivalente fournie par le **#import** directive.  
  
```  
_variant_t  vtMissingYours(DISP_E_PARAMNOTFOUND, VT_ERROR);   
```  
  
 - ou -  
  
```  
...vtMissing...;  
```  
  
### <a name="declaring-a-variant"></a>Déclaration d’un Variant  
 En Visual Basic, un **Variant** est déclaré avec le **Dim** instruction comme suit :  
  
```  
Dim VariableName As Variant  
```  
  
 Dans Visual C++, déclarez une variable en tant que type **_variant_t**. Schéma quelques **_variant_t** déclarations sont indiquées ci-dessous.  
  
> [!NOTE]
>  Ces déclarations donnent simplement une idée approximative de ce que vous feriez le code dans votre propre programme. Pour plus d’informations, consultez les exemples ci-dessous et la documentation de Visual C++.  
  
```  
_variant_t  VariableName(value);  
_variant_t  VariableName((data type cast) value);  
_variant_t  VariableName(value, VT_DATATYPE);  
_variant_t  VariableName(interface * value, bool fAddRef = true);  
```  
  
### <a name="using-arrays-of-variants"></a>Utilisation des tableaux de variantes  
 En Visual Basic, les tableaux de **variantes** peuvent être codées avec les **Dim** instruction, ou vous pouvez utiliser la **tableau** de fonction, comme illustré dans l’exemple de code suivant :  
  
```  
Public Sub ArrayOfVariants  
Dim cn As ADODB.Connection  
Dim rs As ADODB.Recordset  
Dim fld As ADODB.Field  
  
    cn.Open "DSN=pubs"  
    rs = cn.OpenSchema(adSchemaColumns, _  
        Array(Empty, Empty, "authors", Empty))  
    For Each fld in rs.Fields  
        Debug.Print "Name = "; fld.Name  
    Next fld  
    rs.Close  
    cn.Close  
End Sub  
```  
  
 L’exemple Visual C++ suivant montre comment utiliser un **SafeArray** utilisé avec un **_variant_t**.  
  
#### <a name="notes"></a>Remarques  
 Les remarques suivantes correspondent aux sections commentées dans l’exemple de code.  
  
1.  Une fois encore, la fonction inline TESTHR() est définie pour tirer parti du mécanisme de gestion des erreurs existant.  
  
2.  Vous devez uniquement un tableau unidimensionnel, afin de pouvoir utiliser **SafeArrayCreateVector**, au lieu de l’objectif général **SAFEARRAYBOUND** déclaration et **SafeArrayCreate** fonction. Voici ce code l’aspect à l’aide de **SafeArrayCreate**:  
  
    ```  
       SAFEARRAYBOUND   sabound[1];  
       sabound[0].lLbound = 0;  
       sabound[0].cElements = 4;  
       pSa = SafeArrayCreate(VT_VARIANT, 1, sabound);  
    ```  
  
3.  Le schéma identifié par la constante énumérée, **adSchemaColumns**, est associé à quatre colonnes de contrainte : TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME et COLUMN_NAME. Par conséquent, un tableau de **Variant** valeurs à quatre éléments est créé. Ensuite, une valeur de contrainte correspondant à la troisième colonne, TABLE_NAME, est spécifiée.  
  
     Le **Recordset** qui est retourné se compose de plusieurs colonnes, un sous-ensemble des colonnes de contrainte. Les valeurs des colonnes de contrainte pour chaque ligne retournée doivent être le même que les valeurs de contrainte correspondante.  
  
4.  Ceux qui connaissent **SafeArrays** peut-être surpris que **SafeArrayDestroy**() n’est pas appelé avant la sortie. En fait, l’appel **SafeArrayDestroy**() entraîne dans ce cas une exception d’exécution. La raison est que le destructeur de `vtCriteria` appellera **VariantClear**() lors de la **_variant_t** est hors de portée, ce qui permettra de libérer le **SafeArray**. Appel de **SafeArrayDestroy**, sans effacer manuellement les **_variant_t**, provoquerait le destructeur tenter d’effacer un non valide **SafeArray** pointeur.  
  
     Si **SafeArrayDestroy** a été appelée, le code ressemble à ceci :  
  
    ```  
          TESTHR(SafeArrayDestroy(pSa));  
       vtCriteria.vt = VT_EMPTY;  
          vtCriteria.parray = NULL;  
    ```  
  
     Toutefois, il est beaucoup plus simple pour vous permettre des **_variant_t** gérer le **SafeArray**.  
  
```  
// Visual_CPP_ADO_Prog_1.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Note 1  
inline void TESTHR( HRESULT _hr ) {   
   if FAILED(_hr)   
      _com_issue_error(_hr);   
}  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _RecordsetPtr pRs("ADODB.Recordset");  
      _ConnectionPtr pCn("ADODB.Connection");  
      _variant_t vtTableName("authors"), vtCriteria;  
      long ix[1];  
      SAFEARRAY *pSa = NULL;  
  
      pCn->Provider = "sqloledb";  
      pCn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
      // Note 2, Note 3  
      pSa = SafeArrayCreateVector(VT_VARIANT, 1, 4);  
      if (!pSa)   
         _com_issue_error(E_OUTOFMEMORY);  
  
      // Specify TABLE_NAME in the third array element (index of 2).   
      ix[0] = 2;        
      TESTHR(SafeArrayPutElement(pSa, ix, &vtTableName));  
  
      // There is no Variant constructor for a SafeArray, so manually set the   
      // type (SafeArray of Variant) and value (pointer to a SafeArray).  
  
      vtCriteria.vt = VT_ARRAY | VT_VARIANT;  
      vtCriteria.parray = pSa;  
  
      pRs = pCn->OpenSchema(adSchemaColumns, vtCriteria, vtMissing);  
  
      long limit = pRs->GetFields()->Count;  
      for ( long x = 0 ; x < limit ; x++ )  
         printf( "%d: %s\n", x + 1, ((char*) pRs->GetFields()->Item[x]->Name) );  
      // Note 4  
      pRs->Close();  
      pCn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Error:\n");  
      printf("Code = %08lx\n", e.Error());  
      printf("Code meaning = %s\n", (char*) e.ErrorMessage());  
      printf("Source = %s\n", (char*) e.Source());  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   CoUninitialize();  
}  
```  
  
### <a name="using-property-getputputref"></a>À l’aide de la propriété Get/Put/PutRef  
 En Visual Basic, le nom d’une propriété n’est pas qualifié par si elle est récupérée, assignée ou une référence.  
  
```  
Public Sub GetPutPutRef  
Dim rs As New ADODB.Recordset  
Dim cn As New ADODB.Connection  
Dim sz as Integer  
cn.Open "Provider=sqloledb;Data Source=yourserver;" & _  
         "Initial Catalog=pubs;Integrated Security=SSPI;"  
rs.PageSize = 10  
sz = rs.PageSize  
rs.ActiveConnection = cn  
rs.Open "authors",,adOpenStatic  
' ...  
rs.Close  
cn.Close  
End Sub  
```  
  
 Cet exemple Visual C++ illustre les **obtenir**/**Put**/**PutRef *** propriété*.  
  
#### <a name="notes"></a>Remarques  
 Les remarques suivantes correspondent aux sections commentées dans l’exemple de code.  
  
1.  Cet exemple utilise deux formes d’un argument de chaîne manquante : une constante explicite, **strMissing**et une chaîne que le compilateur utilisera pour créer un fichier temporaire **_bstr_t** qui existera pour l’étendue de la  **Ouvrez** (méthode).  
  
2.  Il n’est pas nécessaire d’effectuer un cast de l’opérande de `rs->PutRefActiveConnection(cn)` à `(IDispatch *)` , car le type de l’opérande est déjà `(IDispatch *)`.  
  
```  
// Visual_CPP_ado_prog_2.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _bstr_t strMissing(L"");  
      long oldPgSz = 0, newPgSz = 5;  
  
      // Note 1  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", strMissing, "", adConnectUnspecified);  
  
      oldPgSz = rs->GetPageSize();  
      // -or-  
      // oldPgSz = rs->PageSize;  
  
      rs->PutPageSize(newPgSz);  
      // -or-  
      // rs->PageSize = newPgSz;  
  
      // Note 2  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockReadOnly, adCmdTable);  
      printf("Original pagesize = %d, new pagesize = %d\n", oldPgSz, rs->GetPageSize());  
      rs->Close();  
      cn->Close();  
  
   }  
   catch (_com_error &e) {  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```  
  
### <a name="using-getitemx-and-itemx"></a>À l’aide de GetItem et l’élément [x]  
 Cet exemple Visual Basic illustre la syntaxe standard et alternative de **élément**().  
  
```  
Public Sub GetItemItem  
Dim rs As New ADODB.Recordset  
Dim name as String  
rs = rs.Open "authors", "DSN=pubs;", adOpenDynamic, _  
         adLockBatchOptimistic, adTable  
name = rs(0)  
' -or-  
name = rs.Fields.Item(0)  
rs(0) = "Test"  
rs.UpdateBatch  
' Restore name  
rs(0) = name  
rs.UpdateBatch  
rs.Close  
End Sub  
```  
  
 Cet exemple Visual C++ illustre **élément**.  
  
> [!NOTE]
>  La remarque suivante correspond aux sections commentées dans l’exemple de code : lorsque la collection est accessible avec **élément**, l’index, **2**, doivent être converties en **long** donc un constructeur approprié est appelé.  
  
```  
// Visual_CPP_ado_prog_3.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
void main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _variant_t vtFirstName;  
  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", "", "", adConnectUnspecified);  
  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockOptimistic, adCmdTable);  
      rs->MoveFirst();  
  
      // Note 1. Get a field.  
      vtFirstName = rs->Fields->GetItem((long)2)->GetValue();  
      // -or-  
      vtFirstName = rs->Fields->Item[(long)2]->Value;  
  
      printf( "First name = '%s'\n", (char*)( (_bstr_t)vtFirstName) );  
  
      rs->Fields->GetItem((long)2)->Value = L"TEST";  
      rs->Update(vtMissing, vtMissing);  
  
      // Restore name  
      rs->Fields->GetItem((long)2)->PutValue(vtFirstName);  
      // -or-  
      rs->Fields->GetItem((long)2)->Value = vtFirstName;  
      rs->Update(vtMissing, vtMissing);  
      rs->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```  
  
### <a name="casting-ado-object-pointers-with-idispatch-"></a>Effectuer un cast des pointeurs d’objet ADO avec (IDispatch *)  
 L’exemple Visual C++ suivant illustre l’utilisation (IDispatch *) pour les pointeurs d’objet ADO cast.  
  
#### <a name="notes"></a>Remarques  
 Les remarques suivantes correspondent aux sections commentées dans l’exemple de code.  
  
1.  Spécifiez open **connexion** objet explicitement codé **variante**. Effectuer un cast avec (IDispatch \*) afin du constructeur approprié est appelé. Également, à définir explicitement la seconde **_variant_t** paramètre à la valeur par défaut de **true**, de sorte que le nombre de références d’objet soit correct lorsque la **Recordset::Open** opération se termine.  
  
2.  L’expression, `(_bstr_t)`, n’est pas un cast, mais un **_variant_t** opérateur extrait un **_bstr_t** chaîne à partir de la **Variant** retourné par **valeur** .  
  
 L’expression, `(char*)`, n’est pas un cast, mais un **_bstr_t** opérateur qui extrait un pointeur vers la chaîne encapsulée dans un **_bstr_t** objet.  
  
 Cette section de code illustre certains comportements utiles de **_variant_t** et **_bstr_t** opérateurs.  
  
```  
// Visual_CPP_ado_prog_4.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr pConn("ADODB.Connection");  
      _RecordsetPtr pRst("ADODB.Recordset");  
  
      pConn->Provider = "sqloledb";  
      pConn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
  
      // Note 1.  
      pRst->Open("authors", _variant_t((IDispatch *) pConn, true), adOpenStatic, adLockReadOnly, adCmdTable);  
      pRst->MoveLast();  
  
      // Note 2.  
      printf("Last name is '%s %s'\n",   
         (char*) ((_bstr_t) pRst->GetFields()->GetItem("au_fname")->GetValue()),  
         (char*) ((_bstr_t) pRst->Fields->Item["au_lname"]->Value));  
  
      pRst->Close();  
      pConn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }     
   ::CoUninitialize();  
}  
```
