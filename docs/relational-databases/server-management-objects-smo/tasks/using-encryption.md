---
title: L’utilisation du chiffrement | Documents Microsoft
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: smo
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- database master key [SMO]
- cryptography [SMO]
- cryptography [SQL Server], SMO
- encryption keys [SMO]
- encryption [SQL Server], SMO
- encryption [SMO]
- certificates [SMO]
- service master key [SMO]
ms.assetid: 405e0ed7-50a9-430e-a343-471f54b4af76
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 36b39fc4a13e3236e569af2209c680e7cdb0d534
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-encryption"></a>Utilisation du chiffrement
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Dans SMO, la clé principale du service est représentée par la <xref:Microsoft.SqlServer.Management.Smo.ServiceMasterKey> objet. Elle est référencée par la <xref:Microsoft.SqlServer.Management.Smo.Server.ServiceMasterKey%2A> propriété de la <xref:Microsoft.SqlServer.Management.Smo.Server> objet. Elle peut être régénérée à l’aide de la <xref:Microsoft.SqlServer.Management.Smo.ServiceMasterKey.Regenerate%2A> (méthode).  
  
 La clé principale de base de données est représentée par la <xref:Microsoft.SqlServer.Management.Smo.MasterKey> objet. Le <xref:Microsoft.SqlServer.Management.Smo.MasterKey.IsEncryptedByServer%2A> propriété indique si la clé principale de base de données est chiffrée par la clé principale du service. La copie chiffrée dans la base de données master est automatiquement mise à jour à chaque modification de la clé principale de la base de données.  
  
 Il est possible de supprimer le chiffrement à clé de service à l’aide du <xref:Microsoft.SqlServer.Management.Smo.MasterKey.DropServiceKeyEncryption%2A> (méthode) et de chiffrer la clé principale de base de données avec un mot de passe. Dans cette situation, vous devrez ouvrir explicitement la clé principale de base de données avant d'accéder aux clés privées qu'elle sécurise.  
  
 Lorsqu’une base de données est attachée à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous devez fournir le mot de passe pour la clé principale de base de données ou exécuter la <xref:Microsoft.SqlServer.Management.Smo.MasterKey.AddServiceKeyEncryption%2A> méthode pour créer une copie non chiffrée de la clé principale de base de données disponibles pour le chiffrement par clé principale du service. Cette étape est recommandée pour éviter d'avoir à ouvrir explicitement la clé principale de base de données.  
  
 Le <xref:Microsoft.SqlServer.Management.Smo.MasterKey.Regenerate%2A> méthode régénère la clé principale de base de données. Lorsque la clé principale de base de données est régénérée, toutes les clés qu'elle a permis de chiffrer sont déchiffrées, puis elles sont à nouveau chiffrées au moyen de la nouvelle clé principale de base de données. Le <xref:Microsoft.SqlServer.Management.Smo.MasterKey.DropServiceKeyEncryption%2A> méthode supprime le chiffrement de la clé principale de base de données par la clé principale du service. <xref:Microsoft.SqlServer.Management.Smo.MasterKey.AddServiceKeyEncryption%2A> entraîne le chiffrement d'une copie de la clé principale à l'aide de la clé principale du service, puis le stockage de cette clé à la fois dans la base de données actuelle et dans la base de données master.  
  
 Dans SMO, les certificats sont représentés par le <xref:Microsoft.SqlServer.Management.Smo.Certificate> objet. Le <xref:Microsoft.SqlServer.Management.Smo.Certificate> objet possède des propriétés qui spécifient la clé publique, le nom du sujet, la période de validité et les informations sur l’émetteur. L'autorisation d'accès au certificat est contrôlée en utilisant les méthodes **Grant**, **Revoke** et **Deny** .  
  
## <a name="example"></a>Exemple  
 Dans les exemples de code suivants, vous devez sélectionner l'environnement, le modèle et le langage de programmation à utiliser pour créer votre application. Pour plus d’informations, consultez [créer un Visual C&#35; projet SMO dans Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="adding-a-certificate-in-visual-c"></a>Ajout d'un certificat en Visual C#  
 L'exemple de code crée un certificat simple avec un mot de passe de chiffrement. Contrairement à d’autres objets, la <xref:Microsoft.SqlServer.Management.Smo.Certificate.Create%2A> méthode a plusieurs surcharges. La surcharge utilisée dans l'exemple crée un certificat avec un mot de passe de chiffrement.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            {  
                Server srv = new Server();  
  
                //Reference the AdventureWorks2012 database.   
                Database db = srv.Databases["AdventureWorks2012"];  
  
                //Define a Certificate object variable by supplying the parent database and name in the constructor.   
                Certificate c = new Certificate(db, "Test_Certificate");  
  
                //Set the start date, expiry date, and description.   
                System.DateTime dt;  
                DateTime.TryParse("January 01, 2010", out dt);  
                c.StartDate = dt;  
                DateTime.TryParse("January 01, 2015", out dt);  
                c.ExpirationDate = dt;  
                c.Subject = "This is a test certificate.";  
                //Create the certificate on the instance of SQL Server by supplying the certificate password argument.   
                c.Create("pGFD4bb925DGvbd2439587y");  
            }  
        }   
```  
  
## <a name="adding-a-certificate-in-powershell"></a>Ajout d'un certificat dans PowerShell  
 L'exemple de code crée un certificat simple avec un mot de passe de chiffrement. Contrairement à d’autres objets, la <xref:Microsoft.SqlServer.Management.Smo.Certificate.Create%2A> méthode a plusieurs surcharges. La surcharge utilisée dans l'exemple crée un certificat avec un mot de passe de chiffrement.  
  
```powershell  
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item AdventureWorks2012  
  
#Create a certificate  
  
$c = New-Object -TypeName Microsoft.SqlServer.Management.Smo.Certificate -argumentlist $db, "Test_Certificate"  
$c.StartDate = "January 01, 2010"  
$c.Subject = "This is a test certificate."  
$c.ExpirationDate = "January 01, 2015"  
  
#Create the certificate on the instance of SQL Server by supplying the certificate password argument.  
$c.Create("pGFD4bb925DGvbd2439587y")  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À l’aide de clés de chiffrement](../../../relational-databases/server-management-objects-smo/tasks/using-encryption.md)  
  
  
