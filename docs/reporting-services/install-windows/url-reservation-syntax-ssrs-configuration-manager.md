---
title: Syntaxe de réservation d’URL (Gestionnaire de configuration de SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/18/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- URL reservations
ms.assetid: 30e4be2e-e65d-462c-895a-5a0a636d042f
caps.latest.revision: 7
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 969c505870bf0354f9b183d940643c641453946f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="url-reservation-syntax--ssrs-configuration-manager"></a>Syntaxe de réservation d’URL (Gestionnaire de configuration de SSRS)
  Cette rubrique décrit les parties de la chaîne d'URL du service Web Report Server et du Gestionnaire de rapports. La chaîne d'URL stockée en interne possède une structure différente de celle d'une URL saisie dans la barre d'adresses d'une fenêtre de navigateur. La chaîne de réservation d'URL apparaît dans la fenêtre Résultats de l'outil de configuration de Reporting Services lorsque vous configurez une URL et dans le fichier RSReportServer.config. Il peut être utile de savoir comment la chaîne d'URL est définie si vous dépannez les problèmes de réservation d'URL ou interrogez HTTP.SYS pour afficher les réservations d'URL internes définies sur votre serveur.  
  
## <a name="url-syntax"></a>Syntaxe d'URL  
 Une URL de serveur de rapports est stockée dans l’élément **UrlString** et l’élément **VirtualDirectory** . La raison de la séparation **d’UrlString** et de **VirtualDirectory** en éléments distincts est que vous pouvez avoir plusieurs chaînes d’URL, mais un seul nom de répertoire virtuel pour chaque application [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Dans HTTP.SYS, la réservation d’URL inclut les deux éléments **UrlString** et **VirtualDirectory**. La syntaxe d'une réservation d'URL comporte les parties suivantes :  
  
 \<schéma>://\<nom_hôte>:\<port>/\<répertoire_virtuel>  
  
 Le tableau suivant décrit chaque propriété et les valeurs valides pour chacune d'elles.  
  
|Propriété|Valeurs valides|Description|  
|--------------|------------------|-----------------|  
|Scheme|http ou https|Préfixes pour connexions SSL et connexions autres que SSL.|  
|HostName|Caractère générique (+) fort, égal à la valeur **(Assigné)** pour l’adresse IP.<br /><br /> Caractère générique (\*) faible, égal à la valeur **(Non assigné)** de l’adresse IP.<br /><br /> Nom de domaine complet<br /><br /> Nom de l'ordinateur<br /><br /> Adresse IP (IPV4)<br /><br /> Adresse IP (IPV6)|Identifie le serveur sur le réseau.<br /><br /> Le caractère générique fort (+) est la valeur par défaut. HTTP.SYS accepte toutes les demandes sur toutes les cartes réseau pour la combinaison d'un port donné et d'un répertoire virtuel. Le serveur de rapports accepte toute demande sur le port.<br /><br /> Caractère générique (\*). HTTP.SYS accepte toutes les demandes non gérées par les autres réservations d'URL sur toutes les cartes réseau pour la combinaison d'un port donné et d'un répertoire virtuel.<br /><br /> Le nom de l'ordinateur est le nom NETBIOS de l'ordinateur sur le réseau.<br /><br /> Le nom de domaine complet inclut l'adresse du domaine et le nom du serveur, tels qu'ils sont inscrits auprès d'un contrôleur de domaine ou d'un serveur de noms de domaine public.<br /><br /> L’adresse IP (IPV4) est l’adresse IP d’une carte réseau sur l’ordinateur au format IPV4 : *nnn.nnn.nnn.nnn*.<br /><br /> L’adresse IP (IPV6) est l’adresse IP d’une carte réseau sur l’ordinateur au format IPV6 : \<en-tête>:\<en-tête>:*nnn.nnn.nnn.nnn*.|  
|d’|80<br /><br /> 443<br /><br /> \<personnalisé>|Le port 80 est le port standard pour les requêtes HTTP adressées depuis ou vers un serveur.<br /><br /> Le port 443 est le port standard pour les connexions SSL.<br /><br /> Vous pouvez utiliser n'importe quel port qui n'est pas déjà réservé par une autre application.|  
|VirtualDirectory|ReportServer *[_InstanceName]*<br /><br /> Reports *[_InstanceName]*<br /><br /> \<personnalisé>|Spécifie le nom de l'application. La valeur est une chaîne. Par défaut, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise ReportServer et Reports comme noms d'applications pour les applications service Web Report Server et Gestionnaire de rapports. Vous pouvez utiliser d'autres noms, si vous le souhaitez.<br /><br /> Cette valeur est requise. Elle identifie la publication.<br /><br /> Spécifiez un seul répertoire virtuel pour chaque instance d'application. Pour créer plusieurs URL pour la même application dans la même instance, créez plusieurs versions de l’élément **UrlString**. Pour créer des noms de répertoire virtuel uniques pour plusieurs instances d'application, pensez à ajouter le nom de l'instance dans le nom de répertoire virtuel à l'aide du trait de soulignement (_). La propriété *InstanceName* est facultative, mais recommandée si vous avez plusieurs instances sur le même ordinateur. Pour plus d’informations sur la définition des réservations d’URL pour les instances nommées, consultez [Réservations d’URL pour les déploiements de serveur de rapports multi-instance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/url-reservations-for-multi-instance-report-server-deployments.md).<br /><br /> La valeur du répertoire virtuel ne respecte pas la casse. Vous pouvez utiliser n'importe quelle chaîne tant qu'elle n'inclut pas les caractères de séparation d'URL ou l'encodage d'URL.|  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer des URL de serveurs de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)   
 [Configurer une URL &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)  
  
  
