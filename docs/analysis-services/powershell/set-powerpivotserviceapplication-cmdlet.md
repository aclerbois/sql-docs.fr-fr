---
title: Applet de commande Set-PowerPivotServiceApplication | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a0951f53451141ead27cc3d7aa5f1346dbb47694
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="set-powerpivotserviceapplication-cmdlet"></a>Applet de commande Set-PowerPivotServiceApplication
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Définit les propriétés d’une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
 **S'applique à :** SharePoint 2010 et SharePoint 2013.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Set-PowerPivotServiceApplication [-Identity] <SPGeminiServiceApplicationPipeBind> [-AdministrationConnectionPoolSize <int>] [-AllowCustomWindowsCredentials] [-BusinessHoursEndTime <string>] [-BusinessHoursStartTime <string>] [-CachedDatabaseholdLimit <int>] [-Confirm <switch>] [-ConnectionPoolSize <int>] [-ConnectionPoolTimeout <int>] [-DataLoadTimeout <int>] [-DataRefreshFailureThreshold <int>] [-DataRefreshInactiveWorkbooksThreshold <int>] [-DataRefreshMaxHistory <int>] [-HealthBasedAllocation <switch>] [-LoadsToConnectionsRatioCollectionInterval <int>] [-LoadsToConnectionsRatioLimit <int>] [-MemoryDatabaseHoldLimit <int>] [-QueryReportingInterval <int>] [-RoundRobinAllocation <switch>] [-UnattendedAccount <string>] [-UsageDataRetentionPeriod <int>] [-UsageExpectedResponseUpperLimit <int>] [-UsageLongResponseUpperLimit <int>] [-UsageQuickResponseUpperLimit <int>] [-UsageTrivialResponseUpperLimit <int>] [-UsageUpdateDayLimit <int>] [<CommonParameters>]  
```  
  
## <a name="description"></a> Description  
 L’applet de commande Set-PowerPivotServiceApplication met à jour les propriétés d’une application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans la batterie de serveurs. Le paramètre Identity est obligatoire. Vous devez fournir le GUID de l'application de service dont vous mettez à jour les propriétés.  
  
 Pour vérifier vos modifications, exécutez l’applet de commande suivante : Get-PowerPivotServiceApplication-Identity \<GUID > | format-list.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-identity-spgeminiserviceapplicationpipebind"></a>-Identité \<SPGeminiServiceApplicationPipeBind >  
 Spécifie l'application de service à mettre à jour. Le type doit être un GUID valide ou une instance d’un objet d’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] valide. Vous pouvez utiliser Get-PowerPivotServiceApplication pour retourner une instance de l'objet.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-administrationconnectionpoolsize-int"></a>-AdministrationConnectionPoolSize \<int >  
 Spécifie le nombre de connexions ouvertes dans un pool de connexions créé pour une connexion du service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] à Analysis Services. Chaque instance de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ouvre une connexion d’administration distincte à une instance Analysis Services sur le même ordinateur. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] crée un pool distinct pour réutiliser les connexions d'administration en vue de vérifier si des connexions sont inactives et de contrôler l'intégrité du serveur. La valeur par défaut est 200 connexions. Les valeurs valides sont -1 (illimité), 0 (désactive le regroupement de connexions administrateur) ou 1 à 10000. Si vous sélectionnez 0, chaque connexion est recréée.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|200|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-allowcustomwindowscredentials-switchparameter"></a>-AllowCustomWindowsCredentials [\<Paramètre_booléen >]  
 Spécifie si les propriétaires de la planification peuvent entrer des informations d'identification Windows arbitraires pour exécuter une planification d'actualisation des données. Si vous activez cette case à cocher, l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] crée et gère une application cible pour chaque ensemble d’informations d’identification stockées. La valeur par défaut est true. Pour désactiver cette fonctionnalité, définissez AllowCustomWindowsCredentials:$false.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-businesshoursendtime-string"></a>-BusinessHoursEndTime \<chaîne >  
 Spécifie le point de fin d'une plage horaire qui définit une journée de travail. Les planifications d'actualisation des données peuvent s'exécuter à la fin d'une journée de travail pour récupérer les données transactionnelles qui ont été générées pendant les heures d'ouverture normales. La valeur par défaut est 8:00 p.m.  Les valeurs valides sont spécifiées entre guillemets, avec l'indication a.m. (pour le matin) ou p.m. (pour l'après-midi) (par exemple, « 08:00 PM »). Les heures doivent être comprises entre 1 et 12. Les minutes doivent être comprises entre 1 et 59.  
  
 Pour spécifier la plage complète des heures de travail, vous devez définir BusinessHoursStartTime et BusinessHoursEndTime. Les deux paramètres définissent l'intervalle horaire qui compose une journée de travail.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|8 PM|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-businesshoursstarttime-string"></a>-BusinessHoursStartTime \<chaîne >  
 Spécifie le point de début d'une plage horaire qui définit une journée de travail. Les planifications d'actualisation des données peuvent s'exécuter à la fin d'une journée de travail pour récupérer les données transactionnelles qui ont été générées pendant les heures d'ouverture normales. La valeur par défaut est 4:00 a.m.  Les valeurs valides sont spécifiées entre guillemets, avec l'indication a.m. (pour le matin) ou p.m. (pour l'après-midi) (par exemple, « 04:00AM »). Les heures doivent être comprises entre 1 et 12. Les minutes doivent être comprises entre 1 et 59.  
  
 Pour spécifier la plage complète des heures de travail, vous devez définir BusinessHoursStartTime et BusinessHoursEndTime. Les deux paramètres définissent l'intervalle horaire qui compose une journée de travail.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|4 AM|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-cacheddatabaseholdlimit-int"></a>-CachedDatabaseholdLimit \<int >  
 Spécifie la durée (en heures) pendant laquelle une base de données inactive est conservée dans le système de fichiers après avoir été déchargée de la mémoire. La valeur par défaut est 120 heures. Le travail de nettoyage utilise ce paramètre pour déterminer les fichiers à supprimer. Toutes les bases de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui sont inactives pendant 168 heures (48 heures en mémoire et 120 heures dans le cache) sont supprimées du disque par le travail de nettoyage.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|120|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-confirm-switch"></a>-Confirmer \<commutateur >  
 Demande une confirmation avant d'exécuter la commande. Cette valeur est activée par défaut. Pour contourner la réponse de confirmation dans une commande, spécifiez Confirm:$false sur la commande.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-connectionpoolsize-int"></a>-ConnectionPoolSize \<int >  
 Spécifie le nombre maximal de connexions inactives créées par le service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans les pools de connexions pour chaque combinaison d’utilisateur SharePoint, de jeu de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et de version. La valeur par défaut est 1000 connexions inactives. Les valeurs valides sont -1 (illimité), 0 (désactive le regroupement de connexions utilisateur) ou une plage comprise entre 1 et 10000. Ces pools de connexions permettent au service de gérer plus efficacement les connexions aux mêmes données en lecture seule par le même utilisateur. Si vous désactivez le regroupement de connexions, chaque connexion sera recréée. Notez que la modification de la limite de taille du pool de connexions, y compris son paramétrage sur la valeur 0, ne provoque pas de perte de connexion. Les pools de connexions servent à réduire les temps d'attente lors de la connexion aux données. Le service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne refuse jamais une connexion fondée sur les paramètres du pool de connexions.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|1000|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-connectionpooltimeout-int"></a>-ConnectionPoolTimeout \<int >  
 Spécifie la durée (en minutes) pendant laquelle une connexion de données inactive restera ouverte. La valeur par défaut est 1800 secondes (ou 30 minutes). Pendant ce laps de temps, l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] réutilise une connexion de données inactive pour les requêtes en lecture seule émanant du même utilisateur SharePoint pour les mêmes données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Si aucune demande supplémentaire n'est reçue pour ces données pendant la période spécifiée, la connexion est supprimée du pool. Les valeurs valides sont comprises entre 1 et 3600 secondes.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|1800|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-dataloadtimeout-int"></a>-DataLoadTimeout \<int >  
 Spécifie le délai pendant lequel l’application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] attend une réponse de l’instance de SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) à laquelle elle a transféré une requête de chargement de données. Étant donné que l'acheminement des jeux de données très volumineux prend du temps, vous devez accorder à l'instance de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] un temps suffisant pour récupérer le classeur Excel et déplacer les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] vers une instance d’Analysis Services en vue du traitement de la demande. La valeur par défaut est 1800 secondes (ou 30 minutes). Les valeurs valides sont comprises entre 1 et 3 600 secondes.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|1800|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-datarefreshfailurethreshold-int"></a>-DataRefreshFailureThreshold \<int >  
 Spécifie le nombre d'échecs consécutifs à l'issue duquel une planification est désactivée. La valeur par défaut est 10. Vous pouvez également entrer 0 pour ne jamais désactiver une planification suite à un échec d'actualisation.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|10|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-datarefreshinactiveworkbooksthreshold-int"></a>-DataRefreshInactiveWorkbooksThreshold \<int >  
 Spécifiez le nombre de cycles d'actualisation des données à l'issue duquel une planification est désactivée, ou entrez 0 pour ne jamais désactiver une planification en raison d'une inactivité. La valeur par défaut est 10 cycles.  
  
 L'inactivité des classeurs est mesurée comme l'absence d'événements de connexion sur plusieurs cycles d'actualisation des données. Un cycle d'actualisation des données est compté chaque fois qu'une opération d'actualisation des données est déclenchée (par la planification ou par une opération Exécuter maintenant), indépendamment de la réussite ou de l'échec de l'opération. Si plusieurs cycles passent (10 par défaut) sans qu'il y ait de demande de connexion pour le classeur, la planification de l'actualisation des données est désactivée en raison d'une inactivité.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|10|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-datarefreshmaxhistory-int"></a>-DataRefreshMaxHistory \<int >  
 Spécifie la durée de rétention d'un enregistrement d'historique du traitement de l'actualisation des données. Ces informations s'affichent dans les pages d'historique de l'actualisation des données qui sont conservées pour chaque classeur utilisant l'actualisation des données. Elles s’affichent également dans le tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . La valeur par défaut est 365 jours.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|365|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-healthbasedallocation-switch"></a>-HealthBasedAllocation \<commutateur >  
 Spécifie l’algorithme d’allocation basé sur l’intégrité, qui transfère les requêtes de connexion au serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint qui a le plus de ressources processeur et mémoire disponibles. Il s'agit de l'algorithme d'allocation par défaut. HealthBasedAllocation et RoundRobinBasedAllocation s'excluent mutuellement. Vous devez spécifier l'un ou l'autre. Si vous leur attribuez à tous deux la valeur false, HealthBasedAllocation sera utilisé car il s'agit de la valeur par défaut. Si vous leur attribuez à tous deux la valeur true, vous obtiendrez une erreur de validation. La syntaxe de ces paramètres inclut soit la saisie du nom du paramètre seul, soit la saisie de parameter:$true ou de parameter:$false.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-loadstoconnectionsratiocollectioninterval-int"></a>-LoadsToConnectionsRatioCollectionInterval \<int >  
 Spécifiez l'intervalle (en heures) pendant lequel calculer les événements de charge et de connexion pour le calcul du rapport charge/connexion. Par défaut, le système calcule un nouveau taux toutes les 4 heures. Les valeurs valides sont comprises entre 1 et 24.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|4|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-loadstoconnectionsratiolimit-int"></a>-LoadsToConnectionsRatioLimit \<int >  
 Spécifie le rapport d'événements de calcul par rapport aux événements de connexion qui est utilisé en tant qu'indicateur de l'intégrité du serveur. La valeur par défaut est 20 pour cent.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|20|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-memorydatabaseholdlimit-int"></a>-MemoryDatabaseHoldLimit \<int >  
 Spécifie le nombre d'heures pendant lequel une base de données inactive est conservée en mémoire pour répondre aux nouvelles requêtes de données. Une base de données active est toujours conservée en mémoire tant que vous l'interrogez ; lorsqu'elle n'est plus active, le système la conserve en mémoire pour une période supplémentaire au cas où ces données seraient interrogées ultérieurement. L'intervalle par défaut est de 48 heures.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|48|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-queryreportinginterval-int"></a>-QueryReportingInterval \<int >  
 Spécifie la durée en secondes pendant laquelle collecter les statistiques de réponse aux requêtes avant de les rapporter comme événement d'utilisation. La valeur par défaut est 300 secondes.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|300|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-roundrobinallocation-switch"></a>-RoundRobinAllocation \<commutateur >  
 Spécifie l’algorithme d’allocation par tourniquet (round robin) qui transfère les demandes de connexion au serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint suivant, en répartissant les demandes de manière égale entre les serveurs disponibles, quelle que soit la charge du serveur. HealthBasedAllocation et RoundRobinBasedAllocation s'excluent mutuellement. Vous devez spécifier l'un ou l'autre. Si vous leur attribuez à tous deux la valeur false, HealthBasedAllocation sera utilisé car il s'agit de la valeur par défaut. Si vous leur attribuez à tous deux la valeur true, vous obtiendrez une erreur de validation. La syntaxe de ces paramètres inclut soit la saisie du nom du paramètre seul, soit la saisie de parameter:$true ou de parameter:$false.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-unattendedaccount-string"></a>-UnattendedAccount \<chaîne >  
 Spécifie le nom d’une application Service Banque d’informations sécurisé, qui stocke un compte prédéfini pour exécuter des travaux d’actualisation de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-usagedataretentionperiod-int"></a>-UsageDataRetentionPeriod \<int >  
 Spécifie le nombre de jours pendant lequel l'historique des données d'utilisation et des statistiques d'intégrité du serveur est conservé. La valeur par défaut est 365 jours. La valeur 0 conserve tous les historiques indéfiniment.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|365|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-usageexpectedresponseupperlimit-int"></a>-UsageExpectedResponseUpperLimit \<int >  
 Définit une limite maximale qui définit un échange requête-réponse attendue. La valeur par défaut est 3000 millisecondes. Toute requête dont la durée d'exécution est comprise entre 1000 et 3000 millisecondes est considérée comme une réponse attendue à des fins de création de rapports.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|3000|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-usagelongresponseupperlimit-int"></a>-UsageLongResponseUpperLimit \<int >  
 Définit une limite maximale qui définit un échange requête-réponse long.  La limite maximale est de 10 000 millisecondes. Toute demande qui dépasse cette limite est classée dans la catégorie Hors limite pour laquelle il n'existe pas de seuil maximal.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|10000|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-usagequickresponseupperlimit-int"></a>-UsageQuickResponseUpperLimit \<int >  
 Définit une limite maximale qui définit un échange requête-réponse rapide. La valeur par défaut est 1 000 millisecondes. Toute requête dont la durée d'exécution est comprise entre 500 et 1000 millisecondes est considérée comme une réponse rapide à des fins de création de rapports.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|1000|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-usagetrivialresponseupperlimit-int"></a>-UsageTrivialResponseUpperLimit \<int >  
 Spécifie une catégorie de temps de réponse trop courts pour être considérés comme pertinents pour la collecte de données. La plupart des réponses faisant partie de cette catégorie sont des communications de serveur à serveur. Par défaut, cette valeur est de 500 millisecondes. Toute demande dont la durée d'exécution est comprise entre 0 et 500 millisecondes est une demande triviale ; elle est ignorée à des fins de création de rapports.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|500|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-usageupdatedaylimit-int"></a>-UsageUpdateDayLimit \<int >  
 Spécifiez le seuil (en jours) de déclenchement d’un avertissement en cas d’échec d’actualisation du fichier de données utilisé par les rapports du tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Par défaut, le système met quotidiennement à jour les données d'utilisation. Le fichier [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Management Dashboard.xlsx, qui est la source de données pour les rapports d’administration, est actualisé selon la même planification. Si le fichier .xlsx n'est pas mis à jour pendant un nombre de jours donné, une règle d'intégrité est générée pour indiquer que le fichier est obsolète. La valeur par défaut est 5 jours. Les valeurs valides sont comprises entre 1 et 30.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|5|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 Cette applet de commande prend en charge les paramètres communs : Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer et OutVariable. Pour plus d’informations, consultez [About_Commonparameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|Aucun.|  
|Sorties|Aucun.|  
  
## <a name="example-1"></a>Exemple 1  
  
```  
C:\PS>Set-PowerPivotServiceApplication -identity 1234567-890a-bcde-fghijklm -AllowCustomWindowsCredentials:$false -UnattendedAccount "MyTargetApp"  
```  
  
 Cet exemple désactive une fonctionnalité d'actualisation des données qui crée et gère automatiquement les applications cibles de service Banque d'informations sécurisé pour le stockage des informations d'identification Windows arbitraires. Il définit également le compte d’actualisation des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sans assistance sur une application cible prédéfinie.  
  
 Utilisez Get-powerpivotserviceapplication pour obtenir une identité valide.  
  
## <a name="example-2"></a>Exemple 2  
  
```  
C:\PS>Set-PowerPivotServiceApplication -identity 1234567-890a-bcde-fghijklm -HealthBasedAllocation  
```  
  
 Cet exemple spécifie l'algorithme d'allocation basé sur l'intégrité qui transfère les requêtes de connexion au serveur qui a le plus de ressources disponibles.  
  
 Utilisez Get-powerpivotserviceapplication pour obtenir une identité valide.  
  
## <a name="example-3"></a>Exemple 3  
  
```  
C:\PS>Set-PowerPivotServiceApplication -identity 1234567-890a-bcde-fghijklmn -BusinessHoursStartTime "07:15AM" -BusinessHoursEndTime "08:00PM"  
```  
  
 Cet exemple montre comment définir les heures de début et de fin d’une journée de travail, utilisées pour planifier l’actualisation des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Les planifications peuvent spécifier une option après les heures de travail pour l'exécution de l'actualisation des données à la fin d'une journée de travail.  
  
 Utilisez Get-powerpivotserviceapplication pour obtenir une identité valide.  
  
  
