---
title: Problèmes de conception de sécurité ADO | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ADO, security
ms.assetid: 86b83a38-efdf-4831-a6d5-7e470d517d1c
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f6b8cf26515276ce4dd9338d64746a2b7a017d99
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-security-design-features"></a>Fonctionnalités de conception de sécurité ADO
Les sections suivantes décrivent les fonctionnalités de conception de sécurité dans ActiveX Data Objects (ADO) 2.8 et versions ultérieures. Ces modifications ont été apportées dans ADO 2.8 pour améliorer la sécurité. ADO 6.0, qui est inclus dans Windows DAC 6.0 sous Windows Vista, est fonctionnellement équivalente à ADO 2.8, qui a été inclus dans MDAC 2.8 dans Windows XP et Windows Server 2003. Cette rubrique fournit des informations sur la façon de mieux sécuriser vos applications dans ADO 2.8 ou version ultérieure.

> [!IMPORTANT]
>  Si vous mettez à jour votre application à partir d’une version antérieure de l’objet ADO, il est recommandé de tester votre application mis à jour sur un ordinateur hors production avant de le déployer pour les clients. De cette manière, vous pouvez vous assurer que vous êtes conscient des problèmes de compatibilité avant de déployer votre application mis à jour.

## <a name="internet-explorer-file-access-scenarios"></a>Scénarios d’accès fichier Internet Explorer
 L’effet de fonctionnalités suivant le fonctionnement d’ADO 2.8 et versions ultérieur lorsqu’elle est utilisée dans un script des pages Web dans Internet Explorer.

### <a name="revised-and-improved-security-warning-message-box-now-used-to-alert-users"></a>Boîte de message d’avertissement de sécurité améliorées révisée maintenant utilisé pour avertir les utilisateurs
 Pour ADO 2.7 et versions antérieur, le message d’avertissement suivant s’affiche lorsqu’une page Web sous forme de script essaie d’exécuter le code ADO à partir d’un fournisseur non approuvé :

```
This page accesses data on another domain. Do you want to allow this? To
avoid this message in Internet Explorer, you can add a secure Web site to
your Trusted Sites zone on the Security tab of the Internet Options dialog
box.
```

 Pour ADO 2.8 et versions ultérieur, le message n’apparaît plus. Au lieu de cela, le message suivant s’affiche dans ce contexte :

```
This Website uses a data provider that may be unsafe. If you trust the
Website, click OK, otherwise click Cancel.
```

 Le message lui permet de prendre une décision informée, tout en sachant les conséquences sur chacune de ces options :

-   Si l’utilisateur approuve le site, en cliquant sur OK autorisera tout le code sécurisé disque (toutes les méthodes et propriétés ADO à l’exception de l’API accessible de disque décrites plus loin dans cette rubrique) pour exécuter et s’exécutent dans la fenêtre du navigateur.

-   Si l’utilisateur n’approuve pas le site, cliquez sur Annuler bloque le code ADO pour l’accès aux données de gestion et l’exécution dans son intégralité.

### <a name="disk-accessible-code-limited-now-to-trusted-sites"></a>Code accessible de disque limité désormais aux sites de confiance
 Les modifications de conception supplémentaires ont été apportées dans ADO 2.8 limiter la capacité d’un ensemble limité d’API, ce qui peut exposer la possibilité de lire ou écrire dans des fichiers sur l’ordinateur local. Voici les méthodes d’API qui ont été plus limitée à la sécurité lors de l’exécution d’Internet Explorer :

-   Pour ADO **flux** de l’objet, si le [LoadFromFile](../../ado/reference/ado-api/loadfromfile-method-ado.md) ou [SaveToFile](../../ado/reference/ado-api/savetofile-method.md) méthodes sont utilisées.

-   Pour ADO **Recordset** si l’objet, soit la [enregistrer](../../ado/reference/ado-api/save-method.md) (méthode) ou le [ouvrir](../../ado/reference/ado-api/open-method-ado-recordset.md) méthode, par exemple lorsque soit la **adCmdFile** option a la valeur ou le [Microsoft OLE DB fournisseur de persistance (MSPersist)](../../ado/guide/appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) est utilisé.

 Pour ces ensembles limités des fonctions potentiellement disque accessible, le comportement suivant se produit pour ADO 2.8 et versions ultérieur, si l’exécution de tout code qui utilise ces méthodes dans Internet Explorer :

-   Si le site qui a fourni le code a été ajouté précédemment à la liste de la zone Sites de confiance, le code s’exécute dans le navigateur et accéder aux fichiers locaux.

-   Si le site n’apparaît pas dans la liste de la zone Sites de confiance, le code est bloqué et l’accès aux fichiers locaux est refusé.

    > [!NOTE]
    >  Dans ADO 2.8 et versions ultérieur, l’utilisateur n’est pas averti ou recommandé d’ajouter des sites à la liste de la zone Sites de confiance. Par conséquent, la gestion de la liste des Sites de confiance est la responsabilité de ceux qui sont de déploiement ou de prise en charge les applications basées sur le site Web qui requièrent l’accès au système de fichiers local.

### <a name="access-blocked-to-the-activecommand-property-on-recordset-objects"></a>Accès bloqué à la propriété ActiveCommand sur les objets de jeu d’enregistrements
 Lors de l’exécution dans Internet Explorer, ADO 2.8 maintenant bloque l’accès à la [ActiveCommand](../../ado/reference/ado-api/activecommand-property-ado.md) propriété actif **Recordset** de l’objet et retourne une erreur. L’erreur se produit même si la page provient d’un site Web dans la liste des Sites de confiance.

### <a name="changes-in-handling-for-ole-db-providers-and-integrated-security"></a>Modifications de gestion pour les fournisseurs OLE DB et la sécurité intégrée
 Lors de la révision ADO 2.7 et versions antérieures pour les éventuels problèmes de sécurité et problèmes, le scénario suivant a été détecté :

 Dans certains cas, les fournisseurs OLE DB qui prennent en charge la sécurité intégrée [DBPROP_AUTH_INTEGRATED](https://msdn.microsoft.com/library/windows/desktop/ms712973.aspx) propriété pourrait permettre à des pages Web sous forme de script à réutiliser l’objet de connexion ADO pour se connecter par inadvertance à d’autres serveurs utilise les informations d’identification de connexion actuel des utilisateurs. Pour éviter ce problème, ADO 2.8 et versions ultérieur gèrent les fournisseurs OLE DB en fonction de la façon dont ils ont choisi pour fournir ou fournissent pas, pour la sécurité intégrée.

 Pour les pages Web qui sont chargés à partir des sites répertoriés dans la liste de la zone Sites de confiance, le tableau suivant fournit une répartition de la façon dont ADO 2.8 et versions ultérieur gère les connexions ADO dans chaque cas.

|Paramètres d’Internet Explorer pour l’authentification utilisateur, d’ouverture de session|Prend en charge de fournisseur « Integrated Security » et UID et PWD sont spécifiés (SQLOLEDB)|Fournisseur ne prend pas en charge les « Integrated Security » (JOLT, MSDASQL, MSPersist)|Fournisseur prend en charge le « Integrated Security » et il est défini sur SSPI (aucun UID/PWD ne spécifiés)|
|------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
|Ouverture de session automatique avec le nom d’utilisateur actuel et le mot de passe|Autoriser la connexion|Autoriser la connexion|Autoriser la connexion|
|Invite de nom d’utilisateur et mot de passe|Autoriser la connexion|Échec de connexion|Échec de connexion|
|Ouverture de session automatique uniquement dans la zone Intranet|Autoriser la connexion|Inviter l’utilisateur avec un avertissement de sécurité|Inviter l’utilisateur avec un avertissement de sécurité|
|Ouverture de session anonyme|Autoriser la connexion|Échec de connexion|Échec de connexion|

 Dans le cas où un avertissement de sécurité maintenant s’affiche, la boîte de message informe les utilisateurs :

```
This Website is using your identity to access a data source. If you trust this Website, click OK, otherwise click Cancel.
```

 Le message lui permet de prendre une décision plus avisée et agissez en conséquence.

> [!NOTE]
>  Pour les sites non fiables (autrement dit, les sites non répertoriés dans la liste de la zone Sites de confiance), si le fournisseur est également non approuvé (comme nous l’avons vu plus haut dans cette section), l’utilisateur peut voir deux avertissements de sécurité dans une ligne, un avertissement concernant le fournisseur unsafe et une deuxième alerte le Essayez d’utiliser leur identité. Si l’utilisateur clique sur OK pour le premier avertissement, les paramètres Internet Explorer et le code de comportement de réponse décrites dans le tableau précédent sont exécutées.

## <a name="controlling-whether-password-text-is-returned-in-ado-connection-strings"></a>Contrôle si le texte du mot de passe est retourné dans les chaînes de connexion ADO
 Lorsque vous essayez d’obtenir la valeur de la [ConnectionString](../../ado/reference/ado-api/connectionstring-property-ado.md) propriété ADO **connexion** de l’objet, les événements suivants se produisent :

1.  Si la connexion est ouverte, un appel de l’initialisation est effectué pour le fournisseur OLE DB sous-jacent pour obtenir la chaîne de connexion.

2.  Selon le paramètre dans le fournisseur OLE DB de la [DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO](https://msdn.microsoft.com/library/windows/desktop/ms714905.aspx) propriété, les mots de passe sont inclus, ainsi que d’autres informations de chaîne de connexion qui sont retournées.

 Par exemple, si la propriété de connexion ADO dynamique **Persist Security Info** a la valeur **True**, les informations de mot de passe sont incluses dans la chaîne de connexion retournée. Sinon, si le fournisseur sous-jacent a défini la propriété sur **False** (par exemple avec le fournisseur SQLOLEDB), les informations de mot de passe sont omises dans la chaîne de connexion retournée.

 Si vous utilisez un tiers (autrement dit, non Microsoft) fournisseurs OLE DB avec votre code d’application ADO, vous pouvez vérifier comment le **DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO** propriété est implémentée pour déterminer si l’inclusion de les informations de mot de passe des chaînes de connexion ADO sont autorisées.

## <a name="checking-for-non-file-devices-when-loading-and-saving-recordsets-or-streams"></a>La vérification des périphériques non-fichier lors du chargement et l’enregistrement des flux ou les jeux d’enregistrements
 Pour ADO 2.7 et versions antérieur, fichier d’entrée/sortie opérations telles que [ouvrir](../../ado/reference/ado-api/open-method-ado-recordset.md) et [enregistrer](../../ado/reference/ado-api/save-method.md) qui ont été utilisés pour lire et écrire des données de fichiers dans certains cas permettrait à un URL ou un nom à utiliser qui a spécifié un disque non- en fonction de type de fichier. Par exemple, LPT1, COM2, PRN. TXT, AUX utilisable en tant qu’alias pour l’entrée/sortie entre les imprimantes et les dispositifs auxiliaires sur le système en utilisant certaines

 ADO 2.8 et versions ultérieur, cette fonctionnalité a été mis à jour. Pour ouvrir et enregistrer **Recordset** et **flux** objets ADO effectue maintenant une vérification de type de fichier pour vous assurer que le périphérique d’entrée ou de sortie spécifié dans un URL ou nom de fichier est un fichier réel.

> [!NOTE]
>  Vérification de type de fichier comme décrit dans cette section s’applique uniquement pour Windows 2000 et versions ultérieures. Il ne s’applique pas aux situations où ADO 2.8 ou version ultérieure s’exécute sous des versions antérieures de Windows, tels que Windows 98.
