---
title: Maintenance de logiciel - système de plateforme Analytique | Documents Microsoft
description: Logiciel de maintenance dans le système de plateforme Analytique (APS).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 7d9991dfb310e2cebc3c61bbd6f9f04a40a0f38e
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="software-servicing-in-analytics-platform-system"></a>Maintenance de logiciel dans le système de plateforme Analytique
Cette section résume le logiciel de configuration requise pour les équipements de système de plateforme Analytique, y compris les correctifs logiciels WSUS et le système de plateforme d’Analytique de maintenance.  
  
## <a name="Basics"></a>Principes fondamentaux de la maintenance de logiciel  
**WSUS :** votre appliance Analytique plate-forme système doit être configuré pour recevoir des mises à jour à partir de Windows Server Update Services (WSUS). Ces mises à jour incluent des modifications importantes apportées au logiciel de l’application. Une fois qu’ils sont configurés, nombreuses mises à jour seront installe automatiquement et ne nécessitent pas de gestion pratique. En règle générale, les mises à jour WSUS sont configurés au cours de la [configurer Windows Server Update Services &#40;WSUS&#41; &#40;système de plateforme Analytique&#41; ](configure-windows-server-update-services-wsus.md) étape effectuée pendant la configuration de l’appliance. Si ce n’est pas le cas, cette étape de configuration peut être effectuée ultérieurement. Pour plus d’informations sur WSUS, consultez le [du site Web WSUS Guide](http://go.microsoft.com/fwlink/?LinkId=202417).  
  
**Les correctifs logiciels :** en outre, vous devrez peut-être appliquer des correctifs de système de plateforme Analytique. A *correctif* est une mise à jour logicielle qui est créé pour un client spécifique résoudre un problème avec le logiciel de système de plateforme Analytique. Chaque correctif logiciel est un fichier exécutable qui installe le correctif pour le problème spécifique au client. Chaque correctif contient également une accumulation de toutes les mises à jour logicielles précédemment publiées pour Windows, SQL Server et système de plateforme d’Analytique. Si vous avez besoin d’installer un correctif logiciel, prise en charge de Microsoft vous fournira les correctifs logiciels et les instructions.  
  
**Étendue des mises à jour :** appliquer un correctif logiciel ou service pack pour le système de plateforme Analytique doit déconnecter l’application entière. Autrement dit, le PDW et HDInsight régions seront affectées.  
  
**Adaptateur de Destination SSIS et outils clients :** lors de l’application d’un correctif logiciel qui inclut des modifications de MSI de l’adaptateur de Destination SSIS ou MSI du Client des outils, les fichiers MSI seront mise à jour dans le **C:\PDWINST\ClientTools** répertoire sur le nœud du contrôle. Le correctif logiciel n’installe pas automatiquement les composants à partir des fichiers MSI mis à jour. Pour mettre à jour ces composants, le client doit désinstaller les anciennes versions des composants et installer les nouvelles versions des fichiers MSI mis à jour. Lors de la désinstallation d’un correctif logiciel qui inclut des modifications de MSI de l’adaptateur de Destination SSIS ou MSI du Client des outils, les fichiers MSI pour ces composants vont être annulée pour les versions précédentes. Pour rétablir les composants à une version antérieure, le client doit désinstaller les versions existantes (plus récentes) des composants et réinstaller les anciennes versions des fichiers MSI restaurées.  
  
## <a name="software-servicing-topics"></a>Rubriques de la maintenance de logiciel  
Les rubriques suivantes décrivent comment gérer des logiciels de maintenance du matériel :  
  
-   [Téléchargez et appliquez les mises à jour Microsoft &#40;Analytique plate-forme système&#41;](download-and-apply-microsoft-updates.md)  
  
-   [Désinstaller les mises à jour Microsoft &#40;Analytique plate-forme système&#41;](uninstall-microsoft-updates.md)  
  
-   [Appliquer des correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](apply-analytics-platform-system-hotfixes.md)  
  
-   [Désinstallation de correctifs de système de plateforme Analytique &#40;Analytique plate-forme système&#41;](uninstall-analytics-platform-system-hotfixes.md)  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
  
