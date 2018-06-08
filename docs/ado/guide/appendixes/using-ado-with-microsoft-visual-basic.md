---
title: À l’aide d’ADO avec Microsoft Visual Basic | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, Visual Basic
- Visual Basic [ADO]
ms.assetid: 9dfb6784-037d-4f9d-bb7f-b506b4498573
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 842671f5859fc0522c30aec8a0d363d3f101473c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-ado-with-microsoft-visual-basic-and-visual-basic-for-applications"></a>À l’aide d’ADO avec Microsoft Visual Basic et Visual Basic pour Applications
Configuration d’un projet ADO et l’écriture de code ADO sont semblable si vous utilisez Visual Basic ou Visual Basic pour Applications. Cette rubrique aborde l’utilisation d’ADO avec Visual Basic et Visual Basic pour Applications et indique les différences.

## <a name="referencing-the-ado-library"></a>Fait référence à la bibliothèque ADO
 La bibliothèque ADO doit être référencée par votre projet.

#### <a name="to-reference-ado-from-microsoft-visual-basic"></a>Pour la référence ADO à partir de Microsoft Visual Basic

1.  En Visual Basic, à partir de la **projet** menu, sélectionnez **références...** .

2.  Sélectionnez **bibliothèque Microsoft ActiveX Data Objects x.x** dans la liste. Vérifiez qu’au moins les bibliothèques suivantes sont également sélectionnés :

    -   Visual Basic pour Applications

    -   Les procédures et les objets d’exécution Visual Basic

    -   Objets et procédures Visual Basic

    -   OLE Automation

3.  Cliquez sur **OK**.

 Vous pouvez utiliser tout aussi facilement des ADO avec Visual Basic pour Applications, à l’aide de Microsoft Access, par exemple.

#### <a name="to-reference-ado-from-microsoft-access"></a>Pour la référence ADO à partir de Microsoft Access

1.  Dans Microsoft Access, sélectionnez ou créez un module à partir de la **Modules** onglet dans le **base de données** fenêtre.

2.  Sur le **outils** menu, sélectionnez **références...** .

3.  Sélectionnez **bibliothèque Microsoft ActiveX Data Objects x.x** dans la liste. Vérifiez qu’au moins les bibliothèques suivantes sont également sélectionnés :

    -   Visual Basic pour Applications

    -   Bibliothèque d’objets Microsoft Access 8.0 (ou version ultérieure)

    -   Bibliothèque d’objets Microsoft DAO 3.5 (ou version ultérieure)

4.  Cliquez sur **OK**.

## <a name="creating-ado-objects-in-visual-basic"></a>Création d’objets ADO dans Visual Basic
 Pour créer une variable automation et une instance d’un objet pour cette variable, vous pouvez utiliser deux méthodes : **Dim** ou **CreateObject**.

### <a name="dim"></a>Dim
 Vous pouvez utiliser la **nouveau** mot clé with **Dim** à déclarer et à créer des instances d’objets ADO en une seule étape :

```
Dim conn As New ADODB.Connection
```

 Vous pouvez également le **Dim** instanciation d’objet et de la déclaration d’instruction peut également être des deux étapes :

```
Dim conn As ADODB.Connection
Set conn = New ADODB.Connection
```

> [!NOTE]
>  Il n’est pas nécessaire d’utiliser explicitement le `ADODB` progid avec la **Dim** instruction, si vous avez référencé correctement de la bibliothèque ADO dans votre projet. Il garantit toutefois que vous n’avez pas d’affectation de noms est en conflit avec d’autres bibliothèques.

> [!NOTE]
>  Par exemple, si vous incluez des références aux objets ADO et DAO dans le même projet, vous devez inclure un qualificateur pour spécifier quel modèle d’objet à utiliser lors de l’instanciation **Recordset** objets, comme dans le code suivant :

```
Dim adoRS As ADODB.Recordset
Dim daoRS As DAO.Recordset
```

### <a name="createobject"></a>CreateObject
 Avec la **CreateObject** instanciation de méthode, la déclaration et l’objet doit être de deux étapes distinctes :

```
Dim conn1
Set conn1 = CreateObject("ADODB.Connection") As Object
```

 Les objets instanciés avec **CreateObject** sont à liaison tardive, ce qui signifie qu’ils ne sont pas fortement typées et de fin de ligne de commande est désactivée. Toutefois, il vous autorise à ne pas référencer la bibliothèque ADO à partir de votre projet et vous permet d’instancier des versions spécifiques d’objets. Par exemple :

```
Set conn1 = CreateObject("ADODB.Connection.2.0") As Object
```

 Peut également cela en définissant une référence à la bibliothèque de type 2.0 ADO version spécifique et la création de l’objet.

 L’instanciation d’objets à l’aide de la **CreateObject** méthode est généralement plus lente que l’utilisation de la **Dim** instruction.

## <a name="handling-events"></a>La gestion des événements
 Afin de gérer les événements ADO dans Microsoft Visual Basic, vous devez déclarer une variable au niveau du module à l’aide du **WithEvents** (mot clé). La variable peut être déclarée uniquement dans le cadre d’un module de classe et doit être déclarée au niveau du module. Pour une discussion plus approfondie de la gestion des événements ADO, consultez [gestion des événements ADO](../../../ado/guide/data/handling-ado-events.md).

## <a name="visual-basic-examples"></a>Exemples Visual Basic
 De nombreux exemples Visual Basic sont inclus dans la documentation ADO. Pour plus d’informations, consultez [exemples de Code ADO dans Microsoft Visual Basic](../../../ado/reference/ado-api/ado-code-examples-in-visual-basic.md).

## <a name="see-also"></a>Voir aussi
 [Microsoft ActiveX Data Objects (ADO)](../../../ado/microsoft-activex-data-objects-ado.md) [à l’aide d’ADO avec Microsoft Visual C++](../../../ado/guide/appendixes/using-ado-with-microsoft-visual-c.md) [à l’aide d’ADO avec des langages de script](../../../ado/guide/appendixes/using-ado-with-scripting-languages.md)
