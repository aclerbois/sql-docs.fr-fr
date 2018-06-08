---
title: Configurer Parallel Data Warehouse pour les copies de la table distante | Documents Microsoft
description: Décrit comment configurer Parallel Data Warehouse pour utiliser la fonctionnalité de copie de table distante pour copier des tables aux bases de données SMP SQL Server sur des serveurs non-appliance.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 3f71a0c67639918820bca8f6f8f38b9f354154f3
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-parallel-data-warehouse-for-remote-table-copies"></a>Configurer Parallel Data Warehouse pour les copies de la table distante
Décrit comment configurer SQL Server PDW pour utiliser la fonctionnalité de copie de table distante pour copier des tables aux bases de données SMP SQL Server sur des serveurs non-appliance.  
  
Cette rubrique décrit l’une des étapes de configuration pour la configuration de copie de la table distante. Pour obtenir la liste de toutes les étapes de configuration, consultez [copie distante de la Table](remote-table-copy.md).  
  
## <a name="before-you-begin"></a>Avant de commencer  
Pour configurer SQL Server PDW pour utiliser la copie de la table distante, vous devez :  
  
-   Disposer d’un compte d’administrateur système de plateforme Analytique avec la possibilité de se connecter directement à la ***appliance_domain *-AD01** et ***appliance_domain *-AD02** nœuds.  
  
-   Connaître le nom d’hôte ou nom IP du serveur de destination.  
  
## <a name="HowToPDW"></a>Configurer SQL Server PDW pour la copie de la Table distante : mettre à jour les noms d’hôte dans DNS  
Le **CREATE REMOTE TABLE** instruction, utilisée pour les copies de la table distante, spécifie le serveur de destination à l’aide de l’adresse IP ou le nom de l’IP du système SMP Windows. Pour utiliser le nom IP, vous devez ajouter des entrées pour une résolution correcte des noms pour le serveur DNS.  
  
Les étapes suivantes expliquent comment mettre à jour le serveur DNS.  
  
1.  Ouvrez une session sur le nœud actif AD (normalement ***appliance_domain *-AD01**).  
  
2.  Ouvrez le Gestionnaire DNS. Celui-ci se trouve sous **outils d’administration** dans les **Démarrer** menu.  
  
3.  Utilisez le Gestionnaire DNS pour ajouter le nom IP.  
  
## <a name="see-also"></a>Voir aussi  
<!-- MISSING LINKS 
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
[Un redirecteur DNS permet de résoudre les noms DNS de l’autre que l’Appliance](use-a-dns-forwarder-to-resolve-non-appliance-dns-names.md)  
<!-- MISSING LINKS 
[Security - Configure Domain Trusts &#40;SQL Server PDW&#41;](../sqlpdw/security-configure-domain-trusts-sql-server-pdw.md)  
-->
  
