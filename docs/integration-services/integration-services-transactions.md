---
title: Transactions Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- containers [Integration Services], transactions
- transactions [Integration Services], about transactions in packages
- tasks [Integration Services], transactions
- transactions [Integration Services]
ms.assetid: 3c78bb26-ddce-4831-a5f8-09d4f4fd53cc
caps.latest.revision: 51
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b19b2e2027c5ad8a2338832d5f057f631fdfc505
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-transactions"></a>Transactions Integration Services
  Les packages utilisent les transactions pour lier les actions de base de données que les tâches effectuent en unités atomiques, et maintiennent ce faisant l'intégrité des données. Tous les types de conteneurs [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] (packages, conteneurs de boucles For et Foreach et conteneurs de séquences, ainsi que les hôtes de tâches qui encapsulent chaque tâche) peuvent être configurés pour utiliser les transactions. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] offre trois options de configuration des transactions : **NotSupported**, **Supported**et **Required**.  
  
-   **Required** indique que le conteneur démarre une transaction, à moins qu’une autre transaction soit déjà démarrée par son conteneur parent. Si la transaction existe déjà, le conteneur rejoint la transaction. Par exemple, si un package non configuré pour prendre en charge les transactions inclut un conteneur de séquences utilisant l’option **Required** , le conteneur de séquences démarre sa propre transaction. Si le package a été configuré pour utiliser l’option **Required** , le conteneur de séquences rejoint la transaction du package.  
  
-   **Supported** indique que le conteneur ne démarre pas de transaction, mais rejoint toute transaction démarrée par son conteneur parent. Par exemple, si un package avec quatre tâches d’exécution SQL démarre une transaction et que les quatre tâches utilisent l’option **Supported** , les mises à jour de la base de données effectuées par les tâches d’exécution SQL sont annulées en cas d’échec d’une des tâches. Si le package ne démarre pas de transaction, les quatre tâches d'exécution SQL ne sont pas liées par une transaction et aucune mise à jour de la base de données n'est annulée, sauf celle effectuée par la tâche qui a échoué.  
  
-   **NotSupported** indique que le conteneur ne démarre pas de transaction et ne rejoint aucune transaction existante. Une transaction démarrée par un conteneur parent n'affecte pas les conteneurs enfants configurés pour ne pas prendre en charge les transactions. Par exemple, si un package est configuré pour démarrer une transaction et qu’un conteneur de boucles For dans le package utilise l’option **NotSupported** , aucune des tâches de la boucle For ne peut être annulée si elle échoue.  
  
 Vous pouvez configurer les transactions en définissant la propriété TransactionOption sur le conteneur. Vous pouvez définir cette propriété dans la fenêtre **Propriétés** de [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]ou par programmation.  
  
> [!NOTE]  
>  La propriété **TransactionOption** a un impact sur la décision d’appliquer ou non la valeur de la propriété **IsolationLevel** demandée par un conteneur. Pour plus d’informations, consultez la description de la propriété **IsolationLevel** dans la rubrique [Définition des propriétés d’un package](../integration-services/set-package-properties.md).  
  
## <a name="configure-a-package-to-use-transactions"></a>Configurer un package pour l'utilisation de transactions
Lorsque vous configurez un package pour l'utilisation de transactions, vous avez le choix entre deux options :  
  
-   Avoir une transaction unique pour le package. Dans ce cas, le package *lance* lui-même cette transaction, alors que les tâches et les conteneurs individuels dans le package participent à cette transaction unique.  
  
-   Avoir plusieurs transactions dans le package. Dans ce cas, le package prend en charge des transactions, mais les tâches et les conteneurs individuels dans le package lancent en réalité les transactions.  
  
 Les procédures ci-dessous montrent comment configurer ces deux options.  
  
### <a name="configure-a-package-to-use-a-single-transaction"></a>Configurer un package pour utiliser une transaction unique  
 Dans cette option, le package lui-même lance une transaction unique. Vous configurez le package pour lancer cette transaction en affectant à la propriété TransactionOption du package la valeur **Required**.  
  
 Ensuite, vous inscrivez des tâches et des conteneurs spécifiques dans cette transaction unique. Pour inscrire une tâche ou un conteneur dans une transaction, vous affectez à la propriété TransactionOption de cette tâche ou de ce conteneur la valeur **Supported**.  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package à configurer pour utiliser une transaction.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Cliquez avec le bouton droit n’importe où dans l’arrière-plan de la surface de dessin du flux de contrôle, puis cliquez sur **Propriétés**.  
  
5.  Dans la fenêtre **Propriétés** , définissez la propriété TransactionOption sur **Required**.  
  
6.  Sur la surface de dessin de l’onglet **Flux de contrôle** , cliquez avec le bouton droit sur la tâche ou le conteneur que vous souhaitez inscrire dans la transaction, puis cliquez sur **Propriétés**.  
  
7.  Dans la fenêtre **Propriétés** , définissez la propriété TransactionOption sur **Supported**.  
  
    > [!NOTE]  
    >  Pour inscrire une connexion dans une transaction, inscrivez les tâches qui utilisent la connexion dans la transaction. Pour plus d’informations, consultez [Connexions Integration Services &#40;SSIS&#41;](../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
8.  Répétez les étapes 6 et 7 pour chaque tâche et conteneur que vous voulez inscrire dans la transaction.  
  
### <a name="configure-a-package-to-use-multiple-transactions"></a>Configurer un package pour utiliser plusieurs transactions  
 Dans cette option, le package lui-même prend en charge les transactions mais ne démarre pas une transaction. Vous configurez le package pour prendre en charge les transactions en affectant à la propriété TransactionOption du package la valeur **Supported**.  
  
 Ensuite, vous configurez les tâches et les conteneurs de votre choix au sein du package pour lancer des transactions ou y participer. Pour configurer une tâche ou un conteneur pour lancer une transaction, vous affectez à la propriété TransactionOption de cette tâche ou de ce conteneur la valeur **Required**.   
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] qui contient le package que vous voulez configurer de façon à utiliser des transactions.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Cliquez avec le bouton droit n’importe où dans l’arrière-plan de la surface de dessin du flux de contrôle, puis cliquez sur **Propriétés**.  
  
5.  Dans la fenêtre **Propriétés** , définissez la propriété TransactionOption sur **Supported**.  
  
    > [!NOTE]  
    >  Le package prend en charge les transactions, mais les transactions sont démarrées par la tâche ou des conteneurs dans le package.  
  
6.  Sur la surface de dessin de l’onglet **Flux de contrôle** , cliquez avec le bouton droit sur la tâche ou le conteneur du package pour lequel vous souhaitez commencer une transaction, puis cliquez sur **Propriétés**.  
  
7.  Dans la fenêtre **Propriétés** , définissez la propriété TransactionOption sur **Required**.  
  
8.  Si une transaction est commencée par un conteneur, cliquez avec le bouton droit sur la tâche ou le conteneur que vous souhaitez inscrire dans la transaction, puis cliquez sur **Propriétés**.  
  
9. Dans la fenêtre **Propriétés** , définissez la propriété TransactionOption sur **Supported**.  
  
    > [!NOTE]  
    >  Pour inscrire une connexion dans une transaction, inscrivez les tâches qui utilisent la connexion dans la transaction. Pour plus d’informations, consultez [Connexions Integration Services &#40;SSIS&#41;](../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
10. Répétez les étapes 6 à 9 pour chaque tâche et conteneur qui démarre une transaction.  

## <a name="multiple-transactions-in-a-package"></a>Plusieurs transactions dans un package
Il est possible qu'un package inclue des transactions non liées entre elles dans un package [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Chaque fois qu'un conteneur, au milieu d'une hiérarchie de conteneurs imbriquée, ne prend pas en charge les transactions, les conteneurs au-dessus et au-dessous de lui dans la hiérarchie démarrent des transactions distinctes s'ils sont configurés pour prendre en charge les transactions. Les transactions sont validées ou annulées dans l'ordre, de la tâche la plus imbriquée dans la hiérarchie de conteneurs jusqu'au package. Cependant, si une transaction interne est validée, elle n'est pas annulée si une transaction externe est abandonnée.  
  
### <a name="example-of-multiple-transactions-in-a-package"></a>Exemple de plusieurs transactions dans un package 
 Par exemple, un package comprend un conteneur de séquences qui contient deux conteneurs de boucles Foreach, et chaque conteneur comprend deux tâches d'exécution de requêtes SQL. Le conteneur de séquences et les tâches d'exécution de requêtes SQL prennent en charge les transactions, contrairement aux conteneurs de boucles Foreach. Dans cet exemple, chaque tâche d'exécution de requêtes SQL démarre sa propre transaction et elle n'est pas annulée si la transaction de la tâche de séquence est abandonnée.  
  
 Les propriétés TransactionOption du conteneur de séquences, du conteneur de boucle Foreach et des tâches d’exécution de requêtes SQL sont définies comme suit :  
  
-   La propriété TransactionOption du conteneur de séquences a pour valeur **Required**.  
  
-   Les propriétés TransactionOption des conteneurs de boucles Foreach ont pour valeur **NotSupported**.  
  
-   Les propriétés TransactionOption des tâches d’exécution de requêtes SQL ont pour valeur **Required**.  
  
 Le diagramme ci-dessous représente les cinq transactions non liées du package. Une transaction est démarrée par le conteneur de séquences et quatre transactions par les tâches d'exécution SQL.  
  
 ![Implémentation de plusieurs transactions](../integration-services/media/mw-dts-trans2.gif "Implémentation de plusieurs transactions")  
 
## <a name="inherited-transactions"></a>Transactions héritées
 Un package peut en exécuter un autre à l'aide de la tâche d'exécution de package. Le package enfant, qui est celui exécuté par la tâche d'exécution de package, peut créer sa propre transaction sur package, ou hériter de celle du package parent.  
  
 Un package enfant hérite de la transaction sur package parent si les deux conditions suivantes sont réunies :  
  
-   Le package est appelé par une tâche d'exécution de package.  
  
-   La tâche d'exécution de package qui a appelé le package a également joint la transaction sur package parent.  
  
 Les conteneurs et les tâches dans le package enfant ne peuvent pas être joints à la transaction de package parent, à moins que le package enfant lui-même soit joint à la transaction.  
  
### <a name="example-of-inherited-transactions"></a>Exemple de transactions héritées  
 Le diagramme suivant présente trois packages utilisant tous des transactions. Chaque package comprend plusieurs tâches. Afin de mettre en évidence le comportement des transactions, seules les tâches d'exécution de package sont indiquées. Le package A exécute les packages B et C. Le package B exécute à son tour les packages D et E, et le package C exécute le package F.  
  
 Les packages et les tâches ont les attributs de transaction suivants :  
  
-   **TransactionOption** a pour valeur **Required** sur les packages A et C.  
  
-   **TransactionOption** a pour valeur **Supported** sur les packages B et D, et sur les tâches d’exécution des packages B, D et F.  
  
-   **TransactionOption** a pour valeur **NotSupported** sur le package E, et sur les tâches d’exécution des packages C et E.  
  
 ![Flux de transactions héritées](../integration-services/media/mw-dts-executepack.gif "Flux de transactions héritées")  
  
 Seuls les packages B, D et F peuvent hériter des transactions de leurs packages parents.  
  
 Les packages B et D héritent de la transaction démarrée par le package A.  
  
 Les package F hérite de la transaction démarrée par le package C.  
  
 Les packages A et C contrôlent leurs propres transactions.  
  
 Le package E n'utilise pas de transactions.  
 
  
## <a name="external-resources"></a>Ressources externes  
  
-   Entrée de blog, [How to Use Transactions in SQL Server Integration Services SSIS](http://go.microsoft.com/fwlink/?LinkId=157783)(Comment utiliser des transactions dans SQL Server Integration Services SSIS), sur www.mssqltips.com  
  
## <a name="see-also"></a> Voir aussi  
 [Transactions héritées](http://msdn.microsoft.com/library/90db5564-d41e-4cfe-8c9e-4e68d41eff1c)   
 [Transactions multiples](http://msdn.microsoft.com/library/c3664a94-be89-40c0-a3a0-84b74a7fedbe)  
  
  
