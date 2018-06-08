---
title: Personnalisation du portail web | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 6f4893d2044676078d102cbd776de03815b8354e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="branding-the-web-portal"></a>Personnalisation du portail web

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)])

Vous pouvez modifier l'apparence du portail web en le personnalisant à l’image de votre entreprise. Cette opération s'effectue via un package de marque. Vous pouvez créer ce package de marque même si vous n’avez les connaissances approfondies requises en matière de feuille de style en cascade (CSS).  
  
<iframe width="560" height="315" src="https://www.youtube.com/embed/m08kLuofwFA?list=PLv2BtOtLblH3F--8WmK9QcLbx6dV_lVkL" frameborder="0" allowfullscreen></iframe>  
   
## <a name="creating-the-brand-package"></a>Création du package de marque
  
Un package de marque pour Reporting Services se compose de trois éléments, et il est empaqueté comme un fichier .zip.   
  
- color.json  
- metadata.xml  
- logo.png (facultatif)  
  
Les fichiers doivent afficher les noms ci-dessus. Le fichier zip peut être nommé comme vous le souhaitez.  
  
### <a name="metadataxml"></a>metadata.xml
  
Le fichier metadata.xml vous permet de définir le nom du package de marque et comporte une entrée de référence pour vos deux fichiers logo.png et colors.json.  
  
Pour modifier le nom de votre package de marque, modifiez l’attribut **nom** de l’élément **SystemResourcePackage** .  
  
    name="Multicolored example brand"  
  
Vous pouvez également inclure un logo dans votre package de marque. Cet élément est répertorié dans l'élément Contents.  
  
Exemple sans fichier de logo.  
  
    <Contents>  
      <Item key="colors" path="colors.json" />  
    </Contents>  
  
Exemple avec un fichier de logo.  
  
    <Contents>  
      <Item key="colors" path="colors.json" />  
      <Item key="logo" path="logo.png" />  
    </Contents>  
  
### <a name="colorsjson"></a>Colors.json
  
Lorsque le package de marque est chargé, le serveur extrait les paires nom/valeur appropriées à partir du fichier colors.json et les fusionne avec la feuille de style LESS master, brand.less. Ce fichier LESS est ensuite traité et le fichier CSS qui en résulte est transmis au client. Toutes les couleurs de la feuille de style suivent la représentation hexadécimale à six caractères d'une couleur.  
  
La feuille de style LESS contient des blocs qui font référence à certaines variables LESS prédéfinies comme suit.  
  
    /* primary buttons */   
    .btn-primary {   
        color:@primaryButtonColor;   
        background-color:@primaryButtonBg;   
    }  
  
Bien que cela ressemble à la syntaxe CSS, les valeurs de couleur, précédées du symbole @symbol, sont spécifiques à LESS. Il s’agit de variables dont les valeurs sont définies par le fichier json.  
  
Par exemple, si le fichier colors.json a les valeurs suivantes.  
  
    "primary":"#009900",   
    "primaryContrast":"#ffffff"   
  
Le résultat traité recherche la variable LESS **@primaryButtonBg** et vérifie qu'elle correspond à la propriété json appelée **primary**, soit #009900 dans cet exemple. Il affiche donc la feuille CSS correcte.  
  
    .btn-primary {   
        color:#ffffff;   
        background-color:#009900;   
    }  
  
Tous les boutons principaux apparaissent en texte blanc sur fond vert foncé.  
  
Le fichier colors.json, pour Reporting Services, comporte deux catégories principales regroupant les éléments.  
  
- **Interface**: inclut les éléments spécifiques au portail web de Reporting Services.  
- **Thème**: inclut les éléments spécifiques aux rapports mobiles que vous créez.  
  
La section Interface est divisée selon les regroupements suivants.  
  
|Section|Description|  
|---|---|  
|primary|Couleurs du bouton et du curseur.|  
|Secondary|Barre de titre, barre de recherche, menu de gauche (si affiché) et couleur du texte de ces éléments.|  
|Principal neutre|Arrière-plans de la page d’accueil et des rapports.|  
|Neutre secondaire|Arrière-plans de la zone de texte et des options de dossier, et menu des paramètres.|  
|Neutre tertiaire|Arrière-plans des paramètres du site.|  
|Messages de danger/avertissement/réussite|Couleurs de ces messages.|  
|Indicateur de performance clé|Contrôle les couleurs des états bon (1), neutre (0), neutre (-1) et aucun.|  
  
La première fois que vous vous connectez à un serveur avec l'Éditeur de rapports mobiles, sur lequel un package de marque a été déployé, le thème sera ajouté aux thèmes disponibles que vous pouvez utiliser dans le menu supérieur droit de l'application.  
  
![ssRSBrandingMobileReportPublisher](../reporting-services/media/ssrsbrandingmobilereportpublisher.png)  
  
Vous pouvez ensuite utiliser ce thème pour les rapports mobiles que vous créez, même s’ils ne sont pas destinés au même serveur sur lequel vous avez déployé le thème.   
  
### <a name="using-a-logo"></a>Utilisation d’un logo
  
Si vous incluez un logo avec votre package de marque, ce logo apparaît dans le portail web à la place de celui que vous définissez pour le portail web dans le menu Paramètres du site.  
  
Le fichier que vous incluez comme logo doit utiliser le format de fichier PNG. Une fois chargées sur le serveur, les dimensions du fichier seront mises à l'échelle. Ces dimensions devraient avoisiner 290 x 60 pixels.  
   
## <a name="applying-the-brand-package-to-the-web-portal"></a>Application du package de marque au portail web
  
Pour ajouter, télécharger ou supprimer un package de marque, vous pouvez procédez comme suit.  
  
1.  Sélectionnez l’icône en forme d’ **engrenage** dans le coin supérieur droit.  
  
2.  Sélectionnez **Paramètres du site**.  
  
    ![ssRSGearMenu](../reporting-services/media/ssrsgearmenu.png)  
  
3.  Sélectionnez **Personnalisation**.  
  
    ![ssRSBranding](../reporting-services/media/ssrsbranding.png)  
  
L’option**Package de marque actuellement installé** affiche le nom du package qui a été chargé, ou aucun nom (None).  
  
L’option**Charger le package de marque** appliquera le package au le portail web La modification est immédiatement appliquée.  
  
Vous pouvez également **télécharger** ou **supprimer** le package. La suppression du package réinitialisera immédiatement le portail web à la marque par défaut.  
  
## <a name="metadataxml-example"></a>Exemple de fichier metadata.xml
  
    <?xml version="1.0" encoding="utf-8"?>  
    <SystemResourcePackage xmlns="http://schemas.microsoft.com/sqlserver/reporting/2016/01/systemresourcepackagemetadata"  
        type="UniversalBrand"  
        version="2.0.2"  
        name="Multicolored example brand"  
        >  
        <Contents>  
            <Item key="colors" path="colors.json" />  
            <Item key="logo" path="logo.png" />  
        </Contents>  
    </SystemResourcePackage>  
   
## <a name="colorsjson-example"></a>Exemple de fichier colors.json
  
    {  
        "name":"Multicolored example brand",  
        "version":"1.0",  
        "interface":{  
            "primary":"#b31e1e",  
            "primaryAlt":"#ca0806",  
            "primaryAlt2":"#621013",  
            "primaryAlt3":"#e40000",  
            "primaryAlt4":"#e14e50",  
            "primaryContrast":"#fff",  
  
            "secondary":"#042200",  
            "secondaryAlt":"#0f4400",  
            "secondaryAlt2":"#155500",  
            "secondaryAlt3":"#217700",  
            "secondaryContrast":"#49e63c",  
  
            "neutralPrimary":"#d8edff",  
            "neutralPrimaryAlt":"#c9e6ff",  
            "neutralPrimaryAlt2":"#aedaff",  
            "neutralPrimaryAlt3":"#88c8ff",  
            "neutralPrimaryContrast":"#0a2b4c",  
  
            "neutralSecondary":"#e9d8eb",  
            "neutralSecondaryAlt":"#d9badc",  
            "neutralSecondaryAlt2":"#b06cb5",  
            "neutralSecondaryAlt3":"#a75bac",  
            "neutralSecondaryContrast":"#250a26",  
  
            "neutralTertiary":"#f79220",  
            "neutralTertiaryAlt":"#f8a54b",  
            "neutralTertiaryAlt2":"#facc9b",  
            "neutralTertiaryAlt3":"#fce3c7",  
            "neutralTertiaryContrast":"#391d00",  
  
            "danger":"#ff0000",  
            "success":"#00ff00",  
            "warning":"#ff8800",  
            "info":"#00ff",  
            "dangerContrast":"#fff",  
            "successContrast":"#fff",  
            "warningContrast":"#fff",  
            "infoContrast":"#fff",  
  
            "kpiGood":"#4fb443",  
            "kpiBad":"#de061a",  
            "kpiNeutral":"#d9b42c",  
            "kpiNone":"#333",  
            "kpiGoodContrast":"#fff",  
            "kpiBadContrast":"#fff",  
            "kpiNeutralContrast":"#fff",  
            "kpiNoneContrast":"#fff"  
           },  
           "theme":{  
            "dataPoints":[  
                "#0072c6",  
                "#f68c1f",  
                "#269657",  
                "#dd5900",  
                "#5b3573",  
                "#22bdef",  
                "#b4009e",  
                "#008274",  
                "#fdc336",  
                "#ea3c00",  
                "#00188f",  
                "#9f9f9f"  
            ],  
  
            "good":"#85ba00",  
            "bad":"#e90000",  
            "neutral":"#edb327",  
            "none":"#333",  
  
            "background":"#fff",  
            "foreground":"#222",  
            "mapBase":"#00aeef",  
            "panelBackground":"#f6f6f6",  
            "panelForeground":"#222",  
            "panelAccent":"#00aeef",  
            "tableAccent":"#00aeef",  
  
            "altBackground":"#f6f6f6",  
            "altForeground":"#000",  
            "altMapBase":"#f68c1f",  
            "altPanelBackground":"#235378",  
            "altPanelForeground":"#fff",  
            "altPanelAccent":"#fdc336",  
            "altTableAccent":"#fdc336"  
        }  
    }  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
