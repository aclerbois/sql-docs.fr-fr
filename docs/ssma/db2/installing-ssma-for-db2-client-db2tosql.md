---
title: L’installation de SSMA pour DB2 Client (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 3ae2a470-6afd-4512-b6d1-fcbe6afe88ad
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d976466251d49f85074864ca39a034a988cbff4f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-ssma-for-db2-client-db2tosql"></a>L’installation de SSMA pour DB2 Client (DB2ToSQL)
Le client SSMA comprend les fichiers de programme qui effectuent les tâches suivantes :  
  
-   Se connecter à une base de données DB2.  
  
-   Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   Convertir les objets de base de données DB2 à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe.  
  
-   Chargez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   Migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Cette rubrique fournit les conditions préalables d’installation et les instructions pour l’installation de SSMA.  
  
## <a name="prerequisites"></a>Configuration requise  
SSMA est conçu pour fonctionner avec DB2 sur z/OS version 9.0 et 10.0 ou DB2 sur LUW version 9,8 et 10.1 ou versions ultérieures et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014.  
  
Avant d’installer SSMA, assurez-vous que l’ordinateur répond aux exigences suivantes :  
  
-   Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 ou une version ultérieure.  
  
-   Le [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.0 ou version ultérieure. Le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.0 est disponible sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] support du produit. Vous pouvez également obtenir de le [.NET Framework Developer Center](http://go.microsoft.com/fwlink/?LinkId=48882).  
  
-   Fournisseur Microsoft OLEDB pour DB2 version 5 ou une version ultérieure et la connectivité aux bases de données DB2 que vous souhaitez migrer.  
  
-   Accès à et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure où vous migrerez des objets de base de données et les données. Pour plus d’informations, consultez [connexion à SQL Server &#40;DB2eToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2etosql.md).  
  
-   4 Go de RAM est recommandé.  
  
## <a name="microsoft-oledb-provider-for-db2"></a>Fournisseur Microsoft OLEDB pour DB2  
Pour télécharger le fournisseur OLE DB pour DB2 version 5.0, accédez à [Microsoft® SQL Server® 2014 Feature Pack](http://www.microsoft.com/download/details.aspx?id=42295).  
  
SSMA est téléchargeable sur le Web. Pour télécharger la version la plus récente, consultez le [page de téléchargement de l’Assistant Migration SQL Server](http://aka.ms/ssmafordb2).  
  
Après avoir téléchargé la dernière version, vous devez extraire les fichiers d’installation avant de pouvoir installer SSMA.  
  
**Pour installer le client SSMA**  
  
1.  Double-cliquez sur SSMA pour DB2 *n*. Install.exe, où *n* est le numéro de build.  
  
2.  Dans la page d’accueil, cliquez sur **suivant**.  
  
    Si vous n’avez pas les composants requis installés, un message s’affiche indiquant que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis et puis exécutez à nouveau le programme d’installation.  
  
3.  Lisez le contrat de licence utilisateur final. Si vous acceptez, cochez **J’accepte les termes du contrat de licence**, puis cliquez sur **suivant**.  
  
4.  Dans la page Choisir un Type d’installation, cliquez sur **standard**.  
  
5.  Cliquez sur **Installer**.  
  
> [!IMPORTANT]  
> 1.  Désinstallez toutes les versions précédentes de SSMA pour DB2 avant d’installer la nouvelle version.  
  
L’emplacement d’installation par défaut est C:\Program Files\Microsoft SQL Server Migration Assistant pour DB2.  
  
## <a name="see-also"></a>Voir aussi  
[Installation des composants SSMA sur SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)  
[Bases de données DB2 migration vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
