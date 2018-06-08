---
title: DataControl, objet (RDS) | Documents Microsoft
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
apitype: COM
f1_keywords:
- DataControl
- RDS.DataControl
helpviewer_keywords:
- DataControl object [ADO]
ms.assetid: d85ea4fc-451c-436e-97b8-58f92b149dd0
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f8784dcbaa65a755a6469edaceb58288ab1e9c25
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datacontrol-object-rds"></a>DataControl, objet (RDS)
Lie une requête de données [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) à un ou plusieurs contrôles (par exemple, une zone de texte, contrôle de grille ou zone de liste déroulante) pour afficher la **Recordset** données sur une page Web.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="http://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
</OBJECT>  
```  
  
## <a name="remarks"></a>Notes  
 L’ID de classe pour le **RDS. DataControl** objet est 0-983A-00C04FC29E33 BD96C556 65A3 - 11 D.  
  
> [!NOTE]
>  Si vous obtenez une erreur une [RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md) ou **RDS. DataControl** objet charger, assurez-vous que vous utilisez l’ID de classe approprié. La classe les identificateurs de ces objets ont changé depuis la version 1.0 et 1.1. En outre, gardez à l’esprit que même les colonnes doivent être définies lorsque vous utilisez la **RDS DataControl** objet.  
  
 Dans un scénario de base, vous devez définir uniquement les **SQL**, **Connect**, et **Server** propriétés de la **RDS. DataControl** objet, qui appelle automatiquement l’objet métier par défaut, [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md).  
  
 Toutes les propriétés de la **RDS. DataControl** sont facultatifs, car des objets métier personnalisés peuvent remplacer leurs fonctionnalités.  
  
> [!NOTE]
>  Si vous interrogez pour plusieurs résultats, seul le premier [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) est retourné. Si plusieurs jeux de résultats est nécessaires, affectez chacun à sa propre **DataControl**. Un exemple d’une requête pour plusieurs résultats peut être les suivantes : `"Select * from Authors, Select * from Topics"`  
  
 Ajout de « DFMode = 20 ; » à votre chaîne de connexion lorsque vous utilisez la **RDS. DataControl** objet peut améliorer les performances de votre serveur lorsque vous mettez à jour des données. Avec ce paramètre, le **RDSServer.DataFactory** objet sur le serveur utilise un mode moins gourmandes en ressources. Toutefois, les fonctionnalités suivantes ne sont pas disponibles dans cette configuration :  
  
-   À l’aide des requêtes paramétrables.  
  
-   Obtention d’informations de paramètre ou la colonne avant d’appeler le **Execute** (méthode).  
  
-   Paramètre **Transact mises à jour** à **True**.  
  
-   Obtenir l’état de la ligne.  
  
-   Appel de la [Resync](../../../ado/reference/ado-api/resync-method.md) (méthode).  
  
-   Actualisation (explicite ou automatique) via la [Update Resync](../../../ado/reference/ado-api/update-resync-property-dynamic-ado.md) propriété.  
  
-   Paramètre **commande** ou [Recordset](../../../ado/reference/rds-api/recordset-sourcerecordset-properties-rds.md) propriétés.  
  
-   À l’aide de **adCmdTableDirect**.  
  
 Le **RDS. DataControl** objet s’exécute en mode asynchrone par défaut. Si vous avez besoin de l’exécution synchrone pour votre application, définissez la [ExecuteOptions](../../../ado/reference/rds-api/executeoptions-property-rds.md) paramètre égal à **est adcExecSync** et [FetchOptions](../../../ado/reference/rds-api/fetchoptions-property-rds.md) paramètre égal à **valeur adcFetchUpFront**, comme illustré dans l’exemple suivant.  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"   
    ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="http://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
   <PARAM NAME="ExecuteOptions" VALUE="1">   <PARAM NAME="FetchOptions" VALUE="1">  
</OBJECT>  
```  
  
 Utilisez une **RDS. DataControl** objet pour lier les résultats d’une requête unique à un ou plusieurs contrôles visuels. Par exemple, supposons que le code une requête demandant client les données telles que le nom, lieu de résidence, lieu de naissance, l’âge et état du client en priorité. Vous pouvez utiliser un seul **RDS. DataControl** objet pour afficher le nom d’un client, l’âge et la région dans trois zones de texte séparé ; État du client priorité dans une case à cocher ; et toutes les données dans un contrôle de grille.  
  
 Utilisez différents **RDS. DataControl** pour lier les résultats de plusieurs requêtes à des contrôles visuels différents objets. Par exemple, supposons que vous utilisez une requête pour obtenir des informations sur un client et une deuxième requête pour obtenir des informations sur les articles que le client a achetés. Vous souhaitez afficher les résultats de la première requête dans trois zones de texte et une case à cocher et les résultats de la seconde requête dans un contrôle de grille. Si vous utilisez l’objet métier par défaut (**RDSServer.DataFactory**), vous devez procédez comme suit :  
  
-   Ajouter deux **RDS. DataControl** objets à votre page Web.  
  
-   Écriture deux requêtes, une pour chaque **SQL** propriété des deux **RDS. DataControl** objets. Un **RDS. DataControl** objet contiendra une requête SQL demandant des informations client ; le deuxième contient une requête demandant la liste des articles achetés par le client.  
  
-   Dans les balises d’objet de chaque contrôle lié, spécifiez la valeur DATAFLD pour définir les valeurs pour les données que vous souhaitez afficher dans chaque contrôle visuel.  
  
 Il n’existe aucune restriction de comptage du nombre de **RDS. DataControl** objets que vous pouvez incorporer à l’aide de balises d’objet sur une page Web.  
  
 Lorsque vous définissez la **RDS. DataControl** de l’objet sur une page Web, utilisez différente de zéro **hauteur** et **largeur** valeurs telles que 1 (pour éviter l’inclusion d’espace supplémentaire).  
  
 Composants de client de Service de données distant sont déjà inclus dans Internet Explorer 4.0 ; Par conséquent, vous n’avez pas besoin d’inclure un paramètre CODEBASE dans votre **RDS. DataControl** balise d’objet.  
  
 Internet Explorer 4.0 ou version ultérieure, vous pouvez lier aux données à l’aide de contrôles HTML et les contrôles ActiveX® uniquement si elles sont marquées comme des contrôles de modèle cloisonné.  
  
> [!NOTE]
>  **Les utilisateurs de Microsoft Visual Basic** le **RDS. DataControl** est sécurisé pour le script et est utilisé uniquement dans les applications basées sur le Web. Il n’y a pas besoin pour celle-ci pour une application cliente de Visual Basic.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet DataControl (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [DataControl, exemple d’objet (VBScript)](../../../ado/reference/rds-api/datacontrol-object-example-vbscript.md)






















