---
title: Télécharger les mises à jour Microsoft - système de plateforme Analytique | Documents Microsoft
description: Cette rubrique explique comment télécharger les mises à jour à partir du catalogue Microsoft Update pour Windows Server Update Services (WSUS) et appliquer ces mises à jour sur les serveurs de matériel système Analytique de la plateforme. Microsoft Update va installer toutes les mises à jour pour Windows et SQL Server. WSUS est installé sur l’ordinateur virtuel VMM de l’application.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: b98a2be90f222fc2c531c1f1983f8882bdab640e
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="download-and-apply-microsoft-updates-for-analytics-platform-system"></a>Téléchargez et appliquez les mises à jour Microsoft pour système de plateforme Analytique
Cette rubrique explique comment télécharger les mises à jour à partir du catalogue Microsoft Update pour Windows Server Update Services (WSUS) et appliquer ces mises à jour sur les serveurs de matériel système Analytique de la plateforme. Microsoft Update va installer toutes les mises à jour pour Windows et SQL Server. WSUS est installé sur l’ordinateur virtuel VMM de l’application.  
  
## <a name="TOP"></a>Avant de commencer  
  
> [!WARNING]  
> N’essayez pas d’appliquer les mises à jour si votre matériel ou n’importe quel composant matériel est arrêté ou dans un échec sur l’état. Dans ce cas, contactez le support technique pour assistance.  
>   
> N’appliquez pas de Microsoft Updates pendant que l’appareil est en cours d’utilisation. Mise à jour peut entraîner des nœuds de dispositifs de redémarrer. Les mises à jour doivent être appliquées pendant une fenêtre de maintenance lorsque l’application n’est pas utilisée.  
  
### <a name="prerequisites"></a>Configuration requise  
Avant d’effectuer ces étapes, vous devez :  
  
-   Configurer WSUS sur votre appareil en suivant les instructions de [configurer Windows Server Update Services &#40;WSUS&#41; &#40;système de plateforme Analytique&#41;](configure-windows-server-update-services-wsus.md).  
  
-   Connaissance des informations de connexion administrateur de domaine de l’ensemble fibre optique.  
  
-   Avoir une connexion avec des autorisations pour accéder à la Console d’administration système Analytique plateforme et afficher des informations d’état appliance.  
  
-   Dans la plupart des cas, WSUS doit accéder aux serveurs en dehors de l’application. Pour prendre en charge ce scénario d’utilisation du système de plateforme Analytique DNS peut être configuré pour prendre en charge d’un redirecteur de nom externe qui autorise les hôtes de système de plateforme Analytique et les Machines virtuelles (VM) à utiliser des serveurs DNS externes pour résoudre les noms en dehors de la équipement. Pour plus d’informations, consultez [utiliser un redirecteur DNS pour résoudre les noms DNS Non Appliance &#40;système de plateforme Analytique&#41;](use-a-dns-forwarder-to-resolve-non-appliance-dns-names.md).  
  
## <a name="bkmk_ImportUpdates"></a>Pour télécharger et appliquer les mises à jour Microsoft  
  
#### <a name="verify-the-appliance-state-indicators"></a>Vérifiez les indicateurs d’état appliance  
  
1.  Ouvrez la Console d’administration et accédez à la page État de l’application. Pour plus d’informations, consultez [contrôler le matériel à l’aide de la Console d’administration &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-the-admin-console.md)  
  
2.  Vérifiez les indicateurs d’état pour tous les nœuds de l’état de l’application.  
  
    -   Il est prudent de continuer avec vert ou des indicateurs de NA.  
  
    -   Évaluer les erreurs d’avertissement (jaune) non critiques. Dans certains cas, les messages d’avertissement n’empêchera pas les mises à jour. S’il existe une erreur de volume de disque non critiques qui n’est pas sur le lecteur C:\, vous pouvez passer à l’étape suivante avant de résoudre l’erreur de volume de disque.  
  
    -   La plupart des indicateurs rouge doivent être résolues avant de continuer. S’il existe des défaillances de disque, utilisez la page des alertes de la Console Administrateur pour vérifier pas plus d’une défaillance de disque au sein de chaque serveur ou une baie de stockage SAN. Cas de défaillance de ne plus d’un disque au sein de chaque serveur ou une baie de stockage SAN, vous pouvez passer à l’étape suivante avant de résoudre les défaillances de disque. Veillez à contacter le support Microsoft pour résoudre les défaillances de disque dès que possible.  
  
#### <a name="synchronize-the-wsus-server"></a>Synchroniser le serveur WSUS  
  
1.  Ouvrez une session en tant qu’administrateur de domaine l’ordinateur virtuel VMM.  
  
2.  Dans le **tableau de bord Gestionnaire de serveur**, dans le **outils** menu, cliquez sur **Windows Server Update Services** (**wsus.msc**).  
  
3.  Dans la Console d’administration WSUS, cliquez sur **synchronisations**.  
  
4.  Si la synchronisation n’est pas en cours d’exécution, cliquez sur **synchroniser maintenant** dans le volet droit. Dans le volet inférieur, il y a un état de synchronisation. Attendez que la synchronisation soit terminée.  
  
#### <a name="approve-microsoft-updates-in-wsus"></a>Approuver les mises à jour dans WSUS  
  
1.  Dans le volet gauche, de la console WSUS, cliquez sur **les mises à jour**.  
  
2.  Dans le **les mises à jour** volet, cliquez sur le **approbation** menu déroulant, définissez **approbation** à **toutes sauf refusées**. Cliquez sur le **état** menu déroulant, définissez **état** à **tout**. Cliquez sur **Actualiser**.  
  
    Avec le bouton droit le **titre** colonne et sélectionnez **le statut du fichier** pour vérifier l’état du fichier une fois le téléchargement terminé.  
  
    Vous pouvez également sélectionner **mises à jour critiques** ou **mises à jour de sécurité** dans les gauche volet et affichage mises à jour disponibles pour ces catégories.  
  
    ![Sélectionnez les mises à jour et modifiez un état. ] (./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSSelectAllUpdates.png "SQL_Server_PDW_WSUSSelectAllUpdates")  
  
3.  Sélectionnez les mises à jour, puis cliquez sur le **approuver** lien dans le volet droit.  
  
    Vous pouvez également cliquez sur les mises à jour sélectionnées, puis cliquez sur **approuver**. Vous pouvez être invité à accepter le « Contrat de licence de Microsoft logiciel ». Dans ce cas, cliquez sur **J’accepte** dans la fenêtre pour continuer.  
  
    ![Sélectionnez toutes les mises à jour s’applique et cliquez sur Approuver. ] (./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSSelectApprove.png "SQL_Server_PDW_WSUSSelectApprove")  
  
4.  Sélectionnez le groupe de serveurs matériel que vous avez créé dans [configurer Windows Server Update Services &#40;WSUS&#41; &#40;système de plateforme Analytique&#41;](configure-windows-server-update-services-wsus.md).  
  
5.  Cliquez sur **approuvée pour l’installation**, puis cliquez sur **OK**.  
  
    ![Approuver les mises à jour pour votre groupe d’ordinateurs. ] (./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSSelectApprovalType.png "SQL_Server_PDW_WSUSSelectApprovalType")  
  
6.  Dans le **progression de l’approbation** boîte de dialogue, lorsque le processus d’approbation est terminé, cliquez sur **fermer**.  
  
    ![Fermez la fenêtre lorsque les mises à jour sont approuvées. ] (./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSCloseApprovalProgressWindow.png "SQL_Server_PDW_WSUSCloseApprovalProgressWindow")  
  
#### <a name="verify-that-the-updates-are-in-wsus"></a>Vérifiez que les mises à jour WSUS  
  
-  Vérifiez l’état des fichiers de mises à jour toutes les. Chaque fichier doit avoir une icône de flèche verte vers la gauche du titre de la. Cela indique que le fichier est prêt pour l’installation.  
  
    ![Le statut du fichier réussit](./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUS_File_Status.png "SQL_Server_PDW_WSUS_File_Status")  
  
    Avant d’installer les mises à jour, assurez-vous qu’ils sont tous téléchargés et disponibles dans la console WSUS.  
  
#### <a name="to-verify-that-all-updates-are-downloaded"></a>Pour vérifier que les mises à jour sont téléchargées.  
  
-  Vérifiez le **état du téléchargement de** des mises à jour dans la console WSUS, comme illustré dans la capture d’écran suivante. Vérifiez que **mises à jour nécessitant des fichiers** est égal à 0 pour confirmer que les mises à jour sont téléchargées. Si ce nombre est supérieur à zéro, vous devrez peut-être revenir en arrière et approuver des mises à jour supplémentaires.  
  
    ![Vérifiez que les mises à jour sont téléchargées. ] (./media/download-and-apply-microsoft-updates/SQL_Server_PDS_WSUS_VerifyDownloadUpdateJPG.png "SQL_Server_PDS_WSUS_VerifyDownloadUpdateJPG")  
  
#### <a name="apply-microsoft-updates"></a>Appliquer les mises à jour Microsoft  
  
1.  Avant de commencer, ouvrez le [contrôler le matériel à l’aide de la Console d’administration &#40;système de plateforme d’Analytique&#41;](monitor-the-appliance-by-using-the-admin-console.md), cliquez sur le **état des appliances** onglet et vérifiez que le  **Cluster** et **réseau** colonnes afficher vert (ou NA) pour tous les nœuds. Si toutes les alertes existent dans une de ces colonnes, l’application n’est peut-être pas en mesure d’installer les mises à jour correctement. Résoudre toutes les alertes existantes dans le **Cluster** et **réseau** colonnes avant de continuer.  
  
2.  Ouvrez une session sur le *< nom_domaine > ***-HST01** nœud en tant que l’administrateur de domaine de l’ensemble fibre optique.  
  
3.  Pour appliquer toutes les mises à jour approuvées pour WSUS, exécutez le programme de mise à jour. Consultez [exécuter le programme de mise à jour](#RunUpdateWizard) ci-dessous pour obtenir des instructions.  
  
#### <a name="verify-the-updates-on-all-nodes"></a>Vérifier les mises à jour sur tous les nœuds  
  
1.  À partir du nœud VMM, lancez la Console d’administration WSUS. Cette application peut être trouvée sous **Démarrer**, **outils d’administration**, **Windows Server Update Services**.  
  
2.  Développez **ordinateurs**.  
  
3.  Développez **tous les ordinateurs**.  
  
4.  Sélectionnez le groupe de serveurs matériel que vous avez créé dans [configurer Windows Server Update Services &#40;WSUS&#41; &#40;système de plateforme Analytique&#41;](configure-windows-server-update-services-wsus.md).  
  
5.  Dans le **état** menu déroulant, sélectionnez **tout** et cliquez sur **Actualiser**.  
  
6.  Développez **mettre à jour des Services**, *<appliance name>*- VMM, **mises à jour**, **les mises à jour**, où *<appliance name>* est le nom de votre application.  
  
7.  Dans le **les mises à jour** ensemble de la fenêtre **approbation** à **toutes sauf refusées**.  
  
8.  Dans le **les mises à jour** , configurez **état** à **ont échoué ou nécessaires**.  
  
9. Cliquez sur **Actualiser**.  
  
10. Si **mises à jour nécessaires** est supérieur à zéro, contactez le support technique pour assistance.  
  
#### <a name="ensure-there-are-no-critical-alerts-in-the-sql-server-pdw-admin-console"></a>Ne vérifiez aucune alerte critique dans la Console d’administration SQL Server PDW  
  
1.  Ouvrez la Console d’administration, cliquez sur l’onglet État de l’application. Consultez [contrôler le matériel à l’aide de la Console d’administration &#40;système de plateforme Analytique&#41;](monitor-the-appliance-by-using-the-admin-console.md).  
  
2.  Vérifiez que le **Cluster** et **réseau** colonnes afficher vert (ou NA) pour tous les nœuds. Si toutes les alertes existent dans une de ces colonnes, l’application n’est peut-être pas en mesure d’installer les mises à jour correctement. Il existe des alertes critiques, contactez le support.  
  
## <a name="RunUpdateWizard"></a>Exécutez le programme de mise à jour  
Suivez ces instructions pour exécuter le programme de mise à jour de système de plateforme Analytique.  
  
> [!NOTE]  
> Le système WSUS est conçu pour exécuter en mode asynchrone peut prendre du temps entièrement appliquer les mises à jour. Lancer une mise à jour permet de planifier une mise à jour, mais ne garantit pas l’activité de mise à jour immédiate.  
  
1.  Assurez-vous que vous êtes connecté dans le nœud HST01 en tant que l’administrateur de domaine de l’ensemble fibre optique.  
  
2.  Ouvrez une fenêtre d’invite de commandes et entrez les commandes suivantes. Remplacez *<parameter>* avec les informations désignées.  
  
**Pour exécuter la mise à jour de Microsoft :**  
  
```  
C:\pdwinst\media\setup.exe /action="MicrosoftUpdate" /DomainAdminPassword="<password>"  
```  
  
**L’état de Microsoft Update :**  
  
```  
C:\pdwinst\media\setup.exe /action="ReportMicrosoftUpdateClientStatus" /DomainAdminPassword="<password>"  
```  
  
## <a name="see-also"></a>Voir aussi  
[Désinstaller les mises à jour Microsoft &#40;Analytique plate-forme système&#41;](uninstall-microsoft-updates.md)  
[Appliquer des correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](apply-analytics-platform-system-hotfixes.md)  
[Désinstallation de correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](uninstall-analytics-platform-system-hotfixes.md)  
[Maintenance de logiciel &#40;Analytique plate-forme système&#41;](software-servicing.md)  
  
