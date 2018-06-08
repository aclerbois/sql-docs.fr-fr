---
title: Se connecter à MySQL (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
ms.assetid: 94099d01-ab19-4d58-a172-340c86b4a0f3
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 2290b67ac66a1fa06d62b88390a538a3e06621ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-mysql-mysqltosql"></a>Se connecter à MySQL (MySQLToSQL)
Utilisez le **se connecter à MySQL** boîte de dialogue se connecter à la base de données MySQL que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le **fichier** menu, sélectionnez **se connecter à MySQL**. Si vous êtes déjà connecté, la commande est **se reconnecter à MySQL**.  
  
## <a name="options"></a>Options  
**Fournisseur**  
  
Fournisseur MySQL disponible est MySQL ODBC 5.1 pilote (approuvé).  
  
**Mode**  
  
Le mode par défaut est Standard. En mode Standard, vous entrez ou sélectionnez les valeurs pour le MySQL, un nom de serveur, un port du serveur, un nom d’utilisateur et un mot de passe.  
  
**Nom du serveur**  
  
Entrez le nom du serveur MySQL. Il s’agit d’une option de mode Standard.  
  
**Port du serveur**  
  
Entrez le port du serveur. Le port du serveur par défaut est 3306. Il s’agit d’une option de mode Standard.  
  
**Nom d'utilisateur**  
  
Entrez le nom d’utilisateur SSMA utilisera pour se connecter à la base de données MySQL.  
  
**Mot de passe**  
  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**SSL**  
  
Si vous souhaitez vous connecter en toute sécurité à MySQL, assurez-vous d’utiliser de Socket Layer (SSL Secure) en vérifiant la **SSL** case à cocher.  
  
**Configurer**  
  
Il fournit une option pour configurer la connexion à MySQL via SSL Secure Socket Layer ().  
  
> [!NOTE]  
> Pour activer **configurer**, SSL doit être défini sur **True**.  
  
En cliquant sur le bouton « Configurer », une boîte de dialogue s’affiche. Pour utiliser le chiffrement lors de la connexion à la base de données MySQL, chemin d’accès aux fichiers de trois certificat suivants présents dans la boîte de dialogue doit être défini [confidentialité améliorée messagerie certificats PEM ()] :  
  
-   **Autorité de certification SSL :** Spécifie le chemin d’accès à un fichier avec une liste de confiance autorités de certification SSL.  
  
-   **Certificat SSL :** Spécifie le nom du fichier de certificat SSL à utiliser pour établir une connexion sécurisée.  
  
-   **CLÉ SSL :** Spécifie le nom du fichier de clé SSL à utiliser pour établir une connexion sécurisée.  
  
> [!NOTE]  
> -   Le **OK** bouton est activé lorsque les informations requises ont été fournies. Si un des chemins de fichier ne sont pas valide, le bouton « OK » reste désactivé.  
> -   Le **Annuler** bouton ferme la boîte de dialogue et **désactive** l’option SSL à partir du formulaire de connexion principal.  
  
