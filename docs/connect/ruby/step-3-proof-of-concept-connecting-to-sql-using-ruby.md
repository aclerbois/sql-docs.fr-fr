---
title: 'Étape 3 : Preuve de concept pour la connexion à SQL à l’aide de Ruby | Documents Microsoft'
ms.custom: ''
ms.date: 08/08/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: ruby
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: cac20b18-0a6d-4243-bbda-a5d1b9476441
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a5bf98585281b4b84869c385c848d9add459d57b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-ruby"></a>Étape 3 : Preuve de concept pour la connexion à SQL à l’aide de Ruby

Cet exemple doit être considéré comme une preuve de concept uniquement.  L’exemple de code est simplifié par souci de clarté et ne représente pas nécessairement les meilleures pratiques recommandées par Microsoft.  
  
## <a name="step-1--connect"></a>Étape 1 : se connecter  
  
Le [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) fonction est utilisée pour se connecter à la base de données SQL.  
  
``` ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
```  
  
## <a name="step-2--execute-a-query"></a>Étape 2 : Exécuter une requête  
  
Copiez et collez le code suivant dans un fichier vide. Appeler test.rb. Puis, exécutez-le en entrant la commande suivante à partir de l’invite de commandes :  
  
    ruby test.rb  
  
Dans l’exemple de code, la [TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds) fonction est utilisée pour récupérer un jeu de résultats d’une requête par rapport à la base de données SQL. Cette fonction accepte une requête et retourne un jeu de résultats. Le jeu de résultats est itéré en utilisant [result.each faire | ligne |](https://github.com/rails-sqlserver/tiny_tds).  
  
``` ruby 
    require 'tiny_tds'    
    print 'test'       
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC")  
    results.each do |row|  
    puts row  
    end  
```  
  
## <a name="step-3--insert-a-row"></a>Étape 3 : Insérer une ligne  
  
Dans cet exemple, vous allez apprendre à passer en toute sécurité une commande [INSERT](../../t-sql/statements/insert-transact-sql.md), en passant des valeurs en paramètres qui protègeront votre application à partir des tentatives [d'injection SQL](../../relational-databases/tables/primary-and-foreign-key-constraints.md).    
  
Pour utiliser TinyTDS avec Azure, il est recommandé d’exécuter plusieurs `SET` instructions pour modifier la façon dont la session en cours gère des informations spécifiques. Recommandé `SET` instructions sont fournies dans l’exemple de code. Par exemple, `SET ANSI_NULL_DFLT_ON` autorisera les nouvelles colonnes créées pour autoriser les valeurs null même si l’état de la possibilité de valeur null de la colonne n’est pas explicitement défini.  
  
Pour les aligner avec Microsoft SQL Server [datetime](http://msdn.microsoft.com/library/ms187819.aspx) mettre en forme, utilisez la [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) fonction effectuer un cast vers le format de date/heure correspondant.  
  
``` ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SET ANSI_NULLS ON")  
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")  
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")  
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")  
    results = client.execute("SET ANSI_PADDING ON")  
    results = client.execute("SET QUOTED_IDENTIFIER ON")  
    results = client.execute("SET ANSI_WARNINGS ON")  
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")  
    require 'date'  
    t = Time.now  
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")  
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)  
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")  
    results.each do |row|  
    puts row  
    end  
```
