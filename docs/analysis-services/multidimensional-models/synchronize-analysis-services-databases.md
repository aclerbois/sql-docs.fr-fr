---
title: Synchroniser les bases de données Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b50cdb2a9b6a32fbd2794e3265dc009f6c6e6bd2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="synchronize-analysis-services-databases"></a>Synchroniser des base de données Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] comprend une fonctionnalité de synchronisation de bases de données qui rend deux bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] équivalentes en copiant les données et les métadonnées depuis une base de données sur un serveur source vers une base de données sur un serveur de destination. Utilisez la fonctionnalité de synchronisation de bases de données pour effectuer l'une des tâches suivantes :  
  
-   Déployer une base de données à partir d'un serveur de mise en lots vers un serveur de production.  
  
-   Mettre à jour une base de données sur un serveur de production avec les modifications apportées aux données et aux métadonnées dans une base de données sur un serveur de mise en lots.  
  
-   Générer un script XMLA qui pourra être exécuté à l'avenir pour synchroniser les bases de données.  
  
-   Dans les charges de travail distribuées où les cubes et les dimensions sont traités sur plusieurs serveurs, utilisez la synchronisation de bases de données pour fusionner les modifications dans une seule base de données.  
  
 La synchronisation de bases de données est initiée sur le serveur de destination, et les données et les métadonnées sont extraites dans une copie de base de données sur le serveur source. Si la base de données n'existe pas, elle est créée. La synchronisation est une opération unidirectionnelle et unique qui s'achève une fois que la base de données est copiée. Elle ne fournit pas de parité en temps réel entre les bases de données.  
  
 Vous pouvez resynchroniser les bases de données qui existent sur les serveurs source et destination pour extraire les dernières modifications d'un serveur de mise en lots dans une base de données de production. Les fichiers sur les deux serveurs seront comparés pour détecter les modifications et les données modifiées seront mises à jour. Une base de données existante sur un serveur de destination reste disponible pendant que la synchronisation se produit en arrière-plan. Les utilisateurs peuvent continuer à interroger la base de données de destination pendant la synchronisation. Une fois la synchronisation terminée, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] bascule automatiquement les utilisateurs vers les données et les métadonnées qui viennent d’être copiées et supprime les anciennes données de la base de données de destination.  
  
 Pour synchroniser des bases de données, exécutez l'Assistant Synchronisation de base de données pour synchroniser immédiatement les bases de données, ou utilisez-le pour générer un script de synchronisation que vous pouvez exécuter ultérieurement. Chacune de ces approches peut être utilisée pour augmenter la disponibilité et l'extensibilité de vos bases de données et du cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
> [!NOTE]  
>  Les livres blancs suivants, écrits pour les versions antérieures d'Analysis Services, s'appliquent également aux solutions multidimensionnelles évolutives créées à l'aide de SQL Server 2012. Pour plus d’informations, consultez [Scale-Out Querying with Analysis Services](http://go.microsoft.com/fwlink/?LinkId=253136) (Requêtes de scale-out avec Analysis Services) et [Scale-Out Querying for Analysis Services with Read-Only Databases](http://go.microsoft.com/fwlink/?LinkId=253137.)(Requêtes de scale-out pour Analysis Services avec des bases de données en lecture seule)  
  
## <a name="prerequisites"></a>Configuration requise  
 Vous devez être membre du rôle d'administrateur de serveur Analysis Services sur le serveur de destination (ou cible) à partir duquel vous lancez la synchronisation de bases de données. Sur le serveur source, votre compte d'utilisateur Windows doit disposer des autorisations de contrôle total sur la base de données source. Si vous synchroniserez les bases de données de manière interactive, souvenez-vous que la synchronisation s'exécute sous le contexte de sécurité de votre identité d'utilisateur Windows. Si votre compte n'a pas accès à des objets spécifiques, ces objets seront exclus de l'opération. Pour plus d’informations sur les rôles d’administrateur de serveur et les autorisations de base de données, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md) et [Octroyer des autorisations de base de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md).  
  
 Le port TCP 2383 doit être ouvert sur les deux serveurs pour autoriser les connexions distantes entre les instances par défaut. Pour plus d’informations sur la création d’une exception dans le Pare-feu Windows, consultez [Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
 Les serveurs source et de destination doivent être la même version et le service pack. Étant donné que les métadonnées du modèle sont également synchronisée, pour garantir la compatibilité de la build nombre pour les deux serveurs doit être le même. L'édition de chaque installation doit prendre en charge la synchronisation de bases de données. Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la synchronisation de bases de données est prise en charge dans les éditions Enterprise, Developer et Business Intelligence. Pour plus d’informations sur les fonctionnalités de chaque édition, consultez [éditions et les fonctionnalités prises en charge pour SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Le mode de déploiement du serveur doit être identique sur chaque serveur. Si la base de données que vous synchroniserez est multidimensionnelle, les serveurs source et de destination doivent être configurés pour le mode de serveur multidimensionnel. Pour plus d’informations sur les modes de déploiement, consultez [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md).  
  
 Désactivez le traitement différé des agrégations si vous l'utilisez sur le serveur source. Les agrégations traitées en arrière-plan peuvent interférer avec la synchronisation de bases de données. Pour plus d’informations sur la définition de cette propriété de serveur, consultez [Propriétés OLAP](../../analysis-services/server-properties/olap-properties.md).  
  
> [!NOTE]  
>  La taille de la base de données est un facteur qui détermine si la synchronisation est une approche appropriée. Il n’existe aucune condition matérielle requise, mais si la synchronisation est trop lente, envisagez de synchroniser plusieurs serveurs en parallèle, comme décrit dans cet article technique : [Analysis Services Synchronization Best Practices](http://go.microsoft.com/fwlink/?LinkID=253136)(Bonnes pratiques en matière de synchronisation Analysis Services).  
  
## <a name="synchronize-database-wizard"></a>Assistant Synchronisation de base de données  
 Utilisez l'Assistant Synchronisation de base de données pour exécuter une synchronisation unidirectionnelle d'une base de données source à une base de données de destination, ou pour générer un script qui spécifie une opération de synchronisation de bases de données. Vous pouvez synchroniser des partitions locales et distantes au cours du processus de synchronisation et choisir si inclure les rôles.  
  
 Cet Assistant vous guide au cours des différentes étapes :  
  
-   sélection de l'instance et de la base de données source à partir desquelles effectuer la synchronisation ;  
  
-   sélection des emplacements de stockage des partitions locales dans l'instance de destination ;  
  
-   sélection des emplacements de stockage des partitions distantes dans d'autres instances de destination ;  
  
-   sélection du niveau de sécurité et des informations d'appartenance à copier à partir de l'instance et de la base de données source vers l'instance de destination ;  
  
-   sélection de la synchronisation immédiate ou de l’enregistrement de la commande **Synchroniser** XML for Analysis (XMLA) générée par l’Assistant Synchronisation de base de données dans un fichier de script pour synchronisation ultérieure.  
  
 Par défaut, l'Assistant synchronise toutes les données et les métadonnées hormis l'appartenance à des groupes de sécurité existants. Vous pouvez aussi copier ou ignorer tous les paramètres de sécurité lorsque vous synchronisez les données et les métadonnées.  
  
#### <a name="run-the-wizard"></a>Exécuter l'assistant  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui exécutera la base de données de destination. Par exemple, si vous déployez une base de données sur un serveur de production, vous devez exécuter l'Assistant sur le serveur de production.  
  
2.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le dossier **Bases de données** , puis sélectionnez **Synchroniser**.  
  
3.  Spécifiez le serveur source et la base de données source. Dans la page Sélectionnez la base de données à synchroniser, dans **Serveur source** et **Base de données source**, entrez le nom du serveur source et de la base de données source. Par exemple, si vous déployez à partir d'un environnement de test vers un serveur de production, la source est la base de données dans le serveur de mise en lots.  
  
     **Serveur de destination** affiche le nom de l’instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] avec laquelle sont synchronisées les données et les métadonnées de la base de données sélectionnée dans **Base de données source** .  
  
     La synchronisation se produit pour les bases de données source et de destination qui portent le même nom. Si le serveur de destination a déjà une base de données qui partage le même nom que la base de données source, la base de données de destination est mise à jour avec les métadonnées et les données de la source. Si la base de données n'existe pas, elle sera créée sur le serveur de destination.  
  
4.  Éventuellement, changez l'emplacement de la partition locale. Utilisez la page **Spécifier les emplacements pour les partitions locales** pour indiquer où les partitions locales doivent être stockées sur le serveur de destination.  
  
    > [!NOTE]  
    >  Cette page ne s'affiche que s'il existe au moins une partition locale dans la base de données spécifiée.  
  
     Si un ensemble de partitions est installé sur le lecteur C du serveur source, l'Assistant vous permet de copier cet ensemble de partitions vers un emplacement différent sur le serveur de destination. Si vous ne modifiez pas les emplacements par défaut, l'Assistant déploie les partitions du groupe de mesures de chaque cube sur le serveur source aux mêmes emplacements sur le serveur de destination. De même, si le serveur source utilise des partitions distantes, les mêmes partitions distantes seront utilisées sur le serveur de destination.  
  
     L’option **Emplacements** affiche une grille indiquant le dossier source, le dossier de destination et la taille estimée des partitions locales à stocker sur l’instance de destination. Cette grille comporte les colonnes suivantes :  
  
     **Dossier source**  
     Affiche le nom du dossier de l’instance source de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contenant la partition locale. Si cette colonne contient la valeur « (Par défaut) », l'emplacement par défaut de l'instance source contient la partition locale.  
  
     **Dossier de destination**  
     Affiche le nom du dossier de l’instance de destination de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans lequel la partition locale doit être synchronisée. Si cette colonne contient la valeur « (Par défaut) », l'emplacement par défaut de l'instance de destination contient la partition locale.  
  
     Cliquez sur le bouton avec des points de suspension (**...**) pour ouvrir la boîte de dialogue **Rechercher un dossier distant** et spécifier un dossier sur l’instance de destination dans lequel synchroniser les partitions locales stockées à l’emplacement sélectionné.  
  
    > [!NOTE]  
    >  Cette colonne ne peut pas être modifiée pour les partitions locales stockées dans l'emplacement par défaut pour l'instance source.  
  
     **Taille**  
     Affiche la taille estimée de la partition locale.  
  
     L’option **Partitions à l’emplacement sélectionné** affiche une grille décrivant les partitions locales stockées à l’emplacement de l’instance source de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] spécifié dans la colonne **Dossier source** de la ligne sélectionnée dans **Emplacements**.  
  
     **Cube**  
     Affiche le nom du cube contenant cette partition.  
  
     **Groupe de mesures**  
     Affiche le nom du groupe de mesures dans le cube contenant cette partition.  
  
     **Nom de la partition**  
     Affiche le nom de la partition.  
  
     **Taille (Mo)**  
     Affiche la taille, en mégaoctets (Mo), de la partition.  
  
5.  Éventuellement, modifiez l’emplacement des partitions distantes. Utilisez la page **Spécifier les emplacements pour les partitions distantes** pour indiquer si les partitions distantes gérées par la base de données spécifiée du serveur source doivent être synchronisées, et pour spécifier une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une base de données de destination dans lesquelles stocker les partitions distantes sélectionnées.  
  
    > [!NOTE]  
    >  Cette page ne s’affiche que si une partition distante au moins est gérée par la base de données spécifiée sur l’instance source de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
     L’option **Emplacements** affiche une grille donnant les détails sur les emplacements dans lesquels sont stockées les partitions distantes de la base de données source, notamment les informations de source et de destination ainsi que la taille du stockage utilisé par chaque emplacement, disponibles dans la base de données sélectionnée. Cette grille comporte les colonnes suivantes :  
  
     **Synchronisation**  
     Sélectionnez cette option pour inclure un emplacement contenant les partitions distantes durant la synchronisation.  
  
    > [!NOTE]  
    >  Si cette option n'est pas sélectionnée pour un emplacement, les partitions distantes contenues dans cet emplacement ne seront pas synchronisées.  
  
     **Serveur source**  
     Affiche le nom de l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contenant les partitions distantes.  
  
     **Dossier source**  
     Affiche le nom du dossier de l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] contenant les partitions distantes. Si cette colonne contient la valeur « (Par défaut) », l’emplacement par défaut de l’instance affichée dans **Serveur source** contient les partitions distantes.  
  
     **Serveur de destination**  
     Affiche le nom de l’instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans laquelle les partitions distantes stockées à l’emplacement spécifié dans **Serveur source** et **Dossier source** doivent être synchronisées.  
  
     Cliquez sur le bouton avec des points de suspension (**...**) pour ouvrir la boîte de dialogue **Gestionnaire de connexions** et spécifier une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans laquelle synchroniser les partitions distantes stockées à l’emplacement spécifié.  
  
     **Dossier de destination**  
     Affiche le nom du dossier de l'instance de destination [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vers lequel la partition distante doit être synchronisée. Si cette colonne contient la valeur « (Par défaut) », l'emplacement par défaut de l'instance de destination doit contenir la partition distante.  
  
     Cliquez sur le bouton avec des points de suspension (**...**) pour ouvrir la boîte de dialogue **Rechercher un dossier distant** et spécifier un dossier sur l’instance de destination dans lequel synchroniser les partitions distantes stockées à l’emplacement sélectionné.  
  
     **Taille**  
     Affiche la taille estimée des partitions distantes stockées dans l'emplacement.  
  
     L’option **Partitions à l’emplacement sélectionné** affiche une grille décrivant les partitions distantes stockées à l’emplacement de l’instance source de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] spécifié dans la colonne **Dossier source** de la ligne sélectionnée dans **Emplacements**. Cette grille comporte les colonnes suivantes :  
  
     **Cube**  
     Affiche le nom du cube contenant cette partition.  
  
     **Groupe de mesures**  
     Affiche le nom du groupe de mesures dans le cube contenant cette partition.  
  
     **Nom de la partition**  
     Affiche le nom de la partition.  
  
     **Taille (Mo)**  
     Affiche la taille, en mégaoctets (Mo), de la partition.  
  
6.  Spécifiez si les informations d'autorisation de l'utilisateur doivent être incluses, et si la compression doit être utilisée. Par défaut, l'Assistant compresse toutes les données et les métadonnées avant de copier les fichiers sur le serveur de destination. Cette option permet une transmission plus rapide des fichiers. Les fichiers sont décompressés une fois qu'ils atteignent le serveur de destination.  
  
     **Copier tout**  
     Sélectionnez cette option pour inclure les définitions de sécurité et les informations d'appartenance durant la synchronisation.  
  
     **Ignorer l'appartenance**  
     Sélectionnez cette option pour inclure les définitions de sécurité mais exclure les informations d'appartenance durant la synchronisation.  
  
     **Ignorer tout**  
     Sélectionnez cette option pour ignorer la définition de sécurité et les informations d'appartenance dans la base de données source. Si une base de données de destination est créée au cours de la synchronisation, aucune définition de sécurité ou information d'appartenance ne sera copiée. Si la base de données de destination existe déjà et possède des rôles et des membres, ces informations de sécurité sont conservées.  
  
7.  Choisissez la méthode de synchronisation. Vous pouvez synchroniser immédiatement ou générer un script qui sera stocké dans un fichier. Par défaut, le fichier est enregistré avec l'extension .xmla et est stocké dans votre dossier Documents.  
  
8.  Cliquez sur **Terminer** pour effectuer la synchronisation. Après avoir vérifié les options dans la page **Fin de l’Assistant** , cliquez à nouveau sur **Terminer** .  
  
## <a name="next-steps"></a>Étapes suivantes  
 Si vous n'avez pas synchronisé les rôles ou l'appartenance, n'oubliez pas de spécifier les autorisations d'accès utilisateur sur la base de données de destination.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément Synchronize &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md)   
 [Déployer des Solutions de modèle à l’aide de XMLA](../../analysis-services/multidimensional-models/deploy-model-solutions-using-xmla.md)   
 [Déployer des Solutions de modèle à l’aide de l’Assistant de déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md)  
  
  
