---
title: Détection d’un fichier plat vide à l’aide de la tâche de script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting-task-examples
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- flat files
- Script task [Integration Services], empty flat files
- SSIS Script task, empty flat files
- Script task [Integration Services], examples
ms.assetid: 1b4defb8-886a-483d-8056-d1b91d37bc90
caps.latest.revision: 32
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ef17791f7bbd34977a50820dec1424ff8e8de66e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="detecting-an-empty-flat-file-with-the-script-task"></a>Détection d'un fichier plat vide à l'aide de la tâche de script
  La source de fichier plat ne détermine pas si un fichier plat contient des lignes de données avant de tenter de les traiter. Vous pouvez améliorer l'efficacité d'un package, notamment d'un package qui parcourt de nombreux fichiers plats, en ignorant les fichiers qui ne contiennent aucune ligne de données. La tâche de script peut rechercher un fichier plat vide avant que le package ne commence à traiter le flux de données.  
  
> [!NOTE]  
>  Si vous souhaitez créer une tâche plus facilement réutilisable sur plusieurs packages, envisagez d'utiliser le code indiqué dans l'exemple de tâche de script comme point de départ d'une tâche personnalisée. Pour plus d’informations, consultez [Développement d’une tâche personnalisée](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>Description  
 L’exemple suivant utilise des méthodes de l’espace de noms **System.IO** pour tester le fichier plat spécifié dans un gestionnaire de connexions de fichiers plats afin de déterminer si le fichier est vide ou s’il contient uniquement les lignes attendues ne correspondant pas à des données, telles que des en-têtes de colonnes ou des lignes vides. Le script vérifie d'abord la taille du fichier ; une taille de zéro octets indique que le fichier est vide. Si la taille du fichier est supérieure à zéro, le script lit les lignes du fichier jusqu'à la dernière, ou jusqu'à ce que le nombre de lignes dépasse le nombre attendu de lignes ne correspondant pas à des données. Si le nombre de lignes dans le fichier est inférieur ou égal au nombre attendu de lignes ne correspondant pas à des données, le fichier est considéré comme vide. Le résultat est retourné sous la forme d'une valeur booléenne dans une variable utilisateur, dont la valeur peut être utilisée pour le branchement du flux de contrôle du package. La méthode **FireInformation** affiche également le résultat dans la fenêtre **Output** de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA).  
  
#### <a name="to-configure-this-script-task-example"></a>Pour configurer cet exemple de tâche de script  
  
1.  Créez et configurez un gestionnaire de connexions de fichiers plats nommé **EmptyFlatFileTest**.  
  
2.  Créez une variable de type entier appelée `FFNonDataRows` et attribuez-lui une valeur égale au nombre attendu de lignes ne correspondant pas à des données dans le fichier plat.  
  
3.  Créez une variable booléenne appelée `FFIsEmpty`.  
  
4.  Ajoutez la variable `FFNonDataRows` à la propriété **ReadOnlyVariables** de la tâche de script.  
  
5.  Ajoutez la variable `FFIsEmpty` à la propriété **ReadWriteVariables** de la tâche de script.  
  
6.  Dans votre code, importez l’espace de noms **System.IO**.  
  
 Si vous parcourez les fichiers avec un énumérateur Foreach File, au lieu d'utiliser un seul gestionnaire de connexions de fichiers plats, vous devez modifier l'exemple de code ci-dessous pour obtenir le nom et le chemin d'accès du fichier à partir de la variable dans laquelle la valeur énumérée est stockée et non à partir du gestionnaire de connexions.  
  
### <a name="code"></a>Code  
  
```vb  
Public Sub Main()  
  
  Dim nonDataRows As Integer = _  
      DirectCast(Dts.Variables("FFNonDataRows").Value, Integer)  
  Dim ffConnection As String = _  
      DirectCast(Dts.Connections("EmptyFlatFileTest").AcquireConnection(Nothing), _  
      String)  
  Dim flatFileInfo As New FileInfo(ffConnection)  
  ' If file size is 0 bytes, flat file does not contain data.  
  Dim fileSize As Long = flatFileInfo.Length  
  If fileSize > 0 Then  
    Dim lineCount As Integer = 0  
    Dim line As String  
    Dim fsFlatFile As New StreamReader(ffConnection)  
    Do Until fsFlatFile.EndOfStream  
      line = fsFlatFile.ReadLine  
      lineCount += 1  
      ' If line count > expected number of non-data rows,  
      '  flat file contains data (default value).  
      If lineCount > nonDataRows Then  
        Exit Do  
      End If  
      ' If line count <= expected number of non-data rows,  
      '  flat file does not contain data.  
      If lineCount <= nonDataRows Then  
        Dts.Variables("FFIsEmpty").Value = True  
      End If  
    Loop  
  Else  
    Dts.Variables("FFIsEmpty").Value = True  
  End If  
  
  Dim fireAgain As Boolean = False  
  Dts.Events.FireInformation(0, "Script Task", _  
      String.Format("{0}: {1}", ffConnection, _  
      Dts.Variables("FFIsEmpty").Value.ToString), _  
      String.Empty, 0, fireAgain)  
  
  Dts.TaskResult = ScriptResults.Success  
  
End Sub  
```  
  
```csharp  
public void Main()  
        {  
  
            int nonDataRows = (int)(Dts.Variables["FFNonDataRows"].Value);  
            string ffConnection = (string)(Dts.Connections["EmptyFlatFileTest"].AcquireConnection(null) as String);  
            FileInfo flatFileInfo = new FileInfo(ffConnection);  
            // If file size is 0 bytes, flat file does not contain data.  
            long fileSize = flatFileInfo.Length;  
            if (fileSize > 0)  
            {  
  
                int lineCount = 0;  
                string line;  
                StreamReader fsFlatFile = new StreamReader(ffConnection);  
                while (!(fsFlatFile.EndOfStream))  
                {  
                    Console.WriteLine (fsFlatFile.ReadLine());  
                    lineCount += 1;  
                    // If line count > expected number of non-data rows,  
                    //  flat file contains data (default value).  
                    if (lineCount > nonDataRows)  
                    {  
                        break;  
                    }  
                    // If line count <= expected number of non-data rows,  
                    //  flat file does not contain data.  
                    if (lineCount <= nonDataRows)  
                    {  
                        Dts.Variables["FFIsEmpty"].Value = true;  
                    }  
                }  
            }  
            else  
            {  
                Dts.Variables["FFIsEmpty"].Value = true;  
            }  
  
            bool fireAgain = false;  
            Dts.Events.FireInformation(0, "Script Task", String.Format("{0}: {1}", ffConnection, Dts.Variables["FFIsEmpty"].Value), String.Empty, 0, ref fireAgain);  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
  
        }  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Exemples de tâche de script](../../integration-services/extending-packages-scripting-task-examples/script-task-examples.md)  
  
  
