---
title: ListReportServersInDatabase, méthode (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: wmi-provider-library-reference
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- ListReportServersInDatabase (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- ListReportServersInDatabase method
ms.assetid: a4bf5968-c46f-484f-a510-65e2dde65a0d
caps.latest.revision: 18
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: dfb91a3cc4326f1fa52661002b2a9d8c69c6e52a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configurationsetting-method---listreportserversindatabase"></a>Méthode ConfigurationSetting - ListReportServersInDatabase
  Retourne la liste des installations du serveur de rapports qui sont présentes dans la base de données du serveur de rapports, même si elles n'ont pas accès aux informations sécurisées.  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Sub ListReportServersInDatabase(ByRef MachineNames() As String, _  
    ByRef InstanceNames() As String, ByRef InstallationIDs() As String, _  
    ByRef IsInitialized() As Boolean, ByRef Length As Int32, _  
    ByRef HRESULT As Int32, ByRef ExtendedErrors() As String)  
```  
  
```csharp  
public void ListReportServersInDatabase (out string[] MachineNames,   
    out string[] InstanceNames, out string[] InstallationIDs,   
    out Boolean[] IsInitialized,out Int32 Length, out Int32 HRESULT,    
    out string[] ExtendedErrors);  
```  
  
## <a name="parameters"></a>Paramètres  
 *MachineNames[]*  
 [out] Tableau contenant les noms de machines des installations du serveur de rapports dans la base de données.  
  
 *InstanceNames[]*  
 [out] Tableau contenant les noms d'instances de chacune des installations du serveur de rapports dans la base de données.  
  
 *InstallationIDs[]*  
 [out] Tableau contenant l'ID d'installation de chaque installation du serveur de rapports dans la base de données.  
  
 *IsInitialized[]*  
 [out] Tableau contenant l'état d'initialisation de chaque installation du serveur de rapports dans la base de données.  
  
 *Longueur*  
 [out] Longueur des tableaux retournés par la méthode. Tous les tableaux retournés ont la même longueur.  
  
 *HRESULT*  
 [out] Valeur indiquant si l'appel a réussi ou échoué.  
  
 *ExtendedErrors[]*  
 [out] Tableau de chaînes contenant les erreurs supplémentaires retournées par l'appel.  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un paramètre *HRESULT* qui indique si l'appel de la méthode a réussi ou a échoué. Une valeur 0 indique que l'appel de méthode a réussi. Une valeur différente de zéro indique qu'une erreur s'est produite.  
  
## <a name="remarks"></a>Notes   
 ListReportServersInDatabase répertorie les installations du serveur de rapports qui sont présentes dans la base de données du serveur de rapports, qu’elles aient accès ou non aux informations sécurisées, et retourne un ensemble de tableaux correspondants qui contiennent des informations à propos de chaque installation.  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
