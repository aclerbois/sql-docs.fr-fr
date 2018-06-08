---
title: Débogage d’objets de base de données CLR | Documents Microsoft
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
- database objects [CLR integration], debugging
- permissions [CLR integration]
- debugging [CLR integration]
- building database objects [CLR integration], debugging
- common language runtime [SQL Server], debugging
ms.assetid: 1332035c-d6ed-424d-8234-46ad21168319
caps.latest.revision: 46
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 8bf6b05d891d18fec8cfc063321f294c914cfa13
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="debugging-clr-database-objects"></a>Débogage d'objets de base de données CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le débogage d'objets [!INCLUDE[tsql](../../includes/tsql-md.md)] et CLR dans la base de données. Les principaux atouts du débogage dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont la facilité d'installation et d'utilisation, et l'intégration du débogueur SQL Server avec le débogueur Microsoft Visual Studio. En outre, le débogage fonctionne sur plusieurs langages. Les utilisateurs peuvent effectuer de façon transparente un pas à pas détaillé dans les objets CLR à partir de [!INCLUDE[tsql](../../includes/tsql-md.md)] et vice versa. Le débogueur Transact-SQL dans SQL Server Management Studio ne peut pas être utilisé pour déboguer des objets de base de données managés, mais vous pouvez déboguer les objets en utilisant les débogueurs de Visual Studio. Le débogage d'objets de base de données managés dans Visual Studio prend en charge toutes les fonctionnalités de débogage classiques, par exemple, les instructions "step into" et "step over" dans les routines qui s'exécutent sur le serveur. Les débogueurs peuvent définir des points d'arrêt, inspecter la pile des appels, inspecter les variables et modifier des valeurs de variables en cours de débogage. Notez que Visual Studio .NET 2003 ne peut pas être utilisé pour le débogage ou la programmation de l'intégration du CLR. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut le .NET Framework préinstallé et Visual Studio .NET 2003 ne peut pas utiliser les assemblys .NET Framework 2.0.  
  
 Pour plus d’informations sur le débogage de code managé à l’aide de Visual Studio, consultez le «[débogage du Code managé](http://go.microsoft.com/fwlink/?LinkId=120377)« rubrique dans la documentation de Visual Studio.  
  
## <a name="debugging-permissions-and-restrictions"></a>Autorisations et restrictions de débogage  
 Le débogage est une opération hautement privilégiée et par conséquent, seuls les membres de la **sysadmin** rôle serveur fixe sont autorisés à effectuer dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les restrictions suivantes s'appliquent dans le cadre du débogage :  
  
-   Le débogage des routines CLR est restreint à une instance de débogueur à la fois. Cette limitation s'applique parce que l'exécution de la totalité du code CLR est gelée lorsqu'un point d'arrêt est atteint et qu'elle ne reprend que lorsque le débogueur continue à partir du point d'arrêt. Toutefois, vous pouvez continuer à déboguer [!INCLUDE[tsql](../../includes/tsql-md.md)] dans d'autres connexions. Bien que le débogage [!INCLUDE[tsql](../../includes/tsql-md.md)] ne gèle pas d'autres exécutions sur le serveur, il peut provoquer la mise en attente d'autres connexions par le maintien d'un verrou.  
  
-   Les connexions existantes ne peuvent pas être déboguées ; seules les nouvelles connexions peuvent l'être, puisque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert des informations sur l'environnement du client et du débogueur pour que la connexion puisse être établie.  
  
 En raison des restrictions précitées, nous recommandons que le débogage du code [!INCLUDE[tsql](../../includes/tsql-md.md)] et CLR soit effectué sur un serveur de test et non sur un serveur de production.  
  
## <a name="overview-of-debugging-managed-database-objects"></a>Vue d'ensemble du débogage des objets de base de données managés  
 Le débogage dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suit un modèle « par connexion ». Un débogueur peut détecter et déboguer des activités uniquement sur la connexion cliente à laquelle il est attaché. Comme les fonctionnalités du débogueur ne sont pas limitées par le type de connexion, il est possible de déboguer à la fois des TDS (Tabular Data Stream) et des connexions HTTP. Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas le débogage des connexions existantes. Le débogage prend en charge toutes les fonctionnalités de débogage communes dans les routines qui s'exécutent sur le serveur. L'interaction entre un débogueur et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'effectue par le biais d'un modèle COM (Component Object Model) distribué.  
  
 Pour plus d’informations et des scénarios de débogage des procédures stockées managées, des fonctions, des déclencheurs, des types définis par l’utilisateur et des agrégats, consultez le «[débogage pour la base de données SQL Server CLR Integration](http://go.microsoft.com/fwlink/?LinkId=120378)« rubrique dans la documentation de Visual Studio.  
  
 Le protocole réseau TCP/IP doit être activé sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d'utiliser Visual Studio pour le développement et le débogage distants. Pour plus d’informations sur l’activation du protocole TCP/IP sur le serveur, consultez [configurer des protocoles clients](../../database-engine/configure-windows/configure-client-protocols.md).  
  
#### <a name="to-debug-a-managed-database-object"></a>Pour déboguer un objet de base de données managé  
  
1.  Ouvrez Microsoft Visual Studio, créez un projet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et établissez une connexion à une base de données sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Créez un type. Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter** et **un nouvel élément...** À partir de la **ajouter un nouvel élément** fenêtre, sélectionnez **la procédure stockée**, **(fonction) définis par l’utilisateur**, **User-Defined Type**, **déclencheur**, **d’agrégation**, ou **classe**. Spécifiez un nom pour le fichier source du nouveau type, cliquez sur **ajouter**.  
  
3.  Ajoutez le code du nouveau type à l'éditeur de texte. Pour obtenir le code d'un exemple de procédure stockée, consultez la section correspondante plus loin dans cette rubrique.  
  
4.  Ajoutez un script qui teste le type. Dans **l’Explorateur de solutions**, développez le **TestScripts** double-clic du répertoire **Test.sql** pour ouvrir le fichier de source du script de test par défaut. Ajoutez à l'éditeur de texte un script de test qui appelle le code à déboguer. Un exemple de script est proposé ci-dessous.  
  
5.  Placez un ou plusieurs points d'arrêt dans le code source. Avec le bouton droit sur une ligne de code dans l’éditeur de texte, dans la fonction ou d’une routine que vous voulez déboguer, puis sélectionnez **point d’arrêt** et **insérer un point d’arrêt**. Le point d'arrêt est ajouté et la ligne de code est affichée en rouge.  
  
6.  Dans le **déboguer** menu, sélectionnez **démarrer le débogage** pour compiler, déployer et tester le projet. Le script de test dans Test.sql est exécuté et l'objet de base de données managé est appelé.  
  
7.  Lorsque la flèche jaune qui désigne le pointeur d'instruction apparaît au niveau du point d'arrêt, l'exécution du code est  suspendue et vous pouvez commencer à déboguer votre objet de base de données managé. Vous pouvez **pas à pas principal** à partir de la **déboguer** menu pour faire avancer le pointeur d’instruction à la ligne suivante de code. Le **variables locales** fenêtre permet d’observer l’état des objets mis en surbrillance par le pointeur d’instruction. Les variables peuvent être ajoutés à la **espion** fenêtre. L'état des variables espionnées peut être consulté tout au long de la session de débogage, et non uniquement lorsque la variable figure dans la ligne de code mise en surbrillance par le pointeur d'instruction. Sélectionnez Continuer dans le menu Déboguer pour faire avancer le pointeur d'instruction jusqu'au point d'arrêt suivant ou pour terminer l'exécution de la routine s'il n'y a plus de points d'arrêt.  
  
### <a name="example"></a>Exemple  
 L'exemple suivant retourne la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'appelant.  
  
 C#  
  
```  
using System;  
using System.Data;  
using System.Data.SqlTypes;  
using System.Data.SqlClient;  
using Microsoft.SqlServer.Server;   
  
public class StoredProcedures   
{  
   [Microsoft.SqlServer.Server.SqlProcedure]  
   public static void GetVersion()  
   {  
   using(SqlConnection connection = new SqlConnection("context connection=true"))   
   {  
      connection.Open();  
      SqlCommand command = new SqlCommand("select @@version",  
                                           connection);  
      SqlContext.Pipe.ExecuteAndSend(command);  
      }  
   }  
}  
```  
  
 Visual Basic  
  
```  
Imports System  
Imports System.Data  
Imports System.Data.Sql  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
Imports System.Data.SqlClient  
  
Partial Public Class StoredProcedures   
    <Microsoft.SqlServer.Server.SqlProcedure> _  
    Public Shared Sub GetVersion()  
        Using connection As New SqlConnection("context connection=true")  
            connection.Open()  
            Dim command As New SqlCommand("SELECT @@VERSION", connection)  
            SqlContext.Pipe.ExecuteAndSend(command)  
        End Using  
    End Sub  
End Class  
```  
  
 Le code suivant est un script de test qui appelle la procédure stockée GetVersion définie précédemment.  
  
```  
EXEC GetVersion  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de programmation pour l’intégration du CLR &#40;Common Language Runtime&#41;](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md)  
  
  
