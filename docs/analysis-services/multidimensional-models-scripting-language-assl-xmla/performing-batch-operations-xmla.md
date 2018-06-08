---
title: Exécution d’opérations de traitement par lots (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9cb2983bbf03256839870961f40a018997544ff0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="performing-batch-operations-xmla"></a>Exécution d'opérations de traitement par lot (XMLA)
  Vous pouvez utiliser la [lot](../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md) commande XML for Analysis (XMLA) pour exécuter plusieurs commandes XMLA à l’aide d’un seul XMLA [Execute](../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode). Vous pouvez exécuter plusieurs commandes contenues dans le **lot** commande comme une transaction unique ou dans des transactions individuelles pour chaque commande, en série ou en parallèle. Vous pouvez également spécifier des liaisons hors ligne et autres propriétés dans le **lot** commande pour le traitement de plusieurs [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] objets.  
  
## <a name="running-transactional-and-nontransactional-batch-commands"></a>Exécution de commandes Batch transactionnelles et non transactionnelles  
 Le **lot** commande exécute les commandes de deux manières :  
  
 **Transactionnelle**  
 Si le **Transaction** attribut de la **lot** commande est définie sur true, le **lot** commande exécute toutes les commandes que contient le **lot** dans une transaction unique : un *transactionnelle* lot.  
  
 Si l’échec d’une commande dans un lot transactionnel, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] annule les commandes le **lot** commande exécutée avant la commande qui a échoué et le **lot** commande se termine immédiatement. Toutes les commandes dans le **lot** commande qui n’ont pas encore été exécuté ne sont pas exécutées. Après le **lot** commande se termine, le **lot** commande signale des erreurs qui se sont produites pour la commande ayant échouée.  
  
 **Nontransactional**  
 Si le **Transaction** attribut est défini sur false, le **lot** commande s’exécute chaque commande contenue par le **lot** dans une transaction distincte — un *non transactionnel* lot. Si l’échec d’une commande dans un lot non transactionnel, la **lot** commande continue d’exécuter des commandes après la commande qui a échoué. Après le **lot** commande tente d’exécuter toutes les commandes qui la **lot** commande contient, le **lot** commande signale des erreurs qui se sont produites.  
  
 Tous les résultats retournés par les commandes contenues dans un **lot** commande sont retournés dans le même ordre que celui dans lequel les commandes sont contenues dans le **lot** commande. Les résultats retournés par une **lot** commande varient selon que le **lot** commande est transactionnelle ou non transactionnelle.  
  
> [!NOTE]  
>  Si un **lot** commande contient une commande qui ne retourne pas de sortie, tels que le [verrou](../../analysis-services/xmla/xml-elements-commands/lock-element-xmla.md) commande, et que commande s’exécute correctement, le **lot** retourne une commande [racine](../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) élément dans l’élément de résultats. Vide **racine** élément garantit que chaque commande contenue dans un **lot** commande peut être associée à approprié **racine** , élément pour les résultats de cette commande.  
  
### <a name="returning-results-from-transactional-batch-results"></a>Retour des résultats d'un lot transactionnel  
 Résultats des commandes exécutées dans un lot transactionnel ne sont pas retournés tant que l’ensemble du **lot** commande est terminée. Les résultats ne sont pas retournés après l’exécution de chaque commande, car toute commande qu’échoue dans un lot transactionnel provoquerait l’ensemble **lot** commande et toutes les commandes contenant à restaurer. Si toutes les commandes Démarrer et s’exécute correctement, le [retourner](../../analysis-services/xmla/xml-elements-properties/return-element-xmla.md) élément de la [ExecuteResponse](../../analysis-services/xmla/xml-elements-objects-executeresponse.md) élément retourné par la **Execute** méthode pour le **lot** commande contient un [résultats](../../analysis-services/xmla/xml-elements-properties/results-element-xmla.md) élément, qui à son tour contient un **racine** , élément pour chaque commande correctement exécutée contenue dans le **lot** commande. Si la commande le **lot** commande ne peut pas être démarré ou ne se termine, le **Execute** méthode retourne une erreur SOAP pour le **lot** commande qui contient l’erreur de la commande qui a échoué.  
  
### <a name="returning-results-from-nontransactional-batch-results"></a>Retour des résultats d'un lot non transactionnel  
 Résultats des commandes exécutées dans un lot non transactionnel sont retournés dans l’ordre dans lequel les commandes sont contenues dans le **lot** commande et à mesure qu’elles sont retournées par chaque commande. Si aucune commande contenue dans le **lot** commande peut être démarrée, le **Execute** méthode retourne une erreur SOAP qui contient une erreur pour le **lot** commande. Si au moins une commande a démarré avec succès, le **retourner** élément de la **ExecuteResponse** élément retourné par la **Execute** méthode pour le **lot** commande contient un **résultats** élément, qui à son tour contient un **racine** élément pour chaque commande contenue dans le **lot** commande. Si une ou plusieurs commandes dans un lot non transactionnel ne peut pas être démarrés ou ne se termine, le **racine** élément pour la commande ayant échouée contient une [erreur](../../analysis-services/xmla/xml-elements-properties/error-element-xmla.md) élément décrivant l’erreur.  
  
> [!NOTE]  
>  Tant qu’au moins une commande dans un lot non transactionnel peut être démarrée, le lot non transactionnel est considérée comme ayant exécuté avec succès, même si toutes les commandes contenues dans le lot non transactionnel retourne une erreur dans les résultats de la **lot** commande.  
  
## <a name="using-serial-and-parallel-execution"></a>Utilisation d'une exécution en série et parallèle  
 Vous pouvez utiliser la **lot** commandes de commande à exécuter inclus en série ou en parallèle. Lorsque les commandes sont exécutées en série, la commande suivante incluse dans le **lot** commande ne peut pas démarrer tant que la commande en cours d’exécution dans le **lot** commande est terminée. Lorsque les commandes sont exécutées en parallèle, plusieurs commandes peuvent être exécutées simultanément par le **lot** commande.  
  
 Pour exécuter des commandes en parallèle, vous ajoutez les commandes à exécuter en parallèle pour le [parallèles](../../analysis-services/xmla/xml-elements-properties/parallel-element-xmla.md) propriété de la **lot** commande. Actuellement, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut exécuter uniquement contiguës et séquentielles [processus](../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md) commandes en parallèle. Les autres commandes XMLA, tel que [créer](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md) ou [Alter](../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md), inclus dans le **parallèles** propriété est exécutée en série.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tente d’exécuter tous les **processus** commandes incluses dans le **parallèles** propriété en parallèle, mais ne peut pas garantir que toutes les inclus **processus** commandes peuvent être exécutées en parallèle. L’instance analyse chaque **processus** commande et, si elle détermine que la commande ne peut pas être exécutée en parallèle, les **processus** commande est exécutée en série.  
  
> [!NOTE]  
>  Pour exécuter des commandes en parallèle, les **Transaction** attribut de la **lot** commande doit être définie à true, car [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge seulement une transaction active par lots de connexion et non transactionnels exécutent chaque commande dans une transaction distincte. Si vous incluez le **parallèles** propriété dans un lot non transactionnel, une erreur se produit.  
  
### <a name="limiting-parallel-execution"></a>Limitation de l'exécution parallèle  
 Un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance tente d’exécuter autant **processus** commandes en parallèle que possible, dans les limites de l’ordinateur sur lequel s’exécute l’instance. Vous pouvez limiter le nombre de manière simultanée **processus** commandes en définissant le **maxParallel** attribut de la **parallèles** propriété une valeur qui indique le nombre maximal de **processus** commandes qui peuvent être exécutés en parallèle.  
  
 Par exemple, un **parallèles** propriété contient les commandes suivantes dans l’ordre indiqué :  
  
1.  **Créer**  
  
2.  **Traiter**  
  
3.  **Alter**  
  
4.  **Traiter**  
  
5.  **Traiter**  
  
6.  **Traiter**  
  
7.  **Delete**  
  
8.  **Traiter**  
  
9. **Traiter**  
  
 Le **maxParalle**attribut l de ce **parallèles** est définie sur 2. Par conséquent, l'instance exécute les listes précédentes de commandes comme décrit dans la liste suivante :  
  
-   Commande 1 s’exécute en série, car la commande 1 est un **créer** commande et uniquement **processus** commandes peuvent être exécutées en parallèle.  
  
-   La commande 2 s'exécute en série une fois que la commande 1 a abouti.  
  
-   La commande 3 s'exécute en série une fois que la commande 2 a abouti.  
  
-   Les commandes 4 et 5 s'exécutent en parallèle une fois que la commande 3 a abouti. Bien que la commande 6 soit également un **processus** commande, commande 6 ne peut pas s’exécuter en parallèle avec les commandes 4 et 5, car le **maxParallel** est définie sur 2.  
  
-   La commande 6 s'exécute en série une fois que les commandes 4 et 5 ont abouti.  
  
-   La commande 7 s'exécute en série une fois que la commande 6 a abouti.  
  
-   Les commandes 8 et 9 s'exécutent en parallèle une fois que la commande 7 a abouti.  
  
## <a name="using-the-batch-command-to-process-objects"></a>Utilisation de la commande Batch pour traiter les objets  
 Le **lot** commande contient plusieurs propriétés et attributs facultatifs spécifiquement inclus pour prendre en charge le traitement de plusieurs [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projets :  
  
-   Le **ProcessAffectedObjects** attribut du **lot** commande indique si l’instance doit également traiter les objets nécessitant un retraitement suite d’un **processus** commande inclus dans le **lot** commande de traitement d’un objet spécifié.  
  
-   Le [liaisons](../../analysis-services/xmla/xml-elements-properties/bindings-element-xmla.md) propriété contient une collection de liaisons hors ligne utilisée par toutes les **processus** commandes dans le **lot** commande.  
  
-   Le [source de données](../../analysis-services/xmla/xml-elements-properties/datasource-element-xmla.md) propriété contient une liaison hors ligne pour une source de données utilisée par toutes la **processus** commandes dans le **lot** commande.  
  
-   Le [DataSourceView](../../analysis-services/xmla/xml-elements-properties/datasourceview-element-xmla.md) propriété contient une liaison hors ligne pour une vue de source de données utilisée par toutes les **processus** commandes dans le **lot** commande.  
  
-   Le [ErrorConfiguration](../../analysis-services/xmla/xml-elements-properties/errorconfiguration-element-xmla.md) propriété spécifie la manière dont le **lot** commande gère les erreurs rencontrées par tous les **processus** commandes contenues dans le **lot** commande.  
  
    > [!IMPORTANT]  
    >  A **processus** commande ne peut pas inclure le **liaisons**, **DataSource**, **DataSourceView**, ou **ErrorConfiguration** propriétés, si le **processus** commande est contenue dans un **lot** commande. Si vous devez spécifier ces propriétés pour un **processus** command, fournissez les informations nécessaires dans les propriétés correspondantes de la **lot** commande qui contient le **processus** commande.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément de lot & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md)   
 [Traiter l’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
