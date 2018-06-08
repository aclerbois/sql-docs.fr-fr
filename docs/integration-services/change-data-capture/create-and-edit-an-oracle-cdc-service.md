---
title: Créer et modifier un service Oracle CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- createSrv
ms.assetid: 10cd612e-d8f1-4af2-97d3-a0c22e1e2326
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9802d1db4b8db647493d917f803c652071a3c983
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-and-edit-an-oracle-cdc-service"></a>Créer et modifier un service de capture de données modifiées Oracle
  Vous créez et modifiez un service Windows de capture de données modifiées Oracle dans la console de configuration du service de capture de données modifiées.  
  
 Pour créer un service Windows de capture de données modifiées Oracle, sélectionnez **Services de capture de données modifiées locaux** et cliquez sur **Nouveau service** dans le volet **Actions** . Vous pouvez également cliquer avec le bouton droit sur **Services de capture de données modifiées locaux** et sélectionner **Nouveau service**. La boîte de dialogue Nouveau service Windows de capture de données modifiées Oracle s'ouvre.  
  
 **OR**  
  
 Pour modifier les propriétés du service de capture de données modifiées, sélectionnez le service pour lequel vous souhaitez modifier les propriétés et cliquez sur **Propriétés** dans le volet **Actions** . Vous pouvez également cliquer avec le bouton droit sur le service que vous utilisez et sélectionner **Propriétés**. La boîte de dialogue de propriétés du service CDC s'ouvre.  
  
 Entrez les informations suivantes dans la boîte de dialogue Nouveau service Windows de capture de données modifiées Oracle ou dans la boîte de dialogue de propriétés du service de capture de données modifiées.  
  
**Nom du service**  
 Entrez le nom du nouveau service Windows de capture de données modifiées Oracle. Vous ne devez pas utiliser des noms longs, si possible. Les caractères / et \ ne peuvent pas être utilisés dans le nom du service.  
  
> [!NOTE]  
> Cette option n'est pas disponible lors de la modification du service. Vous ne pouvez pas modifier le nom d'un service Windows qui existe déjà.  
  
 **Description**  
 Tapez une description du service pour vous aider à l'identifier.  
  
 **Compte de service**  
 Sélectionnez l'une des options suivantes pour déterminer dans quel compte exécuter le service :  
  
-   **Compte système local**  
  
     Ce compte n'est pas recommandé, car il donne trop d'autorisations au service.  
  
-   **Ce compte**  
  
     Sur Windows Vista ou Windows Server 2008, le compte de service par défaut est le compte NETWORK SERVICE.  
  
     Sur Windows 7, Windows Server 2008 R2 et versions ultérieures, le compte de service par défaut est NT Service\\<nom_service>.  
  
     Ces comptes vous permettent de travailler sans utiliser de mots de passe, car aucun mot de passe n'est nécessaire pour ces comptes. En outre, ces comptes fournissent uniquement les autorisations nécessaires requises pour exécuter le service de capture de données modifiées Oracle.  
  
     Vous pouvez utiliser un compte Windows local ou de domaine pour le compte de service. Dans ce cas, vous devez entrer le **Mot de passe** pour ce compte. Ce compte peut correspondre à l'hôte local ou à un compte de domaine. Veillez à mettre à jour le mot de passe lorsqu'il est modifié à l'aide du composant Services locaux dans le Panneau de configuration Windows.  
  
 **Nom du serveur** : sélectionnez l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible à laquelle vous connecter (par exemple, **\\\\<nom_ordinateur>\\<nom_instance>**). La dernière instance de serveur à laquelle une connexion a été établie est affichée par défaut.  
  
 **Authentification**  
 Sélectionnez l'une des options suivantes :  
  
-   **Authentification Windows**: si vous sélectionnez cette option, le service de capture de données modifiées Oracle se connecte à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible à l'aide de l'identité du compte de service. Si l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sur un autre ordinateur, l'authentification Windows doit être utilisée avec les comptes de domaine.  
  
-   **Authentification SQL Server**: si vous sélectionnez cette option, vous devez taper le **Nom d'utilisateur** et le **Mot de passe** pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous souhaitez utiliser. Le service de capture de données modifiées Oracle utilise ces informations d'identification lors de la connexion à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible.  
  
 La connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée par le service de capture de données modifiées Oracle doit être membre du rôle serveur fixe public ; aucun autre privilège n'est nécessaire. Une fois que de nouvelles instances Oracle CDC sont ajoutées, cette connexion a un accès **db_owner** aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associées.  
  
 Pour créer la définition de service Windows de capture de données modifiées Oracle, le programme doit disposer d'un accès de mise à jour à la base de données MSXDBCDC dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associée. Lorsque vous cliquez sur **OK**, une boîte de dialogue vous invite à entrer une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec un accès de mise à jour à la base de données MSXDBCDC.  
  
 Pour plus d'informations sur les données que vous devez taper dans la boîte de dialogue Connexion à SQL Server, consultez [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md).  
  
 **Options**  
 Cliquez sur la flèche pour afficher les options disponibles à configurer. Vous pouvez choisir de conserver ces options avec leur valeur par défaut. Options disponibles :  
  
-   **Délai de connexion**: tapez le délai (en secondes) pendant lequel le service de capture de données modifiées pour Oracle attend une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant expiration. La valeur par défaut est **15**.  
  
-   **Délai d’exécution**: tapez le délai (en secondes) pendant lequel le service Windows de capture de données modifiées Oracle attend l’exécution d’une commande avant expiration. La valeur par défaut est **30**.  
  
-   **Chiffrer la connexion**: sélectionnez **Chiffrer la connexion** pour la communication entre le service de capture de données modifiées Oracle et l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible à l'aide d'une connexion chiffrée.  
  
-   **Avancé**: tapez des propriétés de connexion supplémentaires, si nécessaire.  
  
 **Mot de passe principal**  
 Entrez un mot de passe qui sera utilisé par le service Windows de capture de données modifiées Oracle pour protéger les informations d'identification de l'exploration de données de journaux Oracle.  
  
 Le même mot de passe principal doit également être utilisé lorsque d'autres instances du même service sont configurées sur d'autres nœuds sur un cluster dans une configuration haute disponibilité. Si vous perdez ou modifiez le mot de passe principal, tous les mots de passe d'exploration de données de journaux stockés dans des bases de données d'instance Oracle CDC doivent être entrés de nouveau via la console du concepteur CDC.  
  
## <a name="see-also"></a> Voir aussi  
 [Procédure : créer et modifier un service de capture de données modifiées](../../integration-services/change-data-capture/how-to-create-and-edit-a-cdc-service.md)  
  
  
