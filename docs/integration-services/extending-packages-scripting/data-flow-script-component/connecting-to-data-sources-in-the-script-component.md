---
title: Connexion aux sources de données dans le composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- Script component [Integration Services], connecting to data sources
ms.assetid: 96de63ab-ff48-4e7e-89e0-ffd6a89c63b6
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8e1f1a0d3625c262ad5adab71e23a195410d188a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-data-sources-in-the-script-component"></a>Connexion aux sources de données dans le composant Script
  Un gestionnaire de connexions est une unité pratique qui encapsule et stocke les informations requises pour se connecter à une source de données d'un type particulier. Pour plus d’informations, consultez [Connexions Integration Services &#40;SSIS&#41;](../../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
 Vous pouvez faire en sorte que des gestionnaires de connexions existants soient accessibles par le script personnalisé compris dans le composant source ou de destination en cliquant sur les boutons **Ajouter** et **Supprimer** de la page **Gestionnaires de connexions** de l’**Éditeur de transformation de script**. Toutefois, vous devez écrire votre propre code personnalisé pour charger ou enregistrer vos données et éventuellement ouvrir et fermer la connexion à la source de données. Pour plus d’informations sur la page **Gestionnaires de connexions** de l’**Éditeur de transformation de script**, consultez [Configuration du composant Script dans l’Éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) et [Éditeur de transformation de script &#40;Page Gestionnaires de connexions&#41;](../../../integration-services/data-flow/transformations/script-transformation-editor-connection-managers-page.md).  
  
 Le composant Script crée une classe de collection **Connections** dans l’élément de projet **ComponentWrapper** qui contient un accesseur fortement typé pour chaque gestionnaire de connexions qui porte le même nom que le gestionnaire de connexions lui-même. Cette collection est exposée via la propriété **Connections** de la classe **ScriptMain**. La propriété de l'accesseur renvoie une référence au gestionnaire de connexions sous forme d'instance de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSConnectionManager100>. Par exemple, si vous avez ajouté un gestionnaire de connexion nommé `MyADONETConnection` dans la page Gestionnaires de connexions de la boîte de dialogue, vous pouvez obtenir une référence à ce dernier dans votre script en ajoutant le code suivant :  
  
 `Dim myADONETConnectionManager As IDTSConnectionManager100 = _`  
  
 `Me.Connections.MyADONETConnection`  
  
> [!NOTE]  
>  Vous devez connaître le type de connexion retourné par le gestionnaire de connexions avant d’appeler **AcquireConnection**. Dans la mesure où **Option Strict** est activé pour la tâche de script, vous devez caster la connexion (retournée en tant que type **Object**) en type de connexion approprié avant de pouvoir l’utiliser.  
  
 Ensuite, appelez la méthode **AcquireConnection** du gestionnaire de connexions spécifique pour obtenir soit la connexion sous-jacente, soit les informations requises pour vous connecter à la source de données. Par exemple, vous obtenez une référence au **System.Data.SqlConnection** encapsulé par un gestionnaire de connexions ADO.NET en utilisant le code suivant :  
  
 `Dim myADOConnection As SqlConnection = _`  
  
 `CType(MyADONETConnectionManager.AcquireConnection(Nothing), SqlConnection)`  
  
 Par opposition, le même appel d'un gestionnaire de connexions de fichier plat retourne uniquement le chemin d'accès et le nom de fichier de la source de données.  
  
 `Dim myFlatFile As String = _`  
  
 `CType(MyFlatFileConnectionManager.AcquireConnection(Nothing), String)`  
  
 Vous devez alors fournir ce chemin et ce nom de fichier à un **System.IO.StreamReader** ou **Streamwriter** pour lire ou écrire les données dans le fichier plat.  
  
> [!IMPORTANT]  
>  Lorsque vous écrivez du code managé dans un composant Script, vous ne pouvez pas appeler la méthode AcquireConnection des gestionnaires de connexions qui retournent des objets non managés, comme le gestionnaire de connexions OLE DB et le gestionnaire de connexions Excel. Toutefois, vous pouvez lire la propriété ConnectionString de ces gestionnaires de connexions et vous connecter directement à la source de données dans votre code en utilisant la chaîne de connexion d’une **connexion** OLEDB à partir de l’espace de noms **System.Data.OleDb**.  
>   
>  Si vous devez appeler la méthode AcquireConnection d’un gestionnaire de connexions qui retourne un objet non managé, utilisez un gestionnaire de connexions ADO.NET. Lorsque vous configurez le gestionnaire de connexions ADO.NET afin d'utiliser un fournisseur OLE DB, il se connecte en utilisant le fournisseur de données .NET Framework pour OLE DB. Dans ce cas, la méthode AcquireConnection retourne un **System.Data.OleDb.OleDbConnection** à la place d’un objet non managé. Pour configurer un gestionnaire de connexions ADO.NET en vue de son utilisation avec une source de données Excel, sélectionnez le fournisseur Microsoft OLE DB pour Jet, spécifiez un classeur Excel, puis entrez `Excel 8.0` (pour Excel 97 et versions ultérieures) comme valeur de **Propriétés étendues** dans la page **Tout** de la boîte de dialogue **Gestionnaire de connexions**.  
  
 Pour plus d’informations sur l’utilisation des gestionnaires de connexions avec le composant Script, consultez [Création d’une source avec le composant Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-source-with-the-script-component.md) et [Création d’une destination avec le composant Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Connexions Integration Services &#40;SSIS&#41;](../../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Créer des gestionnaires de connexions](http://msdn.microsoft.com/library/6ca317b8-0061-4d9d-b830-ee8c21268345)  
  
  
