---
title: Guide de référence des erreurs et des messages propres à Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- error numbers [Integration Services]
- hresults [Integration Services]
- errors [Integration Services], listed
ms.assetid: 2c825c07-5074-42ad-90ea-0dc5a588dcf7
caps.latest.revision: 44
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 55b664bda08e6842333fed67dafeab6e58a605e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-error-and-message-reference"></a>Guide de référence des erreurs et des messages propres à Integration Services
  Les tableaux suivants répertorient les erreurs, les avertissements et les messages d'information prédéfinis de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , par ordre croissant en fonction de leur numéro pour chaque catégorie, avec leurs codes numériques et noms symboliques. Chacune de ces erreurs est définie comme un champ de la classe <xref:Microsoft.SqlServer.Dts.Runtime.Hresults> dans l’espace de noms <xref:Microsoft.SqlServer.Dts.Runtime> .  
  
 Cette liste peut être utile lorsque vous rencontrez un code d'erreur sans sa description. La liste ne comporte actuellement aucune information de dépannage.  
  
> [!IMPORTANT]  
>  Nombre des messages d'erreur que vous pouvez rencontrer lorsque vous utilisez [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] proviennent d'autres composants. Cette rubrique présente toutes les erreurs déclenchées par les composants [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Si l'erreur que vous rencontrez ne figure pas dans la liste, cela signifie qu'elle a été déclenchée par un composant autre que [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Il peut s'agir, par exemple, de fournisseurs OLE DB, d'autres composants de base de données tels que le [!INCLUDE[ssDE](../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , ou d'autres services ou composants tels que le système de fichiers, le serveur SMTP ou Message Queuing (également appelé MSMQ), etc. Pour trouver des informations sur ces messages d'erreur externes, consultez la documentation spécifique au composant.  
  
 Cette liste contient les groupes de messages suivants :  
  
-   [Messages d'erreur (DTS_E_*)](#msgError)  
  
-   [Messages d'avertissement (DTS_W_*)](#msgWarning)  
  
-   [Messages d'information (DTS_I_*)](#msgInfo)  
  
-   [Messages généraux et d'événement (DTS_MSG_*)](#msgGeneral)  
  
-   [Messages de réussite (DTS_S_*)](#msgSuccess)  
  
-   [Messages d'erreur des composants de flux de données (DTSBC_E_*)](#msgPipeline)  
  
##  <a name="msgError"></a> Messages d'erreur  
 Les noms symboliques des messages d’erreur de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] commencent par **DTS_E_**.  
  
|Code hexadécimal|Code décimal|Nom symbolique|Description|  
|----------------------|------------------|-------------------|-----------------|  
|0x8002F347|-2147290297|DTS_E_STOREDPROCSTASK_OVERWRITINGSPATDESTINATION|Remplacement de la procédure stockée « %1 » à la destination.|  
|0x8020837E|-2145352834|DTS_E_ADOSRCUNKNOWNTYPEMAPPEDTONTEXT|Le type de données « %1! » rencontré sur la colonne « %2! » n'est pas pris en charge pour %3. Cette colonne sera convertie en DT_NTEXT.|  
|0x8020838C|-2145352820|DTS_E_XMLSRCSCHEMACOLUMNNOTINEXTERNALMETADATA|La colonne %1 dans la table %2 du schéma XML ne comporte pas de mappage dans les colonnes de métadonnées externes.|  
|0xC0000032|-1073741774|DTS_E_NOTINITIALIZED|Un objet interne ou une variable n'a pas été initialisé. C'est une erreur interne au produit.  Cette erreur est retournée lorsqu'une variable doit avoir une valeur valide et que ce n'est pas le cas.|  
|0xC0000033|-1073741773|DTS_E_EXPIRED|La période d'évaluation d'Integration Services a expiré.|  
|0xC0000034|-1073741772|DTS_E_NEGATIVEVALUESNOTALLOWED|Une valeur négative ne peut pas être attribuée à cette propriété. Cette erreur se produit lorsqu'une valeur négative est attribuée à une propriété ne pouvant contenir que des valeurs positives, telle que la propriété COUNT.|  
|0xC0000035|-1073741771|DTS_E_NEGATIVEINDEXNOTALLOWED|Les index ne peuvent pas être négatifs. Cette erreur se produit lorsqu'une valeur négative est utilisée en tant qu'index d'une collection.|  
|0xC00060AB|-1073717077|DTS_E_INVALIDSSISSERVERNAME|Nom de serveur « % 1 » non valide. Le service SSIS ne prenant pas en charge les instances multiples, utilisez uniquement le nom de serveur au lieu de « nom de serveur\instance ».|  
|0xC0008445|-1073707963|DTS_E_SCRIPTMIGRATIONFAILED64BIT|La migration de scripts VSA ne peut pas être effectuée sur les plateformes 64 bits, en raison d'un défaut de prise en charge du concepteur Visual Tools for Applications. Sur les plateformes 64 bits, exécutez la migration sous WOW64.|  
|0xC000931A|-1073704166|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_ERRORSINCOMMAND|L'exécution de la commande a généré des erreurs.|  
|0xC000F427|-1073679321|DTS_E_SSISSTANDALONENOTINSTALLED|Pour exécuter un package SSIS en dehors de [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] , vous devez installer 1 % d'Integration Services ou plus.|  
|0xC0010001|-1073676287|DTS_E_VARIABLENOTFOUND|La variable est introuvable. Ceci se produit lorsqu'une tentative est effectuée pour extraire une variable de la collection Variables sur un conteneur au cours de l'exécution du package, et que la variable est absente. Le nom de la variable a peut-être été modifié ou la variable n'est pas créée.|  
|0xC0010003|-1073676285|DTS_E_VARIABLEREADONLY|Erreur lors de la tentative d'écriture vers une variable en lecture seule, « %1 ».|  
|0xC0010004|-1073676284|DTS_E_MANAGEDCOMPONENTSTORENOTFOUND|Impossible de trouver les répertoires contenant les composants Tasks et Data Flow Task. Vérifiez l'intégrité de votre installation.|  
|0xC0010006|-1073676282|DTS_E_PACKAGENAMETOOLONG|Le nom du package est trop long. La limite est 128 caractères. Raccourcissez le nom du package.|  
|0xC0010007|-1073676281|DTS_E_PACKAGEDESCRIPTIONTOOLONG|La description du package est trop longue. La limite est 1 024 caractères. Raccourcissez la description du package.|  
|0xC0010008|-1073676280|DTS_E_VERCOMMENTSTOOLONG|La propriété VersionComments est trop longue. La limite est 1 024 caractères. Essayez de raccourcir la propriété VersionComments.|  
|0xC0010009|-1073676279|DTS_E_ELEMENTNOTFOUND|L'élément est introuvable dans une collection. Cette erreur se produit lorsque vous essayez d'extraire un élément d'une collection sur un conteneur au cours de l'exécution du package et que l'élément est absent.|  
|0xC001000A|-1073676278|DTS_E_PACKAGENOTFOUND|Le package spécifié n'a pas pu être chargé de la base de données SQL Server.|  
|0xC001000C|-1073676276|DTS_E_INVALIDVARIABLEVALUE|L'assignation de la valeur de variable n'est pas valide. Cette erreur se produit lorsqu'un client ou une tâche assigne un objet d'exécution à la valeur d'une variable.|  
|0xC001000D|-1073676275|DTS_E_RESERVEDNAMESPACE|Erreur lors de l'assignation de l'espace de noms à la variable. L'espace de noms « System » est réservé pour l'utilisation système. Cette erreur se produit lorsqu'un composant ou une tâche tente de créer une variable avec l'espace de noms « System ».|  
|0xC001000E|-1073676274|DTS_E_CONNECTIONNOTFOUND|La connexion « %1 » est introuvable. Cette erreur est retournée par la collection Connections lorsque l'élément de connexion spécifique est introuvable.|  
|0xC001000F|-1073676273|DTS_E_64BITVARIABLERECAST|La variable « %1 » est un entier 64 bits, non pris en charge dans ce système d'exploitation. Cette variable a été remaniée en entier 32 bits.|  
|0xC0010010|-1073676272|DTS_E_CANTCHANGEREADONLYATRUNTIME|Une tentative de modification d'un attribut de lecture seule sur la variable « %1 » s'est produite. Cette erreur se produit lorsqu'un attribut de lecture seule d'une variable est modifié pendant l'exécution. Les attributs de lecture seule ne peuvent être changés que lors de la conception.|  
|0xC0010011|-1073676271|DTS_E_VARIABLEINVALIDCONTAINERREF|Tentative non valide de définir une variable vers une référence de conteneur.  Les variables ne sont pas autorisées à référencer des conteneurs.|  
|0xC0010013|-1073676269|DTS_E_INVALIDVARVALUE|Assignation d'une valeur ou d'un objet non valide à la variable « %1 ». Cette erreur se produit lorsqu'une valeur n'est pas appropriée aux variables.|  
|0xC0010014|-1073676268|DTS_E_GENERICERROR|Une ou plusieurs erreurs se sont produites. D'autres erreurs spécifiques devraient précéder celle-ci pour fournir des informations détaillées sur les erreurs. Ce message est utilisé en tant que valeur retournée par les fonctions qui ont rencontré les erreurs.|  
|0xC0010016|-1073676266|DTS_E_INVALIDARRAYVALUE|Erreur lors de l'obtention ou de la définition d'une valeur de tableau. Le type « % 1 » n'est pas autorisé. Ceci se produit lors du chargement d'un tableau dans une variable.|  
|0xC0010017|-1073676265|DTS_E_UNSUPPORTEDARRAYTYPE|Type non pris en charge dans le tableau. Ceci se produit lors de l'enregistrement d'un tableau de types non pris en charge dans une variable.|  
|0xC0010018|-1073676264|DTS_E_PERSISTENCEERROR|Erreur lors du chargement de la valeur « %1 » du nœud « %2 ».|  
|0xC0010019|-1073676263|DTS_E_INVALIDNODE|Le nœud « %1 » n'est pas valide. Ceci se produit lors de l'échec de l'enregistrement.|  
|0xC0010020|-1073676256|DTS_E_ERRORLOADINGTASK|Échec du chargement de la tâche « %1 », type « %2 ». Les informations de contact de cette tâche sont « %3 ».|  
|0xC0010021|-1073676255|DTS_E_ERRORELEMENTNOTINCOLL|L'élément « %1 » n'existe pas dans la collection « %2 ».|  
|0xC0010022|-1073676254|DTS_E_MISSINGOBJECTDATA|L'élément ObjectData est absent du bloc XML d'un objet hébergé. Ceci se produit lorsque l'analyseur XML tente de chercher l'élément de données d'un objet, sans succès.|  
|0xC0010023|-1073676253|DTS_E_VARIABLENOTFOUNDINCOLL|La variable « %1 » est introuvable. Cette erreur se produit lors de la tentative d'extraction d'une variable d'une collection de variables sur un conteneur au cours de l'exécution du package et que la variable est absente.  Un nom de variable a peut-être changé ou la variable n'est pas créée.|  
|0xC0010025|-1073676251|DTS_E_HASEMPTYTASKHOSTS|Le package ne peut pas s'exécuter, car il contient des tâches qui n'ont pas pu se charger.|  
|0xC0010026|-1073676250|DTS_E_TASKISEMPTY|Échec de chargement de la tâche. Les informations de contact de cette tâche sont « %1 ».|  
|0xC0010027|-1073676249|DTS_E_ERRORLOADINGTASKNOCONTACT|Erreur lors du chargement de la tâche « %1 ».|  
|0xC0010028|-1073676248|DTS_E_ERRORATLOADTASK|Erreur lors du chargement de la tâche. Ceci se produit lorsque le chargement d'une tâche à partir de XML échoue.|  
|0xC0010200|-1073675776|DTS_E_MULTIPLECACHEWRITES|Le %1 ne peut pas écrire dans le cache parce que le %2 y a déjà effectué une opération d'écriture.|  
|0xC0010201|-1073675775|DTS_E_SETCACHEFORINSERTFAILED|Échec de préparation du cache pour les nouvelles données.|  
|0xC0010202|-1073675774|DTS_E_SETCACHEFORFILLFAILED|Échec de marquage du cache comme étant rempli des données.|  
|0xC0010203|-1073675773|DTS_E_READUNINITIALIZEDCACHE|Le cache n'est pas initialisé et ne peut pas être lu par %1.|  
|0xC0010204|-1073675772|DTS_E_SETCACHEFORREADFAILED|Échec de préparation du cache pour la fourniture des données.|  
|0xC0010205|-1073675771|DTS_E_READNOTFILLEDCACHE|Une opération d'écriture dans le cache est actuellement effectuée par %1 et le cache ne peut pas être lu par %2.|  
|0xC0010206|-1073675770|DTS_E_WRITEWHILECACHEINUSE|Le cache est lu à partir de %1 et il ne peut pas y être écrit par %2.|  
|0xC0011001|-1073672191|DTS_E_CANTLOADFROMNODE|Impossible de charger l'objet d'exécution à partir du nœud XML spécifié.  Ceci se produit lors de la tentative de chargement d'un package ou d'un autre objet à partir d'un nœud XML dont le type est incorrect, tel qu'un nœud XML non SSIS.|  
|0xC0011002|-1073672190|DTS_E_OPENPACKAGEFILE|Impossible d'ouvrir le fichier de package « %1 » en raison de l'erreur 0x%2!8.8X! "%3".  Ceci se produit lors du chargement d'un package et que le fichier ne peut s'ouvrir ou se charger correctement dans le document XML. Soit un nom de fichier incorrect a été spécifié lors de l'appel de la méthode LoadPackage, soit le format du fichier XML spécifié est incorrect.|  
|0xC0011003|-1073672189|DTS_E_LOADPACKAGEXML|Impossible de charger XML en raison de l'erreur 0x%1!8.8X! « %2 ». Ceci se produit lors du chargement d'un package et que le fichier ne peut s'ouvrir ou se charger correctement dans le document XML.  Soit le nom du fichier fourni à la méthode LoadPackage est incorrect, soit le format du fichier XML spécifié est incorrect.|  
|0xC0011004|-1073672188|DTS_E_LOADPACKAGEXMLFILE|Impossible de charger XML à partir du fichier de package « %1 » en raison de l'erreur 0x%2!8.8X! "%3".  Ceci se produit lors du chargement d'un package et que le fichier ne peut pas s'ouvrir ou se charger correctement dans un document XML. Soit le nom du fichier fourni à la méthode LoadPackage est incorrect, soit le format du fichier XML spécifié est incorrect.|  
|0xC0011005|-1073672187|DTS_E_OPENFILE|Impossible d'ouvrir le fichier de package. Ceci se produit lors du chargement d'un package et que le fichier ne peut pas s'ouvrir ou se charger correctement dans un document XML. Soit le nom du fichier fourni à la méthode LoadPackage est incorrect, soit le format du fichier XML spécifié est incorrect.|  
|0xC0011006|-1073672186|DTS_E_UNABLETODECODEBINARYFORMAT|Impossible de décoder un format binaire dans le package.|  
|0xC0011007|-1073672185|DTS_E_FUNDAMENTALLOADINGERROR|Impossible de charger le package en XML, car son format XML n'est pas valide. Une erreur spécifique de l'analyseur XML sera publiée.|  
|0xC0011008|-1073672184|DTS_E_LOADFROMXML|Erreur lors du chargement à partir de XML. Aucune information supplémentaire ne peut être fournie pour ce problème, car aucun objet Events n'a été passé là où des informations détaillées sur l'erreur peuvent être stockées.|  
|0xC0011009|-1073672183|DTS_E_XMLDOMERROR|Impossible de créer une instance du modèle d'objet de document XML. MSXML n'est peut-être pas enregistré.|  
|0xC001100D|-1073672179|DTS_E_CANNOTLOADOLDPACKAGES|Impossible de charger le package. Ceci se produit lors de la tentative de chargement d'une version de package plus ancienne, ou le fichier de package se rapporte à un objet dont la structure n'est pas valide.|  
|0xC001100E|-1073672178|DTS_E_SAVEFILE|Impossible d'enregistrer le fichier de package.|  
|0xC001100F|-1073672177|DTS_E_SAVEPACKAGEFILE|Impossible d'enregistrer le fichier de package « %1 » avec l'erreur 0x%2!8.8X! "%3".|  
|0xC001200D|-1073668083|DTS_E_IDTSNAMENOTSUPPORTED|L'objet doit hériter de IDTSName100 et ce n'est pas le cas.|  
|0xC0012018|-1073668072|DTS_E_CONFIGFORMATINVALID_PACKAGEDELIMITER|Le format de l'entrée de configuration « %1 » est incorrect, car il ne commence pas par le séparateur de packages. Il n'y a pas de séparateur « \packages ».|  
|0xC0012019|-1073668071|DTS_E_CONFIGFORMATINVALID|Le format de l'entrée de configuration « %1 » est incorrect. Ceci peut se produire en raison d'un séparateur absent ou d'erreurs de format, comme un séparateur de tableaux non valide.|  
|0xC001201B|-1073668069|DTS_E_CONFIGFILEFAILEDEXPORT|Échec de l'exportation du fichier de configuration.|  
|0xC0012021|-1073668063|DTS_E_PROPERTIESCOLLECTIONREADONLY|La collection Properties ne peut pas être modifiée.|  
|0xC0012022|-1073668062|DTS_E_DTRXMLSAVEFAILURE|Impossible d'enregistrer le fichier de configuration. Le fichier est peut-être en lecture seule.|  
|0xC0012023|-1073668061|DTS_E_FAILPACKAGEONFAILURENA|La propriété FailPackageOnFailure n'est pas applicable au conteneur de package.|  
|0xC0012024|-1073668060|DTS_E_TASKPRODUCTLEVEL|La tâche « %1 » ne peut pas s'exécuter sur la version installée d'Integration Services %2. La version %3 ou une version ultérieure est requise.|  
|0xC0012029|-1073668055|DTS_E_UNABLETOSAVETOFILE|Impossible d'enregistrer xml dans « %1 ». Le fichier est peut-être en lecture seule.|  
|0xC0012037|-1073668041|DTS_E_CONFIGTYPECONVERSIONFAILED|Impossible de convertir un type dans la configuration « %1 » pour le chemin d'accès au package « %2 ».  Ceci se produit lorsqu'une valeur de configuration ne peut être convertie d'une chaîne dans le type de destination approprié. Vérifiez la valeur de la configuration pour vous assurer qu'elle peut être convertie dans le type de propriété ou de variable de destination.|  
|0xC0012049|-1073668023|DTS_E_CONFIGFAILED|Échec de la configuration. Ceci est un avertissement générique pour tous les types de configuration. D'autres avertissements doivent précéder celui-ci avec plus d'informations.|  
|0xC0012050|-1073668016|DTS_E_REMOTEPACKAGEVALIDATION|Le package n'a pas pu être validé par la tâche ExecutePackage. Le package ne peut pas s'exécuter.|  
|0xC0013001|-1073663999|DTS_E_FAILTOCREATEMUTEX|Impossible de créer le mutex « %1 », avec l'erreur 0x%2!8.8X!.|  
|0xC0013002|-1073663998|DTS_E_MUTEXOWNBYDIFFUSER|Le mutex « %1 » existe déjà et est détenu par un autre utilisateur.|  
|0xC0013003|-1073663997|DTS_E_WAITFORMUTEXFAILED|Impossible d'acquérir le mutex « %1 », avec l'erreur 0x%2!8.8X!.|  
|0xC0013004|-1073663996|DTS_E_FAILTORELEASEMUTEX|Impossible de libérer le mutex « %1 », avec l'erreur 0x%2!8.8X!.|  
|0xC0014003|-1073659901|DTS_E_INVALIDTASKPOINTER|Le pointeur de tâche du wrapper n'est pas valide. Le wrapper a un pointeur non valide vers une tâche.|  
|0xC0014004|-1073659900|DTS_E_ALREADYADDED|L'exécutable a été ajouté à la collection Executables d'un autre conteneur. Ceci se produit lorsqu'un client essaie d'ajouter un exécutable à plusieurs collections Executables. Vous devez supprimer l'exécutable de la collection Executables actuelle avant d'essayer de l'ajouter.|  
|0xC0014005|-1073659899|DTS_E_UNKNOWNCONNECTIONMANAGERTYPE|Le type de connexion « %1 » spécifié pour le gestionnaire de connexions « %2 » n'est pas reconnu comme type de gestionnaire de connexions valide. Cette erreur est retournée lorsqu'une tentative est effectuée de créer un gestionnaire de connexions pour un type de connexion inconnu. Vérifiez l'orthographe du nom du type de connexion.|  
|0xC0014006|-1073659898|DTS_E_COLLECTIONCOULDNTADD|Un objet a été créé, mais la tentative de l'ajouter à une collection a échoué. Ceci peut se produire en raison d'une insuffisance de mémoire.|  
|0xC0014007|-1073659897|DTS_E_ODBCERRORENV|Une erreur s'est produire lors de la création d'un environnement ODBC (Open Database Connectivity).|  
|0xC0014008|-1073659896|DTS_E_ODBCERRORDBC|Une erreur s'est produite lors de la création d'une connexion de base de données ODBC (Open Database Connectivity).|  
|0xC0014009|-1073659895|DTS_E_ODBCERRORCONNECT|Une erreur s'est produite en essayant d'établir une connexion ODBC (Open Database Connectivity) avec le serveur de base de données.|  
|0xC001400A|-1073659894|DTS_E_CONNECTIONMANAGERQUALIFIERALREADYSET|Le qualificateur est déjà défini sur cette instance du gestionnaire de connexions. Le qualificateur peut être défini une seule fois par instance.|  
|0xC001400B|-1073659893|DTS_E_CONNECTIONMANAGERQUALIFIERNOTSET|Le qualificateur n'a pas été défini sur cette instance du gestionnaire de connexions. La définition du qualificateur est nécessaire pour terminer l'initialisation.|  
|0xC001400C|-1073659892|DTS_E_CONNECTIONMANAGERQUALIFIERNOTSUPPORTED|Ce gestionnaire de connexions ne prend pas en charge la spécification des qualificateurs.|  
|0xC001400D|-1073659891|DTS_E_CANNOTCLONECONNECTIONMANAGER|Le gestionnaire de connexions « 0x%1 » ne peut pas être dupliqué pour une exécution extra-processus.|  
|0xC001400E|-1073659890|DTS_E_NOSQLPROFILERDLL|Le module fournisseur d'informations de SQL Server Profiler n'a pas réussi à charger pfclnt.dll. Veuillez vérifier que SQL Server Profiler est installé.|  
|0xC001400F|-1073659889|DTS_E_LOGFAILED|L'infrastructure d'enregistrement SSIS a échoué avec le code d'erreur 0x%1!8.8X!. L'erreur indique que ce problème d'enregistrement n'est pas imputable à un module fournisseur d'informations spécifique.|  
|0xC0014010|-1073659888|DTS_E_LOGPROVIDERFAILED|Le module fournisseur d'informations « %1 » a échoué avec le code d'erreur 0x%2!8.8X! (%3).  Ceci indique une erreur d'enregistrement imputable au module fournisseur d'informations spécifié.|  
|0xC0014011|-1073659887|DTS_E_SAVETOSQLSERVER_OLEDB|La méthode SaveToSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2).  L'instruction SQL émise a échoué.|  
|0xC0014012|-1073659886|DTS_E_LOADFROMSQLSERVER_OLEDB|La méthode LoadFromSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2).  L'instruction SQL émise a échoué.|  
|0xC0014013|-1073659885|DTS_E_REMOVEFROMSQLSERVER_OLEDB|La méthode RemoveFromSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2) L'instruction SQL émise a échoué.|  
|0xC0014014|-1073659884|DTS_E_EXISTSONSQLSERVER_OLEDB|La méthode ExistsOnSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2). L'instruction SQL émise a échoué.|  
|0xC0014015|-1073659883|DTS_E_CONNECTIONSTRING|OLE DB n'a pas réussi à créer une connexion de base de données lors de l'utilisation de la chaîne de connexion fournie.|  
|0xC0014016|-1073659882|DTS_E_FROMEXECISNOTCHILD|Lors de l'ajout d'une contrainte de précédence, un exécutable From a été spécifié, mais il n'est pas un enfant de ce conteneur.|  
|0xC0014017|-1073659881|DTS_E_TOEXECISNOTCHILD|Lors de l'ajout d'une contrainte de précédence, l'exécutable To spécifié n'est pas un enfant de ce conteneur.|  
|0xC0014018|-1073659880|DTS_E_ODBCTRANSACTIONENLIST|Une erreur s'est produite en essayant d'inscrire une connexion ODBC dans une transaction. SQLSetConnectAttr n'a pas pu définir l'attribut SQL_ATTR_ENLIST_IN_DTC.|  
|0xC0014019|-1073659879|DTS_E_CONNECTIONOFFLINE|Le gestionnaire de connexions « %1 » n'obtiendra pas de connexion, car la propriété OfflineMode du package a la valeur TRUE. Lorsque OfflineMode a la valeur TRUE, il est impossible d'obtenir des connexions.|  
|0xC001401A|-1073659878|DTS_E_BEGINTRANSACTION|Le Runtime SSIS n'a pas réussi à démarrer la transaction distribuée en raison de l'erreur 0x%1!8.8X! « %2 ». La transaction DTC n'a pas pu démarrer. Ceci a pu se produire parce que le service MSDTC n'est pas en cours d'exécution.|  
|0xC001401B|-1073659877|DTS_E_SETQUALIFIERDESIGNTIMEONLY|La méthode SetQualifier ne peut pas être appelée sur un gestionnaire de connexions au cours de l'exécution du package. Cette méthode est utilisée uniquement au moment de la conception.|  
|0xC001401C|-1073659876|DTS_E_SQLPERSISTENCEVERSION|Pour qu'il soit possible de stocker ou de modifier des packages dans SQL Server, la version de la base de données et du runtime SSIS doivent être identiques. Le stockage de packages dans des versions antérieures n'est pas pris en charge.|  
|0xC001401D|-1073659875|DTS_E_CONNECTIONVALIDATIONFAILED|La validation de la connexion « %1 » a échoué.|  
|0xC001401E|-1073659874|DTS_E_INVALIDFILENAMEINCONNECTION|Le nom de fichier « %1 » spécifié dans la connexion n'est pas valide.|  
|0xC001401F|-1073659873|DTS_E_MULTIPLEFILESONRETAINEDCONNECTION|Il n'est pas possible de spécifier plusieurs noms de fichiers sur une connexion lorsque la propriété Retain a la valeur TRUE. Des barres verticales ont été détectées sur la chaîne de connexion, ce qui signifie que plusieurs noms de fichiers sont spécifiés et, de plus, la propriété Retain a la valeur TRUE.|  
|0xC0014020|-1073659872|DTS_E_ODBCERROR|Une erreur ODBC %1!d! s’est produite.|  
|0xC0014021|-1073659871|DTS_E_PRECEDENCECONSTRAINT|La contrainte de précédence comportait une erreur entre « %1 » et « %2 ».|  
|0xC0014022|-1073659870|DTS_E_FAILEDPOPNATIVEFEE|Impossible de remplir la collection ForEachEnumeratorInfos avec des ForEachEnumerators natifs. Le code d'erreur était le suivant : %1.|  
|0xC0014023|-1073659869|DTS_E_GETENUMERATOR|La méthode GetEnumerator de l'énumérateur ForEach a échoué avec le code d'erreur 0x%1!8.8X! « %2 ». Ceci se produit lorsque l'énumérateur ForEach ne parvient pas à effectuer l'énumération.|  
|0xC0014024|-1073659868|DTS_E_CANTGETCERTDATA|Les données de certificat brutes ne peuvent pas être obtenues auprès de l'objet de certificat fourni (erreur : %1). Ceci se produit lorsque CPackage::put_CertificateObject ne peut pas instancier l'objet ManagedHelper , lorsque l'objet ManagedHelper échoue ou lorsque l'objet ManagedHelper retourne un tableau mal formé.|  
|0xC0014025|-1073659867|DTS_E_CANTCREATECERTCONTEXT|Impossible de créer un contexte de certificat (erreur : %1). Ceci se produit dans CPackage::put_CertificateObject ou dans CPackage::LoadFromXML lorsque la fonction CryptoAPI correspondante échoue.|  
|0xC0014026|-1073659866|DTS_E_CANTOPENCERTSTORE|L'ouverture du magasin de certificats MY a échoué avec le code d'erreur « %1 ». Ceci se produit dans CPackage::LoadUserCertificateByName et CPackage::LoadUserCertificateByHash.|  
|0xC0014027|-1073659865|DTS_E_CANTFINDCERTBYNAME|Le certificat spécifié par le nom dans le magasin MY est introuvable (erreur : %1). Ceci se produit dans CPackage::LoadUserCertificateByName.|  
|0xC0014028|-1073659864|DTS_E_CANTFINDCERTBYHASH|Impossible de trouver le certificat spécifié par hachage dans le magasin « MY » (erreur : %1). Se produit dans CPackage::LoadUserCertificateByHash.|  
|0xC0014029|-1073659863|DTS_E_INVALIDCERTHASHFORMAT|La valeur de hachage n'est pas un tableau unidimensionnel d'octets (erreur : %1). Ceci se produit dans CPackage::LoadUserCertificateByHash.|  
|0xC001402A|-1073659862|DTS_E_CANTACCESSARRAYDATA|Les données du tableau sont inaccessibles (erreur : %1). Cette erreur peut se produire partout où GetDataFromSafeArray est appelé.|  
|0xC001402B|-1073659861|DTS_E_CREATEMANAGEDHELPERFAILED|L'objet ManagedHelper de SSIS a échoué en cours de création avec l'erreur 0x%1!8.8X! « %2 ». Ceci se produit partout où CoCreateInstance CLSID_DTSManagedHelper échoue.|  
|0xC001402C|-1073659860|DTS_E_OLEDBTRANSACTIONENLIST|Le Runtime SSIS a échoué l'inscription de la connexion OLE DB dans une transaction distribuée avec l'erreur 0x%1!8.8X! « %2 ».|  
|0xC001402D|-1073659859|DTS_E_SIGNPACKAGEFAILED|La signature du package a échoué avec l'erreur 0x%1!8.8X! « %2 ». Ceci se produit lorsque la méthode ManagedHelper.SignDocument échoue.|  
|0xC001402E|-1073659858|DTS_E_CHECKENVELOPEFAILED|Échec de la recherche d'enveloppe de signature XML dans le XML du package avec l'erreur 0x%1!8.8X! « %2 ». Ceci se produit dans CPackage::LoadFromXML.|  
|0xC001402F|-1073659857|DTS_E_GETXMLSOURCEFAILED|Échec de l'obtention de la source XML à partir de l'objet XML DOM avec l'erreur 0x%1!8.8X! « %2 ». Cela se produit lorsque IXMLDOMDocument::get_xml échoue.|  
|0xC0014030|-1073659856|DTS_E_PACKAGEVERIFICATIONFAILED|La signature de chiffrement du package n'a pas réussi la vérification en raison de l'erreur 0x%1!8.8X! « %2 ». Ceci se produit lorsque l'opération de vérification de la signature échoue.|  
|0xC0014031|-1073659855|DTS_E_GETKEYFROMCERTFAILED|Échec de l'obtention de la paire de clés de chiffrement associées au certificat spécifié avec l'erreur 0x%1!8.8X! « %2 ». Vérifiez que vous avez la paire de clés correspondant au certificat émis. Cette erreur se produit généralement en tentant de signer un document à l'aide d'un certificat dont la personne n'a pas la clé privée.|  
|0xC0014032|-1073659854|DTS_E_INVALIDSIGNATURE|La signature numérique n'est pas valide. Le contenu du package a été modifié.|  
|0xC0014033|-1073659853|DTS_E_UNTRUSTEDSIGNATURE|La signature numérique est valide ; toutefois le signataire n'est pas approuvé et, par conséquent, l'authenticité ne peut être garantie.|  
|0xC0014034|-1073659852|DTS_E_TRANSACTIONENLISTNOTSUPPORTED|La connexion ne prend pas en charge l'inscription dans une transaction distribuée.|  
|0xC0014035|-1073659851|DTS_E_PACKAGEPROTECT|Impossible d'appliquer la protection de package avec l'erreur 0x%1!8.8X! « %2 ». Cette erreur se produit lors d'un enregistrement en Xml.|  
|0xC0014036|-1073659850|DTS_E_PACKAGEUNPROTECT|Impossible de supprimer la protection de package avec l'erreur 0x%1!8.8X! « %2 ». Ceci se produit dans la méthode CPackage::LoadFromXML.|  
|0xC0014037|-1073659849|DTS_E_PACKAGEPASSWORD|Le package est chiffré avec un mot de passe. Le mot de passe n'est pas spécifié ou est incorrect.|  
|0xC0014038|-1073659848|DTS_E_DUPLICATECONSTRAINT|Une contrainte de précédence existe déjà entre les exécutables spécifiés. Plusieurs contraintes de précédence ne sont pas autorisées.|  
|0xC0014039|-1073659847|DTS_E_PACKAGELOADFAILED|Le package n'a pas pu se charger en raison de l'erreur 0x%1!8.8X! « %2 ». Ceci se produit lorsque CPackage::LoadFromXML échoue.|  
|0xC001403A|-1073659846|DTS_E_PACKAGEOBJECTNOTENVELOPED|Impossible de trouver l'objet de package dans l'enveloppe XML signée avec l'erreur 0x%1!8.8X! « %2 ». Ceci se produit lorsque l'enveloppe XML signée ne contient pas de package SSIS, comme prévu.|  
|0xC001403B|-1073659845|DTS_E_JAGGEDEVENTINFO|Les longueurs des tableaux de paramètres, de types et de descriptions ne sont pas égales. Les longueurs doivent être égales. Ceci se produit lorsque les longueurs des tableaux sont différentes. Il doit y avoir une entrée par paramètre dans chaque tableau.|  
|0xC001403C|-1073659844|DTS_E_GETPACKAGEINFOS|Une erreur OLE DB 0x%1!8.8X! s'est produite lors de l'énumération des packages. Une instruction SQL a été émise et a échoué.|  
|0xC001403D|-1073659843|DTS_E_UNKNOWNLOGPROVIDERTYPE|Le type de module fournisseur d'informations « %1 » spécifié pour le module « %2 » n'est pas reconnu comme étant un type valide. Cette erreur se produit lors d'une tentative de création d'un module fournisseur d'informations pour un type de module inconnu. Vérifiez l'orthographe du nom du type de module fournisseur d'informations.|  
|0xC001403E|-1073659842|DTS_E_UNKNOWNLOGPROVIDERTYPENOSUBS|Le type de module fournisseur d'informations n'est pas reconnu comme étant valide. Cette erreur se produit lors d'une tentative de création d'un module fournisseur d'informations pour un type de module inconnu. Vérifiez l'orthographe du nom du type de module fournisseur d'informations.|  
|0xC001403F|-1073659841|DTS_E_UNKNOWNCONNECTIONMANAGERTYPENOSUBS|Le type de connexion spécifié pour le gestionnaire de connexions n'est pas valide. Cette erreur se produit lorsqu'une tentative est effectuée de créer un gestionnaire de connexions pour un type de connexion inconnu. Vérifiez l'orthographe du nom du type de connexion.|  
|0xC0014040|-1073659840|DTS_E_PACKAGEREMOVEFAILED|Une erreur s'est produite lors de la tentative de suppression du package « %1 » de SQL Server.|  
|0xC0014042|-1073659838|DTS_E_FOLDERADDFAILED|Une erreur s'est produite lors de la création d'un dossier sur SQL Server nommé « %1 » dans le dossier « %2 ».|  
|0xC0014043|-1073659837|DTS_E_CREATEFOLDERONSQLSERVER_OLEDB|La méthode CreateFolderOnSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2) L'instruction SQL émise a échoué.|  
|0xC0014044|-1073659836|DTS_E_FOLDERRENAMEFAILED|Une erreur s’est produite en renommant le dossier « %1\\\\%2 » en « %1\\\\%3 » sur SQL Server.|  
|0xC0014045|-1073659835|DTS_E_RENAMEFOLDERONSQLSERVER_OLEDB|La méthode RenameFolderOnSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2). L'instruction SQL émise a échoué.|  
|0xC0014046|-1073659834|DTS_E_FOLDERDELETEFAILED|Erreur lors de la suppression du dossier SQL Server « %1 ».|  
|0xC0014047|-1073659833|DTS_E_REMOVEFOLDERFROMSQLSERVER_OLEDB|La méthode RemoveFolderOnSQLServer a rencontré le code d'erreur OLE DB 0x%1!8.8X! (%2). L'instruction SQL émise a échoué.|  
|0xC0014048|-1073659832|DTS_E_INVALIDPATHTOPACKAGE|Le chemin d'accès du package spécifié ne contient pas de nom de package. Ceci se produit lorsque le chemin d'accès ne contient pas au moins une barre oblique inverse ou une barre oblique.|  
|0xC0014049|-1073659831|DTS_E_FOLDERNOTFOUND|Le dossier « %1 » est introuvable.|  
|0xC001404A|-1073659830|DTS_E_FINDFOLDERONSQLSERVER_OLEDB|En essayant de rechercher un dossier sur SQL, une erreur OLE DB s'est produite avec le code d'erreur 0x%1!8.8X! (%2).|  
|0xC001404B|-1073659829|DTS_E_OPENLOGFAILED|Le module fournisseur d'informations SSIS n'a pas réussi à ouvrir le journal. Code d'erreur : 0x%1!8.8X!.|  
|0xC001404C|-1073659828|DTS_E_GETCONNECTIONINFOS|Échec de l'obtention de la collection ConnectionInfos avec l'erreur 0x%1!8.8X! « %2 ». Cette erreur se produit lorsque l'appel à IDTSApplication100::get_ConnectionInfos échoue.|  
|0xC001404D|-1073659827|DTS_E_VARIABLEDEADLOCK|Blocage détecté lors de la tentative de verrouillage de variables. Les verrous ne peuvent être obtenus à l'issue de 16 tentatives. Le délai d'attente des verrous a expiré.|  
|0xC001404E|-1073659826|DTS_E_NOTDISPENSED|La collection Variables n'a pas été retournée de VariableDispenser. L'opération qui a été tentée n'est autorisée que sur les collections distribuées.|  
|0xC001404F|-1073659825|DTS_E_VARIABLESALREADYUNLOCKED|Cette collection Variables a déjà été déverrouillée. La méthode Unlock n'est appelée qu'une seule fois sur une collection Variables distribuée.|  
|0xC0014050|-1073659824|DTS_E_VARIABLEUNLOCKFAILED|Échec du déverrouillage d'une ou plusieurs variables.|  
|0xC0014051|-1073659823|DTS_E_DISPENSEDREADONLY|La collection Variables est retournée de VariableDispenser et ne peut être modifiée. Il n'est pas possible d'ajouter des éléments à une collection distribuée ni d'en supprimer.|  
|0xC0014052|-1073659822|DTS_E_VARIABLEALREADYONREADLIST|La variable « %1 » figure déjà dans la liste des verrouillages en lecture. Une variable ne peut être ajoutée qu'une seule fois soit à la liste des verrous de lecture, soit à celle des verrous d'écriture.|  
|0xC0014053|-1073659821|DTS_E_VARIABLEALREADYONWRITELIST|La variable « %1 » figure déjà dans la liste des verrouillages en écriture. Une variable ne peut être ajoutée qu'une seule fois soit à la liste des verrous de lecture, soit à celle des verrous d'écriture.|  
|0xC0014054|-1073659820|DTS_E_LOCKVARIABLEFORREAD|Impossible de verrouiller la variable « %1 » pour l'accès en lecture avec l'erreur 0x%2!8.8X! "%3".|  
|0xC0014055|-1073659819|DTS_E_LOCKVARIABLEFORWRITE|Impossible de verrouiller la variable « %1 » pour l'accès en lecture/écriture avec l'erreur 0x%2!8.8X! "%3".|  
|0xC0014056|-1073659818|DTS_E_CUSTOMEVENTCONFLICT|L'événement personnalisé « %1 » est déjà déclaré avec une autre liste de paramètres. Une tâche essaye de déclarer un événement personnalisé qu'une autre tâche a déjà déclaré avec une liste de paramètres différente.|  
|0xC0014057|-1073659817|DTS_E_EVENTHANDLERNOTALLOWED|La tâche fournissant l'événement personnalisé « %1 » ne permet pas à cet événement d'être géré dans le package. L'événement personnalisé a été déclaré avec AllowEventHandlers = FALSE.|  
|0xC0014059|-1073659815|DTS_E_UNSAFEVARIABLESALREADYSET|VariableDispenser a reçu une collection Variables non fiable. Cette opération ne peut pas être répétée.|  
|0xC001405A|-1073659814|DTS_E_INVALIDPARENTPACKAGEPATH|GetPackagePath a été appelé sur ForEachEnumerator, mais aucun chemin d'accès du package ForEachLoop n'a été spécifié.|  
|0xC001405B|-1073659813|DTS_E_VARIABLEDEADLOCK_READ|Un blocage a été détecté lors de la tentative de verrouillage de la variable « %1 » pour l'accès en lecture. Un verrou n'a pas pu être obtenu à l'issue de 16 tentatives et son délai d'attente a expiré.|  
|0xC001405C|-1073659812|DTS_E_VARIABLEDEADLOCK_READWRITE|Un blocage a été détecté lors de la tentative de verrouillage des variables « %1 » pour l’accès en lecture/écriture. Un verrou ne peut pas être obtenu à l’issue de 16 tentatives. Le délai d'attente des verrous a expiré.|  
|0xC001405D|-1073659811|DTS_E_VARIABLEDEADLOCK_BOTH|Un blocage a été détecté lors de la tentative de verrouillage des variables « %1 » pour l'accès en lecture, et des variables « %2 » pour l'accès en lecture/écriture. Un verrou ne peut pas être obtenu à l’issue de 16 tentatives. Le délai d'attente des verrous a expiré.|  
|0xC001405E|-1073659810|DTS_E_PACKAGEPASSWORDEMPTY|Le niveau de protection du package nécessite un mot de passe, mais la propriété PackagePassword est vide.|  
|0xC001405F|-1073659809|DTS_E_DECRYPTXML_PASSWORD|Impossible de déchiffrer un nœud XML chiffré, car le mot de passe n'a pas été spécifié ou est incorrect. Le chargement du package tentera de poursuivre l'opération sans les informations chiffrées.|  
|0xC0014060|-1073659808|DTS_E_DECRYPTPACKAGE_USERKEY|Impossible de déchiffrer un package qui est chiffré avec une clé utilisateur. Vous n'êtes peut-être pas l'utilisateur qui a chiffré le package ou vous n'utilisez pas le même ordinateur qui a servi à enregistrer le package.|  
|0xC0014061|-1073659807|DTS_E_SERVERSTORAGEDISALLOWED|Le niveau de protection, ServerStorage, ne peut pas être utilisé lors de l'enregistrement dans cette destination. Le système ne peut pas vérifier que la destination prend en charge la fonction de stockage sécurisé.|  
|0xC0014062|-1073659806|DTS_E_LOADFROMSQLSERVER|La méthode LoadFromSQLServer a échoué.|  
|0xC0014063|-1073659805|DTS_E_SIGNATUREPOLICYVIOLATION|Impossible de charger le package, car l'état de la signature numérique ne respecte pas la stratégie de signature. Erreur 0x%1!8.8X! "%2"|  
|0xC0014064|-1073659804|DTS_E_SIGNATURENOTPRESENT|Le package n'est pas signé.|  
|0xC0014065|-1073659803|DTS_E_SQLPROFILERDLL_ONLY_X86|Le module fournisseur d'informations de SQL Server Profiler n'a pas réussi à charger pfclnt.dll car il est pris en charge uniquement sur les systèmes 32 bits.|  
|0xC0014100|-1073659648|DTS_E_NAMEALREADYADDED|Impossible d'ajouter l'objet, car un autre objet portant le même nom existe déjà dans la collection. Utilisez un autre nom pour résoudre cette erreur.|  
|0xC0014101|-1073659647|DTS_E_NAMEALREADYEXISTS|Le nom d'objet ne peut être changé de « %1 » en « %2 », car un autre objet de la collection porte déjà ce nom. Utilisez un autre nom pour résoudre cette erreur.|  
|0xC0014103|-1073659645|DTS_E_FAILEDDEPENDENCIES|Une erreur s'est produite lors de l'énumération des dépendances du package. Recherchez plus d'informations dans les autres messages.|  
|0xC0014104|-1073659644|DTS_E_INVALIDCHECKPOINT_TRANSACTION|Les paramètres du package actuel ne sont pas pris en charge.  Modifiez la propriété SaveCheckpoints ou TransactionOption.|  
|0xC001410E|-1073659634|DTS_E_CONNECTIONMANAGERJOINTRANSACTION|Le gestionnaire de connexions n'a pas pu sortir de la transaction.|  
|0xC0015001|-1073655807|DTS_E_BPDUPLICATE|L'ID de point d'arrêt spécifié existe déjà. Cette erreur se produit lorsqu'une tâche appelle CreateBreakpoint plusieurs fois avec le même ID. Il est possible de créer un point d'arrêt plusieurs fois avec le même ID si la tâche appelle RemoveBreakpoint à la première création, avant de créer le deuxième.|  
|0xC0015002|-1073655806|DTS_E_BPUNKNOWNID|L'ID de point d'arrêt spécifié n'existe pas. Cette erreur se produit lorsqu'une tâche fait référence à un point d'arrêt qui n'existe pas.|  
|0xC0015004|-1073655804|DTS_E_CANTWRITETOFILE|Impossible d'ouvrir le fichier « %1 » pour l'écriture. Le fichier est en lecture seule ou vous ne disposez pas des autorisations appropriées.|  
|0xC0015005|-1073655803|DTS_E_NOROWSETRETURNED|Aucun ensemble de lignes de résultat n'est associé à l'exécution de cette requête. Le résultat n'est pas correctement spécifié.|  
|0xC0015105|-1073655547|DTS_E_DUMP_FAILED|Les fichiers de vidage du débogage n'ont pas été générés correctement. Le hresult est 0x%1!8.8X!.|  
|0xC0016001|-1073651711|DTS_E_INVALIDURL|L'URL spécifiée n'est pas valide. Ceci peut se produire lorsque l'URL du serveur ou du proxy a la valeur NULL ou en cas de format incorrect. Un format d’URL valide est de la forme http://ServerName:Port/ResourcePath ou https://ServerName:Port/ResourcePath.|  
|0xC0016002|-1073651710|DTS_E_INVALIDSCHEME|L'URL %1 n'est pas valide. Ceci peut se produire lorsqu'un schéma autre que http ou https est spécifié, ou que le format de l'URL est incorrect. Un format d’URL valide est de la forme http://ServerName:Port/ResourcePath ou https://ServerName:Port/ResourcePath.|  
|0xC0016003|-1073651709|DTS_E_WINHTTPCANNOTCONNECT|La connexion au serveur %1 ne peut pas être établie. Cette erreur peut se produire lorsque le serveur n'existe pas, ou que les paramètres du proxy sont incorrects.|  
|0xC0016004|-1073651708|DTS_E_CONNECTIONTERMINATED|La connexion avec le serveur a été réinitialisée ou s'est interrompue. Réessayez plus tard.|  
|0xC0016005|-1073651707|DTS_E_LOGINFAILURE|La tentative de connexion a échoué pour « %1 ». Cette erreur se produit lorsque les informations d'identification de connexion fournies sont incorrectes. Vérifiez ces informations.|  
|0xC0016006|-1073651706|DTS_E_INVALIDSERVERNAME|Impossible de résoudre le nom de serveur spécifié dans l'URL %1.|  
|0xC0016007|-1073651705|DTS_E_PROXYAUTH|Échec de l'authentification du proxy. Cette erreur se produit lorsque les informations d'identification de connexion ne sont pas fournies, ou sont incorrectes.|  
|0xC0016008|-1073651704|DTS_E_SECUREFAILURE|La réponse de certificat SSL obtenue du serveur n'est pas valide. Impossible de traiter la requête.|  
|0xC0016009|-1073651703|DTS_E_TIMEOUT|Le délai d'attente de la requête a expiré. Cette erreur peut se produire lorsque le délai d'attente spécifié est trop court ou lorsqu'une connexion au serveur ou au proxy ne peut pas être établie. Assurez-vous que l'URL du serveur et l'URL du proxy sont correctes.|  
|0xC001600A|-1073651702|DTS_E_CLIENTAUTH|Un certificat client est absent. Cette erreur se produit lorsque le serveur attend un certificat client SSL et que l'utilisateur a fourni un certificat non valide, ou n'a fourni aucun certificat. Un certificat client doit être configuré pour cette connexion.|  
|0xC001600B|-1073651701|DTS_E_REDIRECTFAILURE|Le serveur spécifié, URL %1, contient une redirection, et la demande de redirection a échoué.|  
|0xC001600C|-1073651700|DTS_E_SERVERAUTH|Échec de l'authentification du serveur. Cette erreur se produit lorsque les informations d'identification de connexion ne sont pas fournies, ou sont incorrectes.|  
|0xC001600D|-1073651699|DTS_E_WINHTTPUNKNOWNERROR|Impossible de traiter la requête. Réessayez plus tard.|  
|0xC001600E|-1073651698|DTS_E_UNKNOWNSTATUSCODE|Le serveur a retourné le code - %1!u! : %2. Cette erreur se produit lorsque le serveur rencontre des problèmes.|  
|0xC001600F|-1073651697|DTS_E_WINHTTPNOTSUPPORTED|Cette plateforme n'est pas prise en charge par les services WinHttp.|  
|0xC0016010|-1073651696|DTS_E_INVALIDTIMEOUT|La valeur d'expiration n'est pas valide. L'expiration doit être comprise entre %1!d! vers %2!d! (en secondes).|  
|0xC0016011|-1073651695|DTS_E_INVALIDCHUNKSIZE|La valeur du chunk n'est pas valide. La valeur de la propriété ChunkSize doit être comprise entre %1!d! vers %2!d! (en Ko).|  
|0xC0016012|-1073651694|DTS_E_CERTERROR|Erreur lors du traitement du certificat client. Cette erreur peut se produire lorsque le certificat client fourni n'a pas été trouvé dans le magasin de certificats personnels. Vérifiez que le certificat client est valide.|  
|0xC0016013|-1073651693|DTS_E_FORBIDDEN|Le serveur a retourné le code d'erreur « 403 - Interdit ». Cette erreur peut se produire lorsque la ressource spécifiée requiert un accès « https », mais que la période de validité du certificat a expiré, que le certificat n'est pas valide pour l'utilisation requise ou que le certificat a été révoqué, ou encore que la révocation n'a pas pu être vérifiée.|  
|0xC0016014|-1073651692|DTS_E_WINHTTPOPEN|Erreur lors de l'initialisation de la session HTTP avec le proxy « %1 ». Cette erreur peut se produire lorsqu'un proxy non valide a été spécifié. Le gestionnaire de connexions HTTP ne prend en charge que les proxys de type CERN.|  
|0xC0016015|-1073651691|DTS_E_OPENCERTSTORE|Erreur lors de l'ouverture du magasin de certificats.|  
|0xC0016016|-1073651690|DTS_E_UNPROTECTXMLFAILED|Impossible de déchiffrer le nœud XML protégé « %1 », avec le code d'erreur 0x%2!8.8X! "%3". Vous n'êtes peut-être pas autorisé à accéder à ces informations. Cette erreur se produit en cas d'erreur de chiffrement. Vérifiez que la clé appropriée est disponible.|  
|0xC0016017|-1073651689|DTS_E_UNPROTECTCONNECTIONSTRINGFAILED|Impossible de déchiffrer la chaîne de connexion protégée pour le serveur « %1 », avec le code d'erreur 0x%2!8.8X! "%3". Vous n'êtes peut-être pas autorisé à accéder à ces informations. Cette erreur se produit en cas d'erreur de chiffrement. Vérifiez que la clé appropriée est disponible.|  
|0xC0016018|-1073651688|DTS_E_NEGATIVEVERSION|Le numéro de version ne peut pas être négatif. Cette erreur se produit lorsque la propriété VersionMajor, VersionMinor ou VersionBuild du package est définie sur une valeur négative.|  
|0xC0016019|-1073651687|DTS_E_PACKAGEMIGRATED|Le package a subi une migration vers une version ultérieure au cours du chargement. Il doit être rechargé pour terminer le processus. Ceci est un code d'erreur interne.|  
|0xC0016020|-1073651680|DTS_E_PACKAGEMIGRATIONFAILED|Échec de la migration du package de la version %1!d! vers la version %2!d! a échoué avec le code d’erreur 0x%3!8.8X! « %4 ».|  
|0xC0016021|-1073651679|DTS_E_PACKAGEMIGRATIONMODULELOAD|Le module de migration du package n'a pas réussi le chargement.|  
|0xC0016022|-1073651678|DTS_E_PACKAGEMIGRATIONMODULE|Le module de migration du package a échoué.|  
|0xC0016023|-1073651677|DTS_E_CANTDETERMINEWHICHPROPTOPERSIST|Impossible d'assurer la persistance de l'objet à l'aide de la persistance par défaut. Cette erreur se produit lorsque la persistance par défaut ne parvient pas à déterminer quels objets sont sur l'objet hébergé.|  
|0xC0016024|-1073651676|DTS_E_CANTADDREMOVEWHENEXECUTING|Impossible d'ajouter ou de supprimer un objet d'un package en mode d'exécution. Cette erreur se produit lorsqu'une tentative est effectuée d'ajouter ou de supprimer un objet d'une collection alors que le package est en cours d'exécution.|  
|0xC0016025|-1073651675|DTS_E_NODENOTFOUND|Le nœud « %1 » est introuvable dans la persistance par défaut personnalisée. Cette erreur se produit si le XML enregistré par défaut d'un objet extensible a été modifié de telle sorte qu'un objet enregistré est introuvable, ou si l'objet extensible lui-même a changé.|  
|0xC0016026|-1073651674|DTS_E_COLLECTIONLOCKED|Cette collection ne peut pas être modifiée au cours de la validation ou de l'exécution du package.|  
|0xC0016027|-1073651673|DTS_E_COLLOCKED|La collection « %1 » ne peut pas être modifiée au cours de la validation ou de l'exécution du package. Impossible d'ajouter « %2 » à la collection.|  
|0xC0016029|-1073651671|DTS_E_FTPNOTCONNECTED|La connexion avec le serveur FTP n'a pas été établie.|  
|0xC001602A|-1073651670|DTS_E_FTPERROR|Une erreur s'est produite au cours de l'opération FTP demandée. Description détaillée de l'erreur : %1.|  
|0xC001602B|-1073651669|DTS_E_FTPINVALIDRETRIES|Le nombre de tentatives n'est pas valide. Ce nombre doit être compris entre %1!d! et %2!d!.|  
|0xC001602C|-1073651668|DTS_E_LOADWININET|Le gestionnaire de connexions FTP a besoin de la DLL suivante pour fonctionner : %1.|  
|0xC001602D|-1073651667|DTS_E_FTPINVALIDCONNECTIONSTRING|Le port spécifié dans la chaîne de connexion n'est pas valide. Le format de ConnectionString est ServerName:Port. La valeur de Port doit être un entier compris entre %1!d! et %2!d!.|  
|0xC001602E|-1073651666|DTS_E_FTPCREATEFOLDER|Création du dossier « %1 » ... %2.|  
|0xC001602F|-1073651665|DTS_E_FTPDELETEFOLDER|Suppression du dossier « %1 » ... %2.|  
|0xC0016030|-1073651664|DTS_E_FTPCHANGEFOLDER|Passage du répertoire actuel à « %1 ». %2.|  
|0xC0016031|-1073651663|DTS_E_FTPFILESEMPTY|Aucun transfert de fichier. Cette erreur peut se produire lorsqu'au cours d'une opération d'envoi ou de réception, aucun fichier n'est spécifié pour le transfert.|  
|0xC0016032|-1073651662|DTS_E_FTPINVALIDLOCALPATH|Le chemin d'accès local spécifié n'est pas valide. Spécifiez un chemin d'accès local valide. Ceci peut se produire lorsque le chemin d'accès local est vide.|  
|0xC0016033|-1073651661|DTS_E_FTPNOFILESTODELETE|Aucun fichier n'est spécifié pour la suppression.|  
|0xC0016034|-1073651660|DTS_E_WINHTTPCERTDECODE|Une erreur interne s'est produite lors du chargement du certificat. Cette erreur peut se produire lorsque les données du certificat ne sont pas valides.|  
|0xC0016035|-1073651659|DTS_E_WINHTTPCERTENCODE|Une erreur interne s'est produite lors de l'enregistrement des données du certificat.|  
|0xC0016049|-1073651639|DTS_E_CHECKPOINTMISMATCH|Le fichier de point de contrôle « %1 » ne correspond pas à ce package. L'ID du package et l'ID du fichier de point de contrôle ne correspondent pas.|  
|0xC001604A|-1073651638|DTS_E_CHECKPOINTFILEALREADYEXISTS|Un fichier de point de contrôle existant a été détecté avec du contenu ne semblant pas correspondre à ce package, de sorte que le fichier ne peut pas être écrasé pour commencer l'enregistrement des nouveaux points de contrôle. Supprimez le fichier de vérification existant et réessayez. Cette erreur se produit lorsqu'un fichier de point de contrôle est présent, que le package est configuré pour ne pas utiliser de fichier de point de contrôle, mais pour enregistrer les points de contrôle. Le fichier de point de contrôle ne sera pas écrasé.|  
|0xC001604B|-1073651637|DTS_E_CHECKPOINTFILELOCKED|Le fichier de point de contrôle « %1 » est verrouillé par un autre processus. Cette erreur peut se produire si une autre instance de ce package est en cours d'exécution.|  
|0xC001604C|-1073651636|DTS_E_OPENCHECKPOINTFILE|Le fichier de point de contrôle « %1 » n'a pas pu s'ouvrir en raison de l'erreur 0x%2!8.8X! "%3".|  
|0xC001604D|-1073651635|DTS_E_CREATECHECKPOINTFILE|Le fichier de point de contrôle « %1 » a échoué au cours de la création en raison de l'erreur 0x%2!8.8X! "%3".|  
|0xC0016050|-1073651632|DTS_E_FTPINVALIDPORT|Le port FTP contient une valeur non valide. La valeur du port FTP doit être un entier compris entre %1!d! et %2!d!.|  
|0xC00160AA|-1073651542|DTS_E_CONNECTTOSERVERFAILED|La connexion au service SSIS sur l'ordinateur « %1 » a échoué :<br /><br /> %2.|  
|0xC0017002|-1073647614|DTS_E_PROPERTYEXPRESSIONSDISABLEDONVARIABLES|La propriété Expression n'est pas prise en charge sur les objets Variable. Utilisez la propriété EvaluateAsExpression à la place.|  
|0xC0017003|-1073647613|DTS_E_PROPERTYEXPRESSIONEVAL|L'expression « %1 » sur la propriété « %2 » ne peut pas être évaluée. Modifiez l'expression pour qu'elle soit valide.|  
|0xC0017004|-1073647612|DTS_E_PROPERTYEXPRESSIONSET|Le résultat de l'expression « %1 » sur la propriété « %2 » ne peut pas être écrit dans la propriété. L'expression a été évaluée, mais ne peut pas être définie sur la propriété.|  
|0xC0017005|-1073647611|DTS_E_FORLOOPEVALEXPRESSIONINVALID|L'expression d'évaluation de la boucle n'est pas valide. L'expression doit être modifiée. Il doit y avoir des messages d'erreur supplémentaires.|  
|0xC0017006|-1073647610|DTS_E_EXPRESSIONNOTBOOLEAN|L'expression « %1 » doit être équivalente à True ou False. Modifiez l'expression pour qu'elle corresponde à une valeur booléenne.|  
|0xC0017007|-1073647609|DTS_E_FORLOOPHASNOEXPRESSION|La boucle n'a aucune expression à évaluer. Cette erreur se produit lorsque l'expression sur la boucle For est vide. Ajoutez une expression.|  
|0xC0017008|-1073647608|DTS_E_FORLOOPASSIGNEXPRESSIONINVALID|L'expression d'assignation de la boucle n'est pas valide et doit être modifiée. Il doit y avoir des messages d'erreur supplémentaires.|  
|0xC0017009|-1073647607|DTS_E_FORLOOPINITEXPRESSIONINVALID|L'expression d'initialisation de la boucle n'est pas valide et doit être modifiée. Il doit y avoir des messages d'erreur supplémentaires.|  
|0xC001700A|-1073647606|DTS_E_INVALIDVERSIONNUMBER|Le numéro de version du package n'est pas valide. Le numéro de version ne peut pas être supérieur au numéro de version actuel.|  
|0xC001700C|-1073647604|DTS_E_INVALIDVERNUMCANTBENEGATIVE|Le numéro de version du package n'est pas valide. Le numéro de version est négatif.|  
|0xC001700D|-1073647603|DTS_E_PACKAGEUPDATEDISABLED|Le package présente une ancienne version de format, mais la mise à niveau automatique du format du package est désactivée.|  
|0xC001700E|-1073647602|DTS_E_EXPREVALTRUNCATIONASERROR|Une troncation s'est produite au cours de l'évaluation de l'expression,|  
|0xC0019001|-1073639423|DTS_E_FAILEDSETEXECVALVARIABLE|Le wrapper n'a pas pu définir la valeur de la variable spécifiée dans la propriété ExecutionValueVariable.|  
|0xC0019004|-1073639420|DTS_E_VARIABLEEXPRESSIONERROR|L'évaluation de l'expression de la variable « %1 » a échoué. Il y avait une erreur dans l'expression.|  
|0xC0019305|-1073638651|DTS_E_UNSUPPORTEDSQLVERSION|L'opération tentée n'est pas prise en charge avec cette version de la base de données.|  
|0xC001A003|-1073635325|DTS_E_TXNSPECINVALID|La transaction ne peut être spécifiée lorsqu'une connexion maintenue est utilisée. Cette erreur se produit lorsque Retain a la valeur TRUE dans le gestionnaire de connexions, mais que AcquireConnection a été appelé avec un paramètre de transaction non NULL.|  
|0xC001A004|-1073635324|DTS_E_INCOMPATIBLETRANSACTIONCONTEXT|Un contexte de transaction incompatible a été spécifié pour une connexion maintenue. Cette connexion a été établie dans un autre contexte de transaction. Les connexions maintenues peuvent être utilisées dans un seul contexte de transaction.|  
|0xC001B001|-1073631231|DTS_E_NOTSUSPENDED|L'appel de reprise a échoué, car le package n'est pas interrompu. Ceci se produit lorsque les appels clients reprennent, mais le package n'est pas interrompu.|  
|0xC001B002|-1073631230|DTS_E_ALREADYEXECUTING|L'appel d'exécution a échoué, car l'exécutable est déjà en cours d'exécution. Cette erreur se produit lorsque le client appelle Execute sur un conteneur qui est encore en cours d'exécution du dernier appel d'exécution.|  
|0xC001B003|-1073631229|DTS_E_NOTEXECUTING|L'appel d'interruption ou de reprise a échoué, car l'exécutable n'est pas en cours d'exécution, ou n'est pas l'exécutable de premier niveau. Ceci se produit lorsque les appels clients s'interrompent ou reprennent sur un exécutable qui n'est pas en train de traiter un appel d'exécution.|  
|0xC001C002|-1073627134|DTS_E_INVALIDFILE|Le fichier spécifié dans l'énumérateur For Each File n'est pas valide. Vérifiez si le fichier spécifié dans l'énumérateur For Each File existe.|  
|0xC001C010|-1073627120|DTS_E_VALUEINDEXNOTINTEGER|L’index de valeur n’est pas un entier. Mappage d'un numéro de variable For Each %1!d! à la variable « %2 ».|  
|0xC001C011|-1073627119|DTS_E_VALUEINDEXNEGATIVE|L'index de valeur est négatif. Le numéro de mappage de la variable ForEach %1!d! sur la variable « %2 ».|  
|0xC001C012|-1073627118|DTS_E_FOREACHVARIABLEMAPPING|Impossible d'appliquer le numéro de mappage de la variable ForEach %1!d! à la variable « %2 ».|  
|0xC001C013|-1073627117|DTS_E_OBJECTNOTINFOREACHLOOP|Erreur lors de l'ajout d'un objet à ForEachPropertyMapping qui n'est pas un enfant direct du conteneur ForEachLoop.|  
|0xC001F001|-1073614847|DTS_E_FAILEDSYSTEMVARIABLEREMOVE|Impossible de supprimer une variable système. Cette erreur se produit lors de la suppression d'une variable qui est obligatoire.  Les variables obligatoires sont des variables créées par le runtime pour que celui-ci puisse communiquer avec les tâches.|  
|0xC001F002|-1073614846|DTS_E_CHANGESYSTEMVARIABLEREADONLYFAILED|La modification de la propriété d'une variable a échoué, car c'est une variable système. Les variables système sont en lecture seule.|  
|0xC001F003|-1073614845|DTS_E_CHANGESYSTEMVARIABLENAMEFAILED|Le changement de nom d'une variable a échoué, car c'est une variable système. Les variables système sont en lecture seule.|  
|0xC001F004|-1073614844|DTS_E_CHANGESYSTEMVARIABLENAMESPACEFAILED|Le changement d'espace de noms d'une variable a échoué, car c'est une variable système. Les variables système sont en lecture seule.|  
|0xC001F006|-1073614842|DTS_E_EVENTHANDLERNAMEREADONLY|Le changement de nom du gestionnaire d'événements a échoué. Les noms de gestionnaires d'événements sont en lecture seule.|  
|0xC001F008|-1073614840|DTS_E_PATHUNKNOWN|Impossible d'extraire le chemin d'accès à l'objet. Ceci est une erreur système.|  
|0xC001F009|-1073614839|DTS_E_RUNTIMEVARIABLETYPECHANGE|Le type de valeur assignée à la variable « %1 » est différent du type de variable actuel. Les variables ne peuvent pas changer de type en cours d'exécution. Les types des variables sont stricts, à l'exception des variables de type Object.|  
|0xC001F010|-1073614832|DTS_E_INVALIDSTRING|Caractères non valides dans la chaîne : « %1 ». Ceci se produit lorsqu'une chaîne fournie pour une valeur de propriété contient des caractères non imprimables.|  
|0xC001F011|-1073614831|DTS_E_INVALIDOBJECTNAME|Le nom d'objet SSIS n'est pas valide. Des erreurs plus spécifiques auraient été déclenchées expliquant le problème de dénomination exact.|  
|0xC001F021|-1073614815|DTS_E_PROPERTYREADONLY|La propriété « %1 » est en lecture seule. Ceci se produit lorsqu'une modification a été tentée sur une propriété en lecture seule.|  
|0xC001F022|-1073614814|DTS_E_FAILEDGETTYPEINFO|L'objet ne prend pas en charge les informations de type. Ceci se produit lorsque le runtime tente d'obtenir les informations de type d'un objet pour remplir la collection Properties.  L'objet doit prendre en charge les informations de type.|  
|0xC001F023|-1073614813|DTS_E_FAILEDPROPERTYGET|Une erreur s'est produite lors de l'extraction de la valeur de la propriété « %1 ». Le code d'erreur est 0x%2!8.8X!.|  
|0xC001F024|-1073614812|DTS_E_FAILEDPROPERTYGET_ERRORINFO|Une erreur s'est produite lors de l'extraction de la valeur de la propriété « %1 ». Le code d'erreur est 0x%2!8.8X! "%3".|  
|0xC001F025|-1073614811|DTS_E_FAILEDPROPERTYSET|Une erreur s'est produite lors de la définition de la valeur de la propriété « %1 ». L'erreur retournée est 0x%2!8.8X!.|  
|0xC001F026|-1073614810|DTS_E_FAILEDPROPERTYSET_ERRORINFO|Une erreur s'est produite lors de la définition de la valeur de la propriété « %1 ». L'erreur retournée est 0x%2!8.8X! "%3".|  
|0xC001F027|-1073614809|DTS_E_PROPERTYWRITEONLY|La propriété « %1 » est en écriture seule. Cette erreur se produit lors de la tentative d'extraction de la valeur d'une propriété par le biais d'un objet de propriété alors que la propriété est en écriture seule.|  
|0xC001F028|-1073614808|DTS_E_NODISPATCH|L'objet n'implémente pas IDispatch. Cette erreur se produit lorsqu'un objet Property ou une collection Properties tente d'accéder à une interface IDispatch sur un objet.|  
|0xC001F029|-1073614807|DTS_E_NOCONTAININGTYPELIB|Impossible d'extraire la bibliothèque de types de l'objet. Cette erreur se produit lorsque la collection Properties tente d'extraire la bibliothèque de types d'un objet par le biais de son interface IDispatch.|  
|0xC001F02A|-1073614806|DTS_E_INVALIDTASKMONIKER|Impossible de créer une tâche à partir de XML pour la tâche « %1!s! », type « %2!s! » en raison de l’erreur « 0x%3!8.8X! « %4!s! ».|  
|0xC001F02C|-1073614804|DTS_E_FAILEDCREATEXMLDOCUMENT|Impossible de créer un document XML « %1 ».|  
|0xC001F02D|-1073614803|DTS_E_PMVARPROPTYPESDIFFERENT|Une erreur s'est produite, car il y a un mappage de propriété d'une variable vers une propriété dont le type est différent. Le type de la propriété et le type de la variable doivent être identiques.|  
|0xC001F02E|-1073614802|DTS_E_PMINVALIDPROPMAPTARGET|Une tentative a été effectuée pour définir un mappage de propriété de sorte à cibler un type d'objet non pris en charge. Cette erreur se produit lors du passage d'un type d'objet non pris en charge à un mappage de propriété.|  
|0xC001F02F|-1073614801|DTS_E_COULDNOTRESOLVEPACKAGEPATH|Impossible de résoudre un chemin de package vers un objet dans le package « %1 ».  Vérifiez que le chemin d'accès du package est valide.|  
|0xC001F030|-1073614800|DTS_E_PMNODESTPROPERTY|La propriété de destination du mappage de propriété est vide. Définissez le nom de la propriété de destination.|  
|0xC001F031|-1073614799|DTS_E_INVALIDPROPERTYMAPPINGSFOUND|Le package n'a pas réussi à restaurer au moins un mappage de propriété.|  
|0xC001F032|-1073614798|DTS_E_AMBIGUOUSVARIABLENAME|Le nom de variable est ambigu car plusieurs variables portant ce nom existent dans différents espaces de noms. Spécifiez un nom qualifié par un espace de noms pour éviter toute ambiguïté.|  
|0xC001F033|-1073614797|DTS_E_DESTINATIONOBJECTPARENTLESS|L'objet de destination d'un mappage de propriété n'a pas de parent. Il n'est pas l'enfant d'un conteneur de séquences. Il a peut-être été supprimé du package.|  
|0xC001F036|-1073614794|DTS_E_INVALIDPROPERTYMAPPING|Le mappage de propriété n'est pas valide. Le mappage est ignoré.|  
|0xC001F038|-1073614792|DTS_E_PMFAILALERTREMOVE|Erreur en alertant les mappages de propriété qu'une cible est supprimée.|  
|0xC001F03A|-1073614790|DTS_E_INVALIDFOREACHPROPERTYMAPPING|Mappage de propriété non valide détecté sur la boucle For Each. Ceci se produit lorsque la restauration du mappage de la propriété ForEach échoue.|  
|0xC001F040|-1073614784|DTS_E_PMPROPERTYINVALID|Une propriété de destination a été spécifiée sur un mappage de propriété non valide. Ceci se produit lorsqu'une propriété est spécifiée sur un objet de destination qui est introuvable sur cet objet.|  
|0xC001F041|-1073614783|DTS_E_INVALIDTASKMONIKERNOPARAM|Impossible de créer une tâche à partir de XML. Ceci se produit lorsque le runtime ne parvient pas à résoudre le nom pour créer une tâche. Vérifiez que le nom est correct.|  
|0xC001F080|-1073614720|DTS_E_COULDNOTREPLACECHECKPOINTFILE|Impossible de remplacer le fichier de point de contrôle existant par le fichier de point de contrôle mis à jour. Le point de contrôle a été correctement créé dans un fichier temporaire, mais le remplacement du fichier existant par le nouveau fichier a échoué.|  
|0xC001F081|-1073614719|DTS_E_CHECKPOINTFILENOTSPECIFIED|Le package est configuré pour toujours redémarrer à partir d'un point de contrôle, mais le fichier de point de contrôle n'est pas spécifié.|  
|0xC001F082|-1073614718|DTS_E_CHECKPOINTLOADXML|La tentative de chargement du fichier de point de contrôle XML « %1 » a échoué, avec l'erreur 0x%2!8.8X! "%3". Vérifiez que le nom de fichier est correct et que le fichier existe.|  
|0xC001F083|-1073614717|DTS_E_LOADCHECKPOINT|Le package a échoué en cours d'exécution, car il est impossible de charger le fichier de point de contrôle. La poursuite de l'exécution du package nécessite un fichier de point de vérification. Cette erreur se produit généralement lorsque la propriété CheckpointUsage est définie sur la valeur ALWAYS, qui spécifie que le package redémarre toujours.|  
|0xC001F185|-1073614459|DTS_E_NOEVALEXPRESSION|L'expression de condition d'évaluation sur la boucle For « %1 » est vide. Il doit y avoir une expression d'évaluation booléenne dans la boucle For.|  
|0xC001F186|-1073614458|DTS_E_EXPREVALASSIGNMENTTYPEMISMATCH|Le résultat de l'expression d'assignation « %1 » ne peut pas être converti en un type qui est compatible avec la variable à laquelle il a été assigné.|  
|0xC001F187|-1073614457|DTS_E_EXPREVALASSIGNMENTTOREADONLYVARIABLE|Erreur lors de l'utilisation d'une variable en lecture seule « %1 » dans une expression d'assignation. Le résultat de l'expression ne peut pas être assigné à la variable, car celle-ci est en lecture seule. Choisissez une variable dans laquelle il est possible d'écrire, ou supprimez l'expression de cette variable.|  
|0xC001F188|-1073614456|DTS_E_EXPREVALASSIGNMENTVARIABLELOCKFORWRITEFAILED|Impossible d'évaluer l'expression « %1 », car la variable « %2 » n'existe pas ou n'est pas accessible en écriture. Le résultat de l'expression ne peut pas être assigné à la variable, car la variable est introuvable ou n'a pas pu être verrouillée pour l'accès en écriture.|  
|0xC001F189|-1073614455|DTS_E_EXPREVALRESULTTYPENOTSUPPORTED|L'expression « %1 » a un type de résultat de « %2 » qui ne peut pas être converti en un type pris en charge.|  
|0xC001F18A|-1073614454|DTS_E_EXPREVALRESULTTYPECONVERSIONFAILED|La conversion du résultat de l'expression « %1 » du type « %2 » en un type pris en charge a échoué, avec le code d'erreur 0x%3!8.8X!. Une erreur inattendue s'est produite en essayant de convertir le résultat de l'expression en un type pris en charge par le moteur du runtime, même si la conversion du type est prise en charge.|  
|0xC001F200|-1073614336|DTS_E_DTSNAME_NOTNULL|Le nom d'objet n'est pas valide. Le nom ne peut pas avoir la valeur NULL.|  
|0xC001F201|-1073614335|DTS_E_DTSNAME_NOTEMPTY|Le nom d'objet n'est pas valide. Le nom ne peut pas être vide.|  
|0xC001F202|-1073614334|DTS_E_DTSNAME_LEGAL|Le nom d'objet « %1 » n'est pas valide. Le nom ne peut pas contenir les caractères suivants : / \ : [ ] . =|  
|0xC001F203|-1073614333|DTS_E_DTSNAME_PRINTABLE|Le nom d'objet « %1 » n'est pas valide. Le nom ne peut pas contenir des caractères de contrôle qui le rendent non imprimable.|  
|0xC001F204|-1073614332|DTS_E_DTSNAME_NOLEADWHITESP|Le nom d'objet « %1 » n'est pas valide. Le nom ne peut pas commencer par un espace.|  
|0xC001F205|-1073614331|DTS_E_DTSNAME_NOTRAILWHITESP|Le nom d'objet « %1 » n'est pas valide. Le nom ne peut pas se terminer par un espace.|  
|0xC001F206|-1073614330|DTS_E_DTSNAME_BEGINSWITHALPHA|Le nom d'objet « %1 » n'est pas valide. Le nom doit commencer par un caractère alphabétique.|  
|0xC001F207|-1073614329|DTS_E_DTSNAME_BEGINSWITHALPHAUNDERBAR|Le nom d'objet « %1 » n'est pas valide. Le nom doit commencer par un caractère alphabétique ou par le trait de soulignement « _ ».|  
|0xC001F208|-1073614328|DTS_E_DTSNAME_ALPHADIGITUNDERBAR|Le nom d'objet « %1 » n'est pas valide. Le nom ne peut comporter que des caractères alphanumériques ou des traits de soulignement « _ ».|  
|0xC001F209|-1073614327|DTS_E_DTSNAME_VALIDFILENAME|Le nom d'objet « %1 » n'est pas valide. Le nom ne peut pas contenir les caractères suivants : / \ : ? " < > &#124;|  
|0xC001F420|-1073613792|DTS_E_FAILLOADINGPROPERTY|Impossible de charger la propriété de valeur « %1 » à l'aide de la persistance par défaut.|  
|0xC001F422|-1073613790|DTS_E_NODELISTENUM_INVALIDCONNMGRTYPE|Le gestionnaire de connexions « %1 » n'est pas de type « %2 »|  
|0xC001F423|-1073613789|DTS_E_NODELISTENUM_XPATHISEMPTY|« %1 » est vide|  
|0xC001F424|-1073613788|DTS_E_NODELISTENUM_INVALIDDATANODE|Nœud de données non valide dans la section de l'énumérateur de la liste de nœuds|  
|0xC001F425|-1073613787|DTS_E_NODELISTENUM_NOENUMERATORCREATED|Aucun énumérateur ne peut être créé|  
|0xC001F427|-1073613785|DTS_E_OPERATIONFAILCACHEINUSE|L'opération a échoué car le cache est en cours d'utilisation.|  
|0xC001F428|-1073613784|DTS_E_PROPERTYCANNOTBEMODIFIED|Impossible de modifier la propriété.|  
|0xC001F429|-1073613783|DTS_E_PACKAGEUPGRADEFAILED|La mise à niveau de package a échoué.|  
|0xC00220DE|-1073602338|DTS_E_TKEXECPACKAGE_UNABLETOLOADFILE|Erreur 0x%1!8.8X! lors du chargement du fichier de package « %3 ». %2.|  
|0xC00220DF|-1073602337|DTS_E_TKEXECPACKAGE_UNSPECIFIEDPACKAGE|Le package n'est pas spécifié.|  
|0xC00220E0|-1073602336|DTS_E_TKEXECPACKAGE_UNSPECIFIEDCONNECTION|La connexion n'est pas spécifiée.|  
|0xC00220E2|-1073602334|DTS_E_TKEXECPACKAGE_INCORRECTCONNECTIONMANAGERTYPE|Le gestionnaire de connexions « %1 » a un type « %2 » qui n'est pas pris en charge. Seuls les gestionnaires de connexions « FILE » et « OLEDB » sont pris en charge.|  
|0xC00220E3|-1073602333|DTS_E_TKEXECPACKAGE_UNABLETOLOADXML|Erreur 0x%1!8.8X! lors du chargement du fichier de package « %3 » dans un document XML. %2.|  
|0xC00220E4|-1073602332|DTS_E_TKEXECPACKAGE_UNABLETOLOAD|Erreur 0x%1!8.8X! lors de la préparation du chargement du package. %2.|  
|0xC0024102|-1073594110|DTS_E_TASKVALIDATIONFAILED|La méthode Validate a échoué sur la tâche et a retourné le code d'erreur 0x%1!8.8X! (%2). La méthode Validate doit réussir et indiquer le résultat à l'aide d'un paramètre de sortie.|  
|0xC0024104|-1073594108|DTS_E_TASKEXECUTEFAILED|La méthode Execute sur la tâche a retourné le code d'erreur 0x%1!8.8X! (%2). La méthode Execute doit réussir et indiquer le résultat à l'aide d'un paramètre de sortie.|  
|0xC0024105|-1073594107|DTS_E_RETRIEVINGDEPENDENCIES|Une erreur s'est produite sur la tâche « %1 » : 0x%2!8.8X! lors de l'extraction des dépendances. Le runtime extrayait les dépendances de la collection de dépendances de la tâche, lorsque l'erreur s'est produite. La tâche a peut-être implémenté de manière incorrecte une des interfaces de dépendance.|  
|0xC0024107|-1073594105|DTS_E_TASKVALIDATIONERROR|Des erreurs se sont produites au cours de la validation de la tâche.|  
|0xC0024108|-1073594104|DTS_E_CONNECTIONSTRINGFORMAT|Le format de la chaîne de connexion n'est pas valide. Il doit consister en un ou plusieurs composants de la forme X=Y, séparés par des points-virgules. Cette erreur se produit lorsqu'une chaîne de connexion avec zéro composant est définie sur le gestionnaire de connexions de la base de données.|  
|0xC0024109|-1073594103|DTS_E_UNQUOTEDSEMICOLON|Les composants de la chaîne de connexion ne peuvent pas contenir des points-virgules sans guillemets. Si la valeur doit contenir un point-virgule, ajoutez des guillemets de chaque côté de la valeur. Cette erreur se produit lorsque les valeurs de la chaîne de connexion contiennent des points-virgules sans guillemets, telle que la propriété InitialCatalog.|  
|0xC002410A|-1073594102|DTS_E_LOGPROVIDERVALIDATIONFAILED|Échec de la validation d'un ou de plusieurs modules fournisseurs d'informations Le package ne peut pas s'exécuter. Le package ne s'exécute pas lorsque la validation d'un module fournisseur d'informations échoue.|  
|0xC002410B|-1073594101|DTS_E_INVALIDVALUEINARRAY|Valeur non valide dans le tableau.|  
|0xC002410C|-1073594100|DTS_E_ENUMERATIONELEMENTNOTENUMERABLE|Un élément de l'énumérateur retourné par l'énumérateur ForEach n'implémente pas IEnumerator, contredisant la propriété CollectionEnumerator de l'énumérateur ForEach.|  
|0xC002410D|-1073594099|DTS_E_INVALIDENUMERATORINDEX|L'énumérateur n'a pas pu récupérer l'élément au niveau de l'index « %1!d! ».|  
|0xC0029100|-1073573632|DTS_E_AXTASK_MISSING_ENTRY_METHOD_NAME|Fonction introuvable.|  
|0xC0029101|-1073573631|DTS_E_AXTASK_EMPTY_SCRIPT|Fonction introuvable.|  
|0xC0029102|-1073573630|DTS_E_AXTASK_INITIALIZATION_WITH_WRONG_XML_ELEMENT|La tâche de script ActiveX a été démarrée avec un élément XML incorrect.|  
|0xC0029105|-1073573627|DTS_E_AXTASK_HANDLER_NOT_FOUND|Gestionnaire introuvable.|  
|0xC0029106|-1073573626|DTS_E_AXTASKUTIL_ENUMERATE_LANGUAGES_FAILED|Une erreur s'est produite lors de la tentative d'extraction des langages de script installés sur le système.|  
|0xC0029107|-1073573625|DTS_E_AXTASKUTIL_SCRIPTHOST_CREATE_FAILED|Une erreur s'est produite lors de la création de l'hôte de script ActiveX. Vérifiez que l'hôte de script est correctement installé.|  
|0xC0029108|-1073573624|DTS_E_AXTASKUTIL_SCRIPTHOSTINIT_FAILED|Une erreur s'est produite en essayant d'instancier l'hôte de script pour le langage choisi. Vérifiez que le langage de script que vous avez choisi est installé sur votre système.|  
|0xC0029109|-1073573623|DTS_E_AXTASKUTIL_ADDVARIABLES_FAILED|Une erreur s'est produite lors de l'ajout des variables SSIS à l'espace de noms de l'hôte de script. Ceci peut empêcher la tâche d'utiliser les variables SSIS dans le script.|  
|0xC002910A|-1073573622|DTS_E_AXTASKUTIL_SCRIPT_PARSING_FAILED|Une erreur irrécupérable s'est produite en essayant d'analyser le texte du script. Vérifiez que le moteur de script du langage choisi est correctement installé.|  
|0xC002910B|-1073573621|DTS_E_AXTASKUTIL_MSG_BAD_FUNCTION|Le nom de fonction entré n'est pas valide Vérifiez qu'un nom de fonction valide a été spécifié.|  
|0xC002910C|-1073573620|DTS_E_AXTASKUTIL_EXECUTION_FAILED|Une erreur s'est produite lors de l'exécution du script. Vérifiez que le moteur de script est correctement installé pour la langue sélectionnée.|  
|0xC002910D|-1073573619|DTS_E_AXTASKUTIL_ADDTYPELIB_FAILED|Une erreur s'est produite lors de l'ajout de la bibliothèque de types managée à l'hôte de script. Vérifiez que le runtime DTS 2000 est installé.|  
|0xC002910E|-1073573618|DTS_E_BITASK_INITIALIZATION_WITH_WRONG_XML_ELEMENT|La tâche d'insertion en bloc a été démarrée avec un élément XML incorrect.|  
|0xC002910F|-1073573617|DTS_E_BITASK_DATA_FILE_NOT_SPECIFIED|Le nom du fichier de données n'est pas spécifié.|  
|0xC0029110|-1073573616|DTS_E_BITASK_HANDLER_NOT_FOUND|Gestionnaire introuvable.|  
|0xC0029111|-1073573615|DTS_E_BITASK_CANNOT_ACQUIRE_CONNECTION|Échec d'acquisition de la connexion spécifiée : « %1 ».|  
|0xC0029112|-1073573614|DTS_E_BITASK_NO_CONNECTION_MANAGER_SPECIFIED|Échec de la tentative d'obtention du gestionnaire de connexions.|  
|0xC0029113|-1073573613|DTS_E_BITASK_INVALID_CONNECTION|La connexion n'est pas valide.|  
|0xC0029114|-1073573612|DTS_E_BITASK_NULL_CONNECTION|La connexion a la valeur NULL.|  
|0xC0029115|-1073573611|DTS_E_BITASK_EXECUTE_FAILED|L'exécution a échoué.|  
|0xC0029116|-1073573610|DTS_E_BITASK_CANNOT_RETRIEVE_TABLES|Une erreur s'est produite lors de l'extraction des tables de la base de données.|  
|0xC0029117|-1073573609|DTS_E_BITASK_CANNOT_RETRIEVE_COLUMN_INFO|Une erreur s'est produite lors de l'extraction des colonnes de la table.|  
|0xC0029118|-1073573608|DTS_E_BITASK_ERROR_IN_DB_OPERATION|Une erreur s'est produite dans l'opération de base de données.|  
|0xC0029119|-1073573607|DTS_E_BITASK_INVALIDSOURCECONNECTIONNAME|La connexion spécifiée « %1 » n'est pas valide ou pointe vers un objet non valide. Indiquez une connexion valide pour continuer.|  
|0xC002911A|-1073573606|DTS_E_BITASK_INVALIDDESTCONNECTIONNAME|La connexion de destination spécifiée n'est pas valide. Indiquez une connexion valide pour continuer.|  
|0xC002911B|-1073573605|DTS_E_BITASK_DESTINATION_TABLE_NOT_SPECIFIED|Vous devez spécifier un nom de table pour continuer.|  
|0xC002911C|-1073573604|DTS_E_BITASK_ERROR_IN_LOAD_FROM_XML|Une erreur s'est produite dans LoadFromXML au niveau de la balise « %1 ».|  
|0xC002911D|-1073573603|DTS_E_BITASK_ERROR_IN_SAVE_TO_XML|Une erreur s'est produite dans SaveToXML au niveau de la balise « %1 ».|  
|0xC002911E|-1073573602|DTS_E_BITASKUNMANCONNECTION_INVALID_CONNECTION|La connexion n'est pas valide.|  
|0xC002911F|-1073573601|DTS_E_BITASKUNMANCONNECTION_EXECUTE_FAILED|L'exécution a échoué.|  
|0xC0029120|-1073573600|DTS_E_BITASKUNMANCONNECTION_CANNOT_RETRIEVE_TABLES|Une erreur s'est produite en extrayant les tables de la base de données.|  
|0xC0029121|-1073573599|DTS_E_BITASKUNMANCONNECTION_CANNOT_RETRIEVE_COLUMN_INFO|Une erreur s'est produite en extrayant les colonnes de la table.|  
|0xC0029122|-1073573598|DTS_E_BITASKUNMANCONNECTION_CANNOT_OPEN_FILE|Une erreur s'est produite en essayant d'ouvrir le fichier de données.|  
|0xC0029123|-1073573597|DTS_E_BITASKUNMANCONNECTION_OEM_CONVERSION_FAILED|Impossible de convertir le fichier d'entrée OEM dans le format spécifié.|  
|0xC0029124|-1073573596|DTS_E_BITASKUNMANCONNECTION_ERROR_IN_DB_OPERATION|Erreur dans l'opération de base de données.|  
|0xC0029125|-1073573595|DTS_E_DTSPROCTASK_NOCONNECTIONSPECIFIED|Aucun gestionnaire de connexions n'est spécifié.|  
|0xC0029126|-1073573594|DTS_E_DTSPROCTASK_CONNECTIONMANAGERNOTOLAP|La connexion « %1 » n'est pas une connexion Analysis Services.|  
|0xC0029127|-1073573593|DTS_E_DTSPROCTASK_UNABLETOLOCATECONNECTIONMANAGER|Impossible de trouver la connexion « %1 ».|  
|0xC0029128|-1073573592|DTS_E_DTSPROCTASK_INVALIDTASKDATANODEEXE|La tâche Analysis Services d'exécution de DDL a reçu un nœud de données de tâche non valide.|  
|0xC0029129|-1073573591|DTS_E_DTSPROCTASK_INVALIDTASKDATANODEPROC|La tâche de traitement Analysis Services a reçu un nœud de données de tâche non valide.|  
|0xC002912A|-1073573590|DTS_E_DTSPROCTASK_INVALIDDDL|Le DDL n'est pas valide.|  
|0xC002912B|-1073573589|DTS_E_DTSPROCTASK_INVALIDDDLPROCESSINGCOMMANDS|Le DDL trouvé dans ProcessingCommands n'est pas valide.|  
|0xC002912C|-1073573588|DTS_E_DTSPROCTASK_CANNOTWRITEINAREADONLYVARIABLE|Le résultat d'exécution ne peut pas être enregistré dans une variable en lecture seule.|  
|0xC002912D|-1073573587|DTS_E_DTSPROCTASK_INVALIDVARIABLE|La variable « %1 » n'est pas définie.|  
|0xC002912E|-1073573586|DTS_E_DTSPROCTASK_CONNECTIONNOTFOUND|Le gestionnaire de connexions « %1 » n'est pas défini.|  
|0xC002912F|-1073573585|DTS_E_DTSPROCTASK_INVALIDCONNECTION|Le gestionnaire de connexions « %1 » n'est pas un gestionnaire de connexions FILE.|  
|0xC0029130|-1073573584|DTS_E_DTSPROCTASK_NONEXISTENTATTRIBUTE|« %1 » n'a pas été trouvé au cours de la désérialisation.|  
|0xC0029131|-1073573583|DTS_E_DTSPROCTASK_TRACEHASBEENSTOPPED|La trace a été arrêtée en raison d'une exception.|  
|0xC0029132|-1073573582|DTS_E_DTSPROCTASK_DDLEXECUTIONFAILED|L'exécution du DDL a échoué.|  
|0xC0029133|-1073573581|DTS_E_DTSPROCTASK_FILEDOESNOTEXIST|Aucun fichier n'est associé à la connexion « %1 ».|  
|0xC0029134|-1073573580|DTS_E_DTSPROCTASK_VARIABLENOTDEFINED|La variable « %1 » n'est pas définie.|  
|0xC0029135|-1073573579|DTS_E_DTSPROCTASK_FILECONNECTIONNOTDEFINED|La connexion de fichier « %1 » n'est pas définie.|  
|0xC0029136|-1073573578|DTS_E_EXEC2000PKGTASK_INITIALIZATION_WITH_WRONG_XML_ELEMENT|La tâche du package Execute DTS 2000 est démarrée avec un élément XML incorrect.|  
|0xC0029137|-1073573577|DTS_E_EXEC2000PKGTASK_HANDLER_NOT_FOUND|Gestionnaire introuvable.|  
|0xC0029138|-1073573576|DTS_E_EXEC2000PKGTASK_PACKAGE_NAME_NOT_SPECIFIED|Le nom de package n'est pas spécifié.|  
|0xC0029139|-1073573575|DTS_E_EXEC2000PKGTASK_PACKAGE_ID_NOT_SPECIFIED|L'ID de package n'est pas spécifié.|  
|0xC002913A|-1073573574|DTS_E_EXEC2000PKGTASK_PACKAGE_VERSIONGUID_NOT_SPECIFIED|Le GUID de la version de package n'est pas spécifié.|  
|0xC002913B|-1073573573|DTS_E_EXEC2000PKGTASK_SQLSERVER_NOT_SPECIFIED|Le serveur SQL n'est pas spécifié.|  
|0xC002913C|-1073573572|DTS_E_EXEC2000PKGTASK_SQL_USERNAME_NOT_SPECIFIED|Le nom d'utilisateur du serveur SQL n'est pas spécifié.|  
|0xC002913D|-1073573571|DTS_E_EXEC2000PKGTASK_FILE_NAME_NOT_SPECIFIED|Le nom du fichier de stockage n'est pas spécifié.|  
|0xC002913E|-1073573570|DTS_E_EXEC2000PKGTASK_DTS2000CANTBEEMPTY|La propriété du package DTS 2000 est vide.|  
|0xC002913F|-1073573569|DTS_E_EXEC2000PKGTASK_ERROR_IN_PACKAGE_EXECUTE|Une erreur s'est produite lors de l'exécution du package DTS 2000.|  
|0xC0029140|-1073573568|DTS_E_EXEC2000PKGTASK_SQLSERVER_NOT_AVAILABLE_NETWORK|Impossible de charger les serveurs SQL disponibles du réseau. Vérifiez la connexion réseau.|  
|0xC0029141|-1073573567|DTS_E_EXEC2000PKGTASK_DATATYPE_NULL|Le type de données ne peut être NULL. Veuillez spécifier le type de données correct à utiliser pour valider la valeur.|  
|0xC0029142|-1073573566|DTS_E_EXEC2000PKGTASK_NULL_VALUE|Impossible de valider une valeur NULL par rapport à un type de données.|  
|0xC0029143|-1073573565|DTS_E_EXEC2000PKGTASK_NULL_VALUE_ARGUMENT|Un argument obligatoire a la valeur NULL.|  
|0xC0029144|-1073573564|DTS_E_EXEC2000PKGTASK_CLS_NOT_REGISTRED_EXCEPTION|Pour activer la tâche d'exécution de package DTS 2000, démarrez le programme d'installation de SQL Server, puis cliquez dans la page Composants à installer sur le bouton Avancé pour sélectionner les composants hérités.|  
|0xC0029145|-1073573563|DTS_E_EXEC2000PKGTASK_NOT_PRIMITIVE_TYPE|« %1 » n'est pas un type de valeur.|  
|0xC0029146|-1073573562|DTS_E_EXEC2000PKGTASK_CONVERT_FAILED|Impossible de convertir « %1 » en « %2 ».|  
|0xC0029147|-1073573561|DTS_E_EXEC2000PKGTASK_ERROR_IN_VALIDATE|Impossible de valider « %1 » par rapport à « %2 ».|  
|0xC0029148|-1073573560|DTS_E_EXEC2000PKGTASK_ERROR_IN_LOAD_FROM_XML|Une erreur s'est produite dans LoadFromXML au niveau de la balise « %1 ».|  
|0xC0029149|-1073573559|DTS_E_EXEC2000PKGTASK_ERROR_IN_SAVE_TO_XML|Une erreur s'est produite dans SaveToXML au niveau de la balise « %1 ».|  
|0xC002914A|-1073573558|DTS_E_EXECPROCTASK_INVALIDTIMEOUT|La valeur de délai d'expiration indiquée n'est pas valide. Spécifiez le nombre de secondes pendant lesquelles la tâche autorise le processus à s'exécuter. La valeur minimale du délai d'expiration est 0, ce qui indique qu'aucun délai d'expiration n'est utilisé et que le processus s'exécute jusqu'au bout ou jusqu'à ce qu'une erreur se produise. La valeur maximale de délai d'expiration est 2147483 (((2^31) - 1)/1000).|  
|0xC002914B|-1073573557|DTS_E_EXECPROCTASK_CANTREDIRECTIO|Impossible de rediriger les flux si le processus peut continuer de s'exécuter au-delà de la durée de vie de la tâche.|  
|0xC002914C|-1073573556|DTS_E_EXECPROCTASK_PROCESSHASTIMEDOUT|Le délai d'attente du processus a expiré.|  
|0xC002914D|-1073573555|DTS_E_EXECPROCTASK_EXECUTABLENOTSPECIFIED|L'exécutable n'est pas spécifié.|  
|0xC002914E|-1073573554|DTS_E_EXECPROCTASK_STDOUTVARREADONLY|La variable de sortie standard est en lecture seule.|  
|0xC002914F|-1073573553|DTS_E_EXECPROCTASK_STDERRVARREADONLY|La variable d'erreur standard est en lecture seule.|  
|0xC0029150|-1073573552|DTS_E_EXECPROCTASK_RECEIVEDINVALIDTASKDATANODE|La tâche d'exécution du processus a reçu un nœud de données de tâche qui n'est pas valide.|  
|0xC0029151|-1073573551|DTS_E_EXECPROCTASK_PROCESSEXITCODEEXCEEDS|Dans l'exécution de « %2 » «%3 » au niveau de « %1 », le code de sortie du processus était « %4 » alors que « %5 » était attendu.|  
|0xC0029152|-1073573550|DTS_E_EXECPROCTASK_WORKINGDIRDOESNOTEXIST|Le répertoire « %1 » n'existe pas.|  
|0xC0029153|-1073573549|DTS_E_EXECPROCTASK_FILEDOESNOTEXIST|Le fichier/processus « %1 » n'existe pas dans le répertoire « %2 ».|  
|0xC0029154|-1073573548|DTS_E_EXECPROCTASK_FILENOTINPATH|Le fichier/processus « %1 » n'est pas dans le chemin d'accès.|  
|0xC0029156|-1073573546|DTS_E_EXECPROCTASK_WORKINGDIRECTORYDOESNOTEXIST|Le répertoire de travail « %1 » n'existe pas.|  
|0xC0029157|-1073573545|DTS_E_EXECPROCTASK_ERROREXECUTIONVALUE|Le processus s'est terminé avec le code de retour « %1 ». Cependant, « %2 » était attendu.|  
|0xC0029158|-1073573544|DTS_E_FSTASK_SYNCFAILED|Échec de l'objet de synchronisation.|  
|0xC0029159|-1073573543|DTS_E_FSTASK_INVALIDDATA|La tâche du système de fichiers a reçu un nœud de données de tâche non valide.|  
|0xC002915A|-1073573542|DTS_E_FSTASK_DIRECTORYEXISTS|Le répertoire existe déjà.|  
|0xC002915B|-1073573541|DTS_E_FSTASK_PATHNOTVALID|« %1 » n'est pas valide sur le type d'opération « %2 ».|  
|0xC002915C|-1073573540|DTS_E_FSTASK_DESTINATIONNOTSET|La propriété de destination de l'opération « %1 » n'est pas définie.|  
|0xC002915D|-1073573539|DTS_E_FSTASK_SOURCENOTSET|La propriété source de l'opération « %1 » n'est pas définie.|  
|0xC002915E|-1073573538|DTS_E_FSTASK_CONNECTIONTYPENOTFILE|Le type de connexion « %1 » n'est pas un fichier.|  
|0xC002915F|-1073573537|DTS_E_FSTASK_VARIABLEDOESNTEXIST|La variable « %1 » n'existe pas.|  
|0xC0029160|-1073573536|DTS_E_FSTASK_VARIABLENOTASTRING|La variable « %1 » n'est pas une chaîne.|  
|0xC0029163|-1073573533|DTS_E_FSTASK_FILEDOESNOTEXIST|Le fichier ou le répertoire « %1 » représenté par la connexion « %2 » n'existe pas.|  
|0xC0029165|-1073573531|DTS_E_FSTASK_DESTCONNUSAGETYPEINVALID|Le gestionnaire de connexions de fichiers de destination « %1 » a un type d'utilisation non valide : « %2 ».|  
|0xC0029166|-1073573530|DTS_E_FSTASK_SRCCONNUSAGETYPEINVALID|Le gestionnaire de connexions de fichiers sources « %1 » a un type d'utilisation non valide « %2 ».|  
|0xC0029167|-1073573529|DTS_E_FSTASK_LOGENTRYGETTINGFILEOPERATION|FileSystemOperation|  
|0xC0029168|-1073573528|DTS_E_FSTASK_LOGENTRYGETTINGFILEOPERATIONDESC|Fournit des informations sur les opérations du système de fichiers.|  
|0xC0029169|-1073573527|DTS_E_FSTASK_TASKDISPLAYNAME|Tâches du système de fichiers|  
|0xC002916A|-1073573526|DTS_E_FSTASK_TASKDESCRIPTION|Effectuent les opérations du système de fichiers, telles que la copie et la suppression de fichiers.|  
|0xC002916B|-1073573525|DTS_E_FTPTASK_SYNCOBJFAILED|Échec de l'objet de synchronisation.|  
|0xC002916C|-1073573524|DTS_E_FTPTASK_UNABLETOOBTAINFILELIST|Impossible d'obtenir la liste des fichiers.|  
|0xC002916D|-1073573523|DTS_E_FTPTASK_LOCALPATHEMPTY|Le chemin d'accès local est vide.|  
|0xC002916E|-1073573522|DTS_E_FTPTASK_REMOTEPATHEMPTY|Le chemin d'accès distant est vide.|  
|0xC002916F|-1073573521|DTS_E_FTPTASK_LOCALVARIBALEEMPTY|La variable locale est vide.|  
|0xC0029170|-1073573520|DTS_E_FTPTASK_REMOTEVARIBALEEMPTY|La variable distante est vide.|  
|0xC0029171|-1073573519|DTS_E_FTPTASK_FTPRCVDINVLDDATANODE|La tâche FTP a reçu un nœud de données de tâche non valide.|  
|0xC0029172|-1073573518|DTS_E_FTPTASK_CONNECTION_NAME_NULL|La connexion est vide. Vérifiez qu'une connexion FTP valide est fournie.|  
|0xC0029173|-1073573517|DTS_E_FTPTASK_CONNECTION_NOT_FTP|La connexion spécifiée n'est pas une connexion FTP. Vérifiez qu'une connexion FTP valide est fournie.|  
|0xC0029175|-1073573515|DTS_E_FTPTASK__INITIALIZATION_WITH_NULL_XML_ELEMENT|Impossible d'initialiser la tâche avec un élément XML qui a la valeur NULL.|  
|0xC0029176|-1073573514|DTS_E_FTPTASK_SAVE_TO_NULL_XML_ELEMENT|Impossible d'enregistrer la tâche dans un document XML qui a la valeur NULL.|  
|0xC0029177|-1073573513|DTS_E_FTPTASK_ERROR_IN_LOAD_FROM_XML|Une erreur s'est produite dans LoadFromXML au niveau de la balise « %1 ».|  
|0xC0029178|-1073573512|DTS_E_FTPTASK_NOFILESATLOCATION|Il n'y a aucun fichier dans « %1 ».|  
|0xC0029179|-1073573511|DTS_E_FTPTASK_LOCALVARIABLEISEMPTY|La variable « %1 » est vide.|  
|0xC002917A|-1073573510|DTS_E_FTPTASK_REMOTEVARIABLEISEMPTY|La variable « %1 » est vide.|  
|0xC002917B|-1073573509|DTS_E_FTPTASK_NOFILESINCONNMGR|Le fichier « %1 » ne contient pas de chemin d'accès au fichier.|  
|0xC002917C|-1073573508|DTS_E_FTPTASK_NOFILEPATHSINLOCALVAR|La variable « %1 » ne contient pas de chemin d'accès au fichier.|  
|0xC002917D|-1073573507|DTS_E_FTPTASK_VARIABLENOTASTRING|La variable « %1 » n'est pas une chaîne.|  
|0xC002917E|-1073573506|DTS_E_FTPTASK_VARIABLENOTFOUND|La variable « %1 » n'existe pas.|  
|0xC002917F|-1073573505|DTS_E_FTPTASK_INVALIDPATHONOPERATION|Chemin d'accès non valide sur l'opération « %1 ».|  
|0xC0029180|-1073573504|DTS_E_FTPTASK_DIRECTORYEXISTS|« %1 » existe déjà.|  
|0xC0029182|-1073573502|DTS_E_FTPTASK_CONNECTIONTYPENOTFILE|Le type de connexion « %1 » n'est pas un fichier.|  
|0xC0029183|-1073573501|DTS_E_FTPTASK_FILEDOESNOTEXIST|Le fichier représenté par « %1 » n'existe pas.|  
|0xC0029184|-1073573500|DTS_E_FTPTASK_INVALIDDIRECTORY|Le répertoire n'est pas spécifié dans la variable « %1 ».|  
|0xC0029185|-1073573499|DTS_E_FTPTASK_NOFILESFOUND|Aucun fichier trouvé dans « %1 ».|  
|0xC0029186|-1073573498|DTS_E_FTPTASK_NODIRECTORYPATHINCONMGR|Le répertoire n'est pas spécifié dans le gestionnaire de connexions de fichiers « %1 ».|  
|0xC0029187|-1073573497|DTS_E_FTPTASK_UNABLETODELETELOCALEFILE|Impossible de supprimer le fichier local « %1 ».|  
|0xC0029188|-1073573496|DTS_E_FTPTASK_UNABLETOREMOVELOCALDIRECTORY|Impossible de supprimer le répertoire local « %1 ».|  
|0xC0029189|-1073573495|DTS_E_FTPTASK_UNABLETOCREATELOCALDIRECTORY|Impossible de créer le répertoire local « %1 ».|  
|0xC002918A|-1073573494|DTS_E_FTPTASK_UNABLETORECEIVEFILES|Impossible de recevoir des fichiers en utilisant « %1 ».|  
|0xC002918B|-1073573493|DTS_E_FTPTASK_UNABLETOSENDFILES|Impossible d'envoyer des fichiers en utilisant « %1 ».|  
|0xC002918C|-1073573492|DTS_E_FTPTASK_UNABLETOMAKEDIRREMOTE|Impossible de créer un répertoire distant en utilisant « %1 ».|  
|0xC002918D|-1073573491|DTS_E_FTPTASK_UNABLETOREMOVEDIRREMOTE|Impossible de supprimer le répertoire distant en utilisant « %1 ».|  
|0xC002918E|-1073573490|DTS_E_FTPTASK_UNABLETODELETEREMOTEFILES|Impossible de supprimer des fichiers distants en utilisant « %1 ».|  
|0xC002918F|-1073573489|DTS_E_FTPTASK_UNABLETOCONNECTTOSERVER|Impossible de se connecter au serveur FTP en utilisant « %1 ».|  
|0xC0029190|-1073573488|DTS_E_FTPTASK_INVALIDVARIABLEVALUE|La variable « %1 » ne commence pas par « / ».|  
|0xC0029191|-1073573487|DTS_E_FTPTASK_INVALIDREMOTEPATH|Le chemin d'accès distant « %1 » ne commence pas par « / ».|  
|0xC0029192|-1073573486|DTS_E_DTS_E_FTPTASK_CANNOT_ACQUIRE_CONNECTION|Une erreur s'est produite lors de l'acquisition de la connexion FTP. Vérifiez que vous avez spécifié un type de connexion valide « %1 ».|  
|0xC0029193|-1073573485|DTS_E_MSGQTASKUTIL_CERT_OPEN_STORE_FAILED|Échec de l'ouverture du magasin de certificats.|  
|0xC0029194|-1073573484|DTS_E_MSGQTASKUTIL_CERT_FAILED_GETTING_DISPLAY_NAME|Une erreur s'est produite lors de l'extraction du nom complet du certificat.|  
|0xC0029195|-1073573483|DTS_E_MSGQTASKUTIL_CERT_FAILED_GETTING_ISSUER_NAME|Une erreur s'est produite lors de l'extraction du nom de l'émetteur du certificat.|  
|0xC0029196|-1073573482|DTS_E_MSGQTASKUTIL_CERT_FAILED_GETTING_FRIENDLY_NAME|Une erreur s'est produite lors de l'extraction du nom convivial du certificat.|  
|0xC0029197|-1073573481|DTS_E_MSMQTASK_NO_CONNECTION|La connexion MSMQ n'est pas définie.|  
|0xC0029198|-1073573480|DTS_E_MSMQTASK_INITIALIZATION_WITH_WRONG_XML_ELEMENT|La tâche a été initialisée avec l'élément XML incorrect.|  
|0xC0029199|-1073573479|DTS_E_MSMQTASK_DATA_FILE_NAME_EMPTY|Le nom du fichier de données est vide.|  
|0xC002919A|-1073573478|DTS_E_MSMQTASK_DATA_FILE_SAVE_NAME_EMPTY|Le nom spécifié pour le fichier de données à enregistrer est vide.|  
|0xC002919B|-1073573477|DTS_E_MSMQTASK_DATA_FILE_SIZE_ERROR|La taille du fichier doit être inférieure à 4 Mo.|  
|0xC002919C|-1073573476|DTS_E_MSMQTASK_DATA_FILE_SAVE_FAILED|Échec de l'enregistrement du fichier de données.|  
|0xC002919D|-1073573475|DTS_E_MSMQTASK_STRING_COMPARE_VALUE_MISSING|La valeur du filtre de chaîne est vide.|  
|0xC002919E|-1073573474|DTS_E_MSMQTASK_INVALID_QUEUE_PATH|Le chemin d'accès de la file d'attente n'est pas valide.|  
|0xC002919F|-1073573473|DTS_E_MSMQTASK_NOT_TRANSACTIONAL|La tâche MSMQ ne prend pas en charge l'inscription dans des transactions distribuées.|  
|0xC00291A0|-1073573472|DTS_E_MSMQTASK_INVALID_MESSAGE_TYPE|Le type de message n'est pas valide.|  
|0xC00291A1|-1073573471|DTS_E_MSMQTASK_TASK_TIMEOUT|Le délai d'attente de la file d'attente des messages a expiré. Aucun message n'a été reçu.|  
|0xC00291A2|-1073573470|DTS_E_MSMQTASK_INVALID_PROPERTY_VALUE|La propriété spécifiée n'est pas valide. Vérifiez que le type d'argument est correct.|  
|0xC00291A3|-1073573469|DTS_E_MSMQTASK_MESSAGE_NON_AUTHENTICATED|Le message n'est pas authentifié.|  
|0xC00291A4|-1073573468|DTS_E_MSMQTASK_INVALID_ENCRYPTION_ALGO_WRAPPER|Vous essayez de définir la valeur de l'algorithme de chiffrement avec un objet non valide.|  
|0xC00291A5|-1073573467|DTS_E_MSMQTASK_VARIABLE_TO_RECEIVE_STRING_MSG_EMPTY|La variable pour recevoir le message de chaîne est vide.|  
|0xC00291A6|-1073573466|DTS_E_MSMQTASK_RECEIVE_VARIABLE_EMPTY|La variable pour recevoir le message de variable est vide.|  
|0xC00291A7|-1073573465|DTS_E_MSMQTASK_CONNECTIONTYPENOTMSMQ|La connexion « %1 » n'est pas de type MSMQ.|  
|0xC00291A8|-1073573464|DTS_E_MSMQTASK_DATAFILE_ALREADY_EXISTS|Le fichier de données « %1 » existe déjà à l'emplacement spécifié. Impossible de remplacer le fichier puisque l'option Remplacer a la valeur FALSE.|  
|0xC00291A9|-1073573463|DTS_E_MSMQTASK_STRING_MSG_TO_VARIABLE_NOT_FOUND|La variable spécifiée « %1 » qui recevra le message de chaîne est introuvable dans la collection de variables du package.|  
|0xC00291AA|-1073573462|DTS_E_MSMQTASK_CONNMNGRNULL|Le gestionnaire de connexions « %1 » est vide.|  
|0xC00291AB|-1073573461|DTS_E_MSMQTASK_CONNMNGRDOESNOTEXIST|Le gestionnaire de connexions « %1 » n'existe pas.|  
|0xC00291AC|-1073573460|DTS_E_SCRIPTTASK_COMPILEERRORMSG|Erreur « %1 » : « %2 »\r\nLigne « %3 » Colonne « %4 » à « %5 ».|  
|0xC00291AD|-1073573459|DTS_E_SCRIPTTASK_COMPILEERRORMSG2|Une erreur s'est produite lors de la compilation du script : « %1 ».|  
|0xC00291AE|-1073573458|DTS_E_SCRIPTTASK_COMPILEERRORMSG3|Erreur « %1 » : « %2 »\r\nLigne « %3 » Colonnes « %4 » - « %5 »\r\nTexte sur plusieurs lignes : « %6 ».|  
|0xC00291AF|-1073573457|DTS_E_SCRIPTTASK_SCRIPTREPORTEDFAILURE|Le script utilisateur a retourné un résultat d'erreur.|  
|0xC00291B0|-1073573456|DTS_E_SCRIPTTASK_SCRIPTFILESFAILEDTOLOAD|Échec du chargement des fichiers du script utilisateur.|  
|0xC00291B1|-1073573455|DTS_E_SCRIPTTASK_SCRIPTTHREWEXCEPTION|Le script utilisateur a renvoyé une exception : « %1 ».|  
|0xC00291B2|-1073573454|DTS_E_SCRIPTTASK_COULDNOTCREATEENTRYPOINTCLASS|Impossible de créer une instance de la classe entrypoint « %1 ».|  
|0xC00291B3|-1073573453|DTS_E_SCRIPTTASK_LOADFROMXMLEXCEPTION|Une exception s'est produite lors du chargement de la tâche de script de XML : « %1 ».|  
|0xC00291B4|-1073573452|DTS_E_SCRIPTTASK_SOURCEITEMNOTFOUNDEXCEPTION|L'élément source « %1 » est introuvable dans le package.|  
|0xC00291B5|-1073573451|DTS_E_SCRIPTTASK_BINARYITEMNOTFOUNDEXCEPTION|L'élément binaire « %1 » est introuvable dans le package.|  
|0xC00291B6|-1073573450|DTS_E_SCRIPTTASK_UNRECOGNIZEDSCRIPTLANGUAGEEXCEPTION|« %1 » n'a pas été reconnu comme langage de script valide.|  
|0xC00291B7|-1073573449|DTS_E_SCRIPTTASK_ILLEGALSCRIPTNAME|Le nom de script n'est pas valide. Il ne peut pas contenir des espaces, des barres obliques, des caractères spéciaux ni commencer par un numéro.|  
|0xC00291B8|-1073573448|DTS_E_SCRIPTTASK_INVALIDSCRIPTLANGUAGE|Le langage de script spécifié n'est pas valide.|  
|0xC00291B9|-1073573447|DTS_E_SCRIPTTASK_CANTINITNULLTASK|Impossible d'initialiser une tâche NULL.|  
|0xC00291BA|-1073573446|DTS_E_SCRIPTTASK_MUSTINITWITHRIGHTTASK|L'interface utilisateur de la tâche de script doit s'initialiser avec une tâche de script.|  
|0xC00291BB|-1073573445|DTS_E_SCRIPTTASK_WASNOTINITED|L'interface utilisateur de la tâche de script n'est pas initialisée.|  
|0xC00291BC|-1073573444|DTS_E_SCRIPTTASK_HOST_NAME_CANT_EMPTY|Le nom n'est pas vide.|  
|0xC00291BD|-1073573443|DTS_E_SCRIPTTASK_INVALID_SCRIPT_NAME|Le nom de projet n'est pas valide. Il ne peut pas contenir des espaces, des barres obliques, des caractères spéciaux ni commencer par un numéro.|  
|0xC00291BE|-1073573442|DTS_E_SCRIPTTASK_INVALID_SCRIPT_LANGUAGE|Le langage de script spécifié n'est pas valide.|  
|0xC00291BF|-1073573441|DTS_E_SCRIPTTASK_INVALID_ENTRY_POINT|Le point d'entrée est introuvable.|  
|0xC00291C0|-1073573440|DTS_E_SCRIPTTASK_LANGUAGE_EMPTY|Le langage de script n'est pas spécifié. Vérifiez qu'un langage de script valide est spécifié.|  
|0xC00291C1|-1073573439|DTS_E_SCRIPTTASK_INITIALIZATION_WITH_NULL_TASK|Initialisation de l'interface utilisateur : la tâche a la valeur NULL.|  
|0xC00291C2|-1073573438|DTS_E_SCRIPTTASK_UI_INITIALIZATION_WITH_WRONG_TASK|L'interface utilisateur de la tâche de script est initialisée avec une tâche incorrecte.|  
|0xC00291C3|-1073573437|DTS_E_SENDMAILTASK_RECIPIENT_EMPTY|Aucun destinataire n'est spécifié.|  
|0xC00291C4|-1073573436|DTS_E_SENDMAILTASK_SMTP_SERVER_NOT_SPECIFIED|Le serveur SMTP (Simple Mail Transfer Protocol) n'est pas spécifié. Indiquez un nom ou une adresse IP valide pour le serveur SMTP.|  
|0xC00291C5|-1073573435|DTS_E_SENDMAILTASK_TASK_INITIALIZATION_WITH_WRONG_XML_ELEMENT|La tâche Envoyer un message a été lancée avec un élément XML incorrect.|  
|0xC00291CB|-1073573429|DTS_E_SENDMAILTASK_INVALIDATTACHMENT|Le fichier « %1 » n'existe pas ou vous ne disposez pas des autorisations appropriées pour accéder au fichier.|  
|0xC00291CD|-1073573427|DTS_E_SENDMAILTASK_CHECK_VALID_SMTP_SERVER|Vérifiez que le serveur SMTP (Simple Mail Transfer Protocol) spécifié est valide.|  
|0xC00291CE|-1073573426|DTS_E_SENDMAILTASK_CONNECTIONTYPENOTFILE|La connexion « %1 » n'est pas du type File.|  
|0xC00291CF|-1073573425|DTS_E_SENDMAILTASK_FILEDOESNOTEXIST|Sur l'opération « %1 », le fichier « %2 » n'existe pas.|  
|0xC00291D0|-1073573424|DTS_E_SENDMAILTASK_VARIABLETYPEISNOTSTRING|La variable « %1 » n'est pas de type String.|  
|0xC00291D1|-1073573423|DTS_E_SENDMAILTASK_CONNECTIONTYPENOTSMTP|La connexion « %1 » n'est pas de type SMTP.|  
|0xC00291D2|-1073573422|DTS_E_SENDMAILTASK_CONNMNGRNULL|La connexion « %1 » est vide.|  
|0xC00291D3|-1073573421|DTS_E_SENDMAILTASK_NOCONNMNGR|La connexion spécifiée « %1 » n'existe pas.|  
|0xC00291D4|-1073573420|DTS_E_SQLTASK_NOSTATEMENTSPECIFIED|Aucune instruction Transact-SQL n'est spécifiée.|  
|0xC00291D5|-1073573419|DTS_E_SQLTASK_NOXMLSUPPORT|La connexion ne prend pas en charge les ensembles de résultats XML.|  
|0xC00291D6|-1073573418|DTS_E_SQLTASK_NOHANDLERFORCONNECTION|Impossible de trouver le gestionnaire pour le type de connexion spécifié.|  
|0xC00291D7|-1073573417|DTS_E_SQLTASK_NOCONNECTIONMANAGER|Aucun gestionnaire de connexions n'est spécifié.|  
|0xC00291D8|-1073573416|DTS_E_SQLTASK_CANNOTACQUIRECONNMANAGER|Impossible d'obtenir une connexion à partir du gestionnaire de connexions.|  
|0xC00291D9|-1073573415|DTS_E_SQLTASK_NULLPARAMETERNAME|Impossible d'avoir un nom de paramètre NULL.|  
|0xC00291DA|-1073573414|DTS_E_SQLTASK_INVALIDPARAMETERNAME|Le nom du paramètre n'est pas valide.|  
|0xC00291DB|-1073573413|DTS_E_SQLTASK_VALIDPARAMETERTYPES|Les noms de paramètre sont du type Int ou String.|  
|0xC00291DC|-1073573412|DTS_E_SQLTASK_READONLYVARIABLE|La variable « %1 » est inutilisable dans une liaison de résultat parce qu’elle est en lecture seule.|  
|0xC00291DD|-1073573411|DTS_E_SQLTASK_INDESNOTINCOLLECTION|L'index n'est pas attribué dans cette collection.|  
|0xC00291DE|-1073573410|DTS_E_SQLTASK_ROVARINOUTPARAMETER|La variable « %1 » ne peut pas être utilisée comme paramètre de « sortie » ou valeur de retour dans une liaison de paramètre car elle est en lecture seule.|  
|0xC00291DF|-1073573409|DTS_E_SQLTASK_OBJECTNOTINCOLLECTION|L'objet n'existe pas dans cette collection.|  
|0xC00291E0|-1073573408|DTS_E_SQLTASK_UNABLETOACQUIREMANAGEDCONN|Impossible d'acquérir une connexion managée.|  
|0xC00291E1|-1073573407|DTS_E_UNABLETOPOPRESULT|Impossible de remplir les colonnes de résultat pour un seul type de résultat de ligne. La requête a retourné un ensemble de résultats vide.|  
|0xC00291E2|-1073573406|DTS_E_SQLTASK_INVALIDNUMOFRESULTBINDINGS|Un nombre non valide de liaisons de résultats a été retourné pour le ResultSetType : « %1 ».|  
|0xC00291E3|-1073573405|DTS_E_SQLTASK_RESULTBINDTYPEFORROWSETXML|Le nom de liaison des résultats doit avoir la valeur zéro pour le jeu de résultats complet et les résultats XML.|  
|0xC00291E4|-1073573404|DTS_E_SQLTASK_INVALIDEPARAMDIRECTIONFALG|L'indicateur de direction du paramètre n'est pas valide.|  
|0xC00291E5|-1073573403|DTS_E_SQLTASK_NOSQLTASKDATAINXMLFRAGMENT|Le fragment XML ne contient aucune donnée de tâche SQL.|  
|0xC00291E6|-1073573402|DTS_E_SQLTASK_MULTIPLERETURNVALUEPARAM|Le premier paramètre n'est pas un paramètre avec une valeur de retour de type ou il existe plusieurs paramètres de valeur de retour de type.|  
|0xC00291E7|-1073573401|DTS_E_SQLTASK_CONNECTIONTYPENOTFILE|La connexion « %1 » n'est pas un gestionnaire de connexions de fichiers.|  
|0xC00291E8|-1073573400|DTS_E_SQLTASK_FILEDOESNOTEXIST|Le fichier représenté par « %1 » n'existe pas.|  
|0xC00291E9|-1073573399|DTS_E_SQLTASK_VARIABLETYPEISNOTSTRING|La variable « %1 » n'est pas de type String.|  
|0xC00291EA|-1073573398|DTS_E_SQLTASK_VARIABLENOTFOUND|La variable « %1 » n'existe pas ou n'a pas pu être verrouillée.|  
|0xC00291EB|-1073573397|DTS_E_SQLTASK_CANNOTLOCATECONNMANAGER|Le gestionnaire de connexions « %1 » n'existe pas.|  
|0xC00291EC|-1073573396|DTS_E_SQLTASK_FAILEDTOACQUIRECONNECTION|Échec de l'acquisition de la connexion « %1 ». La connexion n'est peut-être pas configurée correctement ou vous ne disposez pas des autorisations appropriées pour cette connexion.|  
|0xC00291ED|-1073573395|DTS_E_SQLTASK_RESULTBYNAMENOTSUPPORTED|La liaison de résultats par nom « %1 » n'est pas prise en charge pour ce type de connexion.|  
|0xC00291EE|-1073573394|DTS_E_SQLTASKCONN_ERR_NO_ROWS|Un type de jeu de résultats de lignes uniques est spécifié, mais aucune ligne n'est retournée.|  
|0xC00291EF|-1073573393|DTS_E_SQLTASKCONN_ERR_NO_DISCONNECTED_RS|Aucun jeu d'enregistrements déconnectés n'est disponible pour l'instruction Transact-SQL.|  
|0xC00291F0|-1073573392|DTS_E_SQLTASKCONN_ERR_UNSUPPORTED_TYPE|Type non pris en charge.|  
|0xC00291F1|-1073573391|DTS_E_SQLTASKCONN_ERR_UNKNOWN_TYPE|Type inconnu.|  
|0xC00291F2|-1073573390|DTS_E_SQLTASKCONN_ERR_PARAM_DATA_TYPE|Type de données non pris en charge dans la liaison de paramètre \\"%s\\".|  
|0xC00291F3|-1073573389|DTS_E_SQLTASKCONN_ERR_PARAM_NAME_MIX|Les noms de paramètres ne peuvent pas être une combinaison de numéros d'ordre et de noms.|  
|0xC00291F4|-1073573388|DTS_E_SQLTASKCONN_ERR_PARAM_DIR|La direction du paramètre dans la liaison de paramètre \\"%s\\" n’est pas valide.|  
|0xC00291F5|-1073573387|DTS_E_SQLTASKCONN_ERR_RESULT_DATA_TYPE|Le type de données dans la liaison de jeu de résultats \\"%s\\" n’est pas pris en charge.|  
|0xC00291F6|-1073573386|DTS_E_SQLTASKCONN_ERR_RESULT_COL_INDEX|L'index de la colonne de résultats %d n'est pas valide.|  
|0xC00291F7|-1073573385|DTS_E_SQLTASKCONN_ERR_UNKNOWN_RESULT_COL|La colonne \\"%s\\" est introuvable dans le jeu de résultats.|  
|0xC00291F9|-1073573383|DTS_E_SQLTASKCONN_ERR_NOROWSET|Aucun ensemble de lignes de résultat n'est associé à l'exécution de cette requête.|  
|0xC00291FA|-1073573382|DTS_E_SQLTASKCONN_ERR_ODBC_DISCONNECTED|Les jeux d'enregistrements déconnectés ne sont pas disponibles à partir des connexions ODBC.|  
|0xC00291FB|-1073573381|DTS_E_SQLTASKCONN_ERR_RESULT_SET_DATA_TYPE|Le type de données du jeu de résultats, colonne %hd, n'est pas pris en charge.|  
|0xC00291FC|-1073573380|DTS_E_SQLTASKCONN_ERR_CANT_LOAD_XML|Impossible de charger le fichier XML avec le résultat de la requête.|  
|0xC00291FD|-1073573379|DTS_E_TTGENTASK_NOCONNORVARIABLE|Un nom de connexion ou de variable doit être indiqué pour le package.|  
|0xC00291FE|-1073573378|DTS_E_TTGENTASK_FAILEDCREATE|Impossible de créer le package.|  
|0xC00291FF|-1073573377|DTS_E_TTGENTASK_BADTABLEMETADATA|TableMetaDataNode n'est pas de type XMLNode.|  
|0xC0029200|-1073573376|DTS_E_TTGENTASK_FAILEDCREATEPIPELINE|Impossible de créer le pipeline.|  
|0xC0029201|-1073573375|DTS_E_TTGENTASK_BADVARIABLETYPE|Le type de variable est incorrect.|  
|0xC0029202|-1073573374|DTS_E_TTGENTASK_NOTFILECONNECTION|Le gestionnaire de connexions spécifié n'est pas un gestionnaire de connexions de fichiers.|  
|0xC0029203|-1073573373|DTS_E_TTGENTASK_BADFILENAME|Nom de fichier incorrect spécifié dans le gestionnaire de connexions « %1 ».|  
|0xC0029204|-1073573372|DTS_E_WEBSERVICETASK_CONNECTION_NAME_NULL|La connexion est vide. Vérifiez qu'une connexion HTTP valide est spécifiée.|  
|0xC0029205|-1073573371|DTS_E_WEBSERVICETASK_CONNECTION_NOT_FOUND|La connexion n'existe pas. Vérifiez qu'une connexion HTTP existante valide est spécifiée.|  
|0xC0029206|-1073573370|DTS_E_WEBSERVICETASK_CONNECTION_NOT_HTTP|La connexion spécifiée n'est pas une connexion HTTP. Vérifiez qu'une connexion HTTP valide est spécifiée.|  
|0xC0029207|-1073573369|DTS_E_WEBSERVICETASK_SERVICE_NULL|Le nom du service Web est vide. Vérifiez qu'un service Web valide est spécifié.|  
|0xC0029208|-1073573368|DTS_E_WEBSERVICETASK_METHODNAME_NULL|Le nom de la méthode Web est vide. Vérifiez qu'une méthode Web valide est spécifiée.|  
|0xC0029209|-1073573367|DTS_E_WEBSERVICETASK_WEBMETHODINFO_NULL|La méthode Web est vide ou n'existe pas. Vérifiez qu'une méthode Web existe.|  
|0xC002920A|-1073573366|DTS_E_WEBSERVICETASK_OUTPUTLOC_NULL|L'emplacement de destination est vide. Vérifiez qu'une connexion ou une variable de fichier existante est spécifiée.|  
|0xC002920B|-1073573365|DTS_E_WEBSERVICETASK_VARIABLE_NOT_FOUND|La variable est introuvable. Vérifiez que la variable existe dans le package.|  
|0xC002920C|-1073573364|DTS_E_WEBSERVICETASK_VARIABLE_READONLY|Impossible d'enregistrer le résultat. Vérifiez que la variable n'est pas en lecture seule.|  
|0xC002920D|-1073573363|DTS_E_WEBSERVICETASK_ERROR_IN_LOAD_FROM_XML|Une erreur s'est produite dans LoadFromXML au niveau de la balise « %1 ».|  
|0xC002920E|-1073573362|DTS_E_WEBSERVICETASK_ERROR_IN_SAVE_TO_XML|Une erreur s'est produite dans SaveToXML au niveau de la balise « %1 ».|  
|0xC002920F|-1073573361|DTS_E_WEBSERVICETASK_TASK_SAVE_TO_NULL_XML_ELEMENT|Impossible d'enregistrer la tâche dans un document XML qui a la valeur NULL.|  
|0xC0029210|-1073573360|DTS_E_WEBSERVICETASK_TASK_INITIALIZATION_WITH_NULL_XML_ELEMENT|Impossible d'initialiser la tâche avec un élément XML qui a la valeur NULL.|  
|0xC0029211|-1073573359|DTS_E_WEBSERVICETASK_TASK_INITIALIZATION_WITH_WRONG_XML_ELEMENT|La tâche de service Web est initiée avec un élément XML incorrect.|  
|0xC0029212|-1073573358|DTS_E_WEBSERVICETASK_UNEXPECTED_XML_ELEMENT|Un élément XML inattendu a été trouvé.|  
|0xC0029213|-1073573357|DTS_E_WEBSERVICETASK_CANNOT_ACQUIRE_CONNECTION|Une erreur s'est produite lors de l'acquisition de la connexion HTTP. Vérifiez qu'un type de connexion valide est spécifié.|  
|0xC0029214|-1073573356|DTS_E_WEBSERVICETASK_FILE_CONN_NOT_FOUND|Impossible d'enregistrer le résultat. Vérifiez qu'une connexion à un fichier existe.|  
|0xC0029215|-1073573355|DTS_E_WEBSERVICETASK_FILE_NOT_FOUND|Impossible d'enregistrer le résultat. Vérifiez que le fichier existe.|  
|0xC0029216|-1073573354|DTS_E_WEBSERVICETASK_FILE_NULL|Impossible d'enregistrer le résultat. Le nom de fichier est vide, ou le fichier est utilisé par un autre processus.|  
|0xC0029217|-1073573353|DTS_E_WEBSERVICETASK_CANNOT_ACQUIRE_FILE_CONNECTION|Une erreur s'est produite lors de l'acquisition de la connexion au fichier. Vérifiez qu'une connexion à un fichier valide est spécifiée.|  
|0xC0029218|-1073573352|DTS_E_WEBSERVICETASK_DATATYPE_NOT_SUPPORTED|Seuls les types complexes avec des valeurs de primitives, des tableaux de primitives et des énumérations sont pris en charge.|  
|0xC0029219|-1073573351|DTS_E_WEBSERVICETASK_PARAMTYPE_NOT_SUPPORTED|Seuls les types Primitive, Enum, Complex, PrimitiveArray et ComplexArray sont pris en charge.|  
|0xC002921A|-1073573350|DTS_E_WEBSERVICETASK_WSDL_VERSION_NOT_SUPPORTED|Cette version de WSDL n'est pas prise en charge.|  
|0xC002921B|-1073573349|DTS_E_WEBSERVICETASK_WRONG_XML_ELEMENT|Initialisation avec un élément XML incorrect.|  
|0xC002921C|-1073573348|DTS_E_WEBSERVICETASK_XML_ATTRIBUTE_NOT_FOUND|Un attribut obligatoire est introuvable.|  
|0xC002921D|-1073573347|DTS_E_WEBSERVICETASK_ENUM_NO_VALUES|L'énumération « %1 » ne comporte aucune valeur. WSDL est corrompu.|  
|0xC002921E|-1073573346|DTS_E_WEBSERVICETASK_CONNECTIONNOTFOUND|La connexion est introuvable.|  
|0xC002921F|-1073573345|DTS_E_WEBSERVICETASK_CONNECTION_ALREADY_EXISTS|Une connexion de ce nom existe déjà.|  
|0xC0029220|-1073573344|DTS_E_WEBSERVICETASK_NULL_CONNECTION|Une connexion ne peut pas être NULL ou vide.|  
|0xC0029221|-1073573343|DTS_E_WEBSERVICETASK_NOT_HTTP_CONNECTION|La connexion spécifiée n'est pas une connexion HTTP. Vérifiez qu'une connexion HTTP valide est spécifiée.|  
|0xC0029222|-1073573342|DTS_E_WEBSERVICETASK_WSDL_NOT_FOUND|L'URI (Uniform Resource Identifier) ne contient aucun fichier WSDL valide.|  
|0xC0029223|-1073573341|DTS_E_WEBSERVICETASK_ERROR_IN_DOWNLOAD|Impossible de lire le fichier WSDL. Le fichier WSDL d'entrée n'est pas valide. Le lecteur a retourné l'erreur suivante : « %1 ».|  
|0xC0029224|-1073573340|DTS_E_WEBSERVICETASK_SERVICE_DESC_NULL|La description du service ne peut pas être NULL.|  
|0xC0029225|-1073573339|DTS_E_WEBSERVICETASK_SERVICENULL|Le nom de service ne peut pas être NULL.|  
|0xC0029226|-1073573338|DTS_E_WEBSERVICETASK_WSDL_NULL|L'URL ne peut pas être NULL.|  
|0xC0029227|-1073573337|DTS_E_WEBSERVICETASK_SERVICE_NOT_FOUND|Le service n'est pas disponible actuellement.|  
|0xC0029228|-1073573336|DTS_E_WEBSERVICETASK_SOAPPORT_NOT_FOUND|Le service n'est pas disponible sur le port SOAP.|  
|0xC0029229|-1073573335|DTS_E_WEBSERVICETASK_SOAPBINDING_NOT_FOUND|Impossible d'analyser WSDL (Web Services Description Language). Impossible de trouver la liaison qui correspond au port SOAP.|  
|0xC002922A|-1073573334|DTS_E_WEBSERVICETASK_SOAPPORTTYPE_NOT_FOUND|Impossible d'analyser WSDL (Web Services Description Language). Impossible de trouver un PortType qui correspond au port SOAP.|  
|0xC002922B|-1073573333|DTS_E_WEBSERVICETASK_MSG_NOT_FOUND|Impossible de trouver le message qui correspond à la méthode spécifiée.|  
|0xC002922C|-1073573332|DTS_E_WEBSERVICETASK_CANNOT_GEN_PROXY|Impossible de générer le proxy pour le service Web spécifié. Les erreurs suivantes se sont produites lors de la création du proxy « %1 ».|  
|0xC002922D|-1073573331|DTS_E_WEBSERVICETASK_CANNOT_LOAD_PROXY|Impossible de charger le proxy pour le service Web spécifié. L'erreur exacte est comme suit : « %1 ».|  
|0xC002922E|-1073573330|DTS_E_WEBSERVICETASK_INVALID_SERVICE|Impossible de trouver le service spécifié. L'erreur exacte est comme suit : « %1 ».|  
|0xC002922F|-1073573329|DTS_E_WEBSERVICETASK_WEBMETHOD_INVOKE_FAILED|Le service Web a retourné l'erreur suivante lors de l'exécution de la méthode : « %1 ».|  
|0xC0029230|-1073573328|DTS_E_WEBSERVICETASK_INVOKE_ERR|Impossible d'exécuter la méthode Web. L'erreur exacte est comme suit : « %1 ».|  
|0xC0029231|-1073573327|DTS_E_WEBSERVICETASK_METHODINFO_NULL|MethodInfo ne peut pas être NULL.|  
|0xC0029232|-1073573326|DTS_E_WEBSERVICETASK_VALUE_NOT_PRIMITIVE|La méthode WebMethodInfo spécifiée est incorrecte. La valeur ParamValue fournie ne correspond pas au type ParamType. DTSParamValue n'est pas de type PrimitiveValue.|  
|0xC0029233|-1073573325|DTS_E_WEBSERVICETASK_VALUE_NOT_ENUM|La méthode WebMethodInfo spécifiée est incorrecte. La valeur ParamValue fournie ne correspond pas au type ParamType. La valeur DTSParamValue trouvée n'est pas de type EnumValue.|  
|0xC0029234|-1073573324|DTS_E_VALUE_WEBSERVICETASK_NOT_COMPLEX|La méthode WebMethodInfo spécifiée est incorrecte. La valeur ParamValue fournie ne correspond pas au type ParamType. La valeur DTSParamValue trouvée n'est pas de type ComplexValue.|  
|0xC0029235|-1073573323|DTS_E_WEBSERVICETASK_VALUE_NOT_ARRAY|La méthode WebMethodInfo spécifiée est incorrecte. La valeur ParamValue fournie ne correspond pas au type ParamType. La valeur DTSParamValue trouvée n'est pas de type ArrayValue.|  
|0xC0029236|-1073573322|DTS_E_WEBSERVICETASK_TYPE_NOT_PRIMITIVE|La méthode WebMethodInfo spécifiée est incorrecte. « %1 » n'est pas de type Primitive.|  
|0xC0029237|-1073573321|DTS_E_WEBSERVICETASK_ARRAY_VALUE_INVALID|Le format de ArrayValue n'est pas valide. Au moins un élément doit figurer dans le tableau.|  
|0xC0029238|-1073573320|DTS_E_WEBSERVICETASK_SELECTED_VALUE_NULL|La valeur de l'énumération ne peut pas être NULL. Sélectionnez une valeur par défaut pour l'énumération.|  
|0xC0029239|-1073573319|DTS_E_WEBSERVICETASK_NULL_VALUE|Impossible de valider une valeur NULL dans un type de données.|  
|0xC002923A|-1073573318|DTS_E_WEBSERVICETASK_ENUM_VALUE_NOT_FOUND|La valeur de l'énumération est incorrecte.|  
|0xC002923B|-1073573317|DTS_E_WEBSERVICETASK_PROP_NOT_EXISTS|La classe spécifiée ne contient aucune propriété publique du nom « %1 ».|  
|0xC002923C|-1073573316|DTS_E_WEBSERVICETASK_CONVERT_FAILED|Impossible de convertir « %1 » en « %2 ».|  
|0xC002923D|-1073573315|DTS_E_WEBSERVICETASK_CLEANUP_FAILED|Échec du nettoyage. Le proxy créé pour le service Web n'a peut-être pas été supprimé.|  
|0xC002923E|-1073573314|DTS_E_WEBSERVICETASK_CREATE_INSTANCE_FAILED|Impossible de créer un objet de type « %1 ». Vérifiez si le constructeur par défaut existe.|  
|0xC002923F|-1073573313|DTS_E_WEBSERVICETASK_NOT_PRIMITIVE_TYPE|« %1 » n'est pas un type de valeur.|  
|0xC0029240|-1073573312|DTS_E_WEBSERVICETASK_ERROR_IN_VALIDATE|Impossible de valider « %1 » par rapport à « %1 ».|  
|0xC0029241|-1073573311|DTS_E_WEBSERVICETASK_DATATYPE_NULL|Le type de données ne peut être NULL. Spécifiez la valeur du type de données à valider.|  
|0xC0029242|-1073573310|DTS_E_WEBSERVICETASK_INDEX_OUT_OF_BOUNDS|Impossible d'insérer ParamValue à cette position. L'index spécifié est peut-être inférieur à zéro ou supérieur à la longueur.|  
|0xC0029243|-1073573309|DTS_E_WEBSERVICETASK_WRONG_WSDL|Le fichier WSDL d'entrée n'est pas valide.|  
|0xC0029244|-1073573308|DTS_E_WMIDRTASK_SYNCOBJECTFAILED|Échec de l'objet de synchronisation.|  
|0xC0029245|-1073573307|DTS_E_WMIDRTASK_MISSINGWQLQUERY|La requête WQL est manquante.|  
|0xC0029246|-1073573306|DTS_E_WMIDRTASK_DESTINATIONMUSTBESET|La destination doit être définie.|  
|0xC0029247|-1073573305|DTS_E_WMIDRTASK_MISSINGCONNECTION|Aucune connexion WMI n'est définie.|  
|0xC0029248|-1073573304|DTS_E_WMIDRTASK_INVALIDDATANODE|La tâche Lecteur de données WMI a reçu un nœud de données de tâche incorrect.|  
|0xC0029249|-1073573303|DTS_E_WMIDRTASK_FAILEDVALIDATION|Échec de la validation de la tâche.|  
|0xC002924A|-1073573302|DTS_E_WMIDRTASK_FILEDOESNOTEXIST|Le fichier « %1 » n'existe pas.|  
|0xC002924B|-1073573301|DTS_E_WMIDRTASK_CONNECTIONMNGRDOESNTEXIST|Le gestionnaire de connexions « %1 » n'existe pas.|  
|0xC002924C|-1073573300|DTS_E_WMIDRTASK_VARIABLETYPEISNOTSTRINGOROBJECT|La variable « %1 » n'est pas de type String ou Object.|  
|0xC002924D|-1073573299|DTS_E_WMIDRTASK_CONNECTIONTYPENOTFILE|La connexion « %1 » n'est pas de type « FILE ».|  
|0xC002924E|-1073573298|DTS_E_WMIDRTASK_CONNECTIONTYPENOTWMI|La connexion « %1 » n'est pas de type « WMI ».|  
|0xC002924F|-1073573297|DTS_E_WMIDRTASK_FILEALREADYEXISTS|Le fichier « %1 » existe déjà.|  
|0xC0029250|-1073573296|DTS_E_WMIDRTASK_CONNECTIONMANAGEREMPTY|Le gestionnaire de connexions « %1 » est vide.|  
|0xC0029251|-1073573295|DTS_E_WMIDRTASK_VARNOTOBJECT|La variable « %1 » doit être un objet de type à affecter à une table de données.|  
|0xC0029252|-1073573294|DTS_E_WMIDRTASK_TASKFAILURE|Échec de la tâche en raison d'une requête WMI non valide : « %1 ».|  
|0xC0029253|-1073573293|DTS_E_WMIDRTASK_CANTWRITETOVAR|Impossible d'écrire dans la variable « %1 » car elle est définie pour conserver sa valeur d'origine.|  
|0xC0029254|-1073573292|DTS_E_WMIEWTASK_SYNCOBJECTFAILED|Échec de l'objet de synchronisation.|  
|0xC0029255|-1073573291|DTS_E_WMIEWTASK_MISSINGWQLQUERY|La requête WQL est manquante.|  
|0xC0029256|-1073573290|DTS_E_WMIEWTASK_MISSINGCONNECTION|La connexion WMI est manquante.|  
|0xC0029257|-1073573289|DTS_E_WMIEWTASK_QUERYFAILURE|La tâche n'a pas pu exécuter la requête WMI.|  
|0xC0029258|-1073573288|DTS_E_WMIEWTASK_INVALIDDATANODE|La tâche Observateur d'événement WMI a reçu un nœud de données de tâche non valide.|  
|0xC0029259|-1073573287|DTS_E_WMIEWTASK_CONNECTIONMNGRDOESNTEXIST|Le gestionnaire de connexions « %1 » n'existe pas.|  
|0xC002925A|-1073573286|DTS_E_WMIEWTASK_FILEDOESNOTEXIST|Le fichier « %1 » n'existe pas.|  
|0xC002925B|-1073573285|DTS_E_WMIEWTASK_VARIABLETYPEISNOTSTRING|La variable « %1 » n'est pas de type String.|  
|0xC002925C|-1073573284|DTS_E_WMIEWTASK_CONNECTIONTYPENOTFILE|La connexion « %1 » n'est pas de type « FILE ».|  
|0xC002925D|-1073573283|DTS_E_WMIEWTASK_CONNECTIONTYPENOTWMI|La connexion « %1 » n'est pas de type « WMI ».|  
|0xC002925E|-1073573282|DTS_E_WMIEWTASK_FILEALREADYEXISTS|Le fichier « %1 » existe déjà.|  
|0xC002925F|-1073573281|DTS_E_WMIEWTASK_CONNECTIONMANAGEREMPTY|Le gestionnaire de connexions « %1 » est vide.|  
|0xC0029260|-1073573280|DTS_E_WMIEWTASK_TIMEOUTOCCURRED|Délai d'attente de « %1 » seconde(s) dépassé avant l'événement représenté par « %2 ».|  
|0xC0029261|-1073573279|DTS_E_WMIEWTASK_ERRMESSAGE|La surveillance de la requête WQL a provoqué l'exception système suivante : « %1 ». Vérifiez les erreurs dans la requête ou les droits/autorisations d'accès de la connexion WMI.|  
|0xC0029262|-1073573278|DTS_E_XMLTASK_NODEFAULTOPERTION|Les opérations spécifiées ne sont pas définies.|  
|0xC0029263|-1073573277|DTS_E_XMLTASK_CONNECTIONTYPENOTFILE|La connexion n'est pas de type File.|  
|0xC0029264|-1073573276|DTS_E_XMLTASK_CANTGETREADERFROMSOURCE|Impossible d'obtenir XmlReader à partir du document XML source.|  
|0xC0029265|-1073573275|DTS_E_XMLTASK_CANTGETREADERFROMDEST|Impossible d'obtenir XmlReader à partir du document XML modifié.|  
|0xC0029266|-1073573274|DTS_E_XMLTASK_CANTGETREADERFROMDIFFGRAM|Impossible d'obtenir le lecteur DiffGram XDL à partir du fichier XML DiffGram XDL.|  
|0xC0029268|-1073573272|DTS_E_XMLTASK_EMPTYNODELIST|La liste de nœuds est vide.|  
|0xC0029269|-1073573271|DTS_E_XMLTASK_NOELEMENTFOUND|L'élément est introuvable.|  
|0xC002926A|-1073573270|DTS_E_XMLTASK_UNDEFINEDOPERATION|Les opérations spécifiées ne sont pas définies.|  
|0xC002926B|-1073573269|DTS_E_XMLTASK_XPATHNAVERROR|Élément de contenu inattendu dans XPathNavigator.|  
|0xC002926C|-1073573268|DTS_E_XMLTASK_NOSCHEMAFOUND|Schéma introuvable pour appliquer la validation.|  
|0xC002926D|-1073573267|DTS_E_XMLTASK_VALIDATIONERROR|Une erreur de validation s'est produite lors de la validation de l'instance du document.|  
|0xC002926E|-1073573266|DTS_E_XMLTASK_SYNCOBJECTFAILED|Échec de l'objet de synchronisation.|  
|0xC002926F|-1073573265|DTS_E_XMLTASK_ROOTNOODESNOTMATCHED|Les nœuds racines ne correspondent pas.|  
|0xC0029270|-1073573264|DTS_E_XMLTASK_INVALIDEDITSCRIPT|Le type d'opération d'édition de script n'est pas valide dans le script d'édition final.|  
|0xC0029271|-1073573263|DTS_E_XMLTASK_CDATANODESISSUE|Les nœuds CDATA doivent être ajoutés à la classe DiffgramAddSubtrees.|  
|0xC0029272|-1073573262|DTS_E_XMLTASK_COMMENTSNODEISSUE|Les nœuds de commentaires doivent être ajoutés à la classe DiffgramAddSubtrees.|  
|0xC0029273|-1073573261|DTS_E_XMLTASK_TEXTNODEISSUES|Les nœuds de texte doivent être ajoutés à la classe DiffgramAddSubtrees.|  
|0xC0029274|-1073573260|DTS_E_XMLTASK_WHITESPACEISSUE|Les nœuds d'espaces significatifs doivent être ajoutés à la classe DiffgramAddSubtrees.|  
|0xC0029275|-1073573259|DTS_E_XMLTASK_DIFFENUMISSUE|Corrigez le tableau OperationCost de façon à ce qu'il reflète l'énumération XmlDiffOperation.|  
|0xC0029276|-1073573258|DTS_E_XMLTASK_TASKISEMPTY|La tâche ne contient aucune opération.|  
|0xC0029277|-1073573257|DTS_E_XMLTASK_DOCUMENTHASDATA|Le document contient déjà des données et ne doit pas être réutilisé.|  
|0xC0029278|-1073573256|DTS_E_XMLTASK_INVALIDENODETYPE|Le type de nœud n'est pas valide.|  
|0xC0029279|-1073573255|DTS_E_XMLTASK_INVALIDDATANODE|La tâche XML a reçu un nœud de données de tâche non valide.|  
|0xC002927B|-1073573253|DTS_E_XMLTASK_VARIABLETYPEISNOTSTRING|Les données de variable ne sont pas de type String.|  
|0xC002927C|-1073573252|DTS_E_XMLTASK_COULDNOTGETENCODINGFROMDOCUMENT|Impossible d'obtenir l'encodage à partir du fichier XML.|  
|0xC002927D|-1073573251|DTS_E_XMLTASK_MISSINGSOURCE|La source n'est pas spécifiée.|  
|0xC002927E|-1073573250|DTS_E_XMLTASK_MISSINGSECONDOPERAND|Le second opérande n'est pas spécifié.|  
|0xC002927F|-1073573249|DTS_E_XMLTASK_INVALIDPATHDESCRIPTOR|Diffgram XDL non valide. « %1 » est un descripteur de chemin d'accès non valide.|  
|0xC0029280|-1073573248|DTS_E_XMLTASK_NOMATCHINGNODE|Diffgram XDL non valide. Aucun nœud ne correspond au descripteur de chemin d'accès « %1 ».|  
|0xC0029281|-1073573247|DTS_E_XMLTASK_EXPECTINGDIFFGRAMELEMENT|Diffgram XDL non valide. xd:xmldiff attendu comme élément racine avec l'URI de l'espace de noms « %1 ».|  
|0xC0029282|-1073573246|DTS_E_XMLTASK_MISSINGSRCDOCATTRIBUTE|DiffGram XDL non valide. L'attribut srcDocHash de l'élément xd:xmldiff est manquant.|  
|0xC0029283|-1073573245|DTS_E_XMLTASK_MISSINGOPTIONSATTRIBUTE|DiffGram XDL non valide. L'attribut options de l'élément xd:xmldiff est manquant.|  
|0xC0029284|-1073573244|DTS_E_XMLTASK_INVALIDSRCDOCATTRIBUTE|DiffGram XDL non valide. La valeur de l'attribut srcDocHash n'est pas valide.|  
|0xC0029285|-1073573243|DTS_E_XMLTASK_INVALIDOPTIONSATTRIBUTE|DiffGram XDL non valide. L'attribut des options a une valeur non valide.|  
|0xC0029286|-1073573242|DTS_E_XMLTASK_SRCDOCMISMATCH|DiffGram XDL ne s'applique pas à ce document XML. La valeur rcDocHash ne correspond pas.|  
|0xC0029287|-1073573241|DTS_E_XMLTASK_MORETHANONENODEMATCHED|DiffGram XDL non valide. Plusieurs nœuds correspondent au descripteur de chemin d'accès « %1 » dans l'élément xd:node ou xd:change.|  
|0xC0029288|-1073573240|DTS_E_XMLTASK_XMLDECLMISMATCH|DiffGram XDL ne s'applique pas à ce document XML. Impossible d'ajouter une nouvelle déclaration XML.|  
|0xC0029289|-1073573239|DTS_E_XMLTASK_INTERNALERRORMORETHANONENODEINLIST|Erreur interne. XmlDiffPathSingleNodeList ne peut contenir qu'un seul nœud.|  
|0xC002928A|-1073573238|DTS_E_XMLTASK_INTERNALERRORMORETHANONENODELEFT|Erreur interne. « %1 » nœuds restants après correction, 1 nœud attendu.|  
|0xC002928B|-1073573237|DTS_E_XMLTASK_XSLTRESULTFILEISNOTXML|Le fichier/texte généré par XSLT n’est pas un XmlDocument valide, et par conséquent ne peut pas être le résultat de l’opération : « %1 ».|  
|0xC002928E|-1073573234|DTS_E_XMLTASK_FILEDOESNOTEXIST|Aucun fichier n'est associé à la connexion « %1 ».|  
|0xC002928F|-1073573233|DTS_E_XMLTASK_XMLTEXTEMPTY|La propriété « %1 » n'a aucun texte XML source ; le texte XML est une chaîne non valide, NULL ou vide.|  
|0xC0029290|-1073573232|DTS_E_XMLTASK_FILEALREADYEXISTS|Le fichier « %1 » existe déjà.|  
|0xC0029293|-1073573229|DTS_E_TRANSFERTASKS_SRCCONNECTIONREQUIRED|Une connexion source doit être indiquée.|  
|0xC0029294|-1073573228|DTS_E_TRANSFERTASKS_DESTCONNECTIONREQUIRED|Une connexion de destination doit être indiquée.|  
|0xC0029295|-1073573227|DTS_E_TRANSFERTASKS_CONNECTIONNOTFOUND|La connexion « %1 » est introuvable dans le package.|  
|0xC0029296|-1073573226|DTS_E_TRANSFERTASKS_SERVERVERSIONNOTALLOWED|La connexion « %1 » spécifie une instance SQL Server dont la version n'est pas prise en charge pour le transfert.  Seules les versions 7, 2000 et 2005 sont prises en charge.|  
|0xC0029297|-1073573225|DTS_E_TRANSFERTASKS_SRCSERVERLESSEQUALDESTSERVER|La connexion source « %1 » doit spécifier une instance SQL Server avec une version antérieure ou égale à la connexion de destination « %2 ».|  
|0xC0029298|-1073573224|DTS_E_TRANSFERTASKS_SRCDBREQUIRED|Une base de données source doit être indiquée.|  
|0xC0029299|-1073573223|DTS_E_TRANSFERTASKS_SRCDBMUSTEXIST|La base de données source « %1 » doit exister sur le serveur source.|  
|0xC002929A|-1073573222|DTS_E_TRANSFERTASKS_DESTDBREQUIRED|Une base de données de destination doit être indiquée.|  
|0xC002929B|-1073573221|DTS_E_TRANSFERTASKS_SRCDBANDDESTDBTHESAME|Les bases de données source et de destination ne peuvent pas être identiques.|  
|0xC002929C|-1073573220|DTS_E_TRANSFERDBTASK_FILENAMEREQUIRED|Le nom de fichier est manquant dans les informations sur le fichier de transfert %1.|  
|0xC002929D|-1073573219|DTS_E_TRANSFERDBTASK_FOLDERREQUIRED|La partie relative au dossier est manquante dans les informations sur le fichier de transfert %1.|  
|0xC002929E|-1073573218|DTS_E_TRANSFERTASKS_NETSHAREREQUIRED|La partie relative au partage réseau est manquante dans les informations sur le fichier de transfert %1.|  
|0xC002929F|-1073573217|DTS_E_TRANSFERTASKS_FILELISTSCOUNTMISMATCH|Le nombre de fichiers de transfert sources et le nombre de fichiers de transfert de destination doivent être identiques.|  
|0xC00292A0|-1073573216|DTS_E_DOESNOTSUPPORTTRANSACTIONS|L'inscription dans les transactions n'est pas prise en charge.|  
|0xC00292A1|-1073573215|DTS_E_TRANSFERDBTASK_OFFLINEERROR|L'exception suivante s'est produite pendant un transfert de base de données hors connexion : %1.|  
|0xC00292A2|-1073573214|DTS_E_TRANSFERDBTASK_NETSHAREDOESNOTEXIST|Le partage réseau « %1 » est introuvable.|  
|0xC00292A3|-1073573213|DTS_E_TRANSFERDBTASK_NETSHARENOACCESS|Le partage réseau « %1 » est inaccessible.  Erreur : %2.|  
|0xC00292A4|-1073573212|DTS_E_TRANSFERDBTASK_USERMUSTBEDBOORSYSADMIN|L'utilisateur « %1 » doit être un propriétaire de base de données ou un administrateur système « %2 » pour effectuer un transfert de base de données en ligne.|  
|0xC00292A5|-1073573211|DTS_E_TRANSFERDBTASK_USERMUSTBESYSADMIN|L'utilisateur « %1 » doit être administrateur système sur « %2 » pour effectuer un transfert de base de données hors connexion.|  
|0xC00292A6|-1073573210|DTS_E_TRANSFERDBTASK_FTCATALOGSOFFLINEYUKONONLY|Les catalogues de texte intégral ne peuvent être inclus que lors d'un transfert de base de données hors connexion entre 2 serveurs SQL Server 2005.|  
|0xC00292A7|-1073573209|DTS_E_TRANSFERDBTASK_NOOVERWRITEDB|La base de données « %1 » existe déjà sur le serveur de destination « %2 ».|  
|0xC00292A8|-1073573208|DTS_E_TRANSFERDBTASK_MUSTHAVESOURCEFILES|Au moins un fichier source doit être indiqué.|  
|0xC00292A9|-1073573207|DTS_E_TRANSFERDBTASKS_SRCFILENOTFOUND|Impossible de trouver le fichier « %1 » dans la base de données source « %2 ».|  
|0xC00292B3|-1073573197|DTS_E_MSMQTASK_FIPS1402COMPLIANCE|L'opération demandée n'est pas autorisée sur les systèmes conformes à la norme U.S. FIPS 140-2.|  
|0xC002F210|-1073548784|DTS_E_SQLTASK_ERROREXECUTINGTHEQUERY|L'exécution de la requête « %1 » a échoué avec l'erreur suivante : « %2 ». Causes possibles de cet échec : problèmes liés à la requête, propriété « ResultSet » non définie correctement, paramètres non définis correctement ou connexion non établie correctement.|  
|0xC002F300|-1073548544|DTS_E_TRANSFERSPTASK_ERRORREADINGSPNAMES|Erreur lors de la lecture des noms des procédures stockées dans le fichier XML.|  
|0xC002F301|-1073548543|DTS_E_TRANSFERSPTASK_INVALIDDATANODE|Nœud de données non valide pour la tâche de transfert de procédures stockées.|  
|0xC002F302|-1073548542|DTS_E_TRANSFERTASKS_CONNECTIONTYPEISNOTSMOSERVER|La connexion « %1 » n'est pas de type « SMOServer ».|  
|0xC002F303|-1073548541|DTS_E_TRANSFERSPTASK_EXECUTIONFAILED|L'exécution a échoué avec l'erreur suivante : « %1 ».|  
|0xC002F304|-1073548540|DTS_E_ERROROCCURREDWITHFOLLOWINGMESSAGE|Une erreur s'est produite avec le message d'erreur suivant : « %1 ».|  
|0xC002F305|-1073548539|DTS_E_BITASK_EXECUTION_FAILED|Échec d'exécution de l'insertion en bloc.|  
|0xC002F306|-1073548538|DTS_E_FSTASK_INVALIDDESTPATH|Chemin de destination non valide.|  
|0xC002F307|-1073548537|DTS_E_FSTASK_CANTCREATEDIR|Impossible de créer le répertoire. L'utilisateur a choisi l'échec de la tâche si le répertoire existe.|  
|0xC002F308|-1073548536|DTS_E_SQLTASK_ODBCNOSUPPORTTRANSACTION|La tâche comporte une option de transaction « Nécessaire » et la connexion « %1 » est de type « ODBC ». Les connexions ODBC ne prennent pas en charge les transactions.|  
|0xC002F309|-1073548535|DTS_E_SQLTASK_ERRORASSIGINGVALUETOVAR|Une erreur s'est produite lors de l'affectation d'une valeur à la variable « %1 » : « %2 ».|  
|0xC002F30A|-1073548534|DTS_E_FSTASK_SOURCEISEMPTY|La source est vide.|  
|0xC002F30B|-1073548533|DTS_E_FSTASK_DESTINATIONISEMPTY|La destination est vide.|  
|0xC002F30C|-1073548532|DTS_E_FSTASK_FILEDIRNOTFOUND|Le fichier ou le répertoire « %1 » n'existe pas.|  
|0xC002F30D|-1073548531|DTS_E_FSTASK_VARSRCORDESTISEMPTY|La variable « %1 » est utilisée comme source ou destination et est vide.|  
|0xC002F30E|-1073548530|DTS_E_FSTASK_FILEDELETED|Le fichier ou le répertoire « %1 » a été supprimé.|  
|0xC002F30F|-1073548529|DTS_E_FSTASK_DIRECTORYDELETED|Le répertoire « %1 » a été supprimé.|  
|0xC002F310|-1073548528|DTS_E_WMIDRTASK_VARIABLETYPEISNOTOBJECT|La variable « %1 » doit être un objet de type à affecter à une table de données.|  
|0xC002F311|-1073548527|DTS_E_WMIDRTASK_VARIABLETYPEISNOTSTRING|La variable « %1 » ne comporte pas un type de données String.|  
|0xC002F312|-1073548526|DTS_E_FTPTASK_CANNOTACQUIRECONNECTION|Une erreur s'est produite lors de l'acquisition de la connexion FTP. Vérifiez qu'un type de connexion valide est spécifié dans « %1 ».|  
|0xC002F313|-1073548525|DTS_E_FTPTASK_CONNECTIONNOTFOUND|Le gestionnaire de connexions FTP « %1 » est introuvable.|  
|0xC002F314|-1073548524|DTS_E_FTPTASK_FILEUSAGETYPEERROR|Le type de connexion d'utilisation de fichiers « %1 » doit être « %2 » pour l'opération « %3 ».|  
|0xC002F315|-1073548523|DTS_E_TRANSFERTASKS_SOURCECANTBESAMEASDESTINATION|Le serveur source ne peut pas être le même que le serveur de destination.|  
|0xC002F316|-1073548522|DTS_E_ERRMSGTASK_EMPTYSOURCELIST|Aucun message d'erreur à transférer.|  
|0xC002F317|-1073548521|DTS_E_ERRMSGTASK_DIFFERENTMESSAGEANDLANGUAGESIZES|Les listes de messages d'erreurs et leurs langues correspondantes sont de tailles différentes.|  
|0xC002F318|-1073548520|DTS_E_ERRMSGTASK_ERRORMESSAGEOUTOFRANGE|L'ID du message d'erreur « %1 » se situe hors de la plage autorisée des messages d'erreur définis par l'utilisateur. Les ID des messages d'erreurs définis par l'utilisateur sont compris entre 50 000 et 2 147 483 647.|  
|0xC002F319|-1073548519|DTS_E_TRANSFERTASKS_NOTRANSACTIONSUPPORT|Cette tâche ne peut pas participer à une transaction.|  
|0xC002F320|-1073548512|DTS_E_ERRMSGTASK_FAILEDTOTRANSFERERRORMESSAGES|Échec du transfert d'une partie ou de la totalité des messages d'erreur.|  
|0xC002F321|-1073548511|DTS_E_ERRMSGTASK_ERRORMESSAGEALREADYEXISTS|Le message d'erreur « %1 » existe déjà sur le serveur de destination.|  
|0xC002F324|-1073548508|DTS_E_ERRMSGTASK_ERRORMESSAGECANTBEFOUND|Le message d'erreur « %1 » est introuvable sur le serveur source.|  
|0xC002F325|-1073548507|DTS_E_TRANSFERTASKS_EXECUTIONFAILED|L'exécution a échoué avec l'erreur suivante : « %1 ».|  
|0xC002F327|-1073548505|DTS_E_JOBSTASK_FAILEDTOTRANSFERJOBS|Échec du transfert du travail ou des travaux.|  
|0xC002F330|-1073548496|DTS_E_JOBSTASK_EMPTYSOURCELIST|Aucun travail à transférer.|  
|0xC002F331|-1073548495|DTS_E_JOBSTASK_JOBEXISTSATDEST|Le travail « %1 » existe déjà sur le serveur de destination.|  
|0xC002F334|-1073548492|DTS_E_JOBSTASK_JOBCANTBEFOUND|Le travail « %1 » est introuvable sur le serveur source.|  
|0xC002F337|-1073548489|DTS_E_LOGINSTASK_EMPTYLIST|La liste des « noms d'accès » à transférer est vide.|  
|0xC002F338|-1073548488|DTS_E_LOGINSTASK_CANTGETLOGINSNAMELIST|Impossible d'obtenir la liste des « noms d'accès » à partir du serveur source.|  
|0xC002F340|-1073548480|DTS_E_LOGINSTASK_ERRORLOGINEXISTS|Le nom d'accès « %1 » existe déjà sur le serveur de destination.|  
|0xC002F342|-1073548478|DTS_E_LOGINSTASK_LOGINNOTFOUND|Le nom d'accès « %1 » n'existe pas à la source.|  
|0xC002F344|-1073548476|DTS_E_LOGINSTASK_FAILEDTOTRANSFERLOGINS|Échec du transfert d'une partie ou de la totalité des noms d'accès.|  
|0xC002F345|-1073548475|DTS_E_STOREDPROCSTASK_FAILEDTOTRANSFERSPS|Échec du transfert de la ou des procédures stockées. Des erreurs plus détaillées devraient avoir été déclenchées.|  
|0xC002F346|-1073548474|DTS_E_STOREDPROCSTASK_STOREDPROCNOTFOUND|La procédure stockée « %1 » est introuvable à la source.|  
|0xC002F349|-1073548471|DTS_E_STOREDPROCSTASK_ERRORSTOREDPROCEDUREEXISTS|La procédure stockée « %1 » existe déjà sur le serveur de destination.|  
|0xC002F350|-1073548464|DTS_E_STOREDPROCSTASK_EMPTYSOURCELIST|Aucune procédure stockée à transférer.|  
|0xC002F353|-1073548461|DTS_E_TRANSOBJECTSTASK_FAILEDTOTRANSFEROBJECTS|Échec du transfert de l'objet ou des objets.|  
|0xC002F354|-1073548460|DTS_E_TRANSOBJECTSTASK_EMPTYLIST|La liste « d'objets » à transférer est vide.|  
|0xC002F355|-1073548459|DTS_E_TRANSOBJECTSTASK_NOSPATSOURCE|La procédure stockée « %1 » n'existe pas à la source.|  
|0xC002F356|-1073548458|DTS_E_TRANSOBJECTSTASK_SPALREADYATDEST|La procédure stockée « %1 » existe déjà à la destination.|  
|0xC002F357|-1073548457|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGSPS|Une erreur s'est produite lors de la tentative de définition de la liste des procédures stockées à transférer : « %1 ».|  
|0xC002F359|-1073548455|DTS_E_TRANSOBJECTSTASK_NORULEATSOURCE|La règle « %1 » n'existe pas à la source.|  
|0xC002F360|-1073548448|DTS_E_TRANSOBJECTSTASK_RULEALREADYATDEST|La règle « %1 » existe déjà à la destination.|  
|0xC002F361|-1073548447|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGRULES|Une erreur s'est produite lors de la tentative de définition de la liste des règles à transférer : « %1 ».|  
|0xC002F363|-1073548445|DTS_E_TRANSOBJECTSTASK_NOTABLEATSOURCE|La table « %1 » n'existe pas à la source.|  
|0xC002F364|-1073548444|DTS_E_TRANSOBJECTSTASK_TABLEALREADYATDEST|La table « %1 » existe déjà à la destination.|  
|0xC002F365|-1073548443|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGTABLES|Une erreur s'est produite lors de la tentative de définition de la liste des tables à transférer : « %1 ».|  
|0xC002F367|-1073548441|DTS_E_TRANSOBJECTSTASK_NOVIEWATSOURCE|La vue « %1 » n'existe pas à la source.|  
|0xC002F368|-1073548440|DTS_E_TRANSOBJECTSTASK_VIEWALREADYATDEST|La vue « %1 » existe déjà à la destination.|  
|0xC002F369|-1073548439|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGVIEWS|Une erreur s'est produite lors de la tentative de définition de la liste des vues à transférer : « %1 ».|  
|0xC002F371|-1073548431|DTS_E_TRANSOBJECTSTASK_NOUDFATSOURCE|La fonction définie par l'utilisateur « %1 » n'existe pas à la source.|  
|0xC002F372|-1073548430|DTS_E_TRANSOBJECTSTASK_UDFALREADYATDEST|La fonction définie par l'utilisateur « %1 » existe déjà à la destination.|  
|0xC002F373|-1073548429|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGUDFS|Une erreur s'est produite lors de la tentative de définition de la liste des fonctions définies par l'utilisateur à transférer : « %1 ».|  
|0xC002F375|-1073548427|DTS_E_TRANSOBJECTSTASK_NODEFAULTATSOURCE|La valeur par défaut « %1 » n'existe pas à la source.|  
|0xC002F376|-1073548426|DTS_E_TRANSOBJECTSTASK_DEFAULTALREADYATDEST|La valeur par défaut « %1 » existe déjà à la destination.|  
|0xC002F377|-1073548425|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGDEFAULTS|Une erreur s'est produite lors de la tentative de définition de la liste des valeurs par défaut à transférer : « %1 ».|  
|0xC002F379|-1073548423|DTS_E_TRANSOBJECTSTASK_NOUDDTATSOURCE|Le type de données défini par l'utilisateur « %1 » n'existe pas à la source.|  
|0xC002F380|-1073548416|DTS_E_TRANSOBJECTSTASK_UDDTALREADYATDEST|Le type de données défini par l'utilisateur « %1 » existe déjà à la destination.|  
|0xC002F381|-1073548415|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGUDDTS|Une erreur s'est produite lors de la tentative de définition de la liste des types de données définis par l'utilisateur à transférer : « %1 ».|  
|0xC002F383|-1073548413|DTS_E_TRANSOBJECTSTASK_NOPFATSOURCE|La fonction de partition « %1 » n'existe pas à la source.|  
|0xC002F384|-1073548412|DTS_E_TRANSOBJECTSTASK_PFALREADYATDEST|La fonction de partition « %1 » existe déjà à la destination.|  
|0xC002F385|-1073548411|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGPFS|Une erreur s'est produite lors de la tentative de définition de la liste de fonctions de partition à transférer : « %1 ».|  
|0xC002F387|-1073548409|DTS_E_TRANSOBJECTSTASK_NOPSATSOURCE|Le schéma de partition « %1 » n'existe pas à la source.|  
|0xC002F388|-1073548408|DTS_E_TRANSOBJECTSTASK_PSALREADYATDEST|Le schéma de partition « %1 » existe déjà à la destination.|  
|0xC002F389|-1073548407|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGPSS|Une erreur s'est produite lors de la tentative de définition de la liste des schémas de partition à transférer : « %1 ».|  
|0xC002F391|-1073548399|DTS_E_TRANSOBJECTSTASK_NOSCHEMAATSOURCE|Le schéma « %1 » n'existe pas à la source.|  
|0xC002F392|-1073548398|DTS_E_TRANSOBJECTSTASK_SCHEMAALREADYATDEST|Le schéma « %1 » existe déjà à la destination.|  
|0xC002F393|-1073548397|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGSCHEMAS|Une erreur s'est produite lors de la tentative de définition de la liste des schémas à transférer : « %1 ».|  
|0xC002F395|-1073548395|DTS_E_TRANSOBJECTSTASK_NOSQLASSEMBLYATSOURCE|SqlAssembly « %1 » n'existe pas à la source.|  
|0xC002F396|-1073548394|DTS_E_TRANSOBJECTSTASK_SQLASSEMBLYALREADYATDEST|SqlAssembly « %1 » existe déjà à la destination.|  
|0xC002F397|-1073548393|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGSQLASSEMBLIES|Une erreur s'est produite lors de la tentative de définition de la liste de SqlAssembly à transférer : « %1 ».|  
|0xC002F399|-1073548391|DTS_E_TRANSOBJECTSTASK_NOAGGREGATEATSOURCE|La fonction d'agrégation définie par l'utilisateur « %1 » n'existe pas à la source.|  
|0xC002F400|-1073548288|DTS_E_TRANSOBJECTSTASK_AGGREGATEALREADYATDEST|La fonction d'agrégation définie par l'utilisateur « %1 » existe déjà à la destination.|  
|0xC002F401|-1073548287|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGAGGREGATES|Une erreur s'est produite lors de la tentative de définition de la liste des fonctions d'agrégation définies par l'utilisateur à transférer : « %1 ».|  
|0xC002F403|-1073548285|DTS_E_TRANSOBJECTSTASK_NOTYPEATSOURCE|Le type défini par l'utilisateur « %1 » n'existe pas à la source.|  
|0xC002F404|-1073548284|DTS_E_TRANSOBJECTSTASK_TYPEALREADYATDEST|Le type défini par l'utilisateur « %1 » existe déjà à la destination.|  
|0xC002F405|-1073548283|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGTYPES|Une erreur s'est produite lors de la tentative de définition de la liste de types définis par l'utilisateur à transférer : « %1 ».|  
|0xC002F407|-1073548281|DTS_E_TRANSOBJECTSTASK_NOXMLSCHEMACOLLECTIONATSOURCE|XmlSchemaCollection « %1 » n'existe pas à la source.|  
|0xC002F408|-1073548280|DTS_E_TRANSOBJECTSTASK_XMLSCHEMACOLLECTIONALREADYATDEST|XmlSchemaCollection « %1 » existe déjà à la destination.|  
|0xC002F409|-1073548279|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGXMLSCHEMACOLLECTIONS|Une erreur s'est produite lors de la tentative de définition de la liste de XmlSchemaCollection à transférer : « %1 ».|  
|0xC002F411|-1073548271|DTS_E_TRANSOBJECTSTASK_SUPPORTEDONYUKONONLY|Les objets de type « %1 » sont uniquement pris en charge entre des serveurs SQL Server 2005 ou plus récents.|  
|0xC002F413|-1073548269|DTS_E_LOGINSTASK_EMPTYDATABASELIST|La liste des bases de données est vide.|  
|0xC002F414|-1073548268|DTS_E_TRANSOBJECTSTASK_NOLOGINATSOURCE|Le nom d'accès « %1 » n'existe pas à la source.|  
|0xC002F416|-1073548266|DTS_E_TRANSOBJECTSTASK_LOGINALREADYATDEST|Le nom d'accès « %1 » existe déjà à la destination.|  
|0xC002F417|-1073548265|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGLOGINS|Une erreur s'est produite lors de la tentative de définition de la liste de noms d'accès à transférer : « %1 ».|  
|0xC002F419|-1073548263|DTS_E_TRANSOBJECTSTASK_NOUSERATSOURCE|Le nom d'accès « %1 » n'existe pas à la source.|  
|0xC002F41B|-1073548261|DTS_E_TRANSOBJECTSTASK_USERALREADYATDEST|L'utilisateur « %1 » existe déjà à la destination.|  
|0xC002F41C|-1073548260|DTS_E_TRANSOBJECTSTASK_ERRORHANDLINGUSERS|Une erreur s'est produite lors de la tentative de définition de la liste d'utilisateurs à transférer : « %1 ».|  
|0xC002F41F|-1073548257|DTS_E_BITASK_CANNOTRETAINCONNINTRANSACTION|Impossible de conserver un gestionnaire de connexions pour la tâche dans une transaction.|  
|0xC002F421|-1073548255|DTS_E_SQLTASKOUTPUTENCODINGNOTSUPPORTED|Impossible d'obtenir des données XML de SQL Server au format Unicode, car le fournisseur ne prend pas en charge la propriété OUTPUTENCODING.|  
|0xC002F426|-1073548250|DTS_E_FTPTASK_FILECONNECTIONNOTFOUND|Pour l'opération FTP « %1 », le gestionnaire de connexions FILE « %2 » est introuvable.|  
|0xC002F428|-1073548248|DTS_E_TRANSOBJECTSTASK_CANNOTDROPOBJECTS|Les « noms d'accès » sont des objets de niveau serveur qui ne peuvent pas être supprimés en premier, car la source et la destination se trouvent sur le même serveur. La suppression des objets en premier supprimera les noms d'accès de la source également.|  
|0xC002F429|-1073548247|DTS_E_SQLTASK_PARAMSIZEERROR|Le paramètre « %1 » ne peut pas être négatif. (-1) est utilisé pour la valeur par défaut.|  
|0xC0040019|-1073479655|DTS_E_UNREGISTEREDPIPELINEXML_LOAD|Impossible de charger les objets de flux de données. Vérifiez que l'inscription de Microsoft.SqlServer.PipelineXml.dll est correcte.|  
|0xC0040020|-1073479648|DTS_E_UNREGISTEREDPIPELINEXML_SAVE|Impossible d'enregistrer les objets de flux de données. Vérifiez que l'inscription de Microsoft.SqlServer.PipelineXml.dll est correcte.|  
|0xC0040040|-1073479616|DTS_E_PIPELINE_SAVE|Échec de l'enregistrement des objets de flux de données.|  
|0xC0040041|-1073479615|DTS_E_PIPELINE_LOAD|Échec du chargement des objets de flux de données|  
|0xC0040042|-1073479614|DTS_E_SAVE_PERSTFORMAT|Échec de l'enregistrement des objets de flux de données. Le format spécifié n'est pas pris en charge.|  
|0xC0040043|-1073479613|DTS_E_LOAD_PERSTFORMAT|Échec du chargement des objets de flux de données. Le format spécifié n'est pas pris en charge.|  
|0xC0040044|-1073479612|DTS_E_SETPERSIST_PROPEVENTS|Échec de la définition de la propriété des événements de persistance XML pour les objets de flux de données.|  
|0xC0040045|-1073479611|DTS_E_SETPERSIST_XMLDOM|Échec de la définition de la propriété DOM XML de persistance pour les objets de flux de données.|  
|0xC0040046|-1073479610|DTS_E_SETPERSIST_XMLNODE|Échec de la définition de la propriété ELEMENT XML de persistance pour les objets de flux de données.|  
|0xC0040047|-1073479609|DTS_E_SETPERSISTPROP_FAILED|Échec de la définition des propriétés de persistance xml pour les objets de flux de données.|  
|0xC0040048|-1073479608|DTS_E_NOCUSTOMPROPCOL|Échec de l'obtention de la collection de propriétés personnalisées pour les composants de flux de données.|  
|0xC0047000|-1073451008|DTS_E_CYCLEINEXECUTIONTREE|Une arborescence d'exécution contient un cycle.|  
|0xC0047001|-1073451007|DTS_E_DISCONNECTEDOBJECT|L'objet %1 « %2 » (%3!d!) est déconnecté de la structure.|  
|0xC0047002|-1073451006|DTS_E_INVALIDOBJECTID|L'ID de l'objet de structure n'est pas valide.|  
|0xC0047003|-1073451005|DTS_E_INPUTWITHOUTPATHS|Un objet d'entrée requis n'est pas connecté à un objet de chemin.|  
|0xC0047005|-1073451003|DTS_E_INVALIDSYNCHRONOUSINPUT|%1 a un ID d'entrée synchrone non valide %2!d!.|  
|0xC0047006|-1073451002|DTS_E_INVALIDOUTPUTLINEAGEID|%1 a l'ID de lignage %2!d!, or %3!d! était attendu.|  
|0xC0047008|-1073451000|DTS_E_DUPLICATENAMESINCOLLECTION|Le package contient deux objets de nom dupliqué « %1 » et « %2 ».|  
|0xC0047009|-1073450999|DTS_E_INVALIDEXCLUSIONGROUP|« %1 » et « %2 » sont dans le même groupe d'exclusions, mais n'ont pas la même entrée synchrone.|  
|0xC004700A|-1073450998|DTS_E_DUPLICATELINEAGEIDSINCOLLECTION|Deux objets de la même collection ont un ID de lignage dupliqué %1!d!. Il s'agit des objets %2 et %3.|  
|0xC004700B|-1073450997|DTS_E_VALIDATIONFAILEDONLAYOUT|Échec de la validation de la structure.|  
|0xC004700C|-1073450996|DTS_E_VALIDATIONFAILEDONCOMPONENTS|Échec de la validation d'un ou de plusieurs composants.|  
|0xC004700D|-1073450995|DTS_E_VALIDATIONFAILED|Échec de la validation de la structure et d'un ou plusieurs composants.|  
|0xC004700E|-1073450994|DTS_E_THREADSTARTUPFAILED|Le moteur de la tâche de flux de données n'a pas pu démarré car il n'a pas pu créer un ou plusieurs threads requis.|  
|0xC004700F|-1073450993|DTS_E_CANTGETMUTEX|Un thread n'a pas pu créer une exclusion mutuelle lors de l'initialisation.|  
|0xC0047010|-1073450992|DTS_E_CANTGETSEMAPHORE|Un thread n'a pas pu créer un sémaphore lors de l'initialisation.|  
|0xC0047011|-1073450991|DTS_E_BUFFERFAILUREDETAILS|Le système rapporte %1!d! % de mémoire. Il y a %2 octets de mémoire physique et %3 octets libres. Il y a %4 octets de mémoire virtuelle et %5 octets libres. Le fichier d'échange a %6 octets et %7 octets libres.|  
|0xC0047012|-1073450990|DTS_E_BUFFERALLOCFAILED|Échec d'un tampon lors de l'allocation de %1!d! octets.|  
|0xC0047013|-1073450989|DTS_E_CANTCREATEBUFFERMANAGER|Impossible de créer le gestionnaire de tampons.|  
|0xC0047015|-1073450987|DTS_E_BUFFERBADSIZE|La taille du type de tampon %1!d! est de %2!I64d! octets.|  
|0xC0047016|-1073450986|DTS_E_DANGLINGWITHPATH|%1 est marqué comme non résolu, mais un chemin d'accès lui est associé.|  
|0xC0047017|-1073450985|DTS_E_INDIVIDUALVALIDATIONFAILED|Échec de la validation de %1. Code d'erreur : 0x%2!8.8X!.|  
|0xC0047018|-1073450984|DTS_E_INDIVIDUALPOSTEXECUTEFAILED|Échec de la phase de post-exécution de %1. Code d'erreur : 0x%2!8.8X!.|  
|0xC0047019|-1073450983|DTS_E_INDIVIDUALPREPAREFAILED|Échec de la phase de préparation de %1. Code d'erreur : 0x%2!8.8X!.|  
|0xC004701A|-1073450982|DTS_E_INDIVIDUALPREEXECUTEFAILED|Échec de la phase de pré-exécution de %1. Code d'erreur : 0x%2!8.8X!.|  
|0xC004701B|-1073450981|DTS_E_INDIVIDUALCLEANUPFAILED|Échec de la phase de nettoyage de %1. Code d'erreur : 0x%2!8.8X!.|  
|0xC004701C|-1073450980|DTS_E_INVALIDINPUTLINEAGEID|%1 a l'ID de lignage ID %2!d! non utilisé précédemment dans la tâche de flux de données.|  
|0xC004701E|-1073450978|DTS_E_EXECUTIONTREECYCLE|Impossible de connecter %1 à %2 car un cycle serait créé.|  
|0xC004701F|-1073450977|DTS_E_CANTCOMPARE|Impossible de comparer le type de données « %1 ». La comparaison de ce type de données n'est pas prise en charge. Par conséquent, il ne peut pas être trié ou utilisé en tant que clé.|  
|0xC0047020|-1073450976|DTS_E_REFUSEDFORSHUTDOWN|Ce thread s'est arrêté et n'accepte pas les tampons d'entrée.|  
|0xC0047021|-1073450975|DTS_E_THREADFAILED|Code d'erreur SSIS DTS_E_THREADFAILED.  Le thread « %1 » n'est plus exécuté. Code d'erreur : 0x%2!8.8X!.  Des messages d'erreur peuvent être envoyés au préalable avec des informations indiquant la raison de la non-exécution du thread.|  
|0xC0047022|-1073450974|DTS_E_PROCESSINPUTFAILED|Code d'erreur SSIS DTS_E_PROCESSINPUTFAILED.  La méthode ProcessInput du composant« %1 » (%2!d!) a échoué avec le code d'erreur 0x%3!8.8X! pendant le traitement de l'entrée « %4 » (%5!d!). Le composant identifié a retourné une erreur de la méthode ProcessInput. Cette erreur, spécifique au composant, est irrécupérable et provoquera l'arrêt de la tâche de flux de données.  Des messages d'erreur peuvent être envoyés au préalable avec des informations indiquant la raison de l'échec.|  
|0xC0047023|-1073450973|DTS_E_CANTREALIZEVIRTUALBUFFERS|Impossible de réaliser un jeu de tampons virtuels.|  
|0xC0047024|-1073450972|DTS_E_PIPELINETOOCOMPLEX|Le nombre de threads requis pour ce pipeline est de %1!d!. Il excède la limite système de %2!d!. Le pipeline nécessite trop de threads par rapport à la configuration. Il y a trop de sorties asynchrones ou la valeur de la propriété EngineThreads est trop élevée. Divisez le pipeline en plusieurs packages ou réduisez la valeur de la propriété EngineThreads.|  
|0xC0047028|-1073450968|DTS_E_SCHEDULERCOULDNOTCOUNTSOURCES|Le planificateur du moteur de flux de données ne peut pas obtenir le nombre de sources dans la structure.|  
|0xC0047029|-1073450967|DTS_E_SCHEDULERCOULDNOTCOUNTDESTINATIONS|Le planificateur du moteur de flux de données ne peut pas obtenir le nombre de destinations dans la structure.|  
|0xC004702A|-1073450966|DTS_E_COMPONENTVIEWISUNAVAILABLE|La vue du composant n'est pas disponible. Vérifiez qu'elle a été créée.|  
|0xC004702B|-1073450965|DTS_E_INCORRECTCOMPONENTVIEWID|L'ID de la vue du composant est incorrect. Elle n'est peut-être pas synchronisée. Essayez de libérer la vue du composant et de la recréer.|  
|0xC004702C|-1073450964|DTS_E_BUFFERNOTLOCKED|Ce tampon n'est pas verrouillé et ne peut pas être manipulé.|  
|0xC004702D|-1073450963|DTS_E_CANTBUILDBUFFERTYPE|La tâche de flux de données ne peut pas allouer de mémoire pour créer une définition de tampon. La définition du tampon comporte %1!d! colonnes.|  
|0xC004702E|-1073450962|DTS_E_CANTREGISTERBUFFERTYPE|La tâche de flux de données ne peut pas enregistrer un type de tampon. Le type contient %1!d! colonnes et concerne l'arborescence d'exécution %2!d!.|  
|0xC004702F|-1073450961|DTS_E_INVALIDUSESDISPOSITIONSVALUE|Impossible de modifier la valeur d'origine de la propriété UsesDispositions. Ce problème se produit lorsque le document XML est modifié ainsi que la valeur UsesDispositions. Cette valeur est définie par le composant lorsqu'elle est ajoutée au package et ne peut pas être modifiée.|  
|0xC0047030|-1073450960|DTS_E_THREADFAILEDINITIALIZE|La tâche de flux de données n'a pas pu initialiser un thread obligatoire et ne peut pas commencer à être exécutée. Ce thread a précédemment retourné une erreur.|  
|0xC0047031|-1073450959|DTS_E_THREADFAILEDCREATE|La tâche de flux de données n'a pas pu créer un thread obligatoire et ne peut pas s'exécuter. Cette erreur se produit généralement lorsque la mémoire est insuffisante.|  
|0xC0047032|-1073450958|DTS_E_EXECUTIONTREECYCLEADDINGSYNCHRONOUSINPUT|Impossible de définir l'entrée synchrone « %1 » sur « %2 » car un cycle serait créé.|  
|0xC0047033|-1073450957|DTS_E_INVALIDCUSTOMPROPERTYNAME|La propriété personnalisée « %1 » n'est pas valide car une propriété Stock porte ce nom. Une propriété personnalisée ne peut pas avoir le même nom qu'une propriété stock sur le même objet.|  
|0xC0047035|-1073450955|DTS_E_BUFFERLOCKUNDERFLOW|Le tampon est déjà déverrouillé.|  
|0xC0047036|-1073450954|DTS_E_INDIVIDUALCACHEINTERFACESFAILED|Échec de l'initialisation de %1. Code d'erreur retourné : 0x%2!8.8X!.|  
|0xC0047037|-1073450953|DTS_E_INDIVIDUALRELEASEINTERFACESFAILED|Échec de l'arrêt de %1. Code d'erreur retourné : 0x%2!8.8X!. Un composant n'a pas pu libérer ses interfaces.|  
|0xC0047038|-1073450952|DTS_E_PRIMEOUTPUTFAILED|Code d'erreur SSIS DTS_E_PRIMEOUTPUTFAILED.  La méthode PrimeOutput sur %1 a retourné le code d'erreur 0x%2!8.8X!.  Le composant a retourné un code d'erreur lorsque le moteur du pipeline a appelé PrimeOutput(). La signification du code d'erreur est définie par le composant. Cependant, l'erreur est irrécupérable et le pipeline ne s'exécute plus.  Des messages d'erreur peuvent être envoyés au préalable avec des informations indiquant la raison de l'échec.|  
|0xC0047039|-1073450951|DTS_E_THREADCANCELLED|Code d'erreur SSIS DTS_E_THREADCANCELLED.  Le thread « %1 » a reçu un signal d'arrêt et est en cours d'arrêt. L'utilisateur a demandé l'arrêt ou une erreur dans un autre thread provoque l'arrêt du pipeline.  Des messages d'erreur peuvent être envoyés au préalable avec des informations indiquant la raison de l'annulation du thread.|  
|0xC004703A|-1073450950|DTS_E_DISTRIBUTORCANTSETPROPERTY|Le serveur de distribution du thread « %1 » n'a pas pu initialiser la propriété « %2 » dans le composant « %3 » en raison de l'erreur 0x%8.8X. Le serveur de distribution n'a pas pu initialiser la propriété du composant et son exécution ne peut pas se poursuivre.|  
|0xC004703B|-1073450949|DTS_E_CANTREGISTERVIEWBUFFERTYPE|La tâche de flux de données ne peut pas enregistrer un type de tampon d'affichage. Le type contient %1!d! colonnes et concerne l'ID d'entrée %2!d!.|  
|0xC004703F|-1073450945|DTS_E_CANTCREATEEXECUTIONTREE|Mémoire insuffisante pour créer une arborescence d'exécution.|  
|0xC0047040|-1073450944|DTS_E_CANTINSERTINTOHASHTABLE|Mémoire insuffisante pour insérer un objet dans la table de hachage.|  
|0xC0047041|-1073450943|DTS_E_OBJECTNOTINHASHTABLE|La table de hachage ne contient pas l'objet.|  
|0xC0047043|-1073450941|DTS_E_CANTCREATECOMPONENTVIEW|Impossible de créer une vue du composant car une autre vue existe déjà. Une seule vue du composant peut exister à la fois.|  
|0xC0047046|-1073450938|DTS_E_LAYOUTCANTSETUSAGETYPE|Dans l'entrée « %1 » (%2!d!), la collection de colonnes d'entrée virtuelles ne contient aucune colonne d'entrée virtuelle avec l'ID de lignage %3!d!.|  
|0xC0047047|-1073450937|DTS_E_WRONGOBJECTTYPE|Le type de l'objet demandé est incorrect.|  
|0xC0047048|-1073450936|DTS_E_CANTCREATESPOOLFILE|Le gestionnaire de tampons ne peut pas créer un fichier de stockage temporaire dans un chemin d'accès de la propriété BufferTempStoragePath. Le nom du fichier est incorrect, aucune autorisation n'est définie ou les chemins d'accès sont complets.|  
|0xC0047049|-1073450935|DTS_E_SEEKFAILED|Le gestionnaire de tampons n'a pas trouvé le décalage %1!d! dans le fichier « %2 ". Le fichier est endommagé.|  
|0xC004704A|-1073450934|DTS_E_EXTENDFAILED|Le gestionnaire de tampons ne peut pas étendre la longueur du fichier « %1 » à %2!lu! octets.  L'espace disque est insuffisant.|  
|0xC004704B|-1073450933|DTS_E_FILEWRITEFAILED|Le gestionnaire de tampons ne peut pas écrire %1!d! octets dans le fichier « %2 ». L'espace disque ou le quota de disque est insuffisant.|  
|0xC004704C|-1073450932|DTS_E_FILEREADFAILED|Le gestionnaire de tampons ne peut pas lire %1!d! octets dans le fichier « %2 ». Le fichier est endommagé.|  
|0xC004704D|-1073450931|DTS_E_VIRTUALNOTSEQUENTIAL|L'ID de tampon %1!d! prend en charge d'autres tampons virtuels et ne peut pas être placé en mode séquentiel. IDTSBuffer100.SetSequentialMode a été appelé sur un tampon qui prend en charge les tampons virtuels.|  
|0xC004704E|-1073450930|DTS_E_BUFFERISREADONLY|Impossible d'effectuer cette opération car le tampon est en lecture seule. Un tampon en lecture seule ne peut pas être modifié.|  
|0xC004704F|-1073450929|DTS_E_EXECUTIONTREECYCLESETTINGID|Impossible de définir l'ID %1 sur %2!d! car un cycle serait créé.|  
|0xC0047050|-1073450928|DTS_E_NOMOREBUFFERTYPES|Mémoire insuffisante du gestionnaire de tampons lors de l'extension de la table de types de tampon. Cette erreur est due à une condition de mémoire insuffisante.|  
|0xC0047051|-1073450927|DTS_E_CANTCREATENEWTYPE|Le gestionnaire de tampons n'a pas pu créer un nouveau type de tampon.|  
|0xC0047053|-1073450925|DTS_E_SCHEDULERBADTREE|Le planificateur du moteur de flux de données n'a pas pu récupérer l'arborescence d'exécution avec l'index %1!d! dans la structure. Le planificateur a reçu un nombre d'arborescences d'exécution supérieur au nombre réel.|  
|0xC0047056|-1073450922|DTS_E_CANTCREATEPRIMEOUTPUTBUFFER|La tâche de flux de données n'a pas pu créer un tampon pour appeler PrimeOutput pour la sortie « %3 » (%4!d!) dans le composant « %1 » (%2!d!). Cette erreur se produit généralement en raison d'une condition de mémoire insuffisante.|  
|0xC0047057|-1073450921|DTS_E_SCHEDULERTHREADMEMORY|Le planificateur du moteur de flux de données n'a pas pu créer un objet thread car la mémoire est insuffisante. Cette erreur est due à une condition de mémoire insuffisante.|  
|0xC004705A|-1073450918|DTS_E_SCHEDULEROBJECT|Le planificateur du moteur de flux de données ne peut pas récupérer l'objet avec l'ID %1!d! dans la structure. Ce planificateur a précédemment localisé un objet qui n'est plus disponible.|  
|0xC004705B|-1073450917|DTS_E_PREPARETREENODEFAILED|La tâche de flux de données n'a pas pu préparer les tampons pour le nœud de l'arborescence d'exécution commençant à la sortie « %1 » (%2!d!).|  
|0xC004705C|-1073450916|DTS_E_CANTCREATEVIRTUALBUFFER|La tâche de flux de données ne peut pas créer un tampon virtuel pour préparer l'exécution.|  
|0xC004705E|-1073450914|DTS_E_NOMOREIDS|Le nombre maximal d'ID a été atteint. Il n'y a plus d'ID disponibles à affecter aux objets.|  
|0xC004705F|-1073450913|DTS_E_ALREADYATTACHED|%1 est déjà attaché et ne peut plus être attaché.  Détachez-le, puis réessayez.|  
|0xC0047060|-1073450912|DTS_E_OUTPUTCOLUMNNAMECONFLICT|Impossible d'utiliser le nom de colonne « %1 » dans la sortie « %2 », car il est en conflit avec une colonne du même nom dans l'entrée synchrone « %3 ».|  
|0xC0047061|-1073450911|DTS_E_EOFANNOUNCEMENTFAILED|La tâche de flux de données ne peut pas créer un tampon pour marquer la fin de l'ensemble de lignes.|  
|0xC0047062|-1073450910|DTS_E_USERCOMPONENTEXCEPTION|Un composant utilisateur géré a levé l'exception « %1 ».|  
|0xC0047063|-1073450909|DTS_E_SCHEDULERMEMORY|Le planificateur du moteur de flux de données ne peut pas allouer suffisamment de mémoire pour les structures d'exécution. La mémoire système était insuffisante avant le début de l'exécution.|  
|0xC0047064|-1073450908|DTS_E_BUFFERNOOBJECTMEMORY|Une condition de mémoire insuffisante a empêché la création de l'objet Buffer.|  
|0xC0047065|-1073450907|DTS_E_BUFFERNOMAPMEMORY|Une condition de mémoire insuffisante empêche le mappage des ID de lignage d'un tampon dans les index DTP_HCOL.|  
|0xC0047066|-1073450906|DTS_E_INDIVIDUALPUTVARIABLESFAILED|« %1!s! » ne peut pas mettre en cache la collection Variables et a retourné le code d’erreur 0x%2!8.8X.|  
|0xC0047067|-1073450905|DTS_E_INDIVIDUALPUTCOMPONENTMETADATAFAILED|« %1 » n'a pas pu mettre en cache l'objet Metadata du composant et a retourné le code d'erreur 0x%2!8.8X!.|  
|0xC0047068|-1073450904|DTS_E_SORTEDOUTPUTHASINVALIDSORTKEYPOSITION|La valeur SortKeyPosition de « %1 » n'est pas NULL, mais trop grande (%2!ld!). Elle doit être inférieure ou égale au nombre de colonnes.|  
|0xC004706A|-1073450902|DTS_E_SORTEDOUTPUTHASINVALIDSORTKEYPOSITIONS|La propriété IsSorted de %1 a la valeur TRUE, mais les valeurs absolues de la colonne de sortie non NULL SortKeyPositions ne forment pas une séquence à croissance monolithique partant de 1.|  
|0xC004706B|-1073450901|DTS_E_INDIVIDUALVALIDATIONSTATUSFAILED|Échec de la validation de « %1 ». État de validation retourné : « %2 ».|  
|0xC004706C|-1073450900|DTS_E_CANTCREATECOMPONENT|Impossible de créer le composant « %1!s! ». Code d’erreur retourné : 0x%2!8.8X! « %3!s! ». Vérifiez que le composant est correctement inscrit dans le Registre.|  
|0xC004706D|-1073450899|DTS_E_COMPONENTNOTREGISTERED|Le module contenant « %1 » n'est pas inscrit ou n'est pas installé correctement.|  
|0xC004706E|-1073450898|DTS_E_COMPONENTNOTFOUND|Le module contenant « %1 » est introuvable, bien qu'il soit inscrit.|  
|0xC004706F|-1073450897|DTS_E_BINARYCODENOTFOUND|Le composant de script est configuré pour précompiler le script, mais le code binaire est introuvable. Visitez l'IDE dans l'éditeur de composant de script en cliquant sur le bouton Créer un script afin de générer le code binaire.|  
|0xC0047070|-1073450896|DTS_E_CANTCREATEBLOBFILE|Le gestionnaire de tampons ne peut pas créer un fichier pour spouler un objet long dans les répertoires nommés dans la propriété BLOBTempStoragePath.  Le nom de fichier spécifié est incorrect, aucune autorisation n'est définie ou les chemins d'accès sont complets.|  
|0xC0047071|-1073450895|DTS_E_SYNCHRONOUSIDMISMATCH|La valeur de la propriété SynchronousInputID sur « %1 » est %2!d!, or %3!d! était attendu.|  
|0xC0047072|-1073450894|DTS_E_OBJECTIDNOTFOUND|Il n'existe aucun objet avec l'ID %1!d!.|  
|0xC0047073|-1073450893|DTS_E_OBJECTIDLOOKUPFAILED|Impossible de localiser un objet avec l'ID %1!d! en raison du code d'erreur 0x%2!8.8X!.|  
|0xC0047074|-1073450892|DTS_E_INVALIDCODEPAGE|La page de codes %1!d! spécifiée dans la colonne de sortie « %2 » (%3!d!) n'est pas valide. Sélectionnez une autre page de codes pour la colonne de sortie « %2 ».|  
|0xC0047075|-1073450891|DTS_E_INDIVIDUALPUTEVENTINFOSFAILED|« %1 » n'a pas pu mettre en cache la collection EventInfos. Code d'erreur retourné : 0x%2!8.8X!.|  
|0xC0047077|-1073450889|DTS_E_DUPLICATEOUTPUTCOLUMNNAMES|Le nom de « %1 » est un doublon.  Les noms doivent être uniques.|  
|0xC0047078|-1073450888|DTS_E_NOOUTPUTCOLUMNFORINPUTCOLUMN|Aucune colonne de sortie n'est associée à la colonne d'entrée « %1 » (%2!d!).|  
|0xC0047079|-1073450887|DTS_E_EXCLGRPNOSYNCINP|« %1 » possède un tampon virtuel qui s'étend à partir d'une source racine. Il y a un groupe d'exclusions non NULL avec une entrée synchrone de type NULL.|  
|0xC004707A|-1073450886|DTS_E_ERROROUTCANTBEONSYNCNONEXCLUSIVEOUTPUT|« %1 » ne peut pas être une sortie d'erreur car les sorties d'erreur ne peuvent pas être placées sur des sorties synchrones non exclusives.|  
|0xC004707B|-1073450885|DTS_E_EXPREVALDIVBYZERO|Une erreur de division par zéro s'est produite. L'opérande droit a la valeur zéro dans l'expression « %1 ».|  
|0xC004707C|-1073450884|DTS_E_EXPREVALLITERALOVERFLOW|Le littéral « %1 » est trop grand pour le type %2. La grandeur du littéral dépasse le type.|  
|0xC004707D|-1073450883|DTS_E_EXPREVALBINARYOPNUMERICOVERFLOW|Le résultat de l'opération binaire « %1 » sur les types de données %2 et %3 excède la taille maximale autorisée pour les types numériques. Impossible d'effectuer une conversion implicite des types d'opérandes en résultat numérique (DT_NUMERIC) sans perte de précision ou d'échelle. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC004707E|-1073450882|DTS_E_EXPREVALBINARYOPOVERFLOW|Le résultat de l'opération binaire « %1 » excède la taille maximale pour le type de données de résultat « %2 ». La grandeur du résultat de l'opération dépasse les valeurs admises pour le type de résultat.|  
|0xC004707F|-1073450881|DTS_E_EXPREVALFUNCTIONOVERFLOW|Le résultat de l'appel de fonction « %1 » est trop grand pour le type « %2 ». La grandeur du résultat de l'appel de fonction dépasse le type de l'opérande. Une conversion explicite en un type plus grand peut être nécessaire.|  
|0xC0047080|-1073450880|DTS_E_EXPREVALBINARYTYPEMISMATCH|Les types de données« %1 » et « %2 » sont incompatibles pour l'opérateur binaire « %3 ». Impossible d'effectuer une conversion implicite des types d'opérandes en types compatibles pour l'opération. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC0047081|-1073450879|DTS_E_EXPREVALUNSUPPORTEDBINARYTYPE|Impossible d'utiliser le type de données « %1 » avec l'opérateur binaire « %2 ». Le type de l'un des opérandes ou des deux n'est pas pris en charge pour l'opération. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC0047082|-1073450878|DTS_E_EXPREVALBINARYSIGNMISMATCH|Incompatibilité de signe pour l'opérateur binaire « %1 » au niveau du bit dans l'opération « %2 ». Les deux opérandes de cet opérateur doivent être positifs ou négatifs.|  
|0xC0047083|-1073450877|DTS_E_EXPREVALBINARYOPERATIONFAILED|Échec de l'opération binaire « %1 ». Code d'erreur : 0x%2!8.8X!. Une erreur interne s'est produite ou une condition de mémoire insuffisante existe.|  
|0xC0047084|-1073450876|DTS_E_EXPREVALBINARYOPERATIONSETTYPEFAILED|Échec de la définition du type de résultat de l'opération binaire « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC0047085|-1073450875|DTS_E_EXPREVALSTRINGCOMPARISONFAILED|Échec de la comparaison de « %1 » à la chaîne « %2 ».|  
|0xC0047086|-1073450874|DTS_E_EXPREVALUNSUPPORTEDUNNARYTYPE|Impossible d'utiliser le type de données « %1 » avec l'opérateur unaire « %2 ». Ce type d'opérande n'est pas pris en charge pour l'opération. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC0047087|-1073450873|DTS_E_EXPREVALUNARYOPERATIONFAILED|Échec de l'opération unaire « %1 ». Code d'erreur : 0x%2!8.8X!. Une erreur interne s'est produite, ou une condition de mémoire insuffisante existe.|  
|0xC0047088|-1073450872|DTS_E_EXPREVALUNARYOPERATIONSETTYPEFAILED|Échec de la définition du type de résultat de l'opération unaire « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC0047089|-1073450871|DTS_E_EXPREVALPARAMTYPEMISMATCH|La fonction « %1 » ne prend pas en charge le type de données « %2 » pour le paramètre %3!d!. Impossible d'effectuer une conversion explicite du type de paramètre en un type compatible pour la fonction. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC004708A|-1073450870|DTS_E_EXPREVALINVALIDFUNCTION|La fonction « %1 » n'est pas reconnue. Le nom de la fonction n'est pas correct ou n'existe pas.|  
|0xC004708B|-1073450869|DTS_E_EXPREVALFNSUBSTRINGINVALIDLENGTH|La longueur %1!d! n'est pas valide pour la fonction « %2 ». Ce paramètre ne peut pas être négatif. Modifiez la valeur du paramètre de longueur en zéro ou en valeur positive.|  
|0xC004708C|-1073450868|DTS_E_EXPREVALFNSUBSTRINGINVALIDSTARTINDEX|L'index de début %1!d! n'est pas valide pour la fonction « %2 ». La valeur de l'index de début doit être un entier supérieur à zéro. L'index de début est une valeur de base un, et non pas une valeur de base zéro.|  
|0xC004708E|-1073450866|DTS_E_EXPREVALCHARMAPPINGFAILED|La fonction « %1 » ne peut pas mapper les caractères sur la chaîne « %2 ».|  
|0xC004708F|-1073450865|DTS_E_EXPREVALINVALIDDATEPART|« %1 » n'est pas un élément de date valide pour la fonction « %2 ».|  
|0xC0047090|-1073450864|DTS_E_EXPREVALINVALIDNULLPARAM|Le paramètre %1!d! de la fonction NULL avec le type de données « %2 » n'est pas valide. Les paramètres de NULL() doivent être statiques et ne peuvent pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0047091|-1073450863|DTS_E_EXPREVALINVALIDNULLPARAMTYPE|Le paramètre %1!d! de la fonction NULL avec le type de données « %2 » n'est pas un entier. Un paramètre de NULL() doit être un entier ou un type qui peut être converti en un entier.|  
|0xC0047092|-1073450862|DTS_E_EXPREVALFUNCTIONPARAMNOTSTATIC|Le paramètre %1!d! de la fonction « %2 » n'est pas statique. Ce paramètre doit être statique, et il ne peut pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0047093|-1073450861|DTS_E_EXPREVALINVALIDCASTPARAM|Le paramètre %1!d! de la conversion en type de données « %2 » n'est pas valide. Les paramètres des opérateurs de conversion doivent être statiques et ne peuvent pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0047094|-1073450860|DTS_E_EXPREVALINVALIDCASTPARAMTYPE|Le paramètre %1!d! de la conversion en type de données « %2 » n'est pas un entier. Un paramètre d'un opérateur de conversion doit être un entier ou un type qui peut être converti en un entier.|  
|0xC0047095|-1073450859|DTS_E_EXPREVALINVALIDCAST|Impossible de convertir l'expression « %1 » du type de données « %2 » en type de données « %3 ». La conversion requise n'est pas prise en charge.|  
|0xC0047096|-1073450858|DTS_E_EXPREVALINVALIDTOKEN|Échec de l'analyse de l'expression « %1 ».  Le jeton « %2 » à la ligne « %3 », numéro de caractère « %4 » n'est pas reconnu. Impossible d'analyser l'expression car elle contient des éléments non valides à l'emplacement spécifié.|  
|0xC0047097|-1073450857|DTS_E_EXPREVALUNEXPECTEDPARSEERROR|Une erreur s'est produite lors de l'analyse de l'expression « %1 ». Échec de l'analyse de l'expression pour une raison inconnue.|  
|0xC0047098|-1073450856|DTS_E_EXPREVALFAILEDTOPARSEEXPRESSIONWITHHR|Échec de l'analyse de l'expression « %1 ». Code d'erreur : 0x%2!8.8X!. Impossible d'analyser l'expression. Elle contient peut-être des éléments non valides ou n'est peut-être pas bien formée. Cela peut aussi se produire en raison d'une erreur de mémoire insuffisante.|  
|0xC0047099|-1073450855|DTS_E_EXPREVALFAILEDTOPARSEEXPRESSION|L'expression « %1 » n'est pas valide et ne peut pas être analysée. L'expression contient peut-être des éléments non valides ou n'est peut-être pas bien formée.|  
|0xC004709A|-1073450854|DTS_E_EXPREVALEXPRESSIONEMPTY|Aucune expression à calculer. Tentative de calcul ou d'obtention de la chaîne d'une expression vide.|  
|0xC004709B|-1073450853|DTS_E_EXPREVALCOMPUTEFAILED|Échec du calcul de l'expression « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC004709C|-1073450852|DTS_E_EXPREVALBUILDSTRINGFAILED|Échec de la génération d'une représentation de chaîne de l'expression. Code d'erreur : 0x%2!8.8X!. Échec lors de la création d'une chaîne qui peut être affichée représentant l'expression.|  
|0xC004709D|-1073450851|DTS_E_EXPREVALCANNOTCONVERTRESULT|Impossible de convertir le type de données du résultat de l'expression « % » en type de données de colonne « %2 ». Le résultat de l'expression doit être écrit dans une colonne d'entrée/sortie, mais le type de données de l'expression ne peut pas être converti en type de données de la colonne.|  
|0xC004709E|-1073450850|DTS_E_EXPREVALCONDITIONALOPINVALIDCONDITIONTYPE|L'expression conditionnelle « %1 » de l'opérateur conditionnel possède un type de données non valide de « %2 ». L'expression conditionnelle de l'opérateur conditionnel doit retourner une valeur booléenne de type DT_BOOL.|  
|0xC004709F|-1073450849|DTS_E_EXPREVALCONDITIONALOPTYPEMISMATCH|Les types de données « %1 » et « %2 » sont incompatibles pour l'opérateur conditionnel. Impossible d'effectuer une conversion implicite des types d'opérandes en types compatibles pour l'opération conditionnelle. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC00470A0|-1073450848|DTS_E_EXPREVALCONDITIONALOPSETTYPEFAILED|Échec de la définition du type de résultat de l'opération conditionnelle « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC00470A1|-1073450847|DTS_E_BUFFERORPHANED|Ce tampon est orphelin. Le gestionnaire de tampons s'est arrêté, laissant un tampon en cours d'utilisation. Le tampon ne sera pas nettoyé. Il est possible qu'il y ait des fuites de mémoire et d'autres problèmes.|  
|0xC00470A2|-1073450846|DTS_E_EXPREVALINPUTCOLUMNNAMENOTFOUND|Échec de la recherche de la colonne d'entrée « %1 ». Code d'erreur : 0x%2!8.8X!. La colonne d'entrée spécifiée est introuvable dans la collection de colonnes d'entrée.|  
|0xC00470A3|-1073450845|DTS_E_EXPREVALINPUTCOLUMNIDNOTFOUND|Échec de la recherche de la colonne d'entrée avec l'ID de lignage %1!d!. Code d’erreur : 0x%2!8.8X!. La colonne d'entrée est introuvable dans la collection de colonnes d'entrée.|  
|0xC00470A4|-1073450844|DTS_E_EXPREVALNOINPUTCOLUMNCOLLECTIONFORCOLUMNNAME|L'expression contient le jeton non reconnu « %1 ». Si « %1 » est une variable, elle doit être exprimée sous la forme « @%1 ». Le jeton spécifié n'est pas valide. S'il représente un nom de variable, il doit comporter le symbole @ en préfixe.|  
|0xC00470A5|-1073450843|DTS_E_EXPREVALNOINPUTCOLUMNCOLLECTIONFORCOLUMNID|L'expression contient le jeton non reconnu « #%1!d! ».|  
|0xC00470A6|-1073450842|DTS_E_EXPREVALVARIABLENOTFOUND|La variable « %1 » est introuvable dans la collection Variables. Cette variable n'existe peut-être pas dans l'étendue correcte.|  
|0xC00470A7|-1073450841|DTS_E_EXPREVALINVALIDTOKENSTATE|Échec de l'analyse de l'expression « %1 ». L'expression contient peut-être un jeton non valide, un jeton incomplet ou un élément non valide. Peut-être qu'elle n'est pas formée de manière appropriée ou qu'il manque un élément obligatoire tel qu'une parenthèse.|  
|0xC00470A8|-1073450840|DTS_E_BLANKOUTPUTCOLUMNNAME|Le nom de « %1 » est vide, or les noms ne peuvent pas être vides.|  
|0xC00470A9|-1073450839|DTS_E_HASSIDEEFFECTSWITHSYNCINP|La propriété HasSideEffects de « %1 » a la valeur TRUE, mais « %1 » est synchrone et ne peut pas avoir d'effets secondaires. Affectez la valeur FALSE à la propriété HasSideEffects.|  
|0xC00470AA|-1073450838|DTS_E_EXPREVALINVALIDCASTCODEPAGE|La valeur %1!d! spécifiée pour le paramètre de page de codes de la conversion en type de données « %2 » n'est pas valide. La page de codes n'est pas installée sur l'ordinateur.|  
|0xC00470AB|-1073450837|DTS_E_EXPREVALINVALIDCASTPRECISION|La valeur %1!d! spécifiée pour le paramètre de précision de la conversion en type de données « %2 » n'est pas valide. La précision doit être comprise entre %3!d! et %4!d! et est hors limites pour la conversion de type.|  
|0xC00470AC|-1073450836|DTS_E_EXPREVALINVALIDCASTSCALE|La valeur %1!d! spécifiée pour le paramètre d'échelle de la conversion en type de données « %2 » n'est pas valide. L’échelle doit être comprise entre %3!d! et %4!d! et elle est hors limites pour la conversion du type. L'échelle ne doit pas dépasser la précision et doit être positive.|  
|0xC00470AD|-1073450835|DTS_E_NONSORTEDOUTPUTHASSORTKEYPOSITIONS|La propriété IsSorted de « %1 » a la valeur FALSE, mais %2!lu! des SortKeyPositions de ses colonnes de sortie sont différentes de zéro.|  
|0xC00470AF|-1073450833|DTS_E_EXPREVALCONDITIONALOPCODEPAGEMISMATCH|Les pages de codes doivent correspondre aux opérandes d'opération conditionnelle « %1 » pour le type %2. La page de codes de l'opérande gauche ne correspond pas à la page de codes de l'opérande droit. Les pages de codes doivent être identiques pour l'opérateur conditionnel du type spécifié.|  
|0xC00470B1|-1073450831|DTS_E_REFERENCEDMETADATABADCOUNT|L'entrée « %1 » (%2!d!) référence l'entrée « %3 » (%4!d!), mais le nombre de colonnes est différent. L'entrée %5!d! comporte %6!d! colonnes, tandis que l'entrée %7!d! comporte %8!d! colonnes.|  
|0xC00470B2|-1073450830|DTS_E_OBJECTLINEAGEIDNOTFOUND|Il n'existe aucun objet avec l'ID de lignage %1!d!.|  
|0xC00470B3|-1073450829|DTS_E_FILENAMEOUTPUTCOLUMNOTFOUND|La colonne de sortie pour le nom de fichier est introuvable.|  
|0xC00470B4|-1073450828|DTS_E_FILENAMEOUTPUTCOLUMNINVALIDDATATYPE|La colonne de sortie pour le nom de fichier n'est pas une chaîne de caractères Unicode terminée par un caractère nul, de type de données DT_WSTR.|  
|0xC00470B5|-1073450827|DTS_E_DISTRIBUTORADDFAILED|Un serveur de distribution n'a pas pu donner un tampon au thread « %1 » en raison de l'erreur 0x%2!8.8X!. Le thread cible est probablement en cours d'arrêt.|  
|0xC00470B6|-1073450826|DTS_E_LOCALENOTINSTALLED|LocaleID %1!ld! n'est pas installé sur cet ordinateur.|  
|0xC00470B7|-1073450825|DTS_E_EXPREVALILLEGALHEXESCAPEINSTRINGLITERAL|Le littéral de chaîne « %1 » contient une séquence d'échappement hexadécimale non conforme de « \x%2 ». La séquence d'échappement n'est pas prise en charge dans les littéraux de chaîne de l'évaluateur d'expression. Les séquences d'échappement hexadécimales doivent être au format \xhhhh où h est un chiffre hexadécimal valide.|  
|0xC00470B8|-1073450824|DTS_E_EXPREVALILLEGALESCAPEINSTRINGLITERAL|Le littéral de chaîne « %1 » contient une séquence d’échappement non conforme de «\\%2!c! ». La séquence d'échappement n'est pas prise en charge dans les littéraux de chaîne de l'évaluateur d'expression. Si la chaîne nécessite une barre oblique inverse, utilisez une double barre oblique inverse, «\\\\».|  
|0xC00470B9|-1073450823|DTS_E_NOOUTPUTCOLUMNS|« %1 » ne contient aucune colonne de sortie. Une sortie asynchrone doit contenir des colonnes de sortie.|  
|0xC00470BA|-1073450822|DTS_E_LOBDATATYPENOTSUPPORTED|« %1 » possède un type de données d'objet Long dans DT_TEXT, DT_NTEXT ou DT_IMAGE, lequel n'est pas pris en charge.|  
|0xC00470BB|-1073450821|DTS_E_OUTPUTWITHMULTIPLEERRORS|Échec de l'ID de sortie %1!d! ont été fournies à l'ID de sortie %1!d! : en premier, %2!d! et %3!d!, puis %4!d! et %5!d!.|  
|0xC00470BC|-1073450820|DTS_E_FAILEDDURINGOLEDBDATATYPECONVERSIONCHECK|Échec du fournisseur OLE DB lors de la vérification de la conversion du type de données de « %1 ».|  
|0xC00470BD|-1073450819|DTS_E_BUFFERISEOR|Ce tampon représente la fin de l'ensemble de lignes et le nombre de lignes ne peut pas être modifié.  Tentative d'appel d'AddRow ou de RemoveRow sur un tampon qui comporte un indicateur de fin d'ensemble de lignes.|  
|0xC00470BE|-1073450818|DTS_E_EXPREVALUNSUPPORTEDTYPE|Le type de données « %1 » n'est pas pris en charge dans une expression. Le type spécifié n'est pas pris en charge ou n'est pas valide.|  
|0xC00470BF|-1073450817|DTS_E_PRIMEOUTPUTNOEOR|La méthode PrimeOutput a été correctement appliquée sur « %1 », mais elle n'a pas signalé la fin de l'ensemble de lignes. Il existe une erreur dans le composant, Il existe une erreur dans le composant, car une fin de ligne aurait dû être rapportée. Le pipeline arrêtera l'exécution afin d'éviter des résultats imprévisibles.|  
|0xC00470C0|-1073450816|DTS_E_EXPREVALDATACONVERSIONOVERFLOW|Un dépassement s'est produit lors de la conversion du type de données « %1 » en « %2 ». Le type de source est trop grand pour le type de destination.|  
|0xC00470C1|-1073450815|DTS_E_EXPREVALDATACONVERSIONNOTSUPPORTED|La conversion du type de données « %1 » en « %2 » n'est pas prise en charge. Impossible de convertir le type de source en type de destination.|  
|0xC00470C2|-1073450814|DTS_E_EXPREVALDATACONVERSIONFAILED|L'erreur 0x%1!8.8X! s'est produite lors de la conversion du type de données %2 en %3.|  
|0xC00470C3|-1073450813|DTS_E_EXPREVALCONDITIONALOPERATIONFAILED|Échec de l'opération conditionnelle « %1 ». Code d'erreur : 0x%2!8.8X!. Une erreur interne ou de mémoire insuffisante s'est produite.|  
|0xC00470C4|-1073450812|DTS_E_EXPREVALCASTFAILED|Échec de la conversion de l'expression « %1 » du type de données « %2 » en type de données « %3 ».|  
|0xC00470C5|-1073450811|DTS_E_EXPREVALFUNCTIONCOMPUTEFAILED|Échec de l'évaluation de la fonction « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC00470C6|-1073450810|DTS_E_EXPREVALFUNCTIONCONVERTPARAMTOMEMBERFAILED|Le paramètre %1!d! de la fonction « %2 » en valeur statique.|  
|0xC00470C7|-1073450809|DTS_E_REDIRECTROWUNAVAILABLEWITHFASTLOADANDZEROMAXINSERTCOMMITSIZE|Impossible de définir la disposition de ligne d'erreur dans « %1 » pour rediriger la ligne lorsque l'option de chargement rapide est activée et que la taille de validation d'insertion maximale a la valeur zéro.|  
|0xC00470CE|-1073450802|DTS_E_EXPREVALBINARYOPERATORCODEPAGEMISMATCH|Les pages de codes pour les opérandes de l'opérateur binaire « %1 » de type « %2 » doivent correspondre. Actuellement, la page de codes de l'opérande gauche ne correspond pas à la page de codes de l'opérande droit. Les pages de codes doivent être identiques pour l'opérateur binaire spécifié du type donné.|  
|0xC00470CF|-1073450801|DTS_E_EXPREVALVARIABLECOMPUTEFAILED|Échec de la récupération de la valeur de la variable « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC00470D0|-1073450800|DTS_E_EXPREVALVARIABLETYPENOTSUPPORTED|Le type de données de la variable « %1 » n'est pas pris en charge dans une expression.|  
|0xC00470D1|-1073450799|DTS_E_EXPREVALCASTCODEPAGEMISMATCH|Impossible de convertir l'expression « %1 » du type de données « %2 » en « %3 » car la page de codes de la valeur en cours de conversion (%4!d!) ne correspond pas à la page de codes de résultat demandée (%5!d!). La page de codes de la source doit correspondre à la page de codes demandée pour la destination.|  
|0xC00470D2|-1073450798|DTS_E_BUFFERSIZEOUTOFRANGE|La taille du tampon par défaut doit être comprise entre %%1!d! et %2!d! octets. Tentative de définition de la propriété DefaultBufferSize sur une valeur trop petite ou trop grande.|  
|0xC00470D3|-1073450797|DTS_E_BUFFERMAXROWSIZEOUTOFRANGE|Le nombre de lignes maximal du tampon par défaut doit être supérieur à %1!d! lignes. Tentative de définition de la propriété DefaultBufferMaxRows sur une valeur trop petite.|  
|0xC00470D4|-1073450796|DTS_E_EXTERNALCOLUMNMETADATACODEPAGEMISMATCH|La page de codes sur %1 est %2!d! et doit être %3!d!.|  
|0xC00470D5|-1073450795|DTS_E_THREADCOUNTOUTOFRANGE|Échec de l'assignation de %3!d! à la propriété EngineThreads de la tâche de flux. La valeur doit être comprise entre %1!d! et %2!d!.|  
|0xC00470D6|-1073450794|DTS_E_EXPREVALINVALIDTOKENSINGLEQUOTE|Échec de l'analyse de l'expression « %1 ». Guillemet simple inattendu à la ligne « %2 », numéro de caractère « %3 ».|  
|0xC00470D7|-1073450793|DTS_E_EXPREVALINVALIDTOKENSINGLEEQUAL|Échec de l'analyse de l'expression « %1 ». Signe égal (=) inattendu à la ligne « %2 », numéro de caractère « %3 ». Un double signe égal (==) peut être requis à l'emplacement spécifié.|  
|0xC00470DA|-1073450790|DTS_E_INDIVIDUALPUTREFTRACKERFAILED|Le composant « %1 » n'a pas pu mettre en cache la collection de suivi des références d'objets en cours d'exécution et a retourné le code d'erreur 0x%2!8.8X!.|  
|0xC00470DB|-1073450789|DTS_E_EXPREVALAMBIGUOUSINPUTCOLUMNNAME|Il existe plusieurs colonnes d'entrée nommées « %1 ». La colonne d'entrée souhaitée doit être spécifiée de façon unique en tant que [Component Name].[%2] ou référencée par un ID de lignage. Actuellement, la colonne d'entrée spécifiée existe dans plusieurs composants.|  
|0xC00470DC|-1073450788|DTS_E_EXPREVALDOTTEDINPUTCOLUMNNAMENOTFOUND|Échec de la recherche de la colonne d'entrée nommée « [%1].[%2] ». La colonne d'entrée est introuvable dans la collection de colonnes d'entrée.|  
|0xC00470DD|-1073450787|DTS_E_EXPREVALAMBIGUOUSVARIABLENNAME|Il existe plusieurs variables nommées « %1 ». La variable souhaitée doit être spécifiée de façon unique en tant que @[Namespace::%2]. La variable existe dans plusieurs espaces de noms.|  
|0xC00470DE|-1073450786|DTS_E_REDUCTIONFAILED|Le planificateur du moteur de flux de données n'a pas pu réduire le plan d'exécution pour le pipeline. Affectez la valeur False à la propriété OptimizedMode.|  
|0xC00470DF|-1073450785|DTS_E_EXPREVALSQRTINVALIDPARAM|La fonction SQRT ne peut pas être exécutée sur des valeurs négatives, or une valeur négative a été transmise à cette fonction.|  
|0xC00470E0|-1073450784|DTS_E_EXPREVALLNINVALIDPARAM|La fonction LN ne peut pas être exécutée sur des valeurs NULL ou négatives, or une valeur NULL ou négative a été transmise à cette fonction.|  
|0xC00470E1|-1073450783|DTS_E_EXPREVALLOGINVALIDPARAM|La fonction LOG ne peut pas être exécutée sur des valeurs NULL ou négatives, or une valeur NULL ou négative a été transmise à cette fonction.|  
|0xC00470E2|-1073450782|DTS_E_EXPREVALPOWERINVALIDPARAM|Impossible d'évaluer les paramètres transmis à la fonction POWER. Résultat indéterminé.|  
|0xC00470E3|-1073450781|DTS_E_NOCANCELEVENT|L'exécution ne peut pas fournir un événement d'annulation en raison de l'erreur 0x%1!8.8X!.|  
|0xC00470E4|-1073450780|DTS_E_CANCELRECEIVED|Le pipeline a reçu une demande d'annulation et s'arrête.|  
|0xC00470E5|-1073450779|DTS_E_EXPREVALUNARYOPOVERFLOW|Le résultat de l'opération moins unaire (négation) « %1 » dépasse la taille maximale autorisée pour le type de données de résultat « %2 ». La grandeur du résultat de l'opération dépasse les valeurs admises pour le type de résultat.|  
|0xC00470E6|-1073450778|DTS_E_EXPREVALPLACEHOLDERINEXPRESSION|L'espace réservé « %1 » a été trouvé dans une expression. Il doit être remplacé par un paramètre ou un opérande réel.|  
|0xC00470E7|-1073450777|DTS_E_EXPREVALFNRIGHTINVALIDLENGTH|La longueur %1!d! spécifiée pour la fonction « %2 » est négative et n'est pas valide. Le paramètre de longueur doit être positif.|  
|0xC00470E8|-1073450776|DTS_E_EXPREVALFNREPLICATEINVALIDREPEATCOUNT|Le nombre de répétitions %1!d! est négatif et n'est pas valide pour la fonction « %2 ». Le paramètre du nombre de répétitions ne peut pas être négatif.|  
|0xC00470EA|-1073450774|DTS_E_EXPREVALVARIABLECOULDNOTBEREAD|Échec de la lecture de la variable « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC00470EC|-1073450772|DTS_E_EXPREVALBINARYOPDTSTRNOTSUPPORTED|Pour les opérandes d'une opération binaire, le type de données DT_STR est uniquement pris en charge pour les colonnes d'entrée et les opérations de conversion. L'expression « %1 » a un opérande DT_STR qui n'est pas une colonne d'entrée ou le résultat d'une conversion, et elle ne peut pas être utilisée dans une opération binaire. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC00470ED|-1073450771|DTS_E_EXPREVALCONDITIONALOPDTSTRNOTSUPPORTED|Pour les opérandes d'un opérateur conditionnel, le type de données DT_STR est uniquement pris en charge pour les colonnes d'entrée et les opérations de conversion. L'expression « %1 » a un opérande DT_STR qui n'est pas une colonne d'entrée ou le résultat d'une conversion, et elle ne peut pas être utilisée avec l'opération conditionnelle. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC00470EE|-1073450770|DTS_E_EXPREVALFNFINDSTRINGINVALIDOCCURRENCECOUNT|Le nombre d'occurrences %1!d! n'est pas valide pour la fonction « %2 ». Ce paramètre doit être supérieur à zéro.|  
|0xC00470EF|-1073450769|DTS_E_INDIVIDUALPUTLOGENTRYINFOS|« %1 » n'a pas pu mettre en cache la collection LogEntryInfos et a retourné le code d'erreur 0x%2!8.8X!.|  
|0xC00470F0|-1073450768|DTS_E_EXPREVALINVALIDDATEPARTNODE|Le paramètre DATEPART spécifié pour la fonction « %1 » n'est pas valide. Ce doit être une chaîne statique.  Ce paramètre ne peut pas contenir d'éléments dynamiques tels que des colonnes d'entrée et doit être de type DT_WSTR.|  
|0xC00470F1|-1073450767|DTS_E_EXPREVALINVALIDCASTLENGTH|La valeur %1!d! spécifiée pour le paramètre de longueur de la conversion en type de données %2 est négative et non valide. La longueur doit être positive.|  
|0xC00470F2|-1073450766|DTS_E_EXPREVALINVALIDNULLCODEPAGE|La valeur %1!d! spécifiée pour le paramètre de page de codes de la fonction NULL avec le type de données « %2 » n'est pas valide. La page de codes n'est pas installée sur l'ordinateur.|  
|0xC00470F3|-1073450765|DTS_E_EXPREVALINVALIDNULLPRECISION|La valeur %1!d! spécifiée pour le paramètre de précision de la fonction NULL avec le type de données « %2 » est hors limites. La précision doit être comprise entre %3!d! et %4!d!.|  
|0xC00470F4|-1073450764|DTS_E_EXPREVALINVALIDNULLSCALE|La valeur %1!d! spécifiée pour le paramètre d'échelle de la fonction NULL avec le type de données %2 est hors limites. L'échelle doit être comprise entre %3!d! et %4!d!. L'échelle ne doit pas dépasser la précision et ne doit pas être négative.|  
|0xC00470F5|-1073450763|DTS_E_EXPREVALINVALIDNULLLENGTH|La valeur %1!d! spécifiée pour le paramètre de longueur de la fonction « NULL » avec le type de données %2 est négative et n'est pas valide. La longueur doit être positive.|  
|0xC00470F6|-1073450762|DTS_E_NEGATIVESNOTALLOWED|Impossible d'affecter une valeur négative à %1.|  
|0xC00470F7|-1073450761|DTS_E_FASTPARSENOTALLOWED|Impossible d'affecter la valeur TRUE à la propriété personnalisée « %1 » de « %2 ».  Le type de données de la colonne doit être l'un des suivants : DT_I1, DT_I2, DT_I4, DT_I8, DT_UI1, DT_UI2, DT_UI4, DT_UI8, DT_DBTIMESTAMP, DT_DBTIMESTAMP2, DT_DBTIMESTAMPOFFSET, DT_DATE, DT_DBDATE, DT_DBTIME, DT_DBTIME2 ou DT_FILETIME.|  
|0xC00470F8|-1073450760|DTS_E_CANNOTREATTACHPATH|Impossible de rattacher « %1 ». Supprimez le chemin d'accès, ajoutez-en un nouveau, puis attachez-le.|  
|0xC00470F9|-1073450759|DTS_E_EXPREVALINVALIDNUMBEROFPARAMSPLURALSINGULAR|La fonction « %1!s! » nécessite %2!d! paramètres et non pas %3!d! paramètre. Le nom de la fonction est reconnu, mais le nombre de paramètres n'est pas valide.|  
|0xC00470FA|-1073450758|DTS_E_EXPREVALINVALIDNUMBEROFPARAMSSINGULARPLURAL|La fonction « %1!s! » nécessite %2!d! paramètre, et non pas %3!d! paramètres. Le nom de la fonction est reconnu, mais le nombre de paramètres n'est pas valide.|  
|0xC00470FB|-1073450757|DTS_E_EXPREVALINVALIDNUMBEROFPARAMSPLURALPLURAL|La fonction « %1!s! » nécessite %2!d! paramètres et non pas %3!d! paramètres. Le nom de la fonction est reconnu, mais le nombre de paramètres n'est pas valide.|  
|0xC00470FC|-1073450756|DTS_E_EXPREVALFAILEDTOPARSEEXPRESSIONOUTOFMEMORY|La tentative d'analyse de l'expression « %1 » a échoué à cause d'une erreur de mémoire disponible insuffisante.|  
|0xC00470FD|-1073450755|DTS_E_INDIVIDUALCHECKPRODUCTLEVELFAILED|Le %1 n'a pas pu effectuer son contrôle de niveau de produit requis et a retourné le code d'erreur 0x%2!8.8X!.|  
|0xC00470FE|-1073450754|DTS_E_PRODUCTLEVELTOLOW|Code d'erreur SSIS DTS_E_PRODUCTLEVELTOLOW.  Le « %1 » ne peut pas s'exécuter sur la version installée d'Integration Services %2. La version %3 ou une version ultérieure est requise.|  
|0xC00470FF|-1073450753|DTS_E_EXPREVALSTRINGLITERALTOOLONG|Un littéral de chaîne dans l'expression dépasse la longueur maximale autorisée de %1!d! caractères.|  
|0xC0047100|-1073450752|DTS_E_EXPREVALSTRINGVARIABLETOOLONG|La variable %1 contient une chaîne qui dépasse la longueur maximale autorisée de %2!d! caractères.|  
|0xC0047101|-1073450751|DTS_E_COMPONENT_NOINTERFACE|Le %1 a été trouvé, mais il ne prend pas en charge une interface Integration Services requise (IDTSRuntimeComponent100).  Obtenez une version mise à jour de ce composant auprès du fournisseur de composant.|  
|0xC0048000|-1073446912|DTS_E_CANNOTOPENREGISTRYKEY|Impossible d'ouvrir la clé de Registre « %1 ».|  
|0xC0048001|-1073446911|DTS_E_INVALIDCOMPONENTFILENAME|Impossible d'obtenir le nom de fichier pour le composant comportant le CLSID « %1 ». Vérifiez que le composant est enregistré correctement ou que le CLSID spécifié est correct.|  
|0xC0048002|-1073446910|DTS_E_UNKNOWNCOMPONENTHASINVALIDCLSID|Le CLSID de l'un des composants n'est pas valide. Vérifiez que tous les composants du pipeline ont des CLSID valides.|  
|0xC0048003|-1073446909|DTS_E_COMPONENTHASINVALIDCLSID|Le CLSID de l'un des composants avec l'ID %1!d! n’est pas valide.|  
|0xC0048004|-1073446908|DTS_E_INVALIDINDEX|L'index n'est pas valide.|  
|0xC0048005|-1073446907|DTS_E_CANNOTACCESSDTSAPPLICATIONOBJECT|Impossible d'accéder à l'objet Application. Assurez-vous que SSIS est installé correctement.|  
|0xC0048006|-1073446906|DTS_E_ERROROCCURREDWHILERETRIEVINGFILENAME|Impossible de récupérer le nom de fichier d'un composant. Code d'erreur : 0x%1!8.8X!.|  
|0xC0048007|-1073446905|DTS_E_CANNOTRETRIEVEPROPERTYFORCOMPONENT|Impossible de récupérer la propriété « %1 » du composant comportant l'ID %2!d!.|  
|0xC0048008|-1073446904|DTS_E_DUPLICATEIDFOUND|Tentative d'utilisation de l'ID %1!d! plusieurs fois dans la tâche de flux de données.|  
|0xC0048009|-1073446903|DTS_E_CANNOTRETRIEVEBYLINEAGE|Impossible de récupérer un élément par ID de lignage dans une collection qui ne contient aucune colonne.|  
|0xC004800B|-1073446901|DTS_E_CANNOTMAPRUNTIMECONNECTIONMANAGER|Le gestionnaire de connexions comportant l'ID « %1 » est introuvable dans la collection de gestionnaires de connexions, en raison du code d'erreur : 0x%2!8.8X!. « %3 » nécessite ce gestionnaire de connexions dans la collection des gestionnaires de connexions de « %4 ». Vérifiez qu'un gestionnaire de connexions a été créé avec cet ID dans la collection de gestionnaires de connexions Connexions.|  
|0xC004800E|-1073446898|DTS_E_INPUTNOTKNOWN|Le thread « %1 » a reçu un tampon pour l'entrée %2!d!, mais ce thread n'est pas responsable de cette entrée. Une erreur s'est produite et a provoqué la création d'un plan d'exécution incorrect par le planificateur du moteur de flux de données.|  
|0xC004800F|-1073446897|DTS_E_GETRTINTERFACEFAILED|Le composant « %1 » (%2!d!) ne peut pas fournir une interface IDTSRuntimeComponent100.|  
|0xC0048011|-1073446895|DTS_E_CANTGIVEAWAYBUFFER|Le moteur de la tâche de flux de données n'a pas réussi à copier un tampon pour l'affecter à un autre thread.|  
|0xC0048012|-1073446894|DTS_E_CANTCREATEVIEWBUFFER|Le moteur de la tâche de flux de données n'a pas pu créer un tampon d'affichage de type %1!d! sur le type %2!d! du tampon %3!d.|  
|0xC0048013|-1073446893|DTS_E_UNUSABLETEMPORARYPATH|Le gestionnaire de tampons n'a pas pu créer un fichier temporaire dans le chemin d'accès « %1 ». Ce chemin ne sera plus utilisé pour le stockage temporaire.|  
|0xC0048014|-1073446892|DTS_E_DIRECTTONONERROROUTPUT|Le gestionnaire de tampons a tenté de transmettre une ligne d'erreur vers une sortie qui n'est pas enregistrée en tant que sortie d'erreur. Un appel DirectErrorRow a été émis sur une sortie dont la propriété IsErrorOut n'a pas la valeur TRUE.|  
|0xC0048015|-1073446891|DTS_E_BUFFERISPRIVATE|Une méthode de tampon a été appelée dans un tampon privé, or les tampons privés ne prennent pas en charge ce type d'opération.|  
|0xC0048016|-1073446890|DTS_E_BUFFERISFLAT|Les tampons privés ne prennent pas en charge cette opération.|  
|0xC0048017|-1073446889|DTS_E_BUFFERISPRIMEOUTPUT|Impossible d'appeler cette opération dans un tampon transmis à PrimeOutput. Une méthode de tampon a été appelée lors de l'exécution de PrimeOutput, mais cet appel n'est pas autorisé pendant une opération PrimeOutput.|  
|0xC0048018|-1073446888|DTS_E_BUFFERISPROCESSINPUT|Impossible d'appeler cette opération dans un tampon transmis à ProcessInput. Une méthode de tampon a été appelée lors de l'exécution de ProcessInput, mais cet appel n'est pas autorisé pendant une opération ProcessInput.|  
|0xC0048019|-1073446887|DTS_E_BUFFERGETTEMPFILENAME|Le gestionnaire de tampons n'a pas pu obtenir un nom de fichier temporaire.|  
|0xC004801A|-1073446886|DTS_E_REFERENCECOLUMNTOOWIDE|Le code a détecté une colonne trop large.|  
|0xC004801B|-1073446885|DTS_E_CANNOTGETRUNTIMECONNECTIONMANAGERID|Impossible d'obtenir l'ID du gestionnaire de connexions en cours d'exécution spécifié par « %1 » dans la collection des gestionnaires de connexions nommée Connections de « %2 ». Vérifiez que la propriété ConnectionManager.ID de l'objet de connexion en cours d'exécution est définie pour le composant.|  
|0xC004801C|-1073446884|DTS_E_EMPTYRUNTIMECONNECTIONMANAGERID|Aucune valeur n'est définie pour la propriété ID de « %1 » dans la collection des gestionnaires de connexions nommée Connections de « %2 ». Vérifiez que la propriété ConnectionManagerID de l'objet de connexion en cours d'exécution est définie pour le composant.|  
|0xC004801D|-1073446883|DTS_E_METADATAREADONLY|Impossible de modifier les métadonnées lors de l'exécution.|  
|0xC004801F|-1073446881|DTS_E_UPGRADEFAILED|Impossible de mettre à niveau les métadonnées du composant de « %1 » vers la nouvelle version. Échec de la méthode PerformUpgrade.|  
|0xC0048020|-1073446880|DTS_E_COMPONENTVERSIONMISMATCH|La version de %1 n'est pas compatible avec cette version de DataFlow.|  
|0xC0048021|-1073446879|DTS_E_ERRORCOMPONENT|Le composant est manquant, n'est pas enregistré, ne peut pas être mis à niveau ou des interfaces obligatoires sont manquantes. Informations de contact de ce composant : « %1 ».|  
|0xC0048022|-1073446878|DTS_E_BUFFERISNOTPRIMEOUTPUT|La méthode a été appelée dans un tampon incorrect. Les tampons inutilisés pour la sortie du composant ne prennent pas en charge cette opération.|  
|0xC0049014|-1073442796|DTS_E_EXPREVALSTATIC_COMPUTATIONFAILED|Une erreur s'est produite lors du calcul de l'expression.|  
|0xC0049030|-1073442768|DTS_E_EXPREVALSTATIC_DIVBYZERO|Une division par zéro a eu lieu dans l'expression.|  
|0xC0049031|-1073442767|DTS_E_EXPREVALSTATIC_LITERALOVERFLOW|La grandeur de la valeur littérale est trop élevée pour le type demandé.|  
|0xC0049032|-1073442766|DTS_E_EXPREVALSTATIC_BINARYOPNUMERICOVERFLOW|Le résultat d'une opération binaire est trop grand pour la taille maximale de types numériques. Impossible d'effectuer une conversion implicite des types d'opérandes en résultat numérique (DT_NUMERIC) sans perte de précision ou d'échelle. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC0049033|-1073442765|DTS_E_EXPREVALSTATIC_BINARYOPOVERFLOW|La grandeur du résultat d'une opération binaire dépasse la taille maximale du type de données de résultat.|  
|0xC0049034|-1073442764|DTS_E_EXPREVALSTATIC_FUNCTIONOVERFLOW|La grandeur du résultat d'un appel de fonction est trop grande pour le type de résultat et dépasse le type de l'opérande. Une conversion explicite en un type plus grand peut être nécessaire.|  
|0xC0049035|-1073442763|DTS_E_EXPREVALSTATIC_BINARYTYPEMISMATCH|Des types de données incompatibles ont été utilisés avec un opérateur binaire. Impossible d'effectuer une conversion implicite des types d'opérandes en types compatibles pour l'opération. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC0049036|-1073442762|DTS_E_EXPREVALSTATIC_UNSUPPORTEDBINARYTYPE|Un type de données non pris en charge a été utilisé avec un opérateur binaire. Le type de l'un des opérandes ou des deux n'est pas pris en charge par l'opération. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC0049037|-1073442761|DTS_E_EXPREVALSTATIC_BINARYSIGNMISMATCH|Incompatibilité de signe pour l'opérateur binaire. Les opérandes de cet opérateur doivent être tous les deux positifs ou négatifs.|  
|0xC0049038|-1073442760|DTS_E_EXPREVALSTATIC_BINARYOPERATIONFAILED|Échec d'une opération binaire, en raison d'une condition de mémoire insuffisante ou d'une erreur interne.|  
|0xC0049039|-1073442759|DTS_E_EXPREVALSTATIC_BINARYOPERATIONSETTYPEFAILED|Échec de la définition du type de résultat d'une opération binaire.|  
|0xC004903A|-1073442758|DTS_E_EXPREVALSTATIC_STRINGCOMPARISONFAILED|Impossible de comparer deux chaînes.|  
|0xC004903B|-1073442757|DTS_E_EXPREVALSTATIC_UNSUPPORTEDUNNARYTYPE|Un type de données non pris en charge est utilisé avec un opérateur binaire. Le type d'opérande n'est pas pris en charge pour l'opération. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC004903C|-1073442756|DTS_E_EXPREVALSTATIC_UNARYOPERATIONFAILED|Échec d'une opération unaire, en raison d'une condition de mémoire insuffisante ou d'une erreur interne.|  
|0xC004903D|-1073442755|DTS_E_EXPREVALSTATIC_UNARYOPERATIONSETTYPEFAILED|Échec de la définition du type de résultat d'une opération unaire.|  
|0xC004903E|-1073442754|DTS_E_EXPREVALSTATIC_PARAMTYPEMISMATCH|Une fonction a un paramètre comportant un type de données non pris en charge. Impossible d'effectuer la conversion implicite du type de paramètre en un type compatible pour la fonction. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC004903F|-1073442753|DTS_E_EXPREVALSTATIC_INVALIDFUNCTION|Un nom de fonction non valide a été trouvé dans l'expression. Vérifiez que le nom est correct et que la fonction existe.|  
|0xC0049040|-1073442752|DTS_E_EXPREVALSTATIC_FNSUBSTRINGINVALIDLENGTH|Le paramètre de longueur n'est pas valide pour la fonction SUBSTRING. Ce paramètre ne peut pas être négatif.|  
|0xC0049041|-1073442751|DTS_E_EXPREVALSTATIC_FNSUBSTRINGINVALIDSTARTINDEX|L'index de début n'est pas valide pour la fonction SUBSTRING. La valeur de cet index doit être un entier supérieur à zéro. L'index de début est basé sur la valeur 1 et non pas sur la valeur 0.|  
|0xC0049042|-1073442750|DTS_E_EXPREVALSTATIC_INVALIDNUMBEROFPARAMS|Un nombre de paramètres incorrect a été spécifié pour une fonction. Le nom de la fonction est reconnu, mais le nombre de paramètres est incorrect.|  
|0xC0049043|-1073442749|DTS_E_EXPREVALSTATIC_CHARMAPPINGFAILED|Échec d'une fonction de mappage des caractères.|  
|0xC0049044|-1073442748|DTS_E_EXPREVALSTATIC_INVALIDDATEPART|Un paramètre d'élément de date non reconnu a été spécifié pour une fonction de date.|  
|0xC0049045|-1073442747|DTS_E_EXPREVALSTATIC_INVALIDNULLPARAM|Un paramètre non valide a été spécifié pour la fonction NULL. Les paramètres de la fonction NULL doivent être statiques et ne peuvent pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0049046|-1073442746|DTS_E_EXPREVALSTATIC_INVALIDNULLPARAMTYPE|Un paramètre non valide a été spécifié pour la fonction NULL. Un paramètre de fonction NULL doit être un entier ou un type qui peut être converti en entier.|  
|0xC0049047|-1073442745|DTS_E_EXPREVALSTATIC_FUNCTIONPARAMNOTSTATIC|Un paramètre non valide a été spécifié pour une fonction. Ce paramètre doit être statique et ne peut pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0049048|-1073442744|DTS_E_EXPREVALSTATIC_INVALIDCASTPARAM|Un paramètre non valide a été spécifié pour une opération de conversion. Les paramètres des opérateurs de conversion doivent être statiques et ne peuvent pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0049049|-1073442743|DTS_E_EXPREVALSTATIC_INVALIDCASTPARAMTYPE|Un paramètre non valide a été spécifié pour une opération de conversion. Le paramètre d'un opérateur de conversion doit être un entier ou un type qui peut être converti en entier.|  
|0xC004904A|-1073442742|DTS_E_EXPREVALSTATIC_INVALIDCAST|L'expression contient une conversion d'un type non pris en charge.|  
|0xC004904B|-1073442741|DTS_E_EXPREVALSTATIC_INVALIDTOKEN|L'expression contient un jeton non reconnu. Impossible de l'analyser car elle contient des éléments non valides.|  
|0xC004904C|-1073442740|DTS_E_EXPREVALSTATIC_FAILEDTOPARSEEXPRESSION|L'expression n'est pas valide et ne peut pas être analysée. Elle contient peut-être des éléments non valides ou n'est pas correctement formée.|  
|0xC004904D|-1073442739|DTS_E_EXPREVALSTATIC_UNARYOPOVERFLOW|Le résultat d'une opération moins unaire (négation) dépasse la taille maximale autorisée pour le type de données de résultat. La grandeur du résultat de l'opération dépasse les valeurs admises pour le type de résultat.|  
|0xC004904E|-1073442738|DTS_E_EXPREVALSTATIC_COMPUTEFAILED|La tentative de calcul de l'expression a échoué.|  
|0xC004904F|-1073442737|DTS_E_EXPREVALSTATIC_BUILDSTRINGFAILED|La tentative de génération d'une représentation de chaîne de l'expression a échoué.|  
|0xC0049050|-1073442736|DTS_E_EXPREVALSTATIC_CANNOTCONVERTRESULT|Impossible de convertir le type de données du résultat de l'expression en type de données de colonne. Le résultat de l'expression doit être écrit dans une colonne d'entrée/sortie, mais le type de données de l'expression ne peut pas être converti en type de données de la colonne.|  
|0xC0049051|-1073442735|DTS_E_EXPREVALSTATIC_CONDITIONALOPINVALIDCONDITIONTYPE|L'expression conditionnelle de l'opérateur conditionnel a un type de données non valide. L'expression conditionnelle doit être de type DT_BOOL.|  
|0xC0049052|-1073442734|DTS_E_EXPREVALSTATIC_CONDITIONALOPTYPEMISMATCH|Les types de données des opérandes de l'opérateur conditionnel sont incompatibles. Impossible d'effectuer une conversion implicite des types d'opérandes en types compatibles pour l'opération conditionnelle. Pour effectuer cette opération, une conversion explicite de l'un des opérandes ou des deux doit être effectuée à l'aide d'un opérateur de conversion.|  
|0xC0049053|-1073442733|DTS_E_EXPREVALSTATIC_CONDITIONALOPSETTYPEFAILED|Échec de la définition du type de résultat d'une opération conditionnelle.|  
|0xC0049054|-1073442732|DTS_E_EXPREVALSTATIC_INPUTCOLUMNNAMENOTFOUND|La colonne d'entrée spécifiée est introuvable dans la collection de colonnes d'entrée.|  
|0xC0049055|-1073442731|DTS_E_EXPREVALSTATIC_INPUTCOLUMNIDNOTFOUND|Échec de la recherche d'une colonne d'entrée par ID de lignage. La colonne d'entrée est introuvable dans la collection de colonnes d'entrée.|  
|0xC0049056|-1073442730|DTS_E_EXPREVALSTATIC_NOINPUTCOLUMNCOLLECTION|L'expression contient un jeton non reconnu qui est une référence à une colonne d'entrée, mais la collection de colonnes d'entrée n'est pas disponible pour traiter les colonnes d'entrée. La collection de colonnes d'entrée n'a pas été spécifiée pour l'évaluateur d'expression, mais une colonne d'entrée est incluse dans l'expression.|  
|0xC0049057|-1073442729|DTS_E_EXPREVALSTATIC_VARIABLENOTFOUND|Une variable spécifiée est introuvable dans la collection. Elle n'existe peut-être pas dans l'étendue correcte. Vérifiez que la variable existe et que l'étendue est correcte.|  
|0xC0049058|-1073442728|DTS_E_EXPREVALSTATIC_INVALIDTOKENSTATE|Échec de l'analyse de l'expression. L'expression contient un jeton non valide ou incomplet. Elle contient peut-être des éléments non valides, un élément obligatoire manque (notamment des parenthèses fermantes), ou elle n'est peut-être pas correctement formée.|  
|0xC0049059|-1073442727|DTS_E_EXPREVALSTATIC_INVALIDCASTCODEPAGE|La valeur spécifiée pour le paramètre de page de codes de la conversion en type de données DT_STR ou DT_TEXT n'est pas valide. La page de codes spécifiée n'est pas installée sur l'ordinateur.|  
|0xC004905A|-1073442726|DTS_E_EXPREVALSTATIC_INVALIDCASTPRECISION|La valeur spécifiée pour le paramètre de précision d'une opération de conversion est hors limites pour la conversion du type.|  
|0xC004905B|-1073442725|DTS_E_EXPREVALSTATIC_INVALIDCASTSCALE|La valeur spécifiée pour le paramètre d'échelle d'une opération de conversion est hors limites pour la conversion du type. L'échelle ne doit pas dépasser la précision et ne doit pas être négative.|  
|0xC004905C|-1073442724|DTS_E_EXPREVALSTATIC_CONDITIONALOPCODEPAGEMISMATCH|Les pages de codes ne correspondent pas dans une opération conditionnelle. La page de codes de l'opérande gauche ne correspond pas à la page de codes de l'opérande droit. Les pages de codes doivent être identiques pour ce type d'opérateur conditionnel.|  
|0xC004905D|-1073442723|DTS_E_EXPREVALSTATIC_ILLEGALHEXESCAPEINSTRINGLITERAL|Un littéral de chaîne contient une séquence d'échappement hexadécimale non conforme. La séquence d'échappement n'est pas prise en charge dans les littéraux de chaîne de l'évaluateur d'expression. Les séquences d'échappement hexadécimales doivent être au format \xhhhh où h est un chiffre hexadécimal valide.|  
|0xC004905E|-1073442722|DTS_E_EXPREVALSTATIC_ILLEGALESCAPEINSTRINGLITERAL|Le littéral de chaîne contient une séquence d'échappement non conforme. La séquence d'échappement n'est pas prise en charge dans les littéraux de chaîne de l'évaluateur d'expression. Si la chaîne nécessite une barre oblique, utilisez une double barre oblique «\\\\».|  
|0xC004905F|-1073442721|DTS_E_EXPREVALSTATIC_UNSUPPORTEDTYPE|Un type de données non reconnu ou non pris en charge a été utilisé dans l'expression.|  
|0xC0049060|-1073442720|DTS_E_EXPREVALSTATIC_DATACONVERSIONOVERFLOW|Un dépassement s'est produit lors de la conversion des types de données. Le type de source est trop grand pour le type de destination.|  
|0xC0049061|-1073442719|DTS_E_EXPREVALSTATIC_DATACONVERSIONNOTSUPPORTED|L'expression contient une conversion de type de données non prise en charge. Impossible de convertir le type de source en type de destination.|  
|0xC0049062|-1073442718|DTS_E_EXPREVALSTATIC_DATACONVERSIONFAILED|Une erreur s'est produite lors de la conversion des données. Impossible de convertir le type de source en type de destination.|  
|0xC0049063|-1073442717|DTS_E_EXPREVALSTATIC_CONDITIONALOPERATIONFAILED|Échec de l'opération conditionnelle.|  
|0xC0049064|-1073442716|DTS_E_EXPREVALSTATIC_CASTFAILED|Une erreur s'est produite lors de la conversion d'un type.|  
|0xC0049065|-1073442715|DTS_E_EXPREVALFAILEDTOCONVERTSTRCOLUMNTOWSTR|Échec de la conversion de « %1 » du type DT_STR en DT_WSTR. Une erreur s’est produite pendant la conversion implicite dans la colonne d’entrée.|  
|0xC0049066|-1073442714|DTS_E_EXPREVALSTATIC_FAILEDTOCONVERTSTRCOLUMNTOWSTR|Échec de la conversion d'une colonne d'entrée de DT_STR en DT_WSTR. Une erreur s'est produite lors de la conversion implicite dans la colonne d'entrée.|  
|0xC0049067|-1073442713|DTS_E_EXPREVALSTATIC_FUNCTIONCOMPUTEFAILED|Une erreur s'est produite lors de l'évaluation de la fonction.|  
|0xC0049068|-1073442712|DTS_E_EXPREVALSTATIC_FUNCTIONCONVERTPARAMTOMEMBERFAILED|Impossible de convertir un paramètre de fonction en une valeur statique. Ce paramètre doit être statique, et il ne peut pas contenir d'éléments dynamiques tels que des colonnes d'entrée.|  
|0xC0049088|-1073442680|DTS_E_EXPREVALSTATIC_FNRIGHTINVALIDLENGTH|Le paramètre de longueur n'est pas valide pour la fonction RIGHT. Ce paramètre ne peut pas être négatif.|  
|0xC0049089|-1073442679|DTS_E_EXPREVALSTATIC_FNREPLICATEINVALIDREPEATCOUNT|Le paramètre du nombre de répétitions n'est pas valide pour la fonction REPLICATE. Ce paramètre ne peut pas être négatif.|  
|0xC0049096|-1073442666|DTS_E_EXPREVALSTATIC_BINARYOPERATORCODEPAGEMISMATCH|Les pages de codes ne correspondent pas dans une opération binaire. La page de codes de l'opérande gauche ne correspond pas à la page de codes de l'opérande droit. Les pages de codes doivent être identiques pour cette opération binaire.|  
|0xC0049097|-1073442665|DTS_E_EXPREVALSTATIC_VARIABLECOMPUTEFAILED|Échec de la récupération de la valeur d'une variable.|  
|0xC0049098|-1073442664|DTS_E_EXPREVALSTATIC_VARIABLETYPENOTSUPPORTED|L'expression contient une variable avec un type de données non pris en charge.|  
|0xC004909B|-1073442661|DTS_E_EXPREVALSTATIC_CASTCODEPAGEMISMATCH|Impossible de convertir l'expression car la page de codes de la valeur en cours de conversion ne correspond pas à la page de codes de résultat demandé. La page de codes de la source doit correspondre à la page de codes demandée pour la destination.|  
|0xC004909C|-1073442660|DTS_E_EXPREVALSTATIC_INVALIDTOKENSINGLEQUOTE|L'expression contient un guillemet simple inattendu. Un guillemet double peut être nécessaire.|  
|0xC004909D|-1073442659|DTS_E_EXPREVALSTATIC_INVALIDTOKENSINGLEEQUAL|L'expression contient un signe égal (=) inattendu. Cette erreur se produit lorsqu'un double signe égal (==) est nécessaire.|  
|0xC00490AA|-1073442646|DTS_E_EXPREVALSTATIC_AMBIGUOUSINPUTCOLUMNNAME|Un nom de colonne d'entrée ambigu a été spécifié.  La colonne doit être nommée [Component Name].[Column Name] ou référencée par un ID de lignage. Cette erreur se produit lorsque la colonne d'entrée existe dans plusieurs composants et doit être différenciée par l'ajout du nom du composant ou en utilisant l'ID de lignage.|  
|0xC00490AB|-1073442645|DTS_E_EXPREVALSTATIC_PLACEHOLDERINEXPRESSION|Un paramètre de fonction d'espace réservé ou un opérande a été trouvé dans une expression. Il doit être remplacé par un paramètre ou un opérande réel.|  
|0xC00490AC|-1073442644|DTS_E_EXPREVALSTATIC_AMBIGUOUSVARIABLENNAME|Un nom de variable ambigu a été spécifié. La variable souhaitée doit être nommée @[Namespace::Variable]. Cette erreur se produit lorsque la variable existe dans plusieurs espaces de noms.|  
|0xC00490D3|-1073442605|DTS_E_EXPREVALSTATIC_BINARYOPDTSTRNOTSUPPORTED|Pour les opérandes d'opération binaire, le type de données DT_STR est uniquement pris en charge pour les colonnes d'entrée et les opérations de conversion. Un opérande DT_STR qui n'est pas une colonne d'entrée ou le résultat d'une conversion ne peut pas être utilisé avec une opération binaire. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC00490D4|-1073442604|DTS_E_EXPREVALSTATIC_CONDITIONALOPDTSTRNOTSUPPORTED|Pour les opérandes de l'opérateur conditionnel, le type de données DT_STR est uniquement pris en charge pour les colonnes d'entrée et les opérations de conversion. Un opérande DT_STR qui n'est pas une colonne d'entrée ou le résultat d'une conversion ne peut pas être utilisé avec une opération conditionnelle. Pour effectuer cette opération, l'opérande doit être explicitement converti à l'aide d'un opérateur de conversion.|  
|0xC00490D5|-1073442603|DTS_E_EXPREVALSTATIC_FNFINDSTRINGINVALIDOCCURRENCECOUNT|Le paramètre du nombre d'occurrences n'est pas valide pour la fonction FINDSTRING. Ce paramètre doit être supérieur à zéro.|  
|0xC00490DD|-1073442595|DTS_E_EXPREVALSTATIC_INVALIDDATEPARTNODE|Le paramètre DATEPART spécifié pour une fonction de date n'est pas valide. Ce paramètre doit être une chaîne statique, et il ne peut pas contenir d'éléments dynamiques tels que des colonnes d'entrée. Il doit être de type DT_WSTR.|  
|0xC00490DE|-1073442594|DTS_E_EXPREVALSTATIC_INVALIDCASTLENGTH|La valeur spécifiée pour le paramètre de longueur d'une opération de conversion n'est pas valide. La longueur doit être positive. La longueur spécifiée pour la conversion du type est négative. Changez-la en une valeur positive.|  
|0xC00490DF|-1073442593|DTS_E_EXPREVALSTATIC_INVALIDNULLLENGTH|La valeur spécifiée pour le paramètre de longueur d'une fonction NULL n'est pas valide. La longueur doit être positive. La longueur spécifiée pour la fonction NULL est négative. Changez-la en une valeur positive.|  
|0xC00490E0|-1073442592|DTS_E_EXPREVALSTATIC_INVALIDNULLCODEPAGE|La valeur spécifiée pour le paramètre de page de codes de la fonction NULL avec le type de données DT_STR ou DT_TEXT n'est pas valide. La page de codes spécifiée n'est pas installée sur l'ordinateur. Modifiez la page de codes spécifiée ou installez-la sur l'ordinateur.|  
|0xC00490E1|-1073442591|DTS_E_EXPREVALSTATIC_INVALIDNULLPRECISION|La valeur spécifiée pour le paramètre de précision d'une fonction NULL n'est pas valide. La précision spécifiée est hors limites pour la fonction NULL.|  
|0xC00490E2|-1073442590|DTS_E_EXPREVALSTATIC_INVALIDNULLSCALE|La valeur spécifiée pour le paramètre d'échelle d'une fonction NULL n'est pas valide. L'échelle spécifiée est hors limites pour la fonction NULL. L'échelle ne doit pas dépasser la précision et doit être positive.|  
|0xC00490E8|-1073442584|DTS_E_XMLSRCERRORSETTINGERROROUTPUTCOLUMNDATA|%1 n'a pas pu écrire de données dans %2 sur %3. 4|  
|0xC00490F5|-1073442571|DTS_E_TXLOOKUP_CANCEL_REQUESTED|La transformation de recherche a reçu une demande d'annulation de l'utilisateur.|  
|0xC00490F6|-1073442570|DTS_E_LOBLENGTHLIMITEXCEEDED|Le traitement de données de type caractère ou de données d'objets BLOB (Binary Large Object) a été interrompu parce que la limite de 4 Go a été atteinte.|  
|0xC00490F7|-1073442569|DTS_E_CANNOTLOADCOMPONENT|Impossible de charger le composant de pipeline managé « %1 ».  L'exception était : %2.|  
|0xC00F9304|-1072721148|DTS_E_OLEDB_EXCEL_NOT_SUPPORTED|Code d'erreur SSIS DTS_E_OLEDB_EXCEL_NOT_SUPPORTED : le Gestionnaire de connexions Excel n'est pas pris en charge dans la version 64 bits de SSIS, car aucun fournisseur OLE DB n'est disponible.|  
|0xC00F9310|-1072721136|DTS_E_CACHEBADHEADER|Le fichier cache est endommagé ou n'a pas été créé à l'aide du gestionnaire de connexions du cache.  Fournissez un fichier cache valide.|  
|0xC0202001|-1071636479|DTS_E_MISSINGSQLCOMMAND|La commande SQL n'a pas été définie correctement. Vérifiez la propriété SQLCommand.|  
|0xC0202002|-1071636478|DTS_E_COMERROR|Des informations sur l'objet de l'erreur sont disponibles.  Source : « %1 » Code d'erreur : 0x%2!8.8X!  Description : « %3 ».|  
|0xC0202003|-1071636477|DTS_E_ACQUIREDCONNECTIONUNAVAILABLE|Impossible d'accéder aux connexions acquises.|  
|0xC0202004|-1071636476|DTS_E_INCORRECTCOLUMNCOUNT|Le nombre de colonnes est incorrect.|  
|0xC0202005|-1071636475|DTS_E_COLUMNNOTFOUND|La colonne « %1 » est introuvable dans la source de données.|  
|0xC0202007|-1071636473|DTS_E_OLEDBRECORD|Un enregistrement OLE DB est disponible.  Source : « %1 » Hresult : 0x%2!8.8X!  Description : « %3 ».|  
|0xC0202009|-1071636471|DTS_E_OLEDBERROR|Code d'erreur SSIS DTS_E_OLEDBERROR.  Une erreur OLE DB s'est produite. Code d'erreur : 0x%1!8.8X!.|  
|0xC020200A|-1071636470|DTS_E_ALREADYCONNECTED|Le composant est déjà connecté. Déconnectez-le avant d'essayer de le connecter.|  
|0xC020200B|-1071636469|DTS_E_INCORRECTSTOCKPROPERTYVALUE|La valeur de la propriété « %1 » est incorrecte.|  
|0xC020200E|-1071636466|DTS_E_CANNOTOPENDATAFILE|Impossible d'ouvrir le fichier de données « %1 ».|  
|0xC0202010|-1071636464|DTS_E_DESTINATIONFLATFILEREQUIRED|Aucun nom de fichier plat de destination n'est spécifié. Vérifiez que le gestionnaire de connexions de fichiers plats est configuré avec une chaîne de connexion. Si ce gestionnaire est utilisé par plusieurs composants, vérifiez que la chaîne de connexion contient suffisamment de noms de fichiers.|  
|0xC0202011|-1071636463|DTS_E_TEXTQUALIFIERNOTFOUND|L'identificateur de texte de la colonne « %1 » est introuvable.|  
|0xC0202014|-1071636460|DTS_E_CANNOTCONVERTTYPES|La conversion de « %1 » en « %2 » n'est pas prise en charge.|  
|0xC0202015|-1071636459|DTS_E_PROBLEMDETECTINGTYPECOMPATIBILITY|Le code d'erreur 0x%1!8.8X! a été retourné lors de la validation de la conversion du type %2 en %3.|  
|0xC0202016|-1071636458|DTS_E_CANNOTMAPINPUTCOLUMNTOOUTPUTCOLUMN|La colonne d’entrée avec l’ID de lignage « %1!d! » requis par « %2 » est introuvable. Vérifiez la propriété personnalisée SourceInputColumnLineageID de la colonne de sortie.|  
|0xC0202017|-1071636457|DTS_E_INCORRECTMINIMUMNUMBEROFOUTPUTS|Le nombre de sorties est incorrect. Il doit y avoir au moins %1!d! sorties.|  
|0xC0202018|-1071636456|DTS_E_INCORRECTEXACTNUMBEROFOUTPUTS|Le nombre de sorties est incorrect. Il doit être exactement de %1!d! entrée(s).|  
|0xC0202019|-1071636455|DTS_E_STRINGCONVERSIONTOOLONG|Une chaîne trop longue n'a pas pu être convertie.|  
|0xC020201A|-1071636454|DTS_E_INCORRECTEXACTNUMBEROFINPUTS|Le nombre d'entrées est incorrect. Il doit être exactement de %1!d! entrées.|  
|0xC020201B|-1071636453|DTS_E_CANNOTHAVEZEROINPUTCOLUMNS|Le nombre de colonnes d'entrée pour %1 ne peut pas être égal à zéro.|  
|0xC020201C|-1071636452|DTS_E_CANNOTHAVEINPUTS|Ce composant contient %1!d! entrées.  Aucune entrée n'est autorisée dans ce composant.|  
|0xC020201D|-1071636451|DTS_E_PROCESSINPUTCALLEDWITHINVALIDINPUTID|ProcessInput a été appelé avec un ID d'entrée non valide de %1!d!.|  
|0xC020201F|-1071636449|DTS_E_INCORRECTCUSTOMPROPERTYTYPE|La propriété personnalisée « %1 » doit être de type %2.|  
|0xC0202020|-1071636448|DTS_E_INVALIDBUFFERTYPE|Le type de tampon n'est pas valide. Vérifiez que la structure du pipeline et tous les composants sont valides.|  
|0xC0202021|-1071636447|DTS_E_INCORRECTCUSTOMPROPERTYVALUE|Valeur de la propriété personnalisée %1 incorrecte.|  
|0xC0202022|-1071636446|DTS_E_CONNECTIONREQUIREDFORMETADATA|Une erreur s'est produite en raison de l'absence de connexion. Une connexion est nécessaire lors de la demande de métadonnées. Si vous travaillez hors connexion, désactivez Travailler hors connexion dans le menu SSIS pour activer la connexion.|  
|0xC0202023|-1071636445|DTS_E_CANTCREATECUSTOMPROPERTY|Impossible de créer la propriété personnalisée « %1 ».|  
|0xC0202024|-1071636444|DTS_E_CANTGETCUSTOMPROPERTYCOLLECTION|Impossible de récupérer la collection de propriétés personnalisées pour l'initialisation.|  
|0xC0202025|-1071636443|DTS_E_CANNOTCREATEACCESSOR|Impossible de créer un accesseur OLE DB. Vérifiez que les métadonnées de la colonne sont valides.|  
|0xC0202026|-1071636442|DTS_E_PRIMEOUTPUTCALLEDWITHINVALIDOUTPUTID|PrimeOutput a été appelé avec un ID de sortie non valide de %1!d!.|  
|0xC0202027|-1071636441|DTS_E_INCORRECTSTOCKPROPERTY|La valeur de la propriété « %1 » sur « %2 » n'est pas valide.|  
|0xC0202028|-1071636440|DTS_E_CONNECTIONREQUIREDFORREAD|Une connexion est nécessaire pour lire les données.|  
|0xC020202C|-1071636436|DTS_E_ERRORWHILEREADINGHEADERROWS|Une erreur s'est produite lors de la lecture des lignes d'en-tête.|  
|0xC020202D|-1071636435|DTS_E_DUPLICATECOLUMNNAME|Nom de la colonne « %1 » dupliqué.|  
|0xC0202030|-1071636432|DTS_E_CANNOTGETCOLUMNNAME|Impossible d'obtenir le nom de la colonne avec l'ID %1!d!.|  
|0xC0202031|-1071636431|DTS_E_CANTDIRECTROW|La ligne n'a pas pu être dirigée vers la sortie « %1 » (%2!d!).|  
|0xC020203A|-1071636422|DTS_E_CANNOTCREATEBULKINSERTHREAD|Impossible de créer le thread d'insertion en bloc en raison de l'erreur « %1 ».|  
|0xC020203B|-1071636421|DTS_E_BULKINSERTHREADINITIALIZATIONFAILED|Échec de l'initialisation du thread pour la tâche d'insertion en bloc SSIS.|  
|0xC020203E|-1071636418|DTS_E_BULKINSERTTHREADALREADYRUNNING|Le thread pour la tâche d'insertion en bloc SSIS est déjà en cours d'exécution.|  
|0xC020203F|-1071636417|DTS_E_BULKINSERTTHREADABNORMALCOMPLETION|Le thread pour la tâche d'insertion en bloc SSIS s'est arrêté avec des erreurs ou des avertissements.|  
|0xC0202040|-1071636416|DTS_E_CANNOTGETIROWSETFASTLOAD|Impossible d'ouvrir un ensemble de lignes de chargement rapide pour « %1 ». Vérifiez que cet objet existe dans la base de données.|  
|0xC0202041|-1071636415|DTS_E_CONNECTREQUIREDFORMETADATAVALIDATION|Erreur due à l'absence de connexion. Une connexion est nécessaire pour la validation des métadonnées.|  
|0xC0202042|-1071636414|DTS_E_DESTINATIONTABLENAMENOTPROVIDED|Aucune table de destination n'est spécifiée.|  
|0xC0202043|-1071636413|DTS_E_ICONVERTTYPEUNAVAILABLE|Le fournisseur OLE DB utilisé par l'adaptateur OLE DB ne prend pas en charge IConvertType. Affectez la valeur FALSE à la propriété ValidateColumnMetaData de l'adaptateur.|  
|0xC0202044|-1071636412|DTS_E_OLEDBPROVIDERDATATYPECONVERSIONUNSUPPORTED|Le fournisseur OLE DB utilisé par l'adaptateur OLE DB ne peut pas convertir les types « %1 » et « %2 » pour « %3 ».|  
|0xC0202045|-1071636411|DTS_E_VALIDATECOLUMNMETADATAFAILED|Échec de la validation des métadonnées de la colonne.|  
|0xC0202047|-1071636409|DTS_E_ATTEMPTINGTOINSERTINTOAROWIDCOLUMN|« %1 » est une colonne d'ID de ligne et ne peut pas être incluse dans une opération d'insertion de données.|  
|0xC0202048|-1071636408|DTS_E_ATTEMPTINGTOINSERTINTOAROWVERCOLUMN|Tentative d'insertion dans la colonne de version de ligne « %1 ». Impossible d'effectuer cette opération dans une colonne de version de ligne.|  
|0xC0202049|-1071636407|DTS_E_ATTEMPTINGTOINSERTINTOAREADONLYCOLUMN|Échec lors de l'insertion dans la colonne « %1 » en lecture seule.|  
|0xC020204A|-1071636406|DTS_E_UNABLETORETRIEVECOLUMNINFO|Impossible de récupérer les informations sur la colonne dans la source de données. Vérifiez que la table cible est disponible dans la base de données.|  
|0xC020204B|-1071636405|DTS_E_CANTLOCKBUFFER|Impossible de verrouiller un tampon. La mémoire système est insuffisante ou le gestionnaire de tampons a atteint son quota.|  
|0xC020204C|-1071636404|DTS_E_INVALIDCOMPARISONFLAGS|%1 a une propriété ComparisonFlags qui contient des balises supplémentaires avec la valeur %2!d!.|  
|0xC020204D|-1071636403|DTS_E_COLUMNMETADATAUNAVAILABLEFORVALIDATION|Les métadonnées de la colonne ne sont pas disponibles pour la validation.|  
|0xC0202053|-1071636397|DTS_E_CANNOTWRITETODATAFILE|Impossible d'écrire dans le fichier de données.|  
|0xC0202055|-1071636395|DTS_E_COLUMNDELIMITERNOTFOUND|Le délimiteur de colonne de la colonne « %1 » est introuvable.|  
|0xC0202058|-1071636392|DTS_E_COLUMNPARSEFAILED|Impossible d'analyser la colonne « %1 » dans le fichier de données.|  
|0xC020205A|-1071636390|DTS_E_RAWFILENAMEREQUIRED|Le nom de fichier n'est pas correctement spécifié.  Fournissez le chemin d'accès et le nom au fichier brut, soit directement dans la propriété FileName, soit en spécifiant une variable dans la propriété FileNameVariable.|  
|0xC020205B|-1071636389|DTS_E_RAWFILECANTOPEN|Impossible d'écrire dans le fichier « %1 ». Cette erreur peut se produire lorsque aucune autorisation relative aux fichiers n'est définie, ou lorsque le disque est plein.|  
|0xC020205C|-1071636388|DTS_E_RAWFILECANTBUFFER|Impossible de créer un tampon d'entrées/sorties pour le fichier de sortie. Cette erreur peut se produire lorsque aucune autorisation relative aux fichiers n'est définie, ou lorsque le disque est plein.|  
|0xC020205D|-1071636387|DTS_E_RAWCANTWRITE|Impossible d'écrire %1!d! octets dans le fichier « %2 ». Pour plus d'informations, consultez les messages d'erreur précédents.|  
|0xC020205E|-1071636386|DTS_E_RAWBADHEADER|Métadonnées incorrectes dans l'en-tête du fichier. Le fichier est endommagé ou n'est pas un fichier de données brutes généré par SSIS.|  
|0xC020205F|-1071636385|DTS_E_RAWEXISTSCREATEONCE|Une erreur s'est produite car le fichier de sortie existe déjà et la propriété WriteOption est définie à la valeur Créer une fois. Affectez la valeur Toujours créer à la propriété WriteOption ou supprimez le fichier.|  
|0xC0202060|-1071636384|DTS_E_RAWCANTAPPENDTRUNCATE|Erreur causée par des paramètres de propriété conflictuels. La valeur TRUE est affectée aux propriétés AllowAppend et ForceTruncate. Ces deux propriétés ne peuvent posséder cette valeur TRUE. Attribuez la valeur FALSE à l'une des deux propriétés.|  
|0xC0202061|-1071636383|DTS_E_RAWBADVERSION|La version et les balises du fichier sont incorrectes. Le fichier est endommagé ou n'est pas un fichier de données brutes généré par SSIS.|  
|0xC0202062|-1071636382|DTS_E_RAWVERSIONINCOMPATIBLEAPPEND|Le fichier de sortie a été écrit par une version incompatible et ne peut pas être ajouté. Le format du fichier est peut-être ancien et n'est plus utilisable.|  
|0xC0202064|-1071636380|DTS_E_RAWMETADATAMISMATCH|Impossible d'ajouter le fichier de sortie car aucune colonne dans le fichier existant ne correspond à la colonne « %1 » de l'entrée. L'ancien fichier ne correspond pas au niveau des métadonnées.|  
|0xC0202065|-1071636379|DTS_E_RAWMETADATACOUNTMISMATCH|Impossible d'ajouter le fichier de sortie car le nombre de colonnes dans ce fichier ne correspond pas au nombre de colonnes dans la destination. L'ancien fichier ne correspond pas au niveau des métadonnées.|  
|0xC0202067|-1071636377|DTS_E_ERRORRETRIEVINGCOLUMNCODEPAGE|Une erreur s'est produite lors de la récupération des informations de la page de codes de la colonne.|  
|0xC0202068|-1071636376|DTS_E_RAWCANTREAD|Impossible de lire %1!d! octets dans le fichier « %2 ». L'origine de cette erreur a dû être signalée précédemment.|  
|0xC0202069|-1071636375|DTS_E_RAWUNEXPECTEDEOF|Fin de fichier inattendue lors de la lecture de %1!d! octets dans le fichier « %2 ». Le fichier s'est terminé prématurément en raison d'un format de fichier non valide.|  
|0xC020206A|-1071636374|DTS_E_RAWNOLONGTYPES|La colonne %1 ne peut pas être utilisée. Les adaptateurs bruts ne prennent pas en charge les images, le texte ou les données ntext.|  
|0xC020206B|-1071636373|DTS_E_RAWUNEXPECTEDTYPE|L'adaptateur a rencontré un type de données non reconnu de %1!d!. Cela peut être dû à un fichier d'entrée endommagé (source) ou à un type de tampon non valide (destination).|  
|0xC020206C|-1071636372|DTS_E_RAWSTRINGTOOLONG|Chaîne trop longue. L'adaptateur lit une chaîne de %1!d! or une chaîne de %2!d! octets maximum est attendue au décalage %3!d!. Cela peut indiquer un fichier d'entrée endommagé. Le fichier comporte une longueur de chaîne trop importante pour la colonne tampon.|  
|0xC020206E|-1071636370|DTS_E_RAWSKIPFAILED|L'adaptateur brut a tenté d'ignorer %1!d! octets dans le fichier d'entrée pour la colonne non référencée « %2 » avec l'ID de lignage %3!d!, mais une erreur s'est produite. L'erreur retournée par le système d'exploitation doit avoir été signalée précédemment.|  
|0xC020206F|-1071636369|DTS_E_RAWREADFAILED|L'adaptateur brut a tenté de lire %1!d! octets dans le fichier d'entrée pour la colonne « %2 » avec l'ID de lignage %3!d!, mais une erreur s'est produite. L'erreur retournée par le système d'exploitation doit avoir été signalée précédemment.|  
|0xC0202070|-1071636368|DTS_E_RAWFILENAMEINVALID|La propriété du nom de fichier n'est pas valide. Le nom de fichier est un périphérique ou il contient des caractères non valides.|  
|0xC0202071|-1071636367|DTS_E_BULKINSERTAPIPREPARATIONFAILED|Impossible de préparer l'insertion en bloc SSIS pour l'insertion de données.|  
|0xC0202072|-1071636366|DTS_E_INVALIDDATABASEOBJECTNAME|Le nom d'objet de base de données « %1 » n'est pas valide.|  
|0xC0202073|-1071636365|DTS_E_INVALIDORDERCLAUSE|La clause d'ordre n'est pas valide.|  
|0xC0202074|-1071636364|DTS_E_RAWFILECANTOPENREAD|Impossible de lire le fichier « %1 ». Cette erreur peut se produire lorsque aucune autorisation n'est définie ou si le fichier est introuvable. La raison exacte est indiquée dans le message d'erreur précédent.|  
|0xC0202075|-1071636363|DTS_E_TIMEGENCANTCREATE|Impossible de créer Microsoft.AnalysisServices.TimeDimGenerator.TimeDimGenerator.|  
|0xC0202076|-1071636362|DTS_E_TIMEGENCANTCONFIGURE|Impossible de configurer Microsoft.AnalysisServices.TimeDimGenerator.|  
|0xC0202077|-1071636361|DTS_E_TIMEGENCANTCONVERT|Type de données non pris en charge pour la colonne %1!d!.|  
|0xC0202079|-1071636359|DTS_E_TIMEGENCANTREAD|Échec de la lecture à partir de Microsoft.AnalysisServices.TimeDimGenerator. Code d'erreur : 0x%1!8.8X!.|  
|0xC020207A|-1071636358|DTS_E_TIMEGENCANTREADCOLUMN|La tentative de lecture des données de la colonne « %2!d! » à partir de Microsoft.AnalysisServices.TimeDimGenerator a échoué avec le code d’erreur 0x%2!8.8X!.|  
|0xC020207B|-1071636357|DTS_E_RSTDESTBADVARIABLENAME|La propriété VariableName n'est pas définie sur le nom d'une variable valide. Un nom de variable exécutable est nécessaire.|  
|0xC020207C|-1071636356|DTS_E_RSTDESTRSTCONFIGPROBLEM|Impossible de créer ou de configurer l'objet ADODB.Recordset.|  
|0xC020207D|-1071636355|DTS_E_RSTDESTRSTWRITEPROBLEM|Une erreur s'est produite lors de l'écriture dans l'objet ADODB.Recordset.|  
|0xC020207E|-1071636354|DTS_E_FILENAMEINVALID|Le nom de fichier n'est pas valide. Le nom de fichier est un périphérique ou il contient des caractères non valides.|  
|0xC020207F|-1071636353|DTS_E_FILENAMEINVALIDWITHPARAM|Le nom de fichier « %1 » n'est pas valide. Le nom de fichier est un périphérique ou il contient des caractères non valides.|  
|0xC0202080|-1071636352|DTS_E_CMDDESTNOPARAMS|Impossible de récupérer les descriptions des colonnes de destination à partir des paramètres de la commande SQL.|  
|0xC0202081|-1071636351|DTS_E_CMDDESTNOTBOUND|Les paramètres ne sont pas liés. Tous les paramètres de la commande SQL doivent être liés aux colonnes d'entrée.|  
|0xC0202082|-1071636350|DTS_E_TXPIVOTBADUSAGE|La valeur PivotUsage de la colonne d'entrée « %1 » (%2!d!) n'est pas valide.|  
|0xC0202083|-1071636349|DTS_E_TXPIVOTTOOMANYPIVOTKEYS|Trop de clés de tableau croisé dynamique. Une seule colonne d'entrée peut être utilisée en tant que clé de tableau croisé dynamique.|  
|0xC0202084|-1071636348|DTS_E_TXPIVOTNOPIVOTKEY|Aucune clé de tableau croisé dynamique n'a été trouvée. Une colonne d'entrée doit être utilisée en tant que clé de tableau croisé dynamique.|  
|0xC0202085|-1071636347|DTS_E_TXPIVOTINPUTALREADYMAPPED|Plusieurs colonnes de sortie (telles que « %1 » (%2!d!)) sont mappées sur la colonne d'entrée « %3 » (%4!d!).|  
|0xC0202086|-1071636346|DTS_E_TXPIVOTCANTMAPPIVOTKEY|Impossible de mapper la colonne de sortie « %1 » (%2!d!) sur la colonne d'entrée PivotKey.|  
|0xC0202087|-1071636345|DTS_E_TXPIVOTCANTMAPPINGNOTFOUND|La colonne de sortie « %1 » (%2!d!) a un SourceColumn %3!d! qui n'est pas un ID de lignage de colonne d'entrée valide.|  
|0xC0202088|-1071636344|DTS_E_TXPIVOTEMPTYPIVOTKEYVALUE|La colonne de sortie « %1 » (%2!d!) est mappée à une colonne d'entrée de valeur croisée dynamique, mais la valeur de la propriété PivotKeyValue est manquante.|  
|0xC0202089|-1071636343|DTS_E_TXPIVOTDUPLICATEPIVOTKEYVALUE|La colonne de sortie « %1 » (%2!d!) est mappée à une colonne d'entrée de valeur croisée dynamique avec une valeur de propriété PivotKeyValue non unique.|  
|0xC020208A|-1071636342|DTS_E_TXPIVOTOUTPUTNOTMAPPED|La colonne d'entrée « %1 » (%2!d!) n'est pas mappée à une colonne de sortie.|  
|0xC020208B|-1071636341|DTS_E_TXPIVOTCANTCOMPARESETKEYS|Échec de la comparaison des valeurs de clés définies.|  
|0xC020208D|-1071636339|DTS_E_TXPIVOTNOBLOB|La colonne d'entrée « %1 » (%2!d!) ne peut pas être utilisée en tant que clé définie, clé de tableau croisé dynamique ou valeur de tableau croisé dynamique, car elle contient des données de type BLOB.|  
|0xC020208E|-1071636338|DTS_E_TXPIVOTBADOUTPUTTYPE|Type de sortie incorrect. La colonne de sortie « %1 » (%2!d!) doit avoir les mêmes types de données et métadonnées que la colonne d'entrée à laquelle elle est mappée.|  
|0xC020208F|-1071636337|DTS_E_TXPIVOTPROCESSERROR|Échec de la tentative pour ajouter un tableau croisé dynamique des enregistrements sources.|  
|0xC0202090|-1071636336|DTS_E_TXPIVOTBADPIVOTKEYVALUE|La valeur de clé de tableau croisé dynamique « %1 » n'est pas valide.|  
|0xC0202091|-1071636335|DTS_E_ERRORWHILESKIPPINGDATAROWS|Une erreur s'est produite lorsque les lignes de données ont été ignorées.|  
|0xC0202092|-1071636334|DTS_E_ERRORWHILEREADINGDATAROWS|Une erreur s'est produite lors du traitement du fichier « %1 » sur la ligne de données %2!I64d!.|  
|0xC0202093|-1071636333|DTS_E_FAILEDTOINITIALIZEFLATFILEPARSER|Une erreur s'est produite lors de l'initialisation de l'analyseur de fichier plat.|  
|0xC0202094|-1071636332|DTS_E_UNABLETORETRIEVECOLUMNINFOFROMFLATFILECONNECTIONMANAGER|Impossible d'extraire les informations de colonne à partir du gestionnaire de connexions de fichier plat.|  
|0xC0202095|-1071636331|DTS_E_FAILEDTOWRITEOUTCOLUMNNAME|Échec de l'écriture du nom de colonne pour la colonne « %1 ».|  
|0xC0202096|-1071636330|DTS_E_INVALIDFLATFILECOLUMNTYPE|Le type de colonne de la colonne « %1 » est incorrect. Il s'agit du type « %2 ». Seuls les types « %3 » ou « %4 » sont autorisés.|  
|0xC0202097|-1071636329|DTS_E_DISKIOBUFFEROVERFLOW|Échec de la tentative d'écriture de données de %1!d! octets dans l'E/S disque. Le tampon d'E/S disque dispose de %2!d! octets disponibles.|  
|0xC0202098|-1071636328|DTS_E_FAILEDTOWRITEOUTHEADER|Une erreur s'est produite lors de l'écriture de l'en-tête de fichier.|  
|0xC0202099|-1071636327|DTS_E_FAILEDTOGETFILESIZE|Une erreur s'est produite lors de l'obtention de la taille du fichier « %1 ».|  
|0xC020209A|-1071636326|DTS_E_FAILEDTOSETFILEPOINTER|Une erreur s'est produite lors de la définition du pointeur de fichier du fichier « %1 ».|  
|0xC020209B|-1071636325|DTS_E_UNABLETOSETUPDISKIOBUFFER|Une erreur s'est produite lors de la définition du tampon d'E/S disque.|  
|0xC020209C|-1071636324|DTS_E_COLUMNDATAOVERFLOWDISKIOBUFFER|Les données de la colonne « %1 » ont dépassé le tampon d'E/S disque.|  
|0xC020209D|-1071636323|DTS_E_DISKIOFAILED|Une erreur d'E/S disque inattendue s'est produite lors de la lecture du fichier.|  
|0xC020209E|-1071636322|DTS_E_DISKIOTIMEDOUT|Un délai d'expiration d'E/S disque s'est produit lors de la lecture du fichier.|  
|0xC020209F|-1071636321|DTS_E_INPUTSNOTREADONLY|Le type d'utilisation des colonnes d'entrée pour cette transformation ne peut être en lecture/écriture. Modifiez ce type d'utilisation pour qu'il soit en lecture seule.|  
|0xC02020A0|-1071636320|DTS_E_CANNOTCOPYORCONVERTFLATFILEDATA|Impossible de copier ou de convertir les données de fichier plat pour la colonne « %1 ».|  
|0xC02020A1|-1071636319|DTS_E_FAILEDCOLUMNDATACONVERSIONSTATUS|Échec de la conversion de données. La conversion de données de la colonne « %1 » a retourné la valeur d'état %2!d! et le texte d'état « %3 ».|  
|0xC02020A2|-1071636318|DTS_E_VARIABLESCOLLECTIONUNAVAILABLE|La collection Variables n'est pas disponible.|  
|0xC02020A3|-1071636317|DTS_E_TXUNPIVOTDUPLICATEPIVOTKEYVALUE|Valeur PivotKeyValue dupliquée. La colonne d'entrée « %1 » (%2!d!) est mappée à une colonne de sortie avec une valeur croisée dynamique et elle possède une valeur PivotKeyValue non unique.|  
|0xC02020A4|-1071636316|DTS_E_TXUNPIVOTNOUNPIVOTDESTINATION|Destination Unpivot introuvable. Au moins une colonne d'entrée doit être mappée avec une PivotKeyValue vers une DestinationColumn dans la sortie.|  
|0xC02020A5|-1071636315|DTS_E_TXUNPIVOTBADKEYLIST|PivotKeyValue non valide. Dans une transformation UnPivot avec plus d'une DestinationColumn non croisée dynamique, l'ensemble des valeurs PivotKeyValues par destination doit correspondre exactement.|  
|0xC02020A6|-1071636314|DTS_E_TXUNPIVOTBADUNPIVOTMETADATA|Métadonnées UnPivot incorrectes. Dans une transformation UnPivot, toutes les colonnes d'entrée avec PivotKeyValue défini et pointant vers la même DestinationColumn doivent posséder des métadonnées qui correspondent exactement à DestinationColumn.|  
|0xC02020A7|-1071636313|DTS_E_TXPIVOTBADPIVOTKEYCONVERT|Impossible de convertir la valeur de clé du tableau croisé dynamique « %1 » selon le type de données de la colonne clé de tableau croisé dynamique.|  
|0xC02020A8|-1071636312|DTS_E_TXUNPIVOTTOOMANYPIVOTKEYS|Trop de clés de tableau croisé dynamique spécifiées. Une seule colonne de sortie peut être utilisée en tant que clé de tableau croisé dynamique.|  
|0xC02020A9|-1071636311|DTS_E_TXUNPIVOTUNMAPPEDOUTPUT|La colonne de sortie « %1 » (%2!d!) n'est mappée à aucune propriété DestinationColumn de la colonne d'entrée.|  
|0xC02020AA|-1071636310|DTS_E_TXUNPIVOTNOPIVOT|Aucune colonne de sortie n'est indiquée comme PivotKey.|  
|0xC02020AB|-1071636309|DTS_E_TXUNPIVOTNOTINPUTMAP|La colonne d'entrée « %1 » (%2!d!) possède une valeur de propriété DestinationColumn qui ne fait pas référence à un LineageID de colonne de sortie valide.|  
|0xC02020AC|-1071636308|DTS_E_TXUNPIVOTDUPLICATEDESTINATION|Erreur de destination dupliquée. Plus d'une colonne d'entrée non croisée dynamique est mappée à la même colonne de sortie de destination.|  
|0xC02020AD|-1071636307|DTS_E_TOTALINPUTCOLSCANNOTBEZERO|Aucune colonne d'entrée trouvée. Une colonne d'entrée au moins doit être mappée à une colonne de sortie.|  
|0xC02020AE|-1071636306|DTS_E_TXMERGEJOINMUSTHAVESAMENUMBEROFINPUTANDOUTPUTCOLS|Le nombre de colonnes d'entrée et de sortie ne correspond pas. Le nombre total de colonnes d'entrée sur toutes les entrées doit être identique au nombre total de colonnes de sortie.|  
|0xC02020AF|-1071636305|DTS_E_INPUTMUSTBESORTED|Entrée non triée. « %1 » doit être trié.|  
|0xC02020B0|-1071636304|DTS_E_TXMERGEJOININVALIDJOINTYPE|La propriété personnalisée JoinType pour %1 contient une valeur %2!ld! qui n'est pas valide. Les valeurs valides sont 0 (complet), 1 (gauche) ou 2 (interne).|  
|0xC02020B1|-1071636303|DTS_E_TXMERGEJOININVALIDNUMKEYCOLS|La valeur de NumKeyColumns n'est pas valide. Dans %1, la valeur de la propriété personnalisée NumKeyColumns doit être comprise entre 1 et %2!lu!.|  
|0xC02020B2|-1071636302|DTS_E_NOKEYCOLS|Aucune colonne clé trouvée. %1 doit posséder au moins une colonne pour laquelle SortKeyPosition diffère de zéro.|  
|0xC02020B3|-1071636301|DTS_E_TXMERGEJOINNOTENOUGHKEYCOLS|Nombre de colonnes clés insuffisant. %1 doit posséder au moins %2!ld! colonnes avec des valeurs SortKeyPosition différentes de zéro.|  
|0xC02020B4|-1071636300|DTS_E_TXMERGEJOINDATATYPEMISMATCH|Une erreur de correspondance de types de données s'est produite. Les types de données des colonnes possédant la valeur %1!ld! pour SortKeyPosition ne correspondent pas.|  
|0xC02020B5|-1071636299|DTS_E_TXMERGEJOININVALIDSORTKEYPOS|La colonne avec la valeur %1!ld! pour SortKeyPosition n’est pas valide. Cette valeur doit être %2!ld!.|  
|0xC02020B6|-1071636298|DTS_E_TXMERGEJOINSORTDIRECTIONMISMATCH|Sens de tri non correspondant. Les sens de tri des colonnes possédant la valeur %1!ld! pour SortKeyPosition ne correspondent pas.|  
|0xC02020B7|-1071636297|DTS_E_TXMERGEJOINOUTPUTCOLMUSTHAVEASSOCIATEDINPUTCOL|Colonne manquante. %1 doit posséder une colonne d'entrée associée.|  
|0xC02020B8|-1071636296|DTS_E_TXMERGEJOINREADONLYINPUTCOLSWITHNOOUTPUTCOL|Les colonnes d'entrée doivent posséder des colonnes de sortie. Il existe des colonnes d'entrée avec un type d'utilisation en lecture seule sans colonnes de sortie associées.|  
|0xC02020B9|-1071636295|DTS_E_TXMERGEJOINNONSTRINGCOMPARISONFLAGSNOTZERO|Les indicateurs de comparaison n'équivalent pas à zéro. Ces indicateurs pour les colonnes qui ne sont pas des chaînes doivent correspondre à zéro.|  
|0xC02020BA|-1071636294|DTS_E_TXMERGEJOINCOMPARISONFLAGSMISMATCH|Les indicateurs de comparaison pour les colonnes dont SortKeyPosition possède la valeur %1!ld! ne correspondent pas.|  
|0xC02020BB|-1071636293|DTS_E_TXPIVOTBADPIVOTKEYVALUENOSTRING|Valeur de clé de tableau croisé dynamique non reconnue.|  
|0xC02020BC|-1071636292|DTS_E_TXLINEAGEINVALIDLINEAGEITEM|La valeur de l'élément de lignage %1!ld! n’est pas valide. La plage valide se situe entre %2!ld! et %3!ld!.|  
|0xC02020BD|-1071636291|DTS_E_CANNOTHAVEANYINPUTCOLUMNS|Colonnes d'entrée non autorisées. Le nombre de colonnes d'entrée doit être zéro.|  
|0xC02020BE|-1071636290|DTS_E_TXLINEAGEDATATYPEMISMATCH|Le type de données de « %1 » n'est pas valide pour l'élément de lignage spécifié.|  
|0xC02020BF|-1071636289|DTS_E_TXLINEAGEINVALIDLENGTH|La longueur de « %1 » n'est pas valide pour l'élément de lignage spécifié.|  
|0xC02020C1|-1071636287|DTS_E_METADATAMISMATCHWITHOUTPUTCOLUMN|Les métadonnées de « %1 » ne correspondent pas aux métadonnées de la colonne de sortie associée.|  
|0xC02020C3|-1071636285|DTS_E_TXMERGESORTKEYPOSMISMATCH|Il existe des colonnes de sortie dont les valeurs de SortKeyPosition ne correspondent pas aux valeurs SortKeyPosition des colonnes d'entrée associées.|  
|0xC02020C4|-1071636284|DTS_E_ADDROWTOBUFFERFAILED|Échec de la tentative d'ajout d'une ligne au tampon de tâche de flux de données : code d'erreur 0x%1!8.8X!.|  
|0xC02020C5|-1071636283|DTS_E_DATACONVERSIONFAILED|Échec de la conversion de données lors de la conversion de la colonne « %1 » (%2!d!) en colonne « %3 » (%4!d!).  Cette conversion a retourné la valeur d'état %5!d! et le texte d'état « %6 ».|  
|0xC02020C6|-1071636282|DTS_E_FAILEDTOALLOCATEROWHANDLEBUFFER|Échec de la tentative d'allocation d'un tampon de descripteur de ligne : code d'erreur 0x%1!8.8X!.|  
|0xC02020C7|-1071636281|DTS_E_FAILEDTOSENDROWTOSQLSERVER|Échec de la tentative d'envoi d'une ligne vers SQL Server : code d'erreur 0x%1!8.8X!.|  
|0xC02020C8|-1071636280|DTS_E_FAILEDTOPREPAREBUFFERSTATUS|Échec de la tentative de préparation de l'état de tampon : code d'erreur 0x%1!8.8X!.|  
|0xC02020C9|-1071636279|DTS_E_FAILEDTOBUFFERROWSTARTS|Échec de la tentative de récupération de la ligne de début du tampon : code d'erreur 0x%1!8.8X!.|  
|0xC02020CA|-1071636278|DTS_E_BULKINSERTTHREADTERMINATED|Le thread de l'insertion en bloc SSIS n'est plus en cours d'exécution.  Plus aucune ligne ne peut être insérée. Essayez d'augmenter le délai d'expiration du thread d'insertion en bloc.|  
|0xC02020CB|-1071636277|DTS_E_RAWTOOMANYCOLUMNS|Fichier source non valide. Le fichier source retourne un nombre supérieur à 131072 colonnes. Cela se produit généralement lorsque le fichier source n'est pas produit par la destination du fichier brut.|  
|0xC02020CC|-1071636276|DTS_E_TXUNIONALL_EXTRADANGLINGINPUT|%1 est une entrée détachée supplémentaire qui sera supprimée.|  
|0xC02020CD|-1071636275|DTS_E_TXUNIONALL_NONDANGLINGUNATTACHEDINPUT|%1 n'est pas attaché mais n'est pas indiqué comme étant en suspens.  Il sera indiqué comme étant en suspens.|  
|0xC02020CF|-1071636273|DTS_E_TXPIVOTRUNTIMEDUPLICATEPIVOTKEYVALUE|Valeur de clé de tableau croisé dynamique « %1 » dupliquée.|  
|0xC02020D0|-1071636272|DTS_E_TXPIVOTRUNTIMEDUPLICATEPIVOTKEYVALUENOSTRING|Valeur de clé de tableau croisé dynamique dupliquée.|  
|0xC02020D1|-1071636271|DTS_E_FAILEDTOGETCOMPONENTLOCALEID|Erreur lors de la récupération de l'ID de paramètre régional du composant. Code d'erreur : 0x%1!8.8X!.|  
|0xC02020D2|-1071636270|DTS_E_MISMATCHCOMPONENTCONNECTIONMANAGERLOCALEID|Erreur de correspondance entre les ID de paramètres régionaux. L'ID de paramètre régional du composant (%1!d!) ne correspond pas à l'ID de paramètre régional du gestionnaire de connexions (%2!d!).|  
|0xC02020D3|-1071636269|DTS_E_LOCALEIDNOTSET|L'ID de paramètre régional du composant n'a pas été défini. Les adaptateurs de fichier plat doivent posséder un ID de paramètre régional défini sur le gestionnaire de connexions de fichier plat.|  
|0xC02020D4|-1071636268|DTS_E_RAWBYTESTOOLONG|Le champ binaire est trop important. L'adaptateur a tenté de lire un champ binaire de %1!d! octets de long, mais il attendait un champ d'une taille maximale de %2!d! octets au décalage %3!d!. Cela se produit généralement lorsque le fichier d'entrée n'est pas valide. Ce fichier contient une longueur de chaîne trop importante pour la colonne de tampon.|  
|0xC02020D5|-1071636267|DTS_E_TXSAMPLINGINVALIDPCT|Pourcentage %2!ld! non valide pour la propriété « %1 ». Il doit être compris entre 0 et 100.|  
|0xC02020D6|-1071636266|DTS_E_TXSAMPLINGINVALIDROWS|Le nombre de lignes %2!ld! n'est pas valide pour la propriété « %1 ». Il doit être supérieur à 0.|  
|0xC02020D7|-1071636265|DTS_E_RAWSTRINGINPUTTOOLONG|Une écriture de chaîne de %1!I64d! de long a été demandée à l'adaptateur, mais la longueur de l'ensemble des données doit être inférieure à 4 294 967 295 octets.|  
|0xC02020D9|-1071636263|DTS_E_ATLEASTONEINPUTMUSTBEMAPPEDTOOUTPUT|Aucune entrée n'a été mappée à une sortie. « %1 » doit posséder au moins une colonne d'entrée mappée à une colonne de sortie.|  
|0xC02020DB|-1071636261|DTS_E_CANNOTCONVERTDATATYPESWITHDIFFERENTCODEPAGES|La conversion de « %1 » avec la page de codes %2!d! en « %3 » avec la page de codes %4!d! n'est pas prise en charge.|  
|0xC02020DC|-1071636260|DTS_E_COLUMNNOTMAPPEDTOEXTERNALMETADATACOLUMN|Mappage de la colonne de métadonnées externe pour %1 non valide.  L'ID de cette colonne ne peut pas être égal à zéro.|  
|0xC02020DD|-1071636259|DTS_E_COLUMNMAPPEDTONONEXISTENTEXTERNALMETADATACOLUMN|Le %1 est mappé à une colonne de métadonnées externe inexistante.|  
|0xC02020E5|-1071636251|DTS_E_UNABLETOWRITELOBDATATOBUFFER|Échec de l'écriture des données d'objet longues de type DT_TEXT, DT_NTEXT ou DT_IMAGE dans le tampon de la tâche de flux de données de la colonne « %1 ».|  
|0xC02020E8|-1071636248|DTS_E_CANNOTGETIROWSET|Échec de l'ouverture d'un ensemble de lignes pour « %1 ». Vérifiez que cet objet existe dans la base de données.|  
|0xC02020E9|-1071636247|DTS_E_VARIABLEACCESSFAILED|Échec de l'accès à la variable « %1 » : code d'erreur 0x%2!8.8X!.|  
|0xC02020EA|-1071636246|DTS_E_CONNECTIONMANAGERNOTFOUND|Le gestionnaire de connexions « %1 » est introuvable. Un composant n'a pas réussi à le trouver dans la collection Connections.|  
|0xC02020EB|-1071636245|DTS_E_VERSIONUPGRADEFAILED|La mise à niveau depuis la version « %1 » vers la version %2!d! a échoué.|  
|0xC02020EC|-1071636244|DTS_E_RSTDESTBIGBLOB|Une valeur dans une colonne d'entrée est trop importante pour être stockée dans l'objet ADODB.Recordset.|  
|0xC02020ED|-1071636243|DTS_E_CANNOTCONVERTBETWEENUNICODEANDNONUNICODESTRINGCOLUMNS|Les colonnes « %1 » et « %2 » ne peuvent pas effectuer des conversions entre des types de données de chaîne Unicode et non-Unicode.|  
|0xC02020EE|-1071636242|DTS_E_ROWCOUNTBADVARIABLENAME|La variable « %1 » spécifiée par la propriété VariableName n'est pas une variable valide. Nom de variable valide requis pour l'écriture.|  
|0xC02020EF|-1071636241|DTS_E_ROWCOUNTBADVARIABLETYPE|La variable « %1 » spécifiée par la propriété VariableName n'est pas un entier. Modifiez cette variable pour lui attribuer le type VT_I4, VT_UI4, VT_I8 ou VT_UI8.|  
|0xC02020F0|-1071636240|DTS_E_NOCOLUMNADVANCETHROUGHFILE|Aucune colonne spécifiée pour permettre la progression du composant à travers le fichier.|  
|0xC02020F1|-1071636239|DTS_E_MERGEJOINSORTEDOUTPUTHASNOSORTKEYPOSITIONS|La propriété IsSorted de « %1 » possède la valeur TRUE, mais la propriété SortKeyPosition de toutes les colonnes de sortie équivaut à zéro. Modifiez la propriété IsSorted en lui attribuant la valeur FALSE, ou sélectionnez au moins une colonne de sortie qui contienne une valeur SortKeyPosition différente de zéro.|  
|0xC02020F2|-1071636238|DTS_E_METADATAMISMATCHWITHINPUTCOLUMN|Les métadonnées « %1 » ne correspondent pas aux métadonnées de la colonne d'entrée.|  
|0xC02020F3|-1071636237|DTS_E_RSTDESTBADVARIABLE|Impossible de localiser, de verrouiller ou de définir la valeur de la variable spécifiée.|  
|0xC02020F4|-1071636236|DTS_E_CANTPROCESSCOLUMNTYPECODEPAGE|Impossible de traiter la colonne « %1 » car plusieurs pages de codes (%2!d! et %3!d!) ont été spécifiées pour celle-ci.|  
|0xC02020F5|-1071636235|DTS_E_CANTINSERTCOLUMNTYPE|Impossible d'insérer la colonne « %1 » car la conversion entre les types %2 et %3 n'est pas prise en charge.|  
|0xC02020F6|-1071636234|DTS_E_CANNOTCONVERTBETWEENUNICODEANDNONUNICODESTRINGCOLUMN|Impossible de convertir la colonne « %1 » en raison de types de données de chaîne Unicode et non Unicode.|  
|0xC02020F8|-1071636232|DTS_E_COULDNOTFINDINPUTBUFFERCOLUMNBYLINEAGE|%1 ne trouve pas la colonne qui contient le LineageID %2!ld! dans son tampon d'entrée.|  
|0xC02020F9|-1071636231|DTS_E_COULDNOTGETCOLUMNINFOFORINPUTBUFFER|%1 ne peut obtenir les informations sur la colonne %2!lu! à partir de son tampon d'entrée.|  
|0xC02020FA|-1071636230|DTS_E_COULDNOTGETCOLUMNINFOFORCOPYBUFFER|%1 ne peut pas obtenir les informations sur la colonne %2!lu! à partir de sa mémoire tampon de copie.|  
|0xC02020FB|-1071636229|DTS_E_COULDNOTREGISTERCOPYBUFFER|%1 ne peut pas inscrire un type de tampon pour son tampon de copie.|  
|0xC02020FC|-1071636228|DTS_E_COULDNOTCREATECOPYBUFFER|%1 ne peut créer pas un tampon dans lequel copier ses données pour le tri.|  
|0xC02020FD|-1071636227|DTS_E_DATAREADERDESTREADFAILED|Le client DataReader n'a pas pu appeler Read ou a fermé DataReader.|  
|0xC02020FE|-1071636226|DTS_E_NOSCHEMAINFOFOUND|La commande SQL n'a retourné aucune information sur les colonnes.|  
|0xC02020FF|-1071636225|DTS_E_GETSCHEMATABLEFAILED|%1 n'a pas pu récupérer d'informations sur les colonnes pour la commande SQL. L'erreur suivante s'est produite : %2|  
|0xC0202100|-1071636224|DTS_E_SOURCETABLENAMENOTPROVIDED|Aucun nom de table source n'a été spécifié.|  
|0xC0203110|-1071632112|DTS_E_CACHE_INVALID_INDEXPOS|La position d'index du cache, %1!d!, n'est pas valide. Pour les colonnes autres que les colonnes d'index, la position d'index doit être égale à 0. Pour les colonnes d'index, la position d'index doit être un nombre séquentiel positif.|  
|0xC0203111|-1071632111|DTS_E_CACHE_DUPLICATE_INDEXPOS|La position d'index, %1!d!, est un doublon. Pour les colonnes autres que les colonnes d'index, la position d'index doit être égale à 0. Pour les colonnes d'index, la position d'index doit être un nombre séquentiel positif.|  
|0xC0203112|-1071632110|DTS_E_CACHE_TOO_FEW_INDEX_COLUMNS|Au moins une colonne d'index doit être spécifiée pour le gestionnaire de connexions du cache. Pour spécifier une colonne d'index, définissez la propriété Index Position de la colonne du cache.|  
|0xC0203113|-1071632109|DTS_E_CACHE_INDEXPOS_NOT_CONTINUOUS|Les positions d'index du cache doivent être contiguës. Pour les colonnes autres que les colonnes d'index, la position d'index doit être égale à 0. Pour les colonnes d'index, la position d'index doit être un nombre séquentiel positif.|  
|0xC0204000|-1071628288|DTS_E_PROPERTYNOTSUPPORTED|Impossible de définir la propriété « %1 » sur « %2 ». La propriété en cours de définition n'est pas prise en charge sur l'objet spécifié. Vérifiez le nom, la casse et l'orthographe de la propriété.|  
|0xC0204002|-1071628286|DTS_E_CANTCHANGEPROPERTYTYPE|Impossible de modifier le type de la propriété par rapport au type défini par le composant.|  
|0xC0204003|-1071628285|DTS_E_CANTADDOUTPUTID|Échec de l'ID de sortie %1!d! au cours de l'insertion. La nouvelle sortie n'a pas été créée.|  
|0xC0204004|-1071628284|DTS_E_CANTDELETEOUTPUTID|Impossible de supprimer l'ID de sortie %1!d! de la collection de sortie.  Cet ID n'est peut-être pas valide, ou il représente la sortie par défaut ou la sortie d'erreur.|  
|0xC0204006|-1071628282|DTS_E_FAILEDTOSETPROPERTY|Échec de la définition de la propriété « %1 » sur « %2 ».|  
|0xC0204007|-1071628281|DTS_E_FAILEDTOSETOUTPUTCOLUMNTYPE|Échec de la définition du type de %1 en type : « %2 », longueur : %3!d!, précision : %4!d!, échelle : %5!d!, page de codes : %6!d!.|  
|0xC0204008|-1071628280|DTS_E_MORETHANONEERROROUTPUTFOUND|Plus d'une sortie d'erreur a été trouvée sur le composant, or il ne peut y en avoir qu'une seule.|  
|0xC020400A|-1071628278|DTS_E_CANTSETOUTPUTCOLUMNPROPERTY|Impossible de définir la propriété sur une colonne de sortie.|  
|0xC020400B|-1071628277|DTS_E_CANTMODIFYERROROUTPUTCOLUMNDATATYPE|Impossible de modifier le type de données « %1 » dans l'erreur « %2 ».|  
|0xC020400E|-1071628274|DTS_E_CANONLYSETISSORTEDONSOURCE|La propriété IsSorted de « %1 » ne peut pas posséder la valeur TRUE car il ne s'agit pas d'une sortie source. Une sortie source possède une valeur SynchronousInputID égale à zéro.|  
|0xC020400F|-1071628273|DTS_E_CANONLYSETSORTKEYONSOURCE|« %1 » ne peut pas posséder une propriété SortKeyPosition avec une valeur autre que zéro, car « %2 » n'est pas une sortie source. La colonne de sortie « nomcol » ne peut posséder une propriété SortKeyPosition avec une valeur différente de zéro, car sa sortie « nomsortie » (ID) n'est pas une sortie source.|  
|0xC0204010|-1071628272|DTS_E_CANONLYSETCOMPFLAGSONSOURCE|La propriété ComparisonFlags ne peut pas posséder une valeur différente de zéro pour « %1 », car « %2 » n'est pas une sortie source. La colonne de sortie « nomcol » (ID) ne peut posséder une valeur autre que zéro pour la propriété ComparisonFlags, car sa sortie « nomsortie » (ID) n'est pas une sortie source.|  
|0xC0204011|-1071628271|DTS_E_NONSTRINGCOMPARISONFLAGSNOTZERO|Les indicateurs de comparaison de « %1 » doivent être égaux à zéro car ils ne sont pas de type chaîne. ComparisonFlags peut uniquement posséder une valeur différente de zéro pour les colonnes de type chaîne.|  
|0xC0204012|-1071628270|DTS_E_COMPFLAGSONLYONSORTCOL|« %1 » ne peut pas comporter une propriété ComparisonFlags définie à une valeur différente de zéro, car sa propriété SortKeyPosition a la valeur zéro. La propriété ComparisonFlags d'une colonne de sortie peut être différente de zéro uniquement si la valeur de SortKeyPosition diffère également de zéro.|  
|0xC0204013|-1071628269|DTS_E_READONLYSTOCKPROPERTY|Propriété en lecture seule.|  
|0xC0204014|-1071628268|DTS_E_INVALIDDATATYPE|%1 possède une valeur de type de données non valide (%2!ld!) définie.|  
|0xC0204015|-1071628267|DTS_E_CODEPAGEREQUIRED|« %1 » nécessite la définition d'une page de codes mais la valeur transmise était zéro.|  
|0xC0204016|-1071628266|DTS_E_INVALIDSTRINGLENGTH|La longueur de « %1 » n'est pas valide. Cette longueur doit être comprise entre %2!ld! et %3!ld!.|  
|0xC0204017|-1071628265|DTS_E_INVALIDSCALE|L'échelle de « %1 » n'est pas valide. Cette échelle doit être comprise entre %2!ld! et %3!ld!.|  
|0xC0204018|-1071628264|DTS_E_INVALIDPRECISION|La précision de « %1 » n'est pas valide. Cette précision doit être comprise entre %2!ld! et %3!ld!.|  
|0xC0204019|-1071628263|DTS_E_PROPVALUEIGNORED|« %1 » possède une valeur définie pour la longueur, la précision, l'échelle ou la page de codes différente de zéro, mais le type de données requiert une valeur zéro.|  
|0xC020401A|-1071628262|DTS_E_CANTSETOUTPUTCOLUMNDATATYPEPROPERTIES|%1 ne permet pas de définir les propriétés de type de données de la colonne de sortie.|  
|0xC020401B|-1071628261|DTS_E_INVALIDDATATYPEFORERRORCOLUMNS|« %1 » contient un type de données non valide. « %1 » est une colonne d'erreur spéciale et le seul type de données valide est DT_I4.|  
|0xC020401C|-1071628260|DTS_E_NOERRORDESCFORCOMPONENT|Le composant ne fournit pas les descriptions des codes d'erreur.|  
|0xC020401D|-1071628259|DTS_E_UNRECOGNIZEDERRORCODE|Le code d'erreur spécifié n'est pas associé à ce composant.|  
|0xC020401F|-1071628257|DTS_E_TRUNCATIONTRIGGEREDREDIRECTION|Une troncation a causé la redirection d'une ligne, en fonction des paramètres de disposition de troncation.|  
|0xC0204020|-1071628256|DTS_E_CANTSETUSAGETYPETOREADWRITE|« %1 » ne peut modifier la colonne avec l'ID de lignage %2!d! en lecture/écriture, car ce type d'utilisation n'est pas autorisé sur cette colonne. Une tentative de modification du type d'utilisation d'une colonne d'entrée, UT_READWRITE, a été effectuée, mais elle n'est pas prise en charge sur ce composant.|  
|0xC0204023|-1071628253|DTS_E_CANTSETUSAGETYPE|%1 a oublié l'utilisation requise de la colonne d'entrée avec l'ID de lignage %2!d!.|  
|0xC0204024|-1071628252|DTS_E_FAILEDTOSETUSAGETYPE|« %1 » n'a pu apporter la modification requise à la colonne d'entrée avec l'ID de lignage %2!d!. La requête a échoué et retourné le code d'erreur : 0x%3!8.8X!. L'erreur spécifiée s'est produite lors de la tentative de définition du type d'utilisation d'une colonne d'entrée.|  
|0xC0204025|-1071628251|DTS_E_FAILEDTOSETOUTPUTCOLUMNDATATYPEPROPERTIES|Échec de la tentative de définition des propriétés de type de données sur « %1 ». Cette erreur s'est produite lors de la tentative de définition d'une ou plusieurs des propriétés de type de données de la colonne de sortie.|  
|0xC0204026|-1071628250|DTS_E_UNABLETORETRIEVEMETADATA|Impossible d'extraire les métadonnées de « %1 ». Vérifiez que le nom d'objet est correct et que l'objet existe.|  
|0xC0204027|-1071628249|DTS_E_CANNOTMAPOUTPUTCOLUMN|Impossible de mapper la colonne de sortie à une colonne de métadonnées externe.|  
|0xC0204028|-1071628248|DTS_E_UNSUPPORTEDVARIABLETYPE|La variable %1 doit être de type « %2 ».|  
|0xC020402A|-1071628246|DTS_E_CANTSETEXTERNALMETADATACOLUMNDATATYPEPROPERTIES|%1 n'autorise pas la définition des propriétés de type de données de la colonne de métadonnées externe.|  
|0xC020402B|-1071628245|DTS_E_IDNOTINPUTNOROUTPUT|L'ID %1!lu! n'est ni un ID d'entrée, ni un ID de sortie. L'ID spécifié doit être l'ID d'entrée ou l'ID de sortie auquel est associée la collection de métadonnées externe.|  
|0xC020402C|-1071628244|DTS_E_METADATACOLLECTIONNOTUSED|La collection de métadonnées externe sur « %1 » est indiquée comme étant inutilisée. Par conséquent, aucune opération ne peut être effectuée sur celle-ci.|  
|0xC020402D|-1071628243|DTS_E_NOBUFFERTYPEONSYNCOUTPUT|La sortie %1 est une sortie synchrone et le type de mémoire tampon ne peut pas être extrait pour une sortie synchrone.|  
|0xC0207000|-1071616000|DTS_E_INPUTCOLUMNUSAGETYPENOTREADONLY|La colonne d'entrée « %1 » doit être en lecture seule. La colonne d'entrée possède un type d'utilisation autre que lecture seule, ce qui n'est pas autorisé.|  
|0xC0207001|-1071615999|DTS_E_MISSINGCUSTOMPROPERTY|« %1 » ne possède pas la propriété requise « %2 ». Cet objet doit posséder la propriété personnalisée spécifiée.|  
|0xC0207002|-1071615998|DTS_E_ILLEGALCUSTOMOUTPUTPROPERTY|La sortie %1 ne peut pas posséder la propriété « %2 », mais cette propriété lui est affectée actuellement.|  
|0xC0207003|-1071615997|DTS_E_INVALIDOUTPUTEXCLUSIONGROUP|%1 doit être dans le groupe d'exclusions %2!d!. Toutes les sorties doivent se trouver dans le groupe d'exclusions spécifié.|  
|0xC0207004|-1071615996|DTS_E_PROPERTYISEMPTY|La propriété « %1 » est vide. Cette propriété ne peut pas être vide.|  
|0xC0207005|-1071615995|DTS_E_CREATEEXPRESSIONOBJECTFAILED|Impossible d'allouer de la mémoire à l'expression « %1 ». Une erreur de mémoire insuffisante s'est produite lors de la création d'un objet interne pour contenir l'expression.|  
|0xC0207006|-1071615994|DTS_E_EXPRESSIONPARSEFAILED|Impossible d'analyser l'expression « %1 ». Cette expression n'était pas valide, ou une erreur de mémoire insuffisante est survenue.|  
|0xC0207007|-1071615993|DTS_E_EXPRESSIONCOMPUTEFAILED|Échec du calcul de l'expression « %1 » : code d'erreur 0x%2!8.8X!. Cette expression risque de comporter des erreurs, telles que la division par zéro, indétectables au cours de l'analyse, ou une erreur de mémoire insuffisante s'est produite.|  
|0xC0207008|-1071615992|DTS_E_FAILEDTOCREATEEXPRESSIONARRAY|Impossible d'allouer de la mémoire aux objets Expression. Une erreur de mémoire insuffisante s'est produite au cours de la création d'un tableau de pointeurs d'objet Expression.|  
|0xC020700A|-1071615990|DTS_E_FAILEDTOCREATEEXPRESSIONMANANGER|%1 a échoué et retourné le code d'erreur 0x%2!8.8X! lors de la création du Gestionnaire d'expressions.|  
|0xC020700B|-1071615989|DTS_E_SPLITEXPRESSIONNOTBOOLEAN|L'expression « %1 » n'est pas booléenne. Le type de résultat de cette expression doit être booléen.|  
|0xC020700C|-1071615988|DTS_E_EXPRESSIONVALIDATIONFAILED|Expression « %1 » sur « %2 » non valide.|  
|0xC020700E|-1071615986|DTS_E_COLUMNNOTMATCHED|Correspondance impossible entre la colonne « %1 » (%2!d!) et n'importe quelle colonne de fichier d'entrée. Le nom de la colonne de sortie ou celui de la colonne d'entrée est introuvable dans le fichier.|  
|0xC020700F|-1071615985|DTS_E_SETRESULTCOLUMNFAILED|Échec de la tentative de définition d'une colonne de résultats pour l'expression « %1 » sur %2 : code d'erreur 0x%3!8.8X!. Impossible de déterminer la colonne d'entrée ou de sortie devant recevoir le résultat de l'expression, ou impossible de convertir le résultat de l'expression vers le type de colonne.|  
|0xC0207011|-1071615983|DTS_E_FAILEDTOGETLOCALEIDFROMPACKAGE|%1 n'a pas réussi à obtenir l'ID de paramètres régionaux à partir du package.|  
|0xC0207012|-1071615982|DTS_E_INCORRECTPARAMETERMAPPINGFORMAT|La chaîne de mappage de paramètres ne possède pas le format correct.|  
|0xC0207013|-1071615981|DTS_E_NOTENOUGHPARAMETERSPROVIDED|La commande SQL nécessite %1!d! paramètres, mais le mappage de paramètres ne comporte que %2!d! paramètres.|  
|0xC0207014|-1071615980|DTS_E_PARAMETERNOTFOUNDINMAPPING|La commande SQL nécessite un paramètre nommé « %1 », introuvable dans le mappage de paramètres.|  
|0xC0207015|-1071615979|DTS_E_DUPLICATEDATASOURCECOLUMNNAME|Il existe plusieurs colonnes sources de données portant le nom « %1 ».  Les noms des colonnes sources de données doivent être uniques.|  
|0xC0207016|-1071615978|DTS_E_DATASOURCECOLUMNWITHNONAMEFOUND|Une colonne source de données n'a pas de nom.  Chaque colonne source de données doit avoir un nom.|  
|0xC0208001|-1071611903|DTS_E_DISCONNECTEDCOMPONENT|Un composant est déconnecté de la structure.|  
|0xC0208002|-1071611902|DTS_E_INVALIDCOMPONENTID|L'ID d'un composant de la structure n'est pas valide.|  
|0xC0208003|-1071611901|DTS_E_INVALIDINPUTCOUNT|Un composant possède un nombre d'entrées non valide.|  
|0xC0208004|-1071611900|DTS_E_INVALIDOUTPUTCOUNT|Un composant possède un nombre de sorties non valide.|  
|0xC0208005|-1071611899|DTS_E_NOINPUTSOROUTPUTS|Un composant ne possède aucune entrée ni sortie.|  
|0xC0208007|-1071611897|DTS_E_CANTALLOCATECOLUMNINFO|Quantité de mémoire disponible insuffisante pour allouer la liste des colonnes actuellement manipulées par ce composant.|  
|0xC0208008|-1071611896|DTS_E_OUTPUTCOLUMNNOTININPUT|La colonne de sortie « %1 » (%2!d!) fait référence à la colonne d'entrée dotée de l'ID de lignage %3!d!, mais aucune entrée ne correspond à cet ID de lignage.|  
|0xC0208009|-1071611895|DTS_E_SORTNEEDSONEKEY|Une colonne d'entrée au minimum doit être marquée en tant que clé de tri, mais aucune clé n'a été trouvée.|  
|0xC020800A|-1071611894|DTS_E_SORTDUPLICATEKEYWEIGHT|Les deux colonnes « %1 » (%2!d!) et « %3 » (%4!d!) ont été marquées avec la priorité de clé de tri %5!d!.|  
|0xC020800D|-1071611891|DTS_E_CANTMODIFYINVALID|Ce composant ne peut effectuer la modification de métadonnées requise tant que le problème de validation n'est pas réglé.|  
|0xC020800E|-1071611890|DTS_E_CANTADDINPUT|Impossible d'ajouter une entrée à la collection d'entrées.|  
|0xC020800F|-1071611889|DTS_E_CANTADDOUTPUT|Impossible d'ajouter une sortie à la collection de sorties.|  
|0xC0208010|-1071611888|DTS_E_CANTDELETEINPUT|Impossible de supprimer une entrée de la collection d'entrées.|  
|0xC0208011|-1071611887|DTS_E_CANTDELETEOUTPUT|Impossible de supprimer une sortie de la collection de sorties.|  
|0xC0208014|-1071611884|DTS_E_CANTCHANGEUSAGETYPE|Impossible de modifier le type d'utilisation de la colonne.|  
|0xC0208016|-1071611882|DTS_E_INVALIDUSAGETYPEFORCUSTOMPROPERTY|%1 doit être en lecture/écriture pour posséder la propriété personnalisée « %2 ». La colonne d'entrée ou de sortie possède la propriété personnalisée spécifiée, mais elle n'est pas en lecture/écriture. Supprimez cette propriété, ou autorisez l'accès en lecture/écriture à la colonne.|  
|0xC0208017|-1071611881|DTS_E_READWRITECOLUMNMISSINGREQUIREDCUSTOMPROPERTY|%1 est en lecture/écriture et doit posséder la propriété personnalisée « %2 ». Ajoutez la propriété, ou supprimez l'attribut de lecture/écriture de la colonne.|  
|0xC0208018|-1071611880|DTS_E_CANTDELETECOLUMN|Impossible de supprimer la colonne. Ce composant ne permet pas la suppression de colonnes à partir de cette entrée ou de cette sortie.|  
|0xC0208019|-1071611879|DTS_E_CANTADDCOLUMN|Ce composant ne permet pas l'ajout de colonnes à cette entrée ou cette sortie.|  
|0xC020801A|-1071611878|DTS_E_CANNOTTFINDRUNTIMECONNECTIONOBJECT|La connexion « %1 » est introuvable. Vérifiez que le gestionnaire de connexions renferme une connexion de ce nom.|  
|0xC020801B|-1071611877|DTS_E_CANNOTFINDRUNTIMECONNECTIONMANAGER|Le gestionnaire de connexions en cours d'exécution avec l'ID « %1 » est introuvable. Vérifiez que la collection de gestionnaires de connexions possède un gestionnaire de connexions avec cet ID.|  
|0xC020801C|-1071611876|DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER|Code d'erreur SSIS DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER.  Échec de l'appel de la méthode AcquireConnection vers le gestionnaire de connexions « %1 ». Code d'erreur : 0x%2!8.8X!.  Des messages d'erreur peuvent être envoyés au préalable avec des informations indiquant la raison de l'échec de la méthode AcquireConnection.|  
|0xC020801D|-1071611875|DTS_E_ACQUIREDCONNECTIONISINVALID|La connexion acquise à partir du gestionnaire de connexions « %1 » n'est pas valide.|  
|0xC020801E|-1071611874|DTS_E_INCORRECTCONNECTIONMANAGERTYPE|Le type du gestionnaire de connexions « %1 » est incorrect.  Le type requis est « %2 ». Le type disponible pour le composant est « %3 ».|  
|0xC020801F|-1071611873|DTS_E_CANNOTACQUIREMANAGEDCONNECTIONFROMCONNECTIONMANAGER|Impossible d'acquérir une connexion gérée depuis le gestionnaire de connexions en cours d'exécution.|  
|0xC0208020|-1071611872|DTS_E_CANTINITINPUT|Impossible de créer une entrée pour initialiser la collection d'entrées.|  
|0xC0208021|-1071611871|DTS_E_CANTINITOUTPUT|Impossible de créer une sortie pour initialiser la collection de sorties.|  
|0xC0208023|-1071611869|DTS_E_EXTRACTORCANTWRITE|Échec de l'écriture dans le fichier « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC0208024|-1071611868|DTS_E_INCORRECTCONNECTIONOBJECTTYPE|Le gestionnaire de connexions « %1 » a retourné un objet de type incorrect à partir de la méthode AcquireConnection.|  
|0xC0208025|-1071611867|DTS_E_INPUTCOLPROPERTYNOTFOUND|La propriété « %3 » est requise sur la colonne d'entrée « %1 » (%2!d!), mais elle est introuvable. La propriété manquante doit être ajoutée.|  
|0xC0208026|-1071611866|DTS_E_EXTRACTORUNREFERENCED|« %1 » est indiqué en lecture seule mais n'est référencé par aucune autre colonne. Les colonnes non référencées ne sont pas autorisées.|  
|0xC0208027|-1071611865|DTS_E_EXTRACTORREFERENCEDCOLUMNNOTFOUND|« %1 » fait référence à l'ID de colonne %2!d!, or cette colonne est introuvable sur l'entrée. Une référence pointe vers une colonne inexistante.|  
|0xC0208028|-1071611864|DTS_E_EXTRACTORDATACOLUMNNOTBLOB|« %1 » fait référence à « %2 », et cette colonne n'est pas de type BLOB.|  
|0xC0208029|-1071611863|DTS_E_INSERTERREFERENCEDCOLUMNNOTFOUND|« %1 » fait référence à l'ID de colonne de sortie %2!d!, or cette colonne est introuvable sur la sortie.|  
|0xC020802A|-1071611862|DTS_E_INSERTERCANTREAD|Échec de la lecture à partir du fichier « %1 ». Code d'erreur : 0x%2!8.8X!.|  
|0xC020802B|-1071611861|DTS_E_TXSCD_NOTYPEDCOLUMNSATINPUT|Il doit exister au moins une colonne de type Fixe, Variant ou Historique sur l'entrée d'une transformation de dimension à variation lente. Vérifiez qu'une colonne au minimum comporte un attribut de type FixedAttribute, ChangingAttribute ou HistoricalAttribute.|  
|0xC020802C|-1071611860|DTS_E_TXSCD_INVALIDINPUTCOLUMNTYPE|La propriété ColumnType de « %1 » n'est pas valide. La valeur actuelle est en dehors de la plage de valeurs acceptables.|  
|0xC020802D|-1071611859|DTS_E_TXSCD_CANNOTMAPDIFFERENTTYPES|Impossible de mapper la colonne d'entrée « %1 » à la colonne externe « %2 », car elles possèdent des types de données différents. La transformation de dimension à variation lente ne permet pas le mappage entre des colonnes de types différents, sauf pour DT_STR et DT_WSTR.|  
|0xC020802E|-1071611858|DTS_E_NTEXTDATATYPENOTSUPPORTEDWITHANSIFILES|Le type de données de « %1 » est DT_NTEXT. Ce dernier n'est pas pris en charge par les fichiers ANSI. Utilisez DT_TEXT à la place et convertissez les données au type DT_NTEXT à l'aide du composant de conversion de données.|  
|0xC020802F|-1071611857|DTS_E_TEXTDATATYPENOTSUPPORTEDWITHUNICODEFILES|Le type de données de « %1 » est DT_TEXT. Ce dernier n'est pas pris en charge par les fichiers Unicode. Utilisez DT_NTEXT à la place et convertissez les données au type DT_TEXT à l'aide du composant de conversion de données.|  
|0xC0208030|-1071611856|DTS_E_IMAGEDATATYPENOTSUPPORTED|Le type de données de « %1 » est DT_IMAGE. Ce dernier n'est pas pris en charge. Utilisez les types DT_TEXT ou DT_NTEXT à la place et convertissez les données depuis ou vers le type DT_IMAGE à l'aide du composant de conversion de données.|  
|0xC0208031|-1071611855|DTS_E_FLATFILEFORMATNOTSUPPORTED|Le format « %1 » n'est pas pris en charge par le gestionnaire de connexions de fichiers plats. Les formats pris en charge sont Delimited, FixedWidth, RaggedRight et Mixed.|  
|0xC0208032|-1071611854|DTS_E_EXTRACTORFILENAMECOLUMNNOTSTRING|« %1 » doit contenir un nom de fichier, mais il n'est pas de type String.|  
|0xC0208033|-1071611853|DTS_E_EXTRACTORCANTAPPENDTRUNCATE|Erreur causée par des paramètres de propriété conflictuels. « %1 » possède les deux propriétés AllowAppend et ForceTruncate avec la valeur TRUE. Ces deux propriétés ne peuvent posséder cette valeur TRUE. Attribuez la valeur FALSE à l'une des deux propriétés.|  
|0xC0208034|-1071611852|DTS_E_EXTRACTORCOLUMNALREADYREFERENCED|%1 fait référence à l'ID de colonne %2!d!, mais cette colonne est déjà référencée par %3. Supprimez l'une des deux références à cette colonne.|  
|0xC0208035|-1071611851|DTS_E_CONNECTIONMANANGERNOTASSIGNED|Un gestionnaire de connexions n'a pas été affecté à %1.|  
|0xC0208036|-1071611850|DTS_E_INSERTERCOLUMNALREADYREFERENCED|%1 fait référence à la colonne de sortie avec l'ID %2!d!, mais cette colonne est déjà référencée par %3.|  
|0xC0208037|-1071611849|DTS_E_INSERTERCOLUMNNOTREFERENCED|« %1 » n'est référencé par aucune colonne d'entrée. Chaque colonne de sortie doit être référencée par une colonne d'entrée exactement.|  
|0xC0208038|-1071611848|DTS_E_INSERTERDATACOLUMNNOTBLOB|« %1 » fait référence à « %2 », mais le type de cette colonne est incorrect. Ce type doit être DT_TEXT, DT_NTEXT ou DT_IMAGE. Une référence pointe vers une colonne qui doit être un objet BLOB.|  
|0xC0208039|-1071611847|DTS_E_INSERTERFILENAMECOLUMNNOTSTRING|« %1 » doit contenir un nom de fichier, mais il n'est pas de type String.|  
|0xC020803A|-1071611846|DTS_E_INSERTEREXPECTBOMINVALIDTYPE|La propriété ExpectBOM de « %1 » possède la valeur TRUE pour %2, mais la colonne n'est pas de type NT_NTEXT. La propriété ExpectBOM spécifie que la transformation d'importation de colonne attend un BOM (byte-order mark). Attribuez la valeur False à la propriété ExpectBOM ou le type de données DT_NTEXT à la colonne de sortie.|  
|0xC020803B|-1071611845|DTS_E_INSERTERINVALIDDATACOLUMNSETTYPE|Les colonnes de sortie de données doivent être DT_TEXT, DT_NTEXT ou DT_IMAGE. La colonne de sortie de données peut uniquement être définie sur le type BLOB.|  
|0xC020803C|-1071611844|DTS_E_TXSCD_FIXEDATTRIBUTECHANGE|Si la propriété FailOnFixedAttributeChange possède la valeur TRUE, la transformation échouera lors de la détection d'une modification d'attribut fixe. Pour envoyer des lignes vers la sortie d'attribut fixe, attribuez la valeur FALSE à la propriété FailOnFixedAttributeChange.|  
|0xC020803D|-1071611843|DTS_E_TXSCD_LOOKUPFAILURE|Échec de la transformation de recherche pour l'extraction des lignes. La transformation échoue lorsque la valeur TRUE est attribuée à FailOnLookupFailure et qu'aucune ligne n'est extraite.|  
|0xC020803E|-1071611842|DTS_E_TXSCD_INVALIDNUMBERSOFPARAMETERS|Il doit exister au moins une colonne de type Clé sur l'entrée d'une transformation de dimension à variation lente. Définissez au moins une colonne de type Clé.|  
|0xC020803F|-1071611841|DTS_E_TXSCD_CANNOTFINDEXTERNALCOLUMN|Impossible de trouver une colonne externe nommée « %1 ».|  
|0xC0208040|-1071611840|DTS_E_TXSCD_INFFEREDINDICATORNOTBOOL|La colonne d'indicateur inférée « %1 » doit être de type DT_BOOL.|  
|0xC0208107|-1071611641|DTS_E_ERRORROWDISPMUSTBENOTUSED|La valeur de disposition de la ligne d'erreur de %1 doit être RD_NotUsed.|  
|0xC0208108|-1071611640|DTS_E_TRUNCROWDISPMUSTBENOTUSED|La valeur de disposition de la ligne de troncation de %1 doit être RD_NotUsed.|  
|0xC0208201|-1071611391|DTS_E_TXAGG_INPUTNOTFOUNDFOROUTPUT|Impossible de trouver la colonne d'entrée portant l'ID de lignage %1!d! requise par la colonne de sortie portant l'ID %2!d!.|  
|0xC0208202|-1071611390|DTS_E_TXAGG_INVALIDOUTPUTDATATYPEFORAGGREGATE|Type de données de sortie non valide pour le type d'agrégation spécifié sur l'ID de la colonne de sortie %1!d!.|  
|0xC0208203|-1071611389|DTS_E_TXAGG_INVALIDINPUTDATATYPEFORAGGREGATE|Le type de données d'entrée utilisé %1 n'est pas valide pour l'agrégation spécifiée sur %2.|  
|0xC0208204|-1071611388|DTS_E_TXAGG_INPUTOUTPUTDATATYPEMISMATCH|Les types de données portant l'ID de lignage de colonne d'entrée %1!d! et l'ID de colonne de sortie %2!d! ne correspondent pas.|  
|0xC0208205|-1071611387|DTS_E_UNABLETOGETINPUTBUFFERHANDLE|Impossible d'obtenir le descripteur du tampon d'entrée pour l'ID d'entrée %1!d!.|  
|0xC0208206|-1071611386|DTS_E_UNABLETOGETOUTPUTBUFFERHANDLE|Impossible d'obtenir le descripteur du tampon de sortie pour l'ID de sortie %1!d!.|  
|0xC0208207|-1071611385|DTS_E_UNABLETOFINDCOLUMNHANDLEINOUTPUTBUFFER|Impossible de trouver la colonne portant l'ID de lignage %1!d! dans le tampon de sortie.|  
|0xC0208208|-1071611384|DTS_E_UNABLETOFINDCOLUMNHANDLEININPUTBUFFER|Impossible de trouver la colonne portant l'ID de lignage %1!d! dans le tampon d'entrée.|  
|0xC0208209|-1071611383|DTS_E_CANNOTHAVEZEROOUTPUTCOLUMNS|Le nombre de colonnes de sortie pour %1 ne peut être égal à zéro.|  
|0xC020820A|-1071611382|DTS_E_CONNECTIONMANAGERCOLUMNCOUNTMISMATCH|Le nombre de colonnes dans le gestionnaire de connexions de fichiers plats doit être identique au nombre de colonnes dans l'adaptateur de fichier plat. Le nombre de colonnes du gestionnaire de connexions de fichiers plats est de %1!d!, tandis que celui de l'adaptateur de fichier plat est de %2!d!.|  
|0xC020820B|-1071611381|DTS_E_MISMATCHCONNECTIONMANAGERCOLUMN|La colonne « %1 » à l'index %2!d! dans le gestionnaire de connexions de fichiers plats est introuvable à l'index %3!d! dans la collection de colonnes de l'adaptateur de fichier plat.|  
|0xC020820D|-1071611379|DTS_E_EXTERNALMETADATACOLUMNISALREADYMAPPED|La colonne de métadonnées externe portant l'ID %1!d! a déjà été mappée à %2.|  
|0xC020820E|-1071611378|DTS_E_TXAGG_STRING_TOO_LONG|La transformation a rencontré une colonne clé comportant plus de %1!u! caractères.|  
|0xC020820F|-1071611377|DTS_E_DERIVEDRESULT_TOO_LONG|La transformation a rencontré une valeur de résultat supérieure à %1!u! octets.|  
|0xC0208210|-1071611376|DTS_E_TXAGG_MEMALLOCERROUTPUTDESCRIPTORS|Impossible d'allouer de la mémoire.|  
|0xC0208211|-1071611375|DTS_E_TXAGG_MEMALLOCERRWORKSPACEDESCRIPTORS|Impossible d'allouer de la mémoire.|  
|0xC0208212|-1071611374|DTS_E_TXAGG_MEMALLOCERRSORTORDERDESCRIPTORS|Impossible d'allouer de la mémoire.|  
|0xC0208213|-1071611373|DTS_E_TXAGG_MEMALLOCERRNUMERICDESCRIPTORS|Impossible d'allouer de la mémoire.|  
|0xC0208214|-1071611372|DTS_E_TXAGG_MEMALLOCERRCOUNTDISTINCTDESCRIPTOR|Impossible d'allouer de la mémoire.|  
|0xC0208215|-1071611371|DTS_E_TXAGG_MEMALLOCERRWORKSPACESORTORDERDESCRIPTORS|Impossible d'allouer de la mémoire.|  
|0xC0208216|-1071611370|DTS_E_TXAGG_MEMALLOCERRWORKSPACENUMERICDESCRIPTORS|Impossible d'allouer de la mémoire.|  
|0xC0208217|-1071611369|DTS_E_TXAGG_MEMALLOCERRWORKSPACEBUFFCOLS|Impossible d'allouer de la mémoire.|  
|0xC0208218|-1071611368|DTS_E_UNREFERENCEDINPUTCOLUMN|La colonne d'entrée « %1 » n'est pas référencée.|  
|0xC0208219|-1071611367|DTS_E_CANTBUILDTHREADPOOL|La transformation de tri n'a pas pu créer un pool de threads avec %1!d! threads. Mémoire disponible insuffisante.|  
|0xC020821A|-1071611366|DTS_E_QUEUEWORKITEMFAILED|La transformation de tri n'a pas pu mettre un élément de travail en file d'attente. Mémoire disponible insuffisante.|  
|0xC020821B|-1071611365|DTS_E_SORTTHREADSTOPPED|Arrêt d'un thread de travail dans la transformation de tri. Code d'erreur : 0x%1!8.8X!. Une erreur grave s'est produite lors du tri d'un tampon.|  
|0xC020821E|-1071611362|DTS_E_SORTBADTHREADCOUNT|La valeur de MaxThreads est de %1!ld!, alors qu'elle doit se situer entre 1 et %2!ld! inclus, ou être égale à -1 pour correspondre par défaut au nombre d'UC.|  
|0xC020821F|-1071611361|DTS_E_DTPXMLLOADFAILURE|Chargement impossible à partir de XML.|  
|0xC0208220|-1071611360|DTS_E_DTPXMLSAVEFAILURE|Enregistrement impossible dans XML.|  
|0xC0208221|-1071611359|DTS_E_DTPXMLINT32CONVERTERR|Impossible de convertir la valeur « %1 » en entier.|  
|0xC0208222|-1071611358|DTS_E_DTPXMLBOOLCONVERTERR|Impossible de convertir la valeur « %1 » en valeur booléenne.|  
|0xC0208223|-1071611357|DTS_E_DTPXMLPARSEERRORNEARID|Erreur de chargement rencontrée près de l'objet portant l'ID %1!d!.|  
|0xC0208226|-1071611354|DTS_E_DTPXMLPROPERTYTYPEERR|La valeur %1 n'est pas valide pour l'attribut %2.|  
|0xC0208228|-1071611352|DTS_E_DTPXMLSETUSAGETYPEERR|La valeur %1 n'est pas valide pour l'attribut %2.|  
|0xC0208229|-1071611351|DTS_E_DTPXMLDATATYPEERR|La valeur %1 n'est pas valide pour l'attribut %2.|  
|0xC020822A|-1071611350|DTS_E_UNMAPPEDINPUTCOLUMN|%1 n'est pas mappé à une colonne de sortie.|  
|0xC020822B|-1071611349|DTS_E_INPUTCOLUMNBADMAP|%1 possède un mappage non valide.  Une colonne de sortie avec l'ID %2!ld! n'existe pas sur ce composant.|  
|0xC020822D|-1071611347|DTS_E_MULTIPLYMAPPEDOUTCOL|%1 est mappé à une colonne de sortie qui possède déjà un mappage sur cette entrée.|  
|0xC020822E|-1071611346|DTS_E_TXAGG_STRINGPROMOTIONFAILED|Impossible de convertir la colonne d'entrée portant l'ID de lignage %1!ld! au type DT_WSTR, en raison de l'erreur 0x%2!8.8X!.|  
|0xC0208230|-1071611344|DTS_E_DTPXMLIDLOOKUPERR|L'objet référencé avec l'ID %1!d! est introuvable dans le package.|  
|0xC0208231|-1071611343|DTS_E_DTPXMLINVALIDXMLPERSISTPROPERTY|Impossible de lire une propriété de persistance requise pour le module pipelinexml. Cette propriété n'a pas été fournie par le pipeline.|  
|0xC0208232|-1071611342|DTS_E_DTPXMLPROPERTYSTATEERR|La valeur %1 n'est pas valide pour l'attribut %2.|  
|0xC0208233|-1071611341|DTS_E_CANTGETCUSTOMPROPERTY|Impossible d'extraire la propriété personnalisée « %1 ».|  
|0xC0208234|-1071611340|DTS_E_UNABLETOLOCATEINPUTCOLUMNID|Une colonne d'entrée avec l'ID de lignage %1!d!, référencée dans la propriété personnalisée ParameterMap avec le paramètre occupant la position numéro %2!d!, est introuvable dans la collection des colonnes d'entrée.|  
|0xC0208235|-1071611339|DTS_E_TXLOOKUP_UNABLETOLOCATEREFCOLUMN|Colonne de référence « %1 » introuvable.|  
|0xC0208236|-1071611338|DTS_E_TXLOOKUP_INCOMPATIBLEDATATYPES|%1 et la colonne de référence nommée « %2 » possèdent des types de données incompatibles.|  
|0xC0208237|-1071611337|DTS_E_TXLOOKUP_PARAMMETADATAMISMATCH|L'instruction SQL paramétrable produit des métadonnées qui ne correspondent pas à l'instruction SQL principale.|  
|0xC0208238|-1071611336|DTS_E_TXLOOKUP_INCORRECTNUMOFPARAMETERS|L'instruction SQL paramétrable contient un nombre incorrect de paramètres. %1!d! étaient attendus, mais %2!d! ont été trouvés.|  
|0xC0208239|-1071611335|DTS_E_TXLOOKUP_INVALIDJOINTYPE|%1 possède un type de données qui ne peut pas être joint.|  
|0xC020823A|-1071611334|DTS_E_TXLOOKUP_INVALIDCOPYTYPE|%1 possède un type de données qui ne peut pas être copié.|  
|0xC020823B|-1071611333|DTS_E_INSERTERINVALIDCOLUMNDATATYPE|Le %1 possède un type de données non pris en charge. Le type doit être DT_STR ou DT_WSTR.|  
|0xC020823C|-1071611332|DTS_E_EXTRACTORINVALIDCOLUMNDATATYPE|Le %1 possède un type de données non pris en charge. Le type doit être DT_STR, DT_WSTR, DT_TEXT, DT_NTEXT ou DT_IMAGE.|  
|0xC020823D|-1071611331|DTS_E_TXCHARMAPINVALIDCOLUMNDATATYPE|Le %1 possède un type de données non pris en charge. Le type doit être DT_STR, DT_WSTR, DT_TEXT ou DT_NTEXT.|  
|0xC020823E|-1071611330|DTS_E_SORTCANTCREATEEVENT|La transformation de tri ne peut pas créer un événement pour communiquer avec ses threads de travail. Le nombre de descripteurs système disponibles pour la transformation de tri est insuffisant.|  
|0xC020823F|-1071611329|DTS_E_SORTCANTCREATETHREAD|La transformation de tri ne peut pas créer un thread de travail. La mémoire disponible pour la transformation de tri est insuffisante.|  
|0xC0208240|-1071611328|DTS_E_SORTCANTCOMPARE|Échec de la transformation de tri lors de la comparaison de la ligne %1!d! dans l'ID de tampon %2!d! à la ligne %3!d! dans l'ID de tampon %4!d!.|  
|0xC0208242|-1071611326|DTS_E_TXLOOKUP_TOOFEWREFERENCECOLUMNS|Les métadonnées de référence de la transformation de recherche contiennent un nombre de colonnes trop faible. Vérifiez la propriété SQLCommand. L'instruction SELECT doit retourner au moins une colonne.|  
|0xC0208243|-1071611325|DTS_E_TXLOOKUP_MALLOCERR_REFERENCECOLUMNINFO|Impossible d'allouer de la mémoire à un tableau de structures ColumnInfo.|  
|0xC0208244|-1071611324|DTS_E_TXLOOKUP_MALLOCERR_REFERENCECOLUMNPAIR|Impossible d'allouer de la mémoire à un tableau de structures ColumnPair.|  
|0xC0208245|-1071611323|DTS_E_TXLOOKUP_MALLOCERR_BUFFCOL|Impossible d'allouer de la mémoire à un tableau de structures BUFFCOL pour la création d'un espace de travail principal.|  
|0xC0208246|-1071611322|DTS_E_TXLOOKUP_MAINWORKSPACE_CREATEERR|Impossible de créer un tampon d'espace de travail principal.|  
|0xC0208247|-1071611321|DTS_E_TXLOOKUP_HASHTABLE_MALLOCERR|Impossible d'allouer de la mémoire à la table de hachage.|  
|0xC0208248|-1071611320|DTS_E_TXLOOKUP_HASHNODEHEAP_CREATEERR|Impossible d'allouer de la mémoire pour créer un segment de nœuds de hachage.|  
|0xC0208249|-1071611319|DTS_E_TXLOOKUP_HASHNODEHEAP_MALLOCERR|Impossible d'allouer de la mémoire pour un segment de nœud de hachage.|  
|0xC020824A|-1071611318|DTS_E_TXLOOKUP_LRUNODEHEAP_CREATEERR|Impossible de créer un segment pour les derniers nœuds récemment utilisés. Une condition de mémoire insuffisante s'est produite.|  
|0xC020824B|-1071611317|DTS_E_TXLOOKUP_LRUNODEHEAP_MALLOCERR|Impossible d'allouer de la mémoire au segment du dernier nœud récemment utilisé. Une condition de mémoire insuffisante s'est produite.|  
|0xC020824C|-1071611316|DTS_E_TXLOOKUP_OLEDBERR_LOADCOLUMNMETADATA|Une erreur OLE DB s'est produite lors du chargement des métadonnées de colonne. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC020824D|-1071611315|DTS_E_TXLOOKUP_OLEDBERR_GETIROWSET|Une erreur OLE DB s'est produite lors de l'extraction d'un ensemble de lignes. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC020824E|-1071611314|DTS_E_TXLOOKUP_OLEDBERR_FILLBUFFER|Une erreur OLE DB s'est produite lors du remplissage du cache interne. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC020824F|-1071611313|DTS_E_TXLOOKUP_OLEDBERR_BINDPARAMETERS|Une erreur OLE DB s'est produite lors de la liaison de paramètres. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC0208250|-1071611312|DTS_E_TXLOOKUP_OLEDBERR_CREATEBINDING|Une erreur OLE DB s'est produite lors de la création de liaisons. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC0208251|-1071611311|DTS_E_TXLOOKUP_INVALID_CASE|Une casse non valide a été rencontrée dans une instruction switch au cours de l'exécution.|  
|0xC0208252|-1071611310|DTS_E_TXLOOKUP_MAINWORKSPACE_MALLOCERR|Impossible d'allouer de la mémoire à une nouvelle ligne pour le tampon d'espace de travail principal. Une condition de mémoire insuffisante s'est produite.|  
|0xC0208253|-1071611309|DTS_E_TXLOOKUP_OLEDBERR_GETPARAMIROWSET|Une erreur OLE DB s'est produite lors de l'extraction de l'ensemble de lignes paramétrables. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC0208254|-1071611308|DTS_E_TXLOOKUP_OLEDBERR_GETPARAMSINGLEROW|Une erreur OLE DB s'est produite lors de l'extraction de la ligne paramétrable. Vérifiez les propriétés SQLCommand et SqlCommandParam.|  
|0xC0208255|-1071611307|DTS_E_TXAGG_MAINWORKSPACE_MALLOCERR|Impossible d'allouer de la mémoire à une nouvelle ligne pour le tampon d'espace de travail principal. Une condition de mémoire insuffisante s'est produite.|  
|0xC0208256|-1071611306|DTS_E_TXAGG_MAINWORKSPACE_CREATEERR|Impossible de créer un tampon d'espace de travail principal.|  
|0xC0208257|-1071611305|DTS_E_TXAGG_HASHTABLE_MALLOCERR|Impossible d'allouer de la mémoire pour la table de hachage.|  
|0xC0208258|-1071611304|DTS_E_TXAGG_HASHNODEHEAP_CREATEERR|Impossible d'allouer de la mémoire pour créer un segment de nœuds de hachage.|  
|0xC0208259|-1071611303|DTS_E_TXAGG_HASHNODEHEAP_MALLOCERR|Impossible d'allouer de la mémoire pour le segment de nœud de hachage.|  
|0xC020825A|-1071611302|DTS_E_TXAGG_CDNODEHEAP_CREATEERR|Impossible d'allouer de la mémoire pour créer un segment de nœuds CountDistinct.|  
|0xC020825B|-1071611301|DTS_E_TXAGG_CDNODEHEAP_MALLOCERR|Impossible d'allouer de la mémoire pour le segment de nœud CountDistinct.|  
|0xC020825C|-1071611300|DTS_E_TXAGG_CDCHAINHEAP_CREATEERR|Impossible d'allouer de la mémoire pour créer un segment pour les chaînes CountDistinct.|  
|0xC020825D|-1071611299|DTS_E_TXAGG_CDHASHTABLE_CREATEERR|Impossible d'allouer de la mémoire pour une table de hachage CountDistinct.|  
|0xC020825E|-1071611298|DTS_E_TXAGG_CDWORKSPACE_MALLOCERR|Impossible d'allouer de la mémoire pour une nouvelle ligne du tampon d'espace de travail CountDistinct.|  
|0xC020825F|-1071611297|DTS_E_TXAGG_CDWORKSPACE_CREATEERR|Impossible de créer un tampon d'espace de travail CountDistinct.|  
|0xC0208260|-1071611296|DTS_E_TXAGG_CDCOLLASSEARRAY_MALLOCERR|Impossible d'allouer de la mémoire pour un tableau de réduction CountDistinct.|  
|0xC0208261|-1071611295|DTS_E_TXAGG_CDCHAINHEAP_MALLOCERR|Impossible d'allouer de la mémoire pour les chaînes CountDistinct.|  
|0xC0208262|-1071611294|DTS_E_TXCOPYMAP_MISMATCHED_COLUMN_METADATA|Les colonnes avec les ID de lignage %1!d! et %2!d! comportent des métadonnées non concordantes. La colonne d’entrée mappée à une colonne de sortie pour la fonction de copie/mappage ne possède pas les mêmes métadonnées (type de données, précision, échelle, longueur ou page de codes).|  
|0xC0208263|-1071611293|DTS_E_TXCOPYMAP_INCORRECT_OUTPUT_COLUMN_MAPPING|La colonne de sortie portant l’ID de lignage « %1!d! » est incorrectement mappée à une colonne d’entrée. La propriété CopyColumnId de la colonne de sortie n'est pas correcte.|  
|0xC0208265|-1071611291|DTS_E_CANTGETBLOBDATA|Échec de l'extraction de données de type Long pour la colonne « %1 ».|  
|0xC0208266|-1071611290|DTS_E_CANTADDBLOBDATA|Des données de type Long ont été récupérées pour une colonne, mais elles ne peuvent pas être ajoutées au tampon de la tâche de flux de données.|  
|0xC0208267|-1071611289|DTS_E_MCASTOUTPUTCOLUMNS|La sortie « %1 » (%2!d!) possède des colonnes de sortie, mais les sorties de multidiffusion ne déclarent pas de colonnes. Ce package est endommagé.|  
|0xC0208273|-1071611277|DTS_E_UNABLETOGETLOCALIZEDRESOURCE|Impossible de charger l'ID de ressource localisée %1!d!. Vérifiez que le fichier RLL est présent.|  
|0xC0208274|-1071611276|DTS_E_DTPXMLEVENTSCACHEERR|Impossible d'obtenir l'interface d'événements. Une interface d'événements non valide a été transmise au module de flux de données pour la persistance XML.|  
|0xC0208275|-1071611275|DTS_E_DTPXMLPATHLOADERR|Une erreur s'est produite au cours du paramétrage d'un objet de chemin d'accès lors du chargement XML.|  
|0xC0208276|-1071611274|DTS_E_DTPXMLINPUTLOADERR|Erreur de paramétrage de l'objet d'entrée au cours du chargement XML.|  
|0xC0208277|-1071611273|DTS_E_DTPXMLOUTPUTLOADERR|Erreur de paramétrage de l'objet de sortie au cours du chargement XML.|  
|0xC0208278|-1071611272|DTS_E_DTPXMLINPUTCOLUMNLOADERR|Erreur de paramétrage de l'objet de colonne d'entrée au cours du chargement XML.|  
|0xC0208279|-1071611271|DTS_E_DTPXMLOUTPUTCOLUMNLOADERR|Erreur de paramétrage de l'objet de colonne de sortie au cours du chargement XML.|  
|0xC0208280|-1071611264|DTS_E_DTPXMLPROPERTYLOADERR|Erreur de paramétrage de l'objet de propriété au cours du chargement XML.|  
|0xC0208281|-1071611263|DTS_E_DTPXMLCONNECTIONLOADERR|Erreur de paramétrage de l'objet de connexion au cours du chargement XML.|  
|0xC0208282|-1071611262|DTS_E_FG_MISSING_OUTPUT_COLUMNS|Des colonnes spécifiques de transformation spéciale sont manquantes, ou leurs types sont incorrects.|  
|0xC0208283|-1071611261|DTS_E_FG_PREPARE_TABLES_AND_ACCESSORS|Échec de la création des tables et accesseurs requis par la transformation de regroupement approximatif.|  
|0xC0208284|-1071611260|DTS_E_FG_COPY_INPUT|La transformation de regroupement approximatif n'a pas réussi à copier l'entrée.|  
|0xC0208285|-1071611259|DTS_E_FG_GENERATE_GROUPS|La transformation de regroupement approximatif n'a pas réussi à générer les groupes.|  
|0xC0208286|-1071611258|DTS_E_FG_LEADING_TRAILING|Une erreur inattendue s'est produite dans le regroupement approximatif lors de l'application des paramètres de propriété « %1 ».|  
|0xC0208287|-1071611257|DTS_E_FG_PICK_CANONICAL|La transformation de regroupement approximatif n'a pas pu choisir une ligne canonique de données à utiliser dans la standardisation de données.|  
|0xC0208288|-1071611256|DTS_E_FG_NOBLOBS|Le regroupement probable ne prend pas en charge les colonnes d'entrée de type IMAGE, TEXT ou NTEXT.|  
|0xC0208289|-1071611255|DTS_E_FG_FUZZY_MATCH_ON_NONSTRING|Une correspondance approximative est spécifiée sur la colonne « %1 » (%2!d!) dont le type de données ne correspond pas à DT_STR ou à DT_WSTR.|  
|0xC020828A|-1071611254|DTS_E_FUZZYGROUPINGINTERNALPIPELINEERROR|Une erreur de pipeline de transformation de regroupement approximatif s'est produite et a retourné le code d'erreur 0x%1!8.8X! : « %2 ».|  
|0xC020828B|-1071611253|DTS_E_CODE_PAGE_NOT_SUPPORTED|La page de codes %1!d! spécifiée sur la colonne « %2 » (%3!d!) n'est pas prise en charge.  Vous devez d'abord convertir cette colonne en DT_WSTR. Pour cela, insérez une transformation de conversion de données avant celle-ci.|  
|0xC0208294|-1071611244|DTS_E_SETEODFAILED|Échec rencontré lors du paramétrage de la fin d'indicateur de données pour le tampon gérant la sortie « %1 » (%2!d!).|  
|0xC0208296|-1071611242|DTS_E_CANTCLONE|Impossible de cloner le tampon d'entrée en raison d'une condition de mémoire insuffisante ou d'une erreur interne.|  
|0xC02082F9|-1071611143|DTS_E_TXCHARMAP_CANTKATAKANAHIRAGANA|La colonne « %1 » exige que les caractères Katakana et Hiragana soient produits en même temps.|  
|0xC02082FA|-1071611142|DTS_E_TXCHARMAP_CANTSIMPLECOMPLEX|La colonne « %1 » nécessite que les caractères Chinois simplifié et Chinois traditionnel soient produits en même temps.|  
|0xC02082FB|-1071611141|DTS_E_TXCHARMAP_CANTFULLHALF|La colonne « %1 » nécessite que les opérations créent des caractères à demi-chasse et à pleine chasse.|  
|0xC02082FC|-1071611140|DTS_E_TXCHARMAP_CANTCHINAJAPAN|La colonne « %1 » associe les opérations sur les caractères japonais aux opérations pour les caractères chinois.|  
|0xC02082FD|-1071611139|DTS_E_TXCHARMAP_CANTCASECHINESE|La colonne « %1 » associe les opérations sur les caractères chinois aux opérations en majuscules et en minuscules.|  
|0xC02082FE|-1071611138|DTS_E_TXCHARMAP_CANTCASEJAPANESE|La colonne « %1 » associe les opérations sur des caractères japonais aux opérations en caractères majuscules et en minuscules.|  
|0xC02082FF|-1071611137|DTS_E_TXCHARMAP_CANTBOTHCASE|La colonne « %1 » mappe la colonne aux caractères majuscules et minuscules.|  
|0xC0208300|-1071611136|DTS_E_TXCHARMAP_CANTLINGUISTIC|La colonne « %1 » associe les indicateurs autres que les majuscules et les minuscules à l'opération de casse linguistique.|  
|0xC0208301|-1071611135|DTS_E_TXCHARMAP_INVALIDMAPFLAGANDDATATYPE|Le type de données de la colonne « %1 » ne peut pas être mappé comme spécifié.|  
|0xC0208302|-1071611134|DTS_E_TXFUZZYLOOKUP_UNSUPPORTED_MATCH_INDEX_VERSION|La version (%1) de l'index de correspondance déjà existant « %2 » n'est pas prise en charge. La version attendue est « %3 ». Cette erreur se produit si la version qui a persisté dans les métadonnées de l'index ne correspond pas à la version pour laquelle le code actuel a été conçu. Résolvez cette erreur en recréant l'index à l'aide de la version actuelle du code.|  
|0xC0208303|-1071611133|DTS_E_TXFUZZYLOOKUP_INVALID_MATCH_INDEX|La table « %1 » ne semble pas constituer un index valide de correspondance avant génération. Cette erreur se produit lorsque l'enregistrement des métadonnées ne peut pas être chargé à partir de l'index avant génération spécifié.|  
|0xC0208304|-1071611132|DTS_E_TXFUZZYLOOKUP_UNABLE_TO_READ_MATCH_INDEX|Impossible de lire l'index de correspondance avant génération « %1 ».  Code d'erreur OLEDB : 0x%2!8.8X!.|  
|0xC0208305|-1071611131|DTS_E_TXFUZZYLOOKUP_NO_JOIN_COLUMNS|Aucune colonne d'entrée ne comporte une jointure valide vers une colonne de table de référence.  Vérifiez qu'il existe au moins une jointure définie à l'aide des propriétés de la colonne d'entrée JoinToReferenceColumn et JoinType.|  
|0xC0208306|-1071611130|DTS_E_TXFUZZYLOOKUP_INDEX_DOES_NOT_CONTAIN_COLUMN|L'index de correspondance déjà existant « %1 » n'a pas été créé à l'origine avec des informations de correspondance approximative pour la colonne « %2 ».  Il doit être recréé pour inclure ces informations. Cette erreur se produit lorsque l'index a été créé avec une colonne qui n'est pas une colonne de jonction approximative.|  
|0xC0208307|-1071611129|DTS_E_TXFUZZYLOOKUP_IDENTIFIER_PROPERTY|Le nom « %1 » attribué à la propriété « %2 » n'est pas un nom d'identificateur SQL valide. Cette erreur se produit si le nom de la propriété n'est pas conforme aux spécifications d'un nom d'identificateur SQL valide.|  
|0xC0208309|-1071611127|DTS_E_TXFUZZYLOOKUP_MINSIMILARITY_INVALID|La propriété de seuil MinSimilarity sur la transformation de recherche floue doit être une valeur supérieure ou égale à 0.0, mais inférieure à 1.0.|  
|0xC020830A|-1071611126|DTS_E_TXFUZZYLOOKUP_INVALID_PROPERTY_VALUE|La valeur « %1 » de la propriété « %2 » n'est pas valide.|  
|0xC020830B|-1071611125|DTS_E_TXFUZZYLOOKUP_INCOMPATIBLE_FUZZY_JOIN_DATATYPES|La recherche floue spécifiée entre la colonne d'entrée « %1 » et la colonne de référence « %2 » n'est pas valide car les jonctions approximatives sont uniquement prises en charge entre des colonnes de chaîne, de types DT_STR et DT_WSTR.|  
|0xC020830C|-1071611124|DTS_E_TXFUZZYLOOKUP_INCOMPATIBLE_EXACT_JOIN_DATATYPES|Les colonnes de recherche exacte « %1 » et « %2 » ne possèdent pas de types de données égaux ou ne constituent pas des types de chaîne comparables. Les jonctions exactes sont prises en charge entre les colonnes qui comportent des types de données égaux ou une combinaison DT_STR et DT_WSTR.|  
|0xC020830D|-1071611123|DTS_E_TXFUZZYLOOKUP_INCOMPATIBLE_COPYCOLUMN_DATATYPES|Les colonnes de copie « %1 » et « %2 » ne possèdent pas de types de données égaux ou ne constituent pas des types de chaîne convertibles de façon triviale. Cela se produit car la copie depuis une référence vers une sortie entre des colonnes de types de données égaux ou une combinaison DT_STR et DT_WSTR est prise en charge, mais les autres types ne le sont pas.|  
|0xC020830E|-1071611122|DTS_E_TXFUZZYLOOKUP_INCOMPATIBLE_PASSTHRUCOLUMN_DATATYPES|Les colonnes de relais « %1 » et « %2 » ne possèdent pas de types de données égaux. Seules les colonnes comportant des types de données égaux sont prises en charge en tant que colonnes de relais depuis l'entrée vers la sortie.|  
|0xC020830F|-1071611121|DTS_E_TXFUZZYLOOKUP_UNABLETOLOCATEREFCOLUMN|Colonne de référence « %1 » introuvable.|  
|0xC0208311|-1071611119|DTS_E_TXFUZZYLOOKUP_OUTPUT_COLUMN_MUST_BE_PASSTHRU_COLUMN_OR_A_COPY_COLUMN|Une colonne de sortie doit exactement posséder une propriété CopyColumn ou PassThruColumn spécifiée. Cette erreur se produit lorsque les propriétés CopyColumn ou PassThruColumn, ou les deux ensemble sont définies en tant que valeurs non vides.|  
|0xC0208312|-1071611118|DTS_E_TXFUZZYLOOKUP_PASSTHRU_COLUMN_NOT_FOUND|L’ID de lignage source « %1!d! » spécifié pour la propriété « %2 » sur la colonne de sortie « %3 » est introuvable dans la collection de colonnes d’entrée. Cela se produit lorsque l'ID de la colonne d'entrée spécifié sur une colonne de sortie en tant que colonne de relais est introuvable dans l'ensemble des entrées.|  
|0xC0208313|-1071611117|DTS_E_TXFUZZYLOOKUP_INDEXED_COLUMN_NOT_FOUND_IN_REF_TABLE|La colonne « %1 » dans l'index avant génération « %2 » est introuvable dans la table/requête de référence. Cela se produit si le schéma/la requête de la table de référence a été modifié depuis la création de l'index de correspondance déjà existant.|  
|0xC0208314|-1071611116|DTS_E_TXFUZZYLOOKUP_TOKEN_TOO_LONG|Le composant a rencontré un jeton qui comporte plus de 2147483647 caractères.|  
|0xC0208315|-1071611115|DTS_E_RAWMETADATAMISMATCHTYPE|Impossible d'ajouter le fichier de sortie. Le nom de la colonne « %1 » correspond, mais la colonne du fichier possède le type %2 et la colonne d'entrée le type %3. Les types de données des métadonnées de la colonne ne correspondent pas.|  
|0xC0208316|-1071611114|DTS_E_RAWMETADATAMISMATCHSIZE|Impossible d'ajouter le fichier de sortie. Le nom de la colonne « %1 » correspond, mais la colonne du fichier possède une longueur maximale de %2!d! alors que la colonne d'entrée possède la longueur maximale %3!d!. La longueur des métadonnées de la colonne ne correspond pas.|  
|0xC0208317|-1071611113|DTS_E_RAWMETADATAMISMATCHCODEPAGE|Impossible d'ajouter le fichier de sortie. Le nom de la colonne « %1 » correspond, mais la colonne du fichier possède une page de codes %2!d! alors que la colonne d'entrée possède la page de codes %3!d!. Les pages de codes des métadonnées de la colonne nommée ne correspondent pas.|  
|0xC0208318|-1071611112|DTS_E_RAWMETADATAMISMATCHPRECISION|Impossible d'ajouter le fichier de sortie. Le nom de la colonne « %1 » correspond, mais la colonne du fichier comporte la précision %2!d! alors que la colonne d'entrée comporte la précision %3!d!. Les précisions des métadonnées de la colonne nommée ne correspondent pas.|  
|0xC0208319|-1071611111|DTS_E_RAWMETADATAMISMATCHSCALE|Impossible d'ajouter le fichier de sortie. Le nom de la colonne « %1 » correspond, mais la colonne du fichier possède l'échelle %2!d! alors que la colonne d'entrée possède l'échelle %3!d!.  Les échelles des métadonnées de la colonne nommée ne correspondent pas.|  
|0xC020831A|-1071611110|DTS_E_COULD_NOT_DETERMINE_DATASOURCE_DBMSNAME|Impossible de déterminer le nom et la version du système SGBD (système de gestion de bases de données) sur « %1 ». Cela se produit si IDBProperties sur la connexion n'a pas retourné les informations requises pour vérifier le nom et la version du système SGBD.|  
|0xC020831B|-1071611109|DTS_E_INCORRECT_SQL_SERVER_VERSION|Le type ou la version du système SGBD de « %1 » n'est pas pris en charge.  Une connexion a Microsoft SQL Server version 8.0 ou ultérieure est requise. Cela se produit si IDBProperties n'a pas retourné une version correcte sur la connexion.|  
|0xC020831D|-1071611107|DTS_E_CANTDELETEERRORCOLUMNS|%1 est une colonne de sortie d'erreur spéciale et ne peut pas être supprimée.|  
|0xC020831E|-1071611106|DTS_E_UNEXPECTEDCOLUMNDATATYPE|Le type de données spécifié pour la colonne « %1 » n'est pas le type attendu « %2 ».|  
|0xC020831F|-1071611105|DTS_E_INPUTCOLUMNNOTFOUND|L'ID de lignage de colonne d'entrée « %1 » référencé par la propriété « %2 » sur la colonne de sortie « %3 » est introuvable dans la collection de colonnes d'entrée.|  
|0xC0208320|-1071611104|DTS_E_TXGROUPDUPS_INPUTCOLUMNNOTJOINED|La colonne d'entrée « %1 » référencée par la propriété « %2 » sur la colonne de sortie « %3 » doit avoir la propriété ToBeCleaned=True ainsi qu'une valeur de propriété ExactFuzzy valide.|  
|0xC0208322|-1071611102|DTS_E_TXFUZZYLOOKUP_REF_TABLE_MISSING_IDENTITY_INDEX|La table de référence « %1 » ne possède pas d'index cluster sur une colonne d'identité d'entier, ce qui est nécessaire si la propriété « CopyRefTable » possède la valeur FALSE. Lorsque la valeur False est attribuée à la propriété CopyRefTable, la table de référence doit posséder un index cluster sur une colonne d'identité d'entier.|  
|0xC0208323|-1071611101|DTS_E_TXFUZZYLOOKUP_REF_CONTAINS_NON_INTEGER_IDENT_COLUMN|La table de référence « %1 » contient une colonne d'identité de type non entier qui n'est pas prise en charge. Utilisez une vue de la table sans la colonne « %2 ».  Cette erreur se produit lorsqu'une copie de la table de référence est effectuée, qu'une colonne d'identité d'entier est ajoutée, tandis qu'une seule colonne d'identité est autorisée par table.|  
|0xC0208324|-1071611100|DTS_E_TXFUZZY_MATCHCONTRIBUTION_AND_HIERARCHY_SPECIFIED|MatchContribution et les informations de hiérarchie ne peuvent pas être spécifiées en même temps, car ces deux propriétés constituent des facteurs de pondération pour le score.|  
|0xC0208325|-1071611099|DTS_E_TXFUZZY_HIERARCHY_INCORRECT|Les niveaux dans la hiérarchie doivent être des nombres uniques. Un niveau valide dans des valeurs hiérarchiques correspond à des entiers supérieurs ou égaux à 1. Plus le nombre est petit, plus la colonne est située au bas de la hiérarchie. La valeur par défaut 0 indique que la colonne ne fait pas partie d'une hiérarchie. Les chevauchements et les écarts ne sont pas autorisés.|  
|0xC0208326|-1071611098|DTS_E_TXFUZZYGROUPING_INSUFFICIENT_FUZZY_JOIN_COLUMNS|Aucune colonne vers un groupe probable n'est définie.  Une colonne d'entrée doit au moins exister avec les propriétés de colonne ToBeCleaned=true et ExactFuzzy=2.|  
|0xC0208329|-1071611095|DTS_E_TXFUZZYLOOKUP_COLUMNINVALID|La colonne portant l’ID « %1!d! » n’était pas valide pour une raison indéterminée.|  
|0xC020832A|-1071611094|DTS_E_TXFUZZYLOOKUP_UNSUPPORTEDDATATYPE|Le type de données de la colonne « %1 » n'est pas pris en charge.|  
|0xC020832C|-1071611092|DTS_E_TXFUZZYLOOKUP_OUTPUTLENGTHMISMATCH|La longueur de la colonne de sortie « %1 » est inférieure à celle de sa colonne source « %2 ».|  
|0xC020832F|-1071611089|DTS_E_TERMEXTRACTION_INCORRECTEXACTNUMBEROFINPUTCOLUMNS|Une seule colonne d'entrée est autorisée.|  
|0xC0208330|-1071611088|DTS_E_TERMEXTRACTION_INCORRECTEXACTNUMBEROFOUTPUTCOLUMNS|Il doit y avoir exactement deux colonnes de sortie.|  
|0xC0208331|-1071611087|DTS_E_TERMEXTRACTION_INCORRECTDATATYPEOFINPUTCOLUMN|La colonne d'entrée peut uniquement posséder les types de données DT_WSTR ou DT_NTEXT.|  
|0xC0208332|-1071611086|DTS_E_TERMEXTRACTION_INCORRECTDATATYPEOFOUTPUTCOLUMN|La colonne de sortie [%1!d!] peut uniquement posséder le type de données « %2 ».|  
|0xC0208333|-1071611085|DTS_E_TERMEXTRACTION_INCORRECTDATATYPEOFREFERENCECOLUMN|La colonne de référence peut uniquement posséder DT_STR ou DT_WSTR comme type de données.|  
|0xC0208334|-1071611084|DTS_E_TERMEXTRACTION_UNABLETOLOCATEREFCOLUMN|Une erreur s'est produite lors de la recherche de la colonne de référence « %1 ».|  
|0xC0208335|-1071611083|DTS_E_TERMEXTRACTION_INCORRECTTERMTYPE|Le type de terme de la transformation peut être uniquement WordOnly, PhraseOnly ou WordPhrase.|  
|0xC0208336|-1071611082|DTS_E_TERMEXTRACTION_INCORRECTFREQUENCYTHRESHOLD|La valeur du seuil de fréquence ne doit pas être inférieure à « %1!d! ».|  
|0xC0208337|-1071611081|DTS_E_TERMEXTRACTION_INCORRECTMAXLENOFTERM|La valeur de la longueur maximale du terme ne doit pas être inférieure à « %1!d! ».|  
|0xC0208338|-1071611080|DTS_E_TERMEXTRACTION_TOOFEWREFERENCECOLUMNS|Les métadonnées de référence d'extraction de terme contiennent un nombre de colonnes insuffisant.|  
|0xC0208339|-1071611079|DTS_E_TERMEXTRACTION_MALLOCERR_REFERENCECOLUMNINFO|Une erreur s'est produite lors de l'allocation de mémoire.|  
|0xC020833A|-1071611078|DTS_E_TERMEXTRACTION_MAINWORKSPACE_CREATEERR|Une erreur s'est produite lors de la création d'un tampon d'espace de travail.|  
|0xC020833B|-1071611077|DTS_E_TERMEXTRACTION_OLEDBERR_CREATEBINDING|Une erreur OLEDB s'est produite lors de la création de liaisons.|  
|0xC020833C|-1071611076|DTS_E_TERMEXTRACTION_OLEDBERR_GETIROWSET|Une erreur OLEDB s'est produite lors de l'extraction des ensembles de lignes.|  
|0xC020833D|-1071611075|DTS_E_TERMEXTRACTION_OLEDBERR_FILLBUFFER|Une erreur OLEDB s'est produite lors du remplissage du cache interne.|  
|0xC020833E|-1071611074|DTS_E_TERMEXTRACTION_PROCESSERR|Une erreur s'est produite lors de l'extraction de termes sur la ligne %1!ld!, colonne %2!ld!. Code d'erreur retourné : 0x%3!8.8X!. Pour résoudre ce problème, supprimez la ligne de l'entrée.|  
|0xC020833F|-1071611073|DTS_E_TERMEXTRACTIONORLOOKUP_PROCESSERR_DEPOSITFULL|Le nombre de termes candidats est supérieur à la limite de 4G.|  
|0xC0208340|-1071611072|DTS_E_TERMEXTRACTION_INVALIDOUTTERMTABLEORCOLUMN|La table, la vue ou la colonne de référence utilisée pour les termes d'exclusion n'est pas valide.|  
|0xC0208341|-1071611071|DTS_E_TXFUZZYLOOKUP_STRINGCOLUMNTOOLONG|La longueur de la colonne de chaîne « %1 » dépasse 4000 caractères.  Une conversion de DT_STR vers DT_WSTR est nécessaire, afin qu'une troncature se produise.  Réduisez la largeur de la colonne, ou utilisez uniquement les types de colonnes DT_WSTR.|  
|0xC0208342|-1071611070|DTS_E_TERMEXTRACTION_OUTTERMTABLEANDCOLUMNNOTSET|La table, la vue ou la colonne de référence à utiliser pour les termes d'exclusion n'a pas été définie.|  
|0xC0208343|-1071611069|DTS_E_TERMLOOKUP_TOOFEWOUTPUTCOLUMNS|La recherche de termes contient un nombre de colonnes de sortie insuffisant.|  
|0xC0208344|-1071611068|DTS_E_TERMLOOKUP_INCORRECTDATATYPEOFREFERENCECOLUMN|La colonne de référence peut uniquement posséder DT_STR ou DT_WSTR comme type de données.|  
|0xC0208345|-1071611067|DTS_E_TERMLOOKUP_UNABLETOLOCATEREFCOLUMN|Une erreur s'est produite lors de la recherche de la colonne de référence « %1 ».|  
|0xC0208346|-1071611066|DTS_E_TERMLOOKUP_TOOFEWREFERENCECOLUMNS|Les métadonnées de référence de recherche de terme contiennent un nombre de colonnes insuffisant.|  
|0xC0208347|-1071611065|DTS_E_TERMEXTRACTIONORLOOKUP_TESTOFFSETERROR|Une erreur s'est produite au cours de la normalisation de mots.|  
|0xC0208348|-1071611064|DTS_E_TERMLOOKUP_MAINWORKSPACE_CREATEERR|Une erreur s'est produite lors de la création d'un tampon d'espace de travail.|  
|0xC0208349|-1071611063|DTS_E_TERMLOOKUP_OLEDBERR_CREATEBINDING|Une erreur OLEDB s'est produite lors de la création de liaisons.|  
|0xC020834A|-1071611062|DTS_E_TERMLOOKUP_OLEDBERR_GETIROWSET|Une erreur OLEDB s'est produite lors de l'extraction des ensembles de lignes.|  
|0xC020834B|-1071611061|DTS_E_TERMLOOKUP_OLEDBERR_FILLBUFFER|Une erreur OLEDB s'est produite lors du remplissage du cache interne.|  
|0xC020834C|-1071611060|DTS_E_TERMLOOKUP_PROCESSERR|Une erreur s'est produite lors de la recherche de termes à la ligne %1!ld!, colonne %2!ld!. Code d'erreur retourné : 0x%3!8.8X!. Pour résoudre ce problème, supprimez la ligne de l'entrée.|  
|0xC020834D|-1071611059|DTS_E_TERMLOOKUP_TEXTIDINPUTCOLUMNNOTMAPPEDWITHOUTPUTCOLUMN|Une colonne Relais au minimum n'est pas mappée à une colonne de sortie.|  
|0xC020834E|-1071611058|DTS_E_TERMLOOKUP_INCORRECTEXACTNUMBEROFTEXTCOLUMNS|Une colonne d'entrée exactement doit être mappée à une colonne de référence.|  
|0xC020834F|-1071611057|DTS_E_TERMLOOKUP_TEXTINPUTCOLUMNHAVEINCORRECTDATATYPE|La colonne d'entrée mappée à une colonne de référence peut uniquement posséder le type de données DT_NTXT ou DT_WSTR.|  
|0xC0208354|-1071611052|DTS_E_TXFUZZYLOOKUP_INVALID_MATCH_INDEX_NAME|Le nom de la table de référence « %1 » n'est pas un identificateur SQL valide. Cette erreur se produit lorsque le nom de table ne peut être analysé à partir de la chaîne d'entrée. Le nom doit comprendre des espaces sans guillemets. Vérifiez que le nom possède des guillemets corrects.|  
|0xC0208355|-1071611051|DTS_E_TERMEXTRACTION_TERMFILTERSTARTITERATIONERROR|Une erreur s'est produite lors du début de l'itération du filtre terminologique.|  
|0xC0208356|-1071611050|DTS_E_TERMEXTRACTION_EMPTYTERMRESULTERROR|Une erreur s'est produite lors de la récupération du tampon utilisé pour la mise en cache des termes. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208357|-1071611049|DTS_E_TERMEXTRACTION_STDLENGTHERROR|Une erreur std::length_error s'est produite à partir des conteneurs STL.|  
|0xC0208358|-1071611048|DTS_E_TERMLOOKUP_SAVEWORDWITHPUNCTERROR|Une erreur s'est produite lors de l'enregistrement de mots avec des caractères de ponctuation. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208359|-1071611047|DTS_E_TERMLOOKUP_ADDREFERENCETERM|Une erreur s'est produite lors du traitement du terme de référence %1!ld!th. Code d'erreur retourné : 0x%2!8.8X!. Supprimez le terme de référence de votre table de référence pour résoudre ce problème.|  
|0xC020835A|-1071611046|DTS_E_TERMLOOKUP_SORREFERENCETERM|Une erreur s'est produite lors du tri des termes de référence. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020835B|-1071611045|DTS_E_TERMLOOKUP_COUNTTERM|Une erreur s'est produite lors du comptage des termes candidats. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020835C|-1071611044|DTS_E_FUZZYLOOKUP_REFERENCECACHEFULL|La recherche floue n'a pas pu charger la totalité de la table de référence dans la mémoire principale, comme requis lorsque la propriété Exhaustive est activée.  Soit la mémoire système était insuffisante, soit une limite a été spécifiée pour la propriété MaxMemoryUsage, qui était insuffisante pour le chargement de la table de référence.  Définissez la propriété MaxMemoryUsage à la valeur 0 ou augmentez significativement sa valeur.  Une autre solution consiste à désactiver la propriété Exhaustive.|  
|0xC020835D|-1071611043|DTS_E_TERMLOOKUP_INITIALIZE|Une erreur s'est produite lors de l'initialisation du moteur de recherche de termes. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020835E|-1071611042|DTS_E_TERMLOOKUP_PROCESSSENTENCE|Une erreur s'est produite lors du traitement des phrases. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020835F|-1071611041|DTS_E_TEXTMININGBASE_APPENDTOTEMPBUFFER|Une erreur s'est produite lors de l'ajout de chaînes à un tampon interne. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208360|-1071611040|DTS_E_TERMEXTRACTION_SAVEPOSTAG|Une erreur s'est produite au cours de l'enregistrement de balises morphosyntaxiques à partir d'un tampon interne. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208361|-1071611039|DTS_E_TERMEXTRACTION_COUNTTERM|Une erreur s'est produite lors du comptage des termes candidats. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208362|-1071611038|DTS_E_TERMEXTRACTION_INITPOSPROCESSOR|Une erreur s'est produite au cours de l'initialisation du processeur morphosyntaxique. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208363|-1071611037|DTS_E_TERMEXTRACTION_INITFSA|Une erreur s'est produite lors du chargement de l'automate à états finis (FSM). Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208364|-1071611036|DTS_E_TERMEXTRACTION_INITIALIZE|Une erreur s'est produite lors de l'initialisation du moteur d'extraction de termes. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208365|-1071611035|DTS_E_TERMEXTRACTION_PROCESSSENTENCE|Une erreur s'est produite au cours du traitement au sein d'une phrase. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208366|-1071611034|DTS_E_TERMEXTRACTION_INITPOSTAGVECTOR|Une erreur s'est produite au cours de l'initialisation du processeur morphosyntaxique. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208367|-1071611033|DTS_E_TERMEXTRACTION_SAVEPTRSTRING|Une erreur s'est produite lors de l'ajout de chaînes à un tampon interne. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208368|-1071611032|DTS_E_TERMEXTRACTION_ADDWORDTODECODER|Une erreur s'est produite lors de l'ajout de mots à un décodeur de statistiques. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC0208369|-1071611031|DTS_E_TERMEXTRACTION_DECODE|Une erreur s'est produite lors du décodage d'une phrase. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020836A|-1071611030|DTS_E_TERMEXTRACTION_SETEXCLUDEDTERM|Une erreur s'est produite lors du paramétrage des termes d'exclusion. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020836B|-1071611029|DTS_E_TERMEXTRACTION_PROCESSDOCUMENT|Une erreur s'est produite lors du traitement d'un document dans l'entrée. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020836C|-1071611028|DTS_E_TEXTMININGBASE_TESTPERIOD|Une erreur s'est produite lors d'un test servant à déterminer si un point fait partie d'un acronyme. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020836D|-1071611027|DTS_E_TERMLOOKUP_ENGINEADDREFERENCETERM|Une erreur s'est produite lors du paramétrage des termes de référence. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020836E|-1071611026|DTS_E_TERMLOOKUP_PROCESSDOCUMENT|Une erreur s'est produite lors du traitement d'un document dans l'entrée. Code d'erreur retourné : 0x%1!8.8X!.|  
|0xC020836F|-1071611025|DTS_E_INVALIDBULKINSERTPROPERTYVALUE|La valeur de la propriété %1 est %2!d!, ce qui n'est pas autorisé.  Cette valeur doit être supérieure ou égale à %3!d!.|  
|0xC0208370|-1071611024|DTS_E_INVALIDBULKINSERTFIRSTROWLASTROWVALUES|La valeur de la propriété %1 est %2!d!, qui doit être inférieure ou égale à la valeur %3!d! pour la propriété %4.|  
|0xC0208371|-1071611023|DTS_E_FUZZYLOOKUPUNABLETODELETEEXISTINGMATCHINDEX|Une erreur s'est produite lors de la tentative de suppression de l'index de correspondance approximative nommé « %1 ». Il est possible que cette table n'ait pas été créée par la recherche floue (ou cette version de la recherche floue), qu'elle ait été endommagée ou qu'il existe un autre problème. Essayez de supprimer manuellement la table nommée « %2 » ou spécifiez un nom différent pour la propriété MatchIndexName.|  
|0xC0208372|-1071611022|DTS_E_TERMEXTRACTION_INCORRECTSCORETYPE|Le type de score de la transformation peut uniquement être Fréquence ou TFIDF.|  
|0xC0208373|-1071611021|DTS_E_FUZZYLOOKUPREFTABLETOOBIG|La table de référence spécifiée possède trop de lignes. La recherche floue ne fonctionne qu'avec des tables de référence comportant moins d'un milliard de lignes. Essayez d'utiliser une vue plus petite de votre table de référence.|  
|0xC0208374|-1071611020|DTS_E_FUZZYLOOKUPUNABLETODETERMINEREFERENCETABLESIZE|Impossible de déterminer la taille de la table de référence « %1 ».  Il est possible que cet objet soit une vue et pas une table.  La recherche floue ne prend pas en charge les vues lorsque CopyReferentaceTable possède la valeur False.  Vérifiez que cette table existe et que CopyReferenceTable possède la valeur True.|  
|0xC0208377|-1071611017|DTS_E_XMLSRCOUTPUTCOLUMNDATATYPENOTSUPPORTED|Le type de données de la tâche de flux de données SSIS « %1 » sur %2 n'est pas pris en charge pour %3.|  
|0xC0208378|-1071611016|DTS_E_XMLSRCCANNOTFINDCOLUMNTOSETDATATYPE|Impossible de définir les propriétés de type de données pour la colonne de sortie portant l'ID %1!d! sur la sortie portant l'ID %2!d!. La sortie ou la colonne est introuvable.|  
|0xC0208379|-1071611015|DTS_E_CUSTOMPROPERTYISREADONLY|Impossible de modifier la valeur de la propriété personnalisée « %1 » sur %2.|  
|0xC020837A|-1071611014|DTS_E_OUTPUTCOLUMNHASNOERRORCOLUMN|%1 sur la sortie sans erreur ne possède pas de colonne de sortie correspondante sur la sortie d'erreur.|  
|0xC020837B|-1071611013|DTS_E_ERRORCOLUMNHASNOOUTPUTCOLUMN|%1 sur la sortie d'erreur ne possède pas de colonne de sortie correspondante sur la sortie sans erreur.|  
|0xC020837C|-1071611012|DTS_E_ERRORCOLUMNHASINCORRECTPROPERTIES|%1 sur la sortie d'erreur possède des propriétés qui ne correspondent pas aux propriétés de la colonne source de données correspondante.|  
|0xC020837D|-1071611011|DTS_E_ADOSRCOUTPUTCOLUMNDATATYPECANNOTBECHANGED|Impossible de modifier le type de données des colonnes de sortie sur %1, sauf pour les colonnes DT_WSTR et DT_NTEXT.|  
|0xC020837F|-1071611009|DTS_E_ADOSRCDATATYPEMISMATCH|Le type de données de « %1 » ne correspond pas au type de données « %2 » de la colonne source « %3 ».|  
|0xC0208380|-1071611008|DTS_E_ADOSRCCOLUMNNOTINSCHEMAROWSET|%1 ne possède pas de colonne source correspondante dans le schéma.|  
|0xC0208381|-1071611007|DTS_E_TERMLOOKUP_INVALIDREFERENCETERMTABLEORCOLUMN|La table/vue de référence ou la colonne utilisée pour les termes de référence n'est pas valide.|  
|0xC0208382|-1071611006|DTS_E_TERMLOOKUP_REFERENCETERMTABLEANDCOLUMNNOTSET|La table/vue de référence ou la colonne utilisée pour les termes de référence n'a pas été définie.|  
|0xC0208383|-1071611005|DTS_E_COLUMNMAPPEDTOALREADYMAPPEDEXTERNALMETADATACOLUMN|La colonne %1 est mappée sur la colonne de métadonnées externes avec l'ID %2!ld!, qui est déjà mappée sur une autre colonne.|  
|0xC0208384|-1071611004|DTS_E_TXFUZZYLOOKUP_TOOMANYPREFIXES|Le nom d'objet SQL « %1 » spécifié pour la propriété « %2 » contient plus de préfixes que le maximum autorisé.  Le maximum est 2.|  
|0xC0208385|-1071611003|DTS_E_MGDSRCSTATIC_OVERFLOW|La valeur était trop grande pour tenir dans la colonne.|  
|0xC0208386|-1071611002|DTS_E_DATAREADERDESTREADERISCLOSED|Le IDataReader SSIS est fermé.|  
|0xC0208387|-1071611001|DTS_E_DATAREADERDESTREADERISATEND|SSIS IDataReader se trouve au-delà de la fin du jeu de résultats.|  
|0xC0208388|-1071611000|DTS_E_DATAREADERDESTINVALIDCOLUMNORDINAL|La position ordinale de la colonne n'est pas valide.|  
|0xC0208389|-1071610999|DTS_E_DATAREADERDESTCANNOTCONVERT|Impossible de convertir le %1 à partir du type de données « %2 » vers le type de données « %3 ».|  
|0xC020838A|-1071610998|DTS_E_DATAREADERDESTINVALIDCODEPAGE|Le %1 comporte une page de codes non prise en charge %2!d!.|  
|0xC020838B|-1071610997|DTS_E_XMLSRCEXTERNALMETADATACOLUMNNOTINSCHEMA|Le %1 ne comporte pas de mappage au schéma XML.|  
|0xC020838D|-1071610995|DTS_E_TXTERMLOOKUP_MISMATCHED_COLUMN_METADATA|Les colonnes avec les ID de lignage %1!d! et %2!d! comportent des métadonnées non concordantes. La colonne d’entrée qui est mappée à une colonne de sortie ne comporte pas les mêmes métadonnées (type de données, précision, échelle, longueur ou page de codes).|  
|0xC020838E|-1071610994|DTS_E_DATAREADERDESTREADERTIMEOUT|Le IDataReader SSIS est fermé. Le délai d'attente de lecture a expiré.|  
|0xC020838F|-1071610993|DTS_E_ADOSRCINVALIDSQLCOMMAND|Une erreur s'est produite lors de l'exécution de la commande SQL fournie : « %1 ». %2|  
|0xC0208390|-1071610992|DTS_E_JOINTYPEDOESNTMATCHETI|La propriété JoinType spécifiée pour la colonne d'entrée « %1 » est différente de celle spécifiée pour la colonne de la table de référence correspondante lorsque l'index de correspondance a été initialement créé.  Vous devez reconstruire l'index de correspondances avec la propriété JoinType donnée ou modifier la propriété JoinType afin qu'elle corresponde au type utilisé lors de la création de l'index de correspondances.|  
|0xC0208392|-1071610990|DTS_E_SQLCEDESTDATATYPENOTSUPPORTED|Le type de données « %1! » rencontré sur la colonne « %2! » n'est pas pris en charge pour %3.|  
|0xC0208393|-1071610989|DTS_E_DATAREADERDESTDATATYPENOTSUPPORTED|Le type de données « %1! » rencontré sur %2 n'est pas pris en charge pour %3.|  
|0xC0208394|-1071610988|DTS_E_RECORDSETDESTDATATYPENOTSUPPORTED|Le type de données de %1 n'est pas pris en charge pour %2.|  
|0xC0208446|-1071610810|DTS_E_TXSCRIPTMIGRATIONCOULDNOTADDREFERENCE|Impossible d'ajouter la référence de projet « %1 » lors de la migration de %2. Celle-ci devra peut-être être terminée manuellement.|  
|0xC0208447|-1071610809|DTS_E_TXSCRIPTMIGRATIONMULTIPLEENTRYPOINTSFOUND|Plusieurs points d'entrée nommés « %1 » ont été trouvés lors de la migration de %2. Celle-ci devra peut-être être terminée manuellement.|  
|0xC0208448|-1071610808|DTS_E_TXSCRIPTMIGRATIONNOENTRYPOINTFOUND|Aucun point d'entrée n'a été trouvé lors de la migration de %1. Celle-ci devra peut-être être terminée manuellement.|  
|0xC020844B|-1071610805|DTS_E_ADODESTINSERTIONFAILURE|Une exception s'est produite lors de l'insertion des données ; le message retourné par le fournisseur est : %1|  
|0xC020844C|-1071610804|DTS_E_ADODESTCONNECTIONTYPENOTSUPPORTED|Échec de la récupération du nom invariant de fournisseur à partir de %1. Il n'est pas pris en charge actuellement par le composant de destination ADO NET.|  
|0xC020844D|-1071610803|DTS_E_ADODESTARGUMENTEXCEPTION|Une exception d'argument s'est produite pendant que le fournisseur de données essayait d'insérer des données dans la destination. Le message retourné est : %1|  
|0xC020844E|-1071610802|DTS_E_ADODESTWRONGBATCHSIZE|La propriété BatchSize doit être un entier non négatif.|  
|0xC020844F|-1071610801|DTS_E_ADODESTERRORUPDATEROW|Une erreur s'est produite lors de l'envoi de cette ligne à source de données de destination.|  
|0xC0208450|-1071610800|DTS_E_ADODESTEXECUTEREADEREXCEPTION|L'exécution de la commande de tSQL lève une exception, le message est : %1|  
|0xC0208451|-1071610799|DTS_E_ADODESTDATATYPENOTSUPPORTED|Le type de données « %1! » rencontré sur la colonne « %2! » n'est pas pris en charge pour %3.|  
|0xC0208452|-1071610798|DTS_E_ADODESTFAILEDTOACQUIRECONNECTION|La destination ADO NET n'a pas réussi à acquérir la connexion %1. La connexion a peut-être été endommagée.|  
|0xC0208453|-1071610797|DTS_E_ADODESTNOTMANAGEDCONNECTION|La connexion spécifiée %1 n'étant pas managée, utilisez une connexion managée pour la destination ADO NET.|  
|0xC0208454|-1071610796|DTS_E_ADODESTNOERROROUTPUT|Le composant de destination ne dispose d'aucune sortie d'erreur. Il a peut-être été endommagé.|  
|0xC0208455|-1071610795|DTS_E_ADODESTNOLINEAGEID|Le lineageID %1 associé à la colonne externe %2 n'existe pas au moment de l'exécution.|  
|0xC0208456|-1071610794|DTS_E_ADODESTEXTERNALCOLNOTEXIST|Le %1 n'existe pas dans la base de données. Il a peut-être été supprimé ou renommé.|  
|0xC0208457|-1071610793|DTS_E_ADODESTGETSCHEMATABLEFAILED|Impossible d'obtenir les propriétés des colonnes externes. Le nom de table que vous avez entré n'existe peut-être pas, ou vous ne disposez pas de l'autorisation SELECT sur l'objet table et une autre tentative d'obtention des propriétés des colonnes par le biais de la connexion a échoué. Les messages d'erreur détaillés sont : %1|  
|0xC0208458|-1071610792|DTS_E_ADODESTCOLUMNERRORDISPNOTSUPPORTED|La disposition d'erreur de colonne d'entrée n'est pas prise en charge par le composant de destination ADO NET.|  
|0xC0208459|-1071610791|DTS_E_ADODESTCOLUMNTRUNDISPNOTSUPPORTED|La disposition de troncation de colonne d'entrée n'est pas prise en charge par le composant de destination ADO NET.|  
|0xC020845A|-1071610790|DTS_E_ADODESTINPUTTRUNDISPNOTSUPPORTED|La disposition de la ligne de troncation d'entrée n'est pas prise en charge par le composant de destination ADO NET.|  
|0xC020845B|-1071610789|DTS_E_ADODESTTABLENAMEERROR|Le nom de la table ou de la vue n'est pas prévu. \n\t Si vous citez le nom de la table, utilisez le préfixe %1 et le suffixe %2 de votre fournisseur de données sélectionné pour citation. \n\t Si vous utilisez un nom en plusieurs parties, utilisez au maximum trois parties pour le nom de la table.|  
|0xC0209001|-1071607807|DTS_E_FAILEDTOFINDCOLUMNINBUFFER|Échec de la recherche de la colonne « %1 » portant l'ID de lignage %2!d! dans le tampon. Le gestionnaire de tampons a retourné le code d'erreur 0x%3!8.8X!.|  
|0xC0209002|-1071607806|DTS_E_FAILEDTOGETCOLUMNINFOFROMBUFFER|Échec de l'obtention d'informations pour la colonne « %1 » (%2!d!) à partir du tampon. Code d'erreur retourné : 0x%3!8.8X!.|  
|0xC0209011|-1071607791|DTS_E_TXAGG_ARITHMETICOVERFLOW|Débordement arithmétique rencontré lors de l'agrégation de « %1 ».|  
|0xC0209012|-1071607790|DTS_E_FAILEDTOGETCOLINFO|Échec de l'obtention d'informations pour la ligne %1!ld!, colonne %2!ld! à partir du tampon. Code d'erreur retourné : 0x%3!8.8X!.|  
|0xC0209013|-1071607789|DTS_E_FAILEDTOSETCOLINFO|Échec de la définition d'informations pour la ligne %1!ld!, colonne %2!ld! dans le tampon. Code d'erreur retourné : 0x%3!8.8X!.|  
|0xC0209015|-1071607787|DTS_E_REQUIREDBUFFERISNOTAVAILBLE|Un tampon requis n'est pas disponible.|  
|0xC0209016|-1071607786|DTS_E_FAILEDTOGETBUFFERBOUNDARYINFO|Échec de la tentative d'obtention des informations de limite de tampon. Code d'erreur : 0x%1!8.8X!.|  
|0xC0209017|-1071607785|DTS_E_FAILEDTOSETBUFFERENDOFROWSET|Échec de la définition de la fin de l'ensemble de lignes du tampon. Code d'erreur : 0x%1!8.8X!.|  
|0xC0209018|-1071607784|DTS_E_FAILEDTOGETDATAFORERROROUTPUTBUFFER|Échec de l'obtention de données pour le tampon de sortie d'erreur.|  
|0xC0209019|-1071607783|DTS_E_FAILEDTOREMOVEROWFROMBUFFER|Échec de la suppression d'une ligne du tampon. Code d'erreur : 0x%1!8.8X!.|  
|0xC020901B|-1071607781|DTS_E_FAILEDTOSETBUFFERERRORINFO|Échec de la tentative de définition des informations d'erreur sur le tampon. Code d'erreur : 0x%1!8.8X!.|  
|0xC020901C|-1071607780|DTS_E_COLUMNSTATUSERROR|Une erreur s'est produite avec %1 sur %2. État de colonne retourné : « %3 ».|  
|0xC020901D|-1071607779|DTS_E_TXLOOKUP_METADATAXMLCACHEERR|Mise en cache des métadonnées de référence impossible.|  
|0xC020901E|-1071607778|DTS_E_TXLOOKUP_ROWLOOKUPERROR|Aucune correspondance de ligne trouvée au cours de la recherche.|  
|0xC020901F|-1071607777|DTS_E_INVALIDERRORDISPOSITION|%1 présente une erreur ou une structure de ligne tronquée non valide.|  
|0xC0209022|-1071607774|DTS_E_FAILEDTODIRECTERRORROW|Échec de la direction de la ligne vers la sortie d'erreur. Code d'erreur : 0x%1!8.8X!.|  
|0xC0209023|-1071607773|DTS_E_FAILEDTOPREPARECOLUMNSTATUSESFORINSERT|Échec de la préparation des états de colonne pour l'insertion. Code d'erreur : 0x%1!8.8X!.|  
|0xC0209024|-1071607772|DTS_E_FAILEDTOFINDCOLUMNBYLINEAGEID|Échec d'une tentative de recherche de %1 avec l'ID de lignage %2!d! dans le tampon de tâche de flux de données. Code d'erreur : 0x%3!8.8X!.|  
|0xC0209025|-1071607771|DTS_E_FAILEDTOFINDNONSPECIALERRORCOLUMN|Échec de la recherche d'une colonne d'erreur non spéciale dans %1.|  
|0xC0209029|-1071607767|DTS_E_INDUCEDTRANSFORMFAILUREONERROR|Code d'erreur SSIS DTS_E_INDUCEDTRANSFORMFAILUREONERROR.  Échec de l'objet « %1 » en raison du code d'erreur 0x%2!8.8X!. En outre, la disposition de la ligne d'erreur sur « %3 » spécifie un échec sur l'erreur. Une erreur s'est produite sur l'objet spécifié du composant spécifié.  Des messages d'erreur peuvent être envoyés au préalable avec des informations indiquant la raison de l'échec.|  
|0xC020902A|-1071607766|DTS_E_INDUCEDTRANSFORMFAILUREONTRUNCATION|Échec de « %1 » en raison d'une troncation. En outre, la disposition de la ligne de troncation sur « %2 » indique une erreur au niveau de la troncation. Une erreur de troncation s'est produite sur l'objet spécifié du composant spécifié.|  
|0xC020902B|-1071607765|DTS_E_TXSPLITEXPRESSIONEVALUATEDTONULL|L'expression « %1 » sur « %2 » a été évaluée à NULL, mais « %3 » nécessite des résultats booléens. Modifiez la disposition de la ligne d'erreur sur la sortie pour traiter ce résultat comme False (Ignorer l'échec) ou rediriger cette ligne vers la sortie d'erreur (Réacheminer la ligne).  Les résultats de cette expression doivent être de type booléen pour un fractionnement conditionnel.  Un résultat d'expression NULL constitue une erreur.|  
|0xC020902C|-1071607764|DTS_E_TXSPLITSTATIC_EXPRESSIONEVALUATEDTONULL|L'expression a été évaluée à NULL, mais un résultat booléen est requis. Modifiez la disposition de la ligne d'erreur sur la sortie pour traiter ce résultat comme False (Ignorer l'échec) ou rediriger cette ligne vers la sortie d'erreur (Réacheminer la ligne). Les résultats de cette expression doivent être de type booléen pour un fractionnement conditionnel.  Un résultat d'expression NULL constitue une erreur.|  
|0xC020902D|-1071607763|DTS_E_UTF16BIGENDIANFORMATNOTSUPPORTED|Le format de fichier UTF-16 avec primauté des octets de poids fort (Big endian) n'est pas pris en charge.  Seul le format UTF-16 avec primauté des octets de poids faible (Little endian) est pris en charge.|  
|0xC020902E|-1071607762|DTS_E_UTF8FORMATNOTSUPPORTEDASUNICODE|Le format de fichier UTF-8 n'est pas pris en charge en tant qu'Unicode.|  
|0xC020902F|-1071607761|DTS_E_DTPXMLCANTREADIDATTR|Attribut d'ID illisible.|  
|0xC020903E|-1071607746|DTS_E_TXLOOKUP_INDEXCOLUMNREUSED|La colonne d'index du cache %1 est référencée par plusieurs colonnes d'entrée de recherche.|  
|0xC020903F|-1071607745|DTS_E_TXLOOKUP_INDEXCOLUMNSMISMATCH|La recherche ne fait pas référence à toutes les colonnes d'index du gestionnaire de connexions du cache. Nombre de colonnes jointes dans la recherche : %1!d!. Nombre de colonnes d'index : %2!d!.|  
|0xC0209069|-1071607703|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_CANTCONVERTVALUE|Impossible de convertir la valeur des données pour une raison autre que la non-correspondance des signes ou le débordement des données.|  
|0xC020906A|-1071607702|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_SCHEMAVIOLATION|La valeur des données ne respecte pas la contrainte du schéma.|  
|0xC020906B|-1071607701|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_TRUNCATED|Les données ont été tronquées.|  
|0xC020906C|-1071607700|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_SIGNMISMATCH|Échec de la conversion car la valeur des données était signée alors que le type utilisé par le fournisseur ne l'était pas.|  
|0xC020906D|-1071607699|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_DATAOVERFLOW|Échec de la conversion, en raison du débordement de la valeur des données par rapport au type utilisé par le fournisseur.|  
|0xC020906E|-1071607698|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_UNAVAILABLE|Aucun état n'est disponible.|  
|0xC020906F|-1071607697|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_PERMISSIONDENIED|L'utilisateur ne possédait pas les autorisations correctes pour écrire dans la colonne.|  
|0xC0209070|-1071607696|DTS_E_COMMANDDESTINATIONADAPTERSTATIC_INTEGRITYVIOLATION|La valeur des données n'a pas respecté les contraintes d'intégrité de la colonne.|  
|0xC0209071|-1071607695|DTS_E_OLEDBSOURCEADAPTERSTATIC_UNAVAILABLE|Aucun état n'est disponible.|  
|0xC0209072|-1071607694|DTS_E_OLEDBSOURCEADAPTERSTATIC_CANTCONVERTVALUE|Impossible de convertir la valeur des données pour une raison autre que la non-correspondance des signes ou le débordement des données.|  
|0xC0209073|-1071607693|DTS_E_OLEDBSOURCEADAPTERSTATIC_TRUNCATED|Les données ont été tronquées.|  
|0xC0209074|-1071607692|DTS_E_OLEDBSOURCEADAPTERSTATIC_SIGNMISMATCH|Échec de la conversion car la valeur des données était signée alors que le type utilisé par le fournisseur ne l'était pas.|  
|0xC0209075|-1071607691|DTS_E_OLEDBSOURCEADAPTERSTATIC_DATAOVERFLOW|Échec de la conversion, en raison du débordement de la valeur des données par rapport au type utilisé par le fournisseur.|  
|0xC0209076|-1071607690|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_SCHEMAVIOLATION|La valeur des données ne respecte pas la contrainte du schéma.|  
|0xC0209077|-1071607689|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_CANTCONVERTVALUE|Impossible de convertir la valeur des données pour une raison autre que la non-correspondance des signes ou le débordement des données.|  
|0xC0209078|-1071607688|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_TRUNCATED|Les données ont été tronquées.|  
|0xC0209079|-1071607687|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_SIGNMISMATCH|Échec de la conversion car la valeur des données était signée alors que le type utilisé par le fournisseur ne l'était pas.|  
|0xC020907A|-1071607686|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_DATAOVERFLOW|Échec de la conversion, en raison du débordement de la valeur des données par rapport au type utilisé par le fournisseur.|  
|0xC020907B|-1071607685|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_UNAVAILABLE|Aucun état n'est disponible.|  
|0xC020907C|-1071607684|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_PERMISSIONDENIED|L'utilisateur ne possédait pas les autorisations correctes pour écrire dans la colonne.|  
|0xC020907D|-1071607683|DTS_E_OLEDBDESTINATIONADAPTERSTATIC_INTEGRITYVIOLATION|La valeur des données ne respecte pas les contraintes d'intégrité.|  
|0xC020907F|-1071607681|DTS_E_TXDATACONVERTSTATIC_CANTCONVERTVALUE|Impossible de convertir la valeur des données pour une raison autre que la non-correspondance des signes ou le débordement des données.|  
|0xC0209080|-1071607680|DTS_E_TXDATACONVERTSTATIC_TRUNCATED|Les données ont été tronquées.|  
|0xC0209081|-1071607679|DTS_E_TXDATACONVERTSTATIC_SIGNMISMATCH|Échec de la conversion car la valeur des données était signée alors que le type utilisé par le fournisseur ne l'était pas.|  
|0xC0209082|-1071607678|DTS_E_TXDATACONVERTSTATIC_DATAOVERFLOW|Échec de la conversion en raison du débordement de la valeur des données par rapport au type utilisé par la transformation de conversion des données.|  
|0xC0209083|-1071607677|DTS_E_FLATFILESOURCEADAPTERSTATIC_UNAVAILABLE|Aucun état n'est disponible.|  
|0xC0209084|-1071607676|DTS_E_FLATFILESOURCEADAPTERSTATIC_CANTCONVERTVALUE|Impossible de convertir la valeur des données pour une raison autre que la non-correspondance des signes ou le débordement des données.|  
|0xC0209085|-1071607675|DTS_E_FLATFILESOURCEADAPTERSTATIC_TRUNCATED|Les données ont été tronquées.|  
|0xC0209086|-1071607674|DTS_E_FLATFILESOURCEADAPTERSTATIC_SIGNMISMATCH|Échec de la conversion car la valeur des données était signée, alors que le type utilisé par l'adaptateur source de fichier plat ne l'était pas.|  
|0xC0209087|-1071607673|DTS_E_FLATFILESOURCEADAPTERSTATIC_DATAOVERFLOW|Échec de la conversion en raison du débordement de la valeur des données par rapport au type utilisé par l'adaptateur source de fichier plat.|  
|0xC020908E|-1071607666|DTS_E_TXDATACONVERTSTATIC_UNAVAILABLE|Aucun état n'est disponible.|  
|0xC0209090|-1071607664|DTS_E_FILEOPENERR_FORREAD|Échec de l'ouverture du fichier « %1 » en lecture : code d'erreur 0x%2!8.8X!.|  
|0xC0209091|-1071607663|DTS_E_TXFILEINSERTERSTATIC_FILEOPENERR_FORREAD|Échec de l'ouverture du fichier en lecture.|  
|0xC0209092|-1071607662|DTS_E_FILEOPENERR_FORWRITE|Échec de l'ouverture du fichier « %1 » en écriture : code d'erreur 0x%2!8.8X!.|  
|0xC0209093|-1071607661|DTS_E_TXFILEEXTRACTORSTATIC_FILEOPENERR_FORWRITE|Échec de l'ouverture du fichier en écriture.|  
|0xC0209094|-1071607660|DTS_E_TXFILEINSERTERSTATIC_INSERTERCANTREAD|Échec de la lecture à partir du fichier.|  
|0xC0209095|-1071607659|DTS_E_TXFILEEXTRACTORSTATIC_EXTRACTORCANTWRITE|Échec de l'écriture dans le fichier.|  
|0xC0209099|-1071607655|DTS_E_DTPXMLINVALIDPROPERTYARRAYTOOMANYVALUES|Un nombre trop important d'éléments de tableau a été trouvé lors de l'analyse d'une propriété de type tableau. La valeur d'elementCount est inférieure au nombre d'éléments de tableau trouvés.|  
|0xC020909A|-1071607654|DTS_E_DTPXMLINVALIDPROPERTYARRAYNOTENOUGHVALUES|Un nombre trop faible d'éléments de tableau a été trouvé lors de l'analyse d'une propriété de type tableau. La valeur d'elementCount est supérieure au nombre d'éléments de tableau trouvés.|  
|0xC020909E|-1071607650|DTS_E_FILEOPENERR_FORWRITE_FILENOTFOUND|Échec de l'ouverture du fichier « %1 » pour écriture. Fichier introuvable.|  
|0xC020909F|-1071607649|DTS_E_TXFILEEXTRACTORSTATIC_FILEOPENERR_FORWRITE_FILENOTFOUND|Échec de l'ouverture du fichier en écriture. Fichier introuvable.|  
|0xC02090A0|-1071607648|DTS_E_FILEOPENERR_FORWRITE_PATHNOTFOUND|Échec de l'ouverture du fichier « %1 » pour écriture. Le chemin d'accès est introuvable.|  
|0xC02090A1|-1071607647|DTS_E_TXFILEEXTRACTORSTATIC_FILEOPENERR_FORWRITE_PATHNOTFOUND|Échec de l'ouverture du fichier en écriture. Le chemin d'accès est introuvable.|  
|0xC02090A2|-1071607646|DTS_E_FILEOPENERR_FORWRITE_TOOMANYOPENFILES|Échec de l'ouverture du fichier « %1 » pour écriture. Trop de fichiers sont ouverts.|  
|0xC02090A3|-1071607645|DTS_E_TXFILEEXTRACTORSTATIC_FILEOPENERR_FORWRITE_TOOMANYOPENFILES|Échec de l'ouverture du fichier en écriture. Trop de fichiers sont ouverts.|  
|0xC02090A4|-1071607644|DTS_E_FILEOPENERR_FORWRITE_ACCESSDENIED|Échec de l'ouverture du fichier « %1 » pour écriture. Vous ne possédez pas les autorisations appropriées.|  
|0xC02090A5|-1071607643|DTS_E_TXFILEEXTRACTORSTATIC_FILEOPENERR_FORWRITE_ACCESSDENIED|Échec de l'ouverture du fichier en écriture. Vous ne possédez pas les autorisations appropriées.|  
|0xC02090A6|-1071607642|DTS_E_FILEOPENERR_FORWRITE_FILEEXISTS|Échec de l'ouverture du fichier « %1 » pour écriture. Ce fichier existe et ne peut être remplacé. Si la propriété AllowAppend a la valeur FALSE et si la propriété ForceTruncate a elle aussi la valeur FALSE, l'existence du fichier entraîne cet échec.|  
|0xC02090A7|-1071607641|DTS_E_TXFILEEXTRACTORSTATIC_FILEOPENERR_FORWRITE_FILEEXISTS|Échec de l'ouverture d'un fichier en écriture. Ce fichier existe déjà et ne peut être remplacé. Si les propriétés AllowAppend et ForceTruncate ont toutes deux la valeur FALSE, l'existence du fichier entraîne cet échec.|  
|0xC02090A8|-1071607640|DTS_E_INCORRECTCUSTOMPROPERTYVALUEFOROBJECT|La valeur de la propriété personnalisée %1 sur %2 est incorrecte.|  
|0xC02090A9|-1071607639|DTS_E_COLUMNSHAVEINCOMPATIBLEMETADATA|Les colonnes « %1 » et « %2 » possèdent des métadonnées incompatibles.|  
|0xC02090AD|-1071607635|DTS_E_FILEWRITEERR_DISKFULL|Échec de l'ouverture du fichier « %1 » en écriture car le disque est saturé. Espace disque insuffisant pour enregistrer ce fichier.|  
|0xC02090AE|-1071607634|DTS_E_TXFILEEXTRACTORSTATIC_FILEWRITEERR_DISKFULL|Échec de la tentative d'ouverture du fichier en écriture car le disque est plein.|  
|0xC02090B9|-1071607623|DTS_E_TXAGG_SORTKEYGENFAILED|Échec de la génération d'une clé de tri avec l'erreur 0x%1!8.8X!. Les ComparisonFlags sont activés, et la génération d'une clé de tri avec LCMapString a échoué.|  
|0xC02090BA|-1071607622|DTS_E_TXCHARMAPLCMAPFAILED|Échec de la transformation pour le mappage de chaîne. Code d'erreur : 0x%1!8.8X!. Échec de LCMapString.|  
|0xC02090BB|-1071607621|DTS_E_FILEOPENERR_FORREAD_FILENOTFOUND|Échec de l'ouverture du fichier « %1 » pour lecture. Ce fichier est introuvable.|  
|0xC02090BC|-1071607620|DTS_E_TXFILEINSERTERSTATIC_FILEOPENERR_FORREAD_FILENOTFOUND|Échec de l'ouverture d'un fichier en lecture. Ce fichier est introuvable.|  
|0xC02090BD|-1071607619|DTS_E_FILEOPENERR_FORREAD_PATHNOTFOUND|Échec de l'ouverture du fichier « %1 » pour lecture. Le chemin d'accès est introuvable.|  
|0xC02090BE|-1071607618|DTS_E_TXFILEINSERTERSTATIC_FILEOPENERR_FORREAD_PATHNOTFOUND|Échec de l'ouverture d'un fichier en lecture. Le chemin est introuvable.|  
|0xC02090BF|-1071607617|DTS_E_FILEOPENERR_FORREAD_TOOMANYOPENFILES|Échec de l'ouverture du fichier « %1 » pour lecture. Trop de fichiers sont ouverts.|  
|0xC02090C0|-1071607616|DTS_E_TXFILEINSERTERSTATIC_FILEOPENERR_FORREAD_TOOMANYOPENFILES|Échec de l'ouverture du fichier en lecture. Trop de fichiers sont ouverts.|  
|0xC02090C1|-1071607615|DTS_E_FILEOPENERR_FORREAD_ACCESSDENIED|Échec de l'ouverture du fichier « %1 » en lecture. Accès refusé.|  
|0xC02090C2|-1071607614|DTS_E_TXFILEINSERTERSTATIC_FILEOPENERR_FORREAD_ACCESSDENIED|Échec de l'ouverture du fichier en lecture. Vous ne possédez pas les autorisations appropriées.|  
|0xC02090C3|-1071607613|DTS_E_INSERTERINVALIDBOM|La valeur d'indicateur d'ordre des octets (BOM) du fichier « %1 » est 0x%2!4.4X!, mais la valeur attendue est 0x%3!4.4X!. La propriété ExpectBOM a été définie pour ce fichier, mais la valeur BOM est manquante ou non valide dans ce fichier.|  
|0xC02090C4|-1071607612|DTS_E_TXFILEINSERTERSTATIC_INSERTERINVALIDBOM|La valeur d'indicateur d'ordre des octets (BOM) du fichier n'est pas valide. La propriété ExpectBOM a été définie pour ce fichier, mais la valeur BOM est manquante ou non valide dans ce fichier.|  
|0xC02090C5|-1071607611|DTS_E_NOCOMPONENTATTACHED|%1 n'est associé à aucun composant.  Un composant doit être associé.|  
|0xC02090C9|-1071607607|DTS_E_TXLOOKUP_INVALIDMAXMEMORYPROP|Valeur de la propriété personnalisée %1 incorrecte.  Il doit s'agir d'un nombre compris entre %2!d! et %3!I64d!.|  
|0xC02090CA|-1071607606|DTS_E_TXAGG_COMPFLAGS_BADAGGREGATIONTYPE|Impossible de spécifier la propriété personnalisée « %1 » pour le type d'agrégation sélectionné pour cette colonne. La propriété personnalisée d'indicateurs de comparaison peut uniquement être spécifiée pour les types d'agrégation GROUP BY et COUNT DISTINCT.|  
|0xC02090CB|-1071607605|DTS_E_TXAGG_COMPFLAGS_BADDATATYPE|La propriété personnalisée des indicateurs de comparaison « %1 » peut uniquement être spécifiée pour les colonnes comportant les types de données DT_STR ou DT_WSTR.|  
|0xC02090CD|-1071607603|DTS_E_TXAGG_AGGREGATION_FAILURE|Échec de l'agrégation sur %1. Code d'erreur : 0x%2!8.8X!.|  
|0xC02090CF|-1071607601|DTS_E_MAPPINGSETUPERROR|Erreur lors du paramétrage du mappage. %1|  
|0xC02090D0|-1071607600|DTS_E_XMLSRCUNABLETOREADXMLDATA|%1 n'a pu lire les données XML.|  
|0xC02090D1|-1071607599|DTS_E_XMLSRCUNABLETOGETXMLDATAVARIABLE|%1 n'a pas pu obtenir la variable spécifiée par la propriété « %2 ».|  
|0xC02090D2|-1071607598|DTS_E_NODATATABLEMATCHROWID|%1 contient un RowsetID avec la valeur %2 qui ne fait pas référence à une table de données dans le schéma.|  
|0xC02090D6|-1071607594|DTS_E_TXAGG_BADKEYSVALUE|La propriété %1 doit être vide, ou comporter un nombre entre %2!u! et %3!u!. La propriété Keys ou CountDistinctKeys possède une valeur incorrecte. Cette propriété doit correspondre à un nombre entre 0 et ULONG_MAX inclus, ou elle ne doit pas être définie.|  
|0xC02090D7|-1071607593|DTS_E_TXAGG_TOOMANYKEYS|Le composant d'agrégation a rencontré un nombre de combinaisons de clés distinctes trop important. Il ne peut pas gérer plus de %1!u! valeurs de clés distinctes. L'espace de travail principal comporte plus de valeurs de clés distinctes que ULONG_MAX.|  
|0xC02090D8|-1071607592|DTS_E_TXAGG_TOOMANYCOUNTDISTINCTVALUES|Le composant d'agrégation a rencontré un nombre de valeurs distinctes trop important lors du calcul de l'agrégation Count Distinct. Il ne peut pas gérer plus de %1!u! valeurs distinctes. Un nombre de valeurs distinctes supérieur à ULONG_MAX a été trouvé lors du calcul de l'agrégation Count Distinct.|  
|0xC02090D9|-1071607591|DTS_E_FAILEDTOWRITETOTHEFILENAMECOLUMN|Échec de la tentative d'écriture dans la colonne de nom de fichier. Code d'erreur : 0x%1!8.8X!.|  
|0xC02090DC|-1071607588|DTS_E_FAILEDTOFINDERRORCOLUMN|Une erreur s'est produite, mais il est impossible de déterminer la colonne à l'origine de cette erreur.|  
|0xC02090E3|-1071607581|DTS_E_TXLOOKUP_FAILEDUPGRADE_BAD_VERSION|Impossible de mettre à niveau les métadonnées de recherche à partir de la version %1!d! vers %2!d!. La transformation de recherche n'a pu mettre à niveau les métadonnées à partir du numéro de version existant dans un appel à PerformUpgrade().|  
|0xC02090E5|-1071607579|DTS_E_TERMEXTRACTIONORLOOKUP_NTEXTSPLITED|Échec de la recherche d'une limite de fin de phrase.|  
|0xC02090E6|-1071607578|DTS_E_TERMEXTRACTION_EXCEED_MAXWORDNUM|La transformation d'extraction de terme est incapable de traiter le texte d'entrée car une phrase du texte d'entrée est trop longue. Cette phrase est segmentée en plusieurs phrases.|  
|0xC02090E7|-1071607577|DTS_E_XMLSRCFAILEDTOCREATEREADER|%1 n'a pu lire les données XML. %2|  
|0xC02090F0|-1071607568|DTS_E_TXLOOKUP_REINITMETADATAFAILED|Échec de l'appel vers la méthode de transformation de recherche ReinitializeMetadata.|  
|0xC02090F1|-1071607567|DTS_E_TXLOOKUP_NOJOINS|La transformation de recherche doit contenir au moins une colonne d'entrée jointe à une colonne de référence, mais aucune colonne n'a été spécifiée. Vous devez spécifier au moins une colonne de jointure.|  
|0xC02090F2|-1071607566|DTS_E_MANAGEDERR_BADFORMATSPECIFICATION|La chaîne de message en cours de publication par l'infrastructure d'erreur gérée contient une spécification de format incorrecte. Il s'agit d'une erreur interne.|  
|0xC02090F3|-1071607565|DTS_E_MANAGEDERR_UNSUPPORTEDTYPE|Lors de la mise en forme d'une chaîne de message à l'aide de l'infrastructure d'erreur gérée, un type Variant ne possédant pas de prise en charge de la mise en forme a été détecté. Il s'agit d'une erreur interne.|  
|0xC02090F5|-1071607563|DTS_E_DATAREADERSRCUNABLETOPROCESSDATA|%1 n'a pu traiter les données. %2|  
|0xC02090F6|-1071607562|DTS_E_XMLSRCEMPTYPROPERTY|La propriété « %1 » sur %2 était vide.|  
|0xC02090F7|-1071607561|DTS_E_XMLSRCINVALIDOUTPUTNAME|Échec de la tentative de création d'une sortie nommée « %1 » pour la table XML avec le chemin d'accès « %2 », car ce nom n'est pas valide.|  
|0xC02090F8|-1071607560|DTS_E_MGDSRC_OVERFLOW|La valeur était trop grande pour tenir dans le %1.|  
|0xC02090F9|-1071607559|DTS_E_DATAREADERDESTUNABLETOPROCESSDATA|%1 n'a pu traiter les données.|  
|0xC02090FA|-1071607558|DTS_E_XMLSRC_INDUCEDTRANSFORMFAILUREONTRUNCATION|Échec de « %1 » en raison d'une troncation. En outre, la disposition de la ligne de troncation sur « %2 » au niveau de « %3 » indique une erreur au niveau de la troncation. Une erreur de troncation s'est produite sur l'objet spécifié du composant spécifié.|  
|0xC02090FB|-1071607557|DTS_E_XMLSRC_INDUCEDTRANSFORMFAILUREONERROR|Échec de l'objet « %1 » en raison du code d'erreur 0x%2!8.8X!. En outre, la disposition de la ligne d'erreur sur « %3 » au niveau de « %4 » spécifie un échec sur l'erreur. Une erreur s'est produite sur l'objet spécifié du composant spécifié.|  
|0xC0209291|-1071607151|DTS_E_SQLCEDESTSTATIC_FAILEDTOSETVALUES|La destination SQLCE n'a pas pu définir les valeurs des colonnes pour la ligne.|  
|0xC0209292|-1071607150|DTS_E_SQLCEDESTSTATIC_FAILEDTOINSERT|La destination SQLCE n'a pu insérer la ligne.|  
|0xC0209293|-1071607149|DTS_E_TXFUZZYLOOKUP_OLEDBERR_LOADCOLUMNMETADATA|Erreur OLEDB rencontrée lors du chargement des métadonnées de la colonne.|  
|0xC0209294|-1071607148|DTS_E_TXFUZZYLOOKUP_TOOFEWREFERENCECOLUMNS|La recherche de métadonnées de référence contient un nombre de colonnes insuffisant.|  
|0xC0209295|-1071607147|DTS_E_TXSCD_OLEDBERR_LOADCOLUMNMETADATA|Erreur OLEDB rencontrée lors du chargement des métadonnées de la colonne.|  
|0xC0209296|-1071607146|DTS_E_TXSCD_TOOFEWREFERENCECOLUMNS|La recherche de métadonnées de référence contient un nombre de colonnes insuffisant.|  
|0xC0209297|-1071607145|DTS_E_TXSCD_MALLOCERR_REFERENCECOLUMNINFO|Impossible d'allouer de la mémoire.|  
|0xC0209298|-1071607144|DTS_E_TXSCD_MALLOCERR_BUFFCOL|Impossible d'allouer de la mémoire.|  
|0xC0209299|-1071607143|DTS_E_TXSCD_MAINWORKSPACE_CREATEERR|Impossible de créer le tampon d'espace de travail.|  
|0xC020929A|-1071607142|DTS_E_DTPXMLDOMCREATEERROR|Impossible d'instancier le document DOM XML. Vérifiez que les binaires MSXML sont correctement installés et inscrits.|  
|0xC020929B|-1071607141|DTS_E_DTPXMLDOMLOADERROR|Impossible de charger des données XML dans un DOM local pour le traitement.|  
|0xC020929C|-1071607140|DTS_E_RSTDESTBADVARIABLETYPE|Le type de la variable d'exécution « %1 » est incorrect. La variable d'exécution doit posséder le type Object.|  
|0xC020929E|-1071607138|DTS_E_XMLDATAREADERMULTIPLEINLINEXMLSCHEMASNOTSUPPORTED|L'adaptateur de source XML n'a pas pu traiter les données XML. Plusieurs schémas inclus ne sont pas pris en charge.|  
|0xC020929F|-1071607137|DTS_E_XMLDATAREADERANYTYPENOTSUPPORTED|L'adaptateur de source XML n'a pas pu traiter les données XML. Impossible de déclarer le contenu d'un élément avec la valeur anyType.|  
|0xC02092A0|-1071607136|DTS_E_XMLDATAREADERGROUPREFNOTSUPPORTED|L'adaptateur de source XML n'a pas pu traiter les données XML. Le contenu d'un élément ne peut renfermer une référence (réf) à un groupe.|  
|0xC02092A1|-1071607135|DTS_E_XMLDATAREADERMIXEDCONTENTFORCOMPLEXTYPESNOTSUPPORTED|L'adaptateur de source XML ne prend pas en charge le modèle de contenu mixte sur des types complexes.|  
|0xC02092A2|-1071607134|DTS_E_XMLDATAREADERINLINESCHEMAFOUNDINSOURCEXML|L'adaptateur de source XML n'a pas pu traiter les données XML. Un schéma inclus doit constituer le premier nœud enfant dans les données XML sources.|  
|0xC02092A3|-1071607133|DTS_E_XMLDATAREADERNOINLINESCHEMAFOUND|L'adaptateur de source XML n'a pas pu traiter les données XML. Aucun schéma inclus n'a été trouvé dans les données XML sources, mais la valeur True a été attribuée à la propriété « UseInlineSchema ».|  
|0xC02092A4|-1071607132|DTS_E_CONNECTIONMANAGERTRANSACTEDANDRETAINEDINBULKINSERT|Ce composant ne peut utiliser un gestionnaire de connexions qui conserve sa connexion dans une transaction avec Fastload ou Bulk Insert.|  
|0xC02092A5|-1071607131|DTS_E_OUTPUTREDIRECTINTRANSACTIONNOTALLOWED|Impossible de définir %1 pour rediriger l'erreur à l'aide d'une connexion dans une transaction.|  
|0xC02092A6|-1071607130|DTS_E_FOUNDORPHANEDEXTERNALMETADATACOLUMN|Le %1 ne possède pas de colonne d'entrée ou de sortie correspondante.|  
|0xC02092A9|-1071607127|DTS_E_RAWDESTNOINPUTCOLUMNS|Il n'y a pas de colonne sélectionnée pour être écrite dans le fichier.|  
|0xC02092AA|-1071607126|DTS_E_RAWDESTBLOBDATATYPE|Le %1 possède un type de données non valide. Les colonnes possédant des types de données DT_IMAGE, DT_TEXT et DT_NTEXT ne peuvent pas être écrites dans des fichiers bruts.|  
|0xC02092AB|-1071607125|DTS_E_RAWDESTWRONGEXTERNALMETADATAUSAGE|La collection de métadonnées externes est utilisée de manière incorrecte par ce composant. Celui-ci doit utiliser des métadonnées externes lors de l'ajout à un fichier existant ou de la troncation de ce fichier. Sinon, les métadonnées externes ne sont pas nécessaires.|  
|0xC02092AC|-1071607124|DTS_E_RAWDESTMAPPEDINPUTCOLUMN|La colonne %1 est mappée sur une colonne de métadonnées externes avec l'ID %2!d!. Les colonnes d'entrée ne doivent pas être mappées sur des colonnes de métadonnées externes lorsque la valeur de l'option d'écriture sélectionnée est Toujours créer.|  
|0xC02092AD|-1071607123|DTS_E_RAWFILECANTOPENFORMETADATA|Impossible d'ouvrir le fichier pour la lecture des métadonnées. Si le fichier n'existe pas et que le composant a déjà défini des métadonnées externes, vous pouvez définir la propriété « ValidateExternalMetadata » à la valeur « False » et le fichier sera créé au moment de l'exécution.|  
|0xC02092AE|-1071607122|DTS_E_FAILEDTOACCESSLOBCOLUMN|Échec de l'accès aux données LOB à partir du tampon de flux de données pour la colonne de source de données « %1 » avec le code d'erreur 0x%2!8.8X!.|  
|0xC02092AF|-1071607121|DTS_E_XMLSRCUNABLETOPROCESSXMLDATA|%1 n'a pu traiter les données XML. %2|  
|0xC02092B0|-1071607120|DTS_E_XMLSRCSTATIC_UNABLETOPROCESSXMLDATA|L'adaptateur de source XML n'a pas pu traiter les données XML.|  
|0xC02092B1|-1071607119|DTS_E_RAWINVALIDACCESSMODE|La valeur %1!d! n'est pas reconnue comme un mode d'accès valide.|  
|0xC02092B2|-1071607118|DTS_E_INCOMPLETEDATASOURCECOLUMNFOUND|Des informations de métadonnées complètes pour la colonne source des données « %1 » ne sont pas disponibles.  Vérifiez que la colonne est définie correctement dans la source de données.|  
|0xC02092B3|-1071607117|DTS_E_TXAUDIT_ONLYSTRINGLENGTHCHANGEALLOWED|Seules les longueurs des colonnes Nom d'utilisateur, Nom du package, Nom de la tâche et Nom de l'ordinateur peuvent être modifiées.  Toutes les autres informations datatype de colonne d'audit sont en lecture seule.|  
|0xC02092B4|-1071607116|DTS_E_ROWSETUNAVAILABLE|Un ensemble de lignes basé sur la commande SQL n'a pas été retourné par le fournisseur OLE DB.|  
|0xC02092B5|-1071607115|DTS_E_COMMITFAILED|Échec d'une validation.|  
|0xC02092B6|-1071607114|DTS_E_USEBINARYFORMATREQUIRESANSIFILE|La propriété personnalisée « %1 » de %2 ne peut être utilisée qu'avec des fichiers ANSI.|  
|0xC02092B7|-1071607113|DTS_E_USEBINARYFORMATREQUIRESBYTES|La propriété personnalisée « %1 » de %2 ne peut être utilisée qu'avec DT_BYTES.|  
|0xC0209302|-1071607038|DTS_E_OLEDB_NOPROVIDER_ERROR|Code d'erreur SSIS DTS_E_OLEDB_NOPROVIDER_ERROR.  Le fournisseur OLE DB demandé %2 n'est pas inscrit. Code d'erreur : 0x%1!8.8X!.|  
|0xC0209303|-1071607037|DTS_E_OLEDB_NOPROVIDER_64BIT_ERROR|Code d'erreur SSIS DTS_E_OLEDB_NOPROVIDER_64BIT_ERROR.  Le fournisseur OLE DB demandé %2 n'est pas inscrit ; il est possible qu'aucun fournisseur 64 bits ne soit disponible.  Code d'erreur : 0x%1!8.8X!.|  
|0xC0209306|-1071607034|DTS_E_MULTICACHECOLMAPPINGS|La colonne de cache, « %1 », est mappée à plusieurs colonnes. Supprimez les mappages de colonne dupliqués.|  
|0xC0209307|-1071607033|DTS_E_COLNOTMAPPEDTOCACHECOL|Le %1 n'est pas mappé à une colonne de cache valide.|  
|0xC0209308|-1071607032|DTS_E_CACHECOLDATATYPEINCOMPAT|Impossible de mapper la colonne d'entrée « %1 » et la colonne de cache « %2 », car les types de données ne correspondent pas.|  
|0xC0209309|-1071607031|DTS_E_INCORRECTINPUTCACHECOLCOUNT|Le nombre de colonnes d'entrée ne correspond pas au nombre de colonnes du cache.|  
|0xC020930A|-1071607030|DTS_E_INVALIDCACHEFILENAME|Le nom du fichier cache n'est pas fourni ou n'est pas valide. Fournissez un nom de fichier cache valide.|  
|0xC020930B|-1071607029|DTS_E_CACHECOLINDEXPOSMISMATCH|La position d'index de la colonne, « %1 », est différente de la position d'index de la colonne du gestionnaire de connexions du cache, « %2 ».|  
|0xC020930C|-1071607028|DTS_E_FAILEDTOLOADCACHE|Impossible de charger le cache à partir du fichier, « %1 ».|  
|0xC020930D|-1071607027|DTS_E_TXLOOKUP_REFCOLUMNISNOTINDEX|La colonne d'entrée de recherche %1 fait référence à une colonne de cache autre qu'une colonne d'index %2.|  
|0xC020930E|-1071607026|DTS_E_FAILEDTOGETCONNECTIONSTRING|Impossible d'obtenir la chaîne de connexion.|  
|0xC020930F|-1071607025|DTS_E_CACHECOLDATATYPEPROPINCOMPAT|Impossible de mapper la colonne d'entrée « %1 » et la colonne de cache « %2 », car une ou plusieurs propriétés de type de données ne correspondent pas.|  
|0xC0209311|-1071607023|DTS_E_CACHECOLUMNOTFOUND|Impossible de trouver la colonne de cache « %1 » dans le cache.|  
|0xC0209312|-1071607022|DTS_E_CACHECOLUMNMAPPINGFAILED|Impossible de mapper %1 à une colonne de cache. Le hresult est 0x%2!8.8X!.|  
|0xC0209313|-1071607021|DTS_E_CACHELOADEDFROMFILE|Le %1 ne peut pas écrire dans le cache parce que le cache a été chargé à partir d'un fichier par %2.|  
|0xC0209314|-1071607020|DTS_E_CACHERELOADEDDIFFERENTFILES|Le %1 ne peut pas charger le cache à partir du fichier « %2 » parce que le cache a déjà été chargé à partir du fichier « %3 ».|  
|0xC0209316|-1071607018|DTS_E_OUTPUTNOTUSED|La sortie portant l'ID %1!d! du composant d'agrégation n'est utilisée par aucun composant. Supprimez-la ou associez-la à une entrée d'un composant.|  
|0xC0209317|-1071607017|DTS_E_CACHEFILEWRITEFAILED|%1 n'a pas pu écrire le cache dans le fichier « %2 ». Le hresult est 0x%3!8.8X!.|  
|0xC0209318|-1071607016|DTS_E_XMLDATATYPECHANGED|Les informations de type de données de schéma XML de « %1 » sur l'élément « %2 » ont été modifiées.  Réinitialisez les métadonnées de ce composant et passez en revue le mappage de colonnes.|  
|0xC0209319|-1071607015|DTS_E_TXLOOKUP_UNUSEDINPUTCOLUMN|%1 n'est pas utilisée dans la jointure ou la copie. Supprimez la colonne inutile de la liste des colonnes d'entrée.|  
|0xC020931A|-1071607014|DTS_E_SORTSTACKOVERFLOW|Le tri a échoué en raison d'un dépassement de capacité de la pile en triant une mémoire tampon entrante.  Veuillez réduire la propriété DefaultBufferMaxRows sur la tâche de flux de données.|  
|0xC020F42A|-1071582166|DTS_E_OLEDB_OLDPROVIDER_ERROR|Envisagez de changer le FOURNISSEUR dans la chaîne de connexion à %1, ou accédez à http://www.microsoft.com/downloads pour rechercher et installer la prise en charge de %2.|  
|||DTS_E_INITTASKOBJECTFAILED|Échec de l’initialisation de l’objet de tâche pour la tâche « %1!s! », type « %2!s! » en raison de l’erreur « 0x%3!8.8X! « %4!s! ».|  
|||DTS_E_GETCATMANAGERFAILED|Impossible de créer un gestionnaire de catégories de composants COM en raison de l’erreur 0x%1!8.8X! « %2!s! ».|  
|||DTS_E_COMPONENTINITFAILED|Échec de l'initialisation du composant %1!s! en raison de l’erreur 0x%2!8.8X! « %3!s! ».|  
  
##  <a name="msgWarning"></a> Messages d'avertissement  
 Les noms symboliques des messages d’avertissement de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] commencent par **DTS_W_**.  
  
|Code hexadécimal|Code décimal|Nom symbolique|Description|  
|----------------------|------------------|-------------------|-----------------|  
|0x80000036|-2147483594|DTS_W_COUNTDOWN|Il reste %1!lu! jours d'évaluation. Une fois expiré, le package ne pourra plus être exécuté.|  
|0x80010015|-2147418091|DTS_W_GENERICWARNING|Avertissement(s) soulevés. Des avertissements plus spécifiques qui précèdent celui-ci devraient s'afficher pour expliquer les détails des avertissements.|  
|0x80012010|-2147409904|DTS_W_FAILEDXMLDOCCREATION|Impossible de créer une instance d'objet de document XML. Vérifiez que MSXML est correctement installé et enregistré.|  
|0x80012011|-2147409903|DTS_W_FAILEDCONFIGLOAD|Impossible de charger le fichier de configuration XML. Ce dernier est peut-être mal formé ou non valide.|  
|0x80012012|-2147409902|DTS_W_CONFIGFILENAMEINVALID|Le nom de fichier de configuration « %1 » n'est pas valide. Vérifiez le nom du fichier de configuration.|  
|0x80012013|-2147409901|DTS_W_CONFIGFILEINVALID|Le fichier de configuration est chargé, mais non valide. Son format est incorrect, un élément est absent ou il est endommagé.|  
|0x80012014|-2147409900|DTS_W_CONFIGFILENOTFOUND|Le fichier de configuration « %1 » est introuvable. Vérifiez le répertoire et le nom du fichier.|  
|0x80012015|-2147409899|DTS_W_CONFIGKEYNOTFOUND|La clé du Registre de configuration « %1 » est introuvable. Une entrée de configuration spécifie une clé de Registre qui n'est pas disponible. Consultez le Registre pour vous assurer que la clé y figure.|  
|0x80012016|-2147409898|DTS_W_CONFIGTYPEINVALID|Le type de configuration dans l'une des entrées de configuration n'était pas valide. Les types valides sont répertoriés dans la liste DTSConfigurationType.|  
|0x80012017|-2147409897|DTS_W_CANNOTFINDOBJECT|Le chemin d'accès au package a référencé un objet qui est introuvable : « %1 ». Ceci se produit lorsqu'une tentative est effectuée pour résoudre un chemin de package vers un objet qui est introuvable.|  
|0x80012018|-2147409896|DTS_W_CONFIGFORMATINVALID_PACKAGEDELIMITER|Le format de l'entrée de configuration « %1 » est incorrect, car il ne commence pas par le séparateur de packages. Ajoutez « \package » devant le chemin d'accès au package.|  
|0x80012019|-2147409895|DTS_W_CONFIGFORMATINVALID|Le format de l'entrée de configuration « %1 » est incorrect. Ceci peut se produire en raison d'un séparateur absent ou d'erreurs de format, comme un séparateur de tableaux non valide.|  
|0x8001201A|-2147409894|DTS_W_NOPARENTVARIABLES|La configuration à partir d'une variable parente « %1 » ne s'est pas produite, car il n'y avait pas de collection de variables parentes.|  
|0x8001201B|-2147409893|DTS_W_CONFIGFILEFAILEDIMPORT|Échec de l'importation du fichier de configuration : « %1 ».|  
|0x8001201C|-2147409892|DTS_W_PARENTVARIABLENOTFOUND|La configuration à partir d'une variable parente « %1 » ne s'est pas produite, car il n'y avait pas de variable parente. Code d'erreur : 0x%2!8.8X!.|  
|0x8001201D|-2147409891|DTS_W_CONFIGFILEEMPTY|Le fichier de configuration était vide et ne contenait aucune entrée de configuration.|  
|0x80012023|-2147409885|DTS_W_INVALIDCONFIGURATIONTYPE|Le type de configuration de la configuration « %1 » n'est pas valide. Ceci peut se produire lorsqu'une tentative est effectuée pour définir la propriété du type d'un objet de configuration sur un type de configuration non valide.|  
|0x80012025|-2147409883|DTS_W_REGISTRYCONFIGURATIONTYPENOTFOUND|Le type de configuration de la configuration du Registre est introuvable dans la clé « %1 ». Ajoutez une valeur appelée ConfigType à la clé de Registre et donnez-lui une valeur de chaîne de « Variable », « Property », « ConnectionManager », « LoggingProvider » ou « ForEachEnumerator ».|  
|0x80012026|-2147409882|DTS_W_REGISTRYCONFIGURATIONVALUENOTFOUND|La valeur de configuration de la configuration du Registre est introuvable dans la clé « %1 ». Ajoutez une valeur appelée Value à la clé de Registre de type DWORD ou String.|  
|0x80012028|-2147409880|DTS_W_PROCESSCONFIGURATIONFAILEDSET|La configuration du processus n'a pas réussi à définir la destination dans le chemin d'accès du package de « %1 ». Ceci se produit lorsque la tentative de définition d'une propriété ou d'une variable de destination échoue. Vérifiez la propriété ou la variable de destination.|  
|0x80012032|-2147409870|DTS_W_CONFIGUREDVALUESECTIONEMPTY|Impossible d'extraire la valeur du fichier .ini. La section ConfiguredValue est vide ou n'existe pas : « %1 ».|  
|0x80012033|-2147409869|DTS_W_CONFIGUREDTYPESECTIONEMPTY|Impossible d'extraire la valeur du fichier .ini. La section ConfiguredType est vide ou n'existe pas : « %1 ».|  
|0x80012034|-2147409868|DTS_W_PACKAGEPATHSECTIONEMPTY|Impossible d'extraire la valeur du fichier .ini. La section PackagePath est vide ou n'existe pas : « %1 ».|  
|0x80012035|-2147409867|DTS_W_CONFIGUREDVALUETYPE|Impossible d'extraire la valeur du fichier .ini. La section ConfiguredValueType est vide ou n'existe pas : « %1 ».|  
|0x80012051|-2147409839|DTS_W_SQLSERVERFAILEDIMPORT|L'importation de la configuration de SQL Server a échoué : « %1 ».|  
|0x80012052|-2147409838|DTS_W_INICONFIGURATIONPROBLEM|Le fichier de configuration .ini n'est pas valide en raison de champs absents ou vides.|  
|0x80012054|-2147409836|DTS_W_NORECORDSFOUNDINTABLE|La table « %1 » ne comporte pas d'enregistrement de configuration. Ceci se produit lors de la configuration à partir d'une table SQL Server qui ne comporte pas d'enregistrement de configuration.|  
|0x80012055|-2147409835|DTS_W_DUPLICATECUSTOMEVENT|Erreur lors de l'utilisation d'un même nom pour différents événements personnalisés. L'événement personnalisé « %1 » a été défini différemment par différents enfants de ce conteneur. Une erreur peut apparaître lors de l'exécution du gestionnaire d'événements.|  
|0x80012057|-2147409833|DTS_W_CONFIGREADONLYVARIABLE|La configuration a tenté de modifier une variable en lecture seule. La variable est dans le chemin d'accès au package « %1 ».|  
|0x80012058|-2147409832|DTS_W_CONFIGPROCESSCONFIGURATIONFAILED|L'appel de la propriété ProcessConfiguration sur le package a échoué. La configuration a tenté de modifier la propriété dans le chemin d'accès au package « %1 ».|  
|0x80012059|-2147409831|DTS_W_ONEORMORECONFIGLOADFAILED|Impossible de charger au moins une des entrées de configuration du package. Recherchez dans les entrées de configuration pour « %1 » et les précédents avertissements les descriptions de la configuration qui a échoué.|  
|0x8001205A|-2147409830|DTS_W_CONFIGNODEINVALID|L'entrée de configuration « %1 » du fichier de configuration n'est pas valide ou n'a pas réussi à configurer la variable.  Le nom indique l'entrée qui a échoué. Dans certains cas, le nom n'est pas disponible.|  
|0x80014058|-2147401640|DTS_W_FAILURENOTRESTARTABLE|Cette tâche ou ce conteneur a échoué, mais puisque la propriété FailPackageOnFailure a la valeur FALSE, le package va continuer. Cet avertissement est publié lorsque la propriété SaveCheckpoints du package a la valeur TRUE et que la tâche ou le conteneur échoue.|  
|0x80017101|-2147389183|DTS_W_EMPTYPATH|Le chemin d'accès est vide.|  
|0x80019002|-2147381246|DTS_W_MAXIMUMERRORCOUNTREACHED|Code d'avertissement SSIS DTS_W_MAXIMUMERRORCOUNTREACHED.  La méthode Execution a réussi, mais le nombre d'erreurs détectées (%1!d!) a atteint le maximum autorisé (%2!d!) ; aboutissant à un échec. Ceci se produit lorsque le nombre d'erreurs atteint le nombre spécifié dans MaximumErrorCount. Modifiez la valeur de MaximumErrorCount ou résolvez les erreurs.|  
|0x80019003|-2147381245|DTS_W_CONFIGENVVARNOTFOUND|La variable d'environnement de configuration est introuvable.  La variable d'environnement est : « %1 ». Ceci se produit lorsqu'un package spécifie une variable d'environnement pour un paramètre de configuration, mais qu'elle est introuvable. Vérifiez dans la collection de configurations du package si la variable d'environnement spécifiée est disponible est valide.|  
|0x80019316|-2147380458|DTS_W_CONNECTIONPROVIDERCHANGE|Le nom de fournisseur « %2 » du gestionnaire de connexions « %1 » a été remplacé par « %3 ».|  
|0x80019317|-2147380457|DTS_W_READEXTMAPFAILED|Une exception s'est produite lors de la lecture des fichiers de mappage de mise à niveau. L'exception est « %1 ».|  
|0x80019318|-2147380456|DTS_W_DUPLICATEMAPPINGKEY|Le fichier « %1 » contient un mappage en double. La balise est « %2 » et la clé est « %3 ».|  
|0x80019319|-2147380455|DTS_W_IMPLICITUPGRADEMAPPING|L'extension « %1 » a été implicitement mise à niveau vers « %2 ». Ajoutez un mappage pour cette extension au répertoire UpgradeMappings.|  
|0x8001931A|-2147380454|DTS_W_INVALIDEXTENSIONMAPPING|Le fichier « %1 » contient un mappage non valide. Les valeurs ne peuvent pas être Null ou vides. La balise est « %2 », la clé est « %3 » et la valeur est « %4 ».|  
|0x8001931C|-2147380452|DTS_W_ADOCONNECTIONDATATYPECOMPATCHANGE|La propriété DataTypeCompatibility du gestionnaire de connexions de type ADO « %1 » a été définie sur 80 pour des raisons de compatibilité descendante.|  
|0x8001C004|-2147368956|DTS_W_FILEENUMEMPTY|L'énumérateur For Each File est vide. Il n'a trouvé aucun fichier qui correspondait au modèle de fichier, ou le répertoire spécifié est vide.|  
|0x8001F02F|-2147356625|DTS_W_COULDNOTRESOLVEPACKAGEPATH|Impossible de résoudre un chemin de package vers un objet dans le package « %1 ». Vérifiez que le chemin d'accès du package est valide.|  
|0x8001F203|-2147356157|DTS_W_ITERATIONEXPRESSIONISNOTASSIGNMENT|L'expression d'itération n'est pas une expression d'assignation : « %1 ». Cette erreur se produit généralement lorsque l'expression de l'expression d'assignation sur le ForLoop n'est pas une expression d'assignation.|  
|0x8001F204|-2147356156|DTS_W_INITIALIZATIONEXPRESSIONISNOTASSIGNMENT|L'expression d'initialisation n'est pas une expression d'assignation : « %1 ». Cette erreur se produit généralement lorsque l'expression des expressions d'itération sur le ForLoop n'est pas une expression d'assignation.|  
|0x8001F205|-2147356155|DTS_W_LOGPROVIDERNOTDEFINED|L'exécutable « %1 » a été correctement collé. Toutefois, un module fournisseur d'informations auparavant associé à l'exécutable est introuvable dans la collection « LogProviders ».  L'exécutable a été collé sans informations du module fournisseur d'informations.|  
|0x8001F300|-2147355904|DTS_W_PACKAGEUPGRADED|La mise à niveau du package a réussi.|  
|0x8001F42B|-2147355605|DTS_W_LEGACYPROGID|Le ProgID « %1 » est déconseillé. Le nouveau ProgID pour ce composant « %2 » doit être utilisé à la place.|  
|0x80020918|-2147350248|DTS_W_FTPTASK_OPERATIONFAILURE|Échec de l'opération « %1 ».|  
|0x800283A5|-2147318875|DTS_W_MSMQTASK_USE_WEAK_ENCRYPTION|L'algorithme de chiffrement « %1 » utilise un chiffrement faible.|  
|0x80029164|-2147315356|DTS_W_FSTASK_OPERATIONFAILURE|La tâche n'a pas réussi à exécuter l'opération « %1 ».|  
|0x80029185|-2147315323|DTS_W_EXECPROCTASK_FILENOTINPATH|Le fichier/processus « %1 » n'est pas dans le chemin d'accès.|  
|0x800291C6|-2147315258|DTS_W_SENDMAILTASK_SUBJECT_MISSING|Le sujet est vide.|  
|0x800291C7|-2147315257|DTS_W_SENDMAILTASK_ERROR_IN_TO_LINE|L'adresse dans la ligne « À » est mal formée. Le symbole « @ » est absent ou elle n'est pas valide.|  
|0x800291C8|-2147315256|DTS_W_SENDMAILTASK_AT_MISSING_IN_FROM|L'adresse dans la ligne « De » est mal formée. Le symbole « @ » est absent ou elle n'est pas valide.|  
|0x8002927A|-2147315078|DTS_W_XMLTASK_DIFFFAILURE|Les deux documents XML sont différents.|  
|0x8002928C|-2147315060|DTS_W_XMLTASK_DTDVALIDATIONWARNING|La validation DTD utilisera le fichier DTD défini dans la ligne DOCTYPE du document XML. Elle n'utilisera pas ce qui est attribué à la propriété « %1 ».|  
|0x8002928D|-2147315059|DTS_W_XMLTASK_VALIDATIONFAILURE|La tâche n'a pas réussi à valider « %1 ».|  
|0x80029291|-2147315055|DTS_W_TRANSFERDBTASK_ACTIONSETTOCOPY|La valeur de l'action de transfert n'était pas valide.  Elle va être définie à une copie.|  
|0x80029292|-2147315054|DTS_W_TRANSFERDBTASK_METHODSETTOONLINE|La valeur de la méthode de transfert n'était pas valide.  Elle va être définie à un transfert en ligne.|  
|0x8002F304|-2147290364|DTS_W_PROBLEMOCCURREDWITHFOLLOWINGMESSAGE|Un problème est survenu avec les messages suivants : « %1 ».|  
|0x8002F322|-2147290334|DTS_W_ERRMSGTASK_ERRORMESSAGEALREADYEXISTS|Le message d'erreur « %1 » existe déjà sur le serveur de destination.|  
|0x8002F331|-2147290319|DTS_W_JOBSTASK_JOBEXISTSATDEST|Le travail « %1 » existe déjà sur le serveur de destination.|  
|0x8002F332|-2147290318|DTS_W_JOBSTASK_SKIPPINGJOBEXISTSATDEST|Le transfert du travail « %1 » est ignoré car ce travail existe déjà à la destination.|  
|0x8002F333|-2147290317|DTS_W_JOBSTASK_OVERWRITINGJOB|Remplacement du travail « %1 » sur le serveur de destination.|  
|0x8002F339|-2147290311|DTS_W_LOGINSTASK_ENUMVALUEINCORRECT|La valeur d'énumération permanente de la propriété « FailIfExists » a été modifiée et a été rendue non valide. Réinitialisation à la valeur par défaut.|  
|0x8002F343|-2147290301|DTS_W_LOGINSTASK_OVERWRITINGLOGINATDEST|Remplacement du nom d'accès « %1 » à la destination.|  
|0x8002F356|-2147290282|DTS_W_TRANSOBJECTSTASK_SPALREADYATDEST|La procédure stockée « %1 » existe déjà à la destination.|  
|0x8002F360|-2147290272|DTS_W_TRANSOBJECTSTASK_RULEALREADYATDEST|La règle « %1 » existe déjà à la destination.|  
|0x8002F364|-2147290268|DTS_W_TRANSOBJECTSTASK_TABLEALREADYATDEST|La table « %1 » existe déjà à la destination.|  
|0x8002F368|-2147290264|DTS_W_TRANSOBJECTSTASK_VIEWALREADYATDEST|La vue « %1 » existe déjà à la destination.|  
|0x8002F372|-2147290254|DTS_W_TRANSOBJECTSTASK_UDFALREADYATDEST|La fonction définie par l'utilisateur « %1 » existe déjà à la destination.|  
|0x8002F376|-2147290250|DTS_W_TRANSOBJECTSTASK_DEFAULTALREADYATDEST|La valeur par défaut « %1 » existe déjà à la destination.|  
|0x8002F380|-2147290240|DTS_W_TRANSOBJECTSTASK_UDDTALREADYATDEST|Le type de données défini par l'utilisateur « %1 » existe déjà à la destination.|  
|0x8002F384|-2147290236|DTS_W_TRANSOBJECTSTASK_PFALREADYATDEST|La fonction de partition « %1 » existe déjà à la destination.|  
|0x8002F388|-2147290232|DTS_W_TRANSOBJECTSTASK_PSALREADYATDEST|Le schéma de partition « %1 » existe déjà à la destination.|  
|0x8002F391|-2147290223|DTS_W_TRANSOBJECTSTASK_SCHEMAALREADYATDEST|Le schéma « %1 » existe déjà à la destination.|  
|0x8002F396|-2147290218|DTS_W_TRANSOBJECTSTASK_SQLASSEMBLYALREADYATDEST|SqlAssembly « %1 » existe déjà à la destination.|  
|0x8002F400|-2147290112|DTS_W_TRANSOBJECTSTASK_AGGREGATEALREADYATDEST|La fonction d'agrégation définie par l'utilisateur « %1 » existe déjà à la destination.|  
|0x8002F404|-2147290108|DTS_W_TRANSOBJECTSTASK_TYPEALREADYATDEST|Le type défini par l'utilisateur « %1 » existe déjà à la destination.|  
|0x8002F408|-2147290104|DTS_W_TRANSOBJECTSTASK_XMLSCHEMACOLLECTIONALREADYATDEST|XmlSchemaCollection « %1 » existe déjà à la destination.|  
|0x8002F412|-2147290094|DTS_W_TRANSOBJECTSTASK_NOELEMENTSPECIFIEDTOTRANSFER|Aucun élément spécifié à transférer.|  
|0x8002F415|-2147290091|DTS_W_TRANSOBJECTSTASK_LOGINALREADYATDEST|Le nom d'accès « %1 » existe déjà à la destination.|  
|0x8002F41A|-2147290086|DTS_W_TRANSOBJECTSTASK_USERALREADYATDEST|L'utilisateur « %1 » existe déjà à la destination.|  
|0x80047007|-2147192825|DTS_W_NOLINEAGEVALIDATION|Les ID de lignage des colonnes d'entrée ne peuvent pas être validées, car les arborescences d'exécution contiennent des cycles.|  
|0x80047034|-2147192780|DTS_W_EMPTYDATAFLOW|La tâche DataFlow n'a pas de composant. Ajoutez des composants ou supprimez la tâche.|  
|0x80047069|-2147192727|DTS_W_SORTEDOUTPUTHASNOSORTKEYPOSITIONS|La propriété IsSorted de %1 a la valeur TRUE, mais les propriétés SortKeyPositions de toutes ses colonnes de sortie sont définies à zéro.|  
|0x8004706F|-2147192721|DTS_W_SOURCEREMOVED|La source « %1 » (%2!d!) ne sera pas lue, car ses données ne seront jamais visibles en dehors de la tâche de flux de données.|  
|0x80047076|-2147192714|DTS_W_UNUSEDOUTPUTDATA|La colonne de sortie « %1 » (%2!d!) à la sortie « %3 » (%4!d!) et dans le composant « %5 » (%6!d!) n'est pas utilisée par la suite dans la tâche de flux de données. La suppression de cette colonne de sortie inutile peut augmenter les performances de la tâche Data Flow.|  
|0x800470AE|-2147192658|DTS_W_COMPONENTREMOVED|Le composant « %1 » (%2!d!) a été supprimé de la tâche de flux de données, car sa sortie n'est pas utilisée et ses entrées n'ont pas d'effets secondaires ou ne sont pas connectées aux sorties d'autres composants. Si le composant est nécessaire, la propriété HasSideEffects doit être définie sur la valeur True sur au moins une de ses entrées, ou sa sortie doit être connectée à quelque chose.|  
|0x800470B0|-2147192656|DTS_W_NOWORKTODO|Des lignes ont été attribuées à un thread, mais ce thread n'a pas de travail à effectuer. La disposition comporte une sortie déconnectée. L'exécution du pipeline avec la propriété RunInOptimizedMode définie sur la valeur TRUE est plus rapide et évite cet avertissement.|  
|0x800470C8|-2147192632|DTS_W_EXTERNALMETADATACOLUMNSOUTOFSYNC|Les colonnes externes de %1 ne sont pas synchronisées avec les colonnes de source de données. %2|  
|0x800470C9|-2147192631|DTS_W_EXTERNALMETADATACOLUMNCOLLECTIONNEEDSADDITION|La colonne « %1 » doit être ajoutée aux colonnes externes.|  
|0x800470CA|-2147192630|DTS_W_EXTERNALMETADATACOLUMNCOLLECTIONNEEDSUPDATE|La colonne externe « %1 » doit être mise à jour.|  
|0x800470CB|-2147192629|DTS_W_EXTERNALMETADATACOLUMNCOLLECTIONNEEDSREMOVAL|La colonne %1 doit être supprimée des colonnes externes.|  
|0x800470D8|-2147192616|DTS_W_EXPREVALPOTENTIALSTRINGTRUNCATION|La chaîne de résultats de l'expression « %1 » peut être tronquée si elle dépasse la longueur maximale de %2!d! caractères. L'expression pourrait avoir une valeur de résultat qui dépasse la taille maximale d'un DT_WSTR.|  
|0x800470E9|-2147192599|DTS_W_COMPONENTLEAKPROCESSINPUT|Un appel à la méthode ProcessInput pour l'entrée %1!d! sur %2 a conservé de manière inattendue une référence vers la mémoire tampon dans laquelle elle a été passée. La valeur du compteur refcount sur cette mémoire tampon était %3!d! avant l'appel, et %4!d! une fois l'appel retourné.|  
|0x800470EB|-2147192597|DTS_W_EXPREVALUNREFERENCEDINPUTCOLUMN|La colonne « %1 » sur « %2 » possède le type d'utilisation READONLY, mais n'est pas référencée par une expression. Supprimez la colonne de la liste des colonnes d'entrée disponibles ou référencez-la dans une expression.|  
|0x8004801E|-2147188706|DTS_W_COULDNOTFINDCURRENTVERSION|La valeur « %1 » du composant %2 est introuvable. Impossible de trouver la valeur CurrentVersion du composant. Cette erreur se produit si le composant n'a pas défini ses informations de Registre pour contenir une valeur CurrentVersion dans la section DTSInfo. Ce message apparaît au cours du développement du composant ou lorsque le composant est utilisé dans un package, si le composant n'est pas correctement enregistré.|  
|0x80049300|-2147183872|DTS_W_BUFFERGETTEMPFILENAME|Le gestionnaire de tampons n'a pas pu obtenir un nom de fichier temporaire.|  
|0x80049301|-2147183871|DTS_W_UNUSABLETEMPORARYPATH|Le gestionnaire de tampons n'a pas pu créer un fichier temporaire dans le chemin d'accès « %1 ». Ce chemin ne sera plus utilisé pour le stockage temporaire.|  
|0x80049304|-2147183868|DTS_W_DF_PERFCOUNTERS_DISABLED|Avertissement : impossible d'ouvrir la mémoire partagée globale pour communiquer avec la DLL de performance ; les compteurs de performance du flux de données ne sont pas disponibles  Pour résoudre le problème, exécutez ce package en tant qu'administrateur ou sur la console du système.|  
|0x8020200F|-2145378289|DTS_W_PARTIALROWFOUNDATENDOFFILE|Une ligne partielle a été trouvée à la fin du fichier.|  
|0x8020202B|-2145378261|DTS_W_ENDOFFILEREACHWHILEREADINGHEADERROWS|La fin du fichier de données a été atteinte au cours de la lecture des lignes d'en-tête. Assurez-vous que le séparateur de lignes d'en-tête et que le nombre de lignes d'en-tête à ignorer sont corrects.|  
|0x80202066|-2145378202|DTS_W_CANTRETRIEVECODEPAGEFROMOLEDBPROVIDER|Impossible de récupérer les infos de la page de codes de la colonne à partir du fournisseur OLE DB.  Si le composant prend en charge la propriété « %1 », la page de codes de cette propriété sera utilisée.  Modifiez la valeur de la propriété si les valeurs de la page de codes de la chaîne actuelle sont incorrectes.  Si le composant ne prend pas en charge la propriété, la page de codes provenant de l'ID des paramètres régionaux du composant sera utilisée.|  
|0x802020F7|-2145378057|DTS_W_TXSORTSORTISTHESAME|Les données étant déjà triées de la manière indiquée, la transformation peut être supprimée.|  
|0x8020400D|-2145370099|DTS_W_NOPIPELINEDATATYPEMAPPINGAVAILABLE|Le %1 fait référence à un type de données externe qui ne peut pas être mappé à un type de données de la tâche de flux de données. Le type de données de tâche Data Flow DT_WSTR sera utilisé à la place.|  
|0x802070CC|-2145357620|DTS_W_STATICTRUNCATIONINEXPRESSION|L'expression « %1 » aura toujours pour résultat une troncation des données. Elle comporte une troncation statique (la troncation d'une valeur fixe).|  
|0x8020820C|-2145353204|DTS_W_UNMAPPEDINPUTCOLUMN|La colonne d'entrée « %1 » portant l'ID %2!d! à l'index %3!d! n'est pas mappée. L'ID de lignage de la colonne est zéro.|  
|0x80208305|-2145352955|DTS_W_TXFUZZYLOOKUP_DELIMITERS_DONT_MATCH|Les séparateurs spécifiés ne correspondent pas à ceux qui sont utilisés pour créer l'index de correspondance préexistant « %1 ». Cette erreur se produit lorsque des séparateurs utilisés pour marquer des champs ne correspondent pas. Ceci peut avoir des effets inconnus sur le comportement ou le résultat qui correspond.|  
|0x80208308|-2145352952|DTS_W_TXFUZZYLOOKUP_MAXRESULTS_IS_ZERO|La propriété MaxOutputMatchesPerInput sur la transformation Fuzzy Lookup a la valeur zéro. Il n'en sortira aucun résultat.|  
|0x80208310|-2145352944|DTS_W_TXFUZZYLOOKUP_NO_FUZZY_JOIN_COLUMNS|Aucune colonne d'entrée valide ne comporte la propriété de colonne JoinType définie à Fuzzy.  Pour améliorer les performances sur les jointures de type Exact, utilisez la transformation de recherche au lieu de FuzzyLookup.|  
|0x8020831C|-2145352932|DTS_W_TXFUZZYLOOKUP_TIMESTAMPCAVEAT|La colonne de référence « %1 » est peut-être une colonne horodateur SQL. Lorsque l'index de correspondances approximatives est créé et qu'une copie de la table de référence est effectuée, tous les horodateurs de la table de référence reflètent l'état actuel de la table au moment de la copie. Un comportement inattendu peut se produire si la table CopyReferenceTable a la valeur False.|  
|0x80208321|-2145352927|DTS_W_MATCHINDEXALREADYEXISTS|Une table avec le nom « %1 » attribué à MatchIndexName existe déjà et DropExistingMatchIndex a la valeur FALSE.  L'exécution de la transformation échouera, sauf si cette table est supprimée, si un autre nom est spécifié ou si DropExisitingMatchIndex prend la valeur TRUE.|  
|0x8020832B|-2145352917|DTS_W_TXFUZZYLOOKUP_JOINLENGTHMISMATCH|La longueur de la colonne d'entrée « %1 » n'est pas égale à celle de la colonne de référence « %2 » avec laquelle elle est mise en correspondance.|  
|0x8020832D|-2145352915|DTS_W_TXFUZZYLOOKUP_CODEPAGE_MISMATCH|Les pages de codes de la colonne source DT_STR « %1 » et de la colonne de destination DT_STR « %2 » ne correspondent pas.  Ceci peut engendrer des résultats inattendus.|  
|0x8020832E|-2145352914|DTS_W_FUZZYLOOKUP_TOOMANYEXACTMATCHCOLUMNS|Étant donné qu'il y a plus de 16 jointures de correspondance exactes, les performances risquent de ne pas être optimales. Réduisez le nombre de jointures de correspondance exactes pour améliorer les performances. SQL Server étant limité à 16 colonnes par index, l'index inversé sera utilisé pour toutes les recherches.|  
|0x80208350|-2145352880|DTS_W_FUZZYLOOKUP_MEMLIMITANDEXHAUSTIVESPECIFIED|L'option Exhaustive nécessite le chargement de la référence complète dans la mémoire principale.  Étant donné qu'une limite de mémoire a été spécifiée pour la propriété MaxMemoryUsage, il est possible que la table de référence ne puisse pas être contenue entièrement dans cette limite et que l'opération de correspondance échoue au moment de l'exécution.|  
|0x80208351|-2145352879|DTS_W_FUZZYLOOKUP_EXACTMATCHCOLUMNSEXCEEDBYTELIMIT|Les longueurs cumulées des colonnes spécifiées dans les jointures de correspondance exacte dépassent la limite de 900 octets pour les clés d'index.  La recherche floue crée un index sur les colonnes de correspondance exacte pour augmenter les performances de recherche et il est possible que la création de cet index échoue et que la recherche fasse appel à une autre méthode, éventuellement plus lente, pour trouver des correspondances. Si les performances posent un problème, essayez de supprimer certaines colonnes de jointure de correspondance exacte ou réduisez les longueurs maximales des colonnes de correspondance exacte de longueur variable.|  
|0x80208352|-2145352878|DTS_W_FUZZYLOOKUP_EXACTMATCHINDEXCREATIONFAILED|Impossible de créer un index pour les colonnes de correspondance exact. Retour à l'autre méthode, la recherche floue.|  
|0x80208353|-2145352877|DTS_W_FUZZYGROUPINGINTERNALPIPELINEWARNING|L'avertissement suivant de pipeline interne de regroupement approximatif s'est produit avec le code d'erreur 0x%1!8.8X! : « %2 ».|  
|0x80208375|-2145352843|DTS_W_XMLSRCOUTPUTCOLUMNLENGTHSETTODEFAULT|Aucune longueur maximale n'a été spécifiée pour la %1 avec le type de données externe %2. Le type de données de la tâche de flux de données SSIS « %3 » ayant une longueur de %4!d! sera utilisé.|  
|0x80208376|-2145352842|DTS_W_XMLSRCOUTPUTCOLUMNDATATYPEMAPPEDTOSTRING|%1 fait référence au type de données externe %2, qui ne peut pas être mappé au type de données d'une tâche de flux de données SSIS.  Le type de données DT_WSTR de la tâche de flux de données SSIS ayant une longueur de %3!d! sera utilisé à la place.|  
|0x80208385|-2145352827|DTS_W_NOREDIRECTWITHATTACHEDERROROUTPUTS|Aucune ligne ne sera envoyée aux sorties d'erreur. Configurez les dispositions des erreurs ou des troncations afin de rediriger les lignes vers les sorties d'erreur, ou supprimez les transformations ou les destinations de flux de données qui sont attachées aux sorties d'erreur.|  
|0x80208386|-2145352826|DTS_W_REDIRECTWITHNOATTACHEDERROROUTPUTS|Les lignes envoyées aux sorties d'erreur seront perdues. Ajoutez de nouvelles transformations ou destinations de flux de données pour recevoir des lignes d'erreur, ou reconfigurez le composant de manière à arrêter de rediriger les lignes vers les sorties d'erreur.|  
|0x80208391|-2145352815|DTS_W_XMLSRCOUTPUTCOLUMNLENGTHSETTOMAXIMUM|Pour la %1 avec le type de données externe %2, le schéma XML a spécifié une contrainte maxLength %3!d! qui dépasse la longueur de colonne maximale %4!d!. Le type de données de la tâche de flux de données SSIS « %5 » ayant une longueur de %6!d! sera utilisé.|  
|0x802090E4|-2145349404|DTS_W_TXLOOKUP_DUPLICATE_KEYS|%1 a rencontré des valeurs de clé de référence dupliquées lors de la mise en mémoire cache des données de référence. Cette erreur se produit en mode Cache complet uniquement. Supprimez les valeurs de clé dupliquées ou changez le mode du cache en PARTIAL ou NO_CACHE.|  
|0x802092A7|-2145348953|DTS_W_POTENTIALTRUNCATIONFROMDATAINSERTION|Une troncation peut se produire à cause de l'insertion de données issues de la colonne de flux de données « %1 » avec une longueur de %2!d! dans la colonne de base de données « %3 » avec une longueur de %4!d!.|  
|0x802092A8|-2145348952|DTS_W_POTENTIALTRUNCATIONFROMDATARETRIEVAL|Une troncation peut se produire suite à la récupération de données issues de la colonne de base de données « %1 » avec une longueur de %2!d! dans la colonne de flux de données « %3 » avec une longueur de %4!d!.|  
|0x802092AA|-2145348950|DTS_W_ADODESTBATCHNOTSUPPORTEDFORERRORDISPOSITION|Le mode batch n'est pas pris en charge actuellement lorsque la disposition de ligne d'erreur est utilisée. La propriété BatchSize aura la valeur 1.|  
|0x802092AB|-2145348949|DTS_W_ADODESTNOROWSINSERTED|Aucune ligne n'a été correctement insérée dans la destination. Ceci est peut-être dû à une non-correspondance des types de données entre les colonnes, ou à l'utilisation d'un type de données non pris en charge par votre fournisseur ADO.NET. Dans la mesure où la disposition d'erreur pour ce composant n'est pas « Composant défaillant », les messages d'erreur ne sont pas affichés ici ; pour les afficher, affectez la valeur « Composant défaillant » à la disposition d'erreur.|  
|0x802092AC|-2145348948|DTS_W_ADODESTPOTENTIALDATALOSS|Une perte de données éventuelle peut se produire suite à l'insertion de données de la colonne d'entrée « %1 » avec le type de données « %2 » dans la colonne externe « %3 » avec le type de données « %4 ». Si cela est intentionnel, une autre façon d'effectuer la conversion consiste à utiliser un composant de conversion de données avant le composant de destination ADO NET.|  
|0x802092AD|-2145348947|DTS_W_ADODESTEXTERNALCOLNOTMATCHSCHEMACOL|Le %1 et la colonne de base de données sont désynchronisés.  La colonne la plus récente a %2. Utilisez l’éditeur avancé pour actualiser les colonnes de destination disponibles si nécessaire.|  
|0x802092AE|-2145348946|DTS_W_ADODESTEXTERNALCOLNOTEXIST|Le %1 n'existe pas dans la base de données. Il a peut-être été supprimé ou renommé. Utilisez l'éditeur avancé pour actualiser les colonnes de destination disponibles si nécessaire.|  
|0x802092AF|-2145348945|DTS_W_ADODESTNEWEXTCOL|Une nouvelle colonne portant le nom %1 a été ajoutée à la table de base de données externe. Utilisez l'éditeur avancé pour actualiser les colonnes de destination disponibles si nécessaire.|  
|0x8020930C|-2145348852|DTS_W_NOMATCHOUTPUTGETSNOROWS|Aucune ligne ne sera envoyée à la sortie sans correspondance. Configurez la transformation pour rediriger les lignes sans entrées correspondantes vers la sortie sans correspondance, ou supprimez les transformations du flux de données ou les destinations attachées à la sortie sans correspondance.|  
|0x8020931B|-2145348837|DTS_W_ADODESTINVARIANTEXCEPTION|Exception reçue lors de l'énumération des fournisseurs ADO.NET. Le nom invariant était « %1 ». Le message d'exception est : « %2 »|  
|0xC020822C|-1071611348|DTS_W_UNMAPPEDOUTPUTCOLUMN|%1 ne possède pas de colonne mappée.|  
|0x930D|37645|DTS_W_EXTERNALTABLECOLSOUTOFSYNC|La table « %1 » a été modifiée. Il se peut que de nouvelles colonnes y aient été ajoutées.|  
  
##  <a name="msgInfo"></a> Messages d'information  
 Les noms symboliques des messages d’information de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] commencent par **DTS_I_**.  
  
|Code hexadécimal|Code décimal|Nom symbolique|Description|  
|----------------------|------------------|-------------------|-----------------|  
|0x4001100A|1073811466|DTS_I_STARTINGTRANSACTION|Démarrage de la transaction distribuée pour ce conteneur.|  
|0x4001100B|1073811467|DTS_I_COMMITTINGTRANSACTION|La validation de la transaction distribuée a commencé par ce conteneur.|  
|0x4001100C|1073811468|DTS_I_ABORTINGTRANSACTION|Abandon de la transaction distribuée en cours.|  
|0x40013501|1073820929|DTS_I_GOTMUTEXFROMWAIT|Le mutex « %1 » a été correctement acquis.|  
|0x40013502|1073820930|DTS_I_RELEASEACQUIREDMUTEX|Le mutex « %1 » a été correctement libéré.|  
|0x40013503|1073820931|DTS_I_NEWMUTEXCREATED|Le mutex « %1 » a été correctement créé.|  
|0x40015101|1073828097|DTS_I_DUMP_ON_ANY_ERR|Des fichiers de vidage du débogage seront générés pour tout événement d'erreur.|  
|0x40015102|1073828098|DTS_I_DUMP_ON_CODES|Des fichiers de vidage du débogage seront générés pour les codes d'événement suivants : « %1 »|  
|0x40015103|1073828099|DTS_I_START_DUMP|Le code d'événement 0x%1!8.8X! a déclenché la génération de fichiers de vidage du débogage dans le dossier « %2 ».|  
|0x40015104|1073828100|DTS_I_SSIS_INFO_DUMP|Création du fichier de vidage d'informations SSIS « %1 ».|  
|0x40015106|1073828102|DTS_I_FINISH_DUMP|Les fichiers de vidage du débogage ont été correctement créés.|  
|0x40016019|1073831961|DTS_I_PACKAGEMIGRATED|Le format de package a été migré de la version %1!d! à la version %2!d!. Le package doit être enregistré pour conserver les modifications de migration.|  
|0x4001601A|1073831962|DTS_I_SCRIPTSMIGRATED|Les scripts du package ont été migrés. Pour que les modifications de la migration soient conservées, le package doit être enregistré.|  
|0x40016025|1073831973|DTS_I_FTPRECEIVEFILE|Réception du fichier « %1 » en cours.|  
|0x40016026|1073831974|DTS_I_FTPSENDFILE|Envoi du fichier « %1 » en cours.|  
|0x40016027|1073831975|DTS_I_FTPFILEEXISTS|Le fichier « %1 » existe déjà.|  
|0x40016028|1073831976|DTS_I_FTPERRORLOADINGMSG|Impossible d'obtenir des informations complémentaires sur l'erreur, en raison d'une erreur interne.|  
|0x40016036|1073831990|DTS_I_FTPDELETEFILE|Échec de la tentative de suppression du fichier « %1 ». Cela peut se produire lorsque le fichier n'existe pas, que le nom du fichier comporte une erreur orthographique, ou que vous ne disposez pas des autorisations nécessaires à la suppression du fichier.|  
|0x40016037|1073831991|DTS_I_CONFIGFROMREG|Le package tente d'effectuer une configuration à partir d'une entrée du Registre à l'aide de la clé de Registre « %1 ».|  
|0x40016038|1073831992|DTS_I_CONFIGFROMENVVAR|Le package tente d'effectuer une configuration à partir de la variable d'environnement « %1 ».|  
|0x40016039|1073831993|DTS_I_CONFIGFROMINIFILE|Le package tente d'effectuer une configuration à partir du fichier .ini « %1 ».|  
|0x40016040|1073832000|DTS_I_CONFIGFROMSQLSERVER|Le package tente d'effectuer une configuration à partir de SQL Server à l'aide de la chaîne de configuration « %1 ».|  
|0x40016041|1073832001|DTS_I_CONFIGFROMFILE|Le package tente d'effectuer une configuration à partir du fichier XML « %1 ».|  
|0x40016042|1073832002|DTS_I_CONFIGFROMPARENTVARIABLE|Le package tente d'effectuer une configuration à partir de la variable parente « %1 ».|  
|0x40016043|1073832003|DTS_I_ATTEMPTINGUPGRADEOFDTS|Tentative de mise à niveau de SSIS, de la version « %1 » à la version « %2 ». Ce package tente de mettre à niveau le fichier exécutable.|  
|0x40016044|1073832004|DTS_I_ATTEMPTINGUPGRADEOFANEXTOBJ|Tentative de mise à niveau de « %1 ». Ce package tente de mettre à niveau un objet extensible.|  
|0x40016045|1073832005|DTS_I_SAVECHECKPOINTSTOFILE|Le package enregistrera des points de contrôle dans le fichier « %1 » au cours de l'exécution. Ce package est configuré de manière à enregistrer les points de contrôle.|  
|0x40016046|1073832006|DTS_I_RESTARTFROMCHECKPOINTFILE|Le package a redémarré à partir du fichier de point de contrôle « %1 ». Ce package a été configuré de manière à redémarrer à partir du point de contrôle.|  
|0x40016047|1073832007|DTS_I_CHECKPOINTSAVEDTOFILE|Le fichier de point de contrôle « %1 » a été mis à jour pour enregistrer l'achèvement de ce conteneur.|  
|0x40016048|1073832008|DTS_I_CHECKPOINTFILEDELETED|Le fichier de point de contrôle « %1 » a été supprimé après l'achèvement réussi du package.|  
|0x40016049|1073832009|DTS_I_CHECKPOINTSAVINGTOFILE|Démarrage de la mise à jour du fichier de point de contrôle « %1 ».|  
|0x40016051|1073832017|DTS_I_CHOSENMAXEXECUTABLES|En fonction de la configuration du système, le nombre maximum de fichiers exécutables concurrents est de %1!d!.|  
|0x40016052|1073832018|DTS_I_MAXEXECUTABLES|Le nombre maximum de fichiers exécutables concurrents est de %1!d!.|  
|0x40016053|1073832019|DTS_I_PACKAGESTART|Début de l'exécution du package.|  
|0x40016054|1073832020|DTS_I_PACKAGEEND|Fin de l'exécution du package.|  
|0x40029161|1073910113|DTS_I_FSTASK_DIRECTORYDELETED|Le répertoire « %1 » a été supprimé.|  
|0x40029162|1073910114|DTS_I_FSTASK_FILEDELETED|Le fichier ou le répertoire « %1 » a été supprimé.|  
|0x400292A8|1073910440|DTS_I_TRANSFERDBTASK_OVERWRITEDB|Remplacement de la base de données « %1 » sur le serveur de destination « %2 ».|  
|0x4002F304|1073935108|DTS_I_SOMETHINGHAPPENED|"%1".|  
|0x4002F323|1073935139|DTS_I_ERRMSGTASK_SKIPPINGERRORMESSAGEALREADYEXISTS|Le message d'erreur « %1 » est ignoré car il existe déjà sur le serveur de destination.|  
|0x4002F326|1073935142|DTS_I_ERRMSGTASK_TRANSFEREDNERRORMESSAGES|Les messages d'erreur « %1 » ont été transférés.|  
|0x4002F351|1073935185|DTS_I_STOREDPROCSTASKS_TRANSFEREDNSPS|La tâche a transféré les procédures stockées « %1 ».|  
|0x4002F352|1073935186|DTS_I_TRANSOBJECTSTASK_TRANSFEREDNOBJECTS|Objets « %1 » transférés.|  
|0x4002F358|1073935192|DTS_I_TRANSOBJECTSTASK_NOSPSTOTRANSFER|Aucune procédure stockée à transférer.|  
|0x4002F362|1073935202|DTS_I_TRANSOBJECTSTASK_NORULESTOTRANSFER|Aucune règle à transférer.|  
|0x4002F366|1073935206|DTS_I_TRANSOBJECTSTASK_NOTABLESTOTRANSFER|Aucune table à transférer.|  
|0x4002F370|1073935216|DTS_I_TRANSOBJECTSTASK_NOVIEWSTOTRANSFER|Aucune vue à transférer.|  
|0x4002F374|1073935220|DTS_I_TRANSOBJECTSTASK_NOUDFSTOTRANSFER|Aucune fonction définie par l'utilisateur à transférer.|  
|0x4002F378|1073935224|DTS_I_TRANSOBJECTSTASK_NODEFAULTSTOTRANSFER|Aucune valeur par défaut à transférer.|  
|0x4002F382|1073935234|DTS_I_TRANSOBJECTSTASK_NOUDDTSTOTRANSFER|Aucun type de données défini par l'utilisateur à transférer.|  
|0x4002F386|1073935238|DTS_I_TRANSOBJECTSTASK_NOPFSTOTRANSFER|Aucune fonction de partition à transférer.|  
|0x4002F390|1073935248|DTS_I_TRANSOBJECTSTASK_NOPSSTOTRANSFER|Aucun schéma de partition à transférer.|  
|0x4002F394|1073935252|DTS_I_TRANSOBJECTSTASK_NOSCHEMASTOTRANSFER|Aucun schéma à transférer.|  
|0x4002F398|1073935256|DTS_I_TRANSOBJECTSTASK_NOSQLASSEMBLIESTOTRANSFER|Aucun SqlAssembly à transférer.|  
|0x4002F402|1073935362|DTS_I_TRANSOBJECTSTASK_NOAGGREGATESTOTRANSFER|Aucune fonction d'agrégation à transférer.|  
|0x4002F406|1073935366|DTS_I_TRANSOBJECTSTASK_NOTYPESTOTRANSFER|Aucun type défini par l'utilisateur à transférer.|  
|0x4002F410|1073935376|DTS_I_TRANSOBJECTSTASK_NOXMLSCHEMACOLLECTIONSTOTRANSFER|Aucun XmlSchemaCollection à transférer.|  
|0x4002F418|1073935384|DTS_I_TRANSOBJECTSTASK_NOLOGINSTOTRANSFER|Aucun nom d'accès à transférer.|  
|0x4002F41D|1073935389|DTS_I_TRANSOBJECTSTASK_NOUSERSTOTRANSFER|Aucun utilisateur à transférer.|  
|0x4002F41E|1073935390|DTS_I_TRANSOBJECTSTASK_TRUNCATINGTABLE|Troncation de la table « %1 »|  
|0x40043006|1074016262|DTS_I_EXECUTIONPHASE_PREPAREFOREXECUTE|Début de la phase Préparation à l'exécution.|  
|0x40043007|1074016263|DTS_I_EXECUTIONPHASE_PREEXECUTE|Début de la phase Pré-exécution.|  
|0x40043008|1074016264|DTS_I_EXECUTIONPHASE_POSTEXECUTE|Début de la phase Post-exécution.|  
|0x40043009|1074016265|DTS_I_EXECUTIONPHASE_CLEANUP|Début de la phase Nettoyage.|  
|0x4004300A|1074016266|DTS_I_EXECUTIONPHASE_VALIDATE|Début de la phase Validation.|  
|0x4004300B|1074016267|DTS_I_ROWS_WRITTEN|« %1 » a écrit %2!ld! lignes.|  
|0x4004300C|1074016268|DTS_I_EXECUTIONPHASE_EXECUTE|Début de la phase Exécuter.|  
|0x4004800C|1074036748|DTS_I_CANTRELIEVEPRESSURE|Le gestionnaire de tampons a détecté que la quantité de mémoire virtuelle du système était faible, mais il n'a pas pu permuter les tampons. %1!d! tampons ont été analysés et %2!d! ont été verrouillés. Soit la mémoire disponible pour le pipeline est insuffisante car la mémoire installée ne suffit pas, soit d'autres processus l'utilisent. Il se peut également que le nombre de tampons verrouillés soit trop important.|  
|0x4004800D|1074036749|DTS_I_CANTALLOCATEMEMORYPRESSURE|Le gestionnaire de tampons n'a pas réussi à effectuer un appel d'allocation de mémoire de octets, mais il n'a pas pu permuter les tampons pour que la mémoire soit moins sollicitée. %1!d! tampons ont été analysés et %2!d! ont été verrouillés. Soit la quantité de mémoire disponible pour le pipeline est insuffisante car la mémoire installée ne suffit pas, soit d'autres processus l'utilisent. Il se peut également que le nombre de tampons verrouillés soit trop important.|  
|0x4004800E|1074036750|DTS_I_ALLOCATEDDURINGMEMORYPRESSURE|Le gestionnaire de tampons a alloué %1!d! octets, bien qu'une sollicitation de la mémoire ait été détectée et que des tentatives répétées d'échange de mémoires tampons aient échoué.|  
|0x400490F4|1074041076|DTS_I_TXLOOKUP_CACHE_PROGRESS|%1 a mis en cache %2!d! lignes.|  
|0x400490F5|1074041077|DTS_I_TXLOOKUP_CACHE_FINAL|%1 a mis en cache un total de %2!d! lignes.|  
|0x4020206D|1075847277|DTS_I_RAWSOURCENOCOLUMNS|L'adaptateur de source brute a ouvert un fichier, mais ce dernier ne contient aucune colonne. L'adaptateur ne produira pas de données. Cela peut indiquer un fichier endommagé, ou qu'il n'existe aucune colonne et, par conséquent, aucune donnée.|  
|0x402020DA|1075847386|DTS_I_OLEDBINFORMATIONALMESSAGE|Un message d'information OLE DB est disponible.|  
|0x40208327|1075872551|DTS_I_TXFUZZYLOOKUP_EXACT_MATCH_PERF_COLLATIONS_DONT_MATCH|Les performances de la correspondance approximative peuvent être améliorées si les propriétés FuzzyComparisonFlags des jointures exactes sur la colonne d'entrée « %1 » sont définies pour correspondre à l'assemblage SQL de la colonne de la table de référence « %2 ».  Il est aussi nécessaire qu'aucun indicateur de changement de casse ne soit défini dans FuzzyComparisonFlagsEx.|  
|0x40208328|1075872552|DTS_I_TXFUZZYLOOKUP_EXACT_MATCH_PERF_INDEX_MISSING|Les performances de la correspondance approximative peuvent être améliorées si un index est créé sur la table de référence pour toutes les colonnes de correspondance exacte spécifiées.|  
|0x40208387|1075872647|DTS_I_DISPSNOTREVIEWED|Les dispositions des erreurs et des troncations n'ont pas été revues. Assurez-vous que ce composant est configuré pour rediriger les lignes vers les sorties d'erreur si vous souhaitez transformer ces lignes.|  
|0x402090DA|1075876058|DTS_I_TXAGG_WORKSPACE_REHASH|La transformation d'agrégation a rencontré %1!d! combinaison de touches. Un rehachage des données est nécessaire, car le nombre de combinaisons de clés est supérieur à celui attendu. Ce composant peut être configuré afin d'éviter le rehachage des données, à l'aide des propriétés Keys, KeyScale et AutoExtendFactor.|  
|0x402090DB|1075876059|DTS_I_TXAGG_COUNTDISTINCT_REHASH|La transformation d'agrégation a rencontré %1!d! valeurs distinctes lors d'une agrégation « count distinct » sur « %2 ». Cette transformation effectue le rehachage des données car le nombre de valeurs distinctes est supérieur à celui attendu. Ce composant peut être configuré afin d'éviter le rehachage des données, à l'aide des propriétés CountDistinctKeys, CountDistinctKeyScale et AutoExtendFactor.|  
|0x402090DC|1075876060|DTS_I_STARTPROCESSINGFILE|Le traitement du fichier « %1 » a démarré.|  
|0x402090DD|1075876061|DTS_I_FINISHPROCESSINGFILE|Le traitement du fichier « %1 » est terminé.|  
|0x402090DE|1075876062|DTS_I_TOTALDATAROWSPROCESSEDFORFILE|Le nombre total de lignes de données traitées pour le fichier « %1 » est %2!I64d!.|  
|0x402090DF|1075876063|DTS_I_FINALCOMMITSTARTED|La validation finale de l'insertion de données dans « %1 » a commencé.|  
|0x402090E0|1075876064|DTS_I_FINALCOMMITENDED|La validation finale de l'insertion de données dans « %1 » s'est terminée.|  
|0x402090E1|1075876065|DTS_I_BEGINHASHINGCACHE|%1!u! lignes sont ajoutées au cache. Le système est en train de traiter les lignes.|  
|0x402090E2|1075876066|DTS_I_SUCCEEDEDHASHINGCACHE|%1 a traité %2!u! lignes du cache. Le temps de traitement a été de %3 secondes. Le cache a utilisé %4!I64u! octets de mémoire.|  
|0x402090E3|1075876067|DTS_I_FAILEDHASHINGCACHE|%1 n'a pas pu traiter les lignes du cache.  Le temps de traitement a été de %2 seconde(s).|  
|0x402090E4|1075876068|DTS_I_SUCCEEDEDPREPARINGCACHE|%1 a réussi à préparer le cache. Le temps de préparation a été de %2 secondes.|  
|0x40209314|1075876628|DTS_I_TXLOOKUP_PARTIALPERF|Le %1 a effectué les opérations suivantes : %2!I64u! lignes traitées, %3!I64u! commandes de base de données adressées à la base de données de référence et %4!I64u! recherches effectuées à l'aide du cache partiel.|  
|0x40209315|1075876629|DTS_I_TXLOOKUP_PARTIALPERF2|Le %1 a effectué les opérations suivantes : %2!I64u! lignes traitées, %3!I64u! commandes de base de données adressées à la base de données de référence, %4!I64u! recherches effectuées à l'aide du cache partiel et %5!I64u! recherches utilisant le cache pour les lignes sans entrées correspondantes dans la recherche initiale.|  
|0x40209316|1075876630|DTS_I_CACHEFILEWRITESTARTED|%1 écrit le cache dans le fichier « %2 ».|  
|0x40209317|1075876631|DTS_I_CACHEFILEWRITESUCCEEDED|%1 a écrit le cache dans le fichier « %2 ».|  
|0x4020F42C|1075901484|DTS_I_OLEDBDESTZEROMAXCOMMITSIZE|La propriété Taille de validation d'insertion maximale de la destination OLE DB « %1 » est définie sur 0. Ce paramètre de propriété peut provoquer le blocage du package en cours d'exécution. Pour plus d'informations consultez la rubrique relative à l'aide sur l'Éditeur de destination OLE DB (Page Gestionnaire de connexions) via la touche F1.|  
  
##  <a name="msgGeneral"></a> Messages généraux et d'événement  
 Les noms symboliques des messages d’erreur de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] commencent par **DTS_MSG_**.  
  
|Code hexadécimal|Code décimal|Nom symbolique|Description|  
|----------------------|------------------|-------------------|-----------------|  
|0x1| 1|DTS_MSG_CATEGORY_SERVICE_CONTROL|Fonction incorrecte.|  
|0x2|2|DTS_MSG_CATEGORY_RUNNING_PACKAGE_MANAGEMENT|Le système ne trouve pas le fichier spécifié.|  
|0x100|256|DTS_MSG_SERVER_STARTING|Démarrage du service SSIS Microsoft.<br /><br /> Serveur version %1|  
|0x101|257|DTS_MSG_SERVER_STARTED|Service SSIS Microsoft démarré.<br /><br /> Serveur version %1|  
|0x102|258|DTS_MSG_SERVER_STOPPING|Expiration du délai d'attente de l'opération d'attente.|  
|0x103|259|DTS_MSG_SERVER_STOPPED|Aucune donnée n'est disponible.|  
|0x104|260|DTS_MSG_SERVER_START_FAILED|Échec du démarrage du service SSIS Microsoft.<br /><br /> Erreur : %1|  
|0x105|261|DTS_MSG_SERVER_STOP_ERROR|Erreur lors de l'arrêt du service SSIS.<br /><br /> Erreur : %1|  
|0x110|272|DTS_MSG_SERVER_MISSING_CONFIG|Fichier de configuration du service SSIS Microsoft inexistant.<br /><br /> Chargement avec les paramètres par défaut.|  
|0x111|273|DTS_MSG_SERVER_BAD_CONFIG|Fichier de configuration du service SSIS Microsoft incorrect.<br /><br /> Erreur lors de la lecture du fichier de configuration : %1<br /><br /> Chargement du serveur avec les paramètres par défaut.|  
|0x112|274|DTS_MSG_SERVER_MISSING_CONFIG_REG|Service SSIS Microsoft :<br /><br /> Paramètre de Registre spécifiant que le fichier de configuration n'existe pas.<br /><br /> Tentative de chargement du fichier de configuration par défaut en cours.|  
|0x150|336|DTS_MSG_SERVER_STOPPING_PACKAGE|Service SSIS Microsoft : arrêt de l'exécution du package.<br /><br /> ID d'instance de package : %1<br /><br /> ID du package : %2<br /><br /> Nom du package : %3<br /><br /> Description du package : %4<br /><br /> Package démarré par : %5.|  
|0x40013000|1073819648|DTS_MSG_PACKAGESTART|Package « %1 » démarré.|  
|0x40013001|1073819649|DTS_MSG_PACKAGESUCCESS|Fin du package « %1 » réussie.|  
|0x40013002|1073819650|DTS_MSG_PACKAGECANCEL|Le package « %1 » a été annulé.|  
|0x40013003|1073819651|DTS_MSG_PACKAGEFAILURE|Échec du package « %1 ».|  
|0x40013004|1073819652|DTS_MSG_CANTDELAYLOADDLL|Le module %1 ne peut pas charger le fichier DLL %2 pour appeler le point d'entrée %3 en raison de l'erreur %4. Ce produit requiert l'exécution du fichier DLL, mais ce dernier est introuvable sur ce chemin d'accès.|  
|0x40013005|1073819653|DTS_MSG_CANTDELAYLOADDLLFUNCTION|Le module %1 a chargé le fichier DLL %2 mais il ne peut pas trouver le point d'entrée %3 en raison de l'erreur %4. Le fichier DLL nommé est introuvable sur ce chemin d'accès, or l'exécution de cette DLL est requise par le produit.|  
|0x40103100|1074802944|DTS_MSG_EVENTLOGENTRY|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103101|1074802945|DTS_MSG_EVENTLOGENTRY_PREEXECUTE|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103102|1074802946|DTS_MSG_EVENTLOGENTRY_POSTEXECUTE|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103103|1074802947|DTS_MSG_EVENTLOGENTRY_PREVALIDATE|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103104|1074802948|DTS_MSG_EVENTLOGENTRY_POSTVALIDATE|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103105|1074802949|DTS_MSG_EVENTLOGENTRY_WARNING|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103106|1074802950|DTS_MSG_EVENTLOGENTRY_ERROR|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103107|1074802951|DTS_MSG_EVENTLOGENTRY_TASKFAILED|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103108|1074802952|DTS_MSG_EVENTLOGENTRY_PROGRESS|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x40103109|1074802953|DTS_MSG_EVENTLOGENTRY_EXECSTATCHANGE|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x4010310A|1074802954|DTS_MSG_EVENTLOGENTRY_VARVALCHANGE|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x4010310B|1074802955|DTS_MSG_EVENTLOGENTRY_CUSTOMEVENT|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x4010310C|1074802956|DTS_MSG_EVENTLOGENTRY_PACKAGESTART|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x4010310D|1074802957|DTS_MSG_EVENTLOGENTRY_PACKAGEEND|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
|0x4010310E|1074802958|DTS_MSG_EVENTLOGENTRY_INFORMATION|Nom d'événement : %1<br /><br /> Message : %9<br /><br /> Opérateur : %2<br /><br /> Nom de la source : %3<br /><br /> ID de la source : %4<br /><br /> ID d'exécution : %5<br /><br /> Heure de début : %6<br /><br /> Heure de fin : %7<br /><br /> Code de données : %8|  
  
##  <a name="msgSuccess"></a> Messages de réussite  
 Les noms symboliques des messages de réussite de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] commencent par **DTS_S_**.  
  
|Code hexadécimal|Code décimal|Nom symbolique|Description|  
|----------------------|------------------|-------------------|-----------------|  
|0x40003|262147|DTS_S_NULLDATA|La valeur est NULL.|  
|0x40005|262149|DTS_S_TRUNCATED|La valeur de la chaîne était tronquée. Le tampon a reçu une chaîne qui était trop longue pour la colonne, et la chaîne a été tronquée par le tampon.|  
|0x200001|2097153|DTS_S_EXPREVALTRUNCATIONOCCURRED|Une troncation s'est produite au cours de l'évaluation de l'expression, La troncation risque d'inclure n'importe quel point à une étape intermédiaire.|  
  
##  <a name="msgPipeline"></a> Messages d'erreur des composants de flux de données  
 Les noms symboliques des messages d’erreur de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] commencent par **DTSBC_E_**, où « BC » fait référence à la classe de base native à partir de laquelle la plupart des composants de flux Microsoft sont dérivés.  
  
|Code hexadécimal|Code décimal|Nom symbolique|Description|  
|----------------------|------------------|-------------------|-----------------|  
|0xC8000002|-939524094|DTSBC_E_INCORRECTEXACTNUMBEROFTOTALOUTPUTS|Le nombre total de sorties et de sorties d'erreur, %1!lu!, est incorrect. Il doit y en avoir exactement %2!lu!.|  
|0xC8000003|-939524093|DTSBC_E_FAILEDTOGETOUTPUTBYINDEX|Impossible d'extraire la sortie à partir de l'index %1!lu!.|  
|0xC8000005|-939524091|DTSBC_E_INCORRECTEXACTNUMBEROFERROROUTPUTS|Le nombre de sorties d'erreur, %1!lu!, est incorrect. Il doit y en avoir exactement %2!lu!.|  
|0xC8000006|-939524090|DTSBC_E_INVALIDVALIDATIONSTATUSVALUE|La valeur d’état de validation « %1!lu! » est incorrecte ".  Ce doit être l'une des valeurs figurant dans l'énumération DTSValidationStatus.|  
|0xC8000007|-939524089|DTSBC_E_INPUTHASNOOUTPUT|Aucune sortie synchrone pour l’entrée « %1!lu! ».|  
|0xC8000008|-939524088|DTSBC_E_INPUTHASNOERROROUTPUT|Aucune sortie synchrone n’a pas de sortie d’erreur synchrone.|  
|0xC8000009|-939524087|DTSBC_E_INVALIDHTPIVALUE|La valeur de HowToProcessInput, %1!lu!, n'est pas valide. Ce doit être l'une des valeurs provenant de l'énumération HowToProcessInput.|  
|0xC800000A|-939524086|DTSBC_E_FAILEDTOGETCOLINFO|Échec de l’obtention d’informations pour la ligne « %1!ld! », colonne « %2!ld!» à partir du tampon.  Code d'erreur retourné : 0x%3!8.8X!.|  
|0xC800000B|-939524085|DTSBC_E_FAILEDTOSETCOLINFO|Échec de la définition d’informations pour la ligne « %1!ld! », colonne « %2!ld! » dans le tampon.  Code d'erreur retourné : 0x%3!8.8X!.|  
|0xC800000C|-939524084|DTSBC_E_INVALIDPROPERTY|La propriété « %1 » n'est pas valide.|  
|0xC800000D|-939524083|DTSBC_E_PROPERTYNOTFOUND|La propriété « %1 » est introuvable.|  
|0xC8000010|-939524080|DTSBC_E_READONLYPROPERTY|Erreur lors de l'affectation d'une valeur à la propriété en lecture seule « %1 ».|  
|0xC8000011|-939524079|DTSBC_E_CANTINSERTOUTPUTCOLUMN|%1 ne permet pas l'insertion de colonnes de sortie.|  
|0xC8000012|-939524078|DTSBC_E_OUTPUTCOLUMNSMETADATAMISMATCH|Les métadonnées de la colonne de sortie ne correspondent pas aux métadonnées de la colonne d'entrée associées.  Les métadonnées des colonnes de sortie seront mises à jour.|  
|0xC8000013|-939524077|DTSBC_E_OUTPUTCOLUMNSMISSING|Il existe des colonnes d'entrée sans colonne de sortie associée. Les colonnes de sortie seront ajoutées.|  
|0xC8000014|-939524076|DTSBC_E_TOOMANYOUTPUTCOLUMNS|Il existe des colonnes de sortie sans colonne d'entrée associée. Les colonnes de sortie seront supprimées.|  
|0xC8000015|-939524075|DTSBC_E_OUTPUTCOLUMNSMETADATAMISMATCHUNMAP|Les métadonnées de la colonne de sortie ne correspondent pas aux métadonnées de la colonne d'entrée associées.  Le mappage des colonnes d'entrée sera annulé.|  
|0xC8000016|-939524074|DTSBC_E_UNMAPINPUTCOLUMNS|Il existe des colonnes d'entrée sans colonne de sortie associée. Le mappage des colonnes d'entrée sera annulé.|  
|0xC8000017|-939524073|DTSBC_E_MULTIPLEINCOLSTOOUTCOL|Il existe une colonne d'entrée associée à une colonne de sortie, tandis que cette colonne de sortie est déjà associée à une autre colonne d'entrée sur la même entrée.|  
|0xC8000018|-939524072|DTSBC_E_CANTINSERTEXTERNALMETADATACOLUMN|%1 ne permet pas d'insérer des colonnes de métadonnées externes.|  
  
  
