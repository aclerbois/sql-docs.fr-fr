---
title: "Étape 3 : Ajout de redirection de flux d'erreurs | Microsoft Docs"
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 5683a45d-9e73-4cd5-83ca-fae8b26b488c
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: eb4d16cfa53445fe95a406a8c7c55b80244bd319
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-4-3---adding-error-flow-redirection"></a>Leçon 4-3 : Ajout de redirection de flux d’erreurs
Comme nous l'avons démontré au cours de la tâche précédente, la transformation Lookup Currency Key ne peut pas générer de correspondance lorsqu'elle tente de traiter le fichier plat exemple endommagé qui a généré une erreur. Du fait que la transformation utilise les paramètres par défaut pour l'affichage en sortie des erreurs, toute erreur qui survient entraîne un échec de la transformation. Si la transformation échoue, le reste du package échoue également.  
  
Pour éviter tout échec de la transformation, vous pouvez configurer le composant afin qu'il réachemine la ligne qui a échoué vers un autre chemin de traitement à l'aide de la sortie d'erreur. L'utilisation d'un chemin de traitement des erreurs distinct vous permet d'accomplir un certain nombre de choses. Par exemple, vous pouvez essayer de nettoyer les données puis de retraiter la ligne qui a échoué. Ou vous pouvez enregistrer la ligne qui a échoué avec d'autres informations d'erreur, pour la vérifier et la retraiter ultérieurement.  
  
Au cours de cette tâche, vous allez configurer la transformation Lookup Currency Key afin qu'elle réachemine toutes les lignes échouant lors de la sortie d'erreur. Dans la branche d'erreur du flux de données, ces lignes sont enregistrées dans un fichier.  
  
Par défaut, les deux colonnes supplémentaires d'une sortie d'erreur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , **ErrorCode** et **ErrorColumn**, contiennent uniquement des codes numériques identifiant un numéro d'erreur et l'ID de la colonne dans laquelle l'erreur est survenue. L'usage de ces valeurs numériques peut être limité sans la description d'erreur correspondante.  
  
Pour améliorer l'utilité de la sortie d'erreur, vous allez utiliser un composant Script pour accéder à l'API [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et obtenir une description de l'erreur avant que le package n'enregistre les lignes ayant échoué dans le fichier.  
  
## <a name="to-configure-an-error-output"></a>Pour configurer un affichage des erreurs  
  
1.  Dans la **Boîte à outils SSIS**, développez **Commun**, puis faites glisser **Composant Script** dans l'aire de conception de l'onglet **Flux de données** . Placez le composant **Script** à droite de la transformation **Lookup Currency Key** .  
  
2.  Dans la boîte de dialogue **Sélectionner le type de composant de script** , cliquez sur **Transformation**, puis sur **OK**.  
  
3.  Cliquez sur la transformation **Lookup Currency Key** et faites glisser la flèche rouge vers la transformation **Script** que vous venez d'ajouter pour connecter les deux composants.  
  
    La flèche rouge représente la sortie d'erreur de la transformation **Lookup Currency Key** . En utilisant la flèche rouge pour connecter la transformation avec le composant Script, vous pouvez rediriger toutes les erreurs de traitement vers le composant Script, lequel traite ensuite les erreurs et les transmet au composant de destination.  
  
4.  Dans la colonne **Erreur** de la boîte de dialogue **Configurer la sortie d'erreur** , sélectionnez **Réacheminer la ligne**, puis cliquez sur **OK**.  
  
5.  Sur l’aire de conception **Flux de données** , cliquez sur **Composant Script** dans le **Composant Script**que vous venez d’ajouter, puis remplacez le nom par **Get Error Description**.  
  
6.  Double-cliquez sur la transformation **Get Error Description** .  
  
7.  Dans la boîte de dialogue **Éditeur de transformation de script** , dans la page **Colonnes d'entrée** , sélectionnez la colonne **ErrorCode** .  
  
8.  Dans la page **Entrées et sorties** , développez **Sortie 0**, cliquez sur **Colonnes de sortie**, puis sur **Ajouter une colonne**.  
  
9. Dans la propriété **Name** , tapez **ErrorDescription** et attribuez à la propriété **DataType** la valeur **Unicode string [DT_WSTR]**.  
  
10. Dans la page **Script** , vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)**.  
  
11. Cliquez sur **Modifier le script** pour ouvrir [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA). Dans la méthode **Input0_ProcessInputRow** , tapez ou collez le code suivant.  
  
    [Visual Basic]  
  
    ```vb  
    Row.ErrorDescription =   
      Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
    ```  
  
    [Visual C#]  
  
    ```cs
    Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
    ```  
  
    La sous-routine terminée doit se présenter comme le code ci-dessous.  
  
    [Visual Basic]  
  
    ```vb
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
      Row.ErrorDescription =   
        Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
  
    End Sub  
    ```  
  
    [Visual C#]  
  
    ```cs
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
        {  
  
            Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
  
        }  
    ```  
  
12. Dans le menu **Générer** , cliquez sur **Générer la solution** pour créer le script et enregistrer vos modifications, puis fermez VSTA.  
  
13. Cliquez sur **OK** pour fermer la boîte de dialogue **Éditeur de transformation de script** .  
  
## <a name="next-steps"></a>Next Steps  
[Étape 4 : Ajout d'une destination de fichier plat](../integration-services/lesson-4-4-adding-a-flat-file-destination.md)  
  
  
  
