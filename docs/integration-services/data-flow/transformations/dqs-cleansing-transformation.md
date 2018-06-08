---
title: Nettoyage DQS, transformation | Microsoft Docs
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
f1_keywords:
- sql13.ssdqs.designer.cleansing.f1
- sql13.SSDQS.DESIGNER.DQCONNECTION.F1
helpviewer_keywords:
- data correction
- correct data
ms.assetid: d2ec1b1a-c745-4741-b57c-6fdb524a154c
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5683712d118b9bb378b9e6c2beabac6af721a21a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dqs-cleansing-transformation"></a>Transformation de nettoyage DQS
  La transformation de nettoyage DQS utilise les services Data Quality Services (DQS) pour corriger des données provenant d'une source de données connectée en appliquant des règles approuvées créées pour la source de données connectée ou une source de données similaire. Pour plus d'informations sur les règles de correction des données, consultez [DQS Knowledge Bases and Domains](../../../data-quality-services/dqs-knowledge-bases-and-domains.md). Pour plus d'informations sur DQS, consultez [Data Quality Services Concepts](../../../data-quality-services/data-quality-services-concepts.md).  
  
 Pour déterminer si les données doivent être corrigée, la transformation de nettoyage DQS traite les données d'une colonne d'entrée lorsque les conditions suivantes sont remplies :  
  
-   La colonne est sélectionnée pour la correction des données.  
  
-   Le type de données de la colonne est pris en charge pour la correction des données.  
  
-   La colonne est mappée à un domaine dont le type de données est compatible.  
  
 La transformation inclut également une sortie d'erreur que vous configurez pour gérer les erreurs au niveau des lignes. Pour configurer la sortie d'erreur, utilisez l' **Éditeur de transformation de nettoyage DQS**.  
  
 Vous pouvez inclure la [Fuzzy Grouping Transformation](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md) dans le flux de données pour identifier les lignes de données susceptibles d'être en double.  
  
## <a name="data-quality-projects-and-values"></a>Projets de qualité des données et valeurs  
 Lorsque vous traitez des données avec la transformation de nettoyage DQS, un projet de nettoyage est créé sur le serveur de qualité des données. Vous utilisez le client de qualité des données pour gérer le projet. En outre, vous pouvez utiliser le client de qualité des données pour importer les valeurs du projet dans un domaine de base de connaissances DQS. Vous pouvez importer les valeurs uniquement vers un domaine (ou un domaine lié) configuré pour être utilisé par la transformation de nettoyage DQS.  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Ouvrir des projets Integration Services dans Data Quality Client](../../../data-quality-services/open-integration-services-projects-in-data-quality-client.md)  
  
-   [Import Cleansing Project Values into a Domain](../../../data-quality-services/import-cleansing-project-values-into-a-domain.md)  
  
-   [Appliquer des règles de qualité des données à la source de données](../../../integration-services/data-flow/transformations/apply-data-quality-rules-to-data-source.md)  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Ouvrir, déverrouiller, renommer et supprimer un projet de qualité des données](../../../data-quality-services/open-unlock-rename-and-delete-a-data-quality-project.md)  
  
-   Article [Nettoyage de données complexes à l'aide des domaines composites](http://social.technet.microsoft.com/wiki/contents/articles/13324.using-dqs-cleansing-complex-data-using-composite-domains.aspx), sur social.technet.microsoft.com.  
  
## <a name="dqs-cleansing-transformation-editor-dialog-box"></a>Éditeur de transformation de nettoyage DQS (boîte de dialogue)
  Utilisez la boîte de dialogue de **l’Éditeur de transformation de nettoyage DQS** pour corriger les données à l’aide de DQS (Data Quality Services). Pour plus d’informations, consultez [Concepts Data Quality Services](../../../data-quality-services/data-quality-services-concepts.md).  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir l'Éditeur de transformation de nettoyage DQS](#open)  
  
-   [Définir les options de l'onglet Gestionnaires de connexions](#connection)  
  
-   [Définir les options sur l'onglet mappage](#mapping)  
  
-   [Définir les options de l'onglet Avancé](#advanced)  
  
-   [Définir les options dans la boîte de dialogue Gestionnaire de connexions du nettoyage DQS](#manager)  
  
###  <a name="open"></a> Ouvrir l'Éditeur de transformation de nettoyage DQS  
  
1.  Ajoutez la transformation de nettoyage DQS au package [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
2.  Cliquez avec le bouton droit sur le composant, puis cliquez sur **Modifier**.  
  
###  <a name="connection"></a> Définir les options de l'onglet Gestionnaires de connexions  
 **Gestionnaire de connexions Data Quality Services**  
 Sélectionnez un gestionnaire de connexions DQS existant dans la liste, ou créez une connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Gestionnaire de connexions du nettoyage DQS** . Consultez [Définir les options dans la boîte de dialogue Gestionnaire de connexions du nettoyage DQS](#manager)  
  
 **Base de connaissances Data Quality Services**  
 Sélectionnez une base de connaissances DQS existante pour la source de données connectée. Pour plus d’informations sur la base de connaissances DQS, consultez [Bases de connaissances et domaines DQS](../../../data-quality-services/dqs-knowledge-bases-and-domains.md).  
  
 **Chiffrer la connexion**  
 Spécifiez s’il faut chiffrer la connexion, afin de chiffrer le transfert des données entre le serveur DQS et [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 **Champs disponibles**  
 Répertorie les champs disponibles pour la base de connaissances sélectionnée. Il existe deux types de champs : domaines uniques, et domaines composés qui contiennent au moins deux domaines uniques.  
  
 Pour plus d’informations sur la façon de mapper des colonnes à des domaines composites, consultez [Mapper des colonnes à des domaines composites](../../../integration-services/data-flow/transformations/map-columns-to-composite-domains.md).  
  
 Pour plus d’informations sur les domaines, consultez [Bases de connaissances et domaines DQS](../../../data-quality-services/dqs-knowledge-bases-and-domains.md).  
  
 **Configurer la sortie d'erreur**  
 Spécifiez le mode de gestion des erreurs au niveau des lignes. Des erreurs peuvent se produire lorsque la transformation corrige des données de la source de données connectée, en raison de valeurs de données ou de contraintes de validation inattendues.  
  
 Les valeurs suivantes sont valides :  
  
-   **Composant défaillant**, qui indique que la transformation a échoué et que les données d’entrée ne sont pas insérées dans la base de données Data Quality Services. Il s'agit de la valeur par défaut.  
  
-   **Réacheminer la ligne**, qui indique que les données d’entrée ne sont pas insérées dans la base de données Data Quality Services et qu’elles sont redirigées vers la sortie d’erreur.  
  
###  <a name="mapping"></a> Définir les options sur l'onglet mappage  
 Pour plus d’informations sur la façon de mapper des colonnes à des domaines composites, consultez [Mapper des colonnes à des domaines composites](../../../integration-services/data-flow/transformations/map-columns-to-composite-domains.md).  
  
 **Colonnes d'entrée disponibles**  
 Répertorie les colonnes à partir de la source de données connectée. Sélectionnez une ou plusieurs colonnes qui contiennent les données que vous souhaitez corriger.  
  
 **Colonne d'entrée**  
 Répertorie une colonne d’entrée sélectionnée dans la zone **Colonnes d’entrée disponibles** .  
  
 **Domaine**  
 Sélectionnez un domaine pour la mapper à la colonne d'entrée.  
  
 **Alias source**  
 Répertorie la colonne source qui contient la valeur de colonne d'origine.  
  
 Cliquez dans le champ pour modifier le nom de colonne.  
  
 **Alias de sortie**  
 Répertorie la colonne générée par la **Transformation de nettoyage DQS**. Cette colonne contient la valeur de colonne d'origine ou la valeur corrigée.  
  
 Cliquez dans le champ pour modifier le nom de colonne.  
  
 **Alias d'état**  
 Répertorie la colonne qui contient les informations d'état des données corrigées. Cliquez dans le champ pour modifier le nom de colonne.  
  
###  <a name="advanced"></a> Définir les options de l'onglet Avancé  
 **Normaliser la sortie**  
 Indique s'il faut retourner les données dans un format standardisé en fonction du format de sortie défini pour les domaines. Pour plus d’informations sur le format standardisé, consultez [Nettoyage de données](../../../data-quality-services/data-cleansing.md).  
  
 **Confiance**  
 Indique s'il faut inclure le niveau de confiance pour les données corrigées. Le niveau de confiance indique le degré de certitude de DQS pour la correction ou la suggestion. Pour plus d’informations sur les niveaux de confiance, consultez [Nettoyage de données](../../../data-quality-services/data-cleansing.md).  
  
 **Reason**  
 Indique s'il faut inclure la raison de la correction des données.  
  
 **Données ajoutées**  
 Indique s'il faut retourner des informations supplémentaires reçues d'un fournisseur de données de référence existant. Pour plus d’informations, consultez [Reference Data Services in DQS](../../../data-quality-services/reference-data-services-in-dqs.md).  
  
 **Schéma de données ajoutées**  
 Indique s'il faut retourner le schéma de données. Pour plus d’informations, consultez [Attacher un domaine ou un domaine composite à des données de référence](../../../data-quality-services/attach-domain-or-composite-domain-to-reference-data.md).  
  
###  <a name="manager"></a> Définir les options dans la boîte de dialogue Gestionnaire de connexions du nettoyage DQS  
 **Nom du serveur**  
 Sélectionnez ou entrez le nom du serveur DQS auquel vous souhaitez vous connecter. Pour plus d’informations sur le serveur, consultez [Administration de DQS](../../../data-quality-services/dqs-administration.md).  
  
 **Tester la connexion**  
 Cliquez pour confirmer que la connexion que vous avez spécifiée fonctionne.  
  
 Vous pouvez également ouvrir la boîte de dialogue **Gestionnaire de connexions du nettoyage DQS** à partir de la zone de connexions, en procédant comme suit :  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez un projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] ou créez-en un nouveau.  
  
2.  Cliquez avec le bouton droit dans la zone des connexions, cliquez sur **Nouvelle connexion**, puis cliquez sur **DQS**.  
  
3.  Cliquez sur **Ajouter**.  
  
