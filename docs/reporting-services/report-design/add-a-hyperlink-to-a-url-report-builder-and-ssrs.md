---
title: Ajouter un lien hypertexte à une URL (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d3392c0b-7b62-4d27-bc04-2bd0c5487d08
caps.latest.revision: 11
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 80fb8d9b4f416933e19931aeb29fdb33f533e9f5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-hyperlink-to-a-url-report-builder-and-ssrs"></a>Ajouter un lien hypertexte à une URL (Générateur de rapports et SSRS)
Découvrez comment ajouter des actions de lien hypertexte à des zones de texte, des images, des graphiques et des jauges aux rapports paginés [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)]  . Les liens peuvent mener à d’autres rapports, aux signets d’un rapport ou à des URL dynamiques ou statiques. 

 Vous pouvez ajouter une action de lien hypertexte à un élément doté d’une propriété **Action** , par exemple une zone de texte, une image ou une série calculée dans un graphique. Lorsque l'utilisateur clique sur l'élément de rapport en question, l'action que vous définissez est exécutée.  
  
*   Vous pouvez **ajouter un lien hypertexte qui ouvre un navigateur avec une URL** que vous spécifiez. Le lien hypertexte peut être une URL statique ou une expression apparentée à une URL. Si une base de données comprend un champ avec des URL, vous pouvez insérer ce champ dans l'expression de façon à produire une liste dynamique de liens hypertexte dans le rapport. Assurez-vous que les lecteurs de votre rapport ont accès à l’URL fournie.  
   
*  Vous pouvez également **spécifier des URL pour créer des extractions** vers des rapports sur n’importe quel serveur de rapports que vos utilisateurs et vous-même avez l’autorisation d’afficher en utilisant des demandes d’URL dirigées vers le serveur de rapports. Par exemple, vous pouvez spécifier un rapport et masquer l'explorateur de documents à l'utilisateur lorsqu'il affiche pour la première fois le rapport. Pour plus d’informations, consultez [Accès URL &#40;SSRS&#41;](../../reporting-services/url-access-ssrs.md) et [Spécification de chemins à des éléments externes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specifying-paths-to-external-items-report-builder-and-ssrs.md).
 
 *  Vous pouvez **ajouter un signet à un emplacement spécifique** dans le même rapport. 
  
Essayez d’ajouter des liens hypertexte avec des exemples de données dans le [didacticiel : mettre en forme du texte &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-format-text-report-builder.md).  
  
> [!NOTE]  
>  Les liens qui sont associés à des champs de dataset peuvent être vulnérables à des opérations de falsification à des fins malveillantes. Pour plus d’informations, consultez [Sécuriser des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md).  
  
## <a name="to-add-a-hyperlink-and"></a>Pour ajouter un lien hypertexte et...   
  
1.  En mode Création de rapport, cliquez avec le bouton droit sur la zone de texte, l’image ou le graphique auquel vous voulez ajouter un lien, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue Propriétés, cliquez sur l’onglet **Action** . Lisez la suite pour plus d’informations sur les options.  

## <a name="-add-drillthrough-to-another-report"></a>... ajouter l’extraction à un autre rapport

1. Dans l’onglet **Action** , sélectionnez **Atteindre le rapport**. 

2. Spécifiez les paramètres et le rapport cible que vous souhaitez utiliser. Les noms des paramètres doivent correspondre aux paramètres définis pour le rapport cible. 

3. Utilisez les boutons **Ajouter** et **Supprimer** pour ajouter et supprimer des paramètres et les flèches de direction pour ordonner la liste de paramètres.

4.  Tapez ou sélectionnez une **valeur** à transmettre pour le paramètre nommé dans le rapport d’extraction. Cliquez sur le bouton Expression (fx) pour modifier l’expression.

5. Sélectionnez **Omettre** pour empêcher le paramètre de s’exécuter. Par défaut, cette case à cocher est désactivée et n'est pas active. Pour cocher la case, cliquez sur le bouton Expression (fx) et tapez True ou créez une expression. La case est cochée quand vous cliquez sur **OK** dans la boîte de dialogue Expression.
  
   Pour plus d’informations, consultez [Ajouter une action d’extraction à un rapport (Générateur de rapports et SSRS)](../../reporting-services/report-design/add-a-drillthrough-action-on-a-report-report-builder-and-ssrs.md) . 
   
6. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
   
## <a name="-add-a-bookmark"></a>...ajouter un signet

Vous pouvez définir un lien vers des signets menant à un emplacement du rapport actuel. Pour définir un lien vers un signet, vous devez d’abord définir la propriété de **signet** d’un élément de rapport. Pour définir la propriété de **signet** , sélectionnez un élément de rapport et, dans le volet Propriétés, tapez une valeur ou expression pour l’identificateur du signet, par exemple, SalesChart ou 5TopSales.

1. Dans l’onglet **Action** , sélectionnez **Atteindre le signet**. 

2. Tapez ou sélectionnez l’identificateur de signet du rapport à atteindre. Cliquez sur le bouton Expression (fx) pour modifier l'expression. 

   L'identificateur du signet peut correspondre à un ID statique ou une expression ayant pour résultat un identificateur du signet. L'expression peut inclure un champ qui contient un identificateur de signet.
   
   Consultez [Ajouter un signet à un rapport](../../reporting-services/report-design/add-a-bookmark-to-a-report-report-builder-and-ssrs.md) pour plus d’informations.
   
3. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="-add-a-hyperlink"></a>...ajouter un lien hypertexte 
  
1. Dans l’onglet **Action** , sélectionnez **Atteindre l’URL**. Une section supplémentaire apparaît dans la boîte de dialogue pour cette option.  
  
4.  Dans **Sélectionner une URL**, entrez ou sélectionnez une URL ou une expression qui prend la valeur d’une URL, ou cliquez sur la flèche déroulante et cliquez sur le nom d’un champ qui contient une URL. 

    Pour un élément publié sur un serveur de rapports configuré pour le mode natif, utilisez un chemin d'accès complet ou relatif. Par exemple, `http://<servername>/images/image1.jpg`. 
    
    Pour un élément publié sur un serveur de rapports configuré en mode intégré SharePoint, utilisez une URL complète. Par exemple, `http://<SharePointservername>/<site>/Documents/images/image1.jpg`.
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="after-you-add-a-hyperlink"></a>Après avoir ajouté un lien hypertexte
  
1.  (Facultatif) Le texte n'est pas mis en forme automatiquement en tant que lien. Pour le texte, il est utile de modifier la couleur et l'effet du texte pour indiquer qu'il s'agit d'un lien. Par exemple, changez la couleur en bleu et l'effet en soulignement dans la section **Police** sous l'onglet Accueil du ruban.  
  
7.  Pour tester le lien, cliquez sur **Exécuter** afin d'afficher l'aperçu du rapport, puis cliquez sur l'élément de rapport sur lequel vous avez défini ce lien.  
  
## <a name="see-also"></a> Voir aussi  
 [Tri interactif, Explorateurs de documents et liens &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md)   
 [Créer un explorateur de documents &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-document-map-report-builder-and-ssrs.md)  
  
  
