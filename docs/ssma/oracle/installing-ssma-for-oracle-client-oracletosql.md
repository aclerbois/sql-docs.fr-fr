---
title: L’installation de SSMA pour le Client Oracle (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Licensing SSMA
ms.assetid: d5d4903d-e296-4bbf-8780-63674c4d62d5
caps.latest.revision: 19
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 5b778ea75d6594b8b7ee880a00d4641f6efe5d5e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-ssma-for-oracle-client-oracletosql"></a>L’installation de SSMA pour le Client Oracle (OracleToSQL)
Le client SSMA comprend les fichiers de programme qui effectuent les tâches suivantes :  
  
-   Connectez-vous à une base de données Oracle.  
  
-   Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   Convertir les objets de base de données Oracle à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] syntaxe.  
  
-   Chargez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
-   Migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Cette rubrique fournit les conditions préalables d’installation et les instructions pour l’installation de SSMA.  
  
## <a name="prerequisites"></a>Configuration requise  
SSMA est conçu pour fonctionner avec Oracle 9 ou versions ultérieures et toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
Avant d’installer SSMA, assurez-vous que l’ordinateur répond aux exigences suivantes :  
  
-   Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 ou une version ultérieure.  
  
-   Le [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.0 ou version ultérieure. Le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.0 est disponible sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] support du produit. Vous pouvez également obtenir de le [.NET Framework Developer Center](http://go.microsoft.com/fwlink/?LinkId=48882).  
  
-   Le Client Oracle 9.0 ou une version ultérieure et la connectivité aux bases de données Oracle que vous souhaitez migrer. La version du client Oracle doit être la même version que, ou une version ultérieure à la version de base de données Oracle.  
  
    Vous pouvez installer le Client Oracle à partir du support de produit Oracle ou à partir du site Web d’Oracle. Pour plus d’informations sur la connectivité, consultez [la connexion à la base de données Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md).  
  
-   Accès à et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure où vous migrerez des objets de base de données et les données. Pour plus d’informations, consultez [connexion à SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).  
  
-   4 Go de RAM est recommandé.  
  
## <a name="installing-the-ssma-for-oracle-client"></a>L’installation de SSMA pour Client Oracle  
SSMA est téléchargeable sur le Web. Pour télécharger la version la plus récente, consultez le [page de téléchargement de l’Assistant Migration SQL Server](http://aka.ms/ssmafororacle).  
  
Après avoir téléchargé la dernière version, vous devez extraire les fichiers d’installation avant de pouvoir installer SSMA.  
  
**Pour installer le client SSMA**  
  
1.  Double-cliquez sur SSMA pour Oracle *n*. Install.exe, où *n* est le numéro de build.  
  
2.  Dans la page d’accueil, cliquez sur **suivant**.  
  
    Si vous n’avez pas les composants requis installés, un message s’affiche indiquant que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis et puis exécutez à nouveau le programme d’installation.  
  
3.  Lisez le contrat de licence utilisateur final. Si vous acceptez, cochez **J’accepte les termes du contrat de licence**, puis cliquez sur **suivant**.  
  
4.  Dans la page Choisir un Type d’installation, cliquez sur **standard**.  
  
5.  Cliquez sur **Installer**.  
  
> [!IMPORTANT]  
> 1.  Désinstallez toutes les versions précédentes de SSMA pour Oracle avant d’installer la nouvelle version.  
  
L’emplacement d’installation par défaut est C:\Program Files\Microsoft SQL Server Migration Assistant pour Oracle.  
  
Outre les fichiers de programme SSMA, vous devez également installer SSMA pour le pack d’extension Oracle sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations, consultez [l’installation des composants de SSMA sur SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Installation des composants SSMA sur SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)  
[Bases de données de migration d’Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
