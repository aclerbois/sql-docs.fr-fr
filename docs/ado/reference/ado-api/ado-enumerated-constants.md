---
title: Constantes énumérées ADO | Documents Microsoft
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
- enumerated constants [ADO]
ms.assetid: c97ed131-1a93-463c-9e61-22f029b0c474
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 151b4e88b3f094cd44ac7078e5d16e0b0730a9e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-enumerated-constants"></a>Constantes énumérées ADO
Pour faciliter le débogage, les énumérations ADO comportent une valeur pour chaque constante. Toutefois, cette valeur est purement indicative et peut changer d’une version de l’objet ADO à un autre. Votre code doit uniquement dépendre du nom, pas la valeur réelle de chaque constante énumérée.  
  
|||  
|-|-|  
|[ADCPROP_ASYNCTHREADPRIORITY_ENUM](../../../ado/reference/ado-api/adcprop-asyncthreadpriority-enum.md)|Pour un RDS **Recordset** d’objet, spécifie la priorité d’exécution du thread asynchrone qui extrait des données.|  
|[ADCPROP_AUTORECALC_ENUM](../../../ado/reference/ado-api/adcprop-autorecalc-enum.md)|Spécifie à quel moment le **MSDataShape** fournisseur recalcule les colonnes regroupées et calculées dans une liste hiérarchique **Recordset**.|  
|[ADCPROP_UPDATECRITERIA_ENUM](../../../ado/reference/ado-api/adcprop-updatecriteria-enum.md)|Spécifie les champs qui peuvent être utilisés pour détecter les conflits pendant une mise à jour optimiste d’une ligne de la source de données avec un **Recordset** objet.|  
|[ADCPROP_UPDATERESYNC_ENUM](../../../ado/reference/ado-api/adcprop-updateresync-enum.md)|Spécifie si le **UpdateBatch** méthode est suivie par implicite **Resync** opération de la méthode et dans ce cas, la portée de cette opération.|  
|[AffectEnum](../../../ado/reference/ado-api/affectenum.md)|Spécifie quels enregistrements sont affectés par une opération.|  
|[BookmarkEnum](../../../ado/reference/ado-api/bookmarkenum.md)|Spécifie un signet qui indique où doit commencer l’opération.|  
|[CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)|Spécifie comment un argument de commande doit être interprété.|  
|[CompareEnum](../../../ado/reference/ado-api/compareenum.md)|Spécifie la position relative de deux enregistrements représentée par leur signet.|  
|[ConnectModeEnum](../../../ado/reference/ado-api/connectmodeenum.md)|Spécifie les autorisations disponibles pour la modification des données dans un **connexion**, ouvrez un **enregistrement**, ou spécifier des valeurs pour le **Mode** propriété de la  **Enregistrement** et **flux** objets.|  
|[ConnectOptionEnum](../../../ado/reference/ado-api/connectoptionenum.md)|Spécifie si le **ouvrir** méthode d’un **connexion** objet doit être exécutée après (de façon synchrone) ou avant (de façon asynchrone) la connexion est établie.|  
|[ConnectPromptEnum](../../../ado/reference/ado-api/connectpromptenum.md)|Spécifie si une boîte de dialogue doit être affichée pour demander les paramètres manquants lors de l’ouverture d’une connexion à une source de données ODBC.|  
|[CopyRecordOptionsEnum](../../../ado/reference/ado-api/copyrecordoptionsenum.md)|Spécifie le comportement de la **CopyRecord** (méthode).|  
|[CursorLocationEnum](../../../ado/reference/ado-api/cursorlocationenum.md)|Spécifie l’emplacement du moteur de curseur.|  
|[CursorOptionEnum](../../../ado/reference/ado-api/cursoroptionenum.md)|Spécifie que les fonctionnalités du **prend en charge** méthode doit effectuer un test.|  
|[CursorTypeEnum](../../../ado/reference/ado-api/cursortypeenum.md)|Spécifie le type de curseur utilisé dans un **Recordset** objet.|  
|[DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md)|Spécifie le type de données d’une **champ**, **paramètre**, ou **propriété**.|  
|[EditModeEnum](../../../ado/reference/ado-api/editmodeenum.md)|Spécifie l’état de modification d’un enregistrement.|  
|[ErrorValueEnum](../../../ado/reference/ado-api/errorvalueenum.md)|Spécifie le type d’erreur d’exécution ADO.|  
|[EventReasonEnum](../../../ado/reference/ado-api/eventreasonenum.md)|Spécifie la raison qui a provoqué un événement se produise.|  
|[EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md)|Spécifie l’état actuel de l’exécution d’un événement.|  
|[ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md)|Spécifie comment un fournisseur doit exécuter une commande.|  
|[FieldEnum](../../../ado/reference/ado-api/fieldenum.md)|Spécifie les champs spéciaux référencés dans les **champs** collection d’un **enregistrement** objet.|  
|[FieldAttributeEnum](../../../ado/reference/ado-api/fieldattributeenum.md)|Spécifie un ou plusieurs attributs d’un **champ** objet.|  
|[FieldStatusEnum](../../../ado/reference/ado-api/fieldstatusenum.md)|Spécifie l’état d’un **champ** objet.|  
|[FilterGroupEnum](../../../ado/reference/ado-api/filtergroupenum.md)|Spécifie le groupe d’enregistrements à filtrer à partir d’un **Recordset**.|  
|[GetRowsOptionEnum](../../../ado/reference/ado-api/getrowsoptionenum.md)|Spécifie le nombre d’enregistrements à récupérer un **Recordset**.|  
|[IsolationLevelEnum](../../../ado/reference/ado-api/isolationlevelenum.md)|Spécifie le niveau d’isolation des transactions pour un **connexion** objet.|  
|[LineSeparatorsEnum](../../../ado/reference/ado-api/lineseparatorsenum.md)|Spécifie le caractère utilisé comme séparateur de ligne dans le texte **flux** objets.|  
|[LockTypeEnum](../../../ado/reference/ado-api/locktypeenum.md)|Spécifie le type de verrou placé sur les enregistrements pendant la modification.|  
|[MarshalOptionsEnum](../../../ado/reference/ado-api/marshaloptionsenum.md)|Spécifie les enregistrements qui doivent être renvoyées au serveur.|  
|[MoveRecordOptionsEnum](../../../ado/reference/ado-api/moverecordoptionsenum.md)|Spécifie le comportement de la **enregistrement** objet **MoveRecord** (méthode).|  
|[ObjectStateEnum](../../../ado/reference/ado-api/objectstateenum.md)|Spécifie si un objet est ouvert ou fermé, la connexion à une source de données, l’exécution d’une commande ou l’extraction de données.|  
|[ParameterAttributesEnum](../../../ado/reference/ado-api/parameterattributesenum.md)|Spécifie les attributs d’un **paramètre** objet.|  
|[ParameterDirectionEnum](../../../ado/reference/ado-api/parameterdirectionenum.md)|Spécifie si le **paramètre** représente un paramètre d’entrée, un paramètre de sortie ou les deux, ou si le paramètre est la valeur de retour d’une procédure stockée.|  
|[PersistFormatEnum](../../../ado/reference/ado-api/persistformatenum.md)|Spécifie le format dans lequel enregistrer un **Recordset**.|  
|[PositionEnum](../../../ado/reference/ado-api/positionenum.md)|Spécifie la position actuelle du pointeur d’enregistrement dans un **Recordset**.|  
|[PropertyAttributesEnum](../../../ado/reference/ado-api/propertyattributesenum.md)|Spécifie les attributs d’un **propriété** objet.|  
|[RecordCreateOptionsEnum](../../../ado/reference/ado-api/recordcreateoptionsenum.md)|Spécifie pour le **enregistrement** objet **ouvrir** méthode si existant **enregistrement** doit être ouvert, ou un nouveau **enregistrement** doit être créé.|  
|[RecordOpenOptionsEnum](../../../ado/reference/ado-api/recordopenoptionsenum.md)|Spécifie les options pour l’ouverture un **enregistrement**. Ces valeurs peuvent être combinées à l’aide d’un opérateur OR.|  
|[RecordStatusEnum](../../../ado/reference/ado-api/recordstatusenum.md)|Spécifie l’état d’un enregistrement en ce qui concerne les mises à jour par lots et d’autres opérations en bloc.|  
|[RecordTypeEnum](../../../ado/reference/ado-api/recordtypeenum.md)|Spécifie le type de **enregistrement** objet.|  
|[ResyncEnum](../../../ado/reference/ado-api/resyncenum.md)|Indique si les valeurs sous-jacentes sont remplacées par un appel à **Resync**.|  
|[SaveOptionsEnum](../../../ado/reference/ado-api/saveoptionsenum.md)|Spécifie si un fichier doit être créé ou remplacé lors de l’enregistrement d’un **flux** objet. Les valeurs peuvent être combinées avec un opérateur AND.|  
|[SchemaEnum](../../../ado/reference/ado-api/schemaenum.md)|Spécifie le type de schéma **Recordset** qui le **OpenSchema** récupère de la méthode. Spécifie la direction de la recherche d’un enregistrement dans un **Recordset**.|  
|[SearchDirectionEnum](../../../ado/reference/ado-api/searchdirectionenum.md)|Spécifie la direction de la recherche d’un enregistrement dans un **Recordset**. Spécifie le type de **recherche** à exécuter.|  
|[SeekEnum](../../../ado/reference/ado-api/seekenum.md)|Spécifie le type de **recherche** à exécuter. Spécifie les options pour l’ouverture un **flux** objet. Les valeurs peuvent être combinées avec un opérateur AND.|  
|[StreamOpenOptionsEnum](../../../ado/reference/ado-api/streamopenoptionsenum.md)|Spécifie les options pour l’ouverture un **flux** objet. Les valeurs peuvent être combinées avec un opérateur AND. Spécifie si l’ensemble du flux ou la ligne suivante doit être lue à partir d’un **flux** objet.|  
|[StreamReadEnum](../../../ado/reference/ado-api/streamreadenum.md)|Spécifie si l’ensemble du flux ou la ligne suivante doit être lue à partir d’un **flux** objet. Spécifie le type de données stockées dans un **flux** objet.|  
|[StreamTypeEnum](../../../ado/reference/ado-api/streamtypeenum.md)|Spécifie le type de données stockées dans un **flux** objet. Indique si un séparateur de ligne est ajouté à la chaîne écrite dans un **flux** objet.|  
|[StreamWriteEnum](../../../ado/reference/ado-api/streamwriteenum.md)|Indique si un séparateur de ligne est ajouté à la chaîne écrite dans un **flux** objet. Spécifie le format lors de la récupération une **Recordset** sous forme de chaîne.|  
|[StringFormatEnum](../../../ado/reference/ado-api/stringformatenum.md)|Spécifie le format lors de la récupération une **Recordset** sous forme de chaîne. Spécifie les attributs de transaction d’un **connexion** objet.|  
|[XactAttributeEnum](../../../ado/reference/ado-api/xactattributeenum.md)|Spécifie les attributs de transaction d’un **connexion** objet.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des API ADO](../../../ado/reference/ado-api/ado-api-reference.md)   
 [Collections ADO](../../../ado/reference/ado-api/ado-collections.md)   
 [Propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-properties.md)   
 [Annexe b : erreurs ADO](../../../ado/guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](../../../ado/reference/ado-api/ado-events.md)   
 [Méthodes ADO](../../../ado/reference/ado-api/ado-methods.md)   
 [Modèle objet ADO](../../../ado/reference/ado-api/ado-object-model.md)   
 [Objets et interfaces ADO](../../../ado/reference/ado-api/ado-objects-and-interfaces.md)   
 [Propriétés ADO](../../../ado/reference/ado-api/ado-properties.md)
