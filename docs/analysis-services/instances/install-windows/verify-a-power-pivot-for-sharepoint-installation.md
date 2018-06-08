---
title: Vérifier une Power Pivot for SharePoint Installation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f047593657806b872aafdda802c9c85ac4526b56
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="verify-a-power-pivot-for-sharepoint-installation"></a>Vérifier une installation Power Pivot pour SharePoint
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  La gestion d’une instance de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint installée dans une batterie de serveurs SharePoint s’effectue via l’Administration centrale de SharePoint. Au minimum, vous pouvez vérifier des pages dans l’Administration centrale et sur les sites SharePoint pour vérifier que les composants et fonctionnalités du serveur [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] sont disponibles. Toutefois, pour une vérification complète, vous devez disposer d’un classeur [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] que vous pouvez publier sur SharePoint et auquel vous pouvez accéder à partir d’une bibliothèque. Vous pouvez, à des fins de test, publier un classeur d’exemple contenant déjà des données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et l’utiliser pour confirmer que l’intégration SharePoint est correctement configurée.  

  
##  <a name="verifyinstall"></a> Vérifiez l'intégration de l'Administration centrale  
 Pour vérifier l’intégration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] avec l’Administration centrale, procédez comme suit :  
  
1.  Dans le menu Démarrer, cliquez sur **Tous les programmes**, ouvrez Produits Microsoft SharePoint 2016 ou Produits Microsoft SharePoint 2013 et cliquez sur **Administration centrale de SharePoint 2016 ou Administration centrale de SharePoint 2013**.  
  
2.  Entrez votre nom d'utilisateur et votre mot de passe, puis cliquez sur **OK**.  
  
     Si vous le souhaitez, vous pouvez modifier des paramètres du navigateur pour ne pas avoir à entrer un nom d'utilisateur et un mot de passe chaque fois vous ouvrez l'Administration centrale. Pour ajouter l'Administration centrale comme site de confiance, procédez comme suit.  
  
    1.  Dans Internet Explorer, dans le menu Outils, cliquez sur **Options Internet**.  
  
    2.  Sous l'onglet Sécurité, dans la section **Cliquez sur une zone pour afficher ou modifier les paramètres de sécurité** , cliquez sur Sites de confiance, puis sur Sites.  
  
    3.  Décochez la case **Exiger un serveur sécurisé (https:) pour tous les sites de cette zone** .  
  
    4.  Dans **Ajouter ce site Web à la zone**, tapez l'URL à votre site, puis cliquez sur **Ajouter**.  
  
    5.  Cliquez sur **Fermer**, puis sur **OK**.  
  
        > [!NOTE]  
        >  La documentation d'installation de SharePoint inclut des instructions supplémentaires permettant de contourner des erreurs de serveur proxy et de désactiver l'option Configuration de sécurité renforcée d'Internet Explorer afin de pouvoir télécharger et installer des mises à jour. Pour plus d'informations, consultez la section **Effectuer des tâches supplémentaires** dans [Déployer un serveur unique avec SQL Server](http://go.microsoft.com/fwlink/?LinkId=177754) sur le site Web Microsoft.  
  
3.  Dans l'Administration centrale, sous Paramètres système, cliquez sur **Gérer les fonctionnalités des batteries de serveurs**.  
  
4.  Vérifiez que l’option **Fonctionnalité d’intégration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]** a la valeur **Active**.  
  
5.  Dans l'Administration centrale, sous Paramètres système, cliquez sur **Gérer les services sur le serveur**.  
  
6.  Vérifiez que le **Service système [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]SQL Server** est démarré.  
  
     Dans une batterie SharePoint comprenant plusieurs serveurs, il se peut que vous deviez changer le serveur affiché pour vérifier que tous les serveurs sur lesquels vous avez déployé [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] sont en cours d’exécution.  
  
7.  Dans l'Administration centrale, sous Gestion des applications, cliquez sur **Gérer les applications de service**.  
  
8.  Cliquez sur **Application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] par défaut** pour ouvrir le Tableau de bord de gestion [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] de cette application. À la première utilisation, le tableau de bord prend plusieurs minutes à charger.  
  
     Vous pouvez également cliquer sur l’espace vide en regard de **Application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] par défaut** pour sélectionner la ligne et cliquer sur **Propriétés** pour afficher les paramètres de configuration de cette application de service. Vous pouvez modifier à la fois des paramètres de configuration et des propriétés d'application pour modifier votre configuration du serveur. Pour plus d’informations sur ces paramètres, consultez [Création et configuration d’une application de service PowerPivot](../../../analysis-services/power-pivot-sharepoint/create-and-configure-power-pivot-service-application-in-ca.md).  
  
## <a name="verify-integration-at-the-site-level"></a>Vérifiez l'intégration au niveau du site  
 Pour vérifier l’intégration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] avec un site SharePoint, procédez comme suit :  
  
1.  Dans un navigateur, ouvrez l'application Web que vous avez créée. Si vous avez utilisé les valeurs par défaut, vous pouvez spécifier http://\<votre nom d’ordinateur > dans l’adresse URL.  
  
2.  Vérifiez que l’accès aux données et les fonctionnalités de traitement de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] sont disponibles dans l’application. Pour cela, vous pouvez vérifier la présence de modèles de bibliothèque fournis par [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]:  
  
    1.  Sélectionnez **Contenu du site**.  
  
    2.  Dans la liste d’applications, vous devriez voir **Bibliothèque de flux de données** et **Galerie [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]**. Ces modèles de bibliothèque sont fournis par la fonctionnalité [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et sont visibles dans la liste des bibliothèques si la fonctionnalité est intégrée correctement.  
  
## <a name="verify-data-access-on-the-server"></a>Vérifiez l'accès aux données sur le serveur  
 Pour vérifier l’accès aux données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] sur le serveur, procédez comme suit :  
  
1.  [Téléchargez](http://go.microsoft.com/fwlink/?LinkID=219108) l'exemple de données Picnic qui accompagne un didacticiel Reporting Services. Vous allez utiliser l’exemple de classeur des fichiers téléchargés pour vérifier l’accès aux données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Extrayez les fichiers.  
  
2.  Téléchargez le classeur Excel (.xlsx) dans Documents partagés. Le classeur contient des données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] incorporées.  
  
3.  Cliquez sur le document pour l'ouvrir à partir de la bibliothèque.  
  
4.  Cliquez sur un segment ou un filtre en haut du classeur. Mois, couleur et type sont des segments dans ce classeur. Cliquer sur un segment permet de lancer une requête [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et de démontrer que votre serveur est opérationnel. Le serveur charge les données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] en arrière-plan et retourne les résultats.  
  
5.  Revenez à la bibliothèque. Sélectionnez la flèche vers le bas située à droite du classeur, puis cliquez sur **Lancer Power View**. Cette étape confirme que la fonctionnalité [!INCLUDE[ssCrescent](../../../includes/sscrescent-md.md)] dans Reporting Services est opérationnelle. Si vous n'avez pas installé Reporting Services, ignorez cette étape.  
  
     À l'étape suivante, vous allez vous connecter au serveur dans Management Studio pour vérifier que les données sont chargées et mises en cache.  
  
6.  Démarrez SQL Server Management Studio à partir du groupe de programmes [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)] dans le menu Démarrer. Si cet outil n'est pas installé sur votre serveur, vous pouvez passer à la dernière étape pour confirmer la présence de fichiers mis en cache.  
  
7.  Dans Type de serveur, sélectionnez **Analysis Services**.  
  
8.  Dans nom du serveur, entrez  **\<nom-serveur > \powerpivot**, où  **\<nom-serveur >** est le nom de l’ordinateur qui a le [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour l’installation de SharePoint.  
  
9. Cliquez sur **Se connecter**. Le programme vérifie que le serveur Analysis Services est disponible.  
  
10. Dans l’Explorateur d’objets, vous pouvez cliquer sur **Bases de données** pour afficher la liste des fichiers de données [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] qui sont chargés.  
  
11. Sur le système de fichiers de l'ordinateur, vérifiez le dossier suivant pour déterminer si les fichiers sont mis en cache sur disque. La présence de fichiers mis en cache est une vérification supplémentaire que votre déploiement est opérationnel. Pour afficher le cache de fichiers, accédez à [!INCLUDE[ssInstallPathVar](../../../includes/ssinstallpathvar-md.md)]MSAS13. POWERPIVOT\OLAP\Backup\Sandboxes\Dossier Application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] par défaut. Chaque base de données mise en cache est stockée dans son propre dossier, en utilisant une convention d'affectation de noms basés sur les GUID pour garantir un nom unique.  
  
  
