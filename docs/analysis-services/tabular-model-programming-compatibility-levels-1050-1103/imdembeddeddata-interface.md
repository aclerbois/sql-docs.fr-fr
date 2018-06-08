---
title: Interface de IMDEmbeddedData | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 203eae4b3660aaf5d1f2ed3a92ba844e88a518ff
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="imdembeddeddata-interface"></a>Interface de IMDEmbeddedData
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’interface IMDEmbeddedData est une interface publique utilisée pour gérer un incorporé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] base de données ou une base de données de modèle tabulaire. L’interface hérite de la **IPersistStream** interface. Elle permet les opérations suivantes :  
  
-   Ajouter un identificateur au flux de données incorporé dans le document conteneur.  
  
-   Définir l'URL du document contenant.  
  
-   Définit un indicateur pour indiquer si l'application d'incorporation est dans un environnement hébergé.  
  
-   Définit le chemin d'accès aux fichiers temporaires utilisés par l'application d'incorporation.  
  
-   Annuler l'opération incorporée actuelle.  
  
-   Obtenir la taille estimée (en octets) du flux de données pour enregistrer l'objet incorporé. Hérité de **IPersistStream**.  
  
-   Vérifier si la base de données incorporée a changé depuis le dernier enregistrement. Hérité de **IPersistStream**.  
  
-   Charger la base de données incorporée dans le moteur local ou in-process. Hérité de **IPersistStream**.  
  
-   Enregistrer la base de données locale ou in-process sur le flux de données incorporé dans le document conteneur. Hérité de **IPersistStream**.  
  
## <a name="reference"></a>Référence  
 La référence suivante documente la **IMDEmbeddedData** s’affiche dans l’interface **msmd.h** fichier d’en-tête.  
  
### <a name="source-file-pxoembeddeddataidl"></a>Fichier source : PXOEmbeddedData.idl  
  
```  
[  
  local,                            
  object,                           
  uuid(6B6691CF-5453-41c2-ADD9-4F320B7FD421),                       
  pointer_default(unique)           
]  
interface IMDEmbeddedData : IPersistStream  
{  
 [id(1), helpstring("Set flag indicating if the application is in a hosted environment")]   
 HRESULT SetHosted(  
  [in] BOOL in_fIsHosted);  
  
 [id(2), helpstring("Set the URL for the document containing the embedded stream")]   
 HRESULT SetContainerURL(  
  [in] BSTR in_bstrURL);  
  
 [id(3), helpstring("Get identifier used to look up embedded stream in container document")]   
 HRESULT GetStreamIdentifier(  
  [out, retval] BSTR* out_pbstrStreamId);  
  
 [id(4), helpstring("Set the path used by the embedding application for temporary files")]   
 HRESULT SetTempDirPath(  
  [in]  BSTR in_bstrPath);  
  
 [id(5), helpstring("Cancel the current operation")]   
 HRESULT Cancel();  
};  
```  
  
### <a name="imdembeddeddatagetstreamidentifier"></a>IMDEmbeddedData::GetStreamIdentifier  
  
```  
HRESULT GetStreamIdentifier (  
    [out, retval] BSTR * out_pbstrStreamId  
    )  
```  
  
#### <a name="description"></a> Description  
 Ajoute l'identificateur utilisé par l'application hôte au flux de données incorporé dans le document conteneur.  
  
#### <a name="parameters"></a>Paramètres  
 *out_pbstrStreamId*  
 Spécifie l'emplacement de l'identificateur de flux de données.  
  
#### <a name="return-value"></a>Valeur retournée  
 **S_OK**  
 L'identificateur de flux de données a été retourné avec succès.  
  
 **S_FALSE**  
 Il n'y a aucun identificateur de flux de données.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de l'accès à l'identificateur de flux de données.  
  
#### <a name="remarks"></a>Notes  
 Pour vérifier si la connexion actuelle contient une base de données incorporée, l'utilisateur doit vérifier la valeur de la propriété DBPROP_MSMD_EMBEDDED_DATA depuis les propriétés de connexion OLE DB.  
  
 Les valeurs possibles pour DBPROP_MSMD_EMBEDDED_DATA sont :  
  
|Nom|Valeur|Définition|  
|----------|-----------|----------------|  
|DBPROPVAL_EMBED_NONE|0x00|Aucune base de données incorporée disponible|  
|DBPROPVAL_EMBED_EMBEDDED|0x01|L'application actuelle contient la base de données incorporée|  
|DBPROPVAL_EMBED_LINKED|0x02|La base de données incorporée est hébergée dans une application distante (c.-à-d. SharePoint Server)|  
  
#### <a name="source"></a>Source  
  
```  
[id(1), helpstring("Get identifier used to look up embedded stream in container document")]   
 HRESULT GetStreamIdentifier(  
  [out, retval] BSTR* out_pbstrStreamId);  
```  
  
### <a name="imdembeddeddatasetcontainerurl"></a>IMDEmbeddedData::SetContainerURL  
  
```  
HRESULT SetContainerURL (  
    [in] BSTR in_bstrURL  
    )  
```  
  
#### <a name="description"></a> Description  
 Définit l'URL pour le fichier qui contient le flux de données incorporé.  
  
#### <a name="parameters"></a>Paramètres  
 *in_bstrURL*  
 Spécifie l'URL pour le document contenant.  
  
#### <a name="return-value"></a>Valeur retournée  
 **S_OK**  
 L'URL du conteneur a été définie avec succès.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de la définition de l'URL de conteneur.  
  
#### <a name="source"></a>Source  
  
```  
[id(2), helpstring("Set the URL for the document containing the embedded stream")]   
 HRESULT SetContainerURL(  
  [in] BSTR in_bstrURL);  
```  
  
### <a name="imdembeddeddatasethosted"></a>IMDEmbeddedData::SetHosted  
  
```  
HRESULT SetHosted (  
    [in] BOOL in_fIsHosted  
    )  
```  
  
#### <a name="description"></a> Description  
 Définit un indicateur pour indiquer si l'application d'incorporation est dans un environnement hébergé.  
  
#### <a name="parameters"></a>Paramètres  
 *in_ftHosted*  
 TRUE si l'appelant est hébergé dans une application de service (comme IIS).  
  
#### <a name="return-value"></a>Valeur retournée  
 **S_OK**  
 L'indicateur a été défini avec succès.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de la définition de l'indicateur.  
  
#### <a name="source"></a>Source  
  
```  
[id(5), helpstring("Set flag indicating if the application is in a hosted environment")]   
 HRESULT SetHosted(  
  [in]  BOOL in_fIsHosted);  
```  
  
### <a name="imdembeddeddatasettempdirpath"></a>IMDEmbeddedData::SetTempDirPath  
  
```  
HRESULT SetTempDirPath (  
    [in] BSTR in_bstrPath  
    )  
```  
  
#### <a name="description"></a> Description  
 Définit le chemin d'accès aux fichiers temporaires utilisés par l'application d'incorporation.  
  
#### <a name="parameters"></a>Paramètres  
 *in_bstrPath*  
 Chemin d'accès utilisé par l'application hôte pour les fichiers temporaires.  
  
#### <a name="return-value"></a>Valeur retournée  
 **S_OK**  
 Le répertoire de fichier temporaire a été défini avec succès.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de la définition du chemin d'accès.  
  
#### <a name="source"></a>Source  
  
```  
[id(4), helpstring("Set the path used by the host application for temporary files")]   
 HRESULT SetTempDirPath(  
  [in]  BSTR in_bstrPath);  
```  
  
### <a name="imdembeddeddatacancel"></a>IMDEmbeddedData::Cancel  
  
```  
HRESULT Cancel ( void )  
```  
  
#### <a name="description"></a> Description  
 Annule l'opération de base de données incorporée actuelle  
  
#### <a name="parameters"></a>Paramètres  
 Aucun.  
  
#### <a name="return-value"></a>Valeur retournée  
 **S_OK**  
 L'opération a été annulée avec succès.  
  
 **DB_E_CANTCANCEL**  
 Aucune opération annulable n'est actuellement en cours.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de l'annulation de l'opération incorporée.  
  
#### <a name="source"></a>Source  
  
```  
[id(5), helpstring("Cancel the current operation")]   
 HRESULT Cancel();  
```  
  
### <a name="imdembeddeddatagetsizemax-ipersiststreamgetsizemax"></a>IMDEmbeddedData::GetSizeMax (IPersistStream::GetSizeMax)  
  
```  
HRESULT GetSizeMax (  
    [out] ULARGE_INTEGER * out_pcbSize  
    )  
```  
  
#### <a name="description"></a> Description  
 Obtient la taille estimée (en octets) du flux de données pour enregistrer l'objet incorporé. Hérité de **IPersistStream**.  
  
#### <a name="parameters"></a>Paramètres  
 *in_bstrPath*  
 Taille estimée (en octets) de l'image de la base de données incorporée.  
  
#### <a name="return-value"></a>Valeur retournée  
 **S_OK**  
 La taille a été obtenue avec succès.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de l'obtention de la taille.  
  
### <a name="imdembeddeddataisdirty-ipersiststreamisdirty"></a>IMDEmbeddedData::IsDirty (IPersistStream::IsDirty)  
  
```  
HRESULT IsDirty ( void )  
```  
  
#### <a name="description"></a> Description  
 Vérifie si la base de données incorporée a changé depuis le dernier enregistrement. Hérité de **IPersistStream**.  
  
#### <a name="parameters"></a>Paramètres  
 aucun  
  
#### <a name="return-values"></a>Valeur(s) de retour  
 **S_OK**  
 La base de données a changé depuis le dernier enregistrement.  
  
 **S_FALSE**  
 La base de données n'a pas changé depuis le dernier enregistrement.  
  
 **E_FAIL**  
 Une erreur s'est produite lors de l'obtention de l'état de la base de données.  
  
### <a name="imdembeddeddataload-ipersiststreamload"></a>IMDEmbeddedData::Load (IPersistStream::Load)  
  
```  
HRESULT Load (   
    [in] IStream * in_pStm   
    )  
```  
  
#### <a name="description"></a> Description  
 Charge la base de données incorporée dans le moteur local ou in-process. Hérité de **IPersistStream**.  
  
#### <a name="parameters"></a>Paramètres  
 *in_pStm*  
 Pointeur vers une interface de flux de données à partir de laquelle charger la base de données incorporée.  
  
#### <a name="return-values"></a>Valeur(s) de retour  
 **S_OK**  
 La base de données a été chargée avec succès.  
  
 **E_OUTOFMEMORY**  
 Pas assez de mémoire pour charger la base de données.  
  
 **E_FAIL**  
 Une erreur s’est produite lors du chargement de la base de données différent de celui **E_OUTOFMEMORY**.  
  
### <a name="imdembeddeddatasave-ipersiststreamsave"></a>IMDEmbeddedData::Save (IPersistStream::Save)  
  
```  
HRESULT Save (   
    [in] IStream * in_pStm,  
    [in] BOOL in_fClearDirty  
    )  
```  
  
#### <a name="description"></a> Description  
 Enregistre la base de données locale ou in-process sur le flux de données incorporé dans le document conteneur. Hérité de **IPersistStream**.  
  
#### <a name="parameters"></a>Paramètres  
 *in_pStm*  
 Pointeur vers une interface de flux de données où enregistrer la base de données incorporée.  
  
 *in_fClearDirty*  
 Indicateur qui indique si l'indicateur de changement doit être effacé après cette opération.  
  
#### <a name="return-values"></a>Valeur(s) de retour  
 **S_OK**  
 La base de données a été enregistrée avec succès.  
  
 **STG_E_CANTSAVE**  
 Une erreur s’est produite lors de l’enregistrement de la base de données différent de celui **STG_E_MEDIUMFULL**.  
  
 **STG_E_MEDIUMFULL**  
 La base de données n'a pas pu être enregistrée parce qu'il n'y a plus d'espace disponible dans le périphérique de stockage.  
  
  
