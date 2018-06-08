---
title: Conception d’assemblys | Documents Microsoft
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
- designing assemblies [SQL Server]
- assemblies [CLR integration], designing
ms.assetid: 9c07f706-6508-41aa-a4d7-56ce354f9061
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 33519a8494be8f6a062227b81999f58c4f053071
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="assemblies---designing"></a>Assemblys - conception
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit les facteurs suivants, à prendre en considération lors de la conception d'assemblys :  
  
-   Empaqueter des assemblys  
  
-   Gestion de la sécurité de l’assembly  
  
-   Restrictions sur les assemblys  
  
## <a name="packaging-assemblies"></a>Empaquetage d'assemblys  
 Un assembly peut contenir des fonctionnalités pour plusieurs types ou routines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans ses classes et méthodes. En règle générale, il est opportun d'empaqueter les fonctionnalités de routines qui réalisent des fonctions apparentées dans le même assembly, notamment si ces routines partagent des classes dont les méthodes s'appellent les unes les autres. Par exemple, les classes qui exécutent des tâches de gestion d'entrée de données pour les déclencheurs CLR (Common Language Runtime) et les procédures stockées CLR peuvent être empaquetées dans le même assembly. En effet, les méthodes de ces classes sont davantage susceptibles de s'appeler les unes les autres que d'invoquer les méthodes de tâches moins apparentées.  
  
 Lorsque vous empaquetez du code en assembly, vous devez prendre en considération les points suivants :  
  
-   Les types et index CLR définis par l'utilisateur qui dépendent de fonctions CLR définies par l'utilisateur peuvent amener des données persistantes à figurer dans la base de données qui repose sur l'assembly. La modification du code d'un assembly est souvent plus complexe lorsque la base de données contient des données persistantes dépendant de l'assembly. Par conséquent, il est généralement préférable de séparer le code sur lequel reposent les dépendances des données persistantes (par exemple des types et des index définis par l'utilisateur utilisant des fonctions définies par l'utilisateur) du code dépourvu de ces dépendances de données persistantes. Pour plus d’informations, consultez [mise en œuvre des assemblys](../../relational-databases/clr-integration/assemblies-implementing.md) et [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md).  
  
-   Si une portion de code managé requiert une autorisation plus élevée, il est préférable de la placer dans un assembly distinct du code qui ne requiert pas une telle autorisation.  
  
## <a name="managing-assembly-security"></a>Gestion de la sécurité des assemblys  
 Vous pouvez contrôler la quantité d’un assembly peut accéder aux ressources protégées par la sécurité d’accès du Code .NET lorsqu’il s’exécute du code managé. Pour ce faire, vous spécifiez l'un des trois jeux d'autorisations lorsque vous créez ou modifiez un assembly : SAFE, EXTERNAL_ACCESS ou UNSAFE.  
  
### <a name="safe"></a>SAFE  
 SAFE est le jeu d'autorisations par défaut et il s'agit du plus restrictif. Le code exécuté par un assembly avec des autorisations SAFE ne peut pas accéder aux ressources système externes telles que les fichiers, le réseau, les variables d'environnement ou le Registre. Le code SAFE peut accéder aux données des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locales ou réaliser des calculs et une logique métier qui ne nécessitent pas l'accès aux ressources situées hors de ces bases de données.  
  
 La plupart des assemblys réalisent des calculs et des tâches de gestion de données sans devoir accéder aux ressources situées hors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, il est recommandé d'utiliser SAFE comme jeu d'autorisations des assemblys.  
  
### <a name="externalaccess"></a>EXTERNAL_ACCESS  
 EXTERNAL_ACCESS permet aux assemblys d'accéder à certaines ressources système externes telles que les fichiers, les réseaux, les services Web, les variables d'environnement et le Registre. Seules les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bénéficiant d'autorisations EXTERNAL ACCESS peuvent créer des assemblys EXTERNAL_ACCESS.  
  
 Les assemblys SAFE et EXTERNAL_ACCESS ne peuvent contenir que du code de type sécurisé. Cela signifie que ces assemblys peuvent uniquement accéder à des classes par le biais de points d'entrée correctement définis et valides pour la définition de type. Par conséquent, ils ne peuvent pas accéder arbitrairement à des zones de mémoire tampon non détenues par le code. En outre, ils ne peuvent pas réaliser des opérations susceptibles de nuire à la robustesse du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="unsafe"></a>UNSAFE  
 UNSAFE accorde aux assemblys un accès non restreint aux ressources, à la fois à l'intérieur et à l'extérieur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le code exécuté depuis un assembly UNSAFE peut appeler du code non managé.  
  
 En outre, la spécification de UNSAFE permet au code de l'assembly de réaliser des opérations considérées comme étant de type non sécurisé par le vérificateur CLR. Ces opérations sont susceptibles d'accéder de manière incontrôlée aux zones de tampon mémoire de l'espace réservé aux processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les assemblys UNSAFE peuvent également corrompre le système de sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de CLR (Common Language Runtime). Les autorisations UNSAFE ne doivent être accordées par des administrateurs ou des développeurs expérimentés qu'aux assemblys hautement approuvés. Seuls les membres de la **sysadmin** rôle serveur fixe peut créer des assemblys UNSAFE.  
  
## <a name="restrictions-on-assemblies"></a>Restrictions associées aux assemblys  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impose certaines restrictions au code managé des assemblys afin que ceux-ci puissent s'exécuter de manière fiable et évolutive. Cela signifie que certaines opérations pouvant compromettre la robustesse du serveur ne sont pas autorisées dans les assemblys SAFE et EXTERNAL_ACCESS.  
  
### <a name="disallowed-custom-attributes"></a>Attributs personnalisés interdits  
 Les assemblys ne peuvent pas être annotés avec les attributs personnalisés suivants :  
  
```  
System.ContextStaticAttribute  
System.MTAThreadAttribute  
System.Runtime.CompilerServices.MethodImplAttribute  
System.Runtime.CompilerServices.CompilationRelaxationsAttribute  
System.Runtime.Remoting.Contexts.ContextAttribute  
System.Runtime.Remoting.Contexts.SynchronizationAttribute  
System.Runtime.InteropServices.DllImportAttribute   
System.Security.Permissions.CodeAccessSecurityAttribute  
System.STAThreadAttribute  
System.ThreadStaticAttribute  
```  
  
 En outre, les assemblys SAFE et EXTERNAL_ACCESS ne peuvent pas être annotés avec les attributs personnalisés suivants :  
  
```  
System.Security.SuppressUnmanagedCodeSecurityAttribute  
System.Security.UnverifiableCodeAttribute  
```  
  
### <a name="disallowed-net-framework-apis"></a>API .NET Framework interdites  
 N’importe quel [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] API est annoté avec l’un des **HostProtectionAttributes** ne peut pas être appelée à partir des assemblys SAFE et EXTERNAL_ACCESS.  
  
```  
eSelfAffectingProcessMgmt  
eSelfAffectingThreading  
eSynchronization  
eSharedState   
eExternalProcessMgmt  
eExternalThreading  
eSecurityInfrastructure  
eMayLeakOnAbort  
eUI  
```  
  
### <a name="supported-net-framework-assemblies"></a>Assemblys .NET Framework pris en charge  
 Tout assembly référencé par votre assembly personnalisé doit être chargé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de CREATE ASSEMBLY. Les assemblys [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] ci-dessous sont déjà chargés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peuvent, par conséquent, être référencés par les assemblys personnalisés sans qu'il soit nécessaire d'utiliser CREATE ASSEMBLY.  
  
```  
custommarshallers.dll  
Microsoft.visualbasic.dll  
Microsoft.visualc.dll  
mscorlib.dll  
system.data.dll  
System.Data.SqlXml.dll  
system.dll  
system.security.dll  
system.web.services.dll  
system.xml.dll  
System.Transactions  
System.Data.OracleClient  
System.Configuration  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Assemblys & #40 ; moteur de base de données & #41 ;](../../relational-databases/clr-integration/assemblies-database-engine.md)   
 [Sécurité d’intégration du CLR](../../relational-databases/clr-integration/security/clr-integration-security.md)  
  
  
