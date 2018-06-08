---
title: Création d’un Assembly | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- creating assemblies
- UNSAFE assemblies
- CREATE ASSEMBLY statement
- SAFE assemblies
- EXTERNAL_ACCESS assemblies
- assemblies [CLR integration], creating
ms.assetid: a2bc503d-b6b2-4963-8beb-c11c323f18e0
caps.latest.revision: 27
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 3b22443461b5bb11d4e4ca1933d4f1d1f931fda9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-an-assembly"></a>Création d'un assembly
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les objets de base de données managés, tels que les procédures stockées ou les déclencheurs, sont successivement compilés et déployés dans des unités appelées « assemblys ». Les assemblys DLL managés doivent être enregistrés dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour enregistrer un assembly dans une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , utilisez l'instruction CREATE ASSEMBLY. Cette rubrique explique comment enregistrer un assembly dans une base de données à l'aide de l'instruction CREATE ASSEMBLY, puis comment spécifier les paramètres de sécurité de l'assembly.  
  
## <a name="the-create-assembly-statement"></a>Instruction CREATE ASSEMBLY  
 L'instruction CREATE ASSEMBLY permet de créer un assembly dans une base de données. Par exemple :  
  
```  
CREATE ASSEMBLY SQLCLRTest  
FROM 'C:\MyDBApp\SQLCLRTest.dll';  
```  
  
 La clause FROM spécifie le nom du chemin d'accès de l'assembly à créer. Ce chemin d'accès peut être soit un chemin UNC (Universal Naming Convention), soit le chemin d'accès d'un fichier physique stocké localement sur l'ordinateur.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'autorise pas l'inscription de différentes versions d'un assembly avec le même nom, la même culture et la même clé publique.  
  
 Il est possible de créer des assemblys qui référencent d'autres assemblys. Lorsque vous créez un assembly dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crée également les assemblys référencés par l'assembly au niveau de la racine si les assemblys référencés ne sont pas déjà créés dans la base de données.  
  
 Les utilisateurs ou les rôles d'utilisateurs de base de données se voient accorder des autorisations pour créer, et de ce fait acquérir, des assemblys dans une base de données. Pour être en mesure de créer des assemblys, l'utilisateur ou le rôle d'utilisateur de base de données doit bénéficier de l'autorisation CREATE ASSEMBLY.  
  
 Un assembly peut parvenir à référencer d'autres assemblys uniquement si :  
  
-   l'assembly appelé ou référencé est possédé par le même utilisateur ou rôle ;  
  
-   l'assembly appelé ou référencé a été créé dans la même base de données.  
  
## <a name="specifying-security-when-creating-assemblies"></a>Définition de la sécurité lors de la création d'assemblys  
 Lorsque vous créez un assembly dans une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous pouvez spécifier un des trois niveaux différents de sécurité dans lesquels votre code peut être exécuté : **SAFE**, **EXTERNAL_ACCESS**ou **UNSAFE**. Au moment de l'exécution de l'instruction **CREATE ASSEMBLY** , l'assembly de code est soumis à certains contrôles qui peuvent entraîner l'échec de l'enregistrement de l'assembly sur le serveur. Pour plus d'informations, consultez l'exemple d'emprunt d'identité sur [CodePlex](http://msftengprodsamples.codeplex.com/).  
  
 **SAFE** est le jeu d'autorisations par défaut et fonctionne pour la majorité des scénarios. Pour spécifier un niveau de sécurité donné, vous devez modifier la syntaxe de l'instruction CREATE ASSEMBLY comme suit :  
  
```  
CREATE ASSEMBLY SQLCLRTest  
FROM 'C:\MyDBApp\SQLCLRTest.dll'  
WITH PERMISSION_SET = SAFE;  
```  
  
 Il est également possible de créer un assembly à l'aide du jeu d'autorisations **SAFE** par simple omission de la troisième ligne de code ci-dessus :  
  
```  
CREATE ASSEMBLY SQLCLRTest  
FROM 'C:\MyDBApp\SQLCLRTest.dll';  
```  
  
 Lorsque le code dans un assembly s'exécute sous le jeu d'autorisations **SAFE** , tout calcul ou accès aux données sur le serveur ne peut avoir lieu que par l'entremise du fournisseur managé in-process.  
  
### <a name="creating-externalaccess-and-unsafe-assemblies"></a>Création d'assemblys EXTERNAL_ACCESS et UNSAFE  
 **EXTERNAL_ACCESS** propose des scénarios dans lesquels le code doit accéder à des ressources externes au serveur, tel que des fichiers, le réseau, le Registre et des variables d'environnement. Lorsque le serveur accède à une ressource externe, il emprunte l'identité du contexte de sécurité de l'utilisateur appelant le code managé.  
  
 **UNSAFE** convient dans des situations où la sécurité d'un assembly ne peut être vérifiée ou exige un accès supplémentaire à des ressources restreintes, telles que l'API Win32 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .  
  
 Pour la création d'un assembly **EXTERNAL_ACCESS** ou **UNSAFE** dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'une des deux conditions suivantes doit être remplie :  
  
1.  L'assembly est signé avec un nom fort ou porte une signature Authenticode avec certificat. Ce nom fort (ou certificat) est créé dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en tant que clé asymétrique (ou certificat) et dispose d'une connexion correspondante avec l'autorisation **EXTERNAL ACCESS ASSEMBLY** (pour les assemblys à accès externe) ou l'autorisation **UNSAFE ASSEMBLY** (pour les assemblys non sécurisés).  
  
2.  Le propriétaire de la base de données (DBO) bénéficie de l'autorisation **EXTERNAL ACCESS ASSEMBLY** (pour les assemblys **EXTERNAL ACCESS** ) ou **UNSAFE ASSEMBLY** (pour les assemblys **UNSAFE** ) et la [TRUSTWORTHY Database Property](../../../relational-databases/security/trustworthy-database-property.md) de la base de données est définie sur **ON**.  
  
 Les deux conditions mentionnées ci-dessus sont également vérifiées au moment du chargement de l'assembly (exécution incluse). Une des conditions doit au minimum être satisfaite pour le chargement de l'assembly.  
  
 Nous vous recommandons de ne pas définir la [TRUSTWORTHY Database Property](../../../relational-databases/security/trustworthy-database-property.md) sur **ON** dans une base de données uniquement afin d'exécuter le code du CLR (Common Language Runtime) dans le processus serveur. Il est préférable, à la place, de créer une clé asymétrique à partir du fichier d'assembly dans la base de données master. Une connexion mappée à cette clé asymétrique doit ensuite être créée. Cette connexion doit disposer de l'autorisation **EXTERNAL ACCESS ASSEMBLY** ou **UNSAFE ASSEMBLY** .  
  
 Les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivantes permettent de réaliser les étapes nécessaires pour créer une clé asymétrique, mapper une connexion à cette clé, puis accorder l'autorisation **EXTERNAL_ACCESS** à la connexion. Vous devez exécuter les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivantes avant d'exécuter l'instruction CREATE ASSEMBLY.  
  
```  
USE master;   
GO    
  
CREATE ASYMMETRIC KEY SQLCLRTestKey FROM EXECUTABLE FILE = 'C:\MyDBApp\SQLCLRTest.dll'     
CREATE LOGIN SQLCLRTestLogin FROM ASYMMETRIC KEY SQLCLRTestKey     
GRANT EXTERNAL ACCESS ASSEMBLY TO SQLCLRTestLogin;   
GO   
```  
  
> [!NOTE]  
>  Vous devez créer une connexion à associer à la clé asymétrique. Cette connexion permet uniquement d'accorder des autorisations ; elle ne doit pas être associée à un utilisateur ni utilisée dans l'application.  
  
 Pour créer un assembly **EXTERNAL ACCESS** , la personne chargée de le créer doit posséder l'autorisation **EXTERNAL ACCESS** . Elle est spécifiée au moment de créer l'assembly :  
  
```  
CREATE ASSEMBLY SQLCLRTest  
FROM 'C:\MyDBApp\SQLCLRTest.dll'  
WITH PERMISSION_SET = EXTERNAL_ACCESS;  
```  
  
 Les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivantes permettent de réaliser les étapes nécessaires pour créer une clé asymétrique, mapper une connexion à cette clé, puis accorder l'autorisation **UNSAFE** à la connexion. Vous devez exécuter les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivantes avant d'exécuter l'instruction CREATE ASSEMBLY.  
  
```  
USE master;   
GO    
  
CREATE ASYMMETRIC KEY SQLCLRTestKey FROM EXECUTABLE FILE = 'C:\MyDBApp\SQLCLRTest.dll';     
CREATE LOGIN SQLCLRTestLogin FROM ASYMMETRIC KEY SQLCLRTestKey ;    
GRANT UNSAFE ASSEMBLY TO SQLCLRTestLogin ;  
GO  
```  
  
 Pour préciser qu'un assembly est chargé avec l'autorisation **UNSAFE** , vous devez spécifier l'autorisation **UNSAFE** définie lors du chargement de l'assembly sur le serveur :  
  
```  
CREATE ASSEMBLY SQLCLRTest  
FROM 'C:\MyDBApp\SQLCLRTest.dll'  
WITH PERMISSION_SET = UNSAFE;  
```  
  
 Pour obtenir des informations détaillées sur les autorisations pour chacun de ces paramètres, consultez [CLR Integration Security](../../../relational-databases/clr-integration/security/clr-integration-security.md).  
  
## <a name="see-also"></a>Voir aussi  
 [La gestion des assemblys d’intégration du CLR](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md)   
 [Modification d’un Assembly](../../../relational-databases/clr-integration/assemblies/altering-an-assembly.md)   
 [Suppression d’un Assembly](../../../relational-databases/clr-integration/assemblies/dropping-an-assembly.md)   
 [Sécurité d’accès du Code CLR Integration](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)   
 [Propriété de base de données TRUSTWORTHY](../../../relational-databases/security/trustworthy-database-property.md)   
 [Autoriser partiellement approuvé appelants](http://msdn.microsoft.com/library/20b0248f-36da-4fc3-97d2-3789fcf6e084)  
  
  
