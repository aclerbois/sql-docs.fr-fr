---
title: Destination de fichier brut | Microsoft Docs
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
- sql13.dts.designer.rawfiledest.f1
- sql13.dts.designer.rawfiledestinationconnectionmanager.f1
- sql13.dts.designer.rawfiledestinationcolumns.f1
helpviewer_keywords:
- append options [Integration Services]
- destinations [Integration Services], Raw File
- raw data [Integration Services]
- writing raw data
- Raw File destination
ms.assetid: d311b458-aefc-4b4d-b1a1-4c0ebbb34214
caps.latest.revision: 59
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c749048ddfe4bad75f1bf34d40d806fbe1bb1403
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="raw-file-destination"></a>Destination de fichier brut
  La destination de fichier brut écrit des données brutes dans un fichier. Le format des données étant natif pour la destination, les données ne requièrent aucune traduction et peu d'analyse. Cela signifie que la destination de fichier brut peut écrire des données plus rapidement que d'autres destinations telles que les destinations de fichier plat et OLE DB.  
  
 Outre l'écriture de données brutes dans un fichier, vous pouvez également utiliser la destination de fichier brut pour générer un fichier brut vide qui contient uniquement les colonnes (fichier réservé aux métadonnées), sans avoir à exécuter le package. La source de fichier brut vous permet de récupérer les données brutes précédemment écrites par la destination. Vous pouvez également faire pointer la source de fichier brut vers le fichier réservé aux métadonnées.  
  
 Le format de fichier brut contient les informations de tri. La destination de fichier brut enregistre toutes les informations de tri, y compris les indicateurs de comparaison des colonnes de chaîne. La source de fichier brut lit et applique les informations de tri. Vous avez la possibilité de configurer la source de fichier brut pour ignorer les indicateurs de tri du fichier, à l'aide de l'éditeur avancé. Pour plus d’informations sur les indicateurs de comparaison, consultez [Comparaison de données de type chaîne](../../integration-services/data-flow/comparing-string-data.md).  
  
 Vous pouvez configurer la destination de fichier brut de plusieurs manières :  
  
-   Spécifiez un mode d'accès qui est soit le nom du fichier, soit une variable contenant le nom du fichier dans lequel la destination de fichier brut écrit.  
  
-   Indiquez si la destination de fichier brut ajoute des données à un fichier existant qui possède le même nom ou si elle crée un fichier.  
  
 On utilise fréquemment la destination de fichier brut pour écrire des résultats intermédiaires de données partiellement traitées entre des exécutions de packages. Le stockage de données brutes signifie qu'elles peuvent être lues rapidement par une source de fichier brut, puis davantage transformées avant d'être chargées dans leur destination finale. Par exemple, un package peut s'exécuter à plusieurs reprises et à chaque fois écrire des données brutes dans des fichiers. Ultérieurement, un package différent peut utiliser la source de fichier brut pour lire à partir de chaque fichier, utiliser une transformation d'union totale pour fusionner les données en un jeu de données, puis appliquer des transformations supplémentaires qui résument les données avant de les charger dans leur destination finale, telle qu'une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  La destination de fichier brut prend en charge les données nulles mais non les données d'objets BLOB (Binary Large Objects).  
  
> [!NOTE]  
>  La destination de fichier brut n'utilise pas de gestionnaire de connexions.  
  
 Cette source possède une entrée régulière. Elle ne prend pas en charge de sortie d'erreur.  
  
## <a name="append-and-new-file-options"></a>Options d'ajout et de nouveau fichier  
 La propriété WriteOption inclut des options permettant d’ajouter des données à un fichier existant ou de créer un fichier.  
  
 Le tableau suivant décrit les options disponibles pour la propriété WriteOption.  
  
|Option|Description|  
|------------|-----------------|  
|Ajouter|Ajoute les données à un fichier existant. Les métadonnées des données ajoutées doivent correspondre au format de fichier.|  
|Toujours créer|Crée toujours un nouveau fichier.|  
|Créer une fois|Crée un nouveau fichier. Si le fichier existe, le composant échoue.|  
|Tronquer et ajouter|Tronque un fichier existant, puis écrit les données dans le fichier. Les métadonnées des données ajoutées doivent correspondre au format de fichier.|  
  
 Voici des éléments importants relatifs à l'ajout de données :  
  
-   L'ajout de données à un fichier brut existant n'entraîne pas un nouveau tri des données.  
  
     Vous devez vous assurer que les clés triées restent dans le bon ordre.  
  
-   L'ajout de données à un fichier brut existant n'entraîne pas de modification des métadonnées du fichier (informations de tri).  
  
 Par exemple, un package lit les données triées sur ProductKey (PK). Le flux de données du package ajoute les données à un fichier brut existant. Lors de la première exécution du package, trois lignes sont reçues (PK 1000, 1100, 1200). Le fichier brut contient désormais les données ci-après.  
  
-   1000, productA  
  
-   1100, productB  
  
-   1200, productC  
  
 Lors de la seconde exécution du package, deux nouvelles lignes sont reçues (PK 1001, 1300). Le fichier brut contient désormais les données ci-après.  
  
-   1000, productA  
  
-   1100, productB  
  
-   1200, productC  
  
-   1001, productD  
  
-   1300, productE  
  
 Les nouvelles données sont ajoutées à la fin du fichier brut et les clés triées (PK) ne sont pas ordonnées. Par ailleurs, l'opération d'ajout n'a pas modifié les métadonnées du fichier (informations de tri). Si vous lisez le fichier à l'aide de la source de fichier brut, le composant indique que le fichier est toujours trié sur PK même si les données du fichier ne sont plus dans le bon ordre.  
  
 Pour conserver les clés triées dans le bon ordre lors de l'ajout de données, concevez le flux de données du package comme suit :  
  
1.  Récupérez les nouvelles lignes via Source A.  
  
2.  Récupérez les lignes existantes de RawFile1 via Source B.  
  
3.  Combinez les entrées de Source A et Source B à l'aide de la transformation Union All.  
  
4.  Triez sur PK.  
  
5.  Écrivez dans RawFile2 à l'aide de la destination de fichier brut.  
  
     RawFile1 est verrouillé, car il est lu dans le flux de données.  
  
6.  Remplacez RawFile1 par RawFile2.  
  
### <a name="using-the-raw-file-destination-in-a-loop"></a>Utilisation de la destination de fichier brut dans une boucle  
 Si le flux de données qui utilise la destination de fichier brut est dans une boucle, vous pouvez créer le fichier une fois puis ajouter les données au fichier lorsque la boucle se répète. Pour ajouter des données au fichier, les données ajoutées doivent correspondre au format du fichier existant.  
  
 Pour créer le fichier dans la première itération de la boucle puis ajouter des lignes dans les itérations ultérieures de la boucle, procédez comme suit lors de la conception :  
  
1.  Définissez la propriété sur **CreateOnce** ou **CreateAlways**, et effectuez une itération de la boucle. Le fichier est créé. Cette opération permet de s'assurer de la correspondance entre les métadonnées des données ajoutées et le fichier.  
  
2.  Réinitialisez la propriété WriteOption à **Append** et définissez la propriété ValidateExternalMetadata sur **False**.  
  
 Si vous utilisez l’option **TruncateAppend** au lieu de l’option **Append** , celle-ci tronque les lignes ajoutées dans les itérations précédentes, puis ajoute de nouvelles lignes. L’utilisation de l’option **TruncateAppend** nécessite également que les données correspondent au format du fichier.  
  
## <a name="configuration-of-the-raw-file-destination"></a>Configuration de la destination de fichier brut  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des fichiers bruts](../../integration-services/data-flow/raw-file-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition des propriétés du composant, consultez [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [Raw Files Are Awesome](http://www.sqlservercentral.com/blogs/stratesql/archive/2011/1/1/31-days-of-ssis-_1320_-raw-files-are-awesome-_2800_1_2F00_31_2900_.aspx), sur sqlservercentral.com.  
  
## <a name="raw-file-destination-editor-connection-manager-page"></a>Éditeur de destination de fichier brut (page Gestionnaire de connexions)
  Utilisez l'Éditeur de destination de fichier brut pour configurer la destination de fichier brut et écrire des données brutes dans un fichier.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir l'Éditeur de destination de fichier brut](#open)  
  
-   [Définir les options de l'onglet Gestionnaires de connexions](#connection)  
  
-   [Définir les options de l'onglet Colonnes](#mapping)  
  
###  <a name="open"></a> Ouvrir l'Éditeur de destination de fichier brut  
  
1.  Ajoutez la destination de fichier brut à un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
2.  Cliquez avec le bouton droit sur le composant, puis cliquez sur **Modifier**.  
  
###  <a name="connection"></a> Définir les options de l'onglet Gestionnaires de connexions  
 **Mode d'accès**  
 Sélectionnez la façon dont le nom de fichier est spécifié. Sélectionnez **Nom de fichier** pour entrer directement le nom de fichier et le chemin d'accès, ou bien **Nom de fichier à partir d'une variable** pour spécifier une variable qui contient le nom de fichier.  
  
 **Nom de fichier** ou **Nom de la variable**  
 Entrez le nom et le chemin d'accès du fichier brut, ou sélectionnez la variable contenant le nom du fichier.  
  
 **Option d'écriture**  
 Sélectionnez la méthode utilisée pour créer le fichier et y écrire.  
  
 **Générer le fichier brut initial**  
 Cliquez sur le bouton pour générer un fichier brut vide qui contient uniquement les colonnes (fichier réservé aux métadonnées), sans avoir à exécuter le package. Le fichier contient les colonnes sélectionnées à la page **Colonnes** de la boîte de dialogue **Éditeur de destination de fichier brut**. Vous pouvez faire pointer la source de fichier brut vers ce fichier réservé aux métadonnées.  
  
 Lorsque vous cliquez sur **Générer le fichier brut initial**, un message s'affiche. Cliquez sur **OK** pour commencer la création du fichier. Cliquez sur **Annuler** pour sélectionner une autre liste de colonnes sur la page **Colonnes** .  
  
###  <a name="mapping"></a> Définir les options de l'onglet Colonnes  
 **Colonnes d'entrée disponibles**  
 Sélectionnez une ou plusieurs colonnes d'entrée pour écrire dans le fichier brut.  
  
 **Colonne d'entrée**  
 Une colonne d’entrée est automatiquement ajoutée à cette table quand vous la sélectionnez sous **Colonnes d’entrée disponibles**. Vous pouvez également sélectionner directement la colonne d’entrée dans cette table.  
  
 **Alias de sortie**  
 Spécifiez un autre nom pour la colonne de sortie.  
  
## <a name="raw-file-destination-editor-columns-page"></a>Éditeur de destination de fichier brut (page Colonnes)
  Utilisez l'Éditeur de destination de fichier brut pour configurer la destination de fichier brut et écrire des données brutes dans un fichier.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir l'Éditeur de destination de fichier brut](#open)  
  
-   [Définir les options de l'onglet Gestionnaires de connexions](#connection)  
  
-   [Définir les options de l'onglet Colonnes](#mapping)  
  
###  <a name="open"></a> Ouvrir l'Éditeur de destination de fichier brut  
  
1.  Ajoutez la destination de fichier brut à un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
2.  Cliquez avec le bouton droit sur le composant, puis cliquez sur **Modifier**.  
  
###  <a name="connection"></a> Définir les options de l'onglet Gestionnaires de connexions  
 **Mode d'accès**  
 Sélectionnez la façon dont le nom de fichier est spécifié. Sélectionnez **Nom de fichier** pour entrer directement le nom de fichier et le chemin d'accès, ou bien **Nom de fichier à partir d'une variable** pour spécifier une variable qui contient le nom de fichier.  
  
 **Nom de fichier** ou **Nom de la variable**  
 Entrez le nom et le chemin d'accès du fichier brut, ou sélectionnez la variable contenant le nom du fichier.  
  
 **Option d'écriture**  
 Sélectionnez la méthode utilisée pour créer le fichier et y écrire.  
  
 **Générer le fichier brut initial**  
 Cliquez sur le bouton pour générer un fichier brut vide qui contient uniquement les colonnes (fichier réservé aux métadonnées), sans avoir à exécuter le package. Vous pouvez faire pointer la source de fichier brut vers le fichier réservé aux métadonnées.  
  
 Lorsque vous cliquez sur le bouton, la liste des colonnes s'affiche. Vous pouvez cliquer sur Annuler et modifier les colonnes, ou vous pouvez cliquer sur OK pour continuer à créer le fichier.  
  
###  <a name="mapping"></a> Définir les options de l'onglet Colonnes  
 **Colonnes d'entrée disponibles**  
 Sélectionnez une ou plusieurs colonnes d'entrée pour écrire dans le fichier brut.  
  
 **Colonne d'entrée**  
 Une colonne d’entrée est automatiquement ajoutée à cette table quand vous la sélectionnez sous **Colonnes d’entrée disponibles**. Vous pouvez également sélectionner directement la colonne d’entrée dans cette table.  
  
 **Alias de sortie**  
 Spécifiez un autre nom pour la colonne de sortie.  
  
## <a name="see-also"></a> Voir aussi  
 [Source de fichier brut](../../integration-services/data-flow/raw-file-source.md)   
 [Flux de données](../../integration-services/data-flow/data-flow.md)  
  
  
