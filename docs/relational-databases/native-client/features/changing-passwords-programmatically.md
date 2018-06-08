---
title: Modification des mots de passe par programmation | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], password expiration
- SQL Server Native Client ODBC driver, passwords
- SQL Server Native Client OLE DB provider, passwords
- passwords [SQL Server], expiration
- SQLNCLI, password expiration
- expiration [SQL Server Native Client]
- passwords [SQL Server], modifying
- expired passwords [SQL Server Native Client]
- SQL Server Native Client, password expiration
- modifying passwords
ms.assetid: 624ad949-5fed-4ce5-b319-878549f9487b
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: bd7d14e180a44f974e4d6ba89421caa8cbba3535
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="changing-passwords-programmatically"></a>Modification des mots de passe par programme
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Avant [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], lorsque le mot de passe d'un utilisateur expirait, seul un administrateur pouvait le réinitialiser. Commençant par [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client prend en charge la gestion d’expiration du mot de passe par programme, via le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif et le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client et via les modifications le **connexion SQL Server** boîtes de dialogue.  
  
> [!NOTE]  
>  Si possible, demandez aux utilisateurs de saisir leurs informations d'identification au moment de l'exécution et éviter de les stocker leurs références dans un format permanent. Si vous devez conserver leurs informations d’identification, chiffrez-les à l’aide de la [API de chiffrement Win32](http://go.microsoft.com/fwlink/?LinkId=64532). Pour plus d’informations sur l’utilisation de mots de passe, consultez [mots de passe forts](../../../relational-databases/security/strong-passwords.md).  
  
## <a name="sql-server-login-error-codes"></a>Codes d'erreur des connexions SQL Server  
 Lorsqu'une connexion ne peut pas être établie en raison de problèmes d'authentification, l'un des codes d'erreur SQL Server suivants est disponible pour aider l'application à établir le diagnostic et la récupération.  
  
|Code d'erreur SQL Server|Message d'erreur|  
|---------------------------|-------------------|  
|15113|La connexion a échoué pour l'utilisateur '%.*ls' Motif : échec de la validation du mot de passe. Le compte est verrouillé.|  
|18463|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : échec de changement de mot de passe. Impossible d'utiliser le mot de passe pour l'instant.|  
|18464|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : échec de changement de mot de passe. Ce mot de passe ne répond pas aux exigences de la stratégie, car il est trop court.|  
|18465|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : échec de changement de mot de passe. Ce mot de passe ne répond pas aux exigences de la stratégie, car il est trop long.|  
|18466|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : échec de changement de mot de passe. Ce mot de passe ne répond pas aux exigences de la stratégie, car il n'est pas assez complexe.|  
|18467|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : échec de changement de mot de passe. Le mot de passe ne répond pas aux exigences de la DLL de filtre de mots de passe.|  
|18468|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : échec de changement de mot de passe. Une erreur inattendue s'est produite lors de la validation de mot de passe.|  
|18487|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : le mot de passe associé à ce compte a expiré.|  
|18488|Échec de l'ouverture de session pour l'utilisateur '%.*ls'. Raison : le mot de passe du compte doit être changé.|  
  
## <a name="sql-server-native-client-ole-db-provider"></a>Fournisseur OLE DB SQL Server Native Client  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge l’expiration du mot de passe via une interface utilisateur et par programmation.  
  
### <a name="ole-db-user-interface-password-expiration"></a>Expiration du mot de passe de l'interface utilisateur OLE DB  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fournisseur OLE DB prend en charge l’expiration de mot de passe à travers les modifications apportées à la **connexion SQL Server** boîtes de dialogue. Si la valeur de DBPROP_INIT_PROMPT est définie sur DBPROMPT_NOPROMPT, la tentative de connexion initiale échoue si le mot de passe a expiré.  
  
 Si DBPROP_INIT_PROMPT a été défini sur une autre valeur, l’utilisateur voit le **connexion SQL Server** boîte de dialogue, quel que soit le mot de passe a expiré ou pas. L’utilisateur peut cliquer sur le **Options** bouton et vérifiez **modifier le mot de passe** pour modifier le mot de passe.  
  
 Si l’utilisateur clique sur OK et le mot de passe a expiré, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] invite l’utilisateur à entrer et à confirmer un nouveau mot de passe à l’aide du **mot de passe de modification SQL Server** boîte de dialogue.  
  
#### <a name="ole-db-prompt-behavior-and-locked-accounts"></a>Comportement d'invite OLE DB et comptes verrouillés  
 Les tentatives de connexion peuvent échouer en raison du compte verrouillé. Si cela se produit après l’affichage de la **connexion SQL Server** boîte de dialogue, le message d’erreur de serveur s’affiche à l’utilisateur et la tentative de connexion est abandonnée. Il peut également se produire après l’affichage de la **mot de passe de modification SQL Server** boîte de dialogue si l’utilisateur entre une valeur incorrecte pour l’ancien mot de passe. Dans ce cas le même message d'erreur s'affiche et la tentative de connexion est abandonnée.  
  
#### <a name="ole-db-connection-pooling-password-expiration-and-locked-accounts"></a>Regroupement de connexions OLE DB, expiration de mot de passe et comptes verrouillés  
 Un compte peut être verrouillé ou son mot de passe peut expirer pendant que la connexion est toujours active dans un pool de connexions. Le serveur contrôle les mots de passe périmés et les comptes verrouillés en deux occasions. La première occasion correspond au moment de la création de la connexion. La seconde occasion se produit lors de la réinitialisation de la connexion, quand la connexion est extraite du pool.  
  
 Quand la tentative de réinitialisation échoue, la connexion est supprimée du pool et une erreur est retournée.  
  
### <a name="ole-db-programmatic-password-expiration"></a>Expiration de mot de passe par programmation OLE DB  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge l’expiration du mot de passe via l’ajout de la propriété SSPROP_AUTH_OLD_PASSWORD (type VT_BSTR) qui a été ajoutée au jeu de propriétés DBPROPSET_SQLSERVERDBINIT.  
  
 La propriété « Mot de passe » existante se rapporte à DBPROP_AUTH_PASSWORD et est utilisée pour stocker le nouveau mot de passe.  
  
> [!NOTE]  
>  Dans la chaîne de connexion, la propriété « Ancien Mot de passe » définit SSPROP_AUTH_OLD_PASSWORD, qui correspond au mot de passe en cours (peut-être périmé) qui n'est pas disponible via une propriété de chaîne de fournisseur.  
  
 Le fournisseur ne rend pas la valeur de cette propriété persistante. Lorsque cette propriété est définie, le fournisseur n'utilise pas le pool de connexions de la première connexion, sinon une nouvelle connexion interviendra. Si la modification de mot de passe est réussie, la connexion actuelle ne peut pas être réutilisée, car elle contient toujours l'ancien mot de passe, qui ne sera pas valide après la modification. Également, si la connexion réussit, le fournisseur efface la propriété. Les tentatives suivantes d'extraction de l'ancien mot de passe retournent VT_EMPTY.  
  
> [!NOTE]  
>  SSPROP_AUTH_OLD_PASSWORD ne doit jamais être rendu persistant, car il n'est utilisé que lorsqu'un mot de passe a expiré.  
  
 Notez que chaque fois que la propriété « Ancien Mot de passe » propriété est définie, le fournisseur suppose qu'une tentative de modification du mot de passe a lieu, à moins que l'authentification Windows ne soit également spécifiée, dans quel cas elle est toujours prioritaire.  
  
 Si l’authentification Windows est utilisée, en spécifiant les résultats d’un mot de passe ancien DB_E_ERRORSOCCURRED ou DB_S_ERRORSOCCURRED, selon si l’ancien mot de passe a été spécifié comme REQUIRED ou OPTIONAL, respectivement, et la valeur d’état de DBPROPSTATUS_CONFLICTINGBADVALUE est retournée dans *dwStatus*. Cette exception est détectée lorsque **IDBInitialize::Initialize** est appelée.  
  
 Si une tentative de modifier le mot de passe échoue de façon inattendue, le serveur retourne le code d'erreur 18468. Une erreur OLEDB standard est retournée à partir de la tentative de connexion.  
  
 Pour plus d’informations sur le jeu de propriétés DBPROPSET_SQLSERVERDBINIT, consultez [propriétés d’initialisation et d’autorisation](../../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).  
  
## <a name="sql-server-native-client-odbc-driver"></a>Pilote ODBC SQL Server Native Client  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge l’expiration du mot de passe via une interface utilisateur et par programmation.  
  
### <a name="odbc-user-interface-password-expiration"></a>Expiration du mot de passe de l'interface utilisateur ODBC  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client prend en charge l’expiration de mot de passe à travers les modifications apportées à la **connexion SQL Server** boîtes de dialogue.  
  
 Si [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md) est appelée et la valeur de **DriverCompletion** a la valeur SQL_DRIVER_NOPROMPT, la tentative de connexion initiale échoue si le mot de passe a expiré. La valeur SQLSTATE 28000 et la valeur de code d’erreur native 18487 sont retournées par les appels suivants à **SQLError** ou **SQLGetDiagRec**.  
  
 Si **DriverCompletion** a été défini sur une autre valeur, l’utilisateur voit le **connexion SQL Server** boîte de dialogue, quel que soit le mot de passe a expiré ou pas. L’utilisateur peut cliquer sur le **Options** bouton et vérifiez **modifier le mot de passe** pour modifier le mot de passe.  
  
 Si l’utilisateur clique sur OK et le mot de passe a expiré, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] invites pour entrer et à confirmer un nouveau mot de passe à l’aide du **mot de passe de modification SQL Server** boîte de dialogue.  
  
#### <a name="odbc-prompt-behavior-and-locked-accounts"></a>Comportement d'invite ODBC et comptes verrouillés  
 Les tentatives de connexion peuvent échouer en raison du compte verrouillé. Si cela se produit après l’affichage de la **connexion SQL Server** boîte de dialogue, le message d’erreur de serveur s’affiche à l’utilisateur et la tentative de connexion est abandonnée. Il peut également se produire après l’affichage de la **mot de passe de modification SQL Server** boîte de dialogue si l’utilisateur entre une valeur incorrecte pour l’ancien mot de passe. Dans ce cas le même message d'erreur s'affiche et la tentative de connexion est abandonnée.  
  
#### <a name="odbc-connection-pooling-password-expiry-and-locked-accounts"></a>Regroupement de connexions ODBC, expiration de mot de passe et comptes verrouillés  
 Un compte peut être verrouillé ou son mot de passe peut expirer pendant que la connexion est toujours active dans un pool de connexions. Le serveur contrôle les mots de passe périmés et les comptes verrouillés en deux occasions. La première occasion correspond au moment de la création de la connexion. La seconde occasion se produit lors de la réinitialisation de la connexion, quand la connexion est extraite du pool.  
  
 Quand la tentative de réinitialisation échoue, la connexion est supprimée du pool et une erreur est retournée.  
  
### <a name="odbc-programmatic-password-expiration"></a>Expiration de mot de passe par programmation ODBC  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client prend en charge l’expiration du mot de passe via l’ajout de l’attribut SQL_COPT_SS_OLDPWD défini avant de se connecter au serveur en utilisant la [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) (fonction).  
  
 L'attribut SQL_COPT_SS_OLDPWD du handle de connexion fait référence au mot de passe périmé. Il n'existe aucun attribut de chaîne de connexion pour cet attribut, car il entraînerait une interférence avec le regroupement de connexions. Si la connexion réussit, le pilote efface cet attribut.  
  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client retourne SQL_ERROR dans quatre cas pour cette fonctionnalité : expiration du mot de passe, conflit de stratégie de mot de passe, le verrouillage de compte, et lorsque l’ancienne propriété de mot de passe est définie lors de l’utilisation de l’authentification Windows. Le pilote retourne les messages d’erreur approprié à l’utilisateur lorsque [SQLGetDiagField](../../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md) est appelé.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
