---
title: Gestion des Transactions (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1f2884d49f03d8557d72d8d6d48cdba432a8c37f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="managing-transactions-xmla"></a>Gestion des transactions (XMLA)
  Chaque commande XML for Analysis (XMLA) envoyé à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s’exécute dans le contexte d’une transaction sur la session implicite ou explicite. Pour gérer chacune de ces transactions, vous utilisez la [BeginTransaction](../../analysis-services/xmla/xml-elements-commands/begintransaction-element-xmla.md), [CommitTransaction](../../analysis-services/xmla/xml-elements-commands/committransaction-element-xmla.md), et [RollbackTransaction](../../analysis-services/xmla/xml-elements-commands/rollbacktransaction-element-xmla.md) commandes. En utilisant ces commandes, vous pouvez créer des transactions implicites ou explicites, modifier le nombre de références de transaction, ainsi que les transactions de démarrage, de validation ou d'annulation.  
  
## <a name="implicit-and-explicit-transactions"></a>Transactions implicites et explicites  
 Une transaction est soit implicite, soit explicite :  
  
 **Transaction implicite**  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Crée un *implicite* transaction pour XMLA : une commande si le **BeginTransaction** commande ne spécifie pas le début d’une transaction. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] valide toujours une transaction implicite si la commande aboutit et l'annule si la commande échoue.  
  
 **Transaction explicite**  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Crée un *explicite* transaction si le **BeginTransaction** commande démarre une transaction. Toutefois, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne valide une transaction explicite si un **CommitTransaction** commande est envoyée et annule une transaction explicite si un **RollbackTransaction** commande est envoyée.  
  
 De plus, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] annule les transactions implicites et explicites si la session active se termine avant que la transaction active n'ait abouti.  
  
## <a name="transactions-and-reference-counts"></a>Transactions et nombres de référence  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] comptabilise le nombre de référence de transactions pour chaque session. Toutefois, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne prend pas en charge les transactions imbriquées dans la mesure où une seule transaction active est gérée par session. Si aucune transaction n'est active dans la session active, le nombre de références de transaction est défini à zéro.  
  
 En d’autres termes, chaque **BeginTransaction** commande incrémente le décompte de références d’une unité, tandis que chaque **CommitTransaction** décrémente commande le décompte de références d’une unité. Si un **CommitTransaction** à zéro, la commande définit le nombre de transactions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] valide la transaction.  
  
 Toutefois, le **RollbackTransaction** commande restaure la transaction active quelle que soit la valeur actuelle du décompte de références de transaction. En d’autres termes, un seul **RollbackTransaction** commande restaure la transaction active, quel que soit le nombre **BeginTransaction** commandes ou **CommitTransaction** commandes ont été envoyées, et les jeux de décompte de référence de la transaction à zéro.  
  
## <a name="beginning-a-transaction"></a>Lancement d'une transaction  
 Le **BeginTransaction** commande démarre une transaction explicite dans la session active et incrémente le décompte de références de transaction pour la session en cours d’une unité. Toutes les commandes suivantes sont considérés comme étant dans la transaction active, jusqu'à ce que suffisamment **CommitTransaction** commandes sont envoyées à valider la transaction active ou une seule **RollbackTransaction** commande est envoyée pour restaurer la transaction active.  
  
## <a name="committing-a-transaction"></a>Validation d'une transaction  
 Le **CommitTransaction** commande valide les résultats des commandes exécutées après que le **BeginTransaction** commande a été exécutée sur la session active. Chaque **CommitTransaction** commande décrémente le décompte de références pour les transactions actives sur une session. Si un **CommitTransaction** à zéro, la commande définit le nombre de références [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] valide la transaction active. S’il n’existe aucune transaction active (en d’autres termes, le nombre de références de transaction pour la session active est déjà défini à zéro), un **CommitTransaction** commande génère une erreur.  
  
## <a name="rolling-back-a-transaction"></a>Annulation d'une transaction  
 Le **RollbackTransaction** commande restaure les résultats des commandes exécutées après que le **BeginTransaction** commande a été exécutée sur la session active. Le **RollbackTransaction** commande restaure la transaction active, quel que soit le nombre de références de la transaction actuelle et définit le nombre de références de transaction à zéro. S’il n’existe aucune transaction active (en d’autres termes, le nombre de références de transaction pour la session active est déjà défini à zéro), un **RollbackTransaction** commande génère une erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
