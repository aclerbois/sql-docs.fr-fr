---
title: Définition des Options de suivi | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Visual Studio Analyzer tracing [ODBC]
- ODBC data source administrator [ODBC], tracing options
- tracing options [ODBC], ODBC data source administrator
ms.assetid: 44404a79-b716-4bc1-9ffb-70cd8239d237
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 224832615cbbfb20d61240015fed9b683da6e6c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-tracing-options"></a>Définition des Options de suivi
Le **suivi** onglet de la **administrateur de sources de données ODBC** boîte de dialogue vous permet de configurer la manière dont les appels de fonction ODBC sont suivis.  
  
## <a name="how-tracing-works"></a>Le fonctionnement du suivi  
 Lorsque vous démarrez le suivi à partir de la **suivi** onglet, le Gestionnaire de pilotes enregistrera tous les appels de fonction ODBC pour tous les exécutée par la suite d’applications. Appels de fonction ODBC à partir d’applications qui sont en cours d’exécution avant de démarrer le suivi ne sont pas enregistrés. Appels de fonction ODBC sont enregistrées dans un fichier journal que vous spécifiez.  
  
 Le suivi s’arrête uniquement après avoir cliqué sur **arrêter le traçage maintenant**. Souvenez-vous que pendant le suivi est activé, le fichier journal continue à augmenter et que cela affecte les performances de toutes vos applications ODBC.  
  
 Pour plus d’informations sur le traçage, consultez [suivi](../../odbc/reference/develop-app/tracing.md).  
  
### <a name="changes-in-odbc-tracing"></a>Modifications dans le traçage ODBC  
 Avant de MDAC 2.7 SP2, le traçage ODBC a été autorisé uniquement se produire sur un ordinateur à l’échelle, dans lequel trace capture exposés plus d’informations sur toutes les applications ODBC en cours d’exécution sous des identités. Cette partie du suivi pour l’activité liée à ODBC qui peut-être se produire pour les processus de création ou exécution pour le compte d’autres comptes d’utilisateurs locaux et les principaux de sécurité intégrés tels que le Service Local et Service réseau.  
  
 Par défaut, ODBC suivi utilise désormais le mode par utilisateur. Si vous êtes un administrateur local, toutefois, vous pouvez toujours activer le traçage d’échelle de l’ordinateur à l’aide de l’administrateur de Source de données ODBC.  
  
 Pour configurer le mode de suivi ODBC :  
  
1.  Si nécessaire, connectez-vous à l’aide d’un compte qui est membre du groupe Administrateurs Local.  
  
2.  Dans Outils d’administration, ouvrez l’administrateur de Source de données ODBC.  
  
3.  Cliquez sur le **suivi** onglet.  
  
4.  Configurer le mode de suivi à l’aide de la **suivi d’échelle de l’ordinateur pour toutes les identités des utilisateurs** case à cocher :  
  
5.  Pour activer le traçage de l’échelle de l’ordinateur, sélectionnez la case à cocher.  
  
6.  Pour revenir au suivi par utilisateur, désactivez la case à cocher.  
  
7.  Cliquez sur **Appliquer**.  
  
> [!NOTE]  
>  Si vous avez déjà commencé le suivi dans un mode, vous devez arrêter le suivi et basculez vers l’autre mode pour le mode doit être modifié avec succès.  
  
> [!IMPORTANT]  
>  Suivi de l’échelle de l’ordinateur doit être activé que lorsque cela est nécessaire ; dans le cas contraire, il doit rester désactivée.  
  
## <a name="visual-studio-analyzer-tracing"></a>Suivi de Visual Studio Analyzer  
  
> [!IMPORTANT]  
>  Prise en charge de Visual Studio Analyzer a été supprimée à compter de Windows 8 (Visual Studio Analyzer a été inclus uniquement dans les versions antérieures de Visual Studio.). Pour un autre mécanisme de résolution des problèmes, utilisez le suivi de l’offre.  
  
 Suivi de Visual Studio® Analyzer fournit des performances et des informations de débogage sur la couche ODBC. Tous les événements sortants seront déclenchés sur l’interface de niveau supérieur pour présenter une image précis que possible en matière de temps passé dans ODBC (composants). Suivi de Visual Studio Analyzer requiert n’importe quelle source d’événement pour inscrire lors de la configuration de la source. Pour plus d’informations sur ce type de traçage, consultez la documentation de Visual Studio.
