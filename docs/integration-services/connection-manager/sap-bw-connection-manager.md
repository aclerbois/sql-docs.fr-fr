---
title: Gestionnaire de connexions SAP BW | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 06b166a1-a9df-48ea-a0e8-9b8d6979c0a1
caps.latest.revision: 10
f1_keywords:
- sql13.dts.designer.sapbwconnectionmanager.f1
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f462e43582f0922b596903fc8f6043e240d2c854
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-connection-manager"></a>Gestionnaire de connexions SAP BW
  Le gestionnaire de connexions SAP BW est le composant de gestionnaire de connexions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. Ainsi, le gestionnaire de connexions SAP BW fournit la connectivité à un système SAP Netweaver BW version 7 dont ont besoin les composants source et de destination de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. (La source et la destination SAP BW qui font partie du package [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW sont les seuls composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui utilisent le gestionnaire de connexions SAP BW.)  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 Lorsque vous ajoutez un gestionnaire de connexions SAP BW à un package, la propriété **ConnectionManagerType** du gestionnaire de connexions est définie sur **SAPBI**.  
  
## <a name="configuring-the-sap-bw-connection-manager"></a>Configuration du gestionnaire de connexions SAP BW  
 Vous pouvez configurer le gestionnaire de connexions SAP BW de plusieurs manières :  
  
-   Fournissez le client, le nom d'utilisateur, le mot de passe et la langue pour la connexion.  
  
-   Déterminez si la connexion doit être établie à un serveur d'applications unique ou à un groupe de serveurs à charge équilibrée.  
  
-   Fournissez le numéro d'hôte et du système pour un serveur d'applications unique, ou fournissez le serveur de messages, le groupe et le SID pour un groupe de serveurs à charge équilibrée.  
  
-   Activez la journalisation personnalisée des appels de fonction RFC pour les composants de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. (Cette journalisation diffère de la journalisation facultative que vous pouvez activer sur les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].) Pour activer la journalisation des appels de fonction RFC, spécifiez un répertoire dans lequel stocker les fichiers journaux qui sont créés avant et après chaque appel de fonction RFC. (Cette fonctionnalité de journalisation crée un grand nombre de fichiers journaux au format XML. Comme ces fichiers journaux contiennent également toutes les lignes de données qui sont transférées, ils peuvent occuper beaucoup d'espace sur le disque.) Si vous ne sélectionnez pas de répertoire de journal, la journalisation n'est pas activée.  
  
    > [!IMPORTANT]  
    >  Si les données qui sont transférées contiennent des informations sensibles, les fichiers journaux contiendront également ces informations sensibles.  
  
-   Utilisez les valeurs que vous avez entrées pour tester la connexion.  
  
 Si vous ne connaissez pas toutes les valeurs requises pour configurer le gestionnaire de connexions, adressez-vous à votre administrateur SAP.  
  
 Pour obtenir la procédure pas à pas qui montre comment configurer et utiliser le gestionnaire de connexions, la source et la destination SAP BW, consultez le livre blanc [Utilisation de SQL Server 2008 Integration Services avec SAP BI 7.0](http://go.microsoft.com/fwlink/?LinkID=137090). Ce livre blanc explique également comment configurer les objets nécessaires dans SAP BW.  
  
### <a name="using-the-ssis-designer-to-configure-the-source"></a>Utilisation du concepteur SSIS pour configurer la source  
 Pour plus d'informations sur les propriétés du gestionnaire de connexions SAP BW que vous pouvez définir dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Éditeur du gestionnaire de connexions SAP BW](../../integration-services/connection-manager/sap-bw-connection-manager-editor.md)  
  
## <a name="sap-bw-connection-manager-editor"></a>Éditeur du gestionnaire de connexions SAP BW
  Utilisez l' **Éditeur du gestionnaire de connexions SAP BW** pour spécifier les propriétés à utiliser pour se connecter à un système SAP Netweaver BW version 7.  
  
 Le gestionnaire de connexions SAP BW fournit la connectivité à un système SAP Netweaver BW version 7 en vue d'une utilisation par la source ou la destination SAP BW. Pour en savoir plus sur le gestionnaire de connexions SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW, consultez [Gestionnaire de connexions SAP BW](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 **Pour ouvrir l'Éditeur du gestionnaire de connexions SAP BW**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient le gestionnaire de connexions SAP BW.  
  
2.  Dans la zone Gestionnaires de connexions de l'onglet **Flux de contrôle** , procédez de l'une des façons suivantes :  
  
    -   Double-cliquez sur le gestionnaire de connexions SAP BW.  
  
         —ou—  
  
    -   Cliquez avec le bouton droit sur le gestionnaire de connexions SAP BW, puis sélectionnez **Modifier**.  
  
### <a name="options"></a>Options  
  
> [!NOTE]  
>  Si vous ne connaissez pas toutes les valeurs requises pour configurer le gestionnaire de connexions, adressez-vous à votre administrateur SAP.  
  
 **Client**  
 Spécifiez le numéro de client du système.  
  
 **Langage**  
 Spécifiez la langue utilisée par le système. Par exemple, spécifiez **FR** pour le français.  
  
 **User name**  
 Spécifiez le nom d'utilisateur qui sera utilisé pour se connecter au système.  
  
 **Mot de passe**  
 Spécifiez le mot de passe qui sera utilisé avec le nom d'utilisateur.  
  
 **Utiliser le serveur d'applications unique**  
 Connectez-vous à un serveur d'applications unique.  
  
 Pour vous connecter à un groupe de serveurs à charge équilibrée, utilisez plutôt l’option **Utiliser l’équilibrage de charge** .  
  
 **Hôte**  
 Si vous vous connectez à un serveur d'applications unique, spécifiez le nom d'hôte.  
  
> [!NOTE]  
>  Cette option est disponible uniquement si vous avez sélectionné l’option **Utiliser le serveur d’applications unique** .  
  
 **Numéro système**  
 Si vous vous connectez à un serveur d'applications unique, spécifiez le numéro du système.  
  
> [!NOTE]  
>  Cette option est disponible uniquement si vous avez sélectionné l’option **Utiliser le serveur d’applications unique** .  
  
 **Utiliser l’équilibrage de charge**  
 Connectez-vous à un groupe de serveurs à charge équilibrée.  
  
 Pour vous connecter à un serveur d'applications unique, utilisez plutôt l'option **Utiliser le serveur d'applications unique** .  
  
 **Serveur de messages**  
 Si vous vous connectez à un groupe de serveurs à charge équilibrée, spécifiez le nom du serveur de messages.  
  
> [!NOTE]  
>  Cette option est disponible uniquement si vous avez sélectionné l’option **Utiliser l’équilibrage de charge** .  
  
 **Grouper**  
 Si vous vous connectez à un groupe de serveurs à charge équilibrée, spécifiez le nom du groupe de serveurs.  
  
> [!NOTE]  
>  Cette option est disponible uniquement si vous avez sélectionné l’option **Utiliser l’équilibrage de charge** .  
  
 **SID**  
 Si vous vous connectez à un groupe de serveurs à charge équilibrée, spécifiez l'ID système pour la connexion.  
  
> [!NOTE]  
>  Cette option est disponible uniquement si vous avez sélectionné l’option **Utiliser l’équilibrage de charge** .  
  
 **Répertoire du journal**  
 Activez la journalisation des composants de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW.  
  
 Pour activer la journalisation, spécifiez un répertoire pour les fichiers journaux qui sont créés avant et après chaque appel de fonction RFC. (Cette fonctionnalité de journalisation crée un grand nombre de fichiers journaux au format XML. Comme ces fichiers journaux contiennent également toutes les lignes de données qui sont transférées, ils peuvent occuper beaucoup d'espace sur le disque.)  
  
> [!IMPORTANT]  
>  Si les données qui sont transférées contiennent des informations sensibles, les fichiers journaux contiendront également ces informations sensibles.  
  
 Pour spécifier le répertoire du journal, vous pouvez entrer le chemin d'accès au répertoire manuellement, ou cliquer sur **Parcourir** et accéder au répertoire du journal.  
  
 Si vous ne sélectionnez pas de répertoire de journal, la journalisation n'est pas activée.  
  
 **Parcourir**  
 Parcourez l'arborescence pour sélectionner un dossier pour le répertoire du journal.  
  
 **Tester la connexion**  
 Testez la connexion à l'aide des valeurs que vous avez fournies. Après avoir cliqué sur **Tester la connexion**, un message apparaît et indique si la connexion a été établie ou si elle a échoué.  
  
## <a name="see-also"></a> Voir aussi  
 [Composants Microsoft Connector 1.1 pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
  
