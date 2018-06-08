---
title: Connectez-vous à DB2 (DB2ToSQL) | Documents Microsoft
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
ms.assetid: 9d485fd0-ab5d-402a-a59a-e9982a61b7de
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 09d4e69aefa89ed9930badc575be4fdc302d5f35
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-db2-db2tosql"></a>Connectez-vous à DB2 (DB2ToSQL)
Utilisez le **se connecter à DB2** boîte de dialogue se connecter à la base de données DB2 que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le **fichier** menu, sélectionnez **se connecter à DB2**. Si vous êtes déjà connecté, la commande est **se reconnecter à DB2**.  
  
## <a name="options"></a>Options  
**Fournisseur**  
Sélectionnez le fournisseur d’accès aux données pour la connexion à la base de données DB2. Les fournisseurs disponibles sont le fournisseur du Client DB2 et du fournisseur OLE DB. La valeur par défaut est le fournisseur du Client DB2.  
  
**Mode**  
Sélectionnez le mode Standard, TNSNAME ou chaîne de connexion.  
  
-   En mode Standard, vous entrez ou sélectionnez des valeurs pour le fournisseur, nom du serveur, port du serveur, DB2 SID, nom d’utilisateur et mot de passe.  
  
-   En mode TNSNAME, vous entrez l’identificateur de connexion (alias TNS) de la base de données DB2, le nom d’utilisateur et le mot de passe.  
  
-   En mode de la chaîne de connexion, vous fournissez une chaîne de connexion.  
  
    > [!IMPORTANT]  
    > Nous vous déconseillons d’utiliser le mode de chaîne de connexion, car le texte peut inclure des mots de passe, et il est envoyé en texte clair.  
  
La valeur par défaut est en mode Standard.  
  
**Nom du serveur**  
Entrez le nom du serveur DB2. Le nom du serveur par défaut est le même que le nom de l’ordinateur. Il s’agit d’une option de mode Standard.  
  
**Port du serveur**  
Si vous utilisez un numéro de port autre que 1521 (par défaut) pour les connexions à DB2, entrez le numéro de port. Il s’agit d’une option de mode Standard.  
  
**Identificateur de connexion**  
Entrez le DB2 identificateur de connexion. Il s’agit de l’alias de la base de données tel que défini dans le fichier tnsnames.ora local.  
  
Il s’agit d’une option de mode TNSNAME.  
  
**DB2 SID**  
Entrez le SID de la base de données. Le SID est un identificateur qui le distingue de la base de données DB2 sur un ordinateur. La valeur par défaut de SID pour une base de données est les huit premiers caractères du nom de base de données.  
  
Il s’agit d’une option de mode Standard.  
  
**Nom d'utilisateur**  
Entrez le nom d’utilisateur SSMA utilisera pour se connecter à la base de données DB2.  
  
**Mot de passe**  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**Chaîne de connexion**  
> [!IMPORTANT]  
> Nous vous déconseillons d’utiliser le mode de chaîne de connexion, car le texte peut inclure des mots de passe, et il est envoyé en texte clair.  
  
Si vous utilisez le mode de la chaîne de connexion, entrez la chaîne de connexion complète pour la connexion à DB2.  
  
Chaînes de connexion sont constitués de paires nom / valeur de paramètre.  
  
-   Pour plus d’informations de chaîne de connexion OLE DB, consultez [fournisseur Microsoft OLE DB pour DB2](http://go.microsoft.com/fwlink/?LinkId=85640) l’article sur le site MSDN Library.  
  
Pour les chaînes de connexion de SSMA, toujours inclure le paramètre de fournisseur. En outre, assurez-vous que vous incluez le paramètre de Port lorsque vous vous connectez à DB2.  
  
