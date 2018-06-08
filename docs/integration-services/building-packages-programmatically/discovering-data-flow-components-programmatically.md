---
title: Découverte des composants de flux de données par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: building-packages-programmatically
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- PipelineComponentInfos collection
- data flow task [Integration Services], components
- discovering data flow components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: ff92a96a-8af6-4532-82cc-c0bbff92401b
caps.latest.revision: 44
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f36b4d0d2235c03cee6bfc51c83a03a051e0cebe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="discovering-data-flow-components-programmatically"></a>Découverte des composants de flux de données par programme
  Une fois que vous avez ajouté une tâche de flux de données à un package, l'étape suivante consiste peut-être à déterminer de quels composants de flux de données vous disposez. Vous pouvez découvrir par programme les sources, transformations et destinations de flux de données installées et disponibles sur l'ordinateur local. Pour plus d’informations sur l’ajout d’une tâche de flux de données au package, consultez [Ajout de la tâche flux de données par programmation](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md).  
  
## <a name="discovering-components"></a>Découverte des composants  
 La classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> fournit la collection <xref:Microsoft.SqlServer.Dts.Runtime.Application.PipelineComponentInfos%2A>, qui contient un objet <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo> pour chaque composant installé correctement sur l'ordinateur local. Chaque objet <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo> contient des informations sur un composant, telles que son nom, sa description et son nom de création. Vous pouvez utiliser la valeur retournée dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A> pour définir la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> de l'objet <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> lorsque vous ajoutez un composant à un package.  
  
## <a name="next-step"></a>Étape suivante  
 Après avoir découvert les composants disponibles, l’étape suivante consiste à ajouter et configurer les composants, ce qui est décrit dans la nouvelle rubrique, [Ajout de composants de flux de données par programmation](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md).  
  
## <a name="sample"></a>Exemple  
 L'exemple de code suivant indique comment énumérer la collection <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfos> de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.Application> pour découvrir par programme les composants de flux de données disponibles sur l'ordinateur local. Cet exemple requiert une référence à l’assembly Microsoft.SqlServer.ManagedDTS.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application application = new Application();  
      PipelineComponentInfos componentInfos = application.PipelineComponentInfos;  
  
      foreach (PipelineComponentInfo componentInfo in componentInfos)  
      {  
        Console.WriteLine("Name: " + componentInfo.Name + "\n" +  
          " CreationName: " + componentInfo.CreationName + "\n");  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim application As Application = New Application()  
  
    Dim componentInfos As PipelineComponentInfos = application.PipelineComponentInfos  
  
    For Each componentInfo As PipelineComponentInfo In componentInfos  
      Console.WriteLine("Name: " & componentInfo.Name & vbCrLf & _  
        " CreationName: " & componentInfo.CreationName & vbCrLf)  
    Next  
  
    Console.Read()  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a> Voir aussi  
 [Ajout de composants de flux de données par programmation](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md)  
  
  
