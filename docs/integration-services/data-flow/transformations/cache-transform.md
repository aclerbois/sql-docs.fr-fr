---
title: Transformation du cache | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
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
- sql13.dts.designer.cachetrans.f1
- sql13.dts.designer.cachetranscon.f1
- sql13.dts.designer.cachetransmap.f1
helpviewer_keywords:
- Cache transform
ms.assetid: a5683fc8-9c32-4634-819e-e9815627e4f1
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 82a5b9d18a076aaee938e9593b3e172f3f36eb3a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cache-transform"></a>Transformation du cache
  La transformation du cache génère un dataset de référence pour la transformation de recherche en entrant des données depuis une source de données connectée dans le flux de données dans un gestionnaire de connexions du cache. La transformation de recherche effectue des recherches en joignant les données des colonnes d'entrée d'une source de données connectée aux colonnes de la base de données de référence.  
  
 Vous pouvez utiliser le gestionnaire de connexions du cache lorsque vous souhaitez configurer la transformation de recherche en mode cache complet. Dans ce mode, le dataset de référence est chargé dans le cache avant l'exécution de la transformation de recherche.  
  
 Pour savoir comment configurer la transformation de recherche en mode cache complet à l’aide du gestionnaire de connexions du cache et de la transformation du cache, consultez [Implémenter une transformation de recherche en mode Cache complet à l’aide du gestionnaire de connexions du cache](../../../integration-services/data-flow/transformations/lookup-transformation-full-cache-mode-cache-connection-manager.md).  
  
 Pour plus d'informations sur la mise en cache du dataset de référence, consultez [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
> [!NOTE]  
>  La transformation du cache écrit uniquement des lignes uniques dans le gestionnaire de connexions du cache.  
  
 Dans un package unique, une seule transformation du cache peut écrire des données dans le même gestionnaire de connexions du cache. Si le package contient plusieurs transformations du cache, la première qui est appelée lors de l'exécution du package écrit les données dans le gestionnaire de connexions. Les opérations d'écriture des transformations du cache suivantes échouent.  
  
 Pour plus d’informations, consultez [Gestionnaire de connexions du cache](../../../integration-services/data-flow/transformations/cache-connection-manager.md).  
  
## <a name="configuration-of-the-cache-transform"></a>Configuration de la transformation du cache  
 Vous pouvez configurer le gestionnaire de connexions du cache afin d'enregistrer les données dans un fichier cache (.caw).  
  
 Vous pouvez configurer la transformation du cache comme suit :  
  
-   Spécifiez le gestionnaire de connexions du cache.  
  
-   Mappez les colonnes d'entrée dans la transformation du cache aux colonnes de destination dans le gestionnaire de connexions du cache.  
  
    > [!NOTE]  
    >  Chaque colonne d'entrée doit être mappée à une colonne de destination et les types de données de colonne doivent correspondre. Sinon, le Concepteur [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] affiche un message d'erreur.  
  
     Vous pouvez utiliser l' **Éditeur du gestionnaire de connexions du cache** pour modifier les types de données et les noms de colonne, ainsi que d'autres propriétés de colonne.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] . Pour plus d'informations sur les propriétés que vous pouvez définir dans la boîte de dialogue **Éditeur avancé** , consultez [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="cache-transformation-editor-connection-manager-page"></a>Éditeur de transformation du cache (Page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de transformation du cache** pour sélectionner un gestionnaire de connexions du cache existant ou en créer un.  
  
 Pour en savoir plus sur le gestionnaire de connexions du cache, consultez [Cache Connection Manager](../../../integration-services/data-flow/transformations/cache-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Cache connection manager**  
 Sélectionnez un gestionnaire de connexions du cache existant en utilisant la liste, ou créez une connexion en utilisant le bouton **Nouvelle** .  
  
 **Nouveau**  
 Créez une connexion à l'aide de la boîte de dialogue Éditeur du gestionnaire de connexions du cache.  
  
 **Modifier**  
 Modifiez une connexion existante.  
  
## <a name="see-also"></a> Voir aussi  
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)  
  
  
