---
title: Effacer les Caches Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4ed87e37ca9b13af696977a86eeccd34eec81e87
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="clear-the-analysis-services-caches"></a>Effacer les caches Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Analysis Services met en cache des données pour optimiser les performances des requêtes. Cette rubrique fournit des recommandations pour l'utilisation de la commande XMLA ClearCache en vue d'effacer des caches qui ont été créés en réponse à une requête MDX. Les effets de l'exécution de la commande ClearCache varient selon que vous utilisez un modèle tabulaire ou multidimensionnel.  
  
 **À quel moment effacer le cache pour des modèles MDX**  
  
 Pour les bases de données multidimensionnelles, Analysis Services génère des caches dans le moteur de formule en évaluant un calcul, et dans le moteur de stockage pour les résultats des requêtes de dimension et des requêtes de groupe de mesures. Les requêtes de groupe de mesures se produisent lorsque le moteur de formule a besoin de données de mesure pour une coordonnée de cellule ou un sous-cube. Les requêtes de dimension se produisent en interrogeant des hiérarchies non naturelles et en appliquant des fonctionnalités autoexist.  
  
 L'effacement du cache est recommandé lors du test des performances. En effaçant le cache entre les tests, vous garantissez que la mise en cache ne biaise aucun résultat de test qui mesure l'impact d'une modification de conception de requête.  
  
 **À quel moment effacer le cache pour des modèles tabulaires**  
  
 Les modèles tabulaires sont généralement stockés en mémoire, où les agrégations et d'autres calculs sont effectués au moment de l'exécution d'une requête. Ainsi, la commande ClearCache a un effet limité sur des modèles tabulaires. Pour un modèle tabulaire, des données peuvent être ajoutées dans les caches Analysis Services, si des requêtes MDX sont exécutées sur celui-ci. En particulier, les mesures DAX référencées par des opérations MDX et autoexist peuvent mettre en cache des résultats dans le cache de formule et le cache de dimension, respectivement. Notez toutefois, que ces hiérarchies non naturelles et les requêtes de groupe de mesures ne mettent pas en cache des résultats dans le moteur de stockage. De plus, il est important de noter que les requêtes DAX ne mettent pas en cache des résultats dans le moteur de formule et le moteur de stockage. Dans la mesure où les caches existent en raison de requêtes MDX, l'exécution de la commande ClearCache sur un modèle tabulaire invalidera des données en mémoire cache du système.  
  
 L'exécution de la commande ClearCache efface également les caches en mémoire dans le moteur d'analyse en mémoire xVelocity (VertiPaq). Le moteur xVelocity gère un petit ensemble de résultats mis en cache. L'exécution de la commande ClearCache invalidera ces caches dans le moteur xVelocity.  
  
 Enfin, l'exécution de la commande ClearCache supprimera également les données résiduelles conservées dans la mémoire lorsqu'un modèle tabulaire est reconfiguré pour le mode **DirectQuery** . Cela est particulièrement important si le modèle contient des données sensibles qui sont soumises à des contrôles étroits. Dans ce cas, l'exécution de la commande ClearCache est une mesure de précaution que vous pouvez prendre pour garantir que des données sensibles existent uniquement à l'emplacement prévu. L'effacement manuel du cache est nécessaire si vous utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour déployer le modèle et pour modifier le mode de requête. À l’inverse, l’utilisation des [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour spécifier **DirectQuery** sur le modèle et les partitions permet d’effacer automatiquement le cache quand vous basculez le modèle pour utiliser ce mode de requête.  
  
 Comparées aux recommandations pour effacer les caches de modèles multidimensionnels lors du test des performances, il n'y a aucune recommandation étendue pour effacer les caches de modèles tabulaires. Si vous ne gérer pas le déploiement d'un modèle tabulaire qui contient des données sensibles, il n'y a aucune tâche administrative spécifique qui demande d'effacer le cache.  
  
## <a name="clear-the-cache-for-analysis-services-models"></a>Effacer le cache pour des modèles Analysis Services  
 Pour effacer le cache, utilisez XMLA et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez effacer le cache au niveau de la base de données, du cube, de la dimension, de la table ou du groupe de mesures. Les étapes suivantes pour effacer le cache au niveau de la base de données appliquent aux modèles MDX et aux modèles tabulaires.  
  
> [!NOTE]  
>  Un test rigoureux des performances peut nécessiter une approche plus complète pour effacer le cache. Pour savoir comment vider les caches Analysis Services et du système de fichiers, consultez la section sur l'effacement des caches dans le [Guide des opérations SQL Server 2008 R2 Analysis Services](http://go.microsoft.com/fwlink/?linkID=http://go.microsoft.com/fwlink/?LinkID=225539).  
  
 Pour les modèles MDX et tabulaires, l'effacement de certains de ces caches peut être un processus en deux étapes qui consiste à invalider le cache lorsque la commande ClearCache s'exécute, puis à vider le cache lors de la réception de la requête suivante. Une réduction de la consommation de mémoire n'est évidente qu'après que le cache a été réellement vidé.  
  
 L'effacement du cache requiert la fourniture d'un identificateur d'objet à l'instruction **ClearCache** dans une requête XMLA. La première étape de cette rubrique explique comment obtenir un identificateur d'objet.  
  
#### <a name="step-1-get-the-object-identifier"></a>Étape 1 : obtenir l'identificateur d'objet  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur un objet, sélectionnez **Propriétés**et copiez la valeur de la propriété ID dans le volet **Propriétés** . Cette approche fonctionne pour la base de données, le cube, la dimension ou la table.  
  
2.  Pour obtenir l’ID d’un groupe de mesures, cliquez avec le bouton droit sur le groupe de mesures et sélectionnez **Générer un script du groupe de mesures en tant que**. Choisissez **Create** ou **Alter**, et envoyez la requête dans une fenêtre. L'ID du groupe de mesures sera visible dans la définition de l'objet. Copiez l'ID de la définition d'objet.  
  
#### <a name="step-2-run-the-query"></a>Étape 2 : exécuter la requête  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur une base de données, pointez sur **Nouvelle requête**, puis sélectionnez **XMLA**.  
  
2.  Copiez l'exemple de code suivant dans une fenêtre de requête XMLA. Remplacez **DatabaseID** par l'ID de la base de données sur la connexion actuelle.  
  
    ```  
    <ClearCache xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
      <Object>  
        <DatabaseID> Adventure Works DW Multidimensional</DatabaseID>  
      </Object>  
    </ClearCache>  
  
    ```  
  
     Ou bien, vous pouvez spécifier un chemin d'accès d'un objet enfant, tel qu'un groupe de mesures, pour effacer le cache uniquement pour cet objet.  
  
    ```  
    <ClearCache xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
      <Object>  
        <DatabaseID>Adventure Works DW Multidimensional</DatabaseID>  
            <CubeID>Adventure Works</CubeID>  
            <MeasureGroupID>Fact Currency Rate</MeasureGroupID>  
      </Object>  
    </ClearCache>  
    ```  
  
3.  Appuyez sur F5 pour exécuter la requête. Vous devez voir le résultat suivant :  
  
    ```  
    <return xmlns="urn:schemas-microsoft-com:xml-analysis">  
      <root xmlns="urn:schemas-microsoft-com:xml-analysis:empty" />  
    </return>  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller une Instance Analysis Services](../../analysis-services/instances/monitor-an-analysis-services-instance.md)  
  
  
