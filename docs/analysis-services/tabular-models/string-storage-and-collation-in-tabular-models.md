---
title: Stockage de chaîne et classement dans les modèles tabulaires | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 38a79073648bdab889913050118d7318ca3f536b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="string-storage-and-collation-in-tabular-models"></a>Stockage de chaînes et de classement dans les modèles tabulaires
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les chaînes (valeurs texte) sont stockées dans un format fortement compressé dans les modèles tabulaires ; en raison de cette compression, vous pouvez obtenir des résultats inattendus lorsque vous récupérez des chaînes entières ou partielles. En outre, comme les paramètres régionaux et les classements de chaîne sont hérités hiérarchiquement de l'objet parent le plus proche, si le langage de chaîne n'est pas défini explicitement, les paramètres régionaux et le classement du parent peuvent affecter la façon dont chaque chaîne est stockée et si la chaîne doit être unique ou combinée avec des chaînes semblables tel que le défini le classement parent.  
  
 Cet article décrit le mécanisme par lequel les chaînes sont compressées et stockées et fournit des exemples d’utilisation du classement et du langage affectent les résultats des formules de texte dans les modèles tabulaires.  
  
## <a name="storage"></a>Stockage  
 Dans les modèles tabulaires toutes les données sont fortement compressées de façon à ce qu'elles tiennent en mémoire. Par conséquent, toutes les chaînes qui peuvent être considérées comme des chaînes lexicales équivalentes sont stockées une seule fois. La première instance de la chaîne est utilisée comme la représentation canonique et par la suite chaque chaîne équivalente est indexée à la même valeur compressée que la première occurrence.  
  
 La question clé est la suivante : qu'est-ce qui constitue une chaîne lexicale équivalente ? Deux chaînes sont considérées comme des chaînes lexicales équivalentes si elles peuvent être considérées comme même mot. Par exemple, lorsque vous recherchez le mot **violon** dans un dictionnaire, vous pouvez trouver l'entrée **Violon** ou **violon**, en fonction de la stratégie éditoriale du dictionnaire, mais en général vous considérez les deux mots comme équivalents, et vous ignorez la mise en majuscules. Dans un modèle tabulaire, le facteur qui détermine si deux chaînes sont des chaînes lexicales équivalentes n'est pas une stratégie éditoriale ou une préférence de l'utilisateur, mais les paramètres régionaux et l'ordre de classement affectés à la colonne.  
  
 Par conséquent, le choix en faveur de la gestion des majuscules et des minuscules de la même façon ou non dépend du classement et des paramètres régionaux. Pour tout mot particulier dans ces paramètres régionaux, la première occurrence du mot trouvée dans une colonne particulière sert par conséquent de représentation canonique de ce mot et cette chaîne est stockée au format non compressé.  Toutes les autres chaînes sont testées sur la première occurrence, et si elles correspondent au test d'équivalence, elles sont affectées à la valeur compressée de la première occurrence. Ultérieurement, lorsque les valeurs compressées sont récupérées, elles sont représentées à l'aide de la valeur non compressée de la première occurrence de la chaîne.  
  
 Un exemple permettra de mieux comprendre le fonctionnement. La colonne suivante « Classification - anglais » a été extraite d'une table qui contient des informations sur les plantes et les arbres. Pour chaque plante (les noms des plantes ne sont pas affichés ici) la colonne de classification affiche la catégorie générale de la plante.  
  
|Classification - anglais|  
|-------------------------------|  
|trEE|  
|PlAnT|  
|trEE|  
|PlAnT|  
|PlAnT|  
|trEE|  
|PlAnT|  
|trEE|  
|trEE|  
|PlAnT|  
|trEE|  
  
 Les données peuvent être issues de nombreuses sources, et par conséquent la casse et l'utilisation des accents sont incohérentes, et la base de données relationnelle a stocké ces différences en l'état. Mais en général les valeurs sont **Plant** ou **Tree**, uniquement avec une casse différente.  
  
 Lorsque ces valeurs sont chargées dans un modèle tabulaire qui utilise le classement et l'ordre de tri par défaut pour l'anglais américain, la casse n'est pas importante, seules deux valeurs sont stockées pour la colonne entière :  
  
|Classification - anglais|  
|-------------------------------|  
|trEE|  
|PlAnT|  
  
 Si vous utilisez la colonne, **Classification - anglais**, dans votre modèle, si vous affichez la classification des plantes vous ne verrez pas les valeurs d'origine, avec leurs différentes utilisations de majuscules et minuscules, mais uniquement la première instance. En effet, toutes les variantes de majuscules et minuscules de **tree** sont considérées comme équivalentes dans ce classement et ces paramètres régionaux ; par conséquent, une seule chaîne a été conservée et la première instance de cette chaîne qui est rencontrée par le système est celle qui est enregistrée.  
  
> [!WARNING]  
>  Vous pouvez décider de définir la première chaîne à stocker, en fonction de ce que vous considérez correct, mais cette opération peut être très difficile à effectuer. Il n'existe aucun moyen simple de déterminer à l'avance quelle ligne doit être traitée en premier par le moteur, étant donné que toutes les valeurs sont considérées comme identiques. Au lieu de cela, si vous devez définir la valeur standard, vous devez nettoyer toutes vos chaînes avant de charger le modèle.  
  
## <a name="locale-and-collation-order"></a>Ordre des paramètres régionaux et de classement  
 Lors de la comparaison de chaînes (valeurs texte), ce qui définit l'équivalence est normalement l'aspect culturel de la façon dont ces chaînes sont interprétées. Dans certaines cultures un accent ou la mise en majuscules d'un caractère peut changer complètement la signification de la chaîne ; par conséquent, généralement ces différences sont prises en compte pour déterminer l'équivalence dans n'importe quelle langue ou région spécifique.  
  
 Généralement, lorsque vous utilisez votre ordinateur elle est déjà configurée pour correspondre à vos propres attentes culturelles et comportement linguistique, et les opérations de chaîne, telles que le tri et la comparaison des valeurs texte se comportent de la façon attendue. Les paramètres qui contrôlent le comportement spécifique à une langue sont définis par les **paramètres régionaux** dans Windows. Les applications lisent ces paramètres et modifient leur comportement en conséquence. Dans certains cas, une application peut avoir une fonctionnalité qui vous permet de modifier le comportement culturel de l'application ou la façon dont les chaînes sont comparées.  
  
 Lorsque vous créez une base de données model tabulaire, par défaut la base de données hérite ces paramètres culturels et linguistiques sous la forme d'un identificateur de langue et d'un classement.  
  
-   L'identificateur de langue définit le jeu de caractères que vous voulez utiliser pour vos chaînes en fonction de la culture.  
  
-   Le classement définit l'ordre des caractères et leur équivalence.  
  
 Il est important de noter qu'un identificateur de langue identifie non seulement une langue mais, également le pays ou la zone où la langue est utilisée. Chaque identificateur de langue a également une spécification de classement par défaut. Pour plus d'informations sur les identificateurs de langue, consultez [ID de paramètres régionaux assignés par Microsoft](http://msdn.microsoft.com/goglobal/bb964664.aspx). Vous pouvez utiliser la colonne LCID Dec pour obtenir l'ID correct en insérant manuellement une valeur. Pour plus d’informations sur le concept SQL des classements, consultez [COLLATE &#40;Transact-SQL&#41;](../../t-sql/statements/collations.md). Pour plus d’informations sur les indicateurs de classement et les styles de comparaison pour les noms de classements Windows, consultez [Nom de classement Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md). La rubrique [Nom du classement SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md) mappe les noms de classements Windows aux noms utilisés pour SQL.  
  
 Une fois que votre base de données model tabulaire a été créée, tous les nouveaux objets du modèle héritent des attributs de langue et de classement des attributs de la base de données. Cela est vrai pour tous les objets. Le chemin d'accès d'héritage commence à l'objet, examine le parent pour trouver tous les attributs de langue et de classement à hériter, et si aucun attribut n'est trouvé, continue vers le haut et recherche les attributs de langue et de classement au niveau de la base de données. En d'autres termes, si vous ne spécifiez pas les attributs de langue et de classement pour un objet, par défaut, l'objet hérite les attributs de son parent plus proche.  
  
 Pour les colonnes, les attributs de langue et de classement sont hérités au moment de la création, selon les règles suivantes :  
  
1.  L'objet dimension parente fait l'objet de recherches pour trouver les attributs de langue et de classement. Si les deux valeurs existent, elles sont copiées dans les attributs de colonne ; si une seule valeur existe, l'autre est déduite de la valeur existante et les deux sont affectées ; si aucune valeur n'existe, passez à l'étape suivante.  
  
2.  L'objet base de données fait l'objet de recherche à l'aide du processus décrit à l'étape 1 pour les dimensions ; si aucun attribut n'est trouvé, passez à l'étape suivante.  
  
3.  L'objet serveur fait l'objet de recherche à l'aide du processus décrit à l'étape 1 pour les dimensions ; si aucun attribut n'est trouvé, la colonne utilise l'identificateur de langue Windows et déduit l'attribut de classement de cette valeur.  
  
 Il est important de noter qu'en général l'identificateur de langue et l'ordre de classement dans la base de données source ont peu d'effet, si ce n'est aucun, sur la façon dont les valeurs sont stockées dans la colonne du modèle tabulaire. L'exception indique si la base de données source transforme ou filtre les valeurs demandées.  
  
  
