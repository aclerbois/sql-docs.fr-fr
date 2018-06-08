---
title: GetReportServerUrls, méthode (WMI MSReportServer_Instance) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: wmi-provider-library-reference
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- GetReportServerUrls method
ms.assetid: 4865e32c-0114-465a-be8c-be223a7bc09e
caps.latest.revision: 12
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1cd4e616f22872f87d9df3fc9d624f5513e8b040
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msreportserverinstance-methods---getreportserverurls"></a>Méthodes MSReportServer_Instance - GetReportServerUrls
  Retourne la liste des URL que les utilisateurs peuvent employer pour accéder au serveur de rapports et au [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Sub GetReportServerUrls (ByRef ApplicationName() As String, ByRef URLs()_  
    As String, ByRef Length As Int32, ByRef HRESULT As Int32)  
```  
  
```csharp  
public void GetReportServerUrls(out string[] applicationName,   
    out string[] URLs, out int length, out int HRESULT);  
```  
  
## <a name="parameters"></a>Paramètres  
 *ApplicationName[]*  
 Tableau qui contient les applications installées. Les valeurs sont **ReportServerWebService** ou **ReportServerWebApp**.  
  
 *URLs[]*  
 Tableau qui contient les URL inscrites avec succès.  
  
 *Longueur*  
 Valeur entière qui contient la longueur des tableaux retournés.  
  
 *HRESULT*  
 Valeur qui indique le succès ou un code d'erreur.  
  
## <a name="return-values"></a>Valeurs de retour  
  
## <a name="remarks"></a>Notes   
 Les méthodes exposées par les objets de gestion WMI sont appelées par le biais de la fonction InvokeMethod. Pour plus d’informations, consultez la section relative à l’exécution des méthodes sur des objets de gestion dans la documentation WMI du [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework.  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspc](../../includes/ssrswminmspc-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
