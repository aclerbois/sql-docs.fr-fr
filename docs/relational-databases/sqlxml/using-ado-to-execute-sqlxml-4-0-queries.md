---
title: À l’aide d’ADO pour exécuter SQLXML 4.0 interroge | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- query testers [SQLXML]
- test scripts
- ADO [SQLXML]
- queries [SQLXML], ADO
- SQLXML, ADO
ms.assetid: 3d54e3bb-7c5f-427e-82f8-1403a54c4f53
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6ba44ff764f9adf8cc6b27f5ad298d8ebb5ad2c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-ado-to-execute-sqlxml-40-queries"></a>Utilisation d'ADO pour exécuter des requêtes SQLXML 4.0
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Dans les versions antérieures de SQLXML, l'exécution de requêtes basées sur le protocole HTTP était prise en charge à l'aide de répertoires virtuels IIS SQLXML et du filtre ISAPI SQLXML. Dans SQLXML 4.0, ces composants ont été supprimés du fait que des fonctionnalités semblables et se chevauchant sont fournies avec les services Web XML natifs à compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 Vous pouvez également exécuter des requêtes et utiliser SQLXML 4.0 avec vos applications COM en exploitant les extensions SQLXML aux objets ADO (ActiveX Data Objects) qui ont été introduites dans Microsoft Data Access Components (MDAC) 2.6 et versions ultérieures.  
  
 Cette rubrique illustre l’utilisation de SQLXML et ADO dans le cadre d’une application Visual Basic Scripting Edition (VBScript) (il s’agit d’un script avec l’extension de nom de fichier .vbs). Elle fournit des procédures d'installation initiale pour vous aider à recréer et à tester des exemples de requête dans la documentation SQLXML 4.0.  
  
## <a name="creating-the-sqlxml-40-test-script"></a>Création du script de test SQLXML 4.0  
 Dans cette procédure, vous créez un fichier VBScript (.vbs), Sqlxml4test.vbs, qui peut être utilisé pour exécuter des requêtes SQLXML en exploitant les extensions ADO SQLXML dans ADO 2.6 et versions ultérieures.  
  
#### <a name="to-create-the-sqlxml-40-query-tester-using-ado-vbscript"></a>Pour créer le testeur de requêtes SQLXML 4.0 à l'aide d'ADO (VBScript)  
  
1.  Copiez le code ci-dessous et collez-le dans un fichier texte. Enregistrez le fichier sous Sqlxml4test.vbs.  
  
    ```  
    WScript.Echo "Query process may take a few seconds to complete. Please be patient."  
  
    ' Note that for SQL Server Native Client to be used as the data provider,  
    ' it needs to be installed on the client computer first. Also, SQLXML extensions   
    ' for ADO are used and available in MDAC 2.6 or later.  
  
    'Set script variables.  
    inputFile = "@@FILE_NAME@@"  
    strServer = "@@SERVER_NAME@@"  
    strDatabase = "@@DATABASE_NAME@@"  
    dbGuid = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  
    ' Establish ADO connection to SQL Server and   
    ' create an instance of the ADO Command object.  
    Set conn = CreateObject("ADODB.Connection")  
    Set cmd = CreateObject("ADODB.Command")  
    conn.Open "Provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Server=" & strServer & _  
              ";Database=" & strDatabase & ";Integrated Security=SSPI"  
    Set cmd.ActiveConnection = conn  
  
    ' Create the input stream as an instance of the ADO Stream object.  
    Set inStream = CreateObject("ADODB.Stream")  
    inStream.Open  
    inStream.Charset = "utf-8"  
    inStream.LoadFromFile inputFile  
  
    ' Set ADO Command instance to use input stream.  
    Set cmd.CommandStream = inStream  
  
    ' Set the command dialect.  
    cmd.Dialect = dbGuid  
  
    ' Set a second ADO Stream instance for use as a results stream.   
    Set outStream = CreateObject("ADODB.Stream")  
    outStream.Open  
  
    ' Set dynamic properties used by the SQLXML ADO command instance.   
    cmd.Properties("XML Root").Value = "ROOT"  
    cmd.Properties("Output Encoding").Value = "UTF-8"  
  
    ' Connect the results stream to the command instance and execute the command.  
    cmd.Properties("Output Stream").Value = outStream  
    cmd.Execute , , 1024  
  
    ' Echo cropped/partial results to console.  
    WScript.Echo Left(outStream.ReadText, 1023)  
  
    inStream.Close  
    outStream.Close  
    ```  
  
2.  Mettez à jour les valeurs de script suivantes pour l'exemple que vous essayez de tester et votre environnement de test.  
  
    -   Recherchez "`@@FILE_NAME@@`" et remplacez-le par le nom de votre fichier modèle.  
  
    -   Recherchez "`@@SERVER_NAME@@`" et remplacez-le par le nom de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, "`(local)`" si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute localement).  
  
    -   Recherchez "`@@DATABASE_NAME@@`" et remplacez-le par le nom de la base de données (par exemple "`AdventureWorks2012`" ou "`tempdb`").  
  
     Mettez à jour toute autre valeur mentionnée dans les instructions spécifiques pour l'exemple que vous essayez de recréer localement sur votre ordinateur.  
  
3.  Enregistrez le fichier et fermez-le.  
  
4.  Vérifiez que vous avez créé tous les fichiers supplémentaires, tels que des modèles ou des schémas XML qui font partie de l'exemple que vous essayez de recréer localement sur votre ordinateur. Ces fichiers doivent se trouver dans le répertoire où vous avez enregistré le fichier script de test (Sqlxml4test.vbs).  
  
5.  Suivez les instructions dans la section suivante pour savoir comment utiliser le script de test SQLXML 4.0.  
  
## <a name="using-the-sqlxml-40-test-script"></a>Utilisation du script de test SQLXML 4.0  
 La procédure suivante décrit comment utiliser les fichiers Sqlxml4test.vbs pour tester les exemples de requêtes fournis dans cette documentation.  
  
#### <a name="to-use-the-sqlxml-40-query-tester"></a>Pour utiliser le testeur de requêtes SQLXML 4.0  
  
1.  Vérifiez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est installé, comme suit :  
  
    1.  À partir de la **Démarrer** menu, pointez sur **paramètres**, puis cliquez sur **le panneau de configuration**.  
  
    2.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**  
  
    3.  Dans la liste des programmes actuellement installés, vérifiez que **Microsoft SQL Server Native Client** apparaît dans la liste.  
  
        > [!NOTE]  
        >  Si vous devez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, consultez [l’installation de SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md).  
  
2.  Vérifiez que MDAC 2.6 ou version ultérieure est installée sur l'ordinateur client. Si vous avez besoin de vérifier les informations de version de MDAC, vous pouvez télécharger gratuitement l'outil MDAC Component Checker sur le site Web de Microsoft (www.microsoft.com). Pour plus d'informations, recherchez « MDAC Component Checker » sur le site Web de Microsoft.  
  
3.  Exécutez le script.  
  
     Vous pouvez exécuter le fichier VBScript à la ligne de commande à l'aide de Cscript.exe ou en double-cliquant sur le fichier Sqlxml4test.vbs pour appeler l'environnement d'exécution de scripts WSH (Windows Script Host) (WScript.exe).  
  
     Lorsqu'il est exécuté, le script doit afficher un message pour vous alerter que l'exécution du script peut prendre quelques instants avant de retourner et d'afficher les résultats de la requête en tant que sortie de script. Quand la sortie s'affiche, comparez son contenu aux résultats attendus pour l'exemple.  
  
  
