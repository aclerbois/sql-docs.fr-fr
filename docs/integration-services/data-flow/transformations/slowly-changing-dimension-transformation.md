---
title: Dimension à variation lente, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.slowlychangingdimtrans.f1
helpviewer_keywords:
- Slowly Changing Dimension transformation
- slowly changing dimensions
- SCD transformation
- updating slowly changing dimensions
ms.assetid: f8849151-c171-4725-bd25-f2c33a40f4fe
caps.latest.revision: 55
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7d19c71d2bc62499294e8ba7b16b3072bfb46df8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="slowly-changing-dimension-transformation"></a>Transformation de dimension à variation lente
  La transformation de dimension à variation lente coordonne la mise à jour et l'insertion d'enregistrements dans des tables de dimension d'entrepôts de données. Par exemple, vous pouvez utiliser cette transformation pour configurer les sorties de la transformation qui insèrent et mettent à jour des enregistrements dans la table DimProduct de la base de données [!INCLUDE[ssSampleDBDWobject](../../../includes/sssampledbdwobject-md.md)] à l’aide des données de la table Production.Products de la base de données OLTP AdventureWorks.  
  
> [!IMPORTANT]  
>  L'Assistant Dimension à variation lente prend uniquement en charge les connexions à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 La transformation de dimension à variation lente fournit la fonctionnalité suivante pour la gestion des dimensions à variation lente :  
  
-   Correspondance entre les lignes entrantes et les lignes de la table de recherche afin d'identifier les lignes nouvelles et existantes.  
  
-   Identification des lignes entrantes qui contiennent des modifications lorsque celles-ci ne sont pas autorisées.  
  
-   Identification des enregistrements de membres inférés qui nécessitent une mise à jour.  
  
-   Identification des lignes entrantes qui contiennent des modifications historiques nécessitant l'insertion de nouveaux enregistrements et la mise à jour des enregistrements expirés.  
  
-   Détection des lignes entrantes qui contiennent des modifications nécessitant la mise à jour des enregistrements existants, y compris les enregistrements expirés.  
  
 La transformation de dimension à variation lente prend en charge quatre types de modifications : attribut variable, attribut historique, attribut fixe et membre déduit.  
  
-   Les modifications « modification d'attribut » remplacent les enregistrements existants. Ce type de modification est équivalent à une modification de Type 1. La transformation de dimension à variation lente dirige ces lignes vers une sortie nommée **Sortie de mises à jour d’attribut de validation**.  
  
-   Les modifications « attribut d'historique » créent des enregistrements au lieu de mettre à jour les enregistrements existants. La seule modification autorisée dans un enregistrement existant est une mise à jour d'une colonne qui indique si l'enregistrement est actif ou expiré. Ce type de modification est équivalent à une modification de Type 2. La transformation de dimension à variation lente dirige ces lignes vers deux sorties : **Sortie d’insertions d’attribut d’historique** et **Nouvelle sortie**.  
  
-   Les modifications « attribut fixe » indiquent que la valeur de colonne ne doit pas changer. La transformation de dimension à variation lente détecte les modifications et peut diriger les lignes modifiées vers une sortie nommée **Sortie d’attribut fixe**.  
  
-   « Membre inféré » indique que la ligne est un enregistrement de membre inféré dans la table de dimension. Un membre inféré existe lorsqu'une table de faits fait référence à un membre de la dimension qui n'est pas encore chargé. Un enregistrement de membre inféré minimal est créé en prévision des données de dimension pertinentes, qui sont fournies dans un chargement ultérieur des données de dimension. La transformation de dimension à variation lente dirige ces lignes vers une sortie nommée **Sortie de mises à jour de membre déduit**. Lorsque les données du membre inféré sont chargées, vous pouvez mettre à jour l'enregistrement existant au lieu d'en créer un.  
  
> [!NOTE]  
>  La transformation de dimension à variation lente ne prend pas en charge les modifications de Type 3, qui nécessitent des modifications de la table de dimension. En identifiant les colonnes avec le type de mise à jour d'attribut fixe, vous pouvez capturer les valeurs de données qui sont candidates aux modifications de Type 3.  
  
 Au moment de l'exécution, la transformation de dimension à variation lente essaie tout d'abord de faire correspondre la ligne entrante à un enregistrement de la table de recherche. Si aucune correspondance n’est trouvée, la ligne entrante est un nouvel enregistrement ; ainsi, la transformation de dimension à variation lente n’effectue aucune opération supplémentaire et dirige la ligne vers la **Nouvelle sortie**.  
  
 Si une correspondance est trouvée, la transformation de dimension à variation lente détecte si la ligne contient des modifications. Si c’est le cas, elle identifie le type de mise à jour pour chaque colonne et dirige la ligne vers la **Sortie de mises à jour d’attribut de validation**, la **Sortie d’attribut fixe**, la **Sortie d’insertions d’attribut d’historique**ou la **Sortie de mises à jour de membre déduit**. Si la ligne est inchangée, la transformation de dimension à variation lente dirige la ligne vers la **Sortie inchangée**.  
  
## <a name="slowly-changing-dimension-transformation-outputs"></a>Sorties de la transformation de dimension à variation lente  
 La transformation de dimension à variation lente possède une entrée et jusqu'à six sorties. Une sortie dirige une ligne vers le sous-ensemble du flux de données qui correspond aux exigences de mise à jour et d'insertion de la ligne. Cette transformation ne prend pas de sortie d'erreur en charge.  
  
 Le tableau suivant décrit les sorties de transformation et les exigences de leurs flux de données ultérieurs. Les exigences décrivent le flux de données créé par l'Assistant Dimension à variation lente.  
  
|Sortie|Description|Exigences de flux de données|  
|------------|-----------------|----------------------------|  
|**Sortie de mises à jour d’attribut de validation**|L'enregistrement de la table de recherche est mis à jour. Cette sortie est utilisée pour les lignes à attributs variables.|Une transformation de commande OLE DB met à jour l'enregistrement à l'aide d'une instruction UPDATE.|  
|**Sortie d’attribut fixe**|Les valeurs des lignes qui ne doivent pas changer ne correspondent pas aux valeurs de la table de recherche. Cette sortie est utilisée pour les lignes à attributs fixes.|Aucun flux de données par défaut n'est créé. Si la transformation est configurée de façon à continuer après avoir rencontré des modifications de colonnes d'attributs fixes, vous devez créer un flux de données qui capture ces lignes.|  
|**Sortie d’insertions d’attribut d’historique**|La table de recherche contient au moins une ligne correspondante. La ligne marquée comme « active » doit maintenant être marquée comme ayant « expiré ». Cette sortie est utilisée pour les lignes d'attributs d'historique.|Les transformations de colonnes dérivées créent des colonnes pour les indicateurs de ligne active et de ligne expirée. Une transformation de commande OLE DB met à jour l'enregistrement qui doit maintenant être marqué comme « expiré ». La ligne avec les nouvelles valeurs de colonne est dirigée vers la nouvelle sortie, où la ligne est insérée et marquée comme « active ».|  
|**Sortie de mises à jour de membre déduit**|Des lignes pour les membres de dimension inférés sont insérées. Cette sortie est utilisée pour les lignes de membres inférés.|Une transformation de commande OLE DB met à jour l'enregistrement à l'aide d'une instruction SQL UPDATE.|  
|**Nouvelle sortie**|La table de recherche ne contient aucune ligne correspondante. La ligne est ajoutée à la table de dimension. Cette sortie est utilisée pour les nouvelles lignes et les modifications de lignes à attributs d'historique.|Une transformation de colonnes dérivées définit l'indicateur de ligne active et une destination OLE DB insère la ligne.|  
|**Sortie inchangée**|Les valeurs de la table de recherche correspondent aux valeurs de lignes. Cette sortie est utilisée pour les lignes inchangées.|Aucun flux de données n'est créé car la transformation de dimension à variation lente n'effectue aucune opération. Si vous souhaitez capturer ces lignes, vous devez créer un flux de données pour cette sortie.|  
  
## <a name="business-keys"></a>Clés d'entreprise  
 La transformation de dimension à variation lente requiert au moins une colonne clé d'entreprise.  
  
 La transformation de dimension à variation lente ne prend pas en charge les clés d'entreprise nulles. Si les données incluent des lignes dans lesquelles la colonne clé d'entreprise est nulle, ces lignes doivent être supprimées du flux de données. Vous pouvez utiliser la transformation de fractionnement conditionnel pour filtrer les lignes dont les colonnes clés d'entreprise contiennent des valeurs nulles. Pour plus d’informations, voir [Conditional Split Transformation](../../../integration-services/data-flow/transformations/conditional-split-transformation.md).  
  
## <a name="optimizing-the-performance-of-the-slowly-changing-dimension-transformation"></a>Optimisation des performances de la transformation de dimension à variation lente  
 Pour les suggestions sur l’amélioration des performances de la transformation de dimension à variation lente, consultez [Fonctionnalités de performances de flux de données](../../../integration-services/data-flow/data-flow-performance-features.md).  
  
## <a name="troubleshooting-the-slowly-changing-dimension-transformation"></a>Résolution des problèmes liés à la transformation de dimension à variation lente  
 Vous pouvez consigner les appels émis par la transformation de dimension à variation lente vers des fournisseurs de données externes. Vous pouvez utiliser cette fonctionnalité de journalisation pour résoudre des problèmes liés à des connexions, des commandes et des requêtes à des sources de données externes qu'effectue la transformation de dimension à variation lente. Pour consigner les appels que la transformation de dimension à variation lente adresse à des fournisseurs de données externes, activez la journalisation des packages et sélectionnez l’événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="configuring-the-slowly-changing-dimension-transformation"></a>Configuration de la transformation de dimension à variation lente  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="configuring-the-slowly-changing-dimension-transformation-outputs"></a>Configuration des sorties de la transformation de dimension à variation lente  
 La coordination de la mise à jour et de l'insertion d'enregistrements dans des tables de dimension peut être une tâche complexe, en particulier si des modifications de Type 1 et de Type 2 sont utilisées. [!INCLUDE[ssIS](../../../includes/ssis-md.md)] Le concepteur propose deux manières de configurer la prise en charge des dimensions à variation lente :  
  
-   La boîte de dialogue **Éditeur avancé** , dans laquelle vous sélectionnez une connexion, vous définissez des propriétés de composant courantes et personnalisées, vous sélectionnez des colonnes d’entrée et vous définissez les propriétés des colonnes sur les six sorties. Pour terminer la tâche de configuration de la prise en charge d'une dimension à variation lente, vous devez créer manuellement le flux de données pour les sorties utilisées par la transformation de dimension à variation lente. Pour en savoir plus, voir [Data Flow](../../../integration-services/data-flow/data-flow.md).  
  
-   L'Assistant Chargement de dimension, qui vous guide lors des étapes de configuration de la transformation de dimension à variation lente et de création du flux de données pour les sorties de transformation. Pour modifier la configuration des dimensions à variation lente, réexécutez l'Assistant Chargement de dimension. Pour plus d’informations, consultez [Configurer les sorties à l’aide de l’Assistant Dimension à variation lente](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-content"></a>Contenu associé  
  
-   Entrée de blog [Optimizing the Slowly Changing Dimension Wizard](http://go.microsoft.com/fwlink/?LinkId=199481)(Optimisation de l’Assistant Dimension à variation lente) sur blogs.msdn.com.  
  
  
