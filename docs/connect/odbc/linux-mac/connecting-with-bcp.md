---
title: Connexion avec bcp | Documents Microsoft
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
- bcp
ms.assetid: 3eca5717-e50f-40db-be16-a1cebbdfee70
caps.latest.revision: 33
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 707db709188db15bc3627d65a2dba5a2bc516308
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-with-bcp"></a>Connexion avec bcp
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Le [bcp](http://go.microsoft.com/fwlink/?LinkID=190626) utilitaire est disponible dans le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux et macOS. Cette page décrit les différences de la version Windows de `bcp`.
  
- La marque de fin de champ est une tabulation (« \t  »).  
  
- La marque de fin de ligne est un saut de ligne (« \n »).  
  
- Le mode caractère est le format par défaut pour `bcp` les fichiers de données qui ne contiennent pas de caractères étendus et les fichiers de format.  
  
> [!NOTE]  
> Une barre oblique inverse '\\' sur un argument de ligne de commande doit être entre guillemets ou séquence d’échappement. Par exemple, pour spécifier un saut de ligne comme délimiteur de ligne personnalisée, vous devez utiliser un des mécanismes suivants :  
>   
> -   r -\\\n  
> -   -r"\n"  
> -   -r'\n'  
  
Voici un exemple d’appel de commande de `bcp` pour copier les lignes de la table dans un fichier texte :  
  
```  
bcp AdventureWorks2008R2.Person.Address out test.dat -Usa -Pxxxx -Sxxx.xxx.xxx.xxx  
```  
  
## <a name="available-options"></a>Options disponibles
Dans la version actuelle, la syntaxe et les options suivantes sont disponibles :  

[*base de données ***.**]* schéma ***.*** table * **dans** *data_file* | **hors** *data_file*

- -a *packet_size*  
Spécifie le nombre d’octets, par paquet réseau, envoyés depuis/vers le serveur.  
  
- -b *batch_size*  
Nombre de lignes par lot de données importées.  
  
- -c  
Utilise un type de données caractères.  
  
- -d *database_name*  
Spécifie la base de données à laquelle se connecter.  
  
- -d  
Provoque la valeur passée à la `bcp` option -S doit être interprété comme un nom de source de données (DSN). Pour plus d’informations, consultez « Prise en charge de source de données dans sqlcmd et bcp » dans [connexion avec sqlcmd](../../../connect/odbc/linux-mac/connecting-with-sqlcmd.md).  
  
- e - *error_file* Spécifie le chemin d’accès complet d’un fichier d’erreur utilisé pour stocker les lignes que la `bcp` utilitaire ne peut pas transférer du fichier à la base de données.  
  
- -e  
Utilise une ou plusieurs valeurs d’identité figurant dans le fichier de données importé pour la colonne d’identité.  
  
- -f *format_file*  
Spécifie le chemin complet au fichier de format.  
  
- -F *first_row*  
Spécifie le numéro de la première ligne à exporter à partir d’une table ou à importer à partir d’un fichier de données.  
  
- -k  
Pendant l’opération, les colonnes vides doivent conserver une valeur NULL et les colonnes insérées ne doivent pas prendre de valeur par défaut.  
  
- -l  
Spécifie un délai de connexion. L’option – l spécifie le nombre de secondes au terme duquel une connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] arrive à expiration lorsque vous essayez de vous connecter à un serveur. Le délai de connexion par défaut est de 15 secondes. Le délai de connexion doit être un nombre compris entre 0 et 65534. Si la valeur fournie n'est pas numérique ou n'est pas comprise dans cet intervalle, `bcp` génère un message d'erreur. La valeur 0 indique un délai infini.
  
- -L *last_row*  
Spécifie le numéro de la dernière ligne à exporter à partir d’une table ou à importer à partir d’un fichier de données.  
  
- -m *max_errors*  
Spécifie le nombre maximal d’erreurs de syntaxe pouvant se produire avant le `bcp` opération est annulée.  
  
- -n  
Utilise les types de données (de la base de données) natifs pour effectuer l’opération de copie en bloc.  
  
- -P *password*  
Spécifie le mot de passe de l’ID de connexion.  
  
- -Q  
Exécute l'instruction SET QUOTED_IDENTIFIERS ON dans la connexion entre l'utilitaire `bcp` et une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
- -r *row_terminator*  
Spécifie l’indicateur de fin de ligne.  
  
- -r  
Spécifie que les données de type devise, date et heure sont copiées en bloc dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] en utilisant le format régional défini par les paramètres régionaux de l’ordinateur client.  
  
- -S *server*  
Spécifie le nom de la [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] instance auquel se connecter, ou si -D est utilisé, une source de données.  
  
- -t *field_terminator*  
Spécifie l’indicateur de fin de champ.  
  
- -T  
Spécifie que le `bcp` utilitaire se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] avec une connexion approuvée (sécurité intégrée).  
  
- -U *login_id*  
Spécifie l'ID de connexion utilisé pour une connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
- -v  
Indique le numéro de version et le copyright de l'utilitaire `bcp`.  
  
- -w  
Utilise des caractères Unicode pour effectuer l’opération de copie en bloc.  
  
Dans cette version, les caractères Latin-1 et UTF-16 sont pris en charge.  
  
## <a name="unavailable-options"></a>Options non disponibles
Dans la version actuelle, la syntaxe et les options suivantes ne sont pas disponibles :  

- -c  
Indique la page de codes des données dans le fichier.  
  
- -h *hint*  
Spécifie les indications utilisées pendant l’importation en bloc des données dans une table ou une vue.  
  
- -i *input_file*  
Spécifie le nom d’un fichier réponse.  
  
- -n  
Utilise les types de données (base de données) natifs pour les données non-caractères et les caractères Unicode pour les données caractères.  
  
- -o *output_file*  
Spécifie le nom d’un fichier recevant la sortie redirigée à partir de l’invite de commandes.  
  
- -V (80 | 90 | 100)  
Utilise les types de données à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].  
  
- -X  
Utilisé avec les options format et -f formal_file, génère un fichier au format XML à la place du fichier au format non-XML par défaut.  
  
## <a name="see-also"></a>Voir aussi

[Connexion avec **sqlcmd**](../../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)  
