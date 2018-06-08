---
title: Déploiement d’un assembly personnalisé | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: custom-assemblies
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- deploying custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], deploying
- custom assemblies [Reporting Services], updating
- updating custom assemblies
ms.assetid: c6674cd8-0de7-4a5a-9e7c-12ffa49f6fd2
caps.latest.revision: 46
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d6cf3865befe7c7d717130ddd442eea1d9d9bce9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploying-a-custom-assembly"></a>Déploiement d'un assembly personnalisé
  Pour déployer un assembly personnalisé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], placez-le dans les dossiers d’application du Concepteur de rapports et du serveur de rapports. L’autorisation **Execution** dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est accordée par défaut aux assemblys personnalisés. Pour accorder des privilèges d’assemblys personnalisés supérieurs à l’autorisation Execute, vous devez modifier le fichier de configuration rssrvpolicy.config pour le serveur de rapports et le fichier de configuration rspreviewpolicy.config pour la fenêtre d’aperçu du Concepteur de rapports. Une autre solution consiste à installer votre assembly personnalisé dans le Global Assembly Cache (GAC).  
  
> [!NOTE]  
>  Il y a deux modes d’aperçu pour le Concepteur de rapports : l’onglet Aperçu et la fenêtre d’aperçu contextuelle, qui s’affiche quand votre projet de rapport démarre en mode **DebugLocal**. L’onglet Aperçu exécute toutes les expressions de rapport à l’aide de l’autorisation **FullTrust** définie et n’applique pas de paramètres de stratégie de sécurité. La fenêtre d'aperçu contextuelle a pour but de simuler les fonctionnalités du serveur de rapports ; par conséquent, elle dispose d'un fichier de configuration de stratégie qu'un administrateur ou vous-même devez modifier pour utiliser des assemblys dans le Générateur de rapports. Cet aperçu contextuel verrouille également l'assembly personnalisé. Par conséquent, vous devez fermer la fenêtre d'aperçu afin de modifier ou mettre à jour le code de votre assembly personnalisé.  
  
###### <a name="to-deploy-a-custom-assembly-in-reporting-services"></a>Pour déployer un assembly personnalisé dans Reporting Services  
  
1.  Copiez votre assembly personnalisé à partir de votre emplacement de génération vers le dossier bin du serveur de rapports ou le dossier du Générateur de rapports. L'emplacement par défaut du dossier bin du serveur de rapports est le suivant : %ProgramFiles%\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin. L'emplacement par défaut du Concepteur de rapports est le suivant : %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies.  
  
     Le placement de votre assembly personnalisé dans le dossier Bin du serveur de rapports vous permet de publier des rapports qui font référence à votre assembly personnalisé, et son placement dans le dossier Report Designer vous permet d'exécuter et de déboguer des rapports qui font référence à votre assembly personnalisé dans le Concepteur de rapports.  
  
     Si vous devez accorder au code de votre assembly personnalisé des autorisations supérieures aux autorisations d'exécution par défaut :  
  
2.  Ouvrez le fichier de configuration approprié. L'emplacement par défaut du fichier rssrvpolicy.config est : %ProgramFiles%\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer. L'emplacement par défaut du fichier rspreviewpolicy.config est : %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies.  
  
3.  Ajoutez un groupe de codes pour votre assembly personnalisé. Pour plus d’informations, consultez [Développement sécurisé &#40;Reporting Services&#41;](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
## <a name="updating-custom-assemblies"></a>Mise à jour d'assemblys personnalisés  
 Au bout d'un certain temps, vous risquez d'avoir besoin de mettre à jour une version d'un assembly personnalisé actuellement référencé par plusieurs rapports publiés. Si cet assembly existe déjà dans le répertoire bin du serveur de rapports ou du Générateur de rapports et si le numéro de version de l'assembly est incrémenté ou a changé d'une certaine façon, les rapports actuellement publiés ne fonctionnent plus correctement. Vous devez mettre à jour la version de l’assembly qui est référencé dans l’élément **CodeModules** de la définition de rapport, puis republier les rapports. Si vous savez que vous allez souvent mettre à jour un assembly personnalisé et que vos rapports actuellement publiés vont avoir besoin de référencer le nouvel assembly, vous pouvez envisager d'utiliser le même numéro de version dans toutes les mises à jour d'un assembly particulier.  
  
 Si vous n'avez pas besoin de vos rapports actuellement publiés pour référencer la nouvelle version de l'assembly, vous pouvez déployer votre assembly personnalisé sur le Global Assembly Cache. Le Global Assembly Cache peut gérer plusieurs versions du même assembly, afin que vos rapports actuels puissent référencer la version antérieure de votre assembly et que vos nouveaux rapports publiés puissent référencer l'assembly mis à jour. Pourtant, une autre approche consisterait à définir la redirection de liaison du serveur de rapports afin de forcer une redirection de toutes les demandes de l'ancien assembly vers le nouvel assembly. Il vous faudrait modifier les fichiers Web.config et ReportService.exe.config du serveur de rapports. L'entrée pourrait se présenter de la manière suivante :  
  
```  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <bindingRedirect oldVersion="1.0.0.0"  
                             newVersion="2.0.0.0"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d'assemblys personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [Utilisation d’assemblys et du Global Assembly Cache](http://go.microsoft.com/fwlink/?LinkId=63912)  
  
  
