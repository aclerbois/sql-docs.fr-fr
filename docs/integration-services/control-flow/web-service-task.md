---
title: Service web, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.webservicetask.f1
- sql13.dts.designer.webservicestask.general.f1
- sql13.dts.designer.webservicestask.input.f1
- sql13.dts.designer.webservicestask.output.f1
helpviewer_keywords:
- Web Service task [Integration Services]
ms.assetid: 5c7206f1-7d6a-4923-8dff-3c4912da4157
caps.latest.revision: 57
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c82398a98bcd3233586d0effbc369fe6827321f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="web-service-task"></a>Tâche de service Web
  La tâche de service Web exécute une méthode de service Web. Vous pouvez utiliser la tâche de service Web pour :  
  
-   Écrire dans une variable les valeurs renvoyées par une méthode de service Web. Vous pouvez par exemple obtenir la température la plus élevée de la journée à partir d'une méthode de service Web, puis utiliser cette valeur pour mettre à jour une variable utilisée dans une expression qui définit une valeur de colonne.  
  
-   Écrire dans un fichier les valeurs renvoyées par une méthode de service Web. Une liste de clients potentiels peut par exemple être enregistrée dans un fichier qui est ensuite utilisé comme source de données dans un package qui nettoie les données avant de les écrire dans une base de données.  
  
## <a name="wsdl-file"></a>Fichier WSDL  
 La tâche de service Web utilise un gestionnaire de connexions HTTP pour se connecter au service Web. Ce gestionnaire est configuré séparément de la tâche de service Web et est référencé dans la tâche. Le gestionnaire de connexions HTTP spécifie les paramètres proxy du serveur comme l'URL du serveur, les informations d'identification permettant d'accéder au serveur de services Web et la durée du délai d'attente. Pour plus d’informations, consultez [Gestionnaire de connexions HTTP](../../integration-services/connection-manager/http-connection-manager.md).  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions HTTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
 Le gestionnaire de connexions HTTP peut pointer vers un site Web ou un fichier WSDL (Web Service Description Language). L'URL du gestionnaire de connexions HTTP qui pointe vers un fichier WSDL contient le paramètre `?WSDL` , par exemple `http://MyServer/MyWebService/MyPage.asmx?WSDL`.  
  
 Le fichier WSDL doit être disponible localement pour pouvoir configurer la tâche de service web à l’aide de la boîte de dialogue **Éditeur de tâche de service Web** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
-   Si le gestionnaire de connexions HTTP pointe vers un site Web, le fichier WSDL doit être copié manuellement sur un ordinateur local.  
  
-   Si le gestionnaire de connexions HTTP pointe vers un fichier WSDL, ce fichier peut être téléchargé à partir du site Web vers un fichier local par la tâche de service Web.  
  
 Le fichier WSDL énumère les méthodes offertes par le service Web, les paramètres d'entrée requis par les méthodes, les réponses renvoyées par les méthodes et la manière de communiquer avec le service Web.  
  
 Si la méthode utilise des paramètres d'entrée, la tâche de service Web requiert des valeurs de paramètres. Par exemple, une méthode de service Web qui recommande la longueur des skis que vous devez acheter en fonction de votre taille exige que votre taille soit soumise dans un paramètre d'entrée. Les valeurs de paramètres peuvent être fournies par des chaînes définies dans la tâche ou par des variables définies dans l'étendue de la tâche ou un conteneur parent. L'avantage d'utiliser des variables est de vous permettre de mettre à jour dynamiquement les valeurs de paramètres en utilisant des scripts ou des configurations de package. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Configurations de package](../../integration-services/packages/package-configurations.md).  
  
 De nombreuses méthodes de service Web n'utilisent pas de paramètres d'entrée. Par exemple, une méthode de service Web qui obtient le nom des présidents dont le mois de naissance correspond au mois en cours ne requiert pas de paramètre d'entrée car le service Web peut déterminer le mois en cours localement.  
  
 Les résultats de la méthode de service Web peuvent être écrits dans une variable ou dans un fichier. Vous pouvez utiliser le gestionnaire de connexions de fichiers pour spécifier le fichier ou indiquer le nom de la variable dans laquelle écrire les résultats. Pour plus d’informations, consultez [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md) et [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
## <a name="custom-logging-messages-available-on-the-web-service-task"></a>Messages de journalisation personnalisés disponibles dans la tâche de service Web  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche de service Web. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**WSTaskBegin**|La tâche a commencé à accéder à un service Web.|  
|**WSTaskEnd**|La tâche a terminé une méthode de service Web.|  
|**WSTaskInfo**|Donne des informations détaillées relatives à la tâche.|  
  
## <a name="configuration-of-the-web-service-task"></a>Configuration de la tâche de service Web  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d’une tâche ou d’un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-the-web-service-task"></a>Configuration par programmation de la tâche de service web  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WebServiceTask.WebServiceTask>  
  
## <a name="web-service-task-editor-general-page"></a>Éditeur de tâche de service Web (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche de service Web** pour définir un gestionnaire de connexions HTTP, spécifier l’emplacement du fichier WSDL (Web Services Description Language) qu’utilise la tâche de service web, décrire la tâche de service web et télécharger le fichier WSDL.  
  
### <a name="options"></a>Options  
 **HTTPConnection**  
 Sélectionnez un gestionnaire de connexions dans la liste ou cliquez sur \<**Nouvelle connexion...**> pour en créer un.  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions HTTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
 **Rubriques connexes :**  [Gestionnaire de connexions HTTP](../../integration-services/connection-manager/http-connection-manager.md), [Éditeur du gestionnaire de connexions HTTP &#40;page Server (Serveur)&#41;](../../integration-services/connection-manager/http-connection-manager-editor-server-page.md)  
  
 **WSDLFile**  
 Tapez le chemin d’accès qualifié complet d’un fichier WSDL qui est en local sur l’ordinateur ou cliquez sur le bouton Parcourir **(…)** et recherchez ce fichier.  
  
 Si vous avez déjà téléchargé manuellement le fichier WSDL sur l'ordinateur, sélectionnez-le. Toutefois, si le fichier WSDL n'a pas encore été téléchargé, procédez comme suit :  
  
-   Créez un fichier vide ayant l'extension de nom de fichier .wsdl.  
  
-   Sélectionnez ce fichier vide pour l’option **WSDLFile** .  
  
-   Affectez à **OverwriteWSDLFile** la valeur **True** pour permettre le remplacement du fichier vide par le fichier WSDL réel.  
  
-   Cliquez sur **Télécharger WSDL** pour télécharger le fichier WSDL réel et remplacer le fichier vide.  
  
    > [!NOTE]  
    >  L’option **Télécharger WSDL** n’est pas activée tant que vous n’avez pas indiqué le nom d’un fichier local existant dans la zone **WSDLFile** .  
  
 **OverwriteWSDLFile**  
 Indiquez si le fichier WSDL de la tâche de service Web peut être remplacé.  
  
 Si vous projetez de télécharger le fichier WSDL à l’aide du bouton **Télécharger WSDL** , définissez cette valeur sur **True**.  
  
 **Nom**  
 Fournissez un nom unique pour la tâche de service Web. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche de service Web.  
  
 **Télécharger WSDL**  
 Téléchargez le fichier WSDL.  
  
 Ce bouton n’est pas activé tant que vous n’avez pas indiqué le nom d’un fichier local existant dans la zone **WSDLFile** .  
  
## <a name="web-service-task-editor-input-page"></a>Éditeur de tâche de service Web (page Entrée)
  Utilisez la page **Entrée** de l' **Éditeur de tâche de service Web** pour définir le service Web, la méthode Web et les valeurs à fournir à la méthode Web comme entrée. Vous pouvez fournir les valeurs en tapant des chaînes directement dans la colonne Valeur ou en y sélectionnant des variables.  
  
### <a name="options"></a>Options  
 **Service**  
 Sélectionnez un service Web dans la liste pour l'utiliser afin d'exécuter la méthode Web.  
  
 **Méthode**  
 Dans la liste, sélectionnez la méthode Web que doit exécuter la tâche.  
  
 **WebMethodDocumentation**  
 Entrez la description de la méthode web ou cliquez sur le bouton Parcourir **(…)** et entrez la description dans la boîte de dialogue **Documentation de la méthode Web** .  
  
 **Nom**  
 Contient la liste des noms des entrées dans la méthode Web.  
  
 **Type**  
 Indique le type des données des entrées.  
  
> [!NOTE]  
>  La tâche de service Web prend uniquement en charge les paramètres des types de données suivants : les types de primitives tels que les entiers et les chaînes ; les tableaux et séquences de types de primitives, ainsi que les énumérations.  
  
 **Variable**  
 Activez les cases à cocher afin d'utiliser des variables pour fournir les entrées.  
  
 **Value**  
 Si les cases Variable sont cochées, sélectionnez les variables dans la liste pour fournir les entrées ; sinon, tapez les valeurs à utiliser en guise d’entrées.  
  
## <a name="web-service-task-editor-output-page"></a>Éditeur de tâche de service Web (page Sortie)
  Utilisez la page **Sortie** de la boîte de dialogue **Éditeur de tâche de service Web** pour définir l’emplacement de stockage du résultat retourné par la méthode web.  
  
### <a name="static-options"></a>Options statiques  
 **OutputType**  
 Sélectionnez le type de stockage à utiliser pour stocker le résultat. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Stocke le résultat dans un fichier. Cette valeur affiche l’option dynamique **Fichier**.|  
|**Variable**|Stocke le résultat dans une variable. Cette valeur affiche l’option dynamique **Variable**.|  
  
### <a name="outputtype-dynamic-options"></a>Options dynamiques OutputType  
  
#### <a name="outputtype--file-connection"></a>OutputType = Connexion de fichiers  
 **Fichier**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**Nouvelle connexion...**> pour en créer un.  
  
 **Rubriques connexes :** [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md), [File Connection Manager Editor](../../integration-services/connection-manager/file-connection-manager-editor.md)  
  
#### <a name="outputtype--variable"></a>OutputType = Variable  
 **Variable**  
 Sélectionnez une variable dans la liste ou cliquez sur \<**Nouvelle variable...**> pour en créer une.  
  
 **Rubriques connexes :**  [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](http://msdn.microsoft.com/library/d09b5d31-433f-4f7c-8c68-9df3a97785d5)  
  
## <a name="related-content"></a>Contenu associé  
 Vidéo, [Procédure : appeler un service Web à l’aide de la tâche Service Web (vidéo liée à SQL Server)](http://go.microsoft.com/fwlink/?LinkId=259642), sur technet.microsoft.com.  
