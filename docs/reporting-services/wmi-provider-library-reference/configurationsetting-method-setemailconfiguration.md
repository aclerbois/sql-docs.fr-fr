---
title: SetEmailConfiguration, méthode (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
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
- SetEmailConfiguration (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- SetEmailConfiguration method
ms.assetid: b40a2224-2c90-4d32-892f-1fe73a0591ca
caps.latest.revision: 19
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: bea00d4571559e43e1127ddad41fc194fcdead10
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configurationsetting-method---setemailconfiguration"></a>Méthode ConfigurationSetting - SetEmailConfiguration
  Configure l'extension de remise par messagerie utilisée par le serveur de rapports pour envoyer des messages électroniques.  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Sub SetEmailConfiguration(ByVal SendUsingSMTPServer As Boolean, _  
    ByVal SMTPServer As String, ByVal SenderEmailAddress As String, _  
    ByRef HRESULT As Int32)  
```  
  
```csharp  
public void SetEmailConfiguration (Boolean SendUsingSMTPServer,   
   string SMTPServer, string SenderEmailAddress,   
   out Int32 HRESULT);  
```  
  
## <a name="parameters"></a>Paramètres  
 *SendUsingSMTPServer*  
 Valeur booléenne indiquant si le serveur doit utiliser le serveur SMTP pour envoyer le courrier électronique. Cette valeur peut uniquement être True. La valeur par défaut est false.  
  
 *SMTPServer*  
 Chaîne contenant le nom ou l'adresse IP d'un serveur SMTP.  
  
 *SenderEmailAddress*  
 Adresse de messagerie utilisée dans le champ 'De :' pour les messages électroniques envoyés depuis le serveur de rapports.  
  
 *HRESULT*  
 [out] Valeur indiquant si l'appel a réussi ou échoué.  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un paramètre *HRESULT* qui indique si l'appel de la méthode a réussi ou a échoué. Une valeur 0 indique que l'appel de méthode a réussi. Une valeur différente de zéro indique qu'une erreur s'est produite.  
  
## <a name="remarks"></a>Notes   
 Quand le paramètre *SendUsingSMTPServer* a la valeur **true**, l’entrée **SendUsing** dans le fichier de configuration du serveur de rapports a la valeur 1. Si *SendUsingSMTPServer* a la valeur **false**, l’entrée **SendUsing** n’est pas configurée.  
  
 Avec cette méthode, les utilisateurs ne peuvent pas configurer l’entrée **SendUsing** dans le fichier de configuration du serveur de rapports à une autre valeur que 1. Pour configurer le serveur de rapports pour une fonctionnalité autre que le courrier SMTP, vous devez modifier le fichier de configuration manuellement.  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
