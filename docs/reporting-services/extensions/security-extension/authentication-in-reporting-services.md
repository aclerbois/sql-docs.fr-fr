---
title: Authentification dans Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- security [Reporting Services], authentication
- forms-based authentication [Reporting Services]
- authentication [Reporting Services]
- custom authentication [Reporting Services]
ms.assetid: 103ce1f9-31d8-44bb-b540-2752e4dcf60b
caps.latest.revision: 25
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: b84994cb993c061f006880e4a1f4f727be864ec9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="authentication-in-reporting-services"></a>Authentification dans Reporting Services
  L'authentification est le processus d'établissement du droit d'un utilisateur à une identité. De nombreuses techniques vous permettent d'authentifier un utilisateur. La façon la plus courante consiste à utiliser des mots de passe. Par exemple, lorsque vous implémentez l'authentification par formulaire, vous voulez une implémentation qui interroge les utilisateurs au sujet de leurs informations d'identification (généralement par le biais d'une interface qui demande un nom de connexion et un mot de passe), puis valide les utilisateurs par rapport à une banque de données, telle qu'une table de base de données ou un fichier de configuration. Si les informations d'identification ne peuvent pas être validées, le processus d'authentification échoue et l'utilisateur assume une identité anonyme.  
  
## <a name="custom-authentication-in-reporting-services"></a>Authentification personnalisée dans Reporting Services  
 Dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], le système d'exploitation Windows traite l'authentification des utilisateurs par le biais de la sécurité intégrée ou de la réception explicite et de la validation des informations d'identification des utilisateurs. L'authentification personnalisée peut être développée dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] pour prendre en charge des schémas d'authentification supplémentaires. Cela est possible grâce à l'interface d'extension de sécurité <xref:Microsoft.ReportingServices.Interfaces.IAuthenticationExtension2>. Toutes les extensions héritent de l'interface de base <xref:Microsoft.ReportingServices.Interfaces.IExtension> pour toute extension déployée et utilisée par le serveur de rapports. <xref:Microsoft.ReportingServices.Interfaces.IExtension>, ainsi que <xref:Microsoft.ReportingServices.Interfaces.IAuthenticationExtension2>, sont membres de l'espace de noms <xref:Microsoft.ReportingServices.Interfaces>.  
  
 Le principal moyen d'authentification auprès d'un serveur de rapports dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] est la méthode <xref:ReportService2010.ReportingService2010.LogonUser%2A>. Ce membre du service Web Reporting Services peut être utilisé pour passer les informations d'identification de l'utilisateur à un serveur de rapports pour validation. Votre extension de sécurité sous-jacente implémente **IAuthenticationExtension2.LogonUser** qui contient votre code d’authentification personnalisé. Dans l’exemple d’authentification par formulaire, **LogonUser** effectue un contrôle d’authentification par rapport aux informations d’identification fournies et un magasin d’utilisateurs personnalisé dans une base de données. L’exemple suivant illustre une implémentation de **LogonUser** :  
  
```  
public bool LogonUser(string userName, string password, string authority)  
{  
   return AuthenticationUtilities.VerifyPassword(userName, password);  
}  
  
```  
  
 L'exemple de fonction suivant est utilisé pour vérifier les informations d'identification fournies :  
  
```  
  
internal static bool VerifyPassword(string suppliedUserName,  
   string suppliedPassword)  
{   
   bool passwordMatch = false;  
   // Get the salt and pwd from the database based on the user name.  
   // See "How To: Use DPAPI (Machine Store) from ASP.NET," "How To:  
   // Use DPAPI (User Store) from Enterprise Services," and "How To:  
   // Create a DPAPI Library" for more information about how to use  
   // DPAPI to securely store connection strings.  
   SqlConnection conn = new SqlConnection(  
      "Server=localhost;" +   
      "Integrated Security=SSPI;" +  
      "database=UserAccounts");  
   SqlCommand cmd = new SqlCommand("LookupUser", conn);  
   cmd.CommandType = CommandType.StoredProcedure;  
  
   SqlParameter sqlParam = cmd.Parameters.Add("@userName",  
       SqlDbType.VarChar,  
       255);  
   sqlParam.Value = suppliedUserName;  
   try  
   {  
      conn.Open();  
      SqlDataReader reader = cmd.ExecuteReader();  
      reader.Read(); // Advance to the one and only row  
      // Return output parameters from returned data stream  
      string dbPasswordHash = reader.GetString(0);  
      string salt = reader.GetString(1);  
      reader.Close();  
      // Now take the salt and the password entered by the user  
      // and concatenate them together.  
      string passwordAndSalt = String.Concat(suppliedPassword, salt);  
      // Now hash them  
      string hashedPasswordAndSalt =  
         FormsAuthentication.HashPasswordForStoringInConfigFile(  
         passwordAndSalt,  
         "SHA1");  
      // Now verify them. Returns true if they are equal.  
      passwordMatch = hashedPasswordAndSalt.Equals(dbPasswordHash);  
   }  
   catch (Exception ex)  
   {  
       throw new Exception("Exception verifying password. " +  
          ex.Message);  
   }  
   finally  
   {  
       conn.Close();  
   }  
   return passwordMatch;  
}  
```  
  
## <a name="authentication-flow"></a>Flux d'authentification  
 Le service web Reporting Services fournit des extensions d’authentification personnalisées pour permettre au portail web et au serveur de rapports d’effectuer l’authentification par formulaire.  
  
 La méthode <xref:ReportService2010.ReportingService2010.LogonUser%2A> du service Web Reporting Services est utilisée pour envoyer des informations d'identification au serveur de rapports pour authentification. Le service Web utilise des en-têtes HTTP pour passer un ticket d'authentification (appelé « cookie ») du serveur au client pour les demandes d'ouverture de session validées.  
  
 L'illustration suivante représente la méthode d'authentification d'utilisateurs auprès du service Web lorsque votre application est déployée avec un serveur de rapports configuré pour utiliser une extension d'authentification personnalisée.  
  
 ![Flux d’authentification de sécurité de Reporting Services](../../../reporting-services/extensions/security-extension/media/rosettasecurityextensionauthenticationflow.gif "Flux d’authentification de sécurité de Reporting Services")  
  
 Comme indiqué dans la figure 2, le processus d'authentification est le suivant :  
  
1.  Une application cliente appelle la méthode <xref:ReportService2010.ReportingService2010.LogonUser%2A> du service Web pour authentifier un utilisateur.  
  
2.  Le service web passe un appel à la méthode <xref:ReportService2010.ReportingService2010.LogonUser%2A> de votre extension de sécurité, plus précisément la classe qui implémente **IAuthenticationExtension2**.  
  
3.  Votre implémentation de <xref:ReportService2010.ReportingService2010.LogonUser%2A> valide le nom d'utilisateur et le mot de passe dans le magasin d'utilisateurs ou l'autorité de sécurité.  
  
4.  En cas d'authentification réussie, le service Web crée un cookie et le gère pour la session.  
  
5.  Le service Web retourne le ticket d'authentification à l'application appelante sur l'en-tête HTTP.  
  
 Lorsque le service Web authentifie avec succès un utilisateur par le biais de l'extension de sécurité, il génère un cookie qui est utilisé pour les demandes suivantes. Le cookie n'est pas persistant dans l'autorité de sécurité personnalisée puisque celle-ci n'appartient pas au serveur de rapports. Le cookie est retourné par la méthode <xref:ReportService2010.ReportingService2010.LogonUser%2A> du service Web et est utilisé dans les appels de méthode du service Web suivants et dans l'accès URL.  
  
> [!NOTE]  
>  Pour éviter de compromettre le cookie pendant la transmission, les cookies d'authentification retournés par <xref:ReportService2010.ReportingService2010.LogonUser%2A> doivent être transmis de manière sécurisée à l'aide du chiffrement SSL (Secure Sockets Layer).  
  
 Si vous accédez au serveur de rapports par l'accès URL lorsqu'une extension de sécurité personnalisée est installée, les services Internet (IIS) et [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] gèrent automatiquement la transmission du ticket d'authentification. Si vous accédez au serveur de rapports par le biais de l'API SOAP, votre implémentation de la classe proxy doit inclure la prise en charge supplémentaire pour gérer le ticket d'authentification. Pour plus d'informations sur l'utilisation de l'API SOAP et la gestion du ticket d'authentification, consultez « Utilisation du service Web avec la sécurité personnalisée ».  
  
## <a name="forms-authentication"></a>Authentification par formulaire  
 L'authentification par formulaire est un type d'authentification [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] dans laquelle un utilisateur non authentifié est dirigé vers un formulaire HTML. Lorsque l'utilisateur fournit des informations d'identification, le système émet un cookie qui contient un ticket d'authentification. Lors des demandes ultérieures, le système examine d'abord le cookie pour déterminer si l'utilisateur a déjà été authentifié par le serveur de rapports.  
  
 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] peut être étendu pour prendre en charge l'authentification par formulaire à l'aide des interfaces d'extensibilité de la sécurité disponibles par le biais de l'API Reporting Services. Si vous étendez [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] pour utiliser l'authentification par formulaire, utilisez le chiffrement SSL pour toutes les communications avec le serveur de rapports pour empêcher des utilisateurs malveillants d'accéder au cookie d'un autre utilisateur. Le chiffrement SSL permet aux clients et au serveur de rapports de s'authentifier mutuellement et garantit qu'aucun autre ordinateur ne peut lire le contenu des communications entre les deux ordinateurs. Toutes les données envoyées par un client par un client une connexion SSL sont chiffrées pour empêcher des utilisateurs malveillants d'intercepter des mots de passe ou des données envoyés à un serveur de rapports.  
  
 L'authentification par formulaire est généralement implémentée pour prendre en charge des comptes et l'authentification pour des plateformes autres que Windows. Une interface graphique est présentée à un utilisateur qui demande l'accès à un serveur de rapports, et les informations d'identification fournies sont envoyées à une autorité de sécurité pour authentification.  
  
 L'authentification par formulaire nécessite qu'une personne soit présente pour entrer des informations d'identification. Pour les applications sans assistance qui communiquent directement avec le service Web Reporting Services, l'authentification par formulaire doit être associée à un schéma d'authentification personnalisé.  
  
 L'authentification par formulaire est adaptée à [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] lorsque vous devez répondre aux deux exigences suivantes :  
  
-   Vous devez stocker et authentifier des utilisateurs qui ne possèdent pas de comptes [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows.  
  
-   Vous devez fournir votre propre formulaire d'interface utilisateur en tant que page d'ouverture de session entre différentes pages sur un site Web.  
  
 Tenez compte des points suivants lors de l'écriture d'une extension de sécurité personnalisée qui prend en charge l'authentification par formulaire :  
  
-   Si vous utilisez l'authentification par formulaire, l'accès anonyme doit être activé sur le répertoire virtuel du serveur de rapports dans les services Internet (IIS).  
  
-   L'authentification [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] doit avoir la valeur « Forms ». Vous configurez l'authentification [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] dans le fichier Web.config pour le serveur de rapports.  
  
-   [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] peut authentifier et autoriser des utilisateurs avec l'authentification Windows ou l'authentification personnalisée, mais pas les deux à la fois. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] ne prend pas en charge l'utilisation simultanée de plusieurs extensions de sécurité.  
  
## <a name="see-also"></a> Voir aussi  
 [Implémentation d'une extension de sécurité](../../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
  
  
