---
title: Propriétés des colonnes | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4da1159bfc4582705710735c53300d9c51627398
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="column-properties"></a>Propriétés des colonnes 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cet article décrit les propriétés de colonne de modèle tabulaire.  
  
>  [!NOTE]  
>  Certaines propriétés ne sont pas pris en charge dans tous les niveaux de compatibilité.    
  
##  <a name="bkmk_properties"></a> Propriétés des colonnes  
**Avancé**  
  
|Propriété|Paramètre par défaut| Description|  
|--------------|---------------------|-----------------|  
|**Dossier d’affichage**||Un dossier unique ou imbriqué pour organiser les colonnes dans une liste de champs d’application cliente.|  

**Basic**  
  
|Propriété|Paramètre par défaut| Description|  
|--------------|---------------------|-----------------|  
|**Nom de la colonne**||Le nom de la colonne tel qu'il est stocké dans le modèle et tel qu'il est affiché dans une liste de champs de client de création de rapports.|  
|**Format de données**|Déterminé automatiquement pendant l'importation.|Spécifie le format d'affichage à utiliser pour les données de cette colonne. Cette propriété propose les options suivantes :<br /><br /> **Général**<br /><br /> **Nombre décimal**<br /><br /> **Nombre entier**<br /><br /> **Monétaire (Currency)**<br /><br /> **Pourcentage**<br /><br /> **Scientifique**<br /><br /> Après avoir défini un format de données, vous pouvez définir les propriétés spécifiques à chaque format. Par exemple, si vous choisissez le format **Devise** , vous pouvez définir le nombre de décimales visibles, choisir le séparateur des milliers, ainsi que le symbole monétaire.<br /><br /> <br /><br /> Si les valeurs de colonnes contiennent des images, consultez **Image représentative**.|  
|**Type de données**|Déterminé automatiquement pendant l'importation.|Spécifie le type de données pour toutes les valeurs de la colonne.|  
|**Description**||Description textuelle de la colonne.<br /><br /> Dans certains clients de création de rapports, si un utilisateur place le curseur sur cette colonne dans la liste des champs, la description apparaît sous la forme d'une info-bulle.|  
|**Caché**|False|Spécifie si la colonne est masquée dans les listes de champs de client de création de rapports.<br /><br /> Définissez cette propriété avec la valeur **True** pour masquer cette colonne dans l'affichage. Par exemple, les colonnes qui contiennent des identificateurs ou des clés ne sont généralement pas utiles à l'utilisateur final.<br /><br /> Si vous masquez une colonne dans le client de création de rapports, le champ n'est pas supprimé dans les données de modèle. Le champ est toujours visible si vous créez une requête sur le modèle. Une colonne masquée peut encore être utilisée pour le regroupement ou le tri.<br /><br /> La propriété **Masqué** ne fournit aucune forme de sécurité des données. Pour sécuriser les données, utilisez des filtres de ligne dans les rôles. Pour plus d’informations, consultez [rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md).|  
|**Trier par colonne**||Spécifie une autre colonne pour trier les valeurs de cette colonne. Une relation doit exister entre les deux colonnes.<br /><br /> Cette valeur doit correspondre au nom d'une colonne existante. Vous ne pouvez pas spécifier de formule ou de mesure.|  

 **Divers**  
  
|Propriété|Paramètre par défaut| Description|  
|--------------|---------------------|-----------------|  
|**Indications de codage**|Par défaut|Spécifie l’encodage pour optimiser le traitement. Encodage de valeur peut améliorer les performances des requêtes pour les colonnes numériques généralement utilisées dans les agrégations. Encodage du hachage est de colonnes group by (souvent des valeurs de table de dimension) et les clés étrangères. Colonnes de type chaîne sont toujours encodée de hachage.|  

 **Propriétés de la création de rapports**  
  
|Propriété|Paramètre par défaut| Description|  
|--------------|---------------------|-----------------|  
|Image par défaut|False|Spécifie la colonne qui fournit une image représentant les données de ligne (par exemple, une pièce d'identité avec photo dans un enregistrement d'employé).|  
|Étiquette par défaut|False|Spécifie la colonne qui fournit un nom d'affichage pour représenter les données de ligne (par exemple, nom de l'employé dans un enregistrement d'employé).|  
|URL d'image/Catégorie de données (SP1)|False|Spécifie la valeur de cette colonne en tant que lien hypertexte vers une image sur un serveur. Par exemple : `http://localhost/images/image1.jpg`.|  
|Conserver des lignes uniques|False|Spécifie les colonnes qui fournissent des valeurs qui doivent être traitées comme uniques même en cas de doublons (par exemple, prénom et nom de l'employé, dans les cas où deux employés ou plus portent le même nom).|  
|Identificateur de ligne|False|Spécifie une colonne qui contient seulement des valeurs uniques et permet l'utilisation de cette colonne comme clé de regroupement interne.|  
|Synthétiser par|Par défaut|Spécifie que les outils clients de création de rapports appliquent la fonction d'agrégation SUM pour les calculs de colonne lorsque cette colonne est ajoutée à une liste de champs. Pour modifier le calcul par défaut, sélectionnez-le dans la liste déroulante. Cette propriété s'applique uniquement aux colonnes de type qui peuvent être agrégées.|  
|Position du détail de la table|Aucun champ par défaut défini|Spécifie que cette colonne ou mesure peut être ajoutée à un ensemble de champs à partir d'une seule table pour améliorer l'expérience de visualisation de table dans un client de création de rapports.|  
  
##  <a name="bkmk_config_prop"></a> Configurer des paramètres de propriété de colonne  
  
1.  Dans le générateur de modèles, dans une table, sélectionnez une colonne.  
  
2.  Dans la fenêtre **Propriétés** , cliquez sur une propriété, puis tapez une valeur ou cliquez sur la flèche Bas pour sélectionner une option de paramètre.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher les propriétés de création de rapports de l’alimentation](../../analysis-services/tabular-models/power-view-reporting-properties-ssas-tabular.md)   
 [Masquer ou figer des colonnes](../../analysis-services/tabular-models/hide-or-freeze-columns-ssas-tabular.md)   
 [Ajouter des colonnes à une table](../../analysis-services/tabular-models/add-columns-to-a-table-ssas-tabular.md)  
  
  
