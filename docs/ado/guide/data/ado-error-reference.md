---
title: Référence des erreurs ADO | Documents Microsoft
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
- errors [ADO], number reference
- errors [ADO], ErrorValueEnum
- ErrorValueEnum enumeration [ADO]
ms.assetid: f653393e-d4b0-4c34-ad5f-2bdf56bc1305
caps.latest.revision: 4
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cf0631384cf390752d60e6465372558748880278
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-errors"></a>Erreurs ADO
Le **ErrorValueEnum** constante décrit les valeurs d’erreur ADO. Pour une liste complète de ces constantes énumérées, notamment les valeurs, consultez [annexe b : erreurs ADO](../../../ado/guide/appendixes/appendix-b-ado-errors.md). Cette section examine certaines des erreurs plus intéressantes et explique certaines situations peuvent déclencher ou des solutions pour résoudre le problème. Les deux le **ErrorValueEnum** constante et le nombre décimal positif court sont répertoriés.

|Number|Constante ErrorValueEnum|Description/causes possibles|
|------------|-----------------------------|----------------------------------|
|**3000**|**adErrProviderFailed**|Échec du fournisseur effectuer l’opération demandée.|
|**3001**|**adErrInvalidArgument**|Arguments sont de type incorrect, sont hors des limites autorisées ou sont en conflit avec un autre. Cette erreur est souvent due à une erreur typographique dans une instruction SQL SELECT. Par exemple, un nom de champ mal orthographié ou le nom de la table le peut générer cette erreur. Cette erreur peut également se produire lorsqu’un champ ou une table nommée dans une instruction SELECT n’existe pas dans le magasin de données.|
|**3002**|**adErrOpeningFile**|Fichier n’a pas pu être ouvert. Un nom de fichier mal orthographié a été spécifié, ou un fichier a été déplacé, renommé ou supprimé. Sur un réseau, le lecteur peut être temporairement indisponible ou le trafic réseau susceptibles d’empêcher une connexion.|
|**3003**|**adErrReadFile**|Fichier n’a pas pu être lu. Le nom du fichier est spécifié de manière incorrecte, le fichier a peut-être été déplacé ou supprimé, ou le fichier est peut-être endommagé.|
|**3004**|**adErrWriteFile**|Écriture dans le fichier a échoué. Vous avez fermé un fichier et tenté d’écrire, ou le fichier est peut-être endommagé. Si le fichier se trouve sur un lecteur réseau, les conditions réseau transitoire peuvent empêcher l’écriture sur un lecteur réseau.|
|**3021**|**adErrNoCurrentRecord**|Soit **BOF** ou **EOF** a la valeur True, ou l’enregistrement actif a été supprimé. Opération demandée nécessite un enregistrement en cours.<br /><br /> Une tentative a été effectuée pour mettre à jour des enregistrements à l’aide de **trouver** ou **recherche** pour déplacer le pointeur sur l’enregistrement de votre choix. Si l’enregistrement est introuvable, **EOF** aura la valeur True. Cette erreur peut également se produire après un échec **AddNew** ou **supprimer** , car il n’existe aucun enregistrement en cours en cas d’échouent de ces méthodes.|
|**3219**|**adErrIllegalOperation**|Opération n’est pas autorisée dans ce contexte.|
|**3220**|**adErrCantChangeProvider**|Le fournisseur indiqué est différent de celui déjà en cours d’utilisation.|
|**3246**|**adErrInTransaction**|**Connexion** objet ne peut pas être explicitement fermé pendant une transaction. A **Recordset** ou **connexion** objet participant à une transaction ne peut pas être fermé. Appelez **RollbackTrans** ou **CommitTrans** avant la fermeture de l’objet.|
|**3251**|**adErrFeatureNotAvailable**|L’objet ou le fournisseur n’est pas capable d’effectuer l’opération demandée. Certaines opérations dépendent d’une version de fournisseur particulier.|
|**3265**|**adErrItemNotFound**|Élément introuvable dans la collection correspondant au nom demandé ou ordinales. Un nom de champ ou de table incorrect a été spécifié.|
|**3367**|**adErrObjectInCollection**|Objet est déjà dans la collection. Impossible d’ajouter. Un objet ne peut pas être ajouté à la même collection de deux fois.|
|**3420**|**adErrObjectNotSet**|Objet n’est plus valide.|
|**3421**|**adErrDataConversion**|Application utilise une valeur de type incorrect pour l’opération actuelle. Vous avez peut-être fourni une chaîne à une opération qui attend un flux de données, par exemple.|
|**3704**|**adErrObjectClosed**|Opération n’est pas autorisée lorsque l’objet est fermé. Le **connexion** ou **Recordset** a été fermé. Par exemple, une autre routine a peut-être fermé un objet global. Vous pouvez éviter cette erreur en vérifiant la **état** propriété avant de tenter une opération.|
|**3705**|**adErrObjectOpen**|Opération n’est pas autorisée lorsque l’objet est ouvert. Un objet qui est ouvert ne peut pas être ouvert. Ne peut pas ajouter de champs à open **Recordset**.|
|**3706**|**adErrProviderNotFound**|Impossible de trouver le fournisseur. Il est peut-être pas être installé correctement.<br /><br /> Le nom du fournisseur peut être spécifié de manière incorrecte, le fournisseur spécifié ne peut pas être installé sur l’ordinateur sur lequel votre code est en cours d’exécution, ou l’installation est peut-être endommagée.|
|**3707**|**adErrBoundToCommand**|Le **ActiveConnection** propriété d’un **Recordset** object, qui a un **commande** comme source de l’objet, ne peut pas être modifié. L’application a tenté d’assigner une nouvelle **connexion** de l’objet à un **Recordset** qui a un **commande** objet comme source.|
|**3708**|**adErrInvalidParamInfo**|**Paramètre** objet est défini de manière incorrecte. Des informations incohérentes ou incomplètes ont été fournies.|
|**3709**|**adErrInvalidConnection**|La connexion ne peut pas être utilisée pour effectuer cette opération. Il est fermé ou non valide dans ce contexte.|
|**3710**|**adErrNotReentrant**|Impossible d’effectuer l’opération lors de l’événement de traitement. Une opération n’est pas possible dans un gestionnaire d’événements qui déclenche l’événement pour l’activer de nouveau. Par exemple, des méthodes de navigation ne doivent pas être appelées à partir d’un **WillMove** Gestionnaire d’événements.|
|**3711**|**adErrStillExecuting**|Impossible d’effectuer l’opération lors de l’exécution de façon asynchrone.|
|**3712**|**adErrOperationCancelled**|Opération a été annulée par l’utilisateur. L’application a appelé la **CancelUpdate** ou **CancelBatch** (méthode) et l’opération en cours a été annulée.|
|**3713**|**adErrStillConnecting**|Impossible d’effectuer l’opération lors de la connexion de façon asynchrone.|
|**3714**|**adErrInvalidTransaction**|La transaction de coordination n’est pas valide ou n’a pas démarré.|
|**3715**|**adErrNotExecuting**|Impossible d’effectuer l’opération lors de l’exécution de ne pas.|
|**3716**|**adErrUnsafeOperation**|Les paramètres de sécurité sur cet ordinateur interdisent l’accès à une source de données sur un autre domaine.|
|**3717**|**adWrnSecurityDialog**|À usage interne uniquement N’utilisez pas. (Entrée a été incluse par souci d’exhaustivité. Cette erreur ne doit pas apparaître dans votre code.)|
|**3718**|**adWrnSecurityDialogHeader**|À usage interne uniquement N’utilisez pas. (Entrée incluse par souci d’exhaustivité. Cette erreur ne doit pas apparaître dans votre code.)|
|**3719**|**adErrIntegrityViolation**|Valeur de données est en conflit avec les contraintes d’intégrité du champ. Une nouvelle valeur pour un **champ** entraîne une clé en double. Une valeur représentant un côté d’une relation entre deux enregistrements ne peut pas être mis à jour.|
|**3720**|**adErrPermissionDenied**|Autorisation insuffisante empêche l’écriture dans le champ. L’utilisateur nommé dans la chaîne de connexion n’a pas les autorisations appropriées pour écrire dans un **champ**.|
|**3721**|**adErrDataOverflow**|Valeur de données est trop grande pour être représentée par le type de données. Une valeur numérique est trop grande pour le champ prévu a été attribuée. Par exemple, une valeur d’entier long a été attribuée à un champ d’entier court.|
|**3722**|**adErrSchemaViolation**|Valeur de données est en conflit avec le type de données ou les contraintes du champ. Le magasin de données a des contraintes de validation qui diffèrent de la **champ** valeur.|
|**3723**|**adErrSignMismatch**|Échec de la conversion, car la valeur des données était signée et le type de données utilisé par le fournisseur n’était pas signé.|
|**3724**|**adErrCantConvertvalue**|Valeur de données ne peut pas être convertie pour des raisons autre que la non-correspondance des signes ou données débordement. Par exemple, conversion aurait tronqué les données.|
|**3725**|**adErrCantCreate**|Valeur de données ne peut pas être définie ou récupérée, car le type de données est inconnu ou le fournisseur a des ressources insuffisantes pour effectuer l’opération.|
|**3726**|**adErrColumnNotOnThisRow**|Enregistrement ne contient pas ce champ. Un nom de champ incorrect a été spécifié ou un champ ne figurant pas dans le **champs** collection de l’enregistrement actif a été référencée.|
|**3727**|**adErrURLDoesNotExist**|L’URL source ou le parent de l’URL de destination n’existe pas. Il est une erreur typographique dans l’URL de la source ou de destination. Vous pouvez avoir `http://mysite/photo/myphoto.jpg` lorsque vous devez réellement `http://mysite/photos/myphoto.jpg` à la place. L’erreur typographique dans l’URL parent (dans ce cas, *photo* au lieu de *photos*) a provoqué l’erreur.|
|**3728**|**adErrTreePermissionDenied**|Les autorisations sont insuffisantes pour accéder à l’arborescence ou une sous-arborescence. L’utilisateur nommé dans la chaîne de connexion n’a pas les autorisations appropriées.|
|**3729**|**adErrInvalidURL**|URL contient des caractères non valides. Assurez-vous que l’URL est correcte. L’URL suit le schéma inscrit pour le fournisseur actuel (par exemple, fournisseur de publication Internet est inscrit pour le protocole http).|
|**3730**|**adErrResourceLocked**|Objet représenté par l’URL spécifiée est verrouillé par un ou plusieurs autres processus. Attendez que le processus est terminé et recommencez l’opération. L’objet que vous essayez d’accéder a été verrouillé par un autre utilisateur ou par un autre processus dans votre application. Il s’agit probablement de se produire dans un environnement multi-utilisateur.|
|**3731**|**adErrResourceExists**|Impossible d’effectuer l’opération de copie. Objet nommé par l’URL de destination existe déjà. Spécifiez **adCopyOverwrite** pour remplacer l’objet. Si vous ne spécifiez pas **adCopyOverwrite** lors de la copie des fichiers dans un répertoire, la copie échoue lorsque vous essayez de copier un élément qui existe déjà dans l’emplacement de destination.|
|**3732**|**adErrCannotComplete**|Le serveur ne peut pas terminer l’opération. Cela peut être, car le serveur est occupé avec d’autres opérations, ou il peut être trop peu de ressources.|
|**3733**|**adErrVolumeNotFound**|Fournisseur ne peut pas localiser le périphérique de stockage indiqué par l’URL. Assurez-vous que l’URL est correcte. L’URL du périphérique de stockage est peut-être incorrect, mais cette erreur peut se produire pour d’autres raisons. L’appareil peut être hors connexion ou un gros volume de trafic réseau peut-être empêcher la connexion.|
|**3734**|**adErrOutOfSpace**|Impossible d’effectuer l’opération. Le fournisseur ne peut pas obtenir suffisamment d’espace libre. Il peut être pas de suffisamment de RAM ou d’espace disque pour les fichiers temporaires sur le serveur.|
|**3735**|**adErrResourceOutOfScope**|URL source ou de destination est en dehors de l’étendue de l’enregistrement actif.|
|**3736**|**adErrUnavailable**|Échec de l’opération et l’état n’est pas disponible. Le champ est peut-être indisponible ou l’opération n’a pas eu lieu. Un autre utilisateur peut avoir modifié ou supprimé le champ que vous essayez d’accéder.|
|**3737**|**adErrURLNamedRowDoesNotExist**|Enregistrement portant ce nom n’existe pas. Lors de la tentative d’ouverture d’un fichier en utilisant un **enregistrement** de l’objet, le nom de fichier ou le chemin d’accès au fichier a été mal orthographié.|
|**3738**|**adErrDelResOutOfScope**|L’URL de l’objet à supprimer est en dehors de l’étendue de l’enregistrement actif.|
|**3747**|**adErrCatalogNotSet**|Opération requiert une valide **ParentCatalog**.|
|**3748**|**adErrCantChangeConnection**|Connexion a été refusée. La nouvelle connexion que vous avez demandée a des caractéristiques différentes que celle déjà en cours d’utilisation.|
|**3749**|**adErrFieldsUpdateFailed**|Échec de la mise à jour des champs. Pour plus d’informations, examinez la **état** des propriétés des objets de champ individuel. Cette erreur peut se produire dans deux situations : lorsque vous modifiez un **champ** valeur de l’objet en cours de modification ou ajout d’un enregistrement à la base de données et lorsque vous modifiez les propriétés de la **champ** objet lui-même.<br /><br /> Le **enregistrement** ou **Recordset** mise à jour a échoué en raison d’un problème avec l’un des champs dans l’enregistrement actif. Énumérer les **champs** collecte et vérification de la **état** propriété de chaque champ pour déterminer la cause du problème.|
|**3750**|**adErrDenyNotSupported**|Fournisseur ne prend pas en charge les restrictions de partage. Une tentative a été effectuée pour restreindre le partage de fichiers et votre fournisseur ne prend pas en charge le concept.|
|**3751**|**adErrDenyTypeNotSupported**|Fournisseur ne prend pas en charge le type demandé de restriction de partage. Une tentative a été effectuée pour établir un type particulier de partage de fichiers restriction n’est pas pris en charge par votre fournisseur. Consultez la documentation du fournisseur pour déterminer les restrictions de partage de fichiers sont pris en charge.|
