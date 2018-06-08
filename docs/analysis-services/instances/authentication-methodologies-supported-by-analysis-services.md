---
title: Méthodologies d’authentification pris en charge par Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fe2c4021792c3285165440fc0627aa7eab374882
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="authentication-methodologies-supported-by-analysis-services"></a>Méthodologies d'authentification prises en charge par Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les connexions d'une application cliente à une instance d'Analysis Services nécessitent l'authentification Windows (intégrée). Fournissez une identité d'utilisateur Windows à l'aide de l'une des méthodes suivantes :  
  
-   NTLM  
  
-   Kerberos (consultez [Configurer Analysis Services pour la délégation contrainte Kerberos](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md))  
  
-   EffectiveUserName sur la chaîne de connexion  
  
-   De base ou Anonyme (requiert une configuration de l'accès HTTP)  
  
-   Informations d'identification stockées  
  
 Notez que l’authentification par revendications n’est pas prise en charge. Vous ne pouvez pas utiliser un jeton de revendications vers Windows pour accéder à Analysis Services. Les bibliothèques clientes Analysis Services fonctionnent uniquement selon les principes de sécurité de Windows. Si votre solution de décisionnel inclut des identités de revendications, vous avez besoin de comptes miroirs d'identité Windows pour chaque utilisateur, ou vous devez utiliser des informations d'identification stockées pour accéder aux données Analysis Services.  
  
 Pour plus d’informations sur les flux d’authentification BI et Analysis Services, consultez [Authentification et délégation d’identité Microsoft BI](http://go.microsoft.com/fwlink/?LinkID=286576).  
  
##  <a name="bkmk_auth"></a> Comprendre les solutions de rechange pour l'authentification  
 La connexion à une base de données Analysis Services nécessite une identité de groupe ou d'utilisateur Windows et les autorisations associées. L'identité peut être une connexion destinée à des usages généraux utilisée par quiconque doit consulter un rapport. Cependant le scénario le plus probable comprend l'identité des utilisateurs individuels.  
  
 Généralement, un modèle tabulaire ou multidimensionnel possède différents niveaux d'accès aux données, par objet ou dans les données, selon la personne qui effectue la demande. Pour répondre à cet impératif, utilisez l'authentification NTLM, Kerberos, EffectiveUserName ou l'authentification de base. Toutes ces techniques fournissent une approche permettant de passer différentes identités d'utilisateur avec chaque connexion. Toutefois, la plupart de ces choix sont soumis à la limite d'un seul saut. Seul Kerberos avec la délégation permet de transmettre l'identité de l'utilisateur d'origine pour plusieurs connexions d'ordinateurs à un magasin de données principal sur un serveur distant.  
  
 **NTLM**  
  
 Pour les connexions qui spécifient `SSPI=Negotiate`, NTLM constitue le sous-système d’authentification de secours utilisé quand aucun contrôleur de domaine Kerberos n’est disponible. Sous NTLM, un utilisateur ou une application cliente quelconque a accès à une ressource serveur tant que la demande est une connexion directe d'un client au serveur, la personne qui demande la connexion dispose des autorisations nécessaires dans la ressource et les ordinateurs clients et serveurs se trouvent dans le même domaine.  
  
 Dans les solutions multiniveau, la restriction à un seul saut de NTLM peut être une contrainte. L'identité de l'utilisateur qui effectue la demande peut être empruntée sur un seul serveur distant, mais ne peut pas aller au-delà. Si l'opération actuelle nécessite des services exécutés sur plusieurs ordinateurs, vous devez configurer la délégation contrainte Kerberos de façon à réutiliser le jeton de sécurité sur des serveurs principaux. Utilisez également le stockage des informations d'identification ou l'authentification de base pour transmettre les informations sur la nouvelle identité sur une connexion à simple saut.  
  
 **Authentification Kerberos et délégation contrainte Kerberos**  
  
 L'authentification Kerberos est la base de la sécurité intégrée Windows dans les domaines Active Directory. Tout comme avec NTLM, l'emprunt d'identité sous Kerberos est limité à un seul saut sauf si vous activez la délégation.  
  
 Pour prendre en charge les connexions à plusieurs sauts, Kerberos fournit la délégation contrainte et sans contrainte. Cependant, pour la plupart des scénarios, la délégation contrainte est considérée comme une meilleure pratique de sécurité. La délégation contrainte permet à un service de transmettre le jeton de sécurité de l'identité de l'utilisateur à un service de bas niveau indiqué sur un ordinateur distant. Pour les applications multiniveau, la délégation de l'identité d'un utilisateur d'une application intermédiaire vers une base de données principale telle que Analysis Services est une condition habituelle. Par exemple, un modèle tabulaire ou multidimensionnel qui retourne différentes données en fonction de l'identité de l'utilisateur nécessite la délégation d'identité à partir d'un service intermédiaire afin de ne devoir entrer de nouveau les informations d'identification, ou obtenir les informations d'identification de sécurité d'une autre façon.  
  
 La délégation contrainte nécessite une configuration supplémentaire dans Active Directory, où les services sur les points de terminaison émetteur et récepteur de la demande sont explicitement autorisés pour la délégation. Bien que cela implique des frais de configuration, une fois le service configuré, les mises à jour des mots de passe sont gérées indépendamment dans Active Directory. Vous n'avez pas à mettre à jour les informations de compte stockées dans les applications, comme vous devriez le faire si vous utilisiez l'option de stockage des informations d'identification décrite plus loin.  
  
 Pour plus d’informations sur la configuration d’Analysis Services pour la délégation contrainte, consultez [Configurer Analysis Services pour la délégation contrainte Kerberos](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md).  
  
> [!NOTE]  
>  Windows Server 2012 prend en charge la délégation contrainte dans les domaines. À l'inverse, la configuration de la délégation contrainte Kerberos dans des domaines à des niveaux fonctionnels inférieurs, tels que Windows Server 2008 ou 2008 R2, nécessite que les ordinateurs clients et serveurs soient membres du même domaine.  
  
 **EffectiveUserName**  
  
 EffectiveUserName est une propriété de chaîne de connexion utilisée pour transmettre les informations d'identité à Analysis Services. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint l’utilise pour enregistrer l’activité des utilisateurs dans les journaux d’utilisation. Excel Services et PerformancePoint Services l'utilisent pour récupérer les données utilisées par les classeurs ou les tableaux de bord dans SharePoint. Elle peut également être utilisée dans les applications personnalisées ou les scripts qui effectuent des opérations sur une instance Analysis Services.  
  
 Pour plus d’informations sur l’utilisation d’EffectiveUserName dans SharePoint, consultez [Utiliser Analysis Services EffectiveUserName dans SharePoint Server 2010](http://go.microsoft.com/fwlink/?LinkId=311905).  
  
 **Authentification de base et utilisateur anonyme**  
  
 L'authentification de base fournie une quatrième solution pour la connexion à un serveur principal en tant qu'utilisateur spécifique. Avec l'authentification de base, le nom d'utilisateur et le mot de passe Windows sont transmis sur la chaîne de connexion, ce qui apporte des conditions de chiffrement simultané supplémentaires pour garantir la protection des informations sensibles lors de leur transit. L'un des principaux avantages offerts par l'authentification de base est que la demande d'authentification va au-delà des limites de domaine.  
  
 Pour l'authentification anonyme, vous pouvez définir l'identité d'un utilisateur anonyme dans un compte d'utilisateur Windows spécifique (IUSR_GUEST par défaut) ou une identité du pool d'applications. Le compte d'utilisateur anonyme est utilisé sur la connexion Analysis Services et doit disposer d'autorisations d'accès aux données sur l'instance Analysis Services. Lorsque vous utilisez cette approche, seule l'identité de l'utilisateur associée au compte anonyme est utilisée sur la connexion. Si votre application nécessite la gestion d'identités supplémentaires, choisissez une des autres approches, ou complétez avec une solution de gestion des identités que vous fournissez.  
  
 L'authentification de base ou anonyme est disponible uniquement lorsque vous configurez Analysis Services pour l'accès HTTP, à l'aide d'IIS et du fichier msmdpump.dll pour établir la connexion. Pour plus d’informations, consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).  
  
 **Stored Credentials**  
  
 La plupart des services d'application intermédiaires comprennent une fonctionnalité de stockage du nom d'utilisateur et du mot de passe qui sont ensuite utilisés pour récupérer des données dans un magasin de données de bas niveau, tel qu'Analysis Services ou le moteur relationnel SQL Server. De ce fait, le stockage des informations d'identification offre une cinquième solution pour la récupération des données. Les limitations de cette approche comprennent la surcharge de maintenance associée à la mise à jour des noms d'utilisateur et des mots de passe et l'utilisation d'une seule identité sur la connexion. Si votre solution nécessite l'identité de l'appelant d'origine, le stockage des informations d'identification ne constitue pas une solution viable.  
  
 Pour plus d’informations sur les informations d’identification, consultez [Créer, modifier et supprimer des sources de données partagées &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md) et [Utiliser Excel Services avec le service Banque d’informations sécurisé dans SharePoint Server 2013](http://go.microsoft.com/fwlink/?LinkID=309869).  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d'emprunt d'identité avec sécurité du transport](http://go.microsoft.com/fwlink/?LinkId=311727)   
 [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md)   
 [Configurer Analysis Services pour la délégation contrainte Kerberos](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md)   
 [Inscription SPN pour une instance Analysis Services](../../analysis-services/instances/spn-registration-for-an-analysis-services-instance.md)   
 [Se connecter à Analysis Services](../../analysis-services/instances/connect-to-analysis-services.md)  
  
  
