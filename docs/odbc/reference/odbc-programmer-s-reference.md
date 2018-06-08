---
title: Programmeur ODBC&#39;s référence | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC [ODBC], reference
ms.assetid: b33c3c43-ae66-44a3-be17-9cd82624dd96
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6b4ac57816359fe1b69fb46d5d60967804cde5d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-programmer39s-reference"></a>Programmeur ODBC&#39;s référence
Le *de référence du programmeur ODBC* contient les sections suivantes.  
  
-   [Nouveautés dans ODBC 3.8](../../odbc/reference/what-s-new-in-odbc-3-8.md) répertorie les nouvelles fonctionnalités ODBC qui ont été ajoutées dans le SDK Windows 8.  
  
-   [Exemple de programme ODBC](../../odbc/reference/sample-odbc-program.md) présente un exemple de programme ODBC.  
  
-   [Présentation d’ODBC](../../odbc/reference/introduction-to-odbc.md) fournit un bref historique de Structured Query Language ODBC et des informations conceptuelles sur l’interface ODBC.  
  
-   [Développement d’Applications](../../odbc/reference/develop-app/developing-applications.md) contient des informations sur le développement d’applications qui utilisent l’interface ODBC et les pilotes qui l’implémentent.  
  
-   [Lors de l’installation et configuration du logiciel ODBC](../../odbc/reference/install/installing-and-configuring-the-odbc-software.md) fournit des informations sur l’installation et une référence de fonction DLL le programme d’installation.  
  
-   [Développement d’un pilote ODBC](../../odbc/reference/develop-driver/developing-an-odbc-driver.md) contient des informations sur l’écriture d’un pilote.  
  
-   [Référence de l’API](../../odbc/reference/syntax/odbc-reference.md) contient la syntaxe et les informations de sémantique pour toutes les fonctions ODBC.  
  
-   [Les annexes ODBC](../../odbc/reference/appendixes/odbc-appendixes.md) contiennent des détails techniques et référencer des tables pour les codes d’erreur ODBC, les types de données et grammaire SQL.  
  
## <a name="working-with-the-odbc-documentation"></a>Utilisation de la Documentation ODBC  
 L’interface ODBC est conçue pour une utilisation avec le langage de programmation C. Utilisation de l’interface ODBC s’étend sur les trois zones : les instructions SQL, ODBC appels de fonction et programmation en C. Cette documentation suppose ce qui suit :  
  
-   Une connaissance pratique du langage de programmation C.  
  
-   Base de connaissances générales DBMS et vous familiariser avec SQL.  
  
 Les conventions typographiques suivantes sont utilisées.  
  
|Format|Utilisé pour|  
|------------|--------------|  
|SÉLECTIONNEZ * À PARTIR DE|Lettres majuscules indiquent des instructions SQL, les noms de macros et termes utilisés au niveau de la commande du système d’exploitation.|  
|`RETCODE SQLFetch(hdbc)`|La police à espacement fixe est utilisée pour les exemples de lignes de commande et du code de programme.|  
|*argument*|Les mots en italique indiquent des arguments par programmation, les informations que l’utilisateur ou l’application doit fournir ou word accentuation.|  
|**SQLEndTran**|Gras indique que la syntaxe doit être tapée exactement comme indiqué, y compris les noms de fonction.|  
|&#124;|Une barre verticale sépare les deux options s’excluent mutuellement dans une ligne de syntaxe.|  
|...|Points de suspension indique que les arguments peuvent être répétés plusieurs fois.|  
|. . .|Une colonne de trois points indique que la continuation de lignes de code précédentes.|  
  
## <a name="about-the-code-examples"></a>Sur les exemples de Code  
 Les exemples de code dans ce guide sont destinées uniquement à titre d’illustration. Car elles sont écrites principalement à illustrer les principes d’ODBC, l’efficacité a parfois été définie par souci de clarté. En outre, les sections entières du code ont parfois été omises par souci de clarté. Notamment les définitions de fonctions de non-ODBC (fonctions dont les noms ne commencent pas par « SQL ») et la plupart de gestion des erreurs.  
  
 Tous les exemples de code utilisent des chaînes ANSI et le même schéma de base de données, ce qui est présenté au début de [fonctions de catalogue](../../odbc/reference/develop-app/catalog-functions.md).  
  
## <a name="recommended-reading"></a>Lecture recommandée  
 Pour plus d’informations sur SQL, les normes suivantes sont disponibles :  
  
-   Langue de base de données, SQL d’avec l’intégrité des améliorations, ANSI, 1989 ANSI X3.135-1989.  
  
-   Langue de la base de données : SQL : X3H2 de ANSI et ISO/IEC JTC1/SC21/WG3 9075 : 1992 (SQL-92).  
  
-   Ouvrir le groupe, la gestion des données : Structured Query Language (SQL), Version 2 (The Open Group, 1996).  
  
 En plus des normes et des repères de SQL spécifiques au fournisseur, la documentation de nombreux décrire SQL, y compris :  
  
-   Date, J. C., à avec Darwen, Hugh : *un Guide sur la norme SQL* (Addison-Wesley, 1993).  
  
-   Emerson, Sandra l, Darnovsky, Elodie et Bowman, Judith s : *le Guide pratique SQL* (Addison-Wesley, 1989).  
  
-   Groff, James r et Weinberg, Paul n : *à l’aide de SQL* (Osborne McGraw-Hill, 1990).  
  
-   Gruber, Martin : *présentation SQL* (Sybex, 1990).  
  
-   Hursch, prise l et Caroline j : *SQL, le langage de requête structuré* (onglet documentation, 1988).  
  
-   Melton, Jim et Simon, Alan r : *comprendre le nouveau texte SQL : un Guide complet* (Morgan Kaufmann Publishers, 1993).  
  
-   Pascal, Fabian avoir : *SQL et les principes de base relationnelles* (M & T Books, 1990).  
  
-   Trimble, J. Harvey, Jr. et Chappell, David : *une présentation Visual SQL* (Wiley, 1989).  
  
-   Les réseaux locaux van der, Rick F. : *Introduction à SQL* (Addison-Wesley, 1988).  
  
-   Vang, Soren : *SQL et les bases de données relationnelles* (documentation Microtrend, 1990).  
  
-   Viescas, John : *Guide de référence rapide SQL* (Microsoft Corp., 1989).  
  
 Pour plus d’informations sur le traitement des transactions, consultez :  
  
-   Gris, N. de J. et Reuter, Andreas : *le traitement transactionnel : Concepts et Techniques* (Morgan Kaufmann Publishers, 1993).  
  
-   Hackathorn, Richard d : *connectivité de base de données d’entreprise* (Wiley & Sons, 1993).  
  
 Pour plus d’informations sur les Interfaces de niveau d’appel, les normes suivantes sont disponibles :  
  
-   Ouvrir le groupe, *gestion des données : SQL niveau Interface d’appel (CLI), C451* (Open Group, 1995).  
  
-   ISO/IEC 9075-3:1995, Interface de niveau d’appel (SQL/CLI).  
  
 Pour plus d’informations à propos d’ODBC, un nombre de la documentation sont disponible, notamment :  
  
-   GEIGER, Kyle : *dans ODBC* (Microsoft Press®, 1995).  
  
-   Gryphon, Robert, Charpentier, Luc, Oelschlager, Jon, cordonnier orthopédiste, Andrew, croix, Jim et Lilley, Albert w : *à l’aide de ODBC 2* (File, 1994).  
  
-   Johnston, Tom et Osborne, marque : *Guide du développeur ODBC* (Howard W. Sams & société, 1994).  
  
-   Nord, Ken : *Windows Multi-SGBD programmation : à l’aide de C++, Visual Basic, ODBC, OLE 2 et outils pour les projets de SGBD* (John Wiley & Sons, Inc., 1995).  
  
-   Stegman, Michael o, Signore, Robert et Creamer, John : *la Solution ODBC, de la connectivité de base de données ouverts dans des environnements distribués* (McGraw-Hill, 1995).  
  
-   Welch, Keith : *à l’aide de ODBC 2* (File, 1994).  
  
-   Merlan, facture : *rement ODBC dans les 21 jours* (Howard W. Sams & société, 1994).
