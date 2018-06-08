---
title: Prise en charge d’Azure Active Directory dans SQL Server Data Tools (SSDT) | Microsoft Docs
ms.custom: ''
ms.date: 04/09/2018
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: ssdt
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 906bd42a1a4143217a974dd114adf82fa41e7270
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-active-directory-support-in-sql-server-data-tools-ssdt"></a>Prise en charge d’Azure Active Directory dans SQL Server Data Tools (SSDT)

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md.md](../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

SQL Server Data Tools (SSDT) fournit plusieurs méthodes d’authentification [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis).

![Boîte de dialogue de connexion à SSDT](media/azure-active-directory/interactive.png)

## <a name="active-directory-password-authentication"></a>Authentification par mot de passe Active Directory

L’authentification par mot de passe Active Directory est un mécanisme de connexion à Azure SQL Database à l’aide d’identités dans Azure Active Directory (Azure AD).  Utilisez cette méthode pour vous connecter si vous êtes connecté à Windows avec les informations d’identification d’un domaine qui n’est pas fédéré avec Azure, ou si vous utilisez l’authentification Azure AD avec Azure AD basé sur le domaine client ou initial. Pour plus d’informations, voir [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).  

## <a name="active-directory-integrated-authentication"></a>Authentification intégrée à Active Directory

L’authentification intégrée à Active Directory est un mécanisme de connexion à Azure SQL Database à l’aide d’identités dans Azure Active Directory (Azure AD). Utilisez cette méthode pour vous connecter si vous êtes connecté à Windows avec vos informations d’identification Azure Active Directory à partir d’un domaine fédéré. Pour plus d’informations, voir [Connexion à la base de données SQL à l’aide de l’authentification Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="active-directory-interactive-authentication"></a>Authentification interactive Active Directory

SSDT fournit une nouvelle méthode d’authentification pour la connexion à une base de données SQL Azure : **l’authentification interactive Active Directory**.


> [!NOTE]
> L’authentification interactive Active Directory est disponible lors de la connexion avec SSDT dans [Visual Studio 2017 version 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) et nécessite [le téléchargement et l’installation de .NET Framework 4.7.2](https://www.microsoft.com/net/download/all) sur l’ordinateur qui exécute SSDT. Si [.NET Framework 4.7.2](https://docs.microsoft.com/dotnet/api/?view=netframework-4.7.2) n’est pas installé, l’option Authentification interactive Active Directory n’est pas disponible.


L’authentification interactive Active Directory prend en charge une authentification interactive qui permet d’utiliser l’authentification multifacteur (MFA) Azure Active Directory (AD) pour l’authentification auprès d’Azure SQL Database. Cette méthode prend en charge les utilisateurs Azure AD natifs et fédérés, ainsi que les utilisateurs invités à partir d’autres comptes (notamment les utilisateurs B2B, les comptes Microsoft et non Microsoft comme @outlook.com, @hotmail.com, @live.com ainsi que @gmail.com). Si cette méthode est spécifiée, le **Nom d’utilisateur** doit être spécifié et le champ Mot de passe est désactivé. 

Lors de l’authentification avec *l’authentification interactive Active Directory*, une fenêtre d’authentification s’ouvre qui oblige les utilisateurs à entrer un mot de passe manuellement.

![boîte de dialogue de connexion](media/azure-active-directory/sign-in.png)

La mise en œuvre de l’authentification multifacteur (MFA) est fournie par Azure AD via cette fenêtre contextuelle MFA supplémentaire pendant le processus d’authentification.

> [!NOTE]
> Étant donné que *l’authentification interactive Active Directory* requiert que les utilisateurs entrent manuellement (de manière interactive) leur mot de passe, elle n’est pas recommandée pour les flux de travail automatisés.


## <a name="known-issues-and-limitations"></a>Problèmes connus et limitations

- *L’authentification interactive Active Directory* est uniquement prise en charge lors de la connexion à une base de données SQL Azure. Elle n’est pas prise en charge pour SQL Server (local ou sur une machine virtuelle) ou pour Azure SQL Data Warehouse.
- *L’authentification interactive Active Directory* n’est pas prise en charge dans la boîte de dialogue de connexion dans *l’Explorateur de serveurs*, vous devez vous connecter à l’aide de SSDT avec *l’Explorateur d’objets SQL Server*.
- L’intégration de l’authentification unique avec le compte actuellement connecté dans Visual Studio n’est pas prise en charge pour SSDT.
- Le fichier SQLPackage.exe installé dans le répertoire Extensions lors de l’installation de Visual Studio n’est pas destiné à être utilisé à partir de cet emplacement. Pour utiliser SQLpackage.exe avec AAD, accédez à https://www.microsoft.com/en-us/download/details.aspx?id=55088 
- La comparaison des données SSDT n’est pas prise en charge pour l’authentification AAD, y compris la nouvelle méthode d’authentification.  





## <a name="see-also"></a> Voir aussi  
[Authentification multifacteur](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication)  
[Authentification Azure Active Directory avec SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)  
[SQL Server Data Tools dans Visual Studio](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx)  
[Forum MSDN SSDT](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt)  
[Blog de l’équipe SSDT](http://blogs.msdn.com/b/ssdt/)  
[Référence de l’API DACFx](https://msdn.microsoft.com/library/dn645454.aspx)  
[Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)  
