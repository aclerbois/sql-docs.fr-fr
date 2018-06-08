---
title: Incorporer une tâche de profilage des données dans le flux de travail du package | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling task [Integration Services], using output in workflow
ms.assetid: 39a51586-6977-4c45-b80b-0157a54ad510
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7dfb25f4472479d4e477ceaad65f17ba0b1558a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="incorporate-a-data-profiling-task-in-package-workflow"></a>Incorporer une tâche de profilage des données dans le flux de travail du package
  Le profilage des données et le nettoyage des données ne sont pas des candidats pour un processus automatisé à leur stade initial. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], la sortie de la tâche de profilage des données doit habituellement faire l’objet d’une analyse visuelle et d’un jugement personnel pour qu’il soit déterminé si les violations signalées sont significatives ou excessives. Même après avoir reconnu des problèmes de qualité des données, un plan soigneusement pensé doit être appliqué pour déterminer la meilleure approche pour le nettoyage.  
  
 Toutefois, après avoir établi des critères de qualité des données, vous pouvez souhaiter automatiser une analyse et un nettoyage périodiques de la source de données. Considérez les scénarios suivants :  
  
-   **Vérification de la qualité des données avant une charge incrémentielle**. Utilisez la tâche de profilage des données pour calculer le profil de ratio Null de la colonne de nouvelles données prévues pour la colonne CustomerName dans un tableau Customers. Si le pourcentage de valeurs NULL est supérieur à 20 %, envoyez un message électronique qui contient la sortie de profil à l'opérateur et terminez le package. Dans le cas contraire, continuez la charge incrémentielle.  
  
-   **Automatisation du nettoyage lorsque les conditions spécifiées sont satisfaites**. Utilisez la tâche de profilage des données pour calculer le profil d'inclusion de valeur de la colonne State (État) par rapport à une table de recherche d'états, et de la colonne ZIP Code/Postal Code (Code postal) par rapport à une table de recherche de codes postaux. Si la puissance d'inclusion des valeurs d'état est inférieure à 80 %, mais que la puissance d'inclusion des valeurs de code postal est supérieure à 99 %, cela indique deux choses. En premier lieu, les données d'état sont erronées. En second lieu, les données de code postal sont correctes. Lancez une tâche de flux de données qui nettoie les données d'état en effectuant une recherche de la valeur d'état correcte à partir de la valeur de code postal actuelle.  
  
 Une fois que vous avez un flux de travail dans lequel vous pouvez incorporer la tâche de flux de données, vous devez comprendre les étapes requises pour ajouter cette tâche. La section suivante décrit le processus général d'incorporation de la tâche de flux de données. Les deux sections finales décrivent comment connecter la tâche de flux de données, soit directement à une source de données, soit à des données transformées à partir du flux de données.  
  
## <a name="defining-a-general-workflow-for-the-data-flow-task"></a>Définition d'un flux de travail général pour la tâche de flux de données  
 La procédure décrite ci-dessous esquisse l'approche générale permettant d'utiliser la sortie de la tâche de profilage des données dans le flux de travail d'un package.  
  
#### <a name="to-use-the-output-of-the-data-profiling-task-programmatically-in-a-package"></a>Pour utiliser par programmation la sortie de la tâche de profilage des données dans un package  
  
1.  Ajoutez et configurez la tâche de profilage des données dans un package.  
  
2.  Configurez des variables de package qui contiendront les valeurs que vous souhaitez extraire des résultats du profil.  
  
3.  Ajoutez et configurez une tâche de script. Connectez la tâche de script à la tâche de profilage des données. Dans la tâche de script, écrivez un code pour lire les valeurs souhaitées dans le fichier de sortie de la tâche de profilage des données et remplir les variables de package.  
  
4.  Dans les contraintes de précédence qui connectent la tâche de script aux branches situées en aval dans le flux de travail, écrivez des expressions qui utilisent les valeurs des variables pour diriger le flux de travail.  
  
 Lorsque vous incorporez la tâche de profilage des données dans le flux de travail d'un package, gardez à l'esprit les deux fonctionnalités suivantes de la tâche :  
  
-   **Sortie de la tâche**. La tâche de profilage des données écrit sa sortie dans un fichier ou une variable de package au format XML selon le schéma DataProfile.xsd. Par conséquent, vous devez interroger la sortie XML si vous souhaitez utiliser les résultats du profil dans le flux de travail conditionnel d'un package. Vous pouvez facilement utiliser le langage de requête Xpath pour interroger cette sortie XML. Pour étudier la structure de cette sortie XML, vous pouvez ouvrir un fichier de sortie exemple ou le schéma lui-même. Pour ouvrir le fichier de sortie ou le schéma, vous pouvez utiliser [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], un autre éditeur XML ou un éditeur de texte, tel que le Bloc-notes.  
  
    > [!NOTE]  
    >  Certains résultats du profil, affichés dans la visionneuse du profil des données, sont des valeurs calculées qui n'ont pas été trouvées directement dans la sortie. Par exemple, la sortie du profil de ratio Null de la colonne contient le nombre total de lignes et le nombre de lignes qui contiennent des valeurs Null. Vous devez effectuer une requête sur ces deux valeurs, puis calculer le pourcentage des lignes qui contiennent des valeurs Null, pour obtenir le ratio Null de la colonne.  
  
-   **Entrée de la tâche**. La tâche de profilage des données lit son entrée à partir des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par conséquent, vous devez enregistrer les données en mémoire dans des tables intermédiaires si vous voulez profiler les données qui ont déjà été chargées et transformées dans le flux de données.  
  
 Les sections suivantes appliquent ce flux de travail général pour profiler les données qui proviennent directement d'une source de données externe ou qui proviennent transformées de la tâche de flux de données. Ces sections indiquent également comment gérer les conditions préalables d'entrée et de sortie de la tâche de flux de données.  
  
## <a name="connecting-the-data-profiling-task-directly-to-an-external-data-source"></a>Connexion de la tâche de profilage des données directement à une source de données externe  
 La tâche de profilage des données peut profiler des données qui proviennent directement d'une source de données.  Pour illustrer cette fonction, l'exemple suivant utilise la tâche de profilage des données pour calculer un profil de ratio Null de colonne sur les colonnes de la table Person.Address dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] . Ensuite, cet exemple utilise une tâche de script pour extraire les résultats du fichier de sortie et remplir les variables de package qui peuvent être utilisées pour diriger le flux de travail.  
  
> [!NOTE]  
>  La colonne AddressLine2 a été sélectionnée pour cet exemple simple, car elle contient un pourcentage élevé de valeurs Null.  
  
 Cet exemple inclut les étapes suivantes :  
  
-   Configuration des gestionnaires de connexions qui se connectent à la source de données externe et au fichier de sortie qui contiendra les résultats du profil.  
  
-   Configuration des variables de package qui contiendront les valeurs requises par la tâche de profilage des données.  
  
-   Configuration de la tâche de profilage des données permettant de calculer le profil de ratio Null de la colonne.  
  
-   Configuration de la tâche de script qui utilisera la sortie XML de la tâche de profilage des données.  
  
-   Configuration des contraintes de précédence qui contrôleront quelles branches situées en aval dans le flux de travail seront exécutées, en fonction des résultats de la tâche de profilage des données.  
  
### <a name="configure-the-connection-managers"></a>Configurer les gestionnaires de connexions  
 Pour cet exemple, deux gestionnaires de connexions sont utilisés :  
  
-   Un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] qui se connecte à la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
-   Un gestionnaire de connexions de fichiers qui crée le fichier de sortie qui contiendra les résultats de la tâche de profilage des données.  
  
##### <a name="to-configure-the-connection-managers"></a>Pour configurer les gestionnaires de connexions  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], créez un nouveau package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Ajoutez un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] au package. Configurez ce gestionnaire de connexions pour qu’il utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) et se connecte à une instance disponible de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
     Par défaut, le gestionnaire de connexions porte le nom suivant : \<nom_serveur>.AdventureWorks1.  
  
3.  Ajoutez un gestionnaire de connexions de fichiers au package. Configurez ce gestionnaire de connexions pour créer le fichier de sortie pour la tâche de profilage des données.  
  
     Cet exemple utilise le nom de fichier, DataProfile1.xml. Par défaut, le gestionnaire de connexions a le même nom que le fichier.  
  
### <a name="configure-the-package-variables"></a>Configurer les variables de package  
 Cet exemple utilise deux variables de package :  
  
-   La variable ProfileConnectionName transmet le nom du gestionnaire de connexions de fichiers à la tâche de script.  
  
-   La variable AddressLine2NullRatio transmet au package le ratio de valeurs Null calculé pour cette colonne à partir de la tâche de script.  
  
##### <a name="to-configure-the-package-variables-that-will-hold-profile-results"></a>Pour configurer les variables de package qui contiendront les résultats du profil  
  
-   Dans la fenêtre **Variables** , ajoutez et configurez les deux variables de package suivantes :  
  
    -   Entrez le nom **ProfileConnectionName**pour l’une des variables et définissez le type de cette variable en spécifiant **String**.  
  
    -   Entrez le nom **AddressLine2NullRatio**pour l’autre variable et définissez le type de cette variable en spécifiant **Double**.  
  
### <a name="configure-the-data-profiling-task"></a>Configurer la tâche de profilage des données  
 La tâche de profilage des données doit être configurée de la façon suivante :  
  
-   Pour utiliser les données que le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] fournit comme entrée.  
  
-   Pour effectuer une opération de profil de ratio Null de la colonne sur les données d'entrée.  
  
-   Pour enregistrer les résultats du profil dans le fichier associé au gestionnaire de connexions de fichiers.  
  
##### <a name="to-configure-the-data-profiling-task"></a>Pour configurer la tâche de profilage des données  
  
1.  Ajoutez une tâche de profilage des données au flux de contrôle.  
  
2.  Ouvrez l' **Éditeur de tâche de profilage de données** pour configurer la tâche.  
  
3.  Dans la page **Général** de l'éditeur, pour **Destination**, sélectionnez le nom du gestionnaire de connexions de fichiers que vous avez configuré précédemment.  
  
4.  Dans la page **Demandes de profil** de l'éditeur, créez un nouveau profil de ratio Null de la colonne.  
  
5.  Dans le volet **Propriétés de la demande** , pour **ConnectionManager**, sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] que vous avez configuré précédemment. Ensuite, pour **TableOrView**, sélectionnez Person.Address.  
  
6.  Fermez l'Éditeur de tâche de profilage de données.  
  
### <a name="configure-the-script-task"></a>Configurer la tâche de script  
 La tâche de script doit être configurée pour extraire les résultats du fichier de sortie et remplir les variables de package configurées précédemment.  
  
##### <a name="to-configure-the-script-task"></a>Pour configurer la tâche de script  
  
1.  Ajoutez une tâche de script au flux de contrôle.  
  
2.  Connectez la tâche de script à la tâche de profilage des données.  
  
3.  Ouvrez l' **éditeur de tâche de script** pour configurer la tâche.  
  
4.  Dans la page **Script** , sélectionnez votre langage de programmation préféré. Ensuite, rendez les deux variables de package disponibles pour le script:  
  
    1.  Pour **ReadOnlyVariables**, sélectionnez **ProfileConnectionName**.  
  
    2.  Pour **ReadWriteVariables**, sélectionnez **AddressLine2NullRatio**.  
  
5.  Sélectionnez **Modifier le script** pour ouvrir l'environnement de développement de script.  
  
6.  Ajoutez une référence à l'espace de noms System.Xml.  
  
7.  Entrez l'exemple de code qui correspond à votre langage de programmation :  
  
    ```vb  
    Imports System  
    Imports Microsoft.SqlServer.Dts.Runtime  
    Imports System.Xml  
  
    Public Class ScriptMain  
  
      Private FILENAME As String = "C:\ TEMP\DataProfile1.xml"  
      Private PROFILE_NAMESPACE_URI As String = "http://schemas.microsoft.com/DataDebugger/"  
      Private NULLCOUNT_XPATH As String = _  
        "/default:DataProfile/default:DataProfileOutput/default:Profiles" & _  
        "/default:ColumnNullRatioProfile[default:Column[@Name='AddressLine2']]/default:NullCount/text()"  
      Private TABLE_XPATH As String = _  
        "/default:DataProfile/default:DataProfileOutput/default:Profiles" & _  
        "/default:ColumnNullRatioProfile[default:Column[@Name='AddressLine2']]/default:Table"  
  
      Public Sub Main()  
  
        Dim profileConnectionName As String  
        Dim profilePath As String  
        Dim profileOutput As New XmlDocument  
        Dim profileNSM As XmlNamespaceManager  
        Dim nullCountNode As XmlNode  
        Dim nullCount As Integer  
        Dim tableNode As XmlNode  
        Dim rowCount As Integer  
        Dim nullRatio As Double  
  
        ' Open output file.  
        profileConnectionName = Dts.Variables("ProfileConnectionName").Value.ToString()  
        profilePath = Dts.Connections(profileConnectionName).ConnectionString  
        profileOutput.Load(profilePath)  
        profileNSM = New XmlNamespaceManager(profileOutput.NameTable)  
        profileNSM.AddNamespace("default", PROFILE_NAMESPACE_URI)  
  
        ' Get null count for column.  
        nullCountNode = profileOutput.SelectSingleNode(NULLCOUNT_XPATH, profileNSM)  
        nullCount = CType(nullCountNode.Value, Integer)  
  
        ' Get row count for table.  
        tableNode = profileOutput.SelectSingleNode(TABLE_XPATH, profileNSM)  
        rowCount = CType(tableNode.Attributes("RowCount").Value, Integer)  
  
        ' Compute and return null ratio.  
        nullRatio = nullCount / rowCount  
        Dts.Variables("AddressLine2NullRatio").Value = nullRatio  
  
        Dts.TaskResult = Dts.Results.Success  
  
      End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System;  
    using Microsoft.SqlServer.Dts.Runtime;  
    using System.Xml;  
  
    public class ScriptMain  
    {  
  
      private string FILENAME = "C:\\ TEMP\\DataProfile1.xml";  
      private string PROFILE_NAMESPACE_URI = "http://schemas.microsoft.com/DataDebugger/";  
      private string NULLCOUNT_XPATH = "/default:DataProfile/default:DataProfileOutput/default:Profiles" + "/default:ColumnNullRatioProfile[default:Column[@Name='AddressLine2']]/default:NullCount/text()";  
      private string TABLE_XPATH = "/default:DataProfile/default:DataProfileOutput/default:Profiles" + "/default:ColumnNullRatioProfile[default:Column[@Name='AddressLine2']]/default:Table";  
  
      public void Main()  
      {  
  
        string profileConnectionName;  
        string profilePath;  
        XmlDocument profileOutput = new XmlDocument();  
        XmlNamespaceManager profileNSM;  
        XmlNode nullCountNode;  
        int nullCount;  
        XmlNode tableNode;  
        int rowCount;  
        double nullRatio;  
  
        // Open output file.  
        profileConnectionName = Dts.Variables["ProfileConnectionName"].Value.ToString();  
        profilePath = Dts.Connections[profileConnectionName].ConnectionString;  
        profileOutput.Load(profilePath);  
        profileNSM = new XmlNamespaceManager(profileOutput.NameTable);  
        profileNSM.AddNamespace("default", PROFILE_NAMESPACE_URI);  
  
        // Get null count for column.  
        nullCountNode = profileOutput.SelectSingleNode(NULLCOUNT_XPATH, profileNSM);  
        nullCount = (int)nullCountNode.Value;  
  
        // Get row count for table.  
        tableNode = profileOutput.SelectSingleNode(TABLE_XPATH, profileNSM);  
        rowCount = (int)tableNode.Attributes["RowCount"].Value;  
  
        // Compute and return null ratio.  
        nullRatio = nullCount / rowCount;  
        Dts.Variables["AddressLine2NullRatio"].Value = nullRatio;  
  
        Dts.TaskResult = Dts.Results.Success;  
  
      }  
  
    }  
    ```  
  
    > [!NOTE]  
    >  L'exemple de code indiqué dans cette procédure montre comment charger la sortie de la tâche de profilage des données à partir d'un fichier. Pour charger la sortie de la tâche de profilage des données à partir d'une variable de package, reportez-vous à l'exemple de code alternatif qui suit cette procédure.  
  
8.  Fermez l'environnement de développement de script, puis l'éditeur de tâche de script.  
  
#### <a name="alternative-codereading-the-profile-output-from-a-variable"></a>Code alternatif – Lecture de la sortie du profil à partir d'une variable  
 La procédure précédente montre comment charger la sortie de la tâche de profilage des données à partir d'un fichier. Toutefois, une méthode alternative consiste à charger cette sortie à partir d'une variable de package. Pour charger la sortie à partir d'une variable, vous devez apporter les modifications suivantes dans l'exemple de code :  
  
-   Appelez la méthode **LoadXml** de la classe **XmlDocument** à la place de la méthode **Load** .  
  
-   Dans l'éditeur de tâche de script, ajoutez le nom de la variable de package qui contient la sortie du profil dans la liste **ReadOnlyVariables** de la tâche.  
  
-   Passez la valeur de chaîne de la variable à la méthode **LoadXML** , comme indiqué dans l'exemple de code suivant. (Cet exemple utilise "ProfileOutput" comme nom de la variable de package qui contient la sortie du profil.)  
  
    ```vb  
    Dim outputString As String  
    outputString = Dts.Variables("ProfileOutput").Value.ToString()  
    ...  
    profileOutput.LoadXml(outputString)  
    ```  
  
    ```csharp  
    string outputString;  
    outputString = Dts.Variables["ProfileOutput"].Value.ToString();  
    ...  
    profileOutput.LoadXml(outputString);  
    ```  
  
### <a name="configure-the-precedence-constraints"></a>Configurer les contraintes de précédence  
 Il convient de configurer les contraintes de précédence pour contrôler quelles branches situées en aval dans le flux de travail seront exécutées, en fonction des résultats de la tâche de profilage des données.  
  
##### <a name="to-configure-the-precedence-constraints"></a>Pour configurer les contraintes de précédence  
  
-   Dans les contraintes de précédence qui connectent la tâche de script aux branches situées en aval dans le flux de travail, écrivez des expressions qui utilisent les valeurs des variables pour diriger le flux de travail.  
  
     Par exemple, vous pouvez définir l' **Opération d'évaluation** de la contrainte de précédence en spécifiant **Expression et contrainte**. Ensuite, vous pouvez utiliser `@AddressLine2NullRatio < .90` comme valeur de l'expression. En conséquence, le flux de travail suit le chemin d'accès sélectionné lorsque les tâches précédentes réussissent et lorsque le pourcentage de valeurs NULL dans la colonne sélectionnée est inférieur à 90 %.  
  
## <a name="connecting-the-data-profiling-task-to-transformed-data-from-the-data-flow"></a>Connexion de la tâche de profilage des données aux données transformées à partir du flux de données  
 Au lieu de profiler directement des données à partir d'une source de données, vous pouvez profiler des données qui ont déjà été chargées et transformées dans le flux de données. Toutefois, la tâche de profilage des données fonctionne uniquement par rapport à des données persistantes et non pas par rapport à des données en mémoire. Par conséquent, vous devez en premier lieu utiliser un composant de destination pour enregistrer les données transformées dans une table intermédiaire.  
  
> [!NOTE]  
>  Lorsque vous configurez la tâche de profilage des données, vous devez sélectionner des tables et des colonnes existantes. Par conséquent, vous devez créer la table intermédiaire au moment de la conception avant de pouvoir configurer la tâche. En d'autres termes, ce scénario ne vous permet pas d'utiliser une table temporaire créée au moment de l'exécution.  
  
 Après avoir enregistré les données dans une table intermédiaire, vous pouvez effectuer les actions suivantes :  
  
-   utiliser la tâche de profilage des données pour profiler les données ;  
  
-   utiliser une tâche de script pour lire les résultats comme cela a été décrit précédemment dans cette rubrique ;  
  
-   utiliser ces résultats pour diriger le flux de travail suivant du package.  
  
 La procédure décrite ci-dessous fournit l'approche générale de l'utilisation de la tâche de profilage des données pour profiler des données qui ont été transformées par le flux de données. Un grand nombre de ces étapes sont semblables à celles décrites précédemment pour le profilage de données qui proviennent directement d'une source de données externe. Vous pouvez réexaminer ces étapes précédentes pour obtenir plus d'informations sur la façon de configurer les différents composants.  
  
#### <a name="to-use-the-data-profiling-task-in-the-data-flow"></a>Pour utiliser la tâche de profilage des données dans le flux de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], créez un package.  
  
2.  Dans le flux de données, ajoutez, configurez et connectez les sources et les transformations appropriées.  
  
3.  Dans le flux de données, ajoutez, configurez et connectez un composant de destination qui enregistre les données transformées dans une table intermédiaire.  
  
4.  Dans le flux de contrôle, ajoutez et configurez une tâche de profilage des données qui calcule les profils souhaités par rapport aux données transformées dans la table intermédiaire. Connectez la tâche de profilage des données à la tâche de flux de données.  
  
5.  Configurez des variables de package qui contiendront les valeurs que vous souhaitez extraire des résultats du profil.  
  
6.  Ajoutez et configurez une tâche de script. Connectez la tâche de script à la tâche de profilage des données. Dans la tâche de script, écrivez un code pour lire les valeurs souhaitées dans la sortie de la tâche de profilage des données et remplir les variables de package.  
  
7.  Dans les contraintes de précédence qui connectent la tâche de script aux branches situées en aval dans le flux de travail, écrivez des expressions qui utilisent les valeurs des variables pour diriger le flux de travail.  
  
## <a name="see-also"></a> Voir aussi  
 [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)   
 [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md)  
  
  
