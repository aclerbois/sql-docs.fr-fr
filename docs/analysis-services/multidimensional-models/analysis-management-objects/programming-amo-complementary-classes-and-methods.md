---
title: Classes complémentaires AMO et les méthodes de programmation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: db41dcbcd16d6fe9ba6166f82f2939ef284bd4ba
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="programming-amo-complementary-classes-and-methods"></a>Programmation des classes et méthodes complémentaires AMO
  Cette rubrique contient les sections suivantes :  
  
-   [Assembly (classe)](#Assembly)  
  
-   [Sauvegarde et restauration](#BU)  
  
-   [Trace (classe)](#TRC)  
  
-   [Classe CaptureLog et attribut CaptureXML](#CL)  
  
##  <a name="Assembly"></a>Assembly (classe)  
 Assemblys permettent aux utilisateurs d’étendre les fonctionnalités de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] en ajoutant de nouvelles procédures stockées ou les fonctions MDX (Multidimensional Expressions). Pour plus d’informations, consultez [méthodes et Classes autres AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-other-classes-and-methods.md).  
  
 L'ajout et la suppression d'assemblys est simple et peut être effectuée en ligne. Vous devez être administrateur de base de données pour ajouter un assembly à la base de données ou administrateur de serveur pour ajouter l'assembly à l'objet serveur.  
  
 L'exemple suivant ajoute un assembly à la base de données fournie et affecte le compte de service pour exécuter l'assembly. Si l'assembly existe dans la base de données, il est supprimé avant la tentative d'ajout.  
  
```  
static public void CreateStoredProcedures(Database db)  
{  
    ClrAssembly clrAssembly;  
  
    // Verify That assembly exist in database and drop it  
    if (db.Assemblies.ContainsName("StoredProcedures"))  
    {  
        clrAssembly = db.Assemblies.FindByName("StoredProcedures");  
        clrAssembly.Drop();  
    }  
  
    // Create the CLR assembly  
    clrAssembly = db.Assemblies.Add("StoredProcedures");  
    clrAssembly.ImpersonationInfo = new ImpersonationInfo(  
        ImpersonationMode.ImpersonateServiceAccount);  
    clrAssembly.PermissionSet = PermissionSet.Unrestricted;  
  
    // Load the assembly files  
    clrAssembly.LoadFiles(Environment.CurrentDirectory  
        + @"\StoredProcedures2.dll", false);  
  
    clrAssembly.Update();  
}  
  
```  
  
##  <a name="BU"></a>Méthodes de sauvegarde et restauration  
 Les méthodes Backup et Restore permettent aux administrateurs de sauvegarder les bases de données et de les restaurer.  
  
 L'exemple suivant crée des sauvegardes pour toutes les bases de données du serveur spécifié. Si un fichier de sauvegarde existe déjà, il est remplacé. Les fichiers de sauvegarde sont enregistrés dans le dossier BackUp du dossier Data d'[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
```  
static public void BackUpAllDatabases(Server svr)  
{  
    string fileName;  
    if ((svr != null) && ( svr.Connected))  
        foreach (Database db in svr.Databases)  
        {  
            fileName = db.Name + "_" + ((Int64)(DateTime.Today.Year * 10000 + DateTime.Today.Month * 100 + DateTime.Today.Day)).ToString()+ ".abf";  
            db.Backup(fileName, true);  
        }  
}  
```  
  
 L'exemple suivant restaure la sauvegarde « Adventure Works » de l'exemple précédent. Si la base de données « My Aventure WorksDW » existe déjà, la base de données est remplacée.  
  
```  
static public void RestoreAdventureWorks(Server svr)  
{  
    svr.Restore("Adventure Works DW_20051025.abf", "My Adventure WorksDW", true);  
}  
```  
  
##  <a name="TRC"></a>Trace (classe)  
 L'analyse de l'activité serveur nécessite l'utilisation de deux types de traces : les traces de session et les traces de serveur. Le traçage du serveur peut fournir une indication sur le niveau de performance de la tâche en cours sur le serveur (traces de session). De même, les traces peuvent vous renseigner sur l'activité générale du serveur sans même y être connecté (traces de serveur).  
  
 Lors du traçage de l'activité en cours (traces de session), le serveur envoie des notifications à l'application active à propos des événements qui se produisent dans le serveur et qui sont le fait de l'application. Les événements sont capturés au moyen de gestionnaires d'événements dans l'application active. Vous devez en premier lieu affecter les routines de gestion d'événements à l'objet <xref:Microsoft.AnalysisServices.SessionTrace> puis démarrer la trace de session.  
  
 L'exemple ci-dessous montre comment configurer une trace de session de façon à assurer le suivi des activités en cours. Les routines du gestionnaire d'événements, qui sont situées à la fin de l'exemple suivant, transmettront toutes les informations de trace à l'objet System.Console. Pour générer des événements de traçage, l'exemple de cube « Adventure Works » sera entièrement traité après le démarrage de la trace.  
  
```  
static public void TestSessionTraces(Server svr)  
{  
    // Start the default trace  
  
    svr.SessionTrace.OnEvent  
        += new TraceEventHandler(DefaultTrace_OnEvent);  
    svr.SessionTrace.Stopped  
        += new TraceStoppedEventHandler(DefaultTrace_Stopped);  
    svr.SessionTrace.Start();  
  
    // Process the databases  
    // The trace handlers will output all progress notifications   
    // to the console  
    Database db = svr.Databases.FindByName("Adventure Works DW Multidimensional 2012");  
    Cube cube = db.Cubes.FindByName("Adventure Works");  
    cube.Process(ProcessType.ProcessFull);  
  
    // Stop the default trace  
    svr.SessionTrace.Stop();  
  
}  
static public void DefaultTrace_OnEvent(object sender, TraceEventArgs e)  
{  
    Console.WriteLine("{0}", e.TextData);  
}  
  
static public void DefaultTrace_Stopped(ITrace sender, TraceStoppedEventArgs e)  
{  
    switch (e.StopCause)  
    {  
        case TraceStopCause.StoppedByUser:  
        case TraceStopCause.Finished:  
            Console.WriteLine("Processing completed successfully");  
            break;  
  
        case TraceStopCause.StoppedByException:  
            Console.WriteLine("Processing failed: {0}",  
                e.Exception.Message);  
            break;  
    }  
}  
```  
  
 Les traces de serveur peuvent être configurées pour tout consigner dans un fichier de trace et pour redémarrer automatiquement lorsque le service redémarre.  
  
 Pour définir une trace de serveur, vous devez tout d'abord désigner les événements du serveur qu'il convient de surveiller, ainsi que les données de chaque événement qui doivent être enregistrées dans le fichier de trace. Pour chaque événement, vous devez définir les colonnes de données à enregistrer dans le fichier de trace.  
  
 La création d'une trace de serveur s'effectue en quatre étapes :  
  
1.  Créez l'objet de trace de serveur et remplissez les attributs communs de base.  
  
     LogFileSize définit la taille maximale du fichier de trace en mégaoctets ; LogFileRollOver permet au fichier journal de débuter dans un fichier différent si la limite de LogFileSize est atteinte ; dans ce cas, un numéro de séquence est ajouté au nom du fichier ; AutoRestart permet à la trace de reprendre en cas de redémarrage du service.  
  
2.  Créez les événements et les colonnes de données correspondantes.  
  
3.  Démarrez et arrêtez la trace en fonction des besoins.  
  
     Même après la trace a été arrêtée, la trace existe sur le serveur et doit redémarrer si la trace a été définie comme suit : AutoRestart =**true**.  
  
4.  Supprimez la trace lorsqu'elle n'est plus nécessaire.  
  
 Dans l'exemple suivant, si la trace existe déjà, elle est supprimée puis recréée. Les fichiers de trace sont enregistrés dans le dossier Log des dossiers de données [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
```  
static public void TestServerTraces(Server svr)  
{  
    Trace trc;  
    TraceEvent te;  
  
        trc = svr.Traces.FindByName("TestServerTraces");  
        if (trc != null)  
            trc.Drop();  
        trc = svr.Traces.Add("TestServerTraces", "TestServerTraces");  
        trc.LogFileName = ("TestServerTraces_" +((Int64)(DateTime.Now.Year * 10000 + DateTime.Now.Month * 100 + DateTime.Now.Day)).ToString() + "_" +  
                ((Int64)(DateTime.Now.Hour * 10000 + DateTime.Now.Minute * 100 + DateTime.Now.Second)).ToString() + ".trc");  
        trc.LogFileSize = 100;  
        trc.LogFileRollover = true;  
        trc.AutoRestart = false;  
  
        #region Define Events to trace & data columns per event  
        te = trc.Events.Add(TraceEventClass.ProgressReportBegin);  
        te.Columns.Add(TraceColumn.TextData);  
        te.Columns.Add(TraceColumn.StartTime);  
        te.Columns.Add(TraceColumn.ObjectName);  
        te.Columns.Add(TraceColumn.ObjectPath);  
        te.Columns.Add(TraceColumn.DatabaseName);  
        te.Columns.Add(TraceColumn.NTCanonicalUserName);  
  
        te = trc.Events.Add(TraceEventClass.ProgressReportCurrent);  
        te.Columns.Add(TraceColumn.TextData);  
        te.Columns.Add(TraceColumn.CurrentTime);  
        te.Columns.Add(TraceColumn.ObjectName);  
        te.Columns.Add(TraceColumn.ObjectPath);  
        te.Columns.Add(TraceColumn.DatabaseName);  
  
        te = trc.Events.Add(TraceEventClass.ProgressReportEnd);  
        te.Columns.Add(TraceColumn.TextData);  
        te.Columns.Add(TraceColumn.StartTime);  
        te.Columns.Add(TraceColumn.CurrentTime);  
        te.Columns.Add(TraceColumn.EndTime);  
        te.Columns.Add(TraceColumn.Success);  
        te.Columns.Add(TraceColumn.Error);  
        te.Columns.Add(TraceColumn.ObjectName);  
        te.Columns.Add(TraceColumn.ObjectPath);  
        te.Columns.Add(TraceColumn.DatabaseName);  
        te.Columns.Add(TraceColumn.NTCanonicalUserName);  
        #endregion  
  
        trc.Update();  
        trc.Start();  
  
        #region Process the Adventure Works Cube  
        // The trace settings will output all progress notifications   
        // to the trace file  
        Database db = svr.Databases.FindByName("Adventure Works DW Multidimensional 2012");  
        Cube cube = db.Cubes.FindByName("Adventure Works");  
        cube.Process(ProcessType.ProcessFull);  
        #endregion  
  
        trc.Stop();  
        trc.Drop();  
  
}  
```  
  
##  <a name="CL"></a> Attributs CaptureLog et CaptureXml  
 L'attribut CaptureLog vous permet de créer des fichiers de commandes XMLA à partir de vos opérations AMO. CaptureLog vous permet de générer du script pour désigner les objets serveur en tant que bases de données, cubes, dimensions, structures d'exploration de données, entre autres.  
  
 La création d'un attribut CaptureLog passe par les étapes suivantes :  
  
1.  Commencer à capturer le journal XMLA en définissant le serveur attribut CaptureXml à **true**.  
  
     Cette option lancera l'enregistrement de toutes les opérations AMO dans une collection de chaînes au lieu de les envoyer au serveur.  
  
2.  Débutez l'activité AMO comme d'ordinaire, mais ne perdez pas de vue qu'aucune action n'est envoyée au serveur. Une activité peut consister en une opération de traitement, de création, de suppression, de mise à jour ou en toute autre action effectuée sur un objet.  
  
3.  Arrêter la capture du journal XMLA en redéfinissant CaptureXml à **false**.  
  
4.  Examinez le journal XMLA capturé. Pour cela, vous pouvez soit examiner chaque chaîne contenue dans la collection de chaînes CaptureLog, soit générer une chaîne complète avec la méthode ConcatenateCaptureLog. ConcatenateCaptureLog vous permet de générer l'ensemble du traitement XMLA comme une transaction unique et d'y ajouter l'option de traitement parallèle.  
  
 L'exemple suivant retourne une chaîne avec les commandes batch pour effectuer un traitement complet sur toutes les dimensions et sur tous les cubes de la base de données [Adventure Works DW Multidimensional 2012].  
  
```  
static public string TestCaptureLog(Server svr)  
{  
    String capturedXmla = "";  
    if ((svr != null) && (svr.Connected))  
    {  
        svr.CaptureXml = true;  
  
        #region Actions to be captured to an XMLA file  
        //No action is executed during CaptureXml = true  
        Database db = svr.Databases.FindByName("Adventure Works DW Multidimensional 2012");  
        foreach (Dimension dim in db.Dimensions)  
            dim.Process(ProcessType.ProcessFull);  
        foreach (Cube cube in db.Cubes)  
            cube.Process(ProcessType.ProcessFull);  
        #endregion  
  
        svr.CaptureXml = false;  
  
        capturedXmla = svr.ConcatenateCaptureLog(true, true);  
  
    }  
  
    return capturedXmla;  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [AMO autres Classes et méthodes](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-other-classes-and-methods.md)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)  
  
  
