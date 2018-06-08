---
title: Utilisation d’images à l’aide de la tâche de script | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
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
dev_langs:
- VB
helpviewer_keywords:
- graphics [Integration Services]
- Script task [Integration Services], images
- Drawing namespace
- images [Integration Services]
- SSIS Script task, images
- Script task [Integration Services], examples
- thumbnails [Integration Services]
- System.Drawing namespace
- JPEG format [Integration Services]
- .jpeg files
ms.assetid: 74aeb7ab-51b2-4b9f-84ee-0b46a7908ab9
caps.latest.revision: 42
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 664ac58f2423c1f4c603a32dff3892a3dde3db1c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-images-with-the-script-task"></a>Utilisation d'images à l'aide de la tâche de script
  Une base de données de produits ou d'utilisateurs contient généralement des images, en plus du texte et des données numériques. L’espace de noms **System.Drawing** dans le Microsoft .NET Framework fournit des classes permettant de manipuler des images.  
  
 [Exemple 1 : convertir des images au format JPEG](#example1)  
  
 [Exemple 2 : créer et enregistrer des images miniatures](#example2)  
  
> [!NOTE]  
>  Si vous souhaitez créer une tâche plus facilement réutilisable sur plusieurs packages, envisagez d'utiliser le code indiqué dans l'exemple de tâche de script comme point de départ d'une tâche personnalisée. Pour plus d’informations, consultez [Développement d’une tâche personnalisée](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
##  <a name="example1"></a> Description de l’exemple 1 : convertir des images au format JPEG  
 L'exemple suivant ouvre un fichier image spécifié par une variable et l'enregistre sous la forme d'un fichier JPEG compressé à l'aide d'un encodeur. Le code pour extraire les informations de l'encodeur est encapsulé dans une fonction privée.  
  
#### <a name="to-configure-this-script-task-example-for-use-with-a-single-image-file"></a>Pour configurer cet exemple de tâche de script destiné à un seul fichier image  
  
1.  Créez une variable chaîne appelée `CurrentImageFile` et attribuez lui la valeur du chemin d'accès et du nom d'un fichier image existant.  
  
2.  Dans la page **Script** de l’**Éditeur de tâche de script**, ajoutez la variable `CurrentImageFile` à la propriété **ReadOnlyVariables**.  
  
3.  Dans le projet de script, ajoutez une référence à l’espace de noms **System.Drawing**.  
  
4.  Dans votre code, utilisez des instructions **Imports** pour importer les espaces de noms **System.Drawing** et **System.IO**.  
  
#### <a name="to-configure-this-script-task-example-for-use-with-multiple-image-files"></a>Pour configurer cet exemple de tâche de script destiné à plusieurs fichiers image  
  
1.  Placez la tâche de script dans un conteneur de boucles Foreach.  
  
2.  Dans la page **Collection** de l’**Éditeur de boucle Foreach**, sélectionnez l’**énumérateur Foreach File**, puis spécifiez le chemin et le masque des fichiers sources, par exemple « *.bmp ».  
  
3.  Dans la page **Mappage de variables**, mappez la variable `CurrentImageFile` à l’index 0. Cette variable transmet le nom du fichier en cours à la tâche de script à chaque itération de l'énumérateur.  
  
    > [!NOTE]  
    >  Ces étapes s'ajoutent à celles répertoriées dans la procédure destinée à un seul fichier image.  
  
### <a name="example-1-code"></a>Code de l’exemple 1  
  
```vb  
Public Sub Main()  
  
    'Create and initialize variables.  
    Dim currentFile As String  
    Dim newFile As String  
    Dim bmp As Bitmap  
    Dim eps As New Imaging.EncoderParameters(1)  
    Dim ici As Imaging.ImageCodecInfo  
    Dim supportedExtensions() As String = _  
        {".BMP", ".GIF", ".JPG", ".JPEG", ".EXIF", ".PNG", _  
        ".TIFF", ".TIF", ".ICO", ".ICON"}  
  
    Try  
        'Store the variable in a string for local manipulation.  
        currentFile = Dts.Variables("CurrentImageFile").Value.ToString  
        'Check the extension of the file against a list of  
        'files that the Bitmap class supports.  
        If Array.IndexOf(supportedExtensions, _  
            Path.GetExtension(currentFile).ToUpper) > -1 Then  
  
            'Load the file.  
            bmp = New Bitmap(currentFile)  
  
            'Calculate the new name for the compressed image.  
            'Note: This will overwrite existing JPEGs.  
            newFile = Path.Combine( _  
                Path.GetDirectoryName(currentFile), _  
                String.Concat(Path.GetFileNameWithoutExtension(currentFile), _  
                ".jpg"))  
  
            'Specify the compression ratio (0=worst quality, 100=best quality).  
            eps.Param(0) = New Imaging.EncoderParameter( _  
                Imaging.Encoder.Quality, 75)  
  
            'Retrieve the ImageCodecInfo associated with the jpeg format.  
            ici = GetEncoderInfo("image/jpeg")  
  
            'Save the file, compressing it into the jpeg encoding.  
            bmp.Save(newFile, ici, eps)  
        Else  
            'The file is not supported by the Bitmap class.  
            Dts.Events.FireWarning(0, "Image Resampling Sample", _  
                "File " & currentFile & " is not a supported format.", _  
                "", 0)  
         End If  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        'An error occurred.  
        Dts.Events.FireError(0, "Image Resampling Sample", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
  
Private Function GetEncoderInfo(ByVal mimeType As String) As Imaging.ImageCodecInfo  
  
    'The available image codecs are returned as an array,  
    'which requires code to iterate until the specified codec is found.  
  
    Dim count As Integer  
    Dim encoders() As Imaging.ImageCodecInfo  
  
    encoders = Imaging.ImageCodecInfo.GetImageEncoders()  
  
    For count = 0 To encoders.Length  
        If encoders(count).MimeType = mimeType Then  
            Return encoders(count)  
        End If  
    Next  
  
    'This point is only reached if a codec is not found.  
    Err.Raise(513, "Image Resampling Sample", String.Format( _  
        "The {0} codec is not available. Unable to compress file.", _  
            mimeType))  
    Return Nothing  
  
End Function  
  
```  
  
##  <a name="example2"></a> Description de l’exemple 2 : Créer et enregistrer des images miniatures  
 L'exemple suivant ouvre un fichier image spécifié par une variable, crée une miniature de l'image en respectant les proportions et enregistre la miniature sous un nom de fichier modifié. Le code qui calcule la hauteur et la largeur de la miniature en préservant les proportions est encapsulé dans une sous-routine privée.  
  
#### <a name="to-configure-this-script-task-example-for-use-with-a-single-image-file"></a>Pour configurer cet exemple de tâche de script destiné à un seul fichier image  
  
1.  Créez une variable chaîne appelée `CurrentImageFile` et attribuez lui la valeur du chemin d'accès et du nom d'un fichier image existant.  
  
2.  Créez également la variable de type entier `MaxThumbSize` et attribuez-lui une valeur en pixels, telle que 100.  
  
3.  Dans la page **Script** de l’**Éditeur de tâche de script**, ajoutez les deux variables à la propriété **ReadOnlyVariables**.  
  
4.  Dans le projet de script, ajoutez une référence à l’espace de noms **System.Drawing**.  
  
5.  Dans votre code, utilisez des instructions **Imports** pour importer les espaces de noms **System.Drawing** et **System.IO**.  
  
#### <a name="to-configure-this-script-task-example-for-use-with-multiple-image-files"></a>Pour configurer cet exemple de tâche de script destiné à plusieurs fichiers image  
  
1.  Placez la tâche de script dans un conteneur de boucles Foreach.  
  
2.  Dans la page **Collection** de l’**Éditeur de boucle Foreach**, sélectionnez l’**énumérateur Foreach File** comme **Énumérateur**, puis spécifiez le chemin et le masque des fichiers sources, par exemple « *.jpg ».  
  
3.  Dans la page **Mappage de variables**, mappez la variable `CurrentImageFile` à l’index 0. Cette variable transmet le nom du fichier en cours à la tâche de script à chaque itération de l'énumérateur.  
  
    > [!NOTE]  
    >  Ces étapes s'ajoutent à celles répertoriées dans la procédure destinée à un seul fichier image.  
  
### <a name="example-2-code"></a>Code de l’exemple 2  
  
```vb  
Public Sub Main()  
  
    Dim currentImageFile As String  
    Dim currentImage As Image  
    Dim maxThumbSize As Integer  
    Dim thumbnailImage As Image  
    Dim thumbnailFile As String  
    Dim thumbnailHeight As Integer  
    Dim thumbnailWidth As Integer  
  
    currentImageFile = Dts.Variables("CurrentImageFile").Value.ToString  
    thumbnailFile = Path.Combine( _  
        Path.GetDirectoryName(currentImageFile), _  
        String.Concat(Path.GetFileNameWithoutExtension(currentImageFile), _  
        "_thumbnail.jpg"))  
  
    Try  
        currentImage = Image.FromFile(currentImageFile)  
  
        maxThumbSize = CType(Dts.Variables("MaxThumbSize").Value, Integer)  
        CalculateThumbnailSize( _  
            maxThumbSize, currentImage, thumbnailWidth, thumbnailHeight)  
  
        thumbnailImage = currentImage.GetThumbnailImage( _  
           thumbnailWidth, thumbnailHeight, Nothing, Nothing)  
        thumbnailImage.Save(thumbnailFile)  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        Dts.Events.FireError(0, "Script Task Example", _  
        ex.Message & ControlChars.CrLf & ex.StackTrace, _  
        String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
  
Private Sub CalculateThumbnailSize( _  
    ByVal maxSize As Integer, ByVal sourceImage As Image, _  
    ByRef thumbWidth As Integer, ByRef thumbHeight As Integer)  
  
    If sourceImage.Width > sourceImage.Height Then  
        thumbWidth = maxSize  
        thumbHeight = CInt((maxSize / sourceImage.Width) * sourceImage.Height)  
    Else  
        thumbHeight = maxSize  
        thumbWidth = CInt((maxSize / sourceImage.Height) * sourceImage.Width)  
    End If  
  
End Sub  
```  
  
```csharp  
bool ThumbnailCallback()  
        {  
            return false;  
        }  
        public void Main()  
        {  
  
            string currentImageFile;  
            Image currentImage;  
            int maxThumbSize;  
            Image thumbnailImage;  
            string thumbnailFile;  
            int thumbnailHeight = 0;  
            int thumbnailWidth = 0;  
  
            currentImageFile = Dts.Variables["CurrentImageFile"].Value.ToString();  
            thumbnailFile = Path.Combine(Path.GetDirectoryName(currentImageFile), String.Concat(Path.GetFileNameWithoutExtension(currentImageFile), "_thumbnail.jpg"));  
  
            try  
            {  
  
                currentImage = Image.FromFile(currentImageFile);  
  
                maxThumbSize = (int)Dts.Variables["MaxThumbSize"].Value;  
                CalculateThumbnailSize(maxThumbSize, currentImage, ref thumbnailWidth, ref thumbnailHeight);  
  
                Image.GetThumbnailImageAbort myCallback = new Image.GetThumbnailImageAbort(ThumbnailCallback);  
  
                thumbnailImage = currentImage.GetThumbnailImage(thumbnailWidth, thumbnailHeight, ThumbnailCallback, IntPtr.Zero);  
                thumbnailImage.Save(thumbnailFile);  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                Dts.Events.FireError(0, "Script Task Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
        }  
  
        private void CalculateThumbnailSize(int maxSize, Image sourceImage, ref int thumbWidth, ref int thumbHeight)  
        {  
  
            if (sourceImage.Width > sourceImage.Height)  
            {  
                thumbWidth = maxSize;  
                thumbHeight = (int)(sourceImage.Height * maxSize / sourceImage.Width);  
            }  
            else  
            {  
                thumbHeight = maxSize;  
                thumbWidth = (int)(sourceImage.Width * maxSize / sourceImage.Height);  
  
            }  
  
        }  
  
```  
  
  
