---
title: Fusionner des Partitions dans Analysis Services (SSAS - multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b488997ae97a54a2755847ad9112047015fb0eb5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="merge-partitions-in-analysis-services-ssas---multidimensional"></a>Fusionner des partitions dans Analysis Services (SSAS - Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez fusionner des partitions dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante pour consolider les données de faits de plusieurs partitions du même groupe de mesures.  
  
 [Scénarios courants](#bkmk_Scenario)  
  
 [Spécifications](#bkmk_prereq)  
  
 [Mettre à jour la source de partition après avoir fusionné les partitions](#bkmk_Where)  
  
 [Considérations spéciales pour les partitions segmentées par une table de faits ou une requête nommée](#bkmk_fact)  
  
 [Procédure pour fusionner des partitions à l'aide de SSMS](#bkmk_partitionSSMS)  
  
 [Procédure pour fusionner des partitions à l'aide de XMLA](#bkmk_partitionsXMLA)  
  
##  <a name="bkmk_Scenario"></a> Scénarios courants  
 La configuration individuelle la plus commune pour l'utilisation de partitions repose sur une séparation des données à travers la dimension du temps. La granularité du temps associé à chaque partition varie selon les besoins de l'entreprise spécifiques au projet. Par exemple, la segmentation peut se faire par années, avec l'année la plus récente divisée en mois, plus une partition distincte pour le mois actif. La partition du mois actif accepte régulièrement de nouvelles données.  
  
 Lorsque le mois actif est terminé, cette partition est fusionnée dans les mois de la partition de l'année en cours et le processus continue. À la fin de l'année, une nouvelle partition annuelle toute entière a été formée.  
  
 Comme le montre ce scénario, fusionner des partitions peut devenir une tâche courante effectuée régulièrement et constituer une approche progressive pour consolider et organiser les données d'historique.  
  
##  <a name="bkmk_prereq"></a> Spécifications  
 Les partitions peuvent être fusionnées seulement si elles répondent à tous les critères suivants :  
  
-   Elles ont le même groupe de mesures.  
  
-   Elles ont la même structure.  
  
-   Elles doivent se trouver à l'état traité.  
  
-   Elles ont les mêmes modes de stockage.  
  
-   Elles contiennent des conceptions d'agrégation identiques.  
  
-   Elles partagent le même niveau de compatibilité du magasin de chaînes (s'applique uniquement aux groupes de mesure de comptage de valeurs partitionnés).  
  
 Si la partition cible est vide (autrement dit, elle possède une conception d'agrégation, mais aucune agrégation), la fusion supprime les agrégations des partitions sources. Vous devez exécuter un traitement d'index, un traitement complet ou un traitement par défaut de la partition pour générer les agrégations.  
  
 Les partitions distantes peuvent être fusionnées uniquement avec d’autres partitions distantes qui sont définies avec la même instance distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  Si vous utilisez une combinaison de partitions locales et distantes, une autre méthode consiste à créer des partitions qui contiennent les données combinées, en supprimant les partitions que vous n'utilisez plus.  
  
 Pour créer une partition destinée à être fusionnée ultérieurement, vous pouvez copier la conception d'agrégation d'une autre partition du cube lors de la création de la partition dans l'Assistant Partition. Cette méthode garantit une même conception d'agrégation aux partitions. Au moment de leur fusion, les agrégations de la partition source sont associées à celles de la partition de destination.  
  
##  <a name="bkmk_Where"></a> Mettre à jour la source de partition après avoir fusionné les partitions  
 Les partitions sont segmentées par requête, comme la clause WHERE d'une requête SQL utilisée pour traiter les données, ou par une table ou une requête nommée qui fournit des données à la partition. La propriété **Source** sur la partition indique si la partition est liée à une requête ou une table.  
  
 Quand vous fusionnez des partitions, le contenu des partitions est consolidé, mais la propriété **Source** n’est pas mise à jour pour refléter l’étendue supplémentaire de la partition. Cela signifie que si, plus tard, vous retraitez une partition qui conserve sa propriété **Source**d’origine, vous obtiendrez des données incorrectes de cette partition. La partition agrégera à tort des données au niveau parent. L’exemple suivant illustre ce comportement.  
  
 **Le problème**  
  
 Supposons que vous disposiez d'un cube contenant des informations sur trois produits de boissons non alcoolisées. Il comporte trois partitions qui utilisent la même table de faits. Ces partitions sont segmentées par produit. La partition 1, la partition 2 et la partition 3 contiennent des données sur [ColaFull], [ColaDecaf] et [ColaDiet], respectivement. Si la partition 3 est fusionnée dans la partition 2, les données de la partition obtenue (partition 2) sont correctes et les données du cube sont exactes. Cependant, lors du traitement de la partition 2, son contenu peut être déterminé par le parent des membres au niveau du produit. Ce parent, [SoftDrinks], comprend également [ColaFull], le produit de la partition 1. Le traitement de la partition 2 charge la partition contenant les données relatives à toutes les boissons non alcoolisées, y compris [ColaFull]. Le cube contient alors des données en double pour [ColaFull] et renvoie des données incorrectes aux utilisateurs.  
  
 **La solution**  
  
 La solution consiste à mettre à jour la propriété **Source** , en ajustant la clause WHERE ou la requête nommée, ou en fusionnant manuellement les données des tables de faits sous-jacentes, pour que le traitement suivant soit précis, compte tenu de l’étendue supplémentaire de la partition.  
  
 Dans cet exemple, après la fusion de la partition 3 dans la partition 2, vous pouvez créer un filtre, tel que ("Product" = 'ColaDecaf' OR "Product" = 'ColaDiet') dans la partition obtenue (partition2) pour spécifier que seules les données relatives à [ColaDecaf] et [ColaDiet] doivent être extraites de la table de faits, et que les données de [ColaFull] doivent être exclues. Vous pouvez également spécifier des filtres pour la partition2 et la partition 3 au moment de la création de ces partitions. Ces filtres seront associés pendant la fusion. Dans les deux cas, après le traitement de la partition, le cube ne contiendra pas de données en double.  
  
 **La conclusion**  
  
 Après avoir fusionné des partitions, vérifiez toujours la propriété **Source** pour vous assurer que le filtre est approprié pour les données fusionnées. Si vous avez commencé par une partition qui incluait des données historiques pour Q1, Q2 et Q3 et que vous fusionnez maintenant Q4, vous devez ajuster le filtre de manière à inclure Q4. Sinon, le traitement ultérieur de la partition générera des résultats erronés. Il ne sera pas correct pour Q4.  
  
##  <a name="bkmk_fact"></a> Considérations spéciales pour les partitions segmentées par une table de faits ou une requête nommée  
 Outre les requêtes, les partitions peuvent également être segmentées par une table ou une requête nommée. Si la partition source et la partition cible utilisent la même table de faits dans une source de données ou une vue de source de données, la propriété **Source** est valide après la fusion des partitions. Elle spécifie les données de la table de faits qui sont appropriées à la partition résultante. Étant donné que les faits utilisés pour la partition obtenue existent dans la table de faits, aucune modification de la propriété **Source** n’est nécessaire.  
  
 Les partitions utilisant des données provenant de plusieurs tables de faits ou requêtes nommées requièrent un travail supplémentaire. Vous devez fusionner manuellement les faits de la table de faits de la partition source dans la table de faits de la partition de destination.  
  
 Vous pouvez également remplacer la source pour la partition fusionnée par une requête nommée qui retourne le contenu de deux tables de faits distinctes. Si vous n'effectuez pas cette opération manuelle, les informations contenues dans la table de faits ne seront pas complètes.  
  
 Pour la même raison, les partitions obtenant des données segmentées de requêtes nommées requièrent également une mise à jour. La partition associée doit maintenant avoir une requête nommée qui retourne le jeu de résultats combiné qui a été obtenu précédemment à partir des requêtes nommées distinctes.  
  
## <a name="partition-storage-considerations-molap"></a>Considérations sur le stockage de partitions : MOLAP  
 Lors de la fusion de partitions MOLAP, les faits stockés dans les structures multidimensionnelles des partitions sont également fusionnés. Ceci produit une partition complète et cohérente en interne. Cependant, les faits stockés dans les partitions MOLAP sont des copies des faits de la table de faits. Lorsque la partition est traitée ultérieurement, les faits de la structure multidimensionnelle sont supprimés (seulement pour un traitement complet et d'actualisation) et les données sont copiées à partir de la table de faits, comme indiqué par la source de données et le filtre de la partition. Si la partition source utilise une table de faits différente de celle de la partition de destination, vous devez fusionner manuellement ces deux tables de faits pour vous assurer qu'un ensemble de données complet sera disponible au moment du traitement de la partition résultante. Cela s'applique également si les deux partitions se basent sur des requêtes nommées différentes.  
  
> [!IMPORTANT]  
>  Une partition MOLAP (OLAP multidimensionnel) fusionnée avec une table de faits incomplète contient une copie fusionnée en interne des données de la table de faits et fonctionne correctement jusqu'à son traitement.  
  
## <a name="partition-storage-considerations-holap-and-rolap-partitions"></a>Considérations sur le stockage de partitions : partitions HOLAP et ROLAP  
 Lorsque des partitions HOLAP ou ROLAP ayant des tables de faits différentes sont fusionnées, ces tables de faits ne sont pas automatiquement fusionnées. À moins qu'elles ne soient fusionnées manuellement, seule la table de faits associée à la partition de destination est disponible pour la partition fusionnée. Les faits associés à la partition source ne permettent pas de développer le niveau inférieur d'une partition résultante, et au moment du traitement de la partition, les agrégations ne synthétiseront pas les données de la table non disponible.  
  
> [!IMPORTANT]  
>  Une partition HOLAP ou ROLAP fusionnée avec une table de faits incomplète contient des agrégations exactes mais des faits incomplets. Les requêtes qui font référence à des faits manquants renvoient des données incorrectes. Au moment du traitement de la partition, les agrégations sont uniquement calculées à partir des faits disponibles.  
  
 L'absence des faits non disponibles ne se remarque pas nécessairement, à moins qu'un utilisateur ne tente de développer le niveau inférieur d'un fait d'une table non disponible ou n'exécute une requête qui nécessite un fait d'une table non disponible. Étant donné que les agrégations sont associées pendant le processus de fusion, les requêtes dont les résultats sont uniquement fondés sur les agrégations renvoient des données exactes, tandis que les autres peuvent renvoyer des données erronées. Même après le traitement de la partition résultante, les données manquantes d'une table de faits non disponible peuvent ne pas être remarquées, surtout si elles ne représentent qu'une faible partie des données combinées.  
  
 Les tables de faits peuvent être fusionnées avant ou après les partitions. Cependant, les agrégations ne représenteront pas avec exactitude les faits sous-jacents tant que les deux opérations ne seront pas terminées. Il est recommandé de procéder à la fusion des partitions HOLAP ou ROLAP qui ont accès à différentes tables de faits lorsque les utilisateurs ne sont pas connectés au cube qui contient ces partitions.  
  
##  <a name="bkmk_partitionSSMS"></a> Procédure pour fusionner des partitions à l'aide de SSMS  
  
> [!IMPORTANT]  
>  Avant de fusionner des partitions, commencez par copier les informations de filtrage de données (souvent, la clause WHERE pour les filtres basés sur des requêtes SQL). Plus tard, une fois la fusion terminée, vous devrez mettre à jour la propriété Source de la partition contenant les données de faits cumulées.  
  
1.  Dans l’Explorateur d’objets, développez le nœud **Groupes de mesures** du cube contenant les partitions à fusionner, développez **Partitions**, cliquez avec le bouton droit sur la partition correspondant à la cible ou à la destination de la fusion. Par exemple, si vous déplacez des données de faits trimestrielles vers une partition qui stocke des données de faits annuelles, sélectionnez la partition qui contient les données de faits annuelles.  
  
2.  Cliquez sur **fusionner des Partitions** pour ouvrir le **Partition de fusion \<nom de la partition >** boîte de dialogue.  
  
3.  Sous **Partitions sources**, cochez la case à côté de chaque partition source que vous voulez fusionner avec la partition cible, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    >  Les partitions sources sont immédiatement supprimées une fois la source fusionnée dans la partition cible. Actualisez le dossier Partitions pour mettre à jour son contenu une fois la fusion terminée.  
  
4.  Cliquez avec le bouton droit sur la partition contenant les données cumulées et sélectionnez **Propriétés**.  
  
5.  Ouvrez la propriété **Source** et modifiez la clause WHERE pour qu’elle inclue les données de partition que vous venez de fusionner. N’oubliez pas que la propriété **Source** n’est pas mise à jour automatiquement. Si vous lancez un nouveau traitement sans d’abord mettre à jour la propriété **Source**, vous risquez de ne pas obtenir toutes les données attendues.  
  
##  <a name="bkmk_partitionsXMLA"></a> Procédure pour fusionner des partitions à l'aide de XMLA  
 Pour plus d’informations, consultez [Fusion de partitions &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/merging-partitions-xmla.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Pour des objets de traitement Analysis Services](../../analysis-services/multidimensional-models/processing-analysis-services-objects.md)   
 [Partitions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data.md)   
 [Créer et gérer une Partition locale & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md)   
 [Créer et gérer une Partition distante & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/create-and-manage-a-remote-partition-analysis-services.md)   
 [Définir l’écriture différée de Partition](../../analysis-services/multidimensional-models/set-partition-writeback.md)   
 [Partitions activées en écriture](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-write-enabled-partitions.md)   
 [Configurer le stockage des chaînes pour les Dimensions et Partitions](../../analysis-services/multidimensional-models/configure-string-storage-for-dimensions-and-partitions.md)  
  
  
