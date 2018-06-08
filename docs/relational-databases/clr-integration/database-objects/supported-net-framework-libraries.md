---
title: Prise en charge des bibliothèques .NET Framework | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], .NET Framework libraries
- .NET Framework [CLR Integration]
ms.assetid: 417544ff-c25c-496e-add4-2f278f8a4911
caps.latest.revision: 25
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 24cd0f69a0167b5e742381b0e2e88f2cc84d1a2c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supported-net-framework-libraries"></a>Bibliothèques .NET Framework prises en charge
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Avec le CLR (Common Language Runtime) hébergé dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez créer des procédures stockées, des déclencheurs, des fonctions définies par l'utilisateur, des types définis par l'utilisateur et des agrégats définis par l'utilisateur en code managé. Avec les fonctionnalités présentes dans les bibliothèques de classes .NET Framework, vous avez accès à des classes prégénérées qui fournissent des fonctionnalités pour la manipulation de chaînes, des opérations de mathématique avancées, l'accès au fichier, le chiffrement, etc. Ces classes sont accessibles à partir de procédures stockées managées, de types définis par l'utilisateur, de déclencheurs, de fonctions définies par l'utilisateur ou d'agrégats définis par l'utilisateur, quels qu'ils soient.  
  
> [!NOTE]  
>  Si vous effectuez la maintenance ou mettez à niveau des assemblys non pris en charge dans le Global Assembly Cache (GAC), votre application [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut cesser de fonctionner. Cela est dû au fait que la maintenance ou la mise à niveau de bibliothèques dans le GAC ne met pas à jour ces assemblys dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. S'il existe un assembly dans une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et dans le GAC, les deux copies de l'assembly doivent être exactement les mêmes. Si ce n'est pas le cas, une erreur se produit lorsque l'assembly est utilisé par l'intégration du CLR [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si le service ou de la mise à niveau de tous les assemblys dans le GAC qui sont également enregistrés dans la base de données, y compris les assemblys .NET Framework non pris en charge, assurez-vous également de service ou de mettre à niveau de la copie de l’assembly à l’intérieur de votre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bases de données avec le **ALTER ASSEMBLY** instruction. Pour plus d’informations, consultez [l’article 949080 de la Base de connaissances](http://support.microsoft.com/kb/949080).  
  
## <a name="supported-libraries"></a>Bibliothèques prises en charge  
 À compter de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge une liste de bibliothèques .NET Framework qui ont été testées pour vérifier qu'elles répondent aux normes de fiabilité et de sécurité en matière d'interaction avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Vous n'avez pas besoin d'inscrire explicitement les bibliothèques prises en charge sur le serveur avant de pouvoir les utiliser dans votre code ; [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les charge directement à partir du Global Assembly Cache (GAC).  
  
 Les bibliothèques/espaces de noms pris en charge par l'intégration du CLR dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont les suivants :  
  
-   CustomMarshalers  
  
-   Microsoft.VisualBasic  
  
-   Microsoft.VisualC  
  
-   mscorlib  
  
-   Système  
  
-   System.Configuration  
  
-   System.Data  
  
-   System.Data.OracleClient  
  
-   System.Data.SqlXml  
  
-   System.Deployment  
  
-   System.Security  
  
-   System.Transactions  
  
-   System.Web.Services  
  
-   System.Xml  
  
-   System.Core.dll  
  
-   System.Xml.Linq.dll  
  
## <a name="unsupported-libraries"></a>Bibliothèques non prises en charge  
 Il est toujours possible d'appeler des bibliothèques non prises en charge à partir de vos procédures stockées managées, déclencheurs, fonctions définies par l'utilisateur, types définis par l'utilisateur et agrégats définis par l'utilisateur. La bibliothèque non pris en charge doit d’abord être enregistrée dans le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de base de données, à l’aide de la **CREATE ASSEMBLY** instruction, avant de pouvoir être utilisé dans votre code. Toute bibliothèque non prise en charge qui est inscrite et exécutée sur le serveur doit être examinée et testée en termes de sécurité et de fiabilité.  
  
 Par exemple, le **System.DirectoryServices** espace de noms n’est pas pris en charge. Vous devez inscrire l’assembly System.DirectoryServices.dll avec **UNSAFE** autorisations avant de pouvoir l’appeler à partir de votre code. Le **UNSAFE** autorisation est nécessaire, car des classes dans le **System.DirectoryServices** espace de noms ne respectent pas la configuration requise pour **-SAFE** ou **EXTERNAL_ACCESS**. Pour plus d’informations, consultez [Restrictions du modèle de programmation CLR Integration](../../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md) et [sécurité d’accès du Code CLR Integration](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’un Assembly](../../../relational-databases/clr-integration/assemblies/creating-an-assembly.md)   
 [Sécurité d’accès du Code CLR Integration](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)   
 [Restrictions du modèle de programmation de l’intégration du CLR](../../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md)  
  
  
