---
title: Utilitaire rskeymgmt (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], encryption
- joining report server instances [SQL Server]
- report server scale-out deployments [Reporting Services]
- cryptography [Reporting Services]
- multiple report server instances
- command prompt utilities [Reporting Services]
- report servers [Reporting Services], scale-out deployments
- encryption [Reporting Services]
- rskeymgmt utility
- scale-out deployments [Reporting Services]
ms.assetid: 53f1318d-bd2d-4c08-b19f-c8b698b5b3d3
caps.latest.revision: 56
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: c2bdcd2610eb4a4c6d351868a8fbb7aac9a44bf7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rskeymgmt-utility-ssrs"></a>Utilitaire rskeymgmt (SSRS)
  Extrait, restaure, crée et supprime la clé symétrique utilisée pour protéger les données sensibles de serveur de rapports contre un accès non autorisé. Cet utilitaire sert également à joindre des instances de serveur de rapports dans un déploiement évolutif. Un *déploiement évolutif de serveurs de rapports* correspond à plusieurs instances de serveur de rapports qui partagent une base de données de serveur de rapports unique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
rskeymgmt {-?}  
{–eextract}  
{–aapply}  
{-ddeleteall}  
{–srecreatekey}  
{–rremoveinstancekey}  
{-jjoinfarm}  
{-iinstance}  
{-ffile}  
{-pencryptionpassword}  
{-mremotecomputer}  
{-ninstancenameofremotecomputer}  
{-uadministratoruseraccount}  
{-vadministratorpassword}  
{-ttrace}  
```  
  
## <a name="arguments"></a>Arguments  
 **-?**  
 Affiche la syntaxe des arguments de **rskeymgmt** .  
  
 **-e**  
 Extrait la clé symétrique utilisée pour chiffrer et déchiffrer des données pour l'instance du serveur de rapports afin que vous puissiez les copier dans un fichier.  
  
 Cet argument ne prend pas de valeur. Cependant, vous devez inclure des arguments supplémentaires sur la ligne de commande pour effectuer l'extraction. Les arguments que vous devez spécifier incluent notamment **-f** et **-p**.  
  
 **-a**  
 Remplace une clé symétrique existante par une copie que vous pouvez fournir dans un fichier de sauvegarde protégé par mot de passe. Toutes les instances de la clé symétrique sont mises à jour.  
  
 Cet argument ne prend pas de valeur. Cependant, vous devez inclure des arguments supplémentaires sur la ligne de commande pour sélectionner le fichier contenant la clé à appliquer. Les arguments que vous pouvez spécifier incluent notamment **-f** et **-p**.  
  
 **-d**  
 Supprime toutes les instances de clé symétrique et toutes les données chiffrées dans une base de données de serveur de rapports. Cet argument ne prend pas de valeur.  
  
 **-s**  
 Génère une nouvelle clé symétrique et rechiffre l'ensemble du contenu chiffré à l'aide de la nouvelle clé. Toutes les instances de la clé symétrique sont régénérées.  
  
 **-j**  
 Configure une instance de serveur de rapports distante de manière à partager la base de données de serveur de rapports qui est utilisée par l'instance de serveur de rapports locale.  
  
 **-r**  *installationID*  
 Supprime les informations de clé symétrique pour une instance de serveur de rapports spécifique, supprimant de ce fait le serveur de rapports d'un déploiement évolutif. *installationID* est une valeur GUID se trouvant dans le fichier RSReportserver.config.  
  
 **-f**  *file*  
 Définit un chemin d'accès complet au fichier qui stocke une copie de sauvegarde des clés symétriques.  
  
 Pour **rskeymgmt -e**, la clé symétrique est écrite dans le fichier que vous spécifiez.  
  
 Pour **rskeymgmt -a**, la valeur de clé symétrique stockée dans le fichier est appliquée à l’instance du serveur de rapports.  
  
 **-p**  *password*  
 (Obligatoire pour **-f**) Spécifie le mot de passe utilisé pour sauvegarder ou appliquer une clé symétrique. Cette valeur ne peut pas être vide.  
  
 **-i**  
 Spécifie une instance de serveur de rapports locale. Cet argument est facultatif si vous avez installé le serveur de rapports sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par défaut (la valeur par défaut de **-i** est MSSQLSERVER). Si vous avez installé le serveur de rapports en tant qu’instance nommée, **-i** est obligatoire.  
  
 **-m**  
 Spécifie le nom de l'ordinateur distant qui héberge l'instance de serveur de rapports que vous joignez au déploiement évolutif de serveurs de rapports. Utilisez le nom de l'ordinateur qui l'identifie sur votre réseau.  
  
 **-n**  
 Spécifie le nom de l'instance de serveur de rapports sur un ordinateur distant. Cet argument est facultatif si vous avez installé le serveur de rapports sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par défaut (la valeur par défaut de **-n** est MSSQLSERVER). Si vous avez installé le serveur de rapports en tant qu’instance nommée, **-n** est obligatoire.  
  
 **-u**  *useraccount*  
 Spécifie le compte d'administrateur sur l'ordinateur distant que vous joignez au déploiement évolutif. Si un compte n'est pas spécifié, les informations d'identification de l'utilisateur actuel sont employées.  
  
 **-v**  *password*  
 (Obligatoire pour **-u**) Spécifie le mot de passe d’un compte d’administrateur sur l’ordinateur distant que vous souhaitez joindre au déploiement évolutif.  
  
 **-t**  *trace*  
 Envoie des messages d'erreur au journal de suivi. Cet argument ne prend pas de valeur. Pour plus d’informations, consultez [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md).  
  
## <a name="permissions"></a>Autorisations  
 Vous devez être un administrateur local pour pouvoir exécuter cet outil et vous devez l'exécuter localement sur l'ordinateur qui héberge le serveur de rapports. L'utilitaire rskeymgmt fonctionne avec l'instance locale de Windows Report Server (l'utilitaire ne peut pas se connecter à des instances distantes du service Windows Report Server, il ne peut donc pas être utilisé pour gérer les clés de chiffrement d'une instance distante de serveur de rapports).  
  
> [!NOTE]  
>  Si vous utilisez les arguments **-u** et **-v** , prenez soin de spécifier un compte avec des autorisations d’administrateur sur l’ordinateur distant.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent diverses manières d’utiliser **rskeymgmt**. Les exemples suivants illustrent l'extraction, la restauration et la suppression de clés de chiffrement, ainsi que la configuration d'un déploiement évolutif de serveurs de rapports.  
  
#### <a name="extracting-encryption-keys"></a>Extraction de clés de chiffrement  
 Cet exemple illustre la création d'une copie de sauvegarde de la clé de chiffrement et son enregistrement sur une disquette dans un fichier protégé par mot de passe. Si le serveur de rapports est installé en tant qu’instance nommée, ajoutez l’argument **-i** .  
  
```  
rskeymgmt -e -f a:\backupkey\keys -p <password>  
```  
  
#### <a name="restoring-encryption-keys"></a>Restauration de clés de chiffrement  
 Cet exemple illustre le remplacement de la clé de chiffrement. Vous devez spécifier l'emplacement de la copie de sauvegarde de la clé et le mot de passe qui déverrouille le fichier.  
  
```  
rskeymgmt -a -f a:\backupkey\keys -p <password>  
```  
  
#### <a name="deleting-encryption-keys-and-encrypted-content"></a>Suppression de clés de chiffrement et contenu chiffré  
 Cet exemple illustre la suppression de toutes les clés de chiffrement stockées dans un serveur de rapports. Si votre installation est un déploiement évolutif de serveur de rapports, les clés de chiffrement de toutes les instances de serveur de rapports qui sont incluses dans le déploiement sont supprimées. La suppression d'une clé de chiffrement supprime également toutes les valeurs chiffrées existantes dans la base de données du serveur de rapports. Pour plus d’informations sur le contenu chiffré, consultez [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md).  
  
```  
rskeymgmt -d  
```  
  
#### <a name="joining-a-remote-report-server-named-instance-to-a-scale-out-deployment"></a>Jointure d'une instance nommée de serveur de rapports distante à un déploiement évolutif  
 Cet exemple illustre l'ajout d'une instance de serveur de rapports qui est installée sur un ordinateur distant dans un déploiement évolutif de serveur de rapports. Vous devez exécuter la commande sur l'un des ordinateurs déjà configurés pour utiliser la base de données partagée. Les arguments de la commande spécifient l'instance du serveur de rapports distante que vous souhaitez joindre au déploiement évolutif.  
  
```  
rskeymgmt -j -m <remotecomputer> -n <namedreportserverinstance> -u <administratoraccount> -v <administratorpassword>  
```  
  
> [!NOTE]  
>  Un déploiement évolutif de serveur de rapports se réfère à un modèle de déploiement où plusieurs instances de serveur de rapports partagent la même base de données de serveur de rapports. Une base de données de serveur de rapports peut être utilisée par n'importe quelle instance de serveur de rapports qui stocke ses clés symétriques dans la base de données. Par exemple, si une base de données de serveurs de rapport contient des informations clés pour trois instances de serveur de rapports, les trois instances sont considérées comme étant membres du même déploiement évolutif.  
  
#### <a name="joining-report-server-instances-on-the-same-computer"></a>Jonction d'instances de serveur de rapports sur un même ordinateur  
 Vous pouvez créer un déploiement évolutif à partir de plusieurs instances de serveur de rapports installées sur un même ordinateur. Ne définissez pas les arguments **-u** et **-v** si vous joignez des instances de serveur de rapports installées localement. Utilisez les arguments **-u** et **-v** uniquement si vous joignez une instance d’un ordinateur distant. Si vous spécifiez ces arguments, l'erreur suivante s'affiche : « Les références utilisateur ne peuvent pas être utilisées pour des connexions locales ».  
  
 L'exemple suivant illustre la syntaxe de création d'un déploiement évolutif à l'aide de plusieurs instances locales. Dans cet exemple, \<**initializedinstance**> est le nom d’une instance qui est déjà initialisée pour utiliser la base de données du serveur de rapports, et \<**newinstance**> est le nom de l’instance à ajouter au déploiement :  
  
```  
rskeymgmt -j -i <initializedinstance> -m <computer name> -n <newinstance>  
```  
  
#### <a name="removing-encryption-keys-for-a-single-report-server-in-a-scale-out-deployment"></a>Suppression de clés de chiffrement pour un serveur de rapports unique dans un déploiement évolutif  
 Cet exemple illustre la suppression des clés de chiffrement d'un serveur de rapports unique dans un déploiement évolutif de serveur de rapports. Les clés sont supprimées de la base de données du serveur de rapports. Une fois que les clés de cette instance de serveur de rapports sont retirées, cette instance de serveur de rapports ne peut plus accéder aux données chiffrées dans la base de données, ce qui la retire en fait du déploiement évolutif.  
  
 La suppression d'une instance de serveur de rapports d'un déploiement avec montée en puissance parallèle requiert la spécification d'un ID d'installation. L'ID d'installation est un GUID stocké dans le fichier RSReportserver.config de l'instance de serveur de rapports pour laquelle vous souhaitez supprimer des clés de chiffrement. Vous devez exécuter la commande suivante sur l'ordinateur à retirer du déploiement évolutif. Si le serveur de rapports est installé en tant qu’instance nommée, ajoutez l’argument **-i** pour spécifier l’instance. Pour plus d’informations, consultez [Fichier de configuration RSReportServer](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
```  
rskeymgmt -r <installationID>  
```  
  
## <a name="file-location"></a>Emplacement du fichier  
 Rskeymgmt.exe se trouve dans **\<*lecteur*>:\Program Files\Microsoft SQL Server\110\Tools\Binn** ou dans **\<*lecteur*>:\Program Files (x86)\Microsoft SQL Server\110\Tools\Binn**. Vous pouvez exécuter l'utilitaire à partir de n'importe quel dossier de votre système de fichiers.  
  
## <a name="remarks"></a>Notes   
 Un serveur de rapports chiffre les informations d'identification et les informations de connexion stockées. Une clé publique et une clé symétrique sont utilisées pour chiffrer les données. La base de données d'un serveur de rapports doit posséder les clés valides pour que le serveur de rapports puisse s'exécuter. Vous pouvez utiliser **rskeymgmt** pour sauvegarder, supprimer ou restaurer des clés. Si les clés ne peuvent pas être restaurées, cet outil permet de supprimer un contenu chiffré qui ne peut plus être utilisé.  
  
 L’utilitaire **rskeymgmt** sert à gérer l’ensemble des clés qui est défini durant l’installation ou l’initialisation. Il se connecte au service Windows Report Server par l'intermédiaire d'un point de terminaison de RPC (Remote Procedure Call). Le service Windows Report Server doit être en cours d'exécution pour permettre le fonctionnement de cet utilitaire.  
  
 Pour plus d’informations sur les clés de chiffrement, consultez [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md) et [Initialiser un serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Déploiement avec montée en puissance parallèle - Mode natif de Reporting Services &#40;Gestionnaire de configuration&#41;](http://msdn.microsoft.com/library/4df38294-6f9d-4b40-9f03-1f01c1f0700c)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Utilitaires d’invite de commandes du serveur de rapports &#40;SSRS&#41;](../../reporting-services/tools/report-server-command-prompt-utilities-ssrs.md)   
 [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
