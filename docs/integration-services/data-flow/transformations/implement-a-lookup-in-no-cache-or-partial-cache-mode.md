---
title: Implémenter une recherche en mode Aucun cache ou Cache partiel | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- Lookup transformation
- match exactly [Integration Services]
- lookups [Integration Services]
- exact matches [Integration Services]
ms.assetid: 01b7fbca-5181-4d47-9f75-7f25af6b40d2
caps.latest.revision: 67
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4cfb1b67c43227d8fd27c2536fd762a601651786
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="implement-a-lookup-in-no-cache-or-partial-cache-mode"></a>Implémenter une recherche en mode Aucun cache ou Cache partiel
  Vous pouvez configurer la transformation de recherche afin qu'elle utilise le mode Cache partiel ou le mode Aucun cache.  
  
-   Cache partiel  
  
     Les lignes avec entrées correspondantes dans le dataset de référence et, éventuellement, les lignes sans entrées correspondantes dans le dataset sont stockées dans le cache. Lorsque la taille de mémoire du cache est dépassée, la transformation de recherche supprime automatiquement les lignes le moins fréquemment utilisées du cache.  
  
-   Aucun cache  
  
     Aucune donnée n'est chargée dans le cache.  
  
 Que vous sélectionniez le mode Cache partiel ou le mode Aucun cache, vous utilisez un gestionnaire de connexions OLE DB pour vous connecter au dataset de référence. Le dataset de référence est généré à l’aide d’une table, d’une vue ou d’une requête SQL pendant l’exécution de la transformation de recherche.  
  
### <a name="to-implement-a-lookup-transformation-in-no-cache-or-partial-cache-mode"></a>Pour implémenter une transformation de recherche en mode Aucun cache ou en mode Cache partiel  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] qui contient le package souhaité, puis ouvrez le package.  
  
2.  Sous l’onglet **Flux de données** , ajoutez une transformation de recherche.  
  
3.  Connectez la transformation de recherche au flux de données en faisant glisser un connecteur à partir d'une source ou d'une transformation précédente jusqu'à la transformation de recherche.  
  
    > [!NOTE]  
    >  Une transformation de recherche configurée pour utiliser le mode Aucun cache peut ne pas se valider si cette transformation se connecte à un fichier plat qui contient un champ Date vide. La validation de la transformation dépend si le gestionnaire de connexions pour le fichier plat a été configuré pour conserver des valeurs NULL. Pour que la validation de la transformation de recherche soit validée, dans **l’Éditeur de source de fichier plat**, dans la **page Gestionnaire de connexions**, sélectionnez l’option **Conserver les valeurs NULL de la source comme valeurs NULL dans le flux de données** .  
  
4.  Double-cliquez sur la transformation source ou précédente pour configurer le composant.  
  
5.  Double-cliquez sur la transformation de recherche, puis dans **l’Éditeur de transformation de recherche**, dans la page **Général** , sélectionnez **Cache partiel** ou **Aucun cache**.  
  
6.  Dans la liste **Spécifier comment gérer les lignes sans entrées correspondantes** , sélectionnez une option de gestion des erreurs.  
  
7.  Dans la page **Connexion** , sélectionnez un gestionnaire de connexions dans la liste **Gestionnaire de connexions OLE DB** ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions. Pour plus d’informations, consultez [OLE DB Connection Manager](../../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
8.  Procédez de l’une des manières suivantes :  
  
    -   Cliquez sur **Utiliser une table ou une vue**, puis sélectionnez une table ou une vue, ou cliquez sur **Nouveau** pour créer une table ou une vue.  
  
    -   Cliquez sur **Utiliser les résultats d’une requête SQL**, puis générez une requête dans la fenêtre **Commande SQL** .  
  
         —ou—  
  
         Cliquez sur **Générer la requête** pour générer une requête à l’aide des outils graphiques fournis par le **Générateur de requêtes** .  
  
         —ou—  
  
         Cliquez sur **Parcourir** pour importer une instruction SQL à partir d’un fichier.  
  
     Pour valider la requête SQL, cliquez sur **Analyser la requête**.  
  
     Pour afficher un échantillon des données, cliquez sur **Aperçu**.  
  
9. Cliquez sur la page **Colonnes** , puis faites glisser au moins une colonne de la liste **Colonnes d’entrée disponibles** vers une colonne de la liste **Colonnes de recherche disponibles** .  
  
    > [!NOTE]  
    >  La transformation de recherche mappe automatiquement les colonnes ayant le même nom et le même type de données.  
  
    > [!NOTE]  
    >  Les types de données des colonnes doivent correspondre pour que les colonnes puissent être mappées. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
10. Incluez des colonnes de recherche dans la sortie en procédant comme suit :  
  
    1.  Dans la liste **Colonnes de recherche disponibles** , sélectionnez des colonnes.  
  
    2.  Dans la liste **Opération de recherche** , spécifiez si les valeurs des colonnes de recherche remplacent les valeurs des colonnes d’entrée ou sont écrites dans une nouvelle colonne.  
  
11. Si vous avez sélectionné **Cache partial** à l’étape 5, dans la page **Avancé** , définissez les options de cache suivantes :  
  
    -   Dans la liste **Taille du cache (32 bits)** , sélectionnez la taille du cache pour les environnements 32 bits.  
  
    -   Dans la liste **Taille du cache (64 bits)** , sélectionnez la taille du cache pour les environnements 64 bits.  
  
    -   Pour mettre en cache les lignes sans entrées correspondantes dans la référence, sélectionnez **Activer le cache pour les lignes sans entrées correspondantes**.  
  
    -   Dans la liste **Allocation à partir du cache** , sélectionnez le pourcentage du cache à utiliser pour stocker les lignes sans entrées correspondantes.  
  
12. Pour modifier l’instruction SQL qui génère le dataset de référence, sélectionnez **Modifier l’instruction SQL**et modifiez l’instruction SQL affichée dans la zone de texte.  
  
     Si l’instruction inclut des paramètres, cliquez sur **Paramètres** pour mapper les paramètres aux colonnes d’entrée.  
  
    > [!NOTE]  
    >  L’instruction SQL facultative que vous spécifiez dans cette page substitue et remplace le nom de table que vous avez spécifié dans la page **Connexion** de **l’Éditeur de transformation de recherche**.  
  
13. Pour configurer la sortie d’erreur, cliquez sur la page **Sortie d’erreur** et définissez les options de gestion des erreurs. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Sortie d’erreur&#41;](../../../integration-services/data-flow/transformations/lookup-transformation-editor-error-output-page.md).  
  
14. Cliquez sur **OK** pour enregistrer les modifications apportées à la transformation de recherche, puis exécutez le package.  
  
## <a name="see-also"></a> Voir aussi  
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
