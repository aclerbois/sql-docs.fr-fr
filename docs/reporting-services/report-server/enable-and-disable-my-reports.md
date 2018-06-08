---
title: Activer et désactiver Mes rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- deactivated My Reports folder
- folders [Reporting Services], My Reports
- activated My Reports folder
- My Reports folder [Reporting Services]
- disabling My Reports folder
ms.assetid: 16c76e82-9fd4-417c-9ed3-a7d5bcd1dba2
caps.latest.revision: 37
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: cbeea5fc6c8f8b162471d9afd758f5d740ab97e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-and-disable-my-reports"></a>Activer et désactiver Mes rapports
  La fonctionnalité Mes rapports alloue un espace de stockage personnel dans la base de données du serveur de rapports afin que les utilisateurs puissent enregistrer les rapports qu'ils possèdent dans un dossier privé. En tant qu'administrateur du serveur de rapports, vous pouvez activer ou désactiver cette fonctionnalité, ou modifier la façon dont elle opère en modifiant les paramètres de sécurité qui déterminent ce que les utilisateurs sont autorisés à faire avec cet espace de travail.  
  
 La fonctionnalité Mes rapports est désactivée par défaut. Vous pouvez activer ou désactiver la fonctionnalité pour tous les utilisateurs, mais vous ne pouvez pas l'activer pour un sous-ensemble d'utilisateurs. La plupart des utilisateurs et des organisations trouvent cette fonctionnalité très utile. Examinez les avantages et les inconvénients présentés plus loin dans cette rubrique afin de déterminer si la fonctionnalité convient à votre organisation.  
  
## <a name="how-to-enable-and-disable-my-reports"></a>Comment activer et désactiver Mes rapports  
 Pour activer Mes rapports à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance du serveur de rapports et ouvrez la page **Propriétés du serveur** . Puis sous l’onglet **Général** , sélectionnez l’option **Activer un dossier Mes rapports pour chaque utilisateur** .  
  
 La définition de rôle utilisée pour Mes rapports détermine les actions prises en charge dans l'espace de travail Mes rapports. Si, par exemple, le rôle Mes rapports exclut « Créer des rapports liés », les utilisateurs ne peuvent pas créer de rapports liés dans les dossiers Mes rapports. Pour plus d’informations, consultez [Sécuriser Mes rapports](../../reporting-services/security/secure-my-reports.md).  
  
 Pour désactiver Mes rapports, désactivez l’option **Activer un dossier Mes rapports pour chaque utilisateur**. Le fait de désactiver Mes rapports masque pour les utilisateurs toutes les indications visibles du dossier Mes rapports. Les dossiers de stockage proprement dit (c'est-à-dire les sous-dossiers de Dossiers des utilisateurs) doivent être supprimés manuellement une fois la fonctionnalité désactivée.  
  
### <a name="when-my-reports-is-activated"></a>Lorsque Mes rapports est activé  
 Une fois la fonctionnalité activée, les utilisateurs peuvent voir s'afficher un dossier Mes rapports situé sous le Dossier racine. Outre un dossier Mes rapports, les administrateurs de serveur de rapports voient également un dossier Dossiers des utilisateurs contenant le sous-dossier de chaque utilisateur.  
  
 Pendant que la fonctionnalité est activée, Dossiers des utilisateurs et ses sous-dossiers ne peuvent pas être supprimés. Par ailleurs, le nom « Mes rapports » devient un nom réservé pour les dossiers créés sous le Dossier racine.  
  
 Si vous activez Mes rapports après qu'il a été désactivé, le serveur de rapports crée un nouveau dossier Dossiers des utilisateurs, s'il n'existe pas déjà. Si Dossiers des utilisateurs existe, le serveur de rapports ajoute de nouveaux sous-dossiers lorsque les utilisateurs se connectent à leurs dossiers Mes rapports.  
  
### <a name="when-my-reports-is-deactivated"></a>Lorsque Mes rapports est désactivé  
 Une fois la fonctionnalité désactivée, le nom « Mes rapports » n'est plus réservé ; les utilisateurs peuvent créer un dossier personnel appelé Mes rapports sous le Dossier racine. De plus, la redirection depuis Mes rapports vers des sous-dossiers de Mes rapports propres aux utilisateurs n'est plus assurée. Enfin, tout lien de rapport contenant un dossier Mes rapports propre à un utilisateur dans l'adresse URL ne fonctionnera plus.  
  
## <a name="choosing-to-use-my-reports"></a>Choix d'utiliser Mes rapports  
 La décision d'utiliser Mes rapports dépend de votre choix de dédier ou non des ressources serveur à la prise en charge de l'espace de travail d'un utilisateur. Mes rapports est une fonctionnalité puissante qui permet aux utilisateurs d'avoir un contrôle sur les ressources d'informations qui les aident dans leur activité. Elle leur fournit également un moyen de travailler avec des rapports qui ne sont pas destinés à un usage collectif. L'une des meilleures raisons pour utiliser Mes rapports est que cette fonctionnalité offre une prise en charge sécurisée et gérable à ceux des utilisateurs qui doivent créer et corriger des rapports. Sans cette fonctionnalité, vous pouvez être obligé de créer des dossiers et des stratégies de sécurité à la demande pour divers utilisateurs. Comme les utilisateurs et leurs besoins évoluent, cette approche résulte en un nombre toujours croissant de dossiers et de stratégies difficiles à gérer avec le temps.  
  
 Remarque : Si vous activez Mes rapports, le serveur de rapports crée un dossier Mes rapports pour chaque utilisateur doté d'un compte de domaine qui clique sur le lien Mes rapports, même si cet utilisateur ne souhaite pas utiliser un tel dossier, ou n'en a pas besoin. Il n'existe pas de procédure systématique pour déterminer quels sont les dossiers utilisés. Vous devez vérifier les dossiers manuellement pour déterminer s'ils contiennent des informations.  
  
## <a name="see-also"></a> Voir aussi  
 [Sécuriser Mes rapports](../../reporting-services/security/secure-my-reports.md)   
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)  
  
  
