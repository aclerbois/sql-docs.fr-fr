---
title: Référence technique de Microsoft Time Series algorithme | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5d4634a8d926ec62d05317976f4e1bf1a459f0cd
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-time-series-algorithm-technical-reference"></a>Références techniques relatives à l'algorithme MTS (Microsoft Time Series)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'algorithme MTS (Microsoft Time Series) [!INCLUDE[msCoName](../../includes/msconame-md.md)] inclut deux algorithmes séparés pour l'analyse de la série chronologique :  
  
-   L'algorithme ARTXP, qui a été introduit dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], est optimisé pour la prédiction de la valeur probable suivante d'une série.  
  
-   L’algorithme ARIMA a été ajouté dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] pour améliorer la précision de la prédiction à long terme.  
  
 Par défaut, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise chaque algorithme séparément pour l'apprentissage du modèle, puis fusionne les résultats pour produire la meilleure prédiction pour un nombre variable de prédictions. Vous pouvez également choisir d'utiliser un seul des algorithmes, en fonction de vos données et de vos spécifications de prédiction. Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)], vous pouvez aussi personnaliser le point d’arrêt qui contrôle la fusion d’algorithmes pendant la prédiction.  
  
 Cette rubrique fournit des informations supplémentaires sur la façon dont chaque algorithme est implémenté, ainsi que sur la façon dont vous pouvez personnaliser l'algorithme en définissant des paramètres pour optimiser l'analyse et les résultats de prédiction.  
  
## <a name="implementation-of-the-microsoft-time-series-algorithm"></a>Implémentation de l'algorithme MTS (Microsoft Time Series)  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)]Research a développé l’algorithme ARTXP d’origine qui a été utilisé dans SQL Server 2005, en basant l’implementation sur le [!INCLUDE[msCoName](../../includes/msconame-md.md)] algorithme des arbres de décision. Par conséquent, l'algorithme ARTXP peut être décrit comme un modèle d'arborescence autoregressif pour la représentation de données de série chronologique périodiques. Cet algorithme associe un nombre variable d'éléments passés à chaque élément actuel prédit. Le nom ARTXP vient du fait que l'algorithme ART (ou la méthode d'arbre autorégressif) est appliqué à plusieurs états antérieurs inconnus. Pour obtenir des explications détaillées sur l’algorithme ARTXP, consultez [Autoregressive Tree Models for Time-Series Analysis](http://go.microsoft.com/fwlink/?LinkId=45966)(Modèles d’arbres autorégressifs pour l’analyse de série chronologique).  
  
 L'algorithme ARIMA a été ajouté dans l'algorithme MTS (Microsoft Time Series) dans SQL Server 2008 pour améliorer la prédiction à long terme. C'est une implémentation du processus de calcul de moyennes mobiles intégrées autoregressives décrit par Box et Jenkins. La méthode ARIMA permet de déterminer les dépendances dans les observations effectuées de manière séquentielle dans le temps et peut inclure des chocs aléatoires dans le modèle. Elle prend également en charge le caractère saisonnier multiplicatif. Si vous souhaitez en savoir plus sur l'algorithme ARIMA, nous vous conseillons de lire les travaux de Box et Jenkins ; cette rubrique vise à vous fournir plus de détails sur la façon dont la méthodologie ARIMA a été implémentée dans l'algorithme MTS.  
  
 Par défaut, l'algorithme MTS (Microsoft Time Series) utilise les deux méthodes, ARTXP et ARIMA, et fusionne les résultats pour améliorer la précision de la prédiction. Si vous souhaitez utiliser une seule méthode spécifique, vous pouvez définir les paramètres d'algorithme afin d'utiliser uniquement ARTXP ou ARIMA, ou contrôler la façon dont les résultats des algorithmes sont combinés. Notez que l'algorithme ARTXP prend en charge la prédiction croisée, ce que ne fait pas l'algorithme ARIMA. Par conséquent, la prédiction croisée est disponible uniquement lorsque vous utilisez une fusion d'algorithmes, ou lorsque vous configurez le modèle de façon à utiliser uniquement ARTXP.  
  
## <a name="understanding-arima-difference-order"></a>Présentation de l'ordre des différences ARIMA  
 Cette section définit la terminologie à connaître pour comprendre le modèle ARIMA et aborde l'implémentation spécifique de la *différenciation* dans l'algorithme MTS. Pour une explication complète de ces termes et concepts, nous vous recommandons de lire les travaux de Box et Jenkins.  
  
-   Un terme est un composant d’une équation mathématique. Par exemple, un terme dans une équation polynomiale peut inclure une combinaison de variables et constantes.  
  
-   La formule ARIMA incluse dans l'algorithme MTS utilise à la fois les termes *autorégressif* et *moyenne mobile* .  
  
-   Les modèles de séries chronologiques peuvent être *stationnaires* ou *non stationnaires*. Les*modèles stationnaires* sont ceux qui régressent vers la moyenne, bien qu'ils puissent avoir des cycles, tandis que les modèles *non stationnaires* ne ciblent aucun équilibre et peuvent varier davantage ou changer selon des *chocs*, ou des variables externes.  
  
-   L'objectif de la *différenciation* est de stabiliser les séries chronologiques et de les rendre stationnaires.  
  
-   L'  *ordre de différence* représente le nombre de fois que la différence entre des valeurs est constatée pour une série chronologique.  
  
 L'algorithme MTS (Microsoft Time Series) fonctionne en enregistrant des valeurs dans une série de données et en tentant d'adapter les données à un modèle. Si la série de données n'est pas encore stationnaire, l'algorithme applique un ordre de différence. Chaque augmentation dans l'ordre de différence tend à rendre la série chronologique plus stationnaire.  
  
 Par exemple, si vous avez la série chronologique (z1, z2, …, zn) et que vous effectuez des calculs en utilisant un ordre de différence, vous obtenez une nouvelle série (y1, y2,…., yn-1), où `yi = zi+1-zi`. Quand l’ordre de différence est 2, l’algorithme génère une autre série \`(x1, x2, …, xn-2)`, en fonction de la série y qui était dérivée de l’équation du premier ordre. L'ampleur correcte pour la différenciation dépend des données. Un ordre de différenciation unique est plus commun dans les modèles qui indiquent une tendance constante, le second ordre de différenciation peut indiquer une tendance qui varie dans le temps.  
  
 Par défaut, l'ordre de différence utilisé dans l'algorithme MTS est -1, ce qui signifie que l'algorithme détectera automatiquement la meilleure valeur pour l'ordre de différence. Généralement, la meilleure valeur est 1 (lorsqu'une différenciation est requise), mais dans certains cas, l'algorithme va augmenter cette valeur à 2 au maximum.  
  
 L'algorithme MTS détermine l'ordre de différence ARIMA optimal en utilisant les valeurs autorégressives. Il examine les valeurs AR et définit un paramètre masqué, ARIMA_AR_ORDER, représentant l'ordre des termes AR. Ce paramètre masqué, ARIMA_AR_ORDER, a une plage de valeurs variant de -1 à 8. Selon la valeur par défaut de -1, l'algorithme va sélectionner automatiquement l'ordre de différence approprié.  
  
 Lorsque la valeur de ARIMA_AR_ORDER est supérieure à 1, l'algorithme multiplie la série chronologique par un terme polynomial. Si un terme de la formule polynomiale est résolu en une racine de 1, ou proche de 1, l'algorithme tente de préserver la stabilité du modèle en supprimant le terme et en augmentant l'ordre de différence par 1. Si l'ordre de différence est déjà au maximum, le terme est supprimé et l'ordre de différence ne change pas.  
  
 Par exemple, en supposant que la valeur de AR = 2, le terme polynomial AR résultant devrait ressembler à ceci : `1 – 1.4B + .45B^2 = (1- .9B) (1- 0.5B)`. Notez le terme `(1- .9B)` qui a une racine d'environ 0,9. L'algorithme supprime ce terme de la formule polynomiale, mais ne peut pas augmenter l'ordre de différence d'un car il a déjà atteint la valeur maximale de 2.  
  
 Il est important de noter que le seul moyen de **forcer** un changement dans l’ordre de différence est d’utiliser le paramètre non pris en charge ARIMA_DIFFERENCE_ORDER. Ce paramètre caché contrôle le nombre de fois que l'algorithme exécute une différenciation sur la série chronologique et peut être défini en entrant un paramètre d'algorithme personnalisé. Toutefois, nous ne vous recommandons pas de modifier cette valeur, sauf à titre expérimental et si vous connaissez bien les calculs impliqués. Notez également qu'il n'y a actuellement aucun mécanisme ni paramètre caché, qui vous permet de contrôler le seuil de déclenchement de l'augmentation de l'ordre de différence.  
  
 Enfin, notez que la formule décrite ci-dessus est le cas le plus simple, sans aucune indicateur de saisonnalité. Si des indicateurs de saisonnalité sont fournis, un terme polynomial AR distinct est ajouté à gauche de l'équation pour chaque indicateur de saisonnalité et la même stratégie est appliquée pour éliminer les termes qui pourraient déstabiliser les séries différenciées.  
  
## <a name="customizing-the-microsoft-time-series-algorithm"></a>Personnalisation de l'algorithme MTS (Microsoft Time Series)  
 L'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) prend en charge les paramètres suivants qui affectent le comportement, les performances et la précision du modèle d'exploration de données obtenu.  
  
> [!NOTE]  
>  Il est disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; toutefois, certaines fonctionnalités avancées, dont les paramètres permettant de personnaliser l'analyse de la série chronologique, sont prises en charge uniquement dans les éditions spécifiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).  
  
### <a name="detection-of-seasonality"></a>Détection de saisonnalité  
 Les algorithmes ARIMA et ARTXP prennent tous les deux en charge la détection de la saisonnalité ou de la périodicité. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise la transformation Fast Fourier pour détecter la saisonnalité avant l'apprentissage. Toutefois, vous pouvez affecter la détection de saisonnalité, et les résultats d'analyse en série chronologique, en définissant des paramètres d'algorithme.  
  
-   En modifiant la valeur de *AUTODETECT_SEASONALITY*, vous pouvez influencer le nombre de segments temporels qui sont générés.  
  
-   En définissant une ou plusieurs valeurs pour *PERIODICITY_HINT*, vous pouvez fournir à l’algorithme des informations sur les cycles attendus dans les données et augmenter potentiellement la précision de la détection.  
  
> [!NOTE]  
>  Les algorithmes ARTXP et ARIMA sont tous les deux très sensibles aux indications de saisonnalité. Par conséquent, le fait de fournir une indication fausse peut nuire aux résultats.  
  
### <a name="choosing-an-algorithm-and-specifying-the-blend-of-algorithms"></a>Choix d'un algorithme et spécification de la fusion d'algorithmes  
 Par défaut, ou lorsque vous sélectionnez l'option MIXED, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] combine les algorithmes et leur affecte un poids égal. Cependant, dans Enterprise Edition, vous pouvez spécifier un algorithme particulier ou personnaliser la proportion de chaque algorithme dans les résultats en définissant un paramètre qui pondère les résultats par rapport à la prédiction à court ou long terme. Par défaut, le paramètre *FORECAST_METHOD* a la valeur MIXED, et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les deux algorithmes, puis pondère leurs valeurs pour optimiser les points forts de chacun.  
  
-   Pour contrôler le choix de l’algorithme, vous devez définir le paramètre *FORECAST_METHOD* .  
  
-   Si vous voulez utiliser la prédiction croisée, vous devez utiliser soit ARTXP, soit l'option MIXED, car ARIMA ne prend pas en charge cette fonctionnalité.  
  
-   Attribuez à *FORECAST_METHOD* la valeur ARTXP si vous voulez privilégier la prédiction à court terme.  
  
-   Attribuez à *FORECAST_METHOD* la valeur ARIMA si vous voulez améliorer la prédiction à long terme.  
  
 Dans Enterprise Edition, vous pouvez aussi personnaliser la façon dont [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] associe les algorithmes ARIMA et ARTXP. Vous pouvez contrôler aussi bien le point de départ de l’association que le taux de changement en définissant le paramètre *PREDICTION_SMOOTHING* :  
  
-   Si vous attribuez à *PREDICTION_SMOOTHING* la valeur 0, le modèle utilise uniquement ARTXP.  
  
-   Si vous attribuez à *PREDICTION_SMOOTHING* la valeur 1, le modèle utilise uniquement ARIMA.  
  
-   Si vous attribuez à *PREDICTION_SMOOTHING* une valeur comprise entre 0 et 1, le modèle pondère l’algorithme ARTXP comme une fonction exponentiellement décroissante des étapes de prédiction. Parallèlement, le modèle pondère également l'algorithme ARIMA comme complément à 1 du poids d'ARTXP. Le modèle utilise la normalisation et une constante de stabilisation pour lisser les courbes.  
  
 En général, si vous prédisez jusqu'à 5 tranches de temps, ARTXP est presque toujours le choix le plus adapté. Toutefois, lorsque vous augmentez le nombre de tranches de temps à prédire, ARIMA est généralement plus performant.  
  
 Le diagramme suivant illustre la façon dont le modèle fusionne les algorithmes quand *PREDICTION_SMOOTHING* a la valeur par défaut, 0.5. ARIMA et ARTXP ont la même pondération au début, mais au fil de l'augmentation du nombre d'étapes de prédiction, ARIMA est pondéré de manière plus importante.  
  
 ![courbe par défaut pour la combinaison d’algorithmes de série chronologique](../../analysis-services/data-mining/media/time-series-mixing-default.gif "courbe par défaut pour la combinaison d’algorithmes de série chronologique")  
  
 À l’inverse, le diagramme suivant illustre la fusion des algorithmes quand *PREDICTION_SMOOTHING* a la valeur 0.2. Pour l'étape [!INCLUDE[tabValue](../../includes/tabvalue-md.md)], le modèle pondère ARIMA avec la valeur 0.2 et ARTXP avec la valeur 0.8. La pondération d'ARIMA augmente ensuite de façon exponentielle alors que la pondération d'ARTXP diminue de la même façon.  
  
 ![courbe de délai pour combinaison de modèles de série heure](../../analysis-services/data-mining/media/time-series-blending-curve.gif "courbe de délai pour combinaison de modèles de série heure")  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Le tableau suivant décrit les paramètres qui peuvent être utilisés avec l'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series).  
  
|Paramètre|Description|  
|---------------|-----------------|  
|*AUTO_DETECT_PERIODICITY*|Spécifie une valeur numérique comprise entre [!INCLUDE[tabValue](../../includes/tabvalue-md.md)] et 1 qui détecte la périodicité. La valeur par défaut est 0,6.<br /><br /> Si la valeur est proche de [!INCLUDE[tabValue](../../includes/tabvalue-md.md)], la périodicité n'est détectée que pour les données fortement périodiques.<br /><br /> Une valeur proche de 1 favorise la découverte de nombreux modèles qui sont presque périodiques et la génération automatique d'indications de périodicité.<br /><br /> Remarque : le traitement d’un grand nombre d’indications de périodicité est susceptible d’allonger de façon significative les durées d’apprentissage des modèles, mais peut produire des modèles plus précis.|  
|*COMPLEXITY_PENALTY*|Contrôle la croissance de l'arbre de décision. La valeur par défaut est 0,1.<br /><br /> La diminution de cette valeur augmente la probabilité d'une division. L'augmentation de cette valeur diminue la probabilité d'une division.<br /><br /> Remarque : ce paramètre est uniquement disponible dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*FORECAST_METHOD*|Spécifie l'algorithme à utiliser pour l'analyse et la prédiction. Les valeurs possibles sont ARTXP, ARIMA ou MIXED. La valeur par défaut MIXED.|  
|*HISTORIC_MODEL_COUNT*|Spécifie le nombre de modèles historiques qui seront construits. La valeur par défaut est 1.<br /><br /> Remarque : ce paramètre est uniquement disponible dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*HISTORICAL_MODEL_GAP*|Spécifie le décalage dans le temps entre deux modèles historiques successifs. La valeur par défaut est 10. La valeur représente plusieurs unités de temps, où l'unité est définie par le modèle.<br /><br /> Par exemple, la valeur g produit des modèles historiques générés pour des données tronquées par tranches de temps à des intervalles de g, 2\*g, 3\*g et ainsi de suite.<br /><br /> Remarque : ce paramètre est uniquement disponible dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*INSTABILITY_SENSITIVITY*|Contrôle le point à partir duquel la variance de prédiction dépasse un certain seuil, après quoi l'algorithme ARTXP supprime des prédictions. La valeur par défaut est 1.<br /><br /> Remarque : ce paramètre ne s’applique pas aux modèles qui utilisent uniquement ARIMA.<br /><br /> La valeur par défaut de 1 présente le même comportement que dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] surveille l'écart type normalisé pour chaque prédiction. Dès que cette valeur dépasse le seuil d'une prédiction, l'algorithme de série chronologique retourne une valeur Null et arrête le processus de prédiction.<br /><br /> La valeur [!INCLUDE[tabValue](../../includes/tabvalue-md.md)] arrête la détection d'instabilité. Cela signifie que vous pouvez créer un nombre infini de prédictions, quelle que soit la variance.<br /><br /> Remarque : ce paramètre est uniquement modifiable dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise uniquement la valeur par défaut 1.|  
|*MAXIMUM_SERIES_VALUE*|Spécifie la valeur maximale à utiliser pour les prédictions. Ce paramètre est utilisé avec *MINIMUM_SERIES_VALUE*pour limiter les prédictions à une certaine plage attendue. Par exemple, vous pouvez spécifier que le volume de ventes prédites pour un jour ne doit jamais dépasser le nombre de produits dans l'inventaire.<br /><br /> Remarque : ce paramètre est uniquement disponible dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*MINIMUM_SERIES_VALUE*|Spécifie la valeur minimale qui peut être prédite. Ce paramètre est utilisé avec *MAXIMUM_SERIES_VALUE*pour limiter les prédictions à une certaine plage attendue. Par exemple, vous pouvez spécifier que la quantité de ventes prédite ne doit jamais être un nombre négatif.<br /><br /> Remarque : ce paramètre est uniquement disponible dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|*MINIMUM_SUPPORT*|Spécifie le nombre minimal de tranches de temps qui sont requises pour générer un fractionnement dans chaque arbre de série chronologique. La valeur par défaut est 10.|  
|*MISSING_VALUE_SUBSTITUTION*|Spécifie la façon dont les vides dans les données d'historique sont comblés. Par défaut, les vides dans les données ne sont pas autorisés. Le tableau suivant répertorie les valeurs possibles de ce paramètre :<br /><br /> **Previous**: répète la valeur de la tranche de temps précédente.<br /><br /> **Mean**: utilise une moyenne mobile des tranches de temps utilisées pour l’apprentissage.<br /><br /> Constante numérique : utilise le nombre spécifié pour remplacer toutes les valeurs manquantes.<br /><br /> **None**: valeur par défaut. Remplace les valeurs manquantes par les valeurs représentées le long de la courbe du modèle ayant fait l'objet d'un apprentissage.<br /><br /> <br /><br /> Notez que si vos données contiennent plusieurs séries, les séries ne peuvent pas comporter d’extrémités déséquilibrées. En d'autres termes, toutes les séries doivent avoir les mêmes points de départ et d'arrêt. <br />                    [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise aussi la valeur de ce paramètre pour combler les vides dans les nouvelles données quand vous effectuez une opération **PREDICTION JOIN** sur un modèle de série chronologique.|  
|*PERIODICITY_HINT*|Fournit à l'algorithme une indication de la périodicité des données. Par exemple, si les ventes varient chaque année, et que l'unité de mesure de la série est le mois, la périodicité est égale à 12. Ce paramètre s'affiche sous la forme {n [, n]}, où n est un nombre positif.<br /><br /> Le n entre crochets [] est facultatif et peut être répété autant de fois que nécessaire. Par exemple, pour fournir plusieurs indications de périodicité pour les données fournies mensuellement, vous pouvez entrer {12, 3, 1} pour détecter les modèles pour l'année, le trimestre et le mois. Toutefois, la périodicité a une répercussion importante sur la qualité du modèle. Si l'indication que vous fournissez diffère de la périodicité réelle, vos résultats peuvent être gravement compromis.<br /><br /> La valeur par défaut est {1}.<br /><br /> Notez que les accolades sont obligatoires. Par ailleurs, ce paramètre a un type de données chaîne. Par conséquent, si vous tapez ce paramètre dans une instruction DMX (Data Mining Extensions), vous devez placer le nombre et les accolades entre guillemets.|  
|*PREDICTION_SMOOTHING*|Spécifie la façon dont le modèle doit être combiné pour optimiser la prévision. Vous pouvez taper toute valeur comprise entre [!INCLUDE[tabValue](../../includes/tabvalue-md.md)] et 1, ou utiliser l'une des valeurs suivantes :<br /><br /> [!INCLUDE[tabValue](../../includes/tabvalue-md.md)]:<br />                          Spécifie que la prédiction utilise uniquement ARTXP. La prévision est optimisée pour un petit nombre de prédictions.<br /><br /> 1 : spécifie que la prédiction utilise uniquement ARIMA. La prévision est optimisée pour un grand nombre de prédictions.<br /><br /> 0,5 : valeur par défaut. Spécifie que les deux algorithmes doivent être utilisés et les résultats fusionnés pour la prédiction.<br /><br /> <br /><br /> Quand vous procédez à un lissage des prédictions, utilisez le paramètre *FORECAST_METHOD* pour contrôler l’apprentissage.   Notez que ce paramètre est uniquement disponible dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 L'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) prend en charge les indicateurs de modélisation suivants. Lorsque vous créez la structure d'exploration de données ou le modèle d'exploration de données, vous définissez des indicateurs de modélisation pour spécifier la façon dont les valeurs de chaque colonne sont gérées pendant l'analyse. Pour plus d’informations, consultez [Indicateurs de modélisation &#40;Exploration de données&#41;](../../analysis-services/data-mining/modeling-flags-data-mining.md).  
  
|Indicateur de modélisation|Description|  
|-------------------|-----------------|  
|NOT NULL|Indique que la colonne ne peut pas contenir de valeur Null. Une erreur est générée si Analysis Services rencontre une valeur NULL au cours de l'apprentissage du modèle.<br /><br /> S'applique aux colonnes de structure d'exploration de données.|  
|MODEL_EXISTENCE_ONLY|Signifie que la colonne sera considérée comme ayant deux états possibles : manquant et existant. Une valeur NULL est une valeur manquante.<br /><br /> S'applique aux colonnes de modèle d'exploration de données.|  
  
## <a name="requirements"></a>Spécifications  
 Un modèle de série chronologique doit contenir une colonne Key Time qui contient des valeurs uniques, des colonnes d'entrée et au moins une colonne prédictible.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) prend en charge les types de contenu de colonne d'entrée, types de contenu de colonne prédictible et indicateurs de modélisation spécifiques qui sont répertoriés dans le tableau suivant.  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Continu, Clé, Key Time et Table|  
|Attribut prédictible|Continu, Table|  
  
> [!NOTE]  
>  Les types de contenu Cyclique et Trié sont pris en charge, mais l'algorithme les traite comme des valeurs discrètes et n'effectue pas de traitement spécial.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme de série chronologique de Microsoft](../../analysis-services/data-mining/microsoft-time-series-algorithm.md)   
 [Exemples de requête de modèle de série de temps](../../analysis-services/data-mining/time-series-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles de série chronologique & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md)  
  
  
