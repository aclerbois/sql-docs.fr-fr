---
title: Propriétés de Protocoles pour MSSQLSERVER (onglet Avancé) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: abd5ca68-825f-4c07-b27c-3b3a79d03d74
caps.latest.revision: 7
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 67d9b33fd5326a3f0195acf0987215d6689d1fb3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="protocols-for-mssqlserver-properties-advanced-tab"></a>Propriétés de Protocoles pour MSSQLSERVER (onglet Avancé)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  Utilisez l'onglet **Avancé** dans la boîte de dialogue **Propriétés de Protocoles pour MSSQLSERVER** pour configurer la **protection étendue de l'authentification** du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La**Protection étendue** est une fonctionnalité des composants réseau implémentée par le système d'exploitation. La**protection étendue** est disponible dans Windows 7 et Windows Server 2008 R2 et est incluse dans les Service Packs pour les précédents systèmes d'exploitation. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est plus sécurisé lorsque les connexions sont établies à l'aide de la **protection étendue**. Certains avantages de la **Protection étendue** requièrent de sélectionner l'option **Forcer le chiffrement** sous l'onglet **Indicateurs** .  
  
> [!IMPORTANT]  
>  Windows n'active pas la **protection étendue** par défaut. Pour plus d’informations sur l’activation de la **protection étendue** dans Windows, consultez l’article [Protection étendue de l’authentification](http://go.microsoft.com/fwlink/?LinkId=178431)de la Base de connaissances.  
  
 Pour plus d’informations sur la configuration d’autres services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et pour obtenir une description complète de la **protection étendue**, consultez les informations plus récentes sur [Microsoft.com](http://go.microsoft.com/fwlink/?LinkId=177752).  
  
 La**protection étendue** est totalement prise en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client à partir de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. La prise en charge de la **protection étendue** pour d'autres fournisseurs du client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est actuellement pas prise en charge.  
  
## <a name="options"></a>Options  
 **Protection étendue**  
 Il existe trois valeurs possibles :  
  
-   Lorsque la valeur est **Off**, la **protection étendue** est désactivée. L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptera les connexions de tous les clients, qu'ils soient protégés ou non. La valeur**Off** est compatible avec les anciens systèmes d'exploitation non corrigés, mais elle est moins sécurisée. Utilisez ce paramètre uniquement lorsque vous savez que les systèmes d'exploitation clients ne prennent pas en charge la Protection étendue.  
  
-   Lorsque la valeur est **Autorisée**, la **protection étendue** est requise pour les connexions à partir des systèmes d'exploitation qui prennent en charge la **protection étendue**. Les connexions provenant d'applications clientes non protégées qui s'exécutent sur des systèmes d'exploitation clients protégés sont rejetées. La**Protection étendue** est ignorée pour les connexions provenant de systèmes d'exploitation non protégés. Ce paramètre offre une meilleure protection que la valeur **Off**, mais ce n'est pas la configuration la plus sécurisée. Utilisez ce paramètre dans des environnements mixtes, dans lesquels certains systèmes d'exploitation ou applications prennent en charge la **protection étendue** et d'autres non.  
  
-   Lorsque la valeur est **Requis**, seules les connexions provenant d'applications protégées sur des systèmes d'exploitation protégés sont acceptées. Ce paramètre est le plus sécurisé des trois options mais les connexions provenant de systèmes d'exploitation qui ne prennent pas en charge la **Protection étendue** ne pourront pas se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **SPN NTLM acceptés**  
 Lorsque l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est identifiée par plusieurs noms principaux de service (SPN) NTLM, répertoriez-les ici sous la forme d'une série de chaînes séparées par des points-virgules. Par exemple, la valeur **MSSQLSvc/NomHôte1.Contoso.com;MSSQLSvc/NomHôte2.Contoso.com**indique que les clients qui essaient de se connecter aux noms principaux de service (SPN) **MSSQLSvc/HÔTE1.Contoso.com** et **MSSQLSvc/HÔTE2.Contoso.com** sont autorisés. La variable a une longueur maximale de 2048 caractères.  
  
## <a name="see-also"></a> Voir aussi  
 [Protection étendue de l'authentification avec Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)  
  
  
