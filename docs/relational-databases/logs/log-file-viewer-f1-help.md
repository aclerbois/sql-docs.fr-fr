---
title: Aide sur la visionneuse du fichier journal via la touche F1 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sql13.swb.configurelogs.errorlog.f1
helpviewer_keywords:
- Log File Viewer
ms.assetid: 2243845c-4880-4aa0-9ee8-0a97a128996b
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0a76b3fcf34f246e56e45e6f058fc78a23274d05
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="log-file-viewer-f1-help"></a>Aide sur la visionneuse du fichier journal via la touche F1
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La visionneuse du fichier journal affiche les informations de journalisation de nombreux composants différents. Après avoir ouvert la visionneuse du fichier journal, utilisez le volet **Sélectionner les journaux** pour sélectionner les journaux à afficher. Chaque journal affiche des colonnes appropriées à ce type de journal.  
  
 Les journaux disponibles dépendent de la manière dont la visionneuse du fichier journal est ouverte. Pour plus d’informations, consultez [Ouvrir la Visionneuse du fichier journal](../../relational-databases/logs/open-log-file-viewer.md).  
  
 Vous pouvez configurer le nombre de lignes qui s’affichent pour les journaux d’audit dans la page **Explorateur d’objets SQL Server/Commandes** de la boîte de dialogue **Outils/Options**. Pour obtenir la description des colonnes qui s’affichent pour les journaux d’audit, consultez [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md).  
  
## <a name="options"></a>Options  
 **Charger le journal**  
 Ouvre une boîte de dialogue dans laquelle vous pouvez spécifier un fichier journal à charger.  
  
 **Exporter**  
 Ouvre une boîte de dialogue qui vous permet d’exporter les informations figurant dans la grille **Résumé du fichier journal** vers un fichier texte.  
  
 **Actualiser**  
 Actualise l'affichage des journaux sélectionnés. Le bouton **Actualiser** permet de relire les journaux sélectionnés à partir du serveur cible lors de l'application des paramètres de filtre.  
  
 **Filtre**  
 Ouvre une boîte de dialogue qui vous permet de spécifier les paramètres utilisés pour filtrer le fichier journal, notamment **Connexion**, **Date**et d’autres critères de filtre **Général** .  
  
 **Recherche**  
 Permet de rechercher un texte spécifique dans le fichier journal. La recherche des caractères génériques n'est pas prise en charge.  
  
 **Arrêter**  
 Arrête le chargement des entrées du fichier-journal. Par exemple, vous pouvez utiliser cette option si un fichier de journal distant ou hors connexion est long à charger, et que vous souhaitez seulement consulter les entrées les plus récentes.  
  
 **Résumé du fichier journal**  
 Ce volet d'informations affiche un résumé du filtrage du fichier journal. Si le fichier n'est pas filtré, le texte suivant s'affiche : **Aucun filtre appliqué**. Si un filtre est appliqué au journal, le texte suivant s’affiche : **Filtrer les entrées du journal pour :** \<critères de filtre>.  
  
 **Détails de la ligne sélectionnée**  
 Sélectionnez une ligne pour afficher des détails supplémentaires sur la ligne d'événement sélectionnée en bas de la page. Vous pouvez changer l'ordre des colonnes en les faisant glisser sur la grille. Vous pouvez redimensionner les colonnes en faisant glisser les barres de séparation des colonnes dans l'en-tête de la grille vers la gauche ou la droite. Double-cliquez sur les barres de séparation des colonnes dans l'en-tête de la grille pour ajuster automatiquement la largeur de la colonne au contenu.  
  
 **Instance**  
 Nom de l'instance pour laquelle l'événement s'est produit. Il est affiché sous la forme *nom de l'ordinateur*\\*nom de l'instance*.  
  
## <a name="frequently-displayed-columns"></a>Colonnes fréquemment affichées  
 **Date**  
 Affiche la date de l'événement.  
  
 **Source**  
 Affiche la fonctionnalité source à partir de laquelle l'événement est créé, par exemple le nom du service (comme MSSQLSERVER). Ceci n'apparaît pas pour tous les types de journaux.  
  
 **Message**  
 Affiche les messages associés à l'événement.  
  
 **Type de journal**  
 Affiche le type de journal auquel appartient l'événement. Tous les journaux sélectionnés s'affichent dans la fenêtre de résumé du fichier journal.  
  
 **Source du journal**  
 Affiche une description du journal source dans lequel l'événement est capturé.  
  
## <a name="permissions"></a>Autorisations  
 Pour accéder aux fichiers journaux pour les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont en ligne, l’appartenance au rôle serveur fixe securityadmin est obligatoire.  
  
 L’accès aux fichiers journaux des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hors connexion nécessite un accès en lecture à l’espace de noms WMI **Root\Microsoft\SqlServer\ComputerManagement10** et au dossier où sont stockés les fichiers journaux. Pour plus d’informations, consultez la section Sécurité de la rubrique [Afficher les fichiers journaux hors connexion](../../relational-databases/logs/view-offline-log-files.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Visionneuse du fichier journal](../../relational-databases/logs/log-file-viewer.md)   
 [Ouvrir la Visionneuse du fichier journal](../../relational-databases/logs/open-log-file-viewer.md)   
 [Afficher les fichiers journaux hors connexion](../../relational-databases/logs/view-offline-log-files.md)  
  
  
