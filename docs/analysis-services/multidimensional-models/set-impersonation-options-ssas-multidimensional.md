---
title: Définir les Options d’emprunt d’identité (SSAS - multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6c11064ecc87744999c31080e6a4d57a3849f5d2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="set-impersonation-options-ssas---multidimensional"></a>Définir les options d'emprunt d'identité (SSAS - Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Quand vous créez un objet **source de données** dans un modèle Analysis Services, un des paramètres que vous devez configurer est une option d’emprunt d’identité. Cette option détermine si Analysis Services utilise l'identité d'un compte d'utilisateur Windows spécifique lors d'opérations locales spécifiques associées à la connexion, telles que le téléchargement d'un fournisseur de données OLE DB ou la résolution d'informations de profil utilisateur dans des environnements qui prennent en charge les profils itinérants.  
  
 Pour les connexions qui utilisent l'authentification Windows, l'option d'emprunt d'identité détermine également l'identité de l'utilisateur sous laquelle les requêtes s'exécutent sur la source de données externe. Par exemple, si vous définissez l’option d’emprunt d’identité sur **contoso\dbuser**, les requêtes utilisées pour récupérer les données pendant le traitement sont exécutées en tant que **contoso\dbuser** sur le serveur de base de données.  
  
 Cette rubrique explique comment définir les options d’emprunt d’identité dans la boîte de dialogue **Informations d’emprunt d’identité** pendant la configuration d’un objet source de données.  
  
## <a name="set-impersonation-options-in-sql-server-data-tools"></a>Définir les options d'emprunt d'identité dans SQL Server Data Tools  
  
1.  Double-cliquez sur une source de données dans l'Explorateur de solutions pour ouvrir le Concepteur de source de données.  
  
2.  Cliquez sur l’onglet **Informations d’emprunt d’identité** dans le Concepteur de source de données.  
  
3.  Sélectionnez une option décrite dans la section [Options d’emprunt d’identité](#bkmk_options) de cette rubrique.  
  
## <a name="set-impersonation-options-in-management-studio"></a>Définir les options d'emprunt d'identité dans Management Studio  
 Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ouvrez la boîte de dialogue **Informations d’emprunt d’identité** en cliquant sur le bouton représentant des points de suspension (**...**) pour obtenir les propriétés de boîtes de dialogue suivantes :  
  
-   Boîte de dialogue**Propriétés de la base de données** , via la propriété Informations d’emprunt d’identité de source de données.  
  
-   Boîte de dialogue**Propriétés de la source de données** , via la propriété Informations d’emprunt d’identité.  
  
-   Boîte de dialogue**Propriétés de l’assembly** , via la propriété Informations d’emprunt d’identité.  
  
##  <a name="bkmk_options"></a> Options d’emprunt d’identité  
 Toutes les options sont disponibles dans la boîte de dialogue, mais elles ne sont pertinentes pour tous les scénarios. Utilisez les informations suivantes pour déterminer la meilleure solution pour votre scénario.  
  
 **Utiliser un nom d'utilisateur et un mot de passe spécifiques**  
 Sélectionnez cette option pour que le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] objet utiliser les informations d’identification de sécurité d’un compte d’utilisateur Windows spécifié dans ce format :  *\<nom de domaine >***\\***\<utilisateur nom du compte >*.  
  
 Sélectionnez cette option pour utiliser une identité d'utilisateur Windows dédiée et dotée de privilèges minimaux que vous avez créée spécifiquement à des fins d'accès aux données. Par exemple, si vous créez de manière régulière un compte à usage général pour la récupération de données utilisées dans des rapports, vous pouvez spécifier ce compte ici.  
  
 Pour les bases de données multidimensionnelles, les informations d'identification spécifiées sont utilisées pour le traitement, les requêtes ROLAP, les liaisons hors ligne, les cubes locaux, les modèles d'exploration, les partitions distantes, les objets liés et la synchronisation entre la cible et la source.  
  
 Pour les instructions DMX OPENQUERY, cette option est ignorée et les informations d’identification de l’utilisateur actuel sont utilisées de préférence à celles du compte d’utilisateur spécifié.  
  
 **Utiliser le compte de service**  
 Sélectionnez cette option pour que l'objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les informations d'identification de sécurité associées au service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui gère l'objet. Il s'agit de l'option par défaut. Dans les versions précédentes, il s'agissait de la seule option pouvant être utilisée. Vous préférerez peut-être opter pour cette option si vous souhaitez surveiller l'accès aux données au niveau du service plutôt qu'au niveau des comptes d'utilisateurs individuels.  
  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], selon le système d'exploitation utilisé, le compte de service peut être NetworkService ou un compte virtuel intégré créé pour une instance spécifique d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Si vous sélectionnez le compte de service d'une connexion qui utilise l'authentification Windows, n'oubliez pas de créer un nom d'accès à la base de données pour ce compte et d'accorder des autorisations de lecture, car il sera utilisé pour récupérer les données lors du traitement. Pour plus d’informations sur le compte de service, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
> [!NOTE]  
>  Si vous utilisez l’authentification de base de données, vous devez sélectionner l’option d’emprunt d’identité **Utiliser le compte de service** si le service s’exécute sous le compte virtuel dédié pour Analysis Services. Ce compte possédera les autorisations d'accès aux fichiers locaux. Si le service s’exécute en tant que NetworkService, utilisez un compte d’utilisateur Windows doté de privilèges minimaux qui a des autorisations **Permettre l’ouverture d’une session locale** . En fonction du compte spécifié, vous devrez peut-être aussi accorder des autorisations d'accès aux fichiers dans le dossier du programme Analysis Services.  
  
 Pour les bases de données multidimensionnelles, les informations d'identification du compte de service sont utilisées pour le traitement, les requêtes ROLAP, les partitions distantes, les objets liés et la synchronisation entre la cible et la source.  
  
 Pour les instructions DMX OPENQUERY, les cubes locaux et les modèles d'exploration de données, les informations d'identification de l'utilisateur courant sont utilisées même si vous choisissez l'option de compte de service. L'option de compte de service n'est pas prise en charge pour les liaisons hors ligne.  
  
> [!NOTE]  
>  Des erreurs peuvent se produire lors du traitement d'un modèle d'exploration de données d'un cube si le compte de service ne dispose pas d'autorisations d'administrateur sur l'instance Analysis Services. Pour plus d’informations, consultez [Structure d’exploration de données : problème lors du traitement si la source de données est dans un cube OLAP](http://go.microsoft.com/fwlink/?LinkId=251610).  
  
 **Utiliser les informations d'identification de l'utilisateur actuel**  
 Sélectionnez cette option afin que l'objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les informations d'identification de sécurité de l'utilisateur actuel pour les liaisons hors ligne, les instructions DMX OPENQUERY, les cubes locaux et les modèles d'exploration de données.  
  
 À l'exception des cubes locaux et du traitement au moyen des liaisons hors ligne, cette option n'est pas prise en charge pour les bases de données multidimensionnelles.  
  
 **Par défaut** ou **Hériter**  
 La boîte de dialogue utilise **Par défaut** pour les options d’emprunt d’identité définies au niveau de la base de données et **Hériter** pour les options d’emprunt d’identité définies au niveau de la source de données.  
  
 **Sources de données - Option Hériter**  
  
 Au niveau de la source de données, **Hériter** spécifie qu’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit utiliser l’option d’emprunt d’identité de l’objet parent. Dans un modèle multidimensionnel, l'objet parent est la base de données d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Le choix de l’option **Hériter** vous permet de gérer de manière centralisée les paramètres d’emprunt d’identité pour cette source de données et d’autres sources qui font partie de la même base de données. Pour que cette option soit explicite, choisissez un nom et un mot de passe d'utilisateur Windows spécifiques à la base de données. Sinon, la combinaison de **Hériter** sur la source de données et de **Par défaut** sur la base de données équivaut à utiliser l’option de compte de service.  
  
 Pour spécifier un nom d'utilisateur et un mot de passe Windows au niveau de la base de données, procédez comme suit :  
  
1.  Cliquez avec le bouton droit sur [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et choisissez **Propriétés**.  
  
2.  Dans **Informations d’emprunt d’identité de la source de données**, spécifiez un nom et un mot de passe d’utilisateur Windows.  
  
3.  Cliquez avec le bouton droit sur chaque source de données et affichez ses propriétés pour vérifier que chacune utilise l’option **Hériter**.  
  
 Pour plus d’informations sur les paramètres par défaut au niveau de la base de données, consultez [Définir les propriétés de base de données multidimensionnelle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/set-multidimensional-database-properties-analysis-services.md).  
  
 **Bases de données - Option Par défaut**  

 Pour les bases de données multidimensionnelles, l’option **Par défaut** signifie que l’on utilise le compte de service et l’utilisateur actuel pour les opérations d’exploration de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une source de données &#40;SSAS Multidimensionnel&#41;](../../analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional.md)   
 [Définir les propriétés de Source de données & #40 ; SSAS multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/set-data-source-properties-ssas-multidimensional.md)   

  
  
