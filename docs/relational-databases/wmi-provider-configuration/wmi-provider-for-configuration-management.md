---
title: Fournisseur WMI pour les Concepts de gestion de Configuration | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- WMI Provider for Configuration Management
- SQL Server WMI Provider
- configuration management [WMI]
- WMI Provider for Configuration Management, about WMI Provider for Configuration Management
ms.assetid: 7e41db24-b915-4eb8-a1d6-e6948ee915b7
caps.latest.revision: 24
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 5336850afaa1ac5073d0efbd81b4746b2acd5043
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wmi-provider-for-configuration-management"></a>fournisseur WMI pour Gestion de l'ordinateur
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Le fournisseur WMI est une couche publiée utilisée avec la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le composant logiciel enfichable Gestionnaire de Configuration pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC) et le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager. Il offre une méthode d'interface unifiée avec les appels API qui gèrent les opérations de registre requises par le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il permet également un contrôle et une manipulation avancés des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionnés.  
  
 Le fournisseur WMI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se compose d'une DLL et d'un fichier MOF, compilés automatiquement par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le fournisseur WMI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient un jeu de classes d'objets utilisé pour contrôler les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide des méthodes suivantes :  
  
-   Langage de script tel que VBScript, [!INCLUDE[jsprjscript](../../includes/jsprjscript-md.md)] ou Perl, dans lequel le langage WQL (Windows Query Language) peut être incorporé.  
  
-   Objet <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> d'un programme de code managé SMO.  
  
-   Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou MMC avec le composant logiciel enfichable du fournisseur WMI de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="using-a-script-language"></a>Utilisation d'un langage de script  
 Les avantages de l'utilisation d'un langage de script sont les suivants :  
  
-   Environnement de développement non obligatoire.  
  
-   Vaste disponibilité des fichiers qui prennent en charge le langage de script.  
  
 Le script peut également fonctionner avec d'autres fournisseurs WMI en plus du fournisseur WMI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un administrateur de domaine peut utiliser un script pour installer les services, les paramètres réseau et les paramètres d'alias sur plusieurs ordinateurs d'un réseau.  
  
 Cette section traite plus en détail de l'accès au fournisseur WMI de la gestion de la configuration à partir de scripts.  
  
## <a name="using-the-smo-managedcomputer-object"></a>Utilisation de l'objet SMO ManagedComputer  
 L'objet <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> est un objet SMO managé qui fournit l'accès au fournisseur WMI de la gestion de la configuration. En utilisant un programme SMO, l'objet <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> peut être utilisé pour afficher et modifier les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les paramètres réseau et les paramètres d'alias. Pour plus d’informations, consultez [gestion des Services et les paramètres de réseau en utilisant le fournisseur WMI](../../relational-databases/server-management-objects-smo/tasks/managing-services-and-network-settings-by-using-wmi-provider.md).  
  
## <a name="using-the-microsoft-management-console-or-sql-server-configuration-manager"></a>Utilisation de la console MMC (Microsoft Management Console) ou du Gestionnaire de configuration SQL Server  
 La console MMC fournit une interface pour gérer les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par opposition à un langage de script ou à un programme de code managé. Le composant logiciel enfichable MMC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management peut être utilisé pour arrêter ou démarrer les services, et pour modifier les comptes de service.  
  
 Le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet aussi de gérer les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les protocoles client et serveur, et les alias serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation du fournisseur WMI pour la gestion de la Configuration](../../relational-databases/wmi-provider-configuration/understanding-the-wmi-provider-for-configuration-management.md)   
 [Utilisation du fournisseur WMI pour la gestion de la Configuration](../../relational-databases/wmi-provider-configuration/working-with-the-wmi-provider-for-configuration-management.md)   
 [À l’aide de WQL ou langage de script avec le fournisseur WMI pour la gestion de la Configuration](../../relational-databases/wmi-provider-configuration/using-wql-and-scripting-languages-with-the-wmi-provider.md)  
  
  
