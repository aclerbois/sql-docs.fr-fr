---
title: DatabaseLogonType, propriété (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: wmi-provider-library-reference
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- DatabaseLogonType
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- DatabaseLogonType property
ms.assetid: 6b592582-4c35-4029-ab86-982fff47d8d6
caps.latest.revision: 24
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2b8886f70f7422052467990135783c48b198ead2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configurationsetting-property---databaselogontype"></a>Propriété ConfigurationSetting - DatabaseLogonType
  Spécifie si le serveur de rapports utilise un compte de service [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, un compte d’utilisateur Windows ou un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour accéder à la base de données du serveur de rapports. En lecture seule.  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Dim DatabaseLogonType As Integer  
```  
  
```csharp  
public int DatabaseLogonType;  
```  
  
## <a name="property-values"></a>Valeurs de la propriété  
 Objet **entier** qui représente le type de connexion.  
  
## <a name="example-code"></a>Exemple de code  
 [Classe MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-class.md)  
  
## <a name="remarks"></a>Notes   
 Valeurs possibles :  
  
-   0 pour une connexion Windows  
  
-   1 pour une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   2 pour une connexion en tant que service.  
  
 Si vous spécifiez 0 (Windows), vous devez définir la valeur dans la propriété [DatabaseLogonAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogonaccount.md) en fonction d’un compte d’utilisateur Windows valide correspondant.  
  
 Si vous spécifiez 1 (SQL Server), assurez-vous que la valeur de la propriété [DatabaseLogonAccount](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-databaselogonaccount.md) correspond à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide.  
  
 Si vous spécifiez 2 (service Windows), le serveur de rapports utilise un compte [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] et le compte de service Windows pour accéder à la base de données du serveur de rapports. La propriété DatabaseLogonAccount est ignorée.  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
