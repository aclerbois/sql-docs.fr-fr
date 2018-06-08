---
title: Création d’une transformation asynchrone à l’aide du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting-data-flow-script-component-types
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
- asynchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: 0d814404-21e4-4a68-894c-96fa47ab25ae
caps.latest.revision: 63
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8ee7d74e217d252cf668c98dba3aaa9cc00bd49e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="creating-an-asynchronous-transformation-with-the-script-component"></a>Création d'une transformation asynchrone à l'aide du composant Script
  Vous utilisez un composant de transformation dans le flux de données d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour modifier et analyser les données acheminées de la source à la destination. Une transformation à sorties synchrones traite chacune des lignes d'entrée lorsqu'elles traversent le composant. Une transformation à sorties asynchrones peut attendre d’avoir reçu toutes les lignes d’entrée avant de procéder au traitement des données, ou elle peut exporter certaines lignes avant d’avoir reçu toutes les lignes d’entrée. Cette rubrique examine une transformation asynchrone. Si votre traitement requiert une transformation synchrone, consultez [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md). Pour plus d’informations sur la différence entre les composants synchrones et asynchrones, consultez [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Pour une vue d’ensemble du composant Script, consultez [Extension du flux de données avec le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Le composant Script et le code d'infrastructure qu'il génère simplifient le processus qui consiste à développer un composant de flux de données personnalisé. Toutefois, pour comprendre le fonctionnement du composant Script, il peut être utile de lire les étapes que vous devez suivre pour développer un composant de flux de données personnalisé dans la section [Développement d’un composant de flux de données personnalisé](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) et plus particulièrement dans [Développement d’un composant de transformation personnalisé à sorties synchrones](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md).  
  
## <a name="getting-started-with-an-asynchronous-transformation-component"></a>Mise en route d'un composant de transformation asynchrone  
 Lorsque vous ajoutez un composant Script à l’onglet Flux de données du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)], la boîte de dialogue **Sélectionner le type de composant de script** s’affiche et vous invite à préconfigurer le composant en tant que source, transformation ou destination. Dans cette boîte de dialogue, sélectionnez **Transformation**.  
  
## <a name="configuring-an-asynchronous-transformation-component-in-metadata-design-mode"></a>Configuration d'un composant de transformation asynchrone en mode Création de métadonnées  
 Après avoir sélectionné l’option de création d’un composant de transformation, configurez le composant dans l’**Éditeur de transformation de script**. Pour plus d’informations, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Pour sélectionner le langage de script qui sera utilisé par le composant Script, vous devez définir la propriété **ScriptLanguage** dans la page **Script** de la boîte de dialogue **Éditeur de transformation de script**.  
  
> [!NOTE]  
>  Pour définir le langage de script par défaut du composant Script, utilisez l’option **Langage de script** dans la page **Général** de la boîte de dialogue **Options**. Pour plus d'informations, consultez [General Page](https://msdn.microsoft.com/library/ms189436(v=sql.110).aspx).  
  
 Un composant de transformation de flux de données possède une entrée et prend en charge une ou plusieurs sorties. La configuration de l’entrée et des sorties du composant est l’une des étapes à exécuter en mode Création des métadonnées, à l’aide de l’**Éditeur de transformation de script**, avant d’écrire le script personnalisé.  
  
### <a name="configuring-input-columns"></a>Configuration de colonnes d'entrée  
 Un composant de transformation créé à l'aide du composant Script possède une seule entrée.  
  
 Dans la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**, la liste des colonnes affiche les colonnes disponibles dans la sortie du composant en amont du flux de données. Sélectionnez les colonnes à transformer ou à passer. Marquez toutes les colonnes que vous souhaitez transformer sur place comme accessibles en lecture/écriture.  
  
 Pour plus d’informations sur la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Colonnes d’entrée&#41;](../../integration-services/data-flow/transformations/script-transformation-editor-input-columns-page.md).  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>Configuration des entrées, des sorties et des colonnes de sortie  
 Un composant de transformation prend en charge une ou plusieurs sorties.  
  
 Généralement, une transformation à sorties asynchrones possède deux sorties. Par exemple, lorsque vous comptez le nombre d'adresses situées dans une ville spécifique, vous pouvez transférer les données d'adresse à une sortie, tout en envoyant le résultat de l'agrégation à une autre sortie. La sortie d'agrégation requiert également une nouvelle colonne de sortie.  
  
 Dans la page **Entrées et sorties** de l’**Éditeur de transformation de script**, vous pouvez noter qu’une seule sortie a été créée par défaut, mais qu’aucune colonne de sortie n’a été créée. Dans cette page de l'éditeur, vous pouvez configurer les éléments suivants :  
  
-   Vous pouvez créer une ou plusieurs sorties supplémentaires, telles qu'une sortie pour le résultat d'une agrégation. Les boutons **Ajouter une sortie** et **Supprimer une sortie** permettent de gérer les sorties du composant de transformation asynchrone. Attribuez la valeur zéro à la propriété **SynchronousInputID** de chaque sortie pour indiquer que la sortie ne fait pas que transférer les données depuis un composant en amont ou les transformer sur place dans les lignes et colonnes existantes. C'est ce paramètre qui rend les sorties asynchrones par rapport à l'entrée.  
  
-   Vous pouvez assigner un nom convivial à l'entrée et aux sorties. Le composant Script utilise ces noms pour générer les propriétés d'accesseur typées qui vous permettent de référencer l'entrée et les sorties dans le script.  
  
-   Généralement, une transformation asynchrone ajoute des colonnes au flux de données. Lorsque la valeur zéro est attribuée à la propriété **SynchronousInputID** d’une sortie, ce qui indique que la sortie ne fait pas que transférer les données depuis un composant en amont ou les transformer sur place dans les lignes et colonnes existantes, vous devez ajouter et configurer des colonnes de sortie de manière explicite sur la sortie. Les colonnes de sortie ne portent pas le même nom que les colonnes d'entrée auxquelles elles sont mappées.  
  
-   Vous pouvez ajouter d'autres colonnes pour contenir des informations supplémentaires. Vous devez écrire votre propre code pour remplir les colonnes supplémentaires de données. Pour plus d’informations sur la reproduction du comportement d’une sortie d’erreur standard, consultez [Simulation d’une sortie d’erreur pour le composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Pour plus d’informations sur la page **Entrées et sorties** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Entrées et sorties&#41;](../../integration-services/data-flow/transformations/script-transformation-editor-inputs-and-outputs-page.md).  
  
### <a name="adding-variables"></a>Ajout de variables  
 S’il existe des variables dont vous souhaitez utiliser les valeurs dans votre script, vous pouvez les ajouter dans les champs de propriété ReadOnlyVariables et ReadWriteVariables de la page **Script** de l’**Éditeur de transformation de script**.  
  
 Lorsque vous ajoutez plusieurs variables dans les champs de propriété, séparez les noms de variables par des virgules. Vous pouvez également sélectionner plusieurs variables en cliquant sur le bouton de sélection (**…**) en regard des champs de propriété **ReadOnlyVariables** et **ReadWriteVariables**, puis en sélectionnant les variables dans la boîte de dialogue **Sélectionner des variables**.  
  
 Pour obtenir des informations générales sur l’utilisation de variables avec le composant Script, consultez [Utilisation de variables dans le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Pour plus d’informations sur la page **Script** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Script&#41;](../../integration-services/data-flow/transformations/script-transformation-editor-script-page.md).  
  
## <a name="scripting-an-asynchronous-transformation-component-in-code-design-mode"></a>Script d'un composant de transformation asynchrone en mode Création de code  
 Après avoir configuré toutes les métadonnées du composant, vous pouvez écrire votre script personnalisé. Dans l’**Éditeur de transformation de script**, dans la page **Script**, cliquez sur **Modifier le script** pour ouvrir l’environnement de développement intégré [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) où vous pouvez ajouter votre script personnalisé. Le langage de script utilisé varie selon que vous avez sélectionné [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# comme langage de script pour la propriété **ScriptLanguage** dans la page **Script**.  
  
 Pour obtenir des informations importantes concernant tous les types de composants créés à l’aide du composant Script, consultez [Codage et débogage du composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Fonctionnement du code généré automatiquement  
 Lorsque vous ouvrez l’environnement de développement intégré VSTA après avoir créé et configuré un composant de transformation, la classe **ScriptMain** modifiable apparaît dans l’éditeur de code avec les stubs des méthodes ProcessInputRow et the CreateNewOutputRows. La classe ScriptMain est l’emplacement où vous allez écrire votre code personnalisé et ProcessInputRow est la méthode la plus importante d’un composant de transformation. La méthode **CreateNewOutputRows**, plus généralement utilisée dans un composant source, s’apparente à une transformation asynchrone dans la mesure où les deux composants doivent créer leurs propres lignes de sortie.  
  
 Si vous ouvrez la fenêtre VSTA **Explorateur de projets**, vous constatez que le composant Script a également généré des éléments de projet **BufferWrapper** et **ComponentWrapper** en lecture seule. La classe ScriptMain hérite de la classe UserComponent dans l’élément de projet **ComponentWrapper**.  
  
 Au moment de l’exécution, le moteur de flux de données appelle la méthode PrimeOutput dans la classe **UserComponent**, qui remplace la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PrimeOutput%2A> de la classe parente <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. La méthode PrimeOutput appelle à son tour la méthode CreateNewOutputRows.  
  
 Le moteur de flux de données appelle ensuite la méthode ProcessInput dans la classe UserComponent, qui remplace la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> de la classe parente <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. La méthode ProcessInput parcourt à son tour les lignes du tampon d’entrée et appelle la méthode ProcessInputRow une fois pour chaque ligne.  
  
### <a name="writing-your-custom-code"></a>Écriture du code personnalisé  
 Pour achever la création d’un composant de transformation asynchrone personnalisé, vous devez appliquer la méthode ProcessInputRow remplacée pour traiter les données dans chaque ligne de la mémoire tampon d’entrée. Comme les sorties ne sont pas synchrones avec l'entrée, vous devez écrire explicitement des lignes de données dans les sorties.  
  
 Dans une transformation asynchrone, vous pouvez utiliser la méthode AddRow pour ajouter des lignes à la sortie appropriée à partir des méthodes ProcessInputRow ou ProcessInput. Il est inutile d’utiliser la méthode CreateNewOutputRows. Si vous écrivez une seule ligne de résultats, comme des résultats d’agrégation, dans une sortie particulière, vous pouvez créer au préalable la ligne de sortie à l’aide de la méthode CreateNewOutputRows et spécifier ses valeurs ultérieurement après avoir traité toutes les lignes d’entrée. Toutefois, il est inutile de créer plusieurs lignes dans la méthode CreateNewOutputRows car le composant Script ne vous permet d’utiliser que la ligne en cours dans une entrée ou une sortie. La méthode CreateNewOutputRows est plus importante dans un composant source où il n’existe pas de ligne d’entrée à traiter.  
  
 Vous pouvez également remplacer la méthode ProcessInput elle-même, afin d’effectuer d’autres traitements préliminaires ou finaux avant ou après avoir parcouru la mémoire tampon d’entrée et appelé la méthode ProcessInputRow pour chaque ligne. Par exemple, l’un des exemples de code de cette rubrique remplace la méthode ProcessInput afin de compter le nombre d’adresses dans une ville pendant que ProcessInputRow parcourt les lignes **.** L’exemple écrit la valeur de synthèse dans la deuxième sortie une fois que toutes les lignes ont été traitées. L’exemple exécute l’opération de sortie dans ProcessInput car les mémoires tampons de sortie ne sont plus disponibles lorsque la méthode PostExecute est appelée.  
  
 Selon vos besoins, vous voudrez également écrire le script dans les méthodes PreExecute et PostExecute disponibles dans la classe ScriptMain pour effectuer tout traitement préliminaire ou final.  
  
> [!NOTE]  
>  Si vous développez un composant de flux de données personnalisé entièrement nouveau, il est important de remplacer la méthode PrimeOutput pour mettre en cache les références aux mémoires tampons de sortie afin de pouvoir ajouter des lignes de données aux mémoires tampons ultérieurement. Dans le composant Script, cette opération n’est pas nécessaire car vous disposez d’une classe générée automatiquement qui représente chaque mémoire tampon de sortie dans l’élément de projet **BufferWrapper**.  
  
## <a name="example"></a> Exemple  
 Cet exemple présente le code personnalisé requis dans la classe ScriptMain pour créer un composant de transformation asynchrone.  
  
> [!NOTE]  
>  Ces exemples utilisent la table **Person.Address** de l’exemple de base de données **AdventureWorks** et passent ses première et quatrième colonnes (à savoir les colonnes **intAddressID** et **nvarchar(30)City**) dans le flux de données. Les mêmes données sont utilisées dans les exemples de source, transformation et destination de cette section. Des conditions préalables et des hypothèses supplémentaires sont documentées pour chaque exemple.  
  
 Cet exemple présente un composant de transformation asynchrone à deux sorties. Cette transformation transfère les colonnes **AddressID** et **City** dans une sortie, tout en comptant le nombre d’adresses situées dans une ville spécifique (Redmond, Washington, États-Unis), puis elle exporte la valeur obtenue vers une deuxième sortie.  
  
 Si vous souhaitez exécuter cet exemple de code, vous devez configurer le package et le composant comme suit :  
  
1.  Ajoutez un nouveau composant Script à l'aire du concepteur de flux de données et configurez-le en tant que transformation.  
  
2.  Connectez la sortie d'une source ou d'une autre transformation au nouveau composant de transformation dans le concepteur. Cette sortie doit fournir les données de la table **Person.Address** de l’exemple de base de données **AdventureWorks** qui contient au moins les colonnes **AddressID** et **City**.  
  
3.  Ouvrez l' **Éditeur de transformation de script**. Dans la page **Colonnes d’entrée**, sélectionnez les colonnes **AddressID** et **City**.  
  
4.  Dans la page **Entrées et sorties**, ajoutez et configurez les colonnes de sortie **AddressID** et **City** sur la première sortie. Ajoutez une deuxième sortie, puis ajoutez sur cette dernière une colonne de sortie pour la valeur résumée. Attribuez la valeur 0 à la propriété SynchronousInputID de la première sortie, car cet exemple copie explicitement chaque ligne d'entrée vers la première sortie. La propriété SynchronousInputID de la sortie récemment créée a déjà la valeur 0.  
  
5.  Renommez l'entrée, les sorties et la nouvelle colonne de sortie avec des noms plus descriptifs. L’exemple utilise le nom **MyAddressInput** pour l’entrée, les noms **MyAddressOutput** et **MySummaryOutput** pour les sorties et le nom **MyRedmondCount** pour la colonne de sortie sur la deuxième sortie.  
  
6.  Dans la page **Script** , cliquez sur **Modifier le script** , puis entrez le script suivant. Ensuite, fermez l'environnement de développement de script et l' **Éditeur de transformation de script**.  
  
7.  Créez et configurez un composant de destination pour la première sortie qui attend les colonnes **AddressID** et **City**, comme une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l’exemple de composant de destination présenté dans [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Connectez ensuite la première sortie de la transformation, **MyAddressOutput**, au composant de destination. Vous pouvez créer une table de destination en exécutant la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante dans la base de données **AdventureWorks** :  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
8.  Créez et configurez un autre composant de destination pour la deuxième sortie. Puis connectez la deuxième sortie de la transformation, **MySummaryOutput**, au composant de destination. Comme la deuxième sortie écrit une seule ligne avec une seule valeur, vous pouvez configurer facilement une destination à l'aide d'un gestionnaire de connexions de fichiers plats qui se connecte à un nouveau fichier contenant une seule colonne. Dans l’exemple, cette colonne de destination est nommée **MyRedmondCount**.  
  
9. Exécutez l'exemple.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Private myRedmondAddressCount As Integer  
  
    Public Overrides Sub CreateNewOutputRows()  
  
        MySummaryOutputBuffer.AddRow()  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInput(ByVal Buffer As MyAddressInputBuffer)  
  
        While Buffer.NextRow()  
            MyAddressInput_ProcessInputRow(Buffer)  
        End While  
  
        If Buffer.EndOfRowset Then  
            MyAddressOutputBuffer.SetEndOfRowset()  
            MySummaryOutputBuffer.MyRedmondCount = myRedmondAddressCount  
            MySummaryOutputBuffer.SetEndOfRowset()  
        End If  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        With MyAddressOutputBuffer  
            .AddRow()  
            .AddressID = Row.AddressID  
            .City = Row.City  
        End With  
  
        If Row.City.ToUpper = "REDMOND" Then  
            myRedmondAddressCount += 1  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    private int myRedmondAddressCount;  
  
    public override void CreateNewOutputRows()  
    {  
  
        MySummaryOutputBuffer.AddRow();  
  
    }  
  
    public override void MyAddressInput_ProcessInput(MyAddressInputBuffer Buffer)  
    {  
  
        while (Buffer.NextRow())  
        {  
            MyAddressInput_ProcessInputRow(Buffer);  
        }  
  
        if (Buffer.EndOfRowset())  
        {  
            MyAddressOutputBuffer.SetEndOfRowset();  
            MySummaryOutputBuffer.MyRedmondCount = myRedmondAddressCount;  
            MySummaryOutputBuffer.SetEndOfRowset();  
        }  
  
    }  
  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        {  
            MyAddressOutputBuffer.AddRow();  
            MyAddressOutputBuffer.AddressID = Row.AddressID;  
            MyAddressOutputBuffer.City = Row.City;  
        }  
  
        if (Row.City.ToUpper() == "REDMOND")  
        {  
            myRedmondAddressCount += 1;  
        }  
  
    }  
  
}  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
 [Développement d’un composant de transformation personnalisé avec des sorties asynchrones](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)  
  
  
