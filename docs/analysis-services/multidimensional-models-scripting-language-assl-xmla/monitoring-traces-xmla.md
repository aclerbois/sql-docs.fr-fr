---
title: Surveillance de Traces (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 24aab35b34ed9339ec2d7950efab21a94b2c0d96
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="monitoring-traces-xmla"></a>Surveillance de traces (XMLA)
  Vous pouvez utiliser la [s’abonner](../../analysis-services/xmla/xml-elements-commands/subscribe-element-xmla.md) commande XML for Analysis (XMLA) pour surveiller une trace existante définie sur une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le **s’abonner** commande retourne les résultats d’une trace en tant qu’un ensemble de lignes.  
  
## <a name="specifying-a-trace"></a>Spécification d'une trace  
 Le [objet](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md) propriété de la **s’abonner** commande doit contenir une référence d’objet à un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance ou une trace sur un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance. Si le **objet** propriété n’est pas spécifiée, ou un identificateur de trace n’est pas spécifié dans le **objet** propriété, le **s’abonner** commande surveille la trace de session par défaut pour la session explicite spécifiée dans l’en-tête SOAP pour la commande.  
  
## <a name="returning-results"></a>Retour de résultats  
 Le **s’abonner** retourne un ensemble de lignes contenant les événements de trace capturées par la trace spécifiée. Le **s’abonner** commande retourne les résultats de la trace jusqu'à ce que la commande est annulée par le [Annuler](../../analysis-services/xmla/xml-elements-commands/cancel-element-xmla.md) commande.  
  
 Cet ensemble de lignes se compose des colonnes répertoriées dans le tableau suivant.  
  
|Colonne|Data type| Description|  
|------------|---------------|-----------------|  
|EventClass|Entier|Classe d'événements de l'événement reçu par la trace.|  
|EventSubclass|Entier long|Sous-classe d'événements de l'événement reçu par la trace.|  
|CurrentTime|DateTime|Heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|DateTime|Heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|DateTime|Heure de fin de l'événement, si elle est disponible. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».<br /><br /> Cette colonne n'est pas remplie pour les classes d'événements qui décrivent le lancement d'un processus ou d'une action.|  
|Duration|Entier long|Durée totale (en millisecondes) de l'événement.|  
|CPUTime|Entier long|Temps processeur total (en millisecondes) écoulé pour l'événement.|  
|JobID|Entier long|Identificateur de travail du processus.|  
|SessionID|Chaîne|Identificateur de la session pour laquelle l'événement s'est produit.|  
|SessionType|Chaîne|Type de la session pour laquelle l'événement s'est produit.|  
|ProgressTotal|Entier long|Nombre total ou progression signalé par l'événement.|  
|IntegerData|Entier long|Données entières associées à l'événement. Le contenu de cette colonne dépend de la classe et de la sous-classe d'événements de l'événement.|  
|ObjectID|Chaîne|Identificateur de l'objet pour lequel l'événement s'est produit.|  
|ObjectType|Chaîne|Type de l'objet spécifié dans ObjectName.|  
|ObjectName|Chaîne|Nom de l'objet pour lequel l'événement s'est produit.|  
|ObjectPath|Chaîne|Chemin d'accès hiérarchique de l'objet pour lequel l'événement s'est produit. Le chemin d'accès est représenté par une chaîne délimitée par des virgules correspondant aux identificateurs d'objet des parents de l'objet spécifiés dans ObjectName.|  
|ObjectReference|Chaîne|Représentation XML de la référence de l'objet spécifié dans ObjectName.|  
|NestLevel|Entier|Niveau de la transaction pour laquelle l'événement s'est produit.|  
|NumSegments|Entier long|Nombre de segments de données affectés ou atteints par la commande pour laquelle l'événement s'est produit.|  
|Severity|Entier|Niveau de gravité d'une exception relative à l'événement. La colonne peut contenir l'une des valeurs suivantes :<br /><br /> <br /><br /> 0 : réussite<br /><br /> <br /><br /> 1 : informations<br /><br /> <br /><br /> 2 : avertissement<br /><br /> <br /><br /> 3 : erreur|  
|Réussi|Booléen|Indique si une commande a abouti ou échoué.|  
|Erreur|Entier long|Numéro d'erreur de l'événement, le cas échéant.|  
|ConnectionID|Chaîne|Identificateur de la connexion pour laquelle l'événement s'est produit.|  
|DatabaseName|Chaîne|Nom de la base de données pour laquelle l'événement s'est produit.|  
|NTUserName|Chaîne|Nom d'utilisateur Windows de l'utilisateur associé à l'événement.|  
|NTDomainName|Chaîne|Domaine Windows de l'utilisateur associé à l'événement.|  
|ClientHostName|Chaîne|Nom de l'ordinateur sur lequel l'application cliente est exécutée. Cette colonne est remplie des valeurs transmises par l'application cliente.|  
|ClientProcessID|Entier long|Identificateur de processus de l'application cliente.|  
|ApplicationName|Chaîne|Nom de l'application cliente qui a créé la connexion à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Cette colonne est remplie des valeurs transmises par l'application cliente et non du nom affiché du programme.|  
|NTCanonicalUserName|Chaîne|Nom d'utilisateur canonique Windows de l'utilisateur associé à l'événement.|  
|SPID|Chaîne|ID de processus serveur (SPID) de la session pour laquelle l'événement s'est produit. La valeur de cette colonne correspond directement à l'ID de session spécifié dans l'en-tête SOAP du message XMLA pour lequel l'événement s'est produit.|  
|TextData|Chaîne|Les données de texte associées à l’événement. Le contenu de cette colonne dépend de la classe et de la sous-classe d'événements de l'événement.|  
|ServerName|Chaîne|Nom de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour laquelle l'événement s'est produit.|  
|RequestParameters|Chaîne|Paramètres de la requête paramétrable ou de la commande XMLA pour laquelle l'événement s'est produit.|  
|RequestProperties|Chaîne|Propriétés de la méthode XMLA pour laquelle l'événement s'est produit.|  
  
## <a name="see-also"></a>Voir aussi  
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
