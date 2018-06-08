---
title: SQL Server Integration Services (SSIS) Scale Out | Microsoft Docs
ms.description: This article provides an overview of the SQL Server Integration Services (SSIS) Scale Out feature, which provides high-performance execution of SSIS packages
ms.custom: ''
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: scale-out
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: dcfbd1c5-c001-4fb7-b9ae-916e49ab6a96
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 14f02912f300cfa6b45d38aa95c0d66235aea324
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-scale-out"></a>Integration Services (SSIS) Scale Out
SSIS (SQL Server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]) Scale Out constitue un moyen très performant d’exécuter des packages SSIS en distribuant les exécutions sur plusieurs ordinateurs. Après avoir configuré Scale Out, vous pouvez effectuer plusieurs exécutions de packages en parallèle, en mode Scale Out, à partir de SSMS (SQL Server Management Studio).

## <a name="components"></a>Components
[!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out se compose d’un [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out Master et d’un ou de plusieurs [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out Workers.

-   Le Scale Out Master est responsable de la gestion de Scale Out et reçoit les demandes d’exécution de package des utilisateurs. Pour plus d’informations, consultez [Scale Out Master](integration-services-ssis-scale-out-master.md).

-   Les Scale Out Workers extraient les tâches d’exécution du Scale Out Master et exécutent les packages. Pour plus d’informations, consultez [Scale Out Worker](integration-services-ssis-scale-out-worker.md).

## <a name="configuration-options"></a>Options de configuration
Vous pouvez définir Scale Out dans les configurations suivantes :

-   **Sur un seul ordinateur**, où un Scale Out Master et un Scale Out Worker sont exécutés côte à côte sur le même ordinateur.

-   **Sur plusieurs ordinateurs**, auquel cas chaque Scale Out Worker est sur un ordinateur différent.

## <a name="what-you-can-do"></a>Ce que vous pouvez faire
Après avoir configuré Scale Out, vous pouvez effectuer les opérations suivantes :

-   Exécuter plusieurs packages déployés sur le catalogue SSISDB en parallèle. Pour plus d’informations, consultez [Exécuter des packages dans Scale Out](run-packages-in-integration-services-ssis-scale-out.md).

-   Gérer la topologie Scale Out dans l’application Scale Out Manager. Pour plus d’informations, consultez [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md).

## <a name="next-steps"></a>Étapes suivantes
-   [Bien démarrer avec SSIS (SQL Server Integration Services) Scale Out sur un seul ordinateur](get-started-with-ssis-scale-out-onebox.md)

-   [Procédure pas à pas : Configurer Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md)
