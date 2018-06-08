---
title: Extraction de terme, transformation | Microsoft Docs
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
- sql13.dts.designer.termextractiontrans.f1
- sql13.dts.designer.termextraction.termextraction.f1
- sql13.dts.designer.termextraction.inclusionexclusion.f1
- sql13.dts.designer.termextraction.advanced.f1
helpviewer_keywords:
- word boundaries [Integration Services]
- extracting data [Integration Services]
- sentence boundaries
- word extractions [Integration Services]
- Term Extraction transformation
- tagging words
- normalized data [Integration Services]
- tokenizing text [Integration Services]
- parts of speech [Integration Services]
- text extraction [Integration Services]
- term extractions [Integration Services]
- stemming words [Integration Services]
ms.assetid: d0821526-1603-4ea6-8322-2d901568fbeb
caps.latest.revision: 61
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b3ad1facda17ffa7bdc57f2202f06414225d0669
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="term-extraction-transformation"></a>Transformation d'extraction de terme
  La transformation d'extraction de terme extrait des termes à partir de texte d'une colonne d'entrée de transformation, puis écrit les termes dans une colonne de sortie de transformation. La transformation fonctionne uniquement avec du texte en langue anglaise et utilise uniquement son propre dictionnaire d'anglais et ses propres informations linguistiques.  
  
 Vous pouvez utiliser la transformation d'extraction de terme pour découvrir le contenu d'un dataset. Par exemple, du texte contenant des messages électroniques peut fournir des commentaires utiles sur des produits ; vous pourriez donc utiliser la transformation d'extraction de terme pour extraire les rubriques de discussion dans les messages et ainsi analyser les commentaires.  
  
## <a name="extracted-terms-and-data-types"></a>Termes extraits et types de données  
 La transformation d'extraction de terme peut extraire uniquement des noms, uniquement des expressions nominales, ou les deux. Un nom est un nom unique ; une expression nominale est constituée d'au moins deux mots, dont l'un est un nom et l'autre un nom ou un adjectif. Par exemple, si la transformation utilise l’option « uniquement les noms », elle extrait des termes comme *bicycle* et *landscape*; si elle utilise l’option « uniquement les expressions nominales », elle extrait des termes comme *new blue bicycle*, *bicycle helmet*et *boxed bicycles*.  
  
 Les articles et les pronoms ne sont pas extraits. Par exemple, la transformation d’extraction de termes extrait le terme *bicycle* des textes *the bicycle*, *my bicycle*et *that bicycle*.  
  
 La transformation d'extraction de terme génère un score pour chaque terme extrait. Ce score peut être une valeur TFIDF ou la fréquence brute, autrement dit le nombre de fois que le terme normalisé apparaît dans l'entrée. Dans les deux cas, le score est représenté par un nombre réel supérieur à 0. Par exemple, le score TFIDF peut avoir la valeur 0,5 et la fréquence peut être égale à 1 ou 2.  
  
 La sortie de la transformation d'extraction de terme contient seulement deux colonnes. Une colonne contient les termes extraits, tandis que l'autre contient le score. Les noms par défaut des colonnes sont **Terme** et **Score**. La colonne de texte de l'entrée pouvant contenir plusieurs termes, la sortie de la transformation d'extraction de terme possède généralement plus de lignes que l'entrée.  
  
 Si les termes extraits sont écrits dans une table, ils peuvent être utilisés par d'autres transformations de recherche telles que les transformations de recherche, de recherche de terme et de recherche floue.  
  
 La transformation d'extraction de terme ne peut fonctionner qu'avec du texte d'une colonne dont le type de données est DT_WSTR ou DT_NTEXT. Si une colonne contient du texte mais n'a pas l'un de ces types de données, la transformation de conversion de données peut être utilisée pour ajouter une colonne avec le type de données DT_WSTR ou DT_NTEXT au flux de données et copier les valeurs de colonne dans la nouvelle colonne. La sortie de la transformation de conversion de données peut ensuite être utilisée comme entrée de la transformation d'extraction de terme. Pour plus d’informations, voir [Data Conversion Transformation](../../../integration-services/data-flow/transformations/data-conversion-transformation.md).  
  
## <a name="exclusion-terms"></a>Termes d'exclusion  
 Éventuellement, la transformation d'extraction de terme peut faire référence à une colonne de table qui contient des termes d'exclusion, à savoir des termes qui doivent être ignorés par la transformation lors de l'extraction de termes à partir d'un jeu de données. Cela est utile lorsqu'un ensemble de termes a déjà été identifié comme sans importance dans un secteur d'activité ou une industrie particulière, en général parce qu'ils apparaissent à une fréquence tellement élevée qu'ils en deviennent des mots non significatifs. Par exemple, lors de l'extraction de termes à partir d'un dataset qui contient des informations de support clientèle sur une marque de véhicules spécifique, le nom de la marque peut être exclu car il sera mentionné trop fréquemment pour être significatif. Par conséquent, les valeurs de la liste d'exclusion doivent être personnalisées en fonction du jeu de données avec lequel vous travaillez.  
  
 Lorsque vous ajoutez un terme à la liste d'exclusion, tous les termes—mots ou expressions nominales—qui contiennent le terme sont également exclus. Par exemple, si la liste d’exclusion comprend le mot simple *données*, tous les termes qui contiennent ce mot, comme *données*, *exploration de données*, *intégrité des données*et *validation des données* sont également exclus. Si vous souhaitez exclure uniquement les termes composés qui contiennent le mot *données*, vous devez les ajouter explicitement à la liste d’exclusion. Par exemple, si vous souhaitez extraire les occurrences de *données*tout en excluant *validation des données*, vous ajoutez *validation des données* à la liste d’exclusion et vous vérifiez que *données* est supprimé de celle-ci.  
  
 La table de référence doit être une table d'une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou Access. La transformation d'extraction de terme utilise une connexion OLE DB distincte pour se connecter à la table de référence. Pour plus d’informations, consultez [OLE DB Connection Manager](../../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 La transformation d'extraction de terme fonctionne entièrement en mode de mise en cache préalable. Au moment de l'exécution, elle lit les termes d'exclusion à partir de la table de référence et les stocke dans sa mémoire privée avant de traiter des lignes d'entrée de transformation.  
  
## <a name="extraction-of-terms-from-text"></a>Extraction de termes à partir de texte  
 Pour extraire des termes à partir de texte, la transformation d'extraction de terme effectue les tâches suivantes.  
  
### <a name="identification-of-words"></a>Identification des mots  
 Tout d'abord, la transformation d'extraction de terme identifie les mots en exécutant les tâches suivantes :  
  
-   Séparation du texte en mots à l'aide d'espaces, de sauts de lignes et d'autres terminateurs de langue anglaise. Par exemple, les signes de ponctuation comme *?* et *:* sont des caractères de séparation de mots.  
  
-   Préservation des mots connectés par des tirets ou des traits de soulignement. Par exemple, les mots *Presse-papiers* et *ci-dessous* restent en seul mot.  
  
-   Conservation des acronymes incluant des points. Par exemple, la Société *A.B.C* est divisée en **Société** et **ABC**.  
  
-   Fractionnement des mots avec caractères spéciaux. Par exemple, le mot *date/heure* est extrait comme *date* et *heure*, *(bicycle)* comme *bicycle*et C# est traité comme C. Les caractères spéciaux sont ignorés et ne peuvent pas être lexicalisés.  
  
-   Reconnaissance des cas dans lesquels certains caractères spéciaux (tels que l'apostrophe) ne doivent pas fractionner les mots. Par exemple, le mot *l’élément* n’est pas fractionné en deux mots et génère le terme unique *élément* (nom).  
  
-   Fractionnement des expressions temporelles, des expressions monétaires, des adresses de messagerie et des adresses postales. Par exemple, la date *31 janvier 2004* est fractionnée en trois jetons : *31*, *janvier*et *2004*.  
  
### <a name="tagged-words"></a>Mots avec balises  
 Ensuite, la transformation d'extraction de terme effectue un balisage des mots selon l'une des catégories grammaticales suivantes :  
  
-   Nom au singulier. Par exemple, *bicycle* et *potato*.  
  
-   Nom au pluriel. Par exemple, *bicycles* et *potatoes*. Tous les noms au pluriel qui ne sont pas lemmatisés sont soumis à l'extraction de la racine.  
  
-   Nom propre au singulier. Par exemple, *April* et *Peter*.  
  
-   Nom propre au pluriel. Par exemple, *Aprils* et *Pierres*. Pour qu'un nom propre soit soumis à l'extraction de la racine, il doit faire partie du lexique interne, qui est limité aux mots anglais standard.  
  
-   Adjectif. Par exemple, *blue*.  
  
-   Adjectif comparatif qui compare deux choses. Par exemple, *higher* et *taller*.  
  
-   Adjectif superlatif qui identifie une chose comme ayant une qualité supérieure ou inférieure au niveau d'au moins deux autres choses. Par exemple, *highest* et *tallest*.  
  
-   Nombre. Par exemple, *62* et *2004*.  
  
 Les mots qui n'appartiennent pas à ces catégories grammaticales sont ignorés. Par exemple, les verbes et les pronoms sont ignorés.  
  
> [!NOTE]  
>  Le balisage des catégories grammaticales étant basé sur un modèle statistique, il peut ne pas être totalement exact.  
  
 Si la transformation d'extraction de terme est configurée de façon à extraire uniquement les noms, seuls les mots balisés comme des noms ou des noms propres au singulier ou au pluriel sont extraits.  
  
 Si la transformation d'extraction de terme est configurée de façon à extraire uniquement les expressions nominales, les mots balisés comme noms, noms propres, adjectifs et nombres peuvent être combinés pour composer une expression nominale, mais la phrase doit inclure au moins un mot balisé en tant que nom ou nom propre au singulier ou au pluriel. Par exemple, l’expression nominale *highest mountain* combine un mot étiqueté comme adjectif superlatif (*highest*) et un mot étiqueté comme nom (*mountain*).  
  
 Si la transformation d'extraction de terme est configurée de façon à extraire à la fois les noms et les expressions nominales, les règles relatives aux noms et aux expressions nominales sont applicables. Par exemple, la transformation extrait *bicycle* et *beautiful blue bicycle* du texte *many beautiful blue bicycles*.  
  
> [!NOTE]  
>  Les termes extraits demeurent sujets au seuil de fréquence et à la longueur de terme maximale utilisés par la transformation.  
  
### <a name="stemmed-words"></a>Mots réduits à leur radical  
 La transformation d'extraction de terme réduit les noms à leur racine afin d'extraire uniquement le singulier d'un nom. Par exemple, la transformation extrait *man* de *men*, *mouse* de *mice*et *bicycle* de *bicycles*. La transformation utilise son dictionnaire pour obtenir la racine des noms. Les gérondifs sont traités comme des noms s'ils sont présents dans le dictionnaire.  
  
 Comme le montrent les exemples suivants, la transformation d'extraction de terme réduit les mots à leur forme présente dans le dictionnaire, à l'aide de son dictionnaire interne.  
  
-   Suppression du *s* des noms. Par exemple, *bicycles* devient *bicycle*.  
  
-   Suppression du *es* des noms. Par exemple, *stories* devient *story*.  
  
-   Récupération du singulier des noms irréguliers à partir du dictionnaire. Par exemple, *geese* devient *goose*.  
  
### <a name="normalized-words"></a>Mots normalisés  
 La transformation d'extraction de terme normalise les termes qui commencent par une majuscule uniquement à cause de leur position dans la phrase et les remplace par leur forme minuscule. Par exemple, dans les phrases *Dogs chase cats* et *Mountain paths are steep*, *Dogs* et *Mountain* sont normalisés en *dog* et *mountain*.  
  
 La transformation d'extraction de terme normalise les mots de sorte que les versions commençant par une majuscule et par une minuscule ne soient pas traitées comme des termes différents. Par exemple, dans le texte *You see many bicycles in Seattle* et *Bicycles are blue*, *bicycles* et *Bicycles* sont reconnus comme étant e terme et la transformation conserve uniquement *bicycle*. Les noms propres et les mots non répertoriés dans le dictionnaire interne ne sont pas normalisés.  
  
### <a name="case-sensitive-normalization"></a>Normalisation sensible à la casse  
 La transformation d'extraction de terme peut être configurée de façon à considérer les mots en majuscules et en minuscules comme des termes distincts ou comme des variantes différentes du même terme.  
  
-   Si la transformation est configurée de manière à reconnaître des différences de casse, des termes comme *Method* et *method* sont extraits comme deux termes différents. Les mots commençant par une majuscule qui ne sont pas le premier mot d'une phrase ne sont jamais normalisés et sont balisés comme noms propres.  
  
-   Si la transformation est configurée de manière à ne pas faire de distinction de casse, des termes comme *Method* et *method* sont reconnus comme des variantes d’un même terme. La liste de termes extraits peut inclure *Method* ou *method*, selon le terme apparu en premier dans le dataset en entrée. Si le terme *Method* commence par une majuscule seulement car il s’agit du premier mot d’une phrase, il est extrait sous sa forme normalisée.  
  
## <a name="sentence-and-word-boundaries"></a>Limites de mots et de phrases  
 La transformation d'extraction de terme sépare le texte en phrases en utilisant les caractères suivants comme limites de phrases :  
  
-   Caractères de sauts de ligne ASCII 0x0d (retour chariot) et 0x0a (saut de ligne). Pour que ce caractère soit utilisé comme limite de phrase, il doit y avoir deux caractères de saut de ligne ou plus sur une ligne.  
  
-   Traits d'union (–). Pour que ce caractère soit utilisé comme limite de phrase, les caractères situés immédiatement à gauche et à droite du trait d'union ne doivent pas être des lettres.  
  
-   Caractère de soulignement (_). Pour que ce caractère soit utilisé comme limite de phrase, les caractères situés immédiatement à gauche et à droite du trait d'union ne doivent pas être des lettres.  
  
-   Tous les caractères Unicode inférieurs ou égaux à 0x19, ou supérieurs ou égaux à 0x7b.  
  
-   Combinaison de nombres, signes de ponctuation et caractères alphabétiques. Par exemple, *A23B#99* retourne le terme *A23B*.  
  
-   Caractères %, @, &, $, #, \*, :, ;, ., **,** , !, ?, \<, >, +, =, ^, ~, |, \\, /, (, ), [, ], {, }, “, et ‘.  
  
    > [!NOTE]  
    >  Les acronymes incluant un ou plusieurs points (.) ne sont pas divisés en plusieurs phrases.  
  
 La transformation d'extraction de terme fractionne ensuite la phrase en mots à l'aide des limites de mots suivantes :  
  
-   Espace  
  
-   Onglet  
  
-   ASCII 0x0d (retour chariot)  
  
-   ASCII 0x0a (saut de ligne)  
  
    > [!NOTE]  
    >  Si une apostrophe se trouve dans un mot qui constitue une contraction, comme *we’re* ou *it’s*, le mot est scindé au niveau de l’apostrophe ; sinon, les lettres qui suivent l’apostrophe sont supprimées. Par exemple, *we’re* est divisé en *we* et *’re*, tandis que *bicycle’s* est tronqué en *bicycle*.  
  
## <a name="configuration-of-the-term-extraction-transformation"></a>Configuration de la transformation d’extraction de terme  
 La transformation d'extraction de terme utilise des algorithmes et des modèles statistiques internes pour générer ses résultats. Vous devrez peut-être exécuter la transformation d'extraction de terme à plusieurs reprises et examiner les résultats afin de configurer la transformation de sorte qu'elle génère le type de résultats le mieux adapté à votre solution d'exploration de texte.  
  
 La transformation d'extraction de terme possède une entrée régulière, une sortie et une sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="term-extraction-transformation-editor-term-extraction-tab"></a>Éditeur de transformation d'extraction de terme (onglet Extraction de terme)
  Utilisez l'onglet **Extraction de terme** de la boîte de dialogue **Éditeur de transformation d'extraction de terme** pour définir une colonne de texte qui contient le texte à extraire.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 En utilisant les cases à cocher, sélectionnez une colonne de texte à utiliser pour l'extraction de terme.  
  
 **Terme**  
 Fournissez un nom pour la colonne de sortie qui contiendra les termes extraits.  
  
 **Score**  
 Fournissez un nom pour la colonne de sortie qui contiendra le score de chaque terme extrait.  
  
 **Configurer la sortie d'erreur**  
 Utilisez la boîte de dialogue [Configurer l’affichage des erreurs](http://msdn.microsoft.com/library/5f8da390-fab5-44f8-b268-d8fa313ce4b9) pour spécifier la gestion des erreurs dans les lignes qui provoquent des erreurs.  
  
## <a name="term-extraction-transformation-editor-exclusion-tab"></a>Éditeur de transformation d'extraction de terme (onglet Exclusion)
  Utilisez l'onglet **Exclusion** de la boîte de dialogue **Éditeur de transformation d'extraction de terme** pour définir une connexion à une table de connexion et les colonnes qui contiennent des termes d'exclusion.  
  
### <a name="options"></a>Options  
 **Utiliser les termes d'exclusion**  
 Indique si vous voulez exclure des termes au cours de l'extraction de termes en définissant une colonne qui contient les termes d'exclusion. Vous devez définir les propriétés sources suivantes si vous choisissez d'exclure des termes.  
  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez un gestionnaire de connexions OLE DB existant ou créez une connexion en cliquant sur **Nouvelle**.  
  
 **Nouveau**  
 Créez une connexion à une base de données à l’aide de la boîte de dialogue **Configurer le Gestionnaire de connexions OLE DB** .  
  
 **Table ou vue**  
 Sélectionnez la table ou la vue qui contient les termes d'exclusion.  
  
 **Colonne**  
 Sélectionnez la colonne de la table ou de la vue qui contient les termes d'exclusion.  
  
 **Configurer la sortie d'erreur**  
 Utilisez la boîte de dialogue [Configurer l’affichage des erreurs](http://msdn.microsoft.com/library/5f8da390-fab5-44f8-b268-d8fa313ce4b9) pour spécifier la gestion des erreurs dans les lignes qui provoquent des erreurs.  
  
## <a name="term-extraction-transformation-editor-advanced-tab"></a>Éditeur de transformation d'extraction de terme (onglet Avancé)
  Utilisez l’onglet **Avancé** de la boîte de dialogue **Éditeur de transformation d’extraction de terme** pour définir les propriétés de l’extraction, telles que la fréquence et la longueur, et indiquer si les mots ou les phrases doivent être extraits.  
  
### <a name="options"></a>Options  
 **Nom**  
 Indique que la transformation extrait uniquement des noms individuels.  
  
 **Expression nominale**  
 Indique que la transformation extrait uniquement des expressions nominales.  
  
 **Nom et expression nominale**  
 Indique que la transformation extrait des noms et des expressions nominales.  
  
 **Fréquence**  
 Indique que le score correspond à la fréquence du terme.  
  
 **TFIDF**  
 Indique que le score correspond à la valeur TFIDF du terme. Le score TFIDF est le produit de la fréquence des termes (TF, Term Frequency) et de la fréquence inverse de documents (IDF, Inverse Document Frequency), défini comme suit : TFIDF d’un terme T = (fréquence de T) * log((#lignes en entrée) / (#lignes ayant T))  
  
 **Seuil de fréquence**  
 Définissez le nombre d'occurrences d'un mot ou d'une expression avant son extraction. La valeur par défaut est 2.  
  
 **Longueur maximale du terme**  
 Définissez la longueur maximale d'une expression en nombre de mots. Cette option affecte uniquement les expressions nominales. La valeur par défaut est 12.  
  
 **Utiliser l'extraction de terme respectant la casse**  
 Indiquez si l'extraction doit respecter la casse. La valeur par défaut est **False**.  
  
 **Configurer la sortie d'erreur**  
 Utilisez la boîte de dialogue [Configurer l’affichage des erreurs](http://msdn.microsoft.com/library/5f8da390-fab5-44f8-b268-d8fa313ce4b9) pour spécifier la gestion des erreurs dans les lignes qui provoquent des erreurs.  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../../integration-services/integration-services-error-and-message-reference.md)   
 [Transformation de recherche de terme](../../../integration-services/data-flow/transformations/term-lookup-transformation.md)  

