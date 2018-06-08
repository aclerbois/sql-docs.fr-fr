---
title: Programmation ADO en VBScript | Documents Microsoft
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
- ADO, VBScript
- VBScript [ADO]
ms.assetid: 6aaaf6d0-1376-4473-bea6-b81f2645a9ac
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 495a13d938ef9ab9d82f6158a222505c4f748c92
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="vbscript-ado-programming"></a>Programmation ADO en VBScript
## <a name="creating-an-ado-project"></a>Création d’un projet ADO  
 Microsoft Visual Basic, Scripting Edition ne prend pas en charge les bibliothèques de types, vous n’avez pas besoin de référencer ADO dans votre projet. Par conséquent, aucune fonctionnalité associée comme fin de ligne de commande n’est pris en charge. En outre, par défaut, les constantes énumérées ADO pas sont définis en VBScript.  
  
 En revanche, ADO comporte deux fichiers contenant les définitions suivantes à utiliser avec VBScript :  
  
-   Pour l’utilisation de script côté serveur Adovbs.inc, qui est installé dans le dossier c:\Program Files\Common Files\System\ado\ par défaut.  
  
-   Pour l’utilisation de script côté client Adcvbs.inc, qui est installé dans le dossier c:\Program Files\Common Files\System\msdac\ par défaut.  
  
 Vous pouvez copier et coller les définitions de constante à partir de ces fichiers dans vos pages ASP ou, si vous effectuez le script côté serveur, copiez le fichier Adovbs.inc dans un dossier sur votre site Web et y faire référence à partir de votre page ASP comme suit :  
  
```  
<!--#include File="adovbs.inc"-->  
```  
  
## <a name="creating-ado-objects-in-vbscript"></a>Création d’objets ADO dans VBScript  
 Vous ne pouvez pas utiliser le **Dim** instruction pour assigner des objets à un type spécifique dans VBScript. En outre, VBScript ne prend pas en charge la **nouveau** syntaxe utilisée avec le **Dim** instruction en Visual Basic pour Applications. Vous devez utiliser le **CreateObject** l’appel de fonction :  
  
```  
Dim Rs1  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
```  
  
## <a name="vbscript-examples"></a>Exemples de VBScript  
 Le code suivant est un exemple générique de programmation côté serveur en VBScript dans un fichier de Page ASP (Active Server) :  
  
```  
<%  @LANGUAGE="VBSCRIPT" %>  
<%  Option Explicit %>  
<!--#include File="adovbs.inc"-->  
<HTML>  
    <BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
  
    <!-- Your ASP Code goes here -->  
<%  
Dim Source  
Dim Connect  
Dim Rs1  
  
Source = "SELECT * FROM Authors"  
Connect = "Provider=sqloledb;Data Source=srv;" & _  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
Rs1.Open Source, Connect, adOpenForwardOnly  
Response.Write("Success!")  
%>  
    </BODY>  
</HTML>  
```  
  
 Des exemples VBScript plus spécifiques sont inclus dans la documentation ADO. Pour plus d’informations, consultez [des exemples de Code ADO dans Microsoft Visual Basic Scripting Edition](../../../ado/reference/ado-api/ado-code-examples-vbscript.md).  
  
## <a name="differences-between-vbscript-and-visual-basic"></a>Différences entre VBScript et Visual Basic  
 À l’aide d’ADO avec VBScript est similaire à l’aide d’ADO avec Visual Basic de nombreuses manières, notamment la façon dont la syntaxe est utilisée. Toutefois, il existe quelques différences importantes :  
  
-   VBScript prend en charge uniquement le type de données Variant, qui peut contenir différents types de données. Vous pouvez stocker les données que nécessaires dans un type de données Variant et les données de fonctionner correctement en raison de casting effectué par VBScript. Il reconnaît le type requis par ADO et convertit la valeur de la variante en conséquence.  
  
-   Vous ne pouvez pas utiliser **sur erreur goto \<étiquette >** dans VBScript.  
  
-   VBScript prend en charge les certaines fonctions Visual Basic intégrées telles que **Msgbox**, **Date**, et **IsNumeric**. Toutefois, étant donné que VBScript est un sous-ensemble de Visual Basic, pas toutes les fonctions intégrées sont prises en charge. Par exemple, VBScript ne prend pas en charge la **Format** fonction et les fonctions d’e/s de fichier.
