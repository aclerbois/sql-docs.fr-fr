---
title: L’exécution de requêtes XPath avec des espaces de noms (la Classes managées SQLXML) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- namespaces property
- XPath queries [SQLXML], SQLXML Managed Classes
- queries [SQLXML], SQLXML Managed Classes
- XPath queries [SQLXML], namespaces
- Managed Classes [SQLXML], executing XPath queries
- SQLXML Managed Classes, executing XPath queries
- namespaces [SQLXML], XPath queries
ms.assetid: c6fc46d8-6b42-4992-a8f1-a8d4b8886e6e
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b068d1d9848a7c462833d54fa6e92268fc83433f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executing-xpath-queries-with-namespaces-sqlxml-managed-classes"></a>Exécution de requêtes XPath avec des espaces de noms (classes managées SQLXML)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Les requêtes XPath peuvent inclure des espaces de noms. Si les éléments de schéma sont qualifiés par un espace de noms (utilisent un espace de noms cible), les requêtes XPath sur le schéma doivent spécifier cet espace de noms.  
  
 Le caractère générique (*) n'étant pas prise en charge dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0, vous devez spécifier la requête XPath en utilisant un préfixe d'espace de noms. Pour résoudre le préfixe, utilisez la propriété d’espaces de noms pour spécifier la liaison de l’espace de noms.  
  
 Dans l’exemple suivant, la requête XPath spécifie les espaces de noms à l’aide du caractère générique (\*) et les fonctions XPath dépourvue et namespace-uri(). Cette requête XPath retourne tous les éléments dont le nom local est **employé** et l’espace de noms URI **urn : myschema:Contacts**:  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 Dans SQLXML 4.0, spécifiez cette requête XPath avec un préfixe d'espace de noms. Par exemple **x : Contact**, où **x** est le préfixe d’espace de noms. Prenons le schéma XSD suivant :  
  
```  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:con="urn:myschema:Contacts"  
            targetNamespace="urn:myschema:Contacts">  
<complexType name="ContactType">  
  <attribute name="CID" sql:field="ContactID" type="ID"/>  
  <attribute name="FName" sql:field="FirstName" type="string"/>  
  <attribute name="LName" sql:field="LastName"/>   
</complexType>  
<element name="Contact" type="con:ContactType" sql:relation="Person.Contact"/>  
</schema>  
```  
  
 Comme ce schéma définit l'espace de noms cible, une requête Xpath (telle que « Employee ») sur ce schéma doit inclure l'espace de noms.  
  
 L'exemple d'application C# suivant exécute une requête XPath sur le schéma XSD précédent (MySchema.xml). Pour résoudre le préfixe, spécifiez la liaison de l’espace de noms à l’aide de la propriété d’espaces de noms de l’objet SqlXmlCommand.  
  
> [!NOTE]  
>  Dans le code, vous devez fournir le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans la chaîne de connexion.  
  
```  
using System;  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
      static string ConnString = "Provider=SQLOLEDB;Server=(local);database=AdventureWorks;Integrated Security=SSPI";  
      public static int testXPath()  
      {  
         //Stream strm;  
         SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
         cmd.CommandText = "x:Contact[@CID='1']";  
         cmd.CommandType = SqlXmlCommandType.XPath;  
         cmd.RootTag = "ROOT";  
         cmd.Namespaces = "xmlns:x='urn:myschema:Contacts'";  
         cmd.SchemaPath = "MySchema.xml";  
         using (Stream strm = cmd.ExecuteStream()){  
            using (StreamReader sr = new StreamReader(strm)){  
               Console.WriteLine(sr.ReadToEnd());  
            }  
         }  
         return 0;  
      }  
      public static int Main(String[] args)  
      {  
         testXPath();  
         return 0;  
      }  
   }  
```  
  
 Pour tester cet exemple, le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework doit être installé sur votre ordinateur.  
  
### <a name="to-test-the-application"></a>Pour tester l'application  
  
1.  Enregistrez dans un dossier le schéma XSD (MySchema.xml) fourni dans cet exemple.  
  
2.  Enregistrez le code c# (DocSample.cs) fourni dans cet exemple dans le même dossier que celui dans lequel le schéma est stocké. (Si vous stockez les fichiers dans un dossier différent, vous devrez modifier le code et spécifier le chemin d'accès approprié au répertoire pour le schéma de mappage.)  
  
3.  Compilez le code. Pour compiler le code à l'invite de commandes, utilisez :  
  
    ```  
    csc /reference:Microsoft.Data.SqlXML.dll DocSample.cs  
    ```  
  
     Un fichier exécutable (DocSample.exe) est alors créé.  
  
4.  À l'invite de commandes, exécutez DocSample.exe.  
  
  
