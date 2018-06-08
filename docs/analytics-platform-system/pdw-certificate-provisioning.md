---
title: Configuration de certificats PDW - système de plateforme Analytique | Documents Microsoft
description: La page Configuration de certificats PDW du Gestionnaire de Configuration système Analytique plateforme importe ou supprime le certificat utilisé par la région PDW.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: ea52c615f4629b579f5f239513c84d851de9e487
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="pdw-certificate-provisioning---analytics-platform-system"></a>Configuration de certificats PDW - système de plateforme Analytique
Le **fourniture des certificats PDW** page du système de plateforme Analytique **Configuration Manager** importe ou supprime le certificat utilisé par la région PDW. Un certificat pour chiffrer les connexions peut aider à une communication sécurisée au nœud de contrôle via les clients de SQL Server, les outils qui utilisent les pilotes SQL Server PDW, le [Console d’administration](monitor-the-appliance-by-using-the-admin-console.md), et charge les Services d’intégration.  
  
## <a name="prerequisites"></a>Configuration requise  
Avant d’installer le certificat, procédez comme suit :  
  
1.  Obtenir un certificat de sécurité. Si vous avez besoin de plus d’informations sur la façon d’obtenir un certificat de sécurité, contactez le Support Microsoft.  
  
2.  Enregistrez le certificat sur le nœud de contrôle dans un fichier PFX protégé par mot de passe.  
  
## <a name="for-security-reasons-obtain-a-trusted-certificate"></a>Pour des raisons de sécurité, obtenir un certificat approuvé  
SQL Server PDW prend en charge à l’aide d’un certificat pour chiffrer les connexions au nœud de contrôle ; y compris les connexions à la **Console d’administration**.  
  
Par défaut, le **Console d’administration** inclut un certificat auto-signé qui fournit la confidentialité, mais pas l’authentification du serveur. Cela peut exposer les communications vulnérable à une attaque de man-in-the-middle. Lorsqu’un utilisateur se connecte à la Console d’administration en utilisant le certificat auto-signé, Internet Explorer renvoie l’erreur : « Il existe un problème avec le certificat de sécurité de ce site Web ».  
  
Bien que la connexion via le certificat auto-signé chiffre les données en vol entre le client et le serveur, la connexion est toujours exposé à des personnes malveillantes.  
  
> [!WARNING]  
> Les administrateurs de matériel doivent immédiatement acquérir un certificat lié à une autorité de certification de confiance reconnue par les clients, afin de disposer d’une connexion sécurisée et de supprimer l’erreur qui signale d’Internet Explorer.  
  
Le chemin d’accès de certification doit contenir le nom de domaine complet qui est mappé au nœud de contrôle adresse IP du Cluster (recommandé) ou le nom que les utilisateurs tapent dans la barre d’adresse de leur navigateur pour accéder à la **Console d’administration**.  
  
Utiliser le système de plateforme Analytique**Configuration Manager** pour ajouter ou supprimer le certificat de confiance. Directement à l’aide de l’outil de Configuration de Microsoft Windows HTTP Services de certificat (**winHttpCertCfg.exe**) pour gérer le certificat non pris en charge.  
  
## <a name="import-or-remove-the-certificate"></a>Importer ou de supprimer le certificat  
Les instructions suivantes indiquent comment importer ou de supprimer le certificat de l’appliance.  
  
### <a name="to-import-the-certificate"></a>Pour importer le certificat  
  
1.  Lancer le **Configuration Manager**. Pour plus d’informations, consultez [lancer le Gestionnaire de Configuration &#40;système de plateforme Analytique&#41;](launch-the-configuration-manager.md).  
  
2.  Dans le volet gauche de la **Configuration Manager**, développez **topologie de l’entrepôt de données parallèle**, puis cliquez sur **certificats**.  
  
3.  Sélectionnez **importer un certificat et le configurer pour utiliser**, puis cliquez sur **Parcourir** pour rechercher et sélectionner le fichier de certificat.  
  
4.  Entrez le mot de passe pour le certificat dans le **mot de passe** champ.  
  
5.  Cliquez sur **appliquer** pour configurer le certificat de l’appareil.  
  
SQL Server PDW ne chiffre pas la connexion actuelle à l’aide du certificat importé, mais utilise le certificat pour les nouvelles connexions.  
  
### <a name="to-remove-the-previously-imported-certificate"></a>Pour supprimer le certificat précédemment importé  
  
1.  Lancer le **Configuration Manager**. Pour plus d’informations, consultez [lancer le Gestionnaire de Configuration &#40;système de plateforme Analytique&#41;](launch-the-configuration-manager.md).  
  
2.  Dans le volet gauche de la **Configuration Manager**, développez **topologie de l’entrepôt de données parallèle**, puis cliquez sur **certificats**.  
  
3.  Sélectionnez **supprimer tout certificat configuré dans l’appliance**.  
  
4.  Cliquez sur **appliquer** pour supprimer le certificat précédemment importé à partir de l’application.  
  
SQL Server PDW continuera à chiffrer les connexions en cours, mais n’utilise pas la suppression du certificat pour les nouvelles connexions.  
  
![Certificat PDW des appliances DWConfig](./media/pdw-certificate-provisioning/SQL_Server_PDW_DWConfig_ApplPDWCert.png "SQL_Server_PDW_DWConfig_ApplPDWCert")  
  
## <a name="see-also"></a>Voir aussi  
[Lancez le Gestionnaire de Configuration &#40;Analytique plate-forme système&#41;](launch-the-configuration-manager.md)  
<!-- MISSING LINKS [HDInsight Certificate Provisioning &#40;Analytics Platform System&#41;](hdinsight-certificate-provisioning.md)  -->  
  
