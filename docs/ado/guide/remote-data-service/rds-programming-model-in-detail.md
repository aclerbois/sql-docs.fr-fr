---
title: Modèle de programmation des services Bureau à distance en détail | Documents Microsoft
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
- RDS programming model [ADO], details
ms.assetid: 3e57af8d-519b-4467-a0bd-af468534cefd
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7e11dbaf046124e837b6ef33b8eb98219371ae18
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rds-programming-model-in-detail"></a>Modèle de programmation des services Bureau à distance en détail
Les éléments clés du modèle de programmation des services Bureau à distance sont les suivantes :  
  
-   RDS.DataSpace  
  
-   RDSServer.DataFactory  
  
-   RDS. DataControl  
  
-   Événement  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="rdsdataspace"></a>RDS.DataSpace  
 Votre application cliente doit spécifier le serveur et le programme de serveur à appeler. En retour, votre application reçoit une référence à l’application du serveur et peut traiter la référence comme s’il s’agissait du programme serveur lui-même.  
  
 Le modèle objet RDS fournit cette fonctionnalité avec le [RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md) objet.  
  
 Le programme de serveur est spécifié avec un identificateur de programme, ou *ProgID*. Le serveur utilise le *ProgID* et le Registre du serveur pour rechercher des informations sur le programme à exécuter.  
  
 Services Bureau à distance établit une distinction en interne, selon que le serveur soit sur un serveur distant via Internet ou un intranet ; un serveur sur un réseau local. ou non sur un serveur, mais plutôt sur une bibliothèque de liens dynamiques (DLL) locale. Cette distinction détermine comment les informations sont échangées entre le client et le serveur et fait la différence tangible dans le type de référence retournée à votre application cliente. Toutefois, à partir de votre point de vue, cette distinction n’a aucune signification spéciale. Important est que vous recevez une référence de programme exploitable.  
  
## <a name="rdsserverdatafactory"></a>RDSServer.DataFactory  
 RDS fournit un programme de serveur par défaut capable d’exécuter une requête SQL sur la source de données et retourner un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) de l’objet ou de prendre un **Recordset** de l’objet et de mettre à jour de la source de données.  
  
 Le modèle objet RDS fournit cette fonctionnalité avec le [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet.  
  
 En outre, cet objet a une méthode de création vide **Recordset** objet que vous pouvez remplir par programme ([CreateRecordset](../../../ado/reference/rds-api/createrecordset-method-rds.md)) et une autre méthode pour convertir un **Recordset**  objet en une chaîne de texte pour générer une page Web ([ConvertToString](../../../ado/reference/rds-api/converttostring-method-rds.md)).  
  
 Avec ADO, vous pouvez remplacer certains de la connexion standard et le comportement de commande de la **RDSServer.DataFactory** avec un **DataFactory** gestionnaire et un fichier de personnalisation qui contient la connexion, commande et les paramètres de sécurité.  
  
 Le programme de serveur est parfois appelé un *objet métier*. Vous pouvez écrire votre propre objet métier personnalisé qui peut effectuer des accès aux données complexes, des contrôles de validité et ainsi de suite. Même lorsque vous écrivez un objet métier personnalisé, vous pouvez créer une instance d’un **RDSServer.DataFactory** de l’objet et utiliser certaines de ses méthodes pour réaliser vos propres tâches.  
  
## <a name="rdsdatacontrol"></a>RDS. DataControl  
 Services Bureau à distance permet de combiner les fonctionnalités de la **RDS. DataSpace** et **RDSServer.DataFactory**et également activer des contrôles visuels d’utiliser facilement le **Recordset** objet retourné par une requête à partir d’une source de données. Tente de services Bureau à distance, pour la plupart des cas, faire autant que possible pour accéder aux informations sur un serveur et les afficher dans un contrôle visuel automatiquement.  
  
 Le modèle objet RDS fournit cette fonctionnalité avec le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet.  
  
 Le **RDS. DataControl** présente deux aspects. L’un des aspects se rapporte à la source de données. Si vous définissez la commande et les informations de connexion à l’aide du **Connect** et **SQL** propriétés de la **RDS. DataControl**, il utilisera automatiquement le **RDS. DataSpace** pour créer une référence à la valeur par défaut **RDSServer.DataFactory** objet. Le **RDSServer.DataFactory** utilisera le **Connect** valeur de propriété pour se connecter à la source de données, utilisez la **SQL** valeur de propriété à obtenir un  **Jeu d’enregistrements** à partir de la source de données et retournent le **Recordset** de l’objet à le **RDS. DataControl**.  
  
 Le second aspect porte sur l’affichage de retourné **Recordset** informations dans un contrôle visuel. Vous pouvez associer un contrôle visuel avec le **RDS. DataControl** (dans un processus appelé liaison) et accéder aux informations dans le type **Recordset** objet, en affichant les résultats de requête dans une page Web dans Microsoft® Internet Explorer. Chaque **RDS. DataControl** objet lie une **Recordset** objet représentant les résultats d’une seule requête, à un ou plusieurs contrôles visuels (par exemple, une zone de texte, zone de liste déroulante, contrôle de grille et ainsi de suite). Il peut y avoir plusieurs **RDS. DataControl** objet sur chaque page. Chaque **RDS. DataControl** objet peut être connecté à une source de données différente et contenir les résultats d’une requête distincte.  
  
 Le **RDS. DataControl** objet a également ses propres méthodes de navigation, de tri et de filtrage des lignes de la **Recordset** objet. Ces méthodes sont similaires, mais que les méthodes sur ADO **Recordset** objet.  
  
## <a name="events"></a>Événements  
 Services Bureau à distance prend en charge deux de ses propres événements, qui sont indépendantes du modèle d’événement ADO. Le [onReadyStateChange](../../../ado/reference/rds-api/onreadystatechange-event-rds.md) événement est appelé chaque fois que le **RDS. DataControl** [ReadyState](../../../ado/reference/rds-api/readystate-property-rds.md) propriété change, vous avertissant ainsi lorsqu’une opération asynchrone terminée, arrêtée ou a rencontré une erreur. Le [onError](../../../ado/reference/rds-api/onerror-event-rds.md) événements sont appelé chaque fois qu’une erreur se produit, même si l’erreur se produit pendant une opération asynchrone.  
  
> [!NOTE]
>  Microsoft Internet Explorer fournit deux événements supplémentaires pour les services Bureau à distance : **onDataSetChanged**, ce qui indique que le **Recordset** est fonctionnel mais l’extraction de lignes, et  **onDataSetComplete**, ce qui indique que le **Recordset** a fini d’extraire des lignes.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle de programmation des services Bureau à distance avec des objets](../../../ado/guide/remote-data-service/rds-programming-model-with-objects.md)   
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)   
 [DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)   
 [DataSpace, objet (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)   
 [Scénario des services Bureau à distance](../../../ado/guide/remote-data-service/rds-scenario.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
 [Utilisation et sécurité de RDS](../../../ado/guide/remote-data-service/rds-usage-and-security.md)



