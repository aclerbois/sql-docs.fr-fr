---
title: Déployer une Solution d’exploration de données pour les Versions précédentes de SQL Server | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: df527197f0ddd1eacc2e86e59092f45b1ac78c9a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-a-data-mining-solution-to-previous-versions-of-sql-server"></a>Déployer une solution d'exploration de données sur des versions antérieures de SQL Server
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Cette section décrit des problèmes de compatibilité connus qui peuvent survenir lorsque vous essayez de déployer un modèle d'exploration de données ou une structure d'exploration de données créée dans une instance de [!INCLUDE[ssASCurrent](../../includes/ssascurrent-md.md)] sur une base de données qui utilise SQL Server 2005 Analysis Services, ou lorsque vous déployez des modèles créés dans SQL Server 2005 sur une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Le déploiement sur une instance de SQL Server 2000 Analysis Services n'est pas pris en charge.  
  
 [Déploiement de modèles de séries chronologiques](#bkmk_TimeSeries)  
  
 [Déploiement de modèles avec données d'exclusion](#bkmk_Holdout)  
  
 [Déploiement de modèles avec filtres](#bkmk_Filter)  
  
 [Restauration à partir de sauvegardes de base de données](#bkmk_Backup)  
  
 [Utilisation de la synchronisation de base de données](#bkmk_Synch)  
  
##  <a name="bkmk_TimeSeries"></a> Déploiement de modèles de séries chronologiques  
 L'algorithme MTS (Microsoft Time Series) a été amélioré dans SQL Server 2008 grâce à l'ajout d'un second algorithme complémentaire, ARIMA. Pour plus d’informations sur les modifications apportées à l’algorithme MTS, consultez [Algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/microsoft-time-series-algorithm.md).  
  
 Par conséquent, les modèles d'exploration de données de série chronologique qui utilisent le nouvel algorithme ARIMA peuvent se comporter différemment lorsqu'ils sont déployés sur une instance de SQL Server 2005 Analysis Services.  
  
 Si vous avez défini explicitement le paramètre PREDICTION_SMOOTHING pour contrôler l'association des modèles ARTXP et ARIMA dans la prédiction, lorsque vous déployez ce modèle sur une instance de SQL Server 2005, Analysis Services déclenche une erreur indiquant que le paramètre n'est pas valide. Pour empêcher cette erreur, vous devez supprimer le paramètre PREDICTION_SMOOTHING et convertir les modèles dans un modèle purement ARTXP.  
  
 Inversement, si vous déployez un modèle de série chronologique créé à l'aide de SQL Server 2005 Analysis Services sur une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], lorsque vous ouvrez le modèle d'exploration de données dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], les fichiers de définition sont convertis d'abord au nouveau format, et deux nouveaux paramètres sont ajoutés par défaut à tous les modèles de série chronologique. Le paramètre FORECAST_METHOD est ajouté avec la valeur par défaut de MIXED, et le paramètre PREDICTION_SMOOTHING est ajouté avec la valeur par défaut de 0.5. Toutefois, le modèle continue à utiliser uniquement ARTXP pour la prévision jusqu'à son retraitement. Dès que vous retraitez le modèle, la prédiction change pour utiliser à la fois ARIMA et ARTXP.  
  
 Par conséquent, si vous souhaitez éviter de modifier le modèle, vous devez parcourir uniquement le modèle sans jamais le traiter. Vous pouvez également définir explicitement les paramètres FORECAST_METHOD ou PREDICTION_SMOOTHING.  
  
 Pour plus d’informations sur la configuration des modèles mixtes, consultez [Références techniques relatives à l’algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/microsoft-time-series-algorithm-technical-reference.md).  
  
 Si le fournisseur utilisé pour la source de données du modèle est SQL Server ou le fournisseur de données SQL Client 10, vous devez aussi modifier la définition de la source de données pour spécifier la version antérieure de SQL Server Native Client. Sinon, [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] génère une erreur indiquant que le fournisseur n'est pas enregistré.  
  
##  <a name="bkmk_Holdout"></a> Déploiement de modèles avec données d'exclusion  
 Si vous créez une structure d’exploration de données qui contient une partition d’exclusion utilisée pour tester les modèles d’exploration de données, la structure d’exploration de données peut être déployée à une instance de SQL Server 2005, mais les informations de partition seront perdues.  
  
 Lorsque vous ouvrez la structure d'exploration de données dans SQL Server 2005 Analysis Services, [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] génère une erreur, puis régénère la structure pour supprimer la partition d'exclusion.  
  
 Une fois que la structure a été reconstruite, la taille de la partition d’exclusion n’est plus disponible dans la fenêtre Propriétés. Toutefois, la valeur \<ddl100_100 : holdoutmaxpercent > 30\</ddl100_100:HoldoutMaxPercent >) peuvent toujours être présents dans le fichier de script ASSL.  
  
##  <a name="bkmk_Filter"></a> Déploiement de modèles avec filtres  
 Si vous appliquez un filtre à un modèle d’exploration de données, le modèle peut être déployé à une instance de SQL Server 2005, mais le filtre n’est pas appliqué.  
  
 Lorsque vous ouvrez le modèle d'exploration de données, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] génère une erreur, puis régénère le modèle pour supprimer le filtre.  
  
##  <a name="bkmk_Backup"></a> Restauration à partir de sauvegardes de base de données  
 Vous ne pouvez pas restaurer une sauvegarde de base de données créée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] vers une instance de SQL Server 2005. Si vous le faites, SQL Server Management Studio génère une erreur.  
  
 Si vous créez une sauvegarde d'une base de données SQL Server 2005 Analysis Services et restaurez cette sauvegarde sur une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], tous les modèles de séries chronologiques sont modifiés comme indiqué dans la section précédente.  
  
##  <a name="bkmk_Synch"></a> Utilisation de la synchronisation de base de données  
 La synchronisation de base de données n'est pas prise en charge de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] vers SQL Server 2005.  
  
 Si vous essayez de synchroniser une base de données [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , le serveur retourne une erreur et la synchronisation de la base de données échoue.  
  
## <a name="see-also"></a>Voir aussi  
 [Compatibilité descendante Analysis Services](../../analysis-services/analysis-services-backward-compatibility.md)  
  
  
