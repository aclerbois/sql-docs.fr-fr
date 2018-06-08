---
title: Activer les connexions chiffrées dans le moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
ms.assetid: e1e55519-97ec-4404-81ef-881da3b42006
caps.latest.revision: 48
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 761913c563b27ea0447b6b104d2d05b441aa70b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-encrypted-connections-to-the-database-engine"></a>Activer les connexions chiffrées dans le moteur de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment activer les connexions chiffrées d’une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en spécifiant un certificat pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l’aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'ordinateur serveur doit être accompagné (approvisionné) d'un certificat et vous devez configurer l'ordinateur client pour permettre l'approbation de l'autorité racine du certificat. L'approvisionnement désigne le processus d'installation d'un certificat par son importation dans Windows.  
  
 Le certificat doit être émis pour l' **authentification serveur**. Le nom du certificat doit être le nom de domaine complet de l'ordinateur.  
  
 Les certificats sont stockés localement pour les utilisateurs de l'ordinateur. Pour installer un certificat afin que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’utilise, vous devez exécuter le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec un compte disposant des privilèges d’administrateur local.
 
  
 Le client doit être en mesure de vérifier la propriété du certificat employé par le serveur. Si le client dispose du certificat de clé publique de l'autorité de certification qui a signé le certificat de serveur, aucune configuration supplémentaire n'est requise. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows comprend les certificats de clé publique d'un grand nombre d'autorités de certification. Si le certificat de serveur a été signé par une autorité de certification publique ou privée pour laquelle le client ne dispose pas de certificat de clé publique, vous devez installer le certificat de clé publique de l'autorité de certification ayant signé le certificat de serveur.  
  
> [!NOTE]  
>  Pour utiliser le chiffrement à l'aide d'un cluster de basculement, installez le certificat du serveur avec le nom DNS complet du serveur virtuel sur tous les nœuds du cluster de basculement. Si, par exemple, vous disposez d’un cluster à deux nœuds nommés test1.*\<votre_société>*.com et test2.*\<votre_société>*.com, ainsi que d’un serveur virtuel nommé virtsql, vous devez installer un certificat pour virtsql.*\<votre_société>*.com sur les deux nœuds. Vous pouvez affecter la valeur **ForceEncryption**(Oui) à l’option **ForceEncryption**.  

> [!NOTE]
> Si vous créez des connexions chiffrées entre un indexeur de la Recherche Azure et SQL Server sur une machine virtuelle Azure, consultez [Configurer une connexion à partir d’un indexeur de la Recherche Azure à SQL Server sur une machine virtuelle Azure](https://azure.microsoft.com/documentation/articles/search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers/). 
  
 
##  <a name="Provision"></a> Pour installer un certificat sur le serveur  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**puis, dans la zone **Ouvrir** , tapez **MMC** et cliquez sur **OK**.  
  
2.  Dans la console MMC, dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter/Supprimer un composant logiciel enfichable** , cliquez sur **Ajouter**.  
  
4.  Dans la boîte de dialogue **Ajout d’un composant logiciel enfichable autonome** , cliquez sur **Certificats**, puis sur **Ajouter**.  
  
5.  Dans la boîte de dialogue **Composant logiciel enfichable Certificats** , cliquez sur **Compte d’ordinateur**, puis sur **Terminer**.  
  
6.  Dans la boîte de dialogue **Ajout d’un composant logiciel enfichable autonome** , cliquez sur **Fermer**.  
  
7.  Dans la boîte de dialogue **Ajouter/Supprimer un composant logiciel enfichable** , cliquez sur **OK**.  
  
8.  Dans le composant logiciel enfichable **Certificats** , développez **Certificats**, **Personnel**, cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les tâches**, puis cliquez sur **Importer**.  

9. Cliquez avec le bouton droit sur le certificat importé, pointez sur **Toutes les tâches**, puis cliquez sur **Gérer les clés privées**. Dans la boîte de dialogue **Sécurité** , ajoutez l’autorisation de lecture pour le compte d’utilisateur utilisé par le compte de service SQL Server.  
  
10. Remplissez les informations dans l' **Assistant Importation de certificat**afin d'ajouter un certificat à l'ordinateur, puis fermez la console MMC. Pour plus d'informations sur l'ajout d'un certificat à un ordinateur, consultez la documentation Windows.  
  
##  <a name="Export"></a> Pour exporter le certificat de serveur  
  
1.  Dans le composant logiciel enfichable **Certificats** , recherchez le certificat dans le dossier **Certificats** / **Personnel** , cliquez avec le bouton droit sur **Certificat**, pointez sur **Toutes les tâches**, puis cliquez sur **Exporter**.  
  
2.  Suivez les étapes de l' **Assistant d'exportation de certificats**en stockant le fichier de certificat à un emplacement approprié.  
  
##  <a name="ConfigureServerConnections"></a> Pour configurer le serveur afin qu’il force les connexions chiffrées  
  
1.  Dans le **Gestionnaire de configuration SQL Server**, développez **Configuration du réseau SQL Server**, cliquez avec le bouton droit sur **Protocoles pour** *\<instance de serveur>*, puis sélectionnez **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés de** **Protocoles pour** *\<nom_instance>*, sous l’onglet **Certificat**, sélectionnez le certificat voulu dans la liste déroulante de la zone **Certificat**, puis cliquez sur **OK**.  
  
3.  Sur l'onglet **Indicateurs** , dans la zone **ForceEncryption** , sélectionnez **Oui**, puis cliquez sur **OK** pour fermer la boîte de dialogue.  
  
4.  Redémarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  


> [!NOTE]
> Pour garantir une connectivité sécurisée entre le client et le serveur, configurez le client pour qu’il demande des connexions chiffrées. Plus de détails figurent [plus loin dans cet article](#client-request-encrypt-connect-23h).



### <a name="wildcard-certificates"></a>Certificats génériques  
À partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prennent en charge les certificats génériques. Les autres clients ne prennent pas toujours en charge les certificats génériques. Pour plus d’informations, consultez la documentation du client. Vous ne pouvez pas sélectionner un certificat générique à l’aide du Gestionnaire de configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour utiliser un certificat générique, modifiez le Registre de clé `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQLServer\SuperSocketNetLib`, puis entrez l’empreinte numérique du certificat, sans espaces, comme valeur **Certificat**.  
> [!WARNING]  
> [!INCLUDE[ssnoteregistry_md](../../includes/ssnoteregistry_md.md)]  

<a name="client-request-encrypt-connect-23h"/>
##  <a name="ConfigureClientConnections"></a> Pour configurer le client afin qu'il demande l'établissement de connexions chiffrées  
  
1.  Copiez le certificat d'origine ou le fichier de certificat exporté sur l'ordinateur client.  
  
2.  Sur l’ordinateur client, utilisez le composant logiciel enfichable **Certificats** pour installer le certificat racine ou le fichier de certificat exporté.  
  
3.  Dans le volet de la console, cliquez avec le bouton droit sur **Configuration de SQL Server Native Client**, puis cliquez sur **Propriétés**.  
  
4.  Sur la page **Indicateurs** , dans la zone **Forcer le chiffrement du protocole** , cliquez sur **Oui**.  
  
##  <a name="EncryptConnection"></a> Pour chiffrer une connexion dans SQL Server Management Studio  
  
1.  Dans la barre d'outils de l'Explorateur d'objets, cliquez sur **Connecter**, puis sur **Moteur de base de données**.  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** , fournissez les informations de connexion, puis cliquez sur **Options**.  
  
3.  Sous l'onglet **Propriétés de connexion** , cliquez sur **Chiffrer la connexion**.  
  
## <a name="see-also"></a> Voir aussi

[Prise en charge de TLS 1.2 pour Microsoft SQL Server](https://support.microsoft.com/kb/3135244)  

