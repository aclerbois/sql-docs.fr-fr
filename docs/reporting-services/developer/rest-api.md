---
title: Développer avec les API REST pour Reporting Services | Microsoft Docs
ms.description: The REST API provides programmatic access to the objects in a SQL Server 2017 Reporting Services report server catalog.
ms.date: 05/25/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: developer
ms.reviewer: ''
ms.suite: pro-bi
ms.custom: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 8f2f0959639736379bc28c6add71d09769352fed
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34553830"
---
# <a name="develop-with-the-rest-apis-for-reporting-services"></a>Développer avec les API REST pour Reporting Services

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)])

Microsoft SQL Server 2017 Reporting Services prend en charge les API REST (Representational State Transfer). Les API REST sont des points de terminaison de service prenant en charge des opérations HTTP (méthodes) qui fournissent, créent, récupèrent, mettent à jour ou suppriment l’accès aux ressources d’un serveur de rapports.

L’API REST fournit un accès par programmation aux objets d’un catalogue de serveur de rapports SQL Server 2017 Reporting Services. Parmi ces objets figurent les dossiers, les rapports, les indicateurs de performance clés, les sources de données, les jeux de données, les plans d’actualisation et les abonnements. À l’aide de l’API REST, vous pouvez, par exemple, parcourir l’arborescence des dossiers, découvrir le contenu d’un dossier ou télécharger une définition de rapport. Vous pouvez également créer, mettre à jour et supprimer des objets. Parmi les exemples d’utilisation des objets citons le chargement d’un rapport, l’exécution d’un plan d’actualisation ou la suppression d’un dossier.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-hybrid-note.md)]

## <a name="components-of-a-rest-api-requestresponse"></a>Composants d’une demande/réponse d’API REST

Une paire demande/réponse d’API REST peut être divisée en cinq composants :

* **L’URI de demande**, qui se compose de : `{URI-scheme} :// {URI-host} / {resource-path} ? {query-string}`. Bien que l’URI de demande soit inclus dans l’en-tête de message de la demande, nous l’appelons séparément ici, car la plupart des langages ou des frameworks vous obligent à le transmettre séparément du message de demande.

    * Schéma d’URI : indique le protocole utilisé pour transmettre la demande. Par exemple, `http` ou `https`.
    * Hôte de l’URI : spécifie le nom de domaine ou l’adresse IP du serveur où le point de terminaison de service REST est hébergé, tel que `myserver.contoso.com`.
    * Chemin de la ressource : spécifie la ressource ou la collection de ressources, qui peut inclure plusieurs segments utilisés par le service pour déterminer la sélection de ces ressources. Par exemple : `CatalogItems(01234567-89ab-cdef-0123-456789abcdef)/Properties` peut être utilisé pour obtenir les propriétés spécifiées pour le CatalogItem.
    * Chaîne de requête (facultative) : fournit des paramètres simples supplémentaires, tels que la version de l’API ou les critères de sélection des ressources.

* Champs d’en-tête du message de requête HTTP :

    * [Méthode HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) obligatoire (également appelée opération ou verbe), qui indique au service le type d’opération que vous demandez. Les API REST Reporting Services prennent en charge les méthodes DELETE, GET, HEAD, PUT, POST et PATCH.
    * Des champs d’en-tête supplémentaires facultatifs, suivant l’URI et la méthode HTTP spécifiés.

* Des champs du **corps de message de demande** HTTP, pour prendre en charge l’URI et l’opération HTTP. Par exemple, les opérations POST contiennent des objets codés au format MIME qui sont passés comme paramètres complexes. Pour les opérations POST ou PUT, le type de codage MIME pour le corps doit être spécifié dans l’en-tête de demande `Content-type`. Certains services vous obligent à utiliser un type MIME spécifique, tel que `application/json`.

* Champs **d’en-tête du message de réponse** HTTP :

    * Un [code d’état HTTP](http://www.w3.org/Protocols/HTTP/HTRESP.html), compris entre 2xx (opérations réussies) et 4xx ou 5xx (erreur). Ou bien un code d’état défini par le service peut être retourné, comme indiqué dans la documentation de l’API.
    * Des champs d’en-tête supplémentaires facultatifs, nécessaires à la prise en charge de la réponse de la demande, tels qu’un en-tête de réponse `Content-type`.

* Champs du **corps du message de réponse** HTTP facultatifs :

    * Les objets de réponse codés au format MIME sont retournés dans le corps de la réponse HTTP, par exemple une réponse à partir d’une méthode GET qui retourne des données. En règle générale, ces objets sont retournés dans un format structuré tel que JSON ou XML, comme indiqué par l’en-tête de réponse `Content-type`.

## <a name="api-documentation"></a>Documentation de l’API

Une API REST moderne exige une documentation d’API moderne. L’API REST repose sur la spécification OpenAPI (également appelé spécification Swagger), et la documentation est disponible sur [SwaggerHub](https://app.swaggerhub.com/api/microsoft-rs/SSRS/2.0). Au-delà de la documentation de l’API, SwaggerHub permet de générer une bibliothèque cliente dans le langage souhaité : JavaScript, TypeScript, C#, Java, Python, Ruby, entre autres.

## <a name="testing-api-calls"></a>Test des appels d’API

Pour tester les messages de demande/réponse HTTP, vous pouvez recourir à l’outil [Fiddler](http://www.telerik.com/fiddler). Ce dernier est un proxy de débogage web gratuit capable d’intercepter vos demandes REST, facilitant ainsi le diagnostic des messages de demande/réponse HTTP.

## <a name="next-steps"></a>Étapes suivantes

Passez en revue les API disponibles sur [SwaggerHub](https://app.swaggerhub.com/api/microsoft-rs/SSRS/2.0).

Des exemples sont disponibles sur [GitHub](https://github.com/Microsoft/Reporting-Services). L’exemple inclut une application HTML5 basée sur TypeScript, React et webpack, ainsi qu’un exemple PowerShell.

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
