---
title: Sécurisation des Applications des services Bureau à distance | Documents Microsoft
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
- RDS security [ADO]
ms.assetid: 82fb1330-d6c6-4c17-ad3e-d417ff822b25
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 60cb8cde92d116344a99aea1e55d391906096548
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="securing-rds-applications"></a>Sécurisation des Applications des services Bureau à distance
Cette rubrique fournit des informations de sécurité de RDS.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="microsoft-internet-explorer-security-issues"></a>Problèmes de sécurité de Microsoft Internet Explorer  
 Grâce aux nouvelles améliorations de sécurité ajoutées à Microsoft Internet Explorer, certains objets ADO et des services Bureau à distance sont limitées à l’exécution uniquement dans un environnement en mode « sécurisée ». Cela nécessite que vous prenant en charge de ces problèmes, y compris les différentes zones, les niveaux de sécurité, comportement restrictif, les opérations non sécurisées et les paramètres de sécurité personnalisez.  
  
## <a name="security-and-your-web-server"></a>Sécurité et votre serveur Web  
 Si vous utilisez la [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet sur votre serveur Web Internet, souvenez-vous qu’afin de créer un risque de sécurité potentiel. Les utilisateurs externes qui obtiennent le nom de source de données valide (DSN), ID d’utilisateur et mot de passe pourrait écrire des pages pour envoyer une requête à la source de données. Si vous souhaitez un accès plus limité à une source de données, une option consiste à annuler l’inscription et de supprimer le **RDSServer.DataFactory** (msadcf.dll) de l’objet et à la place utiliser des objets métier personnalisés avec des requêtes codées en dur.  
  
 Pour plus d’informations sur les implications de sécurité de l’utilisation de l’objet RDSServer.DataFactory, consultez le Bulletin de sécurité Microsoft MS99-025 sur le site Web de sécurité de Microsoft.  
  
## <a name="client-impersonation-and-security"></a>Sécurité et l’emprunt d’identité du client  
 Si le **l’authentification du mot de passe** propriété de votre serveur Web IIS est définie pour l’authentification de stimulation/réponse de Windows NT (pour Windows NT 4.0) ou pour l’authentification Windows intégrée (pour Windows 2000), puis sont des objets métier appelé dans le contexte de sécurité du client. Il s’agit d’une nouvelle fonctionnalité de RDS 1.5 qui permet l’emprunt d’identité du Client sur HTTP. Lorsque vous travaillez dans ce mode, l’ouverture de session sur le serveur Web (IIS) n’est pas anonyme mais utilise l’ID utilisateur et un mot de passe que l’ordinateur client est en cours d’exécution sous. Si le DSN ODBC sont configurés pour utiliser une connexion approuvée, puis accès aux bases de données, telles que SQL Server, se produit également sous le contexte de sécurité du client. Mais cela ne fonctionne que si la base de données se trouve sur le même ordinateur que IIS ; les informations d’identification du client ne peut pas être appliquées à un autre ordinateur.  
  
 Par exemple, un client, John Doe, avec l’ID utilisateur = « JeanD » et le mot de passe = « secret » est connecté à un ordinateur client. Il s’exécute une application basée sur navigateur qui doit accéder à la **RDSServer.DataFactory** objet pour créer un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) en exécutant une requête SQL sur l’ordinateur « MonServeur » exécutant IIS. MonServeur, un système exécutant Windows NT Server 4.0, est configuré pour utiliser l’authentification Windows NT Challenge/Response, son DSN ODBC a « Utiliser une connexion approuvée » sélectionné et le serveur contient également la source de données SQL Server. Lorsqu’une demande est reçue sur le serveur Web, il demande au client pour l’ID utilisateur et un mot de passe. Par conséquent, la demande est consignée sur MonServeur comme émanant de « JeanD » / « Secret » au lieu de IUSER_MyServer (est la valeur par défaut lorsque l’authentification de mot de passe anonyme est activé). De même, lors de la connexion à SQL Server, « JeanD » / « Secret » est utilisé.  
  
 Par conséquent, le mode d’authentification IIS Windows NT Challenge/Response permet des pages HTML doit être créé sans que l’utilisateur demande explicitement pour les informations d’ID et mot de passe utilisateur nécessaires pour vous connecter à la base de données. Si l’authentification de base IIS ont été utilisée, alors cela aussi est requis.  
  
## <a name="password-authentication"></a>Authentification du mot de passe  
 Services Bureau à distance peut communiquer avec un serveur Web IIS en cours d’exécution dans l’un des trois modes d’authentification de mot de passe : anonyme, de base, ou authentification stimulation/réponse NT (appelée authentification Windows intégrée dans Windows 2000). Ces paramètres définissent comment de contrôle de l’accès par son intermédiaire, exigeant qu’un ordinateur client ont des privilèges d’accès explicites sur le serveur Web NT d’un serveur Web.


