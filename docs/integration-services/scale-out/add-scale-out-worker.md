---
title: Ajouter un SSIS Scale Out Worker avec Scale Out Manager | Microsoft Docs
ms.description: This article describes how to add an SSIS Scale Out Worker to an existing Scale Out environment by using Scale Out Manager.
ms.custom: ''
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: scale-out
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
caps.latest.revision: 1
author: haoqian
ms.author: haoqian
manager: craigg
ms.openlocfilehash: 2de6918bf7cb795ba1871b4ac56068c719e721d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-scale-out-worker-with-scale-out-manager"></a>Ajouter un Scale Out Worker avec Scale Out Manager

Integration Services Scale Out Manager simplifie l’ajout d’un Scale Out Worker à votre environnement Scale Out existant. 

Suivez les étapes ci-dessous pour ajouter un Scale Out Worker à votre topologie Scale Out :

## <a name="1-install-scale-out-worker"></a>1. Installer Scale Out Worker
Dans l’Assistant Installation de SQL Server, sélectionnez Integration Services et Scale Out Worker dans la page **Sélection de fonctionnalités**. 
![Sélection de la fonctionnalité Worker](media/feature-select-worker.PNG)

Dans la page **Configuration d’Integration Services Scale Out - Nœud Worker**, vous pouvez cliquer sur **Suivant** pour ignorer la configuration à ce stade et utiliser **Scale Out Manager** pour effectuer la configuration après l’installation.

Terminez l’Assistant Installation.

## <a name="2-open-the-firewall-on-the-scale-out-master-computer"></a>2. Ouvrir le pare-feu sur l’ordinateur Scale Out Master
Ouvrez le port spécifié pendant l’installation de Scale Out Master (8391, par défaut) et le port pour SQL Server (1433, par défaut) dans le Pare-feu Windows sur l’ordinateur Scale Out Master.

## <a name="3-add-a-scale-out-worker-with-scale-out-manager"></a>3. Ajouter un Scale Out Worker avec Scale Out Manager
Exécutez SQL Server Management Studio en tant qu’administrateur et connectez-vous à l’instance SQL Server de Scale Out Master.

Dans l’Explorateur d’objets, cliquez avec le bouton droit sur **SSISDB** et sélectionnez **Gérer Scale Out**. 

![Gérer Scale Out](media/manage-scale-out.PNG)

Dans la boîte de dialogue **Scale Out Manager**, basculez vers **Gestionnaire de workers**. Sélectionnez **+** et suivez les instructions de la boîte de dialogue **Connect Worker** (Connecter le nœud Worker). 

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez [Scale Out Manager](integration-services-ssis-scale-out-manager.md).