---
title: 'Étape 5 : DataControl est effectuée utilisable (didacticiel RDS) | Documents Microsoft'
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
helpviewer_keywords:
- RDS tutorial [ADO], datacontrol made usable
ms.assetid: ed5c4a24-9804-4c85-817e-317652acb9b4
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 564c9b1984226c02ab1cb960ff6e4ed32b5527d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-5-datacontrol-is-made-usable-rds-tutorial"></a>Étape 5 : DataControl est effectuée utilisable (didacticiel sur les services Bureau à distance)
Retourné **Recordset** objet est disponible pour utilisation. Vous pouvez examiner, accédez ou la modifier comme vous le feriez pour n’importe quel autre **Recordset**. Vous pouvez faire avec le **Recordset** dépend de votre environnement. Visual Basic et Visual C++ ont des contrôles visuels que vous peuvent utiliser un **Recordset** directement ou indirectement à l’aide d’un contrôle d’activation de données.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Par exemple, si vous affichez une page Web dans Microsoft Internet Explorer, vous pouvez souhaiter afficher le **Recordset** les données dans un contrôle visuel de l’objet. Les contrôles visuels d’une page Web ne peut pas accéder à un **Recordset** directement l’objet. Toutefois, ils peuvent accéder à la **Recordset** via la [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md). Le **RDS. DataControl** peut être utilisé par un élément visuel contrôle lorsque ses [SourceRecordset](../../../ado/reference/rds-api/recordset-sourcerecordset-properties-rds.md) est définie sur le **Recordset** objet.  
  
 L’objet de contrôle visuel doit avoir son **DATASRC** paramètre défini sur le **RDS. DataControl**et son **DATAFLD** propriété définie sur une **Recordset** champ (colonne) l’objet.  
  
 Dans ce didacticiel, définissez la **SourceRecordset** propriété :  
  
```  
Sub RDSTutorial5()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DC as New RDS.DataControl  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "http://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
   DC.SourceRecordset = RS         ' Visual controls can now bind to DC.  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 6 : Les modifications sont envoyées au serveur (didacticiel RDS)](../../../ado/guide/remote-data-service/step-6-changes-are-sent-to-the-server-rds-tutorial.md)   
 [Didacticiel RDS (VBScript)](../../../ado/guide/remote-data-service/rds-tutorial-vbscript.md)   
