---
title: Identifier des lignes de données semblables à l’aide de la transformation de regroupement probable | Microsoft Docs
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
helpviewer_keywords:
- Fuzzy Grouping transformation
- match similar data [Integration Services]
- similar data rows [Integration Services]
- fuzzy matches
ms.assetid: ffcb41a6-e23d-49ea-8c32-ac980e3dc495
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9fdd8970c549cc4d9ebd953561efb8c523b7e028
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="identify-similar-data-rows-by-using-the-fuzzy-grouping-transformation"></a>Identifier des lignes de données semblables à l'aide de la transformation de regroupement probable
  Pour ajouter et configurer une transformation de regroupement probable, le package doit déjà inclure au moins une tâche de flux de données et une source.  
  
### <a name="to-implement-fuzzy-grouping-transformation-in-a-data-flow"></a>Pour implémenter une transformation de regroupement probable dans un flux de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** puis, à partir de la **Boîte à outils**, faites glisser la transformation de regroupement probable sur la surface de dessin.  
  
4.  Connectez la transformation de regroupement probable au flux de données en faisant glisser le connecteur à partir de la source de données ou d'une transformation précédente vers la transformation de regroupement probable.  
  
5.  Double-cliquez sur la transformation de regroupement probable.  
  
6.  Dans la boîte de dialogue **Éditeur de transformation de regroupement probable** , sous l’onglet **Gestionnaire de connexions** , sélectionnez un gestionnaire de connexions OLE DB qui établit une connexion à une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  La transformation requiert une connexion à une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour permettre la création de tables et d’index temporaires.  
  
7.  Cliquez sur l’onglet **Colonnes** et, dans la liste **Colonnes d’entrée disponibles** , cochez la case des colonnes d’entrée à utiliser pour identifier des lignes semblables dans le dataset.  
  
8.  Cochez la case dans la colonne **Transfert direct** pour identifier les colonnes d’entrée comme devant être transférées directement vers la sortie de transformation. Les colonnes à transfert direct ne sont pas incluses dans le processus d'identification des lignes dupliquées.  
  
    > [!NOTE]  
    >  Les colonnes d'entrée utilisées pour le regroupement sont sélectionnées automatiquement comme colonnes à transfert direct et elles ne peuvent pas être désélectionnées tant qu'elles sont utilisées pour le regroupement.  
  
9. Mettez éventuellement à jour les noms des colonnes de sortie dans la colonne **Alias de sortie** .  
  
10. Vous pouvez également mettre à jour les noms des colonnes nettoyées dans la colonne **Alias de sortie de groupe** .  
  
    > [!NOTE]  
    >  Les noms par défaut des colonnes sont les noms des colonnes d'entrée avec un suffixe « _clean ».  
  
11. Mettez éventuellement à jour le type de correspondance à utiliser dans la colonne **Type de correspondance** .  
  
    > [!NOTE]  
    >  Au moins une colonne doit utiliser la correspondance approximative.  
  
12. Spécifiez les colonnes de niveau de similarité minimale dans la colonne **Similarité minimale** . Elle doit être comprise entre 0 et 1. Plus la valeur est proche de 1, plus les valeurs des colonnes d'entrée doivent être similaires pour former un groupe. Une similarité minimale de 1 indique une correspondance exacte.  
  
13. Mettez éventuellement à jour les noms des colonnes de similarité dans la colonne **Alias de sortie de similarité** .  
  
14. Pour spécifier la gestion des nombres dans les valeurs de données, mettez à jour les valeurs dans la colonne **Chiffres** .  
  
15. Pour spécifier la manière dont la transformation compare les données de chaîne dans une colonne, modifiez la sélection par défaut des options de comparaison dans la colonne **Indicateurs de comparaison** .  
  
16. Cliquez sur l’onglet **Avancé** pour modifier les noms des colonnes que la transformation ajoute à la sortie pour l’identificateur de ligne unique (_key_in), l’identificateur de ligne dupliquée (_key_out) et la valeur de similarité (_score).  
  
17. Ajustez éventuellement le seuil de similarité en déplaçant le curseur.  
  
18. Désactivez éventuellement les cases à cocher de séparateurs de jetons pour ignorer les séparateurs dans les données.  
  
19. Cliquez sur **OK**.  
  
20. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a> Voir aussi  
 [Fuzzy Grouping Transformation](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [tâche de flux de données](../../../integration-services/control-flow/data-flow-task.md)  
  
  
