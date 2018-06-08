---
title: Gestion des erreurs dans l’API TOM (Analysis Services AMO-TOM) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b2483d4d6d443a21f43cf11e5271bb11041f1c53
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="handling-errors-in-the-tom-api-analysis-services-amo-tom"></a>Gestion des erreurs dans l’API TOM (Analysis Services AMO-TOM)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Une pratique courante pour les bibliothèques managées, telles que le modèle d’objet tabulaire (TOM) Analysis Services Management Objects (AMO) est d’utiliser des exceptions comme un mécanisme pour signaler des conditions d’erreur à l’utilisateur.  

Lorsqu’une erreur est détectée dans AMO-TOM, en plus de lever des exceptions .NET standard quelques comme **ArgumentException** et **InvalidOperationException**, TOM peut lever plusieurs exceptions spécifiques à TOM.  

TOM exceptions sont dérivées de [AmoException classe](http://msdn.microsoft.com/library/microsoft.analysisservices.amoexception.aspx), couvrant les deux exceptions spécifiques TOM et AMO. 

Pour illustrer la gestion des exceptions dans TOM, passons en revue une des exceptions plus communes, qui est [OperationException classe](http://msdn.microsoft.com/library/microsoft.analysisservices.operationexception.aspx).

**OperationException** est levée lorsqu’un utilisateur initie une opération sur le serveur Analysis Services et le serveur ne parvient pas à effectuer une opération, car l’action est non conforme, ou en raison d’une autre erreur interne ou externe. 

Lors de la levée, ** OperationException ** objet contient une liste d’erreurs XMLA retournés par le serveur. 

Notez que le serveur n’accepte pas les modifications qui ne sont pas valides. Si cela se produit, rétablissez le **modèle** arborescence dans la dernière configuration correcte connue à l’aide du [UndoLocalChanges méthode](http://msdn.microsoft.com/library/microsoft.analysisservices.tabular.model.undolocalchanges.aspx), corrigez le modèle, puis soumettez à nouveau. 

## <a name="code-example-handle-exceptions"></a>L’exemple de code : gérer les exceptions 
 
```
 try 
 { 
  // Change the Model, for example create a table. 
  // … 
   model.saveChanges(); 
 } 
  catch(operationException ex) 
 { 
  foreach(XmlaError err in ex.Results.OfType<XmlaError>().cast<XmlaError>()) 
  { 
   Console.WriteLine(“Error returned from the server:” + err.Messsage ); 
  } 
 } 
```

## <a name="next-steps"></a>Étapes suivantes

D’autres exceptions pertinentes sont les suivantes :

- [Classe de TomInternalException](http://msdn.microsoft.com/library/microsoft.analysisservices.tabular.tominternalexception.aspx)
- [Classe de TomValidationException](http://msdn.microsoft.com/library/microsoft.analysisservices.tabular.tomvalidationexception.aspx)
- [Classe de JsonSerializationException](http://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonSerializationException.htm)
