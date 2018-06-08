---
title: Afficher ou modifier des filtres et des analyseurs lexicaux inscrits | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], word breakers
- full-text search [SQL Server], filters
- filters [full-text search]
- word breakers [full-text search]
ms.assetid: f88c54df-b1aa-4701-807f-dc92c32363fd
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5a34f6aef9577d2b2a8bc204bbb69801c0218a11
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="view-or-change-registered-filters-and-word-breakers"></a>Afficher ou modifier des filtres et des analyseurs lexicaux inscrits
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Après l'installation ou la désinstallation des analyseurs lexicaux ou des filtres sur un système, les modifications n'entrent pas automatiquement en vigueur sur les instances de serveur. Cette rubrique explique comment afficher les analyseurs lexicaux ou les filtres actuellement inscrits et comment inscrire les analyseurs lexicaux et les filtres récemment installés sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="to-view-a-list-of-languages-whose-word-breakers-are-currently-registered"></a>Pour afficher la liste des langues dont les analyseurs lexicaux sont actuellement inscrits  
  
1.  Utilisez la vue de catalogue [sys.fulltext_languages](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md) , comme suit :  
  
    ```  
    SELECT * FROM sys.fulltext_languages;   
    ```  
  
### <a name="to-view-a-list-of-the-filters-that-are-currently-registered"></a>Pour afficher la liste des filtres actuellement inscrits  
  
1.  Utilisez la procédure stockée système [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md) , comme suit :  
  
    ```  
    EXEC sp_help_fulltext_system_components 'filter';    
    ```  
  
### <a name="to-register-newly-installed-word-breakers-and-filters"></a>Pour inscrire les analyseurs lexicaux et les filtres récemment installés  
  
1.  Utilisez la procédure stockée système [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md) pour mettre à jour la liste des langues, comme suit :  
  
    ```  
    exec sp_fulltext_service 'update_languages';   
    ```  
  
### <a name="to-unregister-uninstalled-word-breakers-and-filters"></a>Pour annuler l'inscription des analyseurs lexicaux et des filtres désinstallés  
  
1.  Utilisez **sp_fulltext_service** pour mettre à jour la liste des langues, comme suit :  
  
    ```  
    exec sp_fulltext_service 'update_languages'  
    ```  
  
2.  Utilisez **sp_fulltext_service** pour redémarrer les processus hôtes de démon de filtre (fdhost.exe), comme suit :  
  
    ```  
    exec sp_fulltext_service 'restart_all_fdhosts';  
    ```  
  
### <a name="to-replace-existing-word-breakers-or-filters-when-installing-new-ones"></a>Pour remplacer les analyseurs lexicaux ou les filtres existants et en installer de nouveaux  
  
1.  Lorsque vous préparez l'installation d'un fichier DLL qui contient de nouveaux analyseurs lexicaux ou filtres, assurez-vous que son nom est différent des noms de fichiers DLL existants installés sur votre instance de serveur.  
  
2.  Copiez le nouveau fichier .dll dans le répertoire qui contient les fichiers DDL [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] standard pour l'instance de serveur. L'emplacement par défaut est :  
  
     C:\Program Files\Microsoft SQL Server\MSSQL13.*nom_instance*\MSSQL\Binn  
  
    > [!IMPORTANT]  
    >  Il est fortement recommandé de charger uniquement des composants signés et vérifiés. Nous vous recommandons également d'exécuter le service de lancement FDHOST (MSSQLFDLauncher) avec le moins de privilèges possibles.  
  
3.  Installez les nouveaux analyseurs lexicaux ou filtres.  
  
     **Pour installer et charger des filtres Microsoft Filter Pack IFilters**  
  
    -   [Procédure : inscrire des filtres Microsoft Filter Pack IFilters avec SQL Server](http://go.microsoft.com/fwlink/?LinkId=130439)  
  
4.  Utilisez **sp_fulltext_service** pour charger les filtres et les analyseurs lexicaux récemment installés dans l’instance de serveur, comme suit :  
  
    ```  
    EXEC sp_fulltext_service @action='load_os_resources', @value=1;  
    ```  
  
5.  Utilisez **sp_fulltext_service** pour mettre à jour la liste des langues, comme suit :  
  
    ```  
    EXEC sp_fulltext_service 'update_languages';  
    ```  
  
6.  Redémarrez les processus hôtes de démon de filtre (fdhost.exe) à l’aide de **sp_fulltext_service** , comme suit :  
  
    ```  
    EXEC sp_fulltext_service 'restart_all_fdhosts';   
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Définir le compte du service du Lanceur de démon de filtre de texte intégral](../../relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md)   
 [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md)   
 [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)  
  
  
