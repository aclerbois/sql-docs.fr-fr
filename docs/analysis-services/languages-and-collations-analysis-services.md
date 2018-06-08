---
title: Langues et classements (Analysis Services) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a1b066f23c0c5a4e92b6b1f86886cc54c7451f6c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="languages-and-collations-analysis-services"></a>Langues et classements (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

  [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] prend en charge les langues et les classements fournis par les systèmes d'exploitation [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. Les propriétés**Language** et **Collation** sont définies initialement au niveau de l'instance pendant l'installation, mais vous pouvez les modifier ultérieurement à différents niveaux de la hiérarchie d'objets.  
  
 Dans un modèle multidimensionnel (uniquement), vous pouvez définir ces propriétés sur une base de données ou sur un cube. Vous pouvez également les définir sur les traductions que vous créez pour des objets dans un cube. Dans un modèle tabulaire, la langue et le classement sont hérités du système d’exploitation hôte.  
  
 Lors de la définition de **Language** et **Collation** dans un modèle multidimensionnel, vous spécifiez les paramètres utilisés par le modèle de données pendant le traitement et l’exécution des requêtes ou vous intégrez plusieurs traductions dans un modèle pour que les locuteurs de langue étrangère puissent l’utiliser dans leur langue maternelle. La définition explicite des propriétés **Language** et **Collation** d’un objet (base de données, modèle ou cube) concerne les situations dans lesquelles les serveurs de production et d’environnement de développement sont configurés pour différents paramètres régionaux et vous voulez être sûr que la langue et le classement correspondent à ceux de l’environnement cible.  
  
##  <a name="bkmk_object"></a> Objets qui prennent en charge les propriétés Language et Collation  
 Les propriétés**Language** et **Collation** sont souvent exposées ensemble. Là où vous pouvez définir **Language**, vous pouvez aussi définir **Collation**.  
  
 Vous pouvez définir **Language** et **Collation** sur ces objets :  
  
-   **Instance**. Tous les projets déployés sur l'instance adoptent la langue et le classement de l'instance, en supposant que la langue et le classement ne soient pas définis. Par défaut, un modèle multidimensionnel laisse la langue et le classement vide. Quand le projet est déployé, la base de données et les cubes obtenus reçoivent la langue et le classement de l'instance.  
  
     Initialement, les propriétés de langue et de classement sont établies pendant l'installation, mais un administrateur peut les remplacer dans Management Studio. Pour plus d'informations, consultez [Modifier la langue ou le classement par défaut de l'instance](#bkmk_defaultLang) .  
  
-   **Base de données**. Pour rompre l'héritage, vous pouvez définir explicitement la langue et le classement au niveau du projet utilisé par tous les cubes contenus dans la base de données. Sauf indication contraire, tous les cubes de la base de données reçoivent la langue et le classement que vous spécifiez à ce niveau. Si vous codez et déployez régulièrement avec différents paramètres régionaux (par exemple, si vous développez la solution sur un ordinateur chinois, mais que vous la déployez sur un serveur appartenant à une filiale française), la définition de la langue et du classement au niveau de la base de données est la première et la plus importante étape pour garantir le fonctionnement de la solution dans l'environnement cible. Le meilleur emplacement pour définir ces propriétés est à l’intérieur du projet (via la commande **Modifier la base de données** sur le projet).  
  
-   **Dimension de base de données**. Bien que le concepteur expose les propriétés **Language** et **Collation** sur une dimension de base de données, la définition des propriétés sur cet objet n'est pas utile. Les dimensions de base de données ne sont pas utilisées comme objets autonomes. Il peut donc être difficile, voire impossible, d'utiliser les propriétés que vous définissez. Dans un cube, une dimension hérite toujours des propriétés **Language** et **Collation** de son cube parent. Les valeurs que vous avez pu définir sur l'objet de dimension de base de données autonome sont ignorées.  
  
-   **Cube**. S'agissant d'une structure de requête principale, vous pouvez définir la langue et le classement au niveau du cube. Par exemple, vous souhaiterez peut-être créer plusieurs versions linguistiques d'un cube (une en anglais et une en chinois) dans le même projet, où chaque cube a son propre langage et son propre classement.  
  
     Quels que soient la langue et le classement que vous définissez sur le cube, ils sont utilisés par toutes les mesures et les dimensions contenues dans le cube. La seule façon de définir des propriétés de classement plus fines consiste à créer des traductions sur un attribut de dimension. Sinon, en supposant qu'il n'existe aucune traduction au niveau des attributs, il existe un classement par cube.  
  
 En outre, vous pouvez définir la propriété **Language**sur un objet de **traduction** .  
  
 Un objet de traduction est créé lorsque vous ajoutez des traductions à un cube ou à une dimension. **Language** fait partie de la définition de la traduction. La propriété**Collation**, quant à elle, est définie sur le cube ou plus haut et elle est partagée par toutes les traductions. Ceci est évident dans le code XMLA d'un cube contenant des traductions, où figurent plusieurs propriétés de langue (une pour chaque traduction), mais un seul classement. Notez qu'il existe une exception pour les traductions d'attributs de dimension, où vous pouvez remplacer le classement de cube pour spécifier un classement d'attribut qui correspond à la colonne source (le moteur de base de données prend en charge la définition du classement sur des colonnes spécifiques et il est courant de configurer des traductions pour obtenir des données de membres à partir de colonnes sources différentes). Mais sinon, pour toutes les autres traductions, la propriété **Language** est utilisée seule, sans corollaire **Collation** . Pour plus d’informations, consultez [Prise en charge des traductions dans Analysis Services](../analysis-services/translation-support-in-analysis-services.md) .  
  
##  <a name="bkmk_lang"></a> Prise en charge linguistique dans Analysis Services  
 La propriété **Language** définit les paramètres régionaux d’un objet, utilisés durant le traitement, les requêtes et avec **Captions** et **Translations** pour prendre en charge les scénarios multilingues. Les paramètres régionaux sont basés sur un identificateur de langue, tel qu'Anglais, et un territoire, tel qu'États-Unis ou Australie, qui affine les représentations de date et d'heure.  
  
 Au niveau de l'instance, la propriété est définie lors de l'installation et elle est basée sur la langue du système d'exploitation de serveur Windows (l'une des 37 langues, en supposant qu'un module linguistique soit installé). Vous ne pouvez pas modifier la langue lors de l'exécution du programme d'installation.  
  
 Après l’installation, vous pouvez remplacer **Language** à l’aide de la page de propriétés du serveur dans Management Studio ou dans le fichier de configuration msmdsrv.ini. Vous pouvez choisir parmi de nombreuses langues, y compris toutes celles prises en charge par le client Windows. Quand elle est définie au niveau de l'instance, sur le serveur, la propriété **Language** détermine les paramètres régionaux de toutes les bases de données déployées par la suite. Par exemple, si vous affectez la valeur Allemand à la propriété **Language** , toutes les bases de données déployées sur l'instance auront une propriété Language 1031 (le LCID correspondant à l'allemand).  
  
###  <a name="bkmk_lcid"></a> La valeur de la propriété Language est un identificateur de paramètres régionaux (LCID)  
 Les valeurs valides incluent tout LCID qui figure dans la liste déroulante. Dans Management Studio et SQL Server Data Tools, les LCID sont représentés sous formes de chaînes équivalentes. Les mêmes langues sont utilisées partout où la propriété **Language** est exposée, indépendamment de l'outil. Le fait d'avoir une liste de langues identique permet de s'assurer que vous pouvez implémenter et tester les traductions de manière cohérente dans tout le modèle.  
  
 Bien qu'Analysis Services répertorie les langues par nom, la valeur réelle stockée pour la propriété est un LCID. Quand vous définissez une propriété de langue par programmation ou via le fichier msmdsrv.ini, utilisez [l’identificateur de paramètres régionaux (LCID)](http://en.wikipedia.org/wiki/Locale) comme valeur. Un LCID est une valeur 32 bits constituée d'un ID de langue, d'un ID de tri et de bits réservés qui identifient une langue particulière. [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] utilise des LCID pour spécifier la langue sélectionnée pour les instances et objets [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
 Vous pouvez définir le LCID au format hexadécimal ou décimal. Voici quelques exemples de valeurs valides pour la propriété **Language** :  
  
-   0x0409 ou 1033 pour **Anglais (États-Unis)**  
  
-   0x0411 ou 1041 pour **Japonais**  
  
-   0x0407 ou 1031 pour **Allemand (Allemagne)**  
  
-   0x0416 ou 1046 pour **Portugais (Brésil)**.  
  
 Pour afficher une liste plus longue, consultez [ID de paramètres régionaux assignés par Microsoft](http://msdn.microsoft.com/goglobal/bb964664.aspx). Pour plus d'informations, consultez [Codage et pages de codes](http://msdn.microsoft.com/goglobal/bb688114.aspx).  
  
> [!NOTE]  
>  La propriété **Language** ne détermine pas la langue du retour des messages système ou des chaînes qui apparaissent dans l'interface utilisateur. Les erreurs, avertissements et messages sont traduits dans toutes les langues prises en charge dans Office et Office 365 et sont automatiquement utilisés quand la connexion cliente spécifie l'un des paramètres régionaux pris en charge.  
  
##  <a name="bkmk_collations"></a> Prise en charge du classement dans Analysis Services  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] utilise exclusivement les classements Windows (versions _90 et _100) et binaires. Il n'utilise pas les classements SQL Server hérités. Dans un cube, un classement unique est utilisé partout, à l'exception des traductions au niveau de l'attribut. Pour plus d’informations sur la définition des traductions d’attributs, consultez [Prise en charge des traductions dans Analysis Services](../analysis-services/translation-support-in-analysis-services.md).  
  
 Les classements contrôlent le respect de la casse pour toutes les chaînes dans un script de langue bicaméral, à l'exception des identificateurs d'objets. Si vous utilisez des caractères minuscules et majuscules dans un identificateur d’objet, sachez que le respect de la casse des identificateurs d’objets n’est pas déterminé par le classement, mais par [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Les identificateurs d'objets composés en script anglais ne respectent jamais la casse, quel que soit le classement. Pour le cyrillique et les autres langues bicamérale, c'est l'inverse (la casse est toujours respectée). Pour plus d'informations, consultez [Globalization Tips and Best Practices &#40;Analysis Services&#41;](../analysis-services/globalization-tips-and-best-practices-analysis-services.md) .  
  
 Le classement dans Analysis Services est compatible avec celui du moteur de base de données relationnelle SQL Server, en supposant que vous mainteniez la parité dans les options de tri que vous sélectionnez pour chaque service. Par exemple, si la base de données relationnelle respecte les accents, vous devez configurer le cube de la même façon. Des problèmes peuvent survenir quand les paramètres de classement divergent. Pour obtenir un exemple et des solutions de contournement, consultez [Les vides dans une chaîne Unicode sont traités différemment selon le classement](http://social.technet.microsoft.com/wiki/contents/articles/23979.ssas-processing-error-blanks-in-a-unicode-string-have-different-processing-outcomes-based-on-collation-and-character-set.aspx). Pour plus d'informations sur le classement et le moteur de base de données, consultez [Collation and Unicode Support](../relational-databases/collations/collation-and-unicode-support.md).  
  
###  <a name="bkmk_collationtype"></a> Types de classements  
 Analysis Services prend en charge deux types de classements :  
  
-   **Classements Windows (versions _90 et _100)**  
  
     Les classements Windows ont deux types de version : _90 (version antérieure non marquée) et _100 (version plus récente). Seule la version _100 indique le numéro de version dans le nom du classement :  
  
    -   latin1_general  
  
    -   latin1_general_100  
  
     Un classement Windows trie les caractères selon les caractéristiques linguistiques et culturelles de la langue. Dans Windows, les classements sont plus nombreux que les paramètres régionaux (ou langues) utilisés avec eux, car de nombreuses langues partagent le même alphabet et les mêmes règles de tri et de comparaison des caractères. Par exemple, 33 groupes de paramètres régionaux Windows, notamment tous les groupes de paramètres régionaux portugais et anglais, utilisent la page de codes Latin1 (1252) et possèdent un ensemble de règles communes pour le tri et la comparaison des caractères.  
  
    > [!NOTE]  
    >  Lorsque vous choisissez un classement, il est recommandé d’utiliser le même classement que celui de la base de données sous-jacente. Si vous avez le choix, la version _100 est cependant plus à jour et offre une convention de tri culturelle plus précise sur le plan linguistique.  
  
-   **Classements binaires (BIN ou BIN2)**  
  
     Les classements binaires effectuent un tri sur des points de code Unicode, et non sur des valeurs linguistiques. Par exemple, Latin1_General_BIN et Japanese_BIN renvoient les mêmes résultats de tri lorsqu'ils sont appliqués à des données Unicode. Alors qu'un tri linguistique peut générer des résultats comme aAbBcCdD, un tri binaire serait ABCDabcd car le point de code de tous les caractères majuscules est collectivement plus élevé que les points de code des caractères minuscules.  
  
###  <a name="bkmk_sortorder"></a> Options d'ordre de tri  
 Des options de tri sont utilisées pour affiner les règles de tri et de comparaison en fonction du respect de la casse, des accents, des caractères kana et de la largeur. Par exemple, la valeur par défaut de la propriété de configuration **Collation** d’ [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] est Latin1_General_AS_CS, ce qui indique que le classement Latin1_General est utilisé avec un ordre de tri respectant les accents et la casse.  
  
 Notez que BIN et BIN2 sont mutuellement exclusifs d'autres options de tri. Si vous souhaitez utiliser BIN ou BIN2, désactivez l'option de tri pour le respect des accents. De même, quand BIN2 est activé, les options Respecter la casse, Non-respect de la casse, Respecter les accents, Non-respect des accents, Respecter les caractères Kana et Respecter la largeur ne sont pas disponibles.  
  
 Le tableau suivant décrit les options d'ordre de tri des classements Windows et les suffixes correspondants pour [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
|Ordre de tri (suffixe)|Description de l'ordre de tri|  
|---------------------------|----------------------------|  
|Binaire (_BIN) ou BIN2 (_BIN2)|Il existe deux types de classements binaires dans SQL Server ; les anciens classements BIN et les nouveaux classements BIN2. Dans un classement BIN2, tous les caractères sont triés selon leurs points de code. Dans un classement BIN, seul le premier caractère est trié selon le point de code et les autres caractères sont triés selon leurs valeurs d'octets. (En raison de l'architecture little endian de la plateforme Intel, les caractères de code Unicode sont toujours triés inversés par octet.)<br /><br /> Pour le classement binaire des types de données Unicode, les paramètres régionaux (la langue) ne sont pas pris en compte dans les tris de données. Par exemple, Latin_1_General_BIN et Japanese_BIN produisent des résultats de tri identiques quand ils sont utilisés avec des données Unicode.<br /><br /> L'ordre de tri binaire respecte la casse et les accents. Il s'agit aussi de l'ordre de tri le plus rapide.|  
|Respecter la casse (_CS)|Fait la distinction entre les majuscules et les minuscules. Si cette option est activée, les minuscules sont triées avant leurs équivalents majuscules. Vous pouvez explicitement définir le non-respect de la casse en spécifiant _CI. Les paramètres de casse spécifiques au classement ne s'appliquent pas aux identificateurs d'objets, tels que l'ID d'une dimension, le cube et d'autres objets. Pour plus d'informations, consultez [Globalization Tips and Best Practices &#40;Analysis Services&#41;](../analysis-services/globalization-tips-and-best-practices-analysis-services.md) .|  
|Respecter les accents (_AS)|Fait la distinction entre les caractères accentués et non accentués. Par exemple, « a » n'est pas équivalent à « ấ ». Si cette option est désactivée, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] considère que la version accentuée et la version non accentuée d'une même lettre sont identiques dans les opérations de tri. Vous pouvez explicitement définir le non-respect des accents en spécifiant _AI.|  
|Respecter le jeu de caractères Kana (_KS)|Fait la distinction entre les deux types de caractères japonais Kana : Hiragana et Katakana. Si cette option est désactivée, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] considère que les caractères Hiragana et Katakana sont des caractères identiques dans les opérations de tri. Il n'existe pas de suffixe d'ordre de tri pour les tris ne respectant pas le jeu de caractères Kana.|  
|Respecter la largeur (_WS)|Fait la distinction entre un caractère codé sur un octet et le même caractère représenté sur deux octets. Si cette option est désactivée, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] considère que la version codée sur un octet et la version codée sur deux octets d’un même caractère sont identiques dans les opérations de tri. Il n'existe pas de suffixe d'ordre de tri pour les tris ne respectant pas la largeur.|  
  
##  <a name="bkmk_defaultLang"></a> Modifier la langue ou le classement par défaut de l'instance  
 La langue et le classement par défaut sont établis pendant l'installation, mais vous pouvez les modifier dans le cadre de la configuration de post-installation. La modification du classement au niveau de l'instance n'est pas une opération triviale. Elle comporte les exigences suivantes :  
  
-   Redémarrage du service.  
  
-   Mise à jour des paramètres de classement des objets existants. Les paramètres de classement sont hérités une fois quand l'objet est créé. Les modifications ultérieures apportées au classement doivent être effectuées manuellement. Pour plus d'informations, consultez [Modifier la langue et le classement dans un modèle de données à l'aide de XMLA](#bkmk_XMLA) .  
  
-   Retraitement des partitions et des dimensions une fois le classement mis à jour.  
  
 Vous pouvez utiliser SQL Server Management Studio ou PowerShell AMO pour modifier la langue ou le classement par défaut au niveau du serveur. Vous pouvez également modifier le  **\<langue >** et  **\<CollationName >** paramètres dans le fichier msmdsrv.ini, en spécifiant le LCID de la langue.  
  
1.  Dans Management Studio, cliquez avec le bouton droit sur le nom du serveur | **Propriétés** | **Langue/Classement**.  
  
2.  Choix des options de tri. Pour sélectionner **Binaire** ou **Binaire 2**, décochez d'abord la case **Respecter les accents**.  
  
     Notez que le classement et la langue sont des paramètres entièrement indépendants. Si vous en changez un, les valeurs de l'autre ne sont pas filtrés pour afficher les combinaisons courantes.  
  
3.  Mise à jour du modèle de données pour utiliser le nouveau classement (voir la section suivante).  
  
4.  Redémarrage du service.  
  
##  <a name="bkmk_cube"></a> Modifier la langue ou le classement d'un cube  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur un cube pour l'ouvrir dans le Concepteur de cube.  
  
2.  Dans le volet Mesures ou Dimensions, sélectionnez le nœud supérieur. L'objet de niveau supérieur de l'un des volets est le cube.  
  
3.  Dans Propriétés, définissez **Language** et **Collation**. Les valeurs que vous choisissez sont utilisés par tous les objets du cube, y compris les dimensions et les mesures du cube, et elles affectent les opérations de traitement et de requête.  
  
     La seule façon d'incorporer d'autres propriétés de langue et de classement sur des objets dans le cube consiste à passer par des traductions. Pour plus d’informations, consultez [Prise en charge des traductions dans Analysis Services](../analysis-services/translation-support-in-analysis-services.md) .  
  
##  <a name="bkmk_XMLA"></a> Modifier la langue et le classement dans un modèle de données à l'aide de XMLA  
 Les paramètres de langue et de classement sont hérités une fois quand l'objet est créé. Les modifications ultérieures apportées à ces propriétés doivent être effectuées manuellement. Une approche pour la modification rapide du classement de plusieurs objets consiste à utiliser une commande ALTER sur un script XMLA.  
  
 Par défaut, le classement est défini une seule fois, au niveau de la base de données. L'héritage est implicite dans le reste de la hiérarchie d'objets. Si vous définissez explicitement **Collation** sur des objets du cube, ce qui est autorisé sur des attributs de dimension spécifiques, cela apparaît dans la définition XMLA. Dans le cas contraire, seule la propriété de classement de niveau supérieur existe.  
  
 Avant d'utiliser XMLA pour modifier une base de données existante, assurez-vous de ne pas introduire d'incohérences entre la base de données et les fichiers sources utilisés pour la générer. Par exemple, vous souhaiterez peut-être utiliser XMLA pour modifier rapidement la langue ou le classement à des fins de test de preuve de concept, mais apporter ensuite des modifications au fichier source (consultez [Modifier la langue ou le classement d’un cube](#bkmk_cube)) et redéployer la solution à l’aide des procédures opérationnelles déjà en place.  
  
1.  Dans Management Studio, cliquez avec le bouton droit sur la base de données | **Générer un script de la base de données en tant que** | **ALTER To** | **Nouvelle fenêtre d’éditeur de requête**.  
  
2.  Recherchez et remplacez la langue ou le classement existant par une autre valeur.  
  
3.  Appuyez sur F5 pour exécuter le script.  
  
4.  Retraitez le cube.  
  
##  <a name="bkmk_enablefast1033"></a> Optimisation des performances pour les paramètres régionaux Anglais via EnableFast1033Locale  
 Si vous utilisez l’identificateur de langue Anglais (États-Unis) (0x0409 ou 1033) en tant que langue par défaut de l’instance d’ [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , vous pouvez obtenir de meilleures performances en définissant la propriété de configuration **EnableFast1033Locale** , une propriété de configuration avancée disponible uniquement pour cet identificateur de langue. Si vous attribuez la valeur **True** à cette propriété, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] utilise un algorithme plus rapide pour le hachage de chaînes et les comparaisons. Pour plus d’informations sur la définition des propriétés de configuration, consultez [Propriétés du serveur dans Analysis Services](../analysis-services/server-properties/server-properties-in-analysis-services.md).  
  
##  <a name="bkmk_gb18030"></a> Prise en charge de GB18030 dans Analysis Services  
 GB18030 est une norme distincte utilisée en République populaire de Chine pour l'encodage des caractères chinois. Dans la norme GB18030, les caractères peuvent être encodés sur 1, 2 ou 4 octets de longueur. Dans Analysis Services, il n'y a aucune conversion de données lors du traitement de données provenant de sources externes. Les données sont simplement stockées au format Unicode. Au moment de la requête, une conversion GB18030 est effectuée via les bibliothèques clientes Analysis Services (plus précisément, le fournisseur OLE DB MSOLAP.dll) quand des données texte sont retournées dans les résultats de la requête, en fonction des paramètres du système d'exploitation client. Le moteur de base de données prend également en charge la norme GB18030. Pour plus d'informations, consultez [Collation and Unicode Support](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Scénarios de globalisation pour Analysis Services](../analysis-services/globalization-scenarios-for-analysis-services.md)   
 [Globalisation conseils et meilleures pratiques & #40 ; Analysis Services & #41 ;](../analysis-services/globalization-tips-and-best-practices-analysis-services.md)   
 [Prise en charge d’Unicode et du classement](../relational-databases/collations/collation-and-unicode-support.md)  
  
  
