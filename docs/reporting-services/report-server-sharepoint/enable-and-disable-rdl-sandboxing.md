---
title: Activer et désactiver le sandboxing RDL pour Reporting Services en mode intégré SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 842c02dfb20f6e39e186937bad25f07c40e7b533
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-and-disable-rdl-sandboxing-for-reporting-services-in-sharepoint-integrated-mode"></a>Activer et désactiver le sandboxing RDL pour Reporting Services en mode intégré SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

La fonctionnalité Sandboxing RDL (Report Definition Language) vous permet de détecter et restreindre l’utilisation de types spécifiques de ressources, par les locataires individuels, dans un environnement où plusieurs locataires utilisent une batterie de serveurs web unique de serveurs de rapports. Citons pour exemple un scénario de services d'hébergement où vous pouvez maintenir une batterie de serveurs Web uniques de serveurs de rapports utilisés par plusieurs locataires, et peut-être différentes sociétés. En tant qu'administrateur de serveur de rapports, vous pouvez activer cette fonctionnalité pour aider à accomplir les objectifs suivants :  
  
-   Restreindre la taille des ressources externes. Les ressources externes incluent les images, fichiers .xslt et données cartographiques.  
  
-   Lors de la publication des rapports, limiter les types et membres utilisés dans le texte d'expression.  
  
-   Lors du traitement des rapports, limiter la longueur du texte et la taille de la valeur de retour pour les expressions.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

 Lorsque le Sandboxing RDL est activé, les fonctionnalités suivantes sont désactivées :  
  
-   Code personnalisé dans l’élément **\<<Code>** d’une définition de rapport.  
  
-   Mode de compatibilité descendante RDL pour les éléments de rapport personnalisé [!INCLUDE[ssRSversion2005](../../includes/ssrsversion2005-md.md)] .  
  
-   Paramètres nommés dans les expressions  
  
 Cette rubrique décrit chaque élément de l’élément \<**RDLSandboxing**> dans le fichier RSReportServer.Config. Pour plus d’informations sur la modification de ce fichier, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md). Un journal des traces de serveur enregistre l'activité liée à la fonctionnalité Sandboxing RDL. Pour plus d’informations sur les journaux des traces, consultez [Journal des traces du service Report Server](../../reporting-services/report-server/report-server-service-trace-log.md).  
  
## <a name="example-configuration"></a>Exemple de configuration
 L’exemple suivant montre les paramètres et des exemples de valeurs pour l’élément \<**RDLSandboxing**> dans le fichier RSReportServer.Config.  
  
```  
<RDLSandboxing>  
   <MaxExpressionLength>5000</MaxExpressionLength>  
   <MaxResourceSize>5000</MaxResourceSize>  
   <MaxStringResultLength>3000</MaxStringResultLength>  
   <MaxArrayResultLength>250</MaxArrayResultLength>  
   <Types>  
      <Allow Namespace=”System.Drawing” AllowNew=”True”>Bitmap</Allow>  
      <Allow Namespace=”TypeConverters.Custom” AllowNew=”True”>*</Allow>  
   </Types>  
   <Members>  
      <Deny>Format</Deny>  
      <Deny>StrDup</Deny>  
   </Members>  
</RDLSandboxing>  
```

## <a name="configuration-settings"></a>Paramètres de configuration

 Le tableau suivant contient des informations sur les paramètres de configuration. Les paramètres sont présentés dans l'ordre dans lequel ils apparaissent dans le fichier de configuration.  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**MaxExpressionLength**|Quantité maximale de caractères autorisés dans les expressions RDL.<br /><br /> Par défaut : 1000|  
|**MaxResourceSize**|Quantité maximale de Ko autorisés pour une ressource externe.<br /><br /> Valeur par défaut : 100|  
|**MaxStringResultLength**|Quantité maximale de caractères autorisés dans une valeur de retour pour une expression RDL.<br /><br /> Par défaut : 1000|  
|**MaxArrayResultLength**|Quantité maximale d'éléments autorisés dans un tableau de valeurs retourné pour une expression RDL.<br /><br /> Valeur par défaut : 100|  
|**Types**|Liste des membres à autoriser dans les expressions RDL.|  
|**Allow**|Type ou jeu de types à autoriser dans les expressions RDL.|  
|**Espace de noms**|Attribut pour **Allow** qui est l’espace de noms contenant un ou plusieurs types qui s’appliquent à Valeur. Cette propriété n'est pas sensible à la casse.|  
|**AllowNew**|Attribut booléen pour **Allow** qui contrôle si la création de nouvelles instances du type est autorisée dans les expressions RDL ou dans un élément **\<Class>** RDL.<br /><br /> Quand **RDLSandboxing** est activé, de nouveaux tableaux ne peuvent pas être créés dans les expressions RDL, indépendamment de la valeur de **AllowNew**.|  
|**Value**|Valeur pour **Allow** qui est le nom du type à autoriser dans les expressions RDL. La valeur **\*** indique que tous les types dans l’espace de noms sont autorisés. Cette propriété n'est pas sensible à la casse.|  
|**Membres**|Pour la liste des types qui sont inclus dans l’élément **\<Types>**, la liste des noms de membres qui ne sont pas autorisés dans les expressions RDL.|  
|**Refuser**|Nom d'un membre qui n'est pas autorisé dans les expressions RDL. Cette propriété n'est pas sensible à la casse.<br /><br /> Lorsque **Deny** est spécifié pour un membre, tous les membres avec ce nom pour tous les types ne sont pas autorisés.|  
  
## <a name="working-with-expressions-when-rdl-sandboxing-is-enabled"></a>Utilisation des expressions lorsque le sandboxing RDL est activé

Vous pouvez modifier la fonctionnalité Sandboxing RDL afin d'aider à gérer les ressources utilisées par une expression des manières suivantes :  
  
-   Limitez le nombre de caractères utilisés pour une expression.  
  
-   Limitez la taille du résultat retourné par une expression.  
  
-   Autorisez une liste spécifique de types qui peuvent être utilisés dans une expression.  
  
-   Limitez la liste de membres par nom pour la liste des types autorisés qui peuvent être utilisés dans une expression.  
  
-   La fonctionnalité Sandboxing RDL vous permet de créer une liste de types approuvés et une liste de membres refusés. La liste des types approuvés porte le nom de liste verte. La liste des membres refusés porte le nom de liste rouge.  
  
> [!NOTE]  
>  Dans la définition de rapport, un ordinateur ne peut pas connaître le type de chaque instance d'une référence d'expression. Lorsque vous ajoutez un membre à la liste rouge, vous refusez tous les membres de ce nom parmi tous les types dans la liste verte.  
  
 Les résultats d'expression RDL sont vérifiés au moment de l'exécution. Les expressions RDL sont vérifiées dans la définition de rapport lorsque le rapport est publié. Contrôlez le journal des traces du serveur de rapports afin de détecter toute violation. Pour plus d’informations, consultez [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md).  
  
### <a name="working-with-types"></a>Utilisation des types

 Lorsque vous ajoutez un type à la liste verte, vous contrôlez les points d'entrée suivants pour accéder aux expressions RDL :  
  
-   Membres statiques d'un type.  
  
-   Nouvelle méthode [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] **New** .  
  
-   Élément **\<Classes>** dans la définition de rapport.  
  
-   Membres que vous avez ajoutés à la liste rouge pour un type dans la liste verte.  
  
 La liste verte ne contrôle pas les points d'entrée suivants :  
  
-   Datasets du rapport. Les champs dans les datasets de rapport retournés à partir de requêtes peuvent contenir tout type RDL valide.  
  
-   des paramètres de rapport ; Les valeurs de paramètres fournies par l'utilisateur peuvent contenir tout type RDL valide.  
  
-   Membres d'un type actif qui ne sont pas dans la liste rouge. Par défaut, tous les membres de tous les types dans la liste verte sont activés. Lorsque vous ajoutez un nom de membre à la liste rouge, vous refusez tous les membres avec ce nom parmi tous les types figurant dans la liste verte.  
  
 Pour activer un membre d'un type mais refuser un membre avec le même nom pour un type différent, vous devez effectuer les opérations suivantes :  
  
-   Ajouter un élément **\<Deny>** pour le nom de membre.  
  
-   Créer un membre de proxy avec un nom différent sur une classe dans un assembly personnalisé pour le membre que vous souhaitez activer.  
  
-   Ajouter cette classe nouvelle à la liste verte.  
  
 Pour ajouter des fonctions [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework à la liste autorisée, ajoutez les types correspondants de l’espace de noms Microsoft.VisualBasic à cette liste.  
  
 Pour ajouter des mots clés du type [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework à la liste verte, ajoutez le type CLR correspondant à la liste verte. Par exemple, pour utiliser le mot clé [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework **Integer**, ajoutez le fragment XML suivant à l’élément **\<RDLSandboxing>**  :  
  
```  
<Allow Namespace="System">Int32</Allow>  
```  
  
 Pour ajouter un type générique ou un type nullable [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework à la liste autorisée, vous devez procéder comme suit :  
  
-   Créer un type de proxy pour le type nullable générique ou [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework.  
  
-   Ajouter le type de proxy à la liste verte.  
  
 L'ajout d'un type d'un assembly personnalisé à la liste verte n'accorde pas implicitement l'autorisation Exécuter sur l'assembly. Vous devez modifier spécifiquement le fichier de sécurité d'accès du code et fournir l'autorisation Exécuter à votre assembly. Pour plus d’informations, consultez [Sécurité d’accès du code dans Reporting Services](../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md).  
  
#### <a name="maintaining-the-deny-list-of-members"></a>Maintenance de la liste de membres \<Deny>

 Lorsque vous ajoutez un nouveau type à la liste verte, vous utilisez la liste suivante pour déterminer quand vous devrez peut-être mettre à jour la liste rouge de membres :  
  
-   Lorsque vous mettez à jour un assembly personnalisé avec une version qui introduit de nouveaux types.  
  
-   Lorsque vous ajoutez des membres aux types dans la liste verte.  
  
-   Quand vous mettez à jour le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sur le serveur de rapports.  
  
-   Lorsque vous effectuez une mise à niveau du serveur de rapports vers une version ultérieure de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Lorsque vous mettez à jour un serveur de rapports pour gérer un schéma RDL ultérieur, car de nouveaux membres ont pu être ajoutés aux types RDL.  
  
### <a name="working-with-operators-and-new"></a>Utilisation des opérateurs et de New

 Par défaut, les opérateurs de langage [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework, à l’exception de **New**, sont toujours autorisés. L’opérateur **New** est contrôlé par l’attribut **AllowNew** sur l’élément **\<Allow>**. Les autres opérateurs de langage, comme l’opérateur d’accesseur de collection par défaut **!** et les macros de conversion de type de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET Framework, comme **CInt**, sont toujours autorisés.  
  
 L'ajout d'opérateurs à une liste rouge, y compris des opérateurs personnalisés, n'est pas pris en charge. Pour exclure des opérateurs pour un type, vous devez effectuer les opérations suivantes :  
  
-   Créer un type de proxy qui n'implémente pas les opérateurs que vous souhaitez exclure.  
  
-   Ajouter le type de proxy à la liste verte.  
  
 Pour créer un tableau dans une expression RDL, créez le tableau dans une méthode sur une classe que vous définissez et ajoutez cette classe à la liste verte.  
  
 Pour créer un tableau dans une expression RDL, vous devez effectuer les opérations suivantes :  
  
-   Définir une nouvelle classe et créer le tableau dans une méthode sur cette classe.  
  
-   Ajouter la classe à la liste verte.  
  
## <a name="see-also"></a>Voir aussi

 [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Journal de suivi de service du serveur de rapports](../../reporting-services/report-server/report-server-service-trace-log.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
