---
title: Écrire votre propre gestionnaire personnalisé | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory handler in RDS [ADO]
- customized handler in RDS [ADO]
ms.assetid: d447712a-e123-47b5-a3a4-5d366cfe8d72
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b780e2027e64f7832fd622e66e1d908696d24b0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="writing-your-own-customized-handler"></a>Écrire votre propre gestionnaire personnalisé
Vous pouvez souhaiter écrire votre propre gestionnaire si vous êtes un administrateur de serveur IIS qui souhaite la valeur par défaut prend en charge les services Bureau à distance, mais plus de contrôle sur les demandes d’utilisateur et les droits d’accès.  
  
 Le gestionnaire MSDFMAP. Gestionnaire implémente la **IDataFactoryHandler** interface.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="idatafactoryhandler-interface"></a>String  
 Cette interface possède deux méthodes, **GetRecordset** et **reconnexion**. Les deux méthodes exigent que la [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété être définie sur **adUseClient**.  
  
 Ces deux méthodes acceptent les arguments qui apparaissent après la première virgule dans le «**gestionnaire =**» (mot clé). Par exemple, `"Handler=progid,arg1,arg2;"` passe une chaîne d’argument `"arg1,arg2"`, et `"Handler=progid"` un argument null.  
  
## <a name="getrecordset-method"></a>Méthode GetRecordset  
 Cette méthode interroge la source de données et crée un nouveau [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) de l’objet à l’aide des arguments fournis. Le **Recordset** doit être ouvert avec **adLockBatchOptimistic** et ne doit pas être ouvert de façon asynchrone.  
  
### <a name="arguments"></a>Arguments  
 ***conn*** la chaîne de connexion.  
  
 ***args*** les arguments pour le gestionnaire.  
  
 ***requête*** le texte de commande pour effectuer une requête.  
  
 ***ppRS*** le pointeur sur lequel le **Recordset** doit être retournée.  
  
## <a name="reconnect-method"></a>Se reconnecter (méthode)  
 Cette méthode met à jour la source de données. Il crée un nouveau [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet et attache la donnée **Recordset**.  
  
### <a name="arguments"></a>Arguments  
 ***conn*** la chaîne de connexion.  
  
 ***args*** les arguments pour le gestionnaire.  
  
 ***réservations persistantes*** A **Recordset** objet.  
  
## <a name="msdfhdlidl"></a>msdfhdl.idl  
 Il s’agit de la définition d’interface **IDataFactoryHandler** qui s’affiche dans le **msdfhdl.idl** fichier.  
  
```  
[  
  uuid(D80DE8B3-0001-11d1-91E6-00C04FBBBFB3),  
  version(1.0)  
]  
library MSDFHDL  
{  
    importlib("stdole32.tlb");  
    importlib("stdole2.tlb");  
  
    // TLib : Microsoft ActiveX Data Objects 2.0 Library  
    // {00000200-0000-0010-8000-00AA006D2EA4}  
    #ifdef IMPLIB  
    importlib("implib\\x86\\release\\ado\\msado15.dll");  
    #else  
    importlib("msado20.dll");  
    #endif  
  
    [  
      odl,  
      uuid(D80DE8B5-0001-11d1-91E6-00C04FBBBFB3),  
      version(1.0)  
    ]  
    interface IDataFactoryHandler : IUnknown  
    {  
HRESULT _stdcall GetRecordset(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] BSTR query,  
      [out, retval] _Recordset **ppRS);  
  
// DataFactory will use the ActiveConnection property  
// on the Recordset after calling Reconnect.  
   HRESULT _stdcall Reconnect(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] _Recordset *pRS);  
    };  
};  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Section Connect du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-connect-section.md)   
 [Section de personnalisation de fichiers journaux](../../../ado/guide/remote-data-service/customization-file-logs-section.md)   
 [Section de personnalisation de fichier SQL](../../../ado/guide/remote-data-service/customization-file-sql-section.md)   
 [Section du fichier de personnalisation UserList](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres Client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Présentation du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)


