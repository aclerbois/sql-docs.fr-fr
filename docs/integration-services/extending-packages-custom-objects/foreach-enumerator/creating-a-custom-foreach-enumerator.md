---
title: Création d’un énumérateur ForEach personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-custom-objects
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
- custom foreach enumerators [Integration Services], creating
ms.assetid: 050e8455-2ed0-4b6d-b3ea-4e80e6c28487
caps.latest.revision: 53
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 77582f6511b889641ab730766700d21c98456713
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-a-custom-foreach-enumerator"></a>Création d'un énumérateur Foreach personnalisé
  Les étapes de création d'un énumérateur Foreach personnalisé sont semblables aux étapes de création de tout autre objet personnalisé pour [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] :  
  
-   Créer une classe qui hérite de la classe de base. Pour un énumérateur Foreach, la classe de base est <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>.  
  
-   Appliquer l'attribut qui identifie le type d'objet auprès de la classe. Pour un énumérateur Foreach, l'attribut est <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>.  
  
-   Substituer l'implémentation des méthodes et des propriétés de la classe de base. Pour un énumérateur Foreach, l'élément le plus important est la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>.  
  
-   Développer éventuellement une interface utilisateur personnalisée. Pour un énumérateur Foreach, cette opération requiert une classe qui implémente l'interface <xref:Microsoft.SqlServer.Dts.Runtime.IDTSForEachEnumeratorUI>.  
  
 Un énumérateur personnalisé est hébergé par le conteneur <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>. Au moment de l'exécution, le conteneur <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> appelle la méthode <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> de l'énumérateur personnalisé. L’énumérateur personnalisé retourne un objet qui implémente l’interface **IEnumerable**, par exemple un objet **ArrayList**. <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> parcourt ensuite chaque élément de la collection, fournit la valeur de l'élément actuel au flux de contrôle au moyen d'une variable définie par l'utilisateur et exécute le flux de contrôle dans le conteneur.  
  
## <a name="getting-started-with-a-custom-foreach-enumerator"></a>Mise en route d'un énumérateur Foreach personnalisé  
  
### <a name="creating-projects-and-classes"></a>Création de projets et de classes  
 Comme tous les énumérateurs Foreach managés dérivent de la classe de base <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>, la première étape de création d'un énumérateur Foreach personnalisé consiste à créer un projet Bibliothèque de classes dans votre langage de programmation managé par défaut et créer une classe qui hérite de la classe de base. Dans cette classe dérivée, vous devez substituer les méthodes et les propriétés de la classe de base pour implémenter vos fonctionnalités personnalisées.  
  
 Dans la même solution, créez un deuxième projet Bibliothèque de classes pour l'interface utilisateur personnalisée. Il est recommandé d'utiliser un assembly distinct pour l'interface utilisateur afin de faciliter le déploiement car vous pouvez ainsi mettre à jour et redéployer l'énumérateur Foreach ou son interface utilisateur de manière indépendante.  
  
 Configurez les deux projets pour qu'ils signent les assemblys qui seront créés au moment de la génération à l'aide d'un fichier de clé de nom fort.  
  
### <a name="applying-the-dtsforeachenumerator-attribute"></a>Application de l'attribut DtsForEachEnumerator  
 Appliquez l'attribut <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> à la classe que vous avez créée pour l'identifier en tant qu'énumérateur Foreach. Cet attribut fournit des informations au moment de la conception, telles que le nom et la description de l'énumérateur Foreach. La propriété **Name** apparaît dans la liste déroulante des énumérateurs disponibles sous l’onglet **Collection** de la boîte de dialogue **Éditeur de boucle Foreach**.  
  
 Utilisez la propriété <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A> pour lier l'énumérateur Foreach à son interface utilisateur personnalisée. Pour obtenir le jeton de clé publique requis pour cette propriété, vous pouvez utiliser **sn.exe -t** de manière à afficher le jeton de clé publique du fichier de paire de clés (.snk) que vous voulez utiliser pour signer l’assembly de l’interface utilisateur.  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Namespace Microsoft.Samples.SqlServer.Dts  
    <DtsForEachEnumerator(DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")> _   
    Public Class MyEnumerator  
     Inherits ForEachEnumerator  
        'Insert code here.  
    End Class  
End Namespace  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsForEachEnumerator( DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")]  
    public class MyEnumerator : ForEachEnumerator  
    {  
        //Insert code here.  
    }  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-enumerator"></a>Génération, déploiement et débogage d'un énumérateur personnalisé  
 Les étapes pour générer, déployer et déboguer un énumérateur Foreach personnalisé dans [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] sont très semblables aux étapes requises pour les autres types d'objets personnalisés. Pour plus d’informations, consultez [Génération, déploiement et débogage d’objets personnalisés](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Codage d’un énumérateur Foreach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)   
 [Développement d’une interface utilisateur pour un énumérateur ForEach personnalisé](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
  
  
