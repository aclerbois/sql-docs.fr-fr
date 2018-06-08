---
title: Architecture physique (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 99e2cf7386bdf395ac82f05cc0b94f3e2b5e3123
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="physical-architecture-analysis-services---data-mining"></a>Architecture physique (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les composants serveur et client pour fournir des fonctionnalités d’exploration de données pour les applications décisionnelles :  
  
-   Le composant serveur est implémenté comme un service Microsoft Windows. Vous pouvez avoir plusieurs instances sur le même ordinateur, chaque instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] étant implémentée comme une instance séparée du service Windows.  
  
-   Les clients communiquent avec [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide de la norme publique XMLA (XML for Analysis), un protocole SOAP qui permet d’émettre des commandes et de recevoir des réponses, exposée en tant que service web. Des modèles objet clients sont aussi fournis via XMLA, et sont accessibles soit à l'aide d'un fournisseur managé, notamment ADOMD.NET ou un fournisseur OLE DB natif.  
  
-   Les commandes de requête peuvent être émises à l'aide des extensions DMX (Data Mining Extensions), un langage de requête standard orienté vers l'exploration de données. Vous pouvez aussi utiliser ASSL (Analysis Services Scripting Language) pour gérer les objets de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
## <a name="architectural-diagram"></a>Diagramme architectural  
 Une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s’exécute comme un service autonome et la communication avec le service s’effectue via XML for Analysis (XMLA), à l’aide de HTTP ou de TCP.  
  
 AMO est une couche se trouvant entre l’application utilisateur et l’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui fournit l’accès aux objets d’administration [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . AMO est une bibliothèque de classes qui prend des commandes d’une application cliente et convertit ces commandes en messages XMLA pour l’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . AMO présente les objets d'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en tant que classes à l'application de l'utilisateur final, avec des membres de méthode qui exécutent des commandes et des membres de propriété qui contiennent les données des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 L'illustration suivante présente l'architecture des composants [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , y compris les services dans l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et tous les composants utilisateur qui interagissent avec l'instance.  
  
 L'illustration montre que la seule façon d'accéder à l'instance est d'utiliser le composant d'écoute XMLA (XML for Analysis), à l'aide de HTTP ou TCP.  
  
> [!WARNING]  
>  DSO est déconseillé. Vous ne devez pas utiliser DSO pour développer des solutions.  
  
 ![Diagramme d’Architecture de Analysis Services](../../analysis-services/data-mining/media/analysisservicessystemarchitecture.gif "de diagramme d’Architecture système Analysis Services")  
  
## <a name="server-configuration"></a>Configuration du serveur  
 Une instance de serveur peut prendre en charge plusieurs bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , chacune avec sa propre instance du service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui répond aux demandes des clients et traite les objets.  
  
 Des instances distinctes doivent être installées si vous souhaitez utiliser les modèles tabulaires et l'exploration de données et/ou les modèles multidimensionnels. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge l'installation côte à côte des instances s'exécutant en mode tabulaire (qui utilise le moteur d'analyse en mémoire xVelocity (VertiPaq)) et des instances s'exécutant dans l'une des configurations standard OLAP, MOLAP ou ROLAP. Pour plus d’informations, consultez [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md).  
  
 Toutes les communications entre un client et le serveur Analysis Services utilisent la spécification XMLA, qui est un protocole indépendant de la plateforme et de la langue. Lorsqu'une demande est reçue d'un client, Analysis Services détermine si elle est liée à OLAP ou à l'exploration de données, avant de l'acheminer de manière appropriée. Pour plus d’informations, consultez [Composants serveur du moteur OLAP](../../analysis-services/multidimensional-models/olap-physical/olap-engine-server-components.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Architecture logique & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/logical-architecture-analysis-services-data-mining.md)  
  
  
