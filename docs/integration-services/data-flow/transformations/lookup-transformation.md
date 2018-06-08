---
title: Recherche, transformation | Microsoft Docs
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
- sql13.dts.designer.lookuptrans.f1
- sql13.dts.designer.lookuptransformation.general.f1
- sql13.dts.designer.lookuptransformation.referencetable.f1
- sql13.dts.designer.lookuptransformation.columns.f1
- sql13.dts.designer.lookuptransformation.advanced.f1
helpviewer_keywords:
- Lookup transformation
- joining columns [Integration Services]
- cache [Integration Services]
- match exactly [Integration Services]
- lookups [Integration Services]
- exact matches [Integration Services]
ms.assetid: de1cc8de-e7af-4727-b5a5-a1f0a739aa09
caps.latest.revision: 106
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0681beed559734ecaee000a45be3bc19447d9eeb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lookup-transformation"></a>Transformation de recherche
  La transformation de recherche effectue des recherches en joignant des données de colonnes d'entrée à des colonnes d'un dataset de référence. Vous utilisez la recherche pour accéder à des informations supplémentaires dans une table associée se basant sur des valeurs dans des colonnes communes.  
  
 Le dataset de référence peut être un fichier cache, une table ou une vue existante, une nouvelle table ou le résultat d'une requête SQL. La transformation de recherche utilise un gestionnaire de connexions OLE DB ou un gestionnaire de connexions du cache pour se connecter au dataset de référence. Pour plus d’informations, consultez [Gestionnaire de connexions OLE DB](../../../integration-services/connection-manager/ole-db-connection-manager.md) et [Gestionnaire de connexions du cache](../../../integration-services/data-flow/transformations/cache-connection-manager.md)  
  
 Vous pouvez configurer la transformation de recherche comme suit :  
  
-   Sélectionnez le gestionnaire de connexions que vous souhaitez utiliser. Si vous souhaitez vous connecter à une base de données, sélectionnez un gestionnaire de connexions OLE DB. Si vous souhaitez vous connecter à un fichier cache, sélectionnez un gestionnaire de connexions du cache.  
  
-   Spécifiez la table ou la vue qui contient le dataset de référence.  
  
-   Générez un dataset de référence en spécifiant une instruction SQL.  
  
-   Définissez des jointures entre l'entrée et le dataset de référence.  
  
-   Ajoutez des colonnes du dataset de référence à la sortie de la transformation de recherche.  
  
-   Configurez les options de mise en cache.  
  
 La transformation de recherche prend en charge les fournisseurs de bases de données suivants pour le gestionnaire de connexions OLE DB :  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
-   Oracle  
  
-   DB2  
  
 La transformation de recherche essaie de réaliser une équijointure entre les valeurs de l'entrée de transformation et celles du dataset de référence. (Une équijointure signifie que chaque ligne de l'entrée de transformation doit correspondre à au moins une ligne du dataset de référence.) Si une équijointure n'est pas possible, la transformation de recherche effectue l'une des actions suivantes :  
  
-   En l'absence d'entrée correspondante dans le dataset de référence, aucune jointure n'est réalisée. Par défaut, la transformation de recherche traite les lignes sans entrées correspondantes comme des erreurs. Toutefois, vous pouvez configurer la transformation de recherche pour rediriger ces lignes vers une sortie sans correspondance.  
  
-   En présence de plusieurs correspondances dans la table de référence, la transformation de recherche retourne uniquement la première correspondance retournée par la requête de recherche. Si plusieurs correspondances sont trouvées, la transformation de recherche génère une erreur ou un avertissement uniquement lorsque la transformation a été configurée pour charger tout le dataset de référence dans le cache. Dans ce cas, la transformation de recherche génère un avertissement lorsque la transformation détecte plusieurs correspondances au moment où la transformation remplit le cache.  
  
 La jointure peut être une jointure composite, auquel cas vous pouvez joindre plusieurs colonnes de l'entrée de transformation à des colonnes du dataset de référence. La transformation prend en charge les colonnes de jointure de n'importe quel type de données, à l'exception des types DT_R4, DT_R8, DT_TEXT, DT_NTEXT ou DT_IMAGE. Pour plus d’informations, consultez [Types de données Integration Services](../../../integration-services/data-flow/integration-services-data-types.md).  
  
 En règle générale, les valeurs du dataset de référence sont ajoutées à la sortie de la transformation. Par exemple, la transformation de recherche peut extraire un nom de produit d'une table à l'aide d'une valeur d'une colonne d'entrée, puis l'ajouter à la sortie de la transformation. Les valeurs de la table de référence peuvent remplacer des valeurs de colonne ou être ajoutées à de nouvelles colonnes.  
  
 Les recherches effectuées par la transformation de recherche respectent la casse. Pour éviter les échecs de recherche dus à des différences de casse dans les données, utilisez tout d'abord la transformation de la table des caractères pour convertir les données en majuscules ou minuscules. Incluez ensuite les fonctions UPPER ou LOWER dans l'instruction SQL qui génère la table de référence. Pour plus d’informations, consultez [Transformation de la table de caractères](../../../integration-services/data-flow/transformations/character-map-transformation.md), [UPPER &#40;Transact-SQL&#41;](../../../t-sql/functions/upper-transact-sql.md) et [LOWER &#40;Transact-SQL&#41;](../../../t-sql/functions/lower-transact-sql.md).  
  
 La transformation de recherche a les entrées et sorties suivantes :  
  
-   Entrée.  
  
-   Sortie avec correspondance. La sortie avec correspondance gère les lignes de l'entrée de transformation qui correspondent à au moins une entrée du dataset de référence.  
  
-   Sortie sans correspondance. La sortie sans correspondance gère les lignes de l'entrée qui ne correspondent pas au moins à une entrée dans le dataset de référence. Si vous configurez la transformation de recherche pour traiter les lignes sans entrées correspondantes comme des erreurs, elles sont redirigées vers la sortie d'erreur. Sinon, la transformation redirige ces lignes vers la sortie sans correspondance.  
  
-   Sortie d'erreur.  
  
## <a name="caching-the-reference-dataset"></a>Mise en cache du dataset de référence  
 Un cache en mémoire stocke le dataset de référence et stocke une table de hachage qui indexe les données. Le cache reste en mémoire tant que l'exécution du package n'est pas terminée. Vous pouvez rendre le cache persistant dans un fichier cache (.caw).  
  
 Lorsque vous rendez le cache persistant dans un fichier, le système le charge plus rapidement, ce qui améliore les performances de la transformation de recherche et du package. N'oubliez pas que lorsque vous utilisez un fichier cache, vous travaillez avec des données qui ne sont pas autant à jour que celles de la base de données.  
  
 La persistance du cache dans un fichier présente les autres avantages suivants :  
  
-   ***Partagez le fichier cache entre plusieurs packages. Pour plus d’informations, consultez*** [Implémenter une transformation de recherche en mode Cache complet à l’aide de la transformation du gestionnaire de connexions du cache](../../../integration-services/data-flow/transformations/lookup-transformation-full-cache-mode-cache-connection-manager.md)***.***  
  
-   Déployez le fichier cache avec un package. ***Vous pouvez alors utiliser les données sur plusieurs ordinateurs.*** Pour plus d’informations, consultez [Créer et déployer un cache pour la transformation de recherche](../../../integration-services/data-flow/transformations/create-and-deploy-a-cache-for-the-lookup-transformation.md).  
  
-   Utilisez la source de fichier brut pour lire les données du fichier cache. Vous pouvez alors utiliser d'autres composants de flux de données pour transformer ou déplacer les données. Pour plus d’informations, consultez [Source de fichier brut](../../../integration-services/data-flow/raw-file-source.md).  
  
    > [!NOTE]  
    >  Le gestionnaire de connexions du cache ne prend pas en charge les fichiers cache qui sont créés ou modifiés avec la destination de fichier brut.  
  
-   Effectuez des opérations et définissez des attributs sur le fichier cache en utilisant la tâche de système de fichiers. Pour plus d’informations, consultez [Tâche de système de fichiers](../../../integration-services/control-flow/file-system-task.md).  
  
 Les options de mise en cache sont indiquées ci-dessous :  
  
-   Le dataset de référence est généré à l'aide d'une table, d'une vue ou d'une requête SQL et est chargé dans le cache, avant l'exécution de la transformation de recherche. Vous utilisez le gestionnaire de connexions OLE DB pour accéder au dataset.  
  
     Cette option de mise en cache est compatible avec l’option de mise en cache complète qui est disponible pour la transformation de recherche dans [!INCLUDE[ssISversion2005](../../../includes/ssisversion2005-md.md)].  
  
-   Le dataset de référence est généré à partir d'une source de données connectée dans le flux de données ou à partir d'un fichier cache et est chargé dans le cache avant l'exécution de la transformation de recherche. Vous utilisez le gestionnaire de connexions du cache et éventuellement la transformation du cache pour accéder au dataset. Pour plus d’informations, consultez [Gestionnaire de connexions du cache](../../../integration-services/data-flow/transformations/cache-connection-manager.md) et [Transformation du cache](../../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Le dataset de référence est généré à l'aide d'une table, d'une vue ou d'une requête SQL pendant l'exécution de la transformation de recherche. Les lignes avec des entrées correspondantes dans le dataset de référence et celles sans entrées correspondantes dans le dataset sont chargées dans le cache.  
  
     Lorsque la taille de mémoire du cache est dépassée, la transformation de recherche supprime automatiquement les lignes le moins fréquemment utilisées du cache.  
  
     Cette option de mise en cache est compatible avec l’option de mise en cache partielle qui est disponible pour la transformation de recherche dans [!INCLUDE[ssISversion2005](../../../includes/ssisversion2005-md.md)].  
  
-   Le dataset de référence est généré à l'aide d'une table, d'une vue ou d'une requête SQL pendant l'exécution de la transformation de recherche. Aucune donnée n'est mise en cache.  
  
     Cette option de mise en cache est compatible avec l’option sans mise en cache qui est disponible pour la transformation de recherche dans [!INCLUDE[ssISversion2005](../../../includes/ssisversion2005-md.md)].  
  
 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] diffèrent par leur manière de comparer les chaînes. Si la transformation de recherche est configurée pour charger le dataset de référence dans le cache avant l’exécution de la transformation de recherche, [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] effectue la comparaison de recherche dans le cache. Autrement, l’opération de recherche utilise une instruction SQL paramétrable et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] effectue la comparaison de recherche. Cela signifie que la transformation de recherche peut retourner un nombre différent de correspondances à partir de la même table de recherche, selon le type de cache.  
  
## <a name="related-tasks"></a>Related Tasks  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation. Pour plus d'informations, consultez les rubriques suivantes.  
  
-   [Implémenter une recherche en mode Aucun cache ou Cache partiel](../../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)  
  
-   [Implémenter une transformation de recherche en mode Cache complet à l'aide du gestionnaire de connexions du cache](../../../integration-services/data-flow/transformations/lookup-transformation-full-cache-mode-cache-connection-manager.md)  
  
-   [Implémenter une transformation de recherche en mode Cache complet à l'aide du gestionnaire de connexions OLE DB](../../../integration-services/data-flow/transformations/lookup-transformation-full-cache-mode-ole-db-connection-manager.md)  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-content"></a>Contenu associé  
  
-   Vidéo, [Procédure : implémenter une transformation de recherche en mode Cache complet](http://go.microsoft.com/fwlink/?LinkId=131031), sur msdn.microsoft.com  
  
-   Billet de blog, [Best Practices for Using the Lookup Transformation Cache Modes](http://go.microsoft.com/fwlink/?LinkId=146623)(Bonnes pratiques pour l’utilisation des modes de cache de transformation de recherche), sur blogs.msdn.com  
  
-   Billet de blog, [Lookup Pattern: Case Insensitive](http://go.microsoft.com/fwlink/?LinkId=157782)(Modèle de recherche : non-respect de la casse), sur blogs.msdn.com  
  
-   Exemple, [Lookup Transformation](http://go.microsoft.com/fwlink/?LinkId=267528)(Transformation de recherche), sur msftisprodsamples.codeplex.com.  
  
     Pour plus d’informations sur l’installation d’exemples de produits et de bases de données [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , consultez [SQL Server Integration Services Product Samples](http://go.microsoft.com/fwlink/?LinkId=267527)(Exemples de produits SQL Server Integration Services).  
  
## <a name="lookup-transformation-editor-general-page"></a>Éditeur de transformation de recherche (page Général)
  Utilisez la page **Général** de la boîte de dialogue Éditeur de transformation de recherche pour sélectionner le mode de cache ainsi que le type de connexion et pour spécifier comment gérer les lignes sans entrées correspondantes.  
  
### <a name="options"></a>Options  
 **Cache complet**  
 Générez et chargez le dataset de référence dans le cache avant l'exécution de la transformation de recherche.  
  
 **Cache partiel**  
 Générez le dataset de référence pendant l'exécution de la transformation de recherche. Chargez dans le cache les lignes avec des entrées correspondantes dans le dataset de référence et celles sans entrées correspondantes dans le dataset.  
  
 **Aucun cache**  
 Générez le dataset de référence pendant l'exécution de la transformation de recherche. Aucune donnée n'est chargée dans le cache.  
  
 **Gestionnaire de connexions du cache**  
 Configurez la transformation de recherche pour utiliser un gestionnaire de connexions du cache. Cette option n'est disponible que si l'option Cache complet est sélectionnée.  
  
 **Gestionnaire de connexions OLE DB**  
 Configurez la transformation de recherche pour utiliser un gestionnaire de connexions OLE DB.  
  
 **Spécifier comment gérer les lignes sans entrées correspondantes**  
 Sélectionnez une option pour gérer les lignes qui ne correspondent pas au moins à une entrée dans le dataset de référence.  
  
 Lorsque vous sélectionnez **Rediriger les lignes vers la sortie sans correspondance**, les lignes sont redirigées vers une sortie sans correspondance et ne sont pas gérées comme des erreurs. L'option **Erreur** figurant dans la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de transformation de recherche** n'est pas disponible.  
  
 Lorsque vous sélectionnez une autre option dans la zone de liste **Spécifier comment gérer les lignes sans entrées correspondantes** , les lignes sont gérées comme des erreurs. L'option **Erreur** de la page **Sortie d'erreur** est disponible.  
  
### <a name="external-resources"></a>Ressources externes  
 Entrée de blog, [Lookup cache modes](http://go.microsoft.com/fwlink/?LinkId=219518) sur blogs.msdn.com  
  
## <a name="lookup-transformation-editor-connection-page"></a>Éditeur de transformation de recherche (page Connexion)
  Utilisez la page **Connexion** de la boîte de dialogue **Éditeur de transformation de recherche** pour sélectionner un gestionnaire de connexions. Si vous sélectionnez un gestionnaire de connexions OLE DB, vous sélectionnez également une requête, une table ou une vue pour générer le dataset de référence.  
  
### <a name="options"></a>Options  
 Les options suivantes sont disponibles quand vous sélectionnez **Cache complet** et **Gestionnaire de connexions du cache** dans la page Général de la boîte de dialogue **Éditeur de transformation de recherche** .  
  
 **Cache connection manager**  
 Sélectionnez un gestionnaire de connexions du cache existant dans la liste ou créez une connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez une connexion à l’aide de la boîte de dialogue **Éditeur du gestionnaire de connexions du cache** .  
  
 Les options suivantes sont disponibles quand vous sélectionnez **Cache complet**, **Cache partiel**ou **Aucun cache**et **Gestionnaire de connexions OLE DB**dans la page Général de la boîte de dialogue **Éditeur de transformation de recherche** .  
  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez un gestionnaire de connexions OLE DB existant dans la liste ou créez une connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Crée une connexion en utilisant la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** .  
  
 **Utiliser une table ou une vue**  
 Sélectionnez une table ou une vue existante dans la liste ou créez une table en cliquant sur **Nouveau**.  
  
> [!NOTE]  
>  Si vous spécifiez une instruction SQL dans la page **Avancé** de l’ **Éditeur de transformation de recherche**, cette instruction SQL substitue et remplace le nom de table a sélectionné ici. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Avancé&#41;](../../../integration-services/data-flow/transformations/lookup-transformation-editor-advanced-page.md).  
  
 **Nouveau**  
 Utilisez la boîte de dialogue **Créer une table** pour créer une table.  
  
 **Utiliser les résultats d'une requête SQL**  
 Choisissez cette option pour rechercher une requête existante, générer une requête, vérifier la syntaxe d'une requête et afficher un aperçu des résultats d'une requête.  
  
 **Construire une requête**  
 Créez l’instruction Transact-SQL à exécuter à l’aide du **Générateur de requêtes**. Cet outil graphique permet de créer des requêtes en explorant les données.  
  
 **Parcourir**  
 Permet de rechercher une requête existante enregistrée dans un fichier.  
  
 **Analyser la requête**  
 Contrôle la syntaxe d'une requête.  
  
 **Aperçu**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Visualiser les résultats de la requête** . Cette option affiche jusqu'à 200 lignes.  
  
### <a name="external-resources"></a>Ressources externes  
 Entrée de blog, [Lookup cache modes](http://go.microsoft.com/fwlink/?LinkId=219518) sur blogs.msdn.com  
  
## <a name="lookup-transformation-editor-columns-page"></a>Éditeur de transformation de recherche (page Colonnes)
  Utilisez la page **Colonnes** de la boîte de dialogue **Éditeur de transformation de recherche** pour définir la jointure entre la table source et la table de référence, ainsi que pour sélectionner les colonnes de recherche dans la table de référence.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Les colonnes d'entrée représentent les colonnes dans le flux de données d'une source connectée. Les colonnes d'entrée et la colonne de recherche doivent avoir des types de données identiques.  
  
 Au moyen d'une opération glisser-déplacer, mappez les colonnes d'entrée disponibles aux colonnes de recherche.  
  
 Vous pouvez également mapper les colonnes d'entrée aux colonnes de recherche à l'aide du clavier. Pour ce faire, mettez en surbrillance une colonne dans la table **Colonnes d'entrée disponibles** , appuyez sur la touche de l'application, puis cliquez sur **Modifier les mappages**.  
  
 **Colonnes de recherche disponibles**  
 Affichez la liste des colonnes de recherche. Les colonnes de recherche représentent les colonnes de la table de référence dans lesquelles vous souhaitez rechercher des valeurs qui correspondent aux colonnes d'entrée.  
  
 Au moyen d'une opération glisser-déplacer, mappez les colonnes de recherche disponibles aux colonnes d'entrée.  
  
 Utilisez les cases à cocher pour sélectionner les colonnes de recherche de la table de référence sur lesquelles effectuer les opérations de recherche.  
  
 Vous pouvez également mapper les colonnes de recherche aux colonnes d'entrée à l'aide du clavier. Pour ce faire, mettez en surbrillance une colonne dans la table **Colonnes de recherche disponibles** , appuyez sur la touche de l'application, puis cliquez sur **Modifier les mappages**.  
  
 **colonne de recherche**  
 Affichez les colonnes de recherche sélectionnées. Les sélections se reflètent dans les sélections des cases à cocher de la table **Colonnes de recherche disponibles** .  
  
 **Opération de recherche**  
 Dans la liste, sélectionnez une opération de recherche à exécuter sur la colonne de recherche.  
  
 **Alias de sortie**  
 Permet de saisir un alias pour la sortie de chaque colonne de recherche. La valeur par défaut est le nom de la colonne de recherche. Toutefois, vous pouvez sélectionner n'importe quel nom descriptif unique.  
  
## <a name="lookup-transformation-editor-advanced-page"></a>Éditeur de transformation de recherche (page Avancé)
  La page **Avancé** de la boîte de dialogue **Éditeur de transformation de recherche** permet de configurer la mise en cache partielle et de modifier l’instruction SQL pour la transformation de recherche.  
  
### <a name="options"></a>Options  
 **Taille du cache (32 bits)**  
 Ajustez la taille du cache (en mégaoctets) pour les ordinateurs 32 bits. La valeur par défaut est 5 mégaoctets.  
  
 **Taille du cache (64 bits)**  
 Ajustez la taille du cache (en mégaoctets) pour les ordinateurs 64 bits. La valeur par défaut est 5 mégaoctets.  
  
 **Activer le cache pour les lignes sans entrées correspondantes**  
 Mettez en cache les lignes sans entrées correspondantes dans le dataset de référence.  
  
 **Allocation à partir du cache**  
 Spécifiez le pourcentage de cache à allouer aux lignes sans entrées correspondantes dans le dataset de référence.  
  
 **Modifier l'instruction SQL**  
 Modifiez l'instruction SQL utilisée pour générer le dataset de référence.  
  
> [!NOTE]  
>  L’instruction SQL facultative que vous spécifiez dans cette page substitue et remplace le nom de table que vous avez spécifié dans la page **Connexion** de **l’Éditeur de transformation de recherche**. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Connexion&#41;](../../../integration-services/data-flow/transformations/lookup-transformation-editor-connection-page.md).  
  
 **Définition des paramètres**  
 Mappez les colonnes d’entrée aux paramètres en utilisant la boîte de dialogue **Définition des paramètres de la requête** .  
  
### <a name="external-resources"></a>Ressources externes  
 Entrée de blog, [Lookup cache modes](http://go.microsoft.com/fwlink/?LinkId=219518) sur blogs.msdn.com  
  
## <a name="see-also"></a> Voir aussi  
 [Transformation de recherche floue](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md)   
 [Transformation de recherche de terme](../../../integration-services/data-flow/transformations/term-lookup-transformation.md)   
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
