---
title: Power Pivot Management Dashboard and Usage Data | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 30f0e84ee388a8a452c855fbd045863f7e2389b0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-pivot-management-dashboard-and-usage-data"></a>Tableau de bord de gestion Power Pivot et données d’utilisation
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] est une collection de rapports et de composants WebPart prédéfinis dans l’Administration centrale de SharePoint, qui vous aident à administrer un déploiement de SQL Server [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour SharePoint. Le tableau de bord de gestion fournit des informations sur l'intégrité du serveur, l'activité du classeur et l'actualisation des données. Le tableau de bord utilise les données issues de la collecte des données d'utilisation de SharePoint.  
  
  
##  <a name="prereq"></a> Conditions préalables  
 Pour ouvrir le tableau de bord de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour une application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] que vous gérez, vous devez être administrateur du service.  
  
##  <a name="items"></a> Présentation des différentes sections du tableau de bord  
 [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] contient des composants WebPart et des rapports incorporés qui explorent des catégories d’informations spécifiques. La liste suivante décrit chaque élément du tableau de bord :  
  
|Tableau de bord| Description|  
|---------------|-----------------|  
|Infrastructure - Intégrité du serveur|Affiche les tendances d'utilisation de l'UC, de consommation de mémoire et de temps de réponse aux requêtes au fil du temps afin que vous puissiez évaluer si les ressources système s'approchent de leur capacité maximale ou sont sous-utilisées.|  
|Actions|Contient des liens vers d'autres pages de l'Administration centrale, notamment l'application de service actuelle, une liste des applications de service et la journalisation de l'utilisation.|  
|Activité du classeur - Graphique|Crée des rapports sur la fréquence des accès aux données. Vous pouvez connaître le nombre de connexions aux sources des données [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] établies chaque jour ou chaque semaine.|  
|Activité du classeur - Liste|Crée des rapports sur la fréquence des accès aux données. Vous pouvez connaître le nombre de connexions aux sources des données [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] établies chaque jour ou chaque semaine.|  
|Actualisation des données - Activité récente|Crée des rapports sur l'état des travaux d'actualisation des données, y compris ceux dont l'exécution a échoué. Ce rapport fournit un affichage composite des opérations d'actualisation des données au niveau de l'application. Les administrateurs peuvent d’un coup d’œil voir le nombre de travaux d’actualisation des données définis pour l’application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] dans son intégralité.|  
|Actualisation des données - Défaillances récentes|Répertorie les classeurs [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour lesquels l’actualisation des données n’a pas abouti.|  
|Rapports|Contient des liens vers des rapports que vous pouvez ouvrir dans Excel.|  
  
##  <a name="open"></a> Ouvrir le Tableau de bord de gestion Power Pivot  
 Le tableau de bord affiche les informations d’une seule application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] à la fois. Vous pouvez ouvrir le tableau de bord de gestion à partir de deux emplacements.  
  
### <a name="open-the-dashboard-from-general-application-settings"></a>Ouvrir le tableau de bord à partir des Paramètres généraux de l'application  
  
1.  Dans l’Administration centrale, dans le groupe **Paramètres généraux de l’application**, cliquez sur **Tableau de bord de gestion[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)]**.  
  
2.  Dans la page principale, sélectionnez l’application de service Power Pivot dont vous souhaitez consulter les données opérationnelles.  
  
### <a name="open-the-dashboard-from-a-power-pivot-service-application"></a>Ouvrir le tableau de bord à partir d’une application de service Power Pivot  
  
1.  Dans Administration centrale, sous **Gestion des applications**, cliquez sur **Gérer les applications de service**.  
  
2.  Cliquez sur le nom de l’application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] . Le tableau de bord de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] affiche les données opérationnelles de l’application de service actuelle.  
  
### <a name="change-the-current-service-application"></a>Modifiez l'application de service actuelle.  
 Pour modifier l’application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] actuelle dans le tableau de bord de gestion :  
  
1.  En haut du tableau de bord de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)], repérez le nom de l’application de service actuelle, par exemple **Application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] par défaut**.  
  
2.  Dans le tableau de bord **Actions** , cliquez sur **Lister les applications de service**.  
  
3.  Cliquez sur le nom de l’application de service [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] dont vous souhaitez afficher les rapports du tableau de bord de gestion.  
  
##  <a name="sourcedata"></a> Données sources dans les tableaux de bord  
 Les tableaux de bord, rapports et composants WebPart affichent les données à partir d’un modèle de données interne qui extrait des données du système et des bases de données d’application [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] . Le modèle de données interne est incorporé dans un classeur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] hébergé sur le site Administration centrale. La structure du modèle de données est fixe. Même si vous pouvez utiliser le classeur PowerPivot comme source de données pour créer des rapports, vous ne devez pas en modifier la structure d'une manière qui risquerait de rompre les rapports prédéfinis qui l'utilisent.  
  
 Pour plus d'informations sur la procédure de collecte des données, consultez les rubriques suivantes :  
  
-   [Collecte des données d’utilisation Power Pivot](../../analysis-services/power-pivot-sharepoint/power-pivot-usage-data-collection.md)  
  
-   [Configurer la collecte des données d’utilisation &#40;PowerPivot pour SharePoint](../../analysis-services/power-pivot-sharepoint/configure-usage-data-collection-for-power-pivot-for-sharepoint.md)  
  
 Pour capturer des données sur le système du serveur Power Pivot, vérifiez que la messagerie d’événements, l’historique d’actualisation des données et les autres historiques d’utilisation sont activés pour chaque application de service Power Pivot. Les données sur le serveur et l'utilisation collectées lors du fonctionnement normal du serveur constituent la source de données utilisée dans le modèle de données interne. **Remarque :** si vous désactivez les historiques d'événements ou d'utilisation, les rapports composites seront incomplets ou erronés.  
  
##  <a name="edit"></a> Modifier le tableau de bord Power Pivot  
 Si vous avez des compétences en matière de développement ou de personnalisation de tableaux de bord, vous pouvez modifier le tableau de bord pour y inclure de nouveaux composants WebPart. Vous avez également la possibilité de modifier les propriétés des composants WebPart inclus dans le tableau de bord.  
  
##  <a name="reports"></a> Créer des rapports personnalisés pour le Tableau de bord de gestion Power Pivot  
 À des fins de création de rapports, les données d’utilisation et l’historique [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] sont conservés dans un classeur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] interne créé et configuré en même temps que le tableau de bord. Si les rapports par défaut ne fournissent pas les informations qui vous intéressent, vous pouvez créer des rapports personnalisés dans Excel, basés sur le classeur. Le classeur et tous les rapports personnalisés créés sont conservés si vous mettez à niveau ou désinstallez les fichiers solution [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] ultérieurement. Le classeur et les rapports sont stockés dans la bibliothèque de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] sur le site Administration centrale. Cette bibliothèque n'est pas visible par défaut, mais vous pouvez l'afficher en utilisant l'action Afficher tout le contenu du site dans les Actions du site.  
  
 Pour vous aider à prendre en main la génération de rapports personnalisés, le tableau de bord de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] propose un fichier Office Data Connection (.odc) pour la connexion au classeur source. Par exemple, vous pouvez utiliser le fichier .odc dans Excel pour créer des rapports supplémentaires.  
  
> [!NOTE]  
>  Modifiez le fichier pour éviter l'erreur suivante lors de la tentative d'utilisation du fichier .odc dans Excel : « Échec de l'initialisation de la source de données ». Le fichier .odc généré automatiquement inclut un paramètre qui n'est pas pris en charge par le fournisseur OLE DB MSOLAP. Les instructions suivantes fournissent la solution de contournement permettant de supprimer ces paramètres.  
  
 Vous devez être administrateur de batterie de serveurs ou de service pour générer des rapports basés sur le classeur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] dans l’Administration centrale.  
  
1.  Ouvrez le tableau de bord de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] .  
  
2.  Accédez à la section **Rapports** , au bas de la page.  
  
3.  Cliquez sur **Données de gestion [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)]**.  
  
4.  Enregistrez le fichier .odc dans un dossier local.  
  
5.  Ouvrez le fichier .odc dans un éditeur de texte.  
  
6.  Dans le  **\<odc : ConnectionString >** élément, faites défiler jusqu'à la fin de la ligne et supprimez **données incorporées = False**, puis supprimez **Mode d’édition = 0**. Si le dernier caractère de la chaîne est un point-virgule, supprimez-le.  
  
7.  Enregistrez le fichier. Les étapes restantes dépendent de la version de [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] et d’Excel que vous utilisez.  
  
8.  1.  Démarrez Excel 2013  
  
    2.  Dans le ruban **[!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)]** , cliquez sur **Gérer**.  
  
    3.  Cliquez sur **Données externes** puis sur **Connexions existantes**.  
  
    4.  Cliquez sur le fichier .ODC si vous le voyez. Si vous ne voyez pas le fichier .ODC, cliquez sur **Parcourir** , puis dans le chemin d'accès du fichier, spécifiez le fichier .odc.  
  
    5.  Cliquez sur **Ouvrir**  
  
    6.  Cliquez sur **Tester la connexion** pour vérifier que la connexion aboutit.  
  
    7.  Entrez un nom pour la connexion, puis cliquez sur **Suivant**.  
  
    8.  Dans Spécifier la requête MDX, cliquez sur **Conception** pour ouvrir le concepteur de requêtes MDX afin d'assembler les données avec lesquelles vous souhaitez travailler. **Si vous voyez le message d'erreur** « Le nom de propriété Mode Édition n'est pas correctement mis en forme », vérifiez que vous modifiez le fichier .ODC.  
  
    9. Cliquez sur **OK** , puis sur **Terminer**.  
  
    10. Créez des rapports de tableau croisé dynamique ou de graphique croisé dynamique pour visualiser les données dans Excel.  
  
9. 1.  Démarrez Excel 2010.  
  
    2.  Dans le ruban [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)], cliquez sur **Lancer la fenêtre [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)]**.  
  
    3.  Sur le ruban Conception de la fenêtre [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] , cliquez sur **Connexions existantes**.  
  
    4.  Cliquez sur **Parcourir**.  
  
    5.  Dans le chemin d'accès, spécifiez le fichier .odc.  
  
    6.  Cliquez sur **Ouvrir**. L’Assistant Importation de table démarre, à l’aide de la chaîne de connexion au classeur [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] qui contient les données d’utilisation.  
  
    7.  Cliquez sur **Tester la connexion** pour vérifier l'accès.  
  
    8.  Entrez un nom convivial pour la connexion, puis cliquez sur **Suivant**.  
  
    9. Dans Spécifier la requête MDX, cliquez sur **Conception** pour ouvrir le concepteur de requêtes MDX afin d'assembler les données avec lesquelles vous souhaitez travailler, puis créez des rapports de tableau croisé dynamique ou de graphique croisé dynamique pour visualiser les données dans Excel.  
  
## <a name="see-also"></a>Voir aussi  
 [Actualisation des données Power Pivot avec SharePoint 2010](http://msdn.microsoft.com/en-us/01b54e6f-66e5-485c-acaa-3f9aa53119c9)   
 [Configurer la collecte des données d’utilisation &#40;PowerPivot pour SharePoint](../../analysis-services/power-pivot-sharepoint/configure-usage-data-collection-for-power-pivot-for-sharepoint.md)  
  
  
