---
title: Configurer la destination du fichier plat (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.configureflatfiledest.f1
ms.assetid: 318e8da0-37d3-46cd-943a-fc5d66aad93a
caps.latest.revision: 53
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cee1fcffe26a8f17d5a8c0fd4e547e98b95d30c1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-flat-file-destination-sql-server-import-and-export-wizard"></a>Configurer la destination du fichier plat (Assistant Importation et Exportation SQL Server)
  Si vous avez sélectionné une destination de fichier plat, l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche la page **Configurer la destination du fichier plat** une fois que vous avez spécifié que vous souhaitiez copier une table ou que vous avez fourni une requête. Dans cette page, vous spécifiez les options de mise en forme du fichier plat de destination. Si vous le souhaitez, vous pouvez passer en revue le mappage des colonnes et afficher un aperçu des exemples de données.  
  
## <a name="screen-shot-of-the-configure-flat-file-destination-page"></a>Capture d’écran de la page Configurer la destination du fichier plat  
 La capture d’écran suivante montre un exemple de la page **Configurer la destination du fichier plat** de l’Assistant.
 
 Dans cet exemple, l’utilisateur a spécifié les options suivantes pour créer un fichier CSV (valeurs séparées par des virgules) classique.
-   **Délimiteur de ligne**. Chaque ligne de données de la sortie se termine par une combinaison retour chariot-saut de ligne.
-   **Délimiteur de colonne**. Les colonnes de données dans chaque ligne sont séparées par une virgule.

 ![Page Configurer la destination du fichier plat de l’Assistant Importation et Exportation](../../integration-services/import-export-data/media/flat-file.png)
  
## <a name="pick-a-source-table"></a>Choisir une table source
 **Vue ou table source**  
-   Si vous avez spécifié dans une page précédente que vous vouliez copier une table, sélectionnez la table ou la vue source dans la liste déroulante.
-   Si vous avez fourni une requête, `"Query"` est sélectionné et est la seule option.  

## <a name="specify-row-and-column-delimiters-for-the-output"></a>Spécifier des séparateurs de lignes et des délimiteurs de colonnes pour la sortie
 **Séparateur de lignes**  
 Sélectionnez dans la liste un séparateur utilisé pour séparer les lignes de la sortie. Aucune option n’est prévue pour spécifier un séparateur de ligne *personnalisé*.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**{CR}{LF}**|Délimiter les lignes avec une combinaison retour chariot-saut de ligne.|  
|**{CR}**|Délimiter les lignes avec un retour chariot.|  
|**{LF}**|Délimiter les lignes avec un saut de ligne.|  
|**Point-virgule {;}**|Délimiter les lignes avec un point-virgule.|  
|**Deux-points {:}**|Délimiter les lignes avec un signe deux-points.|  
|**Virgule {,}**|Délimiter les lignes avec une virgule.|  
|**Tabulation {t}**|Délimiter les lignes avec une tabulation.|  
|**Barre verticale {&#124;}**|Délimiter les lignes avec une barre verticale.|  
  
 **Délimiteur de colonne**  
 Sélectionnez dans la liste un délimiteur utilisé pour séparer les colonnes de la sortie. Aucune option n’est prévue pour spécifier un délimiteur de colonne *personnalisé*.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**{CR}{LF}**|Délimiter les colonnes avec une combinaison retour chariot-saut de ligne.|  
|**{CR}**|Délimiter les colonnes avec un retour chariot.|  
|**{LF}**|Délimiter les colonnes avec un saut de ligne.|  
|**Point-virgule {;}**|Délimiter les colonnes avec un point-virgule.|  
|**Deux-points {:}**|Délimiter les colonnes avec un signe deux-points.|  
|**Virgule {,}**|Délimiter les colonnes avec une virgule.|  
|**Tabulation {t}**|Délimiter les colonnes avec une tabulation.|  
|**Barre verticale {&#124;}**|Délimiter les colonnes avec une barre verticale.|  

## <a name="optionally-review-column-mappings-and-preview-data"></a>Éventuellement, vérifier le mappage de colonnes et afficher un aperçu des données

**Modifier les mappages**   
Cliquez sur **Modifier les mappages** pour afficher la boîte de dialogue **Mappage de colonnes** de la table sélectionnée. Utilisez la boîte de dialogue **Mappage de colonnes** pour effectuer les opérations suivantes.
-   Passer en revue le mappage des colonnes entre la source et la destination.
-   Vous pouvez copier uniquement un sous-ensemble de colonnes en sélectionnant **ignorer** pour les colonnes que vous souhaitez ignorer.

Pour plus d’informations, consultez [Mappages de colonnes](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md).  

**Aperçu**  
Cliquez sur **Aperçu** pour afficher un aperçu de 200 lignes d’exemples de données maximum dans la boîte de dialogue **Aperçu des données**. Cela confirme que l’Assistant va copier les données que vous souhaitez copier. Pour plus d’informations, consultez [Aperçu des données](../../integration-services/import-export-data/preview-data-dialog-box-sql-server-import-and-export-wizard.md).  
  
Après avoir affiché un aperçu des données, vous souhaiterez peut-être modifier les options que vous avez sélectionnées dans les pages précédentes de l’Assistant. Pour effectuer ces modifications, retournez dans la page **Configurer la destination du fichier plat** , puis cliquez sur **Précédent** pour revenir aux pages précédentes où vous pouvez modifier vos sélections.  

## <a name="whats-next"></a>Quelle est l’étape suivante ?  
 Après avoir spécifié les options de mise en forme pour le fichier plat de destination, la page suivante est **Enregistrer et exécuter le package**. Dans cette page, vous spécifiez si vous souhaitez exécuter l’opération immédiatement. Selon votre configuration, vous pouvez également être en mesure d’enregistrer vos paramètres sous la forme d’un package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour le personnaliser et le réutiliser ultérieurement. Pour plus d’informations, consultez [Enregistrer et exécuter le package](../../integration-services/import-export-data/save-and-run-package-sql-server-import-and-export-wizard.md).  

