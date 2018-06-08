---
title: Notes de publication pour 2017 de SQL Server sur Linux | Documents Microsoft
description: Cette rubrique contient les notes de publication et les fonctionnalités prises en charge pour SQL Server 2017 fonctionnant sous Linux. Les notes de publication sont incluses dans la version la plus récente et plusieurs versions précédentes.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 05/24/2018
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: 1314744f-fcaf-46db-800e-2918fa7e1b6c
ms.openlocfilehash: 07782a5b8290b41a5a11557c503fcbfd0736790b
ms.sourcegitcommit: a9da0abd3e17fbcd6339980d7331d0418cdada53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
---
# <a name="release-notes-for-sql-server-2017-on-linux"></a>Notes de publication pour 2017 de SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Les notes suivantes s’appliquent à SQL Server 2017 est en cours d’exécution sur Linux. Cet article est divisé en sections pour chaque version. La version GA a détaillées prise en charge et connu des problèmes répertoriés. Chaque version de mise à jour Cumulative (CU) a un lien vers un article décrivant les modifications CU ainsi que des liens vers le package télécharge de Linux.

## <a name="supported-platforms"></a>Plateformes prises en charge

| Plateforme | Système de fichiers | Guide d'installation |
|-----|-----|-----|
| Red Hat Enterprise Linux 7.3 ou 7.4 Workstation, Server, et Desktop | XFS ou EXT4 | [Guide d’installation](quickstart-install-connect-red-hat.md) | 
| Serveur Linux SUSE Enterprise SP2 v12 | XFS ou EXT4 | [Guide d’installation](quickstart-install-connect-suse.md) |
| Ubuntu 16.04LTS | XFS ou EXT4 | [Guide d’installation](quickstart-install-connect-ubuntu.md) | 
| Moteur docker 1.8 + sur Windows, Mac ou Linux | Néant | [Guide d’installation](quickstart-install-connect-docker.md) | 

> [!TIP]
> Pour plus d’informations, consultez la [requise](sql-server-linux-setup.md#system) pour SQL Server sur Linux. Pour la dernière stratégie de prise en charge pour SQL Server 2017, consultez le [politique de support technique pour Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

## <a name="tools"></a>Outils

La plupart des outils clients existants qui ciblent SQL Server peuvent en toute transparence cible SQL Server en cours d’exécution sur Linux. Certains outils peuvent avoir une spécification de version spécifique pour fonctionner avec Linux. Pour obtenir une liste complète des outils SQL Server, consultez [SQL outils et utilitaires pour SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Historique des versions

Le tableau suivant répertorie l’historique de publication pour SQL Server 2017.

| Version | Version | Date de publication |
|-----|-----|-----|
| [CU7](#CU7) | 14.0.3026.27 | 5-2018 |
| [CU6](#CU6) | 14.0.3025.34 | 4-2018 |
| [CU5](#CU5) | 14.0.3023.8 | 3-2018 |
| [CU4](#CU4) | 14.0.3022.28 | 2-2018 |
| [CU3](#CU3) | 14.0.3015.40 | 1-2018 |
| [CU2](#CU2) | 14.0.3008.27 | 11-2017 |
| [CU1](#CU1) | 14.0.3006.16 | 10-2017 |
| [DISPONIBILITÉ GÉNÉRALE](#GA) | 14.0.1000.169 | 10-2017 |

## <a id="cuinstall"></a> Comment installer les mises à jour cumulatives

Si vous avez configuré le référentiel de mise à jour cumulative, vous obtiendrez la mise à jour cumulative la plus récente de packages SQL Server lorsque vous effectuerez de nouvelles installations. Le référentiel de mise à jour cumulative est la valeur par défaut pour tous les articles concernant l’installation de package pour SQL Server sur Linux. Pour plus d’informations sur la configuration du référentiel, consultez [Source référentiels](sql-server-linux-change-repo.md).

Si vous mettez à jour des packages SQL Server existants, exécutez la commande de mise à jour appropriée pour chaque package obtenir la dernière mise à jour cumulative. Pour obtenir des instructions de mise à jour spécifique pour chaque package, consultez les guides d’installation suivantes :

- [Installer le package SQL Server](sql-server-linux-setup.md#upgrade)
- [Installer le package de la recherche en texte intégral](sql-server-linux-setup-full-text-search.md)
- [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Activer l’Agent SQL Server](sql-server-linux-setup-sql-agent.md)

## <a id="CU7"></a> CU7 (mai 2018)

Il s’agit de la version 7 de mise à jour Cumulative (CU7) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3026.27. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/en-us/help/4229789 ](https://support.microsoft.com/en-us/help/4229789).

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3026.27-2 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3026.27-2 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3026.27-2.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3026.27-2.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3026.27-2.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3026.27-2 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3026.27-2_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3026.27-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3026.27-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="CU6"></a> CU6 (avril 2018)

Il s’agit de la version 6 de mise à jour Cumulative (CU6) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3025.34. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/help/4101464 ](https://support.microsoft.com/help/4101464).

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3025.34-3 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3025.34-3 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3025.34-3.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3025.34-3.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3025.34-3 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3025.34-3_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3025.34-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3025.34-3_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="CU5"></a> CU5 (mars 2018)

Il s’agit de la version 5 de mise à jour Cumulative (CU5) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3023.8. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/help/4092643 ](https://support.microsoft.com/help/4092643).

### <a name="known-upgrade-issue"></a>Problème de mise à niveau connu

Lorsque vous mettez à niveau depuis une version précédente à CU5, SQL Server peut échouer démarrer avec l’erreur suivante :

```
Error: 4860, Severity: 16, State: 1.
Cannot bulk load. The file "C:\Install\SqlTraceCollect.dtsx" does not exist or you don't have file access rights.
Error: 912, Severity: 21, State: 2.
Script level upgrade for database 'master' failed because upgrade step 'msdb110_upgrade.sql' encountered error 200, state
```

Pour résoudre cette erreur, activez l’Agent SQL Server et redémarrez SQL Server avec les commandes suivantes :

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
sudo systemctl start mssql-server
```

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3023.8-5 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3023.8-5 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3023.8-5.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3023.8-5.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3023.8-5.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3023.8-5 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3023.8-5_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3023.8-5_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3023.8-5_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="CU4"></a> CU4 (février 2018)

Il s’agit de la version 4 de mise à jour Cumulative (CU4) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3022.28. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/en-us/help/4056498 ](https://support.microsoft.com/en-us/help/4056498).

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

> [!NOTE]
> À compter de CU4, l’Agent SQL Server n’est plus installé comme un lot distinct. Il est installé avec le package de moteur et doit être activé pour utiliser.

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3022.28-2 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3022.28-2 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3022.28-2.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3022.28-2.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3022.28-2.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3022.28-2 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3022.28-2_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3022.28-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3022.28-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="CU3"></a> CU3 (janvier 2018)

Il s’agit de la version à jour Cumulative 3 (CU3) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3015.40. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/en-us/help/4052987 ](https://support.microsoft.com/en-us/help/4052987).

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3015.40-1 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3015.40-1 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3015.40-1.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3015.40-1.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3015.40-1.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3015.40-1.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3015.40-1 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3015.40-1_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3015.40-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3015.40-1_amd64.deb)</br>[Package Debian de SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3015.40-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="CU2"></a> CU2 (2017 novembre)

Il s’agit de la version de Cumulative Update 2 (CU2) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3008.27. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/help/4052574 ](https://support.microsoft.com/help/4052574).

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3008.27-1 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3008.27-1 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3008.27-1.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3008.27-1.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3008.27-1.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3008.27-1.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3008.27-1 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3008.27-1_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3008.27-1_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3008.27-1_amd64.deb)</br>[Package Debian de SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3008.27-1_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="CU1"></a> CU1 (octobre 2017)

Il s’agit de la version à jour Cumulative 1 (CU1) de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.3006.16. Pour plus d’informations sur les correctifs et les améliorations apportées dans cette version, consultez [ https://support.microsoft.com/help/KB4053439 ](https://support.microsoft.com/help/4038634).

### <a name="package-details"></a>Détails du package

Pour les installations de package manuelle ou hors connexion, vous pouvez télécharger les packages RPM et Debian avec les informations contenues dans le tableau suivant :

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.3006.16-3 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.3006.16-3 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.3006.16-3.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.3006.16-3.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.3006.16-3.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.3006.16-3.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.3006.16-3 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.3006.16-3_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.3006.16-3_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.3006.16-3_amd64.deb)</br>[Package Debian de SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.3006.16-3_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a id="GA"></a> GA (octobre 2017)

Il s’agit de la version de disponibilité générale de SQL Server 2017. La version du moteur SQL Server pour cette version est 14.0.1000.169.

### <a name="package-details"></a>Détails du package

Détails du package et les emplacements de téléchargement pour les packages RPM et Debian sont répertoriés dans le tableau suivant. Notez que vous n’avez pas besoin de télécharger ces packages directement si vous utilisez les étapes dans les guides d’installation suivantes :

- [Installer le package SQL Server](sql-server-linux-setup.md)
- [Installer le package de la recherche en texte intégral](sql-server-linux-setup-full-text-search.md)
- [Installer le package de l’Agent SQL Server](sql-server-linux-setup-sql-agent.md)
- [Installer SQL Server Integration Services](sql-server-linux-setup-ssis.md)

| Package | version du package | Téléchargements |
|-----|-----|-----|
| Rpm de Red Hat | 14.0.1000.169-2 | [Moteur le package RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm)</br>[Package SSIS](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-is-14.0.1000.169-1.x86_64.rpm) | 
| Package RPM de SLES | 14.0.1000.169-2 | [package RPM du moteur de serveur MSSQL](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-14.0.1000.169-2.x86_64.rpm)</br>[Rpm de disponibilité élevée](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-ha-14.0.1000.169-2.x86_64.rpm)</br>[Rpm de recherche en texte intégral](https://packages.microsoft.com/sles/12/mssql-server-2017/mssql-server-fts-14.0.1000.169-2.x86_64.rpm)</br>[Package de SQL Server Agent RPM](https://packages.microsoft.com/rhel/7/mssql-server-2017/mssql-server-agent-14.0.1000.169-2.x86_64.rpm) | 
| Package de Debian Ubuntu 16.04 | 14.0.1000.169-2 | [Package Debian moteur](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/mssql-server_14.0.1000.169-2_amd64.deb)</br>[Package de Debian haute disponibilité](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-ha/mssql-server-ha_14.0.1000.169-2_amd64.deb)</br>[Package Debian de recherche en texte intégral](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-fts/mssql-server-fts_14.0.1000.169-2_amd64.deb)</br>[Package Debian de SQL Server Agent](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-agent/mssql-server-agent_14.0.1000.169-2_amd64.deb)<br/>[Package SSIS](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server-is/mssql-server-is_14.0.1000.169-1_amd64.deb) |

## <a name="Unsupported"></a> Fonctionnalités prises en charge et services

Les fonctionnalités et les services suivants ne sont pas disponibles sur Linux au moment de la version GA. La prise en charge de ces fonctionnalités est activée de plus en plus au fil du temps.

| Domaine | Fonctionnalité non prise en charge ou un service |
|-----|-----|
| **Moteur de base de données** | Réplication transactionnelle |
| &nbsp; | Réplication de fusion |
| &nbsp; | Étendre la base de données |
| &nbsp; | PolyBase |
| &nbsp; | Requête distribuée avec des connexions 3ème partie |
| &nbsp; | Procédures système stockées étendues (XP_CMDSHELL, etc.). |
| &nbsp; | Filetable, FILESTREAM |
| &nbsp; | Définir des assemblys CLR avec l’autorisation EXTERNAL_ACCESS ou UNSAFE |
| &nbsp; | Extension du pool de mémoires tampons |
| **SQL Server Agent** |  Sous-systèmes : CmdExec, PowerShell, lecteur de file d’attente, SSIS, SSAS, SSRS |
| &nbsp; | Alertes |
| &nbsp; | l'Agent de lecture du journal ; |
| &nbsp; | Capture de données modifiées |
| &nbsp; | Sauvegarde managée |
| **Haute disponibilité** | Mise en miroir de bases de données  |
| **Sécurité** | Gestion de clés extensible |
| &nbsp; | Authentification d’Active Directory pour les serveurs liés | 
| &nbsp; | Authentification d’Active Directory pour les groupes de disponibilité (groupes de disponibilité) | 
| &nbsp; | outils tiers AD 3e (Centrify, Vintela, Powerbroker) | 
| **Services** | SQL Server Browser |
| &nbsp; | SQL Server R services |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |
| &nbsp; | Coordinateur de transactions distribuées (DTC) |

## <a name="known-issues"></a>Problèmes connus

Les sections suivantes décrivent les problèmes connus avec la version de la disponibilité générale de SQL Server 2017 sur Linux.

#### <a name="general"></a>Général

- Mises à niveau vers la version GA de SQL Server 2017 sont pris en charge uniquement à partir de CTP 2.1 ou version ultérieure. 

- Le nom d’hôte de l'ordinateur sur lequel SQL Server est installé doit avoir 15 caractères ou moins. 

    - **Résolution**: Remplacez le nom d’hôte dans /etc/hostname avec un nom de 15 caractères ou moins.

- Définition manuelle de l’heure système vers l’arrière dans le temps entraîne SQL Server arrêter la mise à jour de l’heure système interne dans SQL Server.

    - **Résolution**: redémarrez SQL Server.

- Seules les installations d’instance unique sont prises en charge.

    - **Résolution**: Si vous souhaitez disposer de plusieurs instances sur un hôte donné, envisagez d’utiliser des machines virtuelles ou des conteneurs Docker. 

- Le gestionnaire de Configuration SQL Server ne peut pas se connecter à SQL Server sur Linux.

- La langue par défaut de la **sa** connexion est l’anglaise.

    - **Résolution**: modifier la langue de la connexion **sa** avec l'instruction **ALTER LOGIN**.

#### <a name="databases"></a>Bases de données

- Impossible de déplacer la base de données master avec l’utilitaire mssql-conf. Les autres bases de données système peuvent être déplacées avec mssql-conf.

- Lorsque vous restaurez une base de données qui a été sauvegardée sur SQL Server sur Windows, vous devez utiliser la clause **WITH MOVE** dans l’instruction Transact-SQL.

- Nécessiter que le service Microsoft Distributed Transaction Coordinator les transactions distribuées ne sont pas pris en charge sur SQL Server est en cours d’exécution sur Linux. SQL Server vers SQL Server, les serveurs liés sont pris en charge, sauf si elles impliquent le DTC. Pour plus d’informations, consultez [nécessiter que le service Microsoft Distributed Transaction Coordinator les transactions distribuées ne sont pas pris en charge sur SQL Server est en cours d’exécution sur Linux](https://blogs.msdn.microsoft.com/bobsql/2017/12/11/sql-server-linux-distributed-transactions-requiring-the-microsoft-distributed-transaction-coordinator-service-are-not-supported-on-sql-server-running-on-linux-sql-server-to-sql-server-distributed-tr/).

- Certains algorithmes (suites de chiffrement) et de sécurité TLS (Transport Layer) ne fonctionnent pas correctement avec SQL Server sur Linux. Cela entraîne des échecs de connexion lorsque vous tentez de vous connecter à SQL Server, ainsi que des problèmes pour établir des connexions entre les réplicas des groupes de disponibilité de.

   - **Résolution**: modifier le **mssql.conf** un script de configuration pour SQL Server sur Linux pour désactiver les suites de chiffrement problématique, en procédant comme suit :

      1. Ajoutez le code suivant /var/opt/mssql/mssql.conf.

      ```
      [network]
      tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
      ```

         >[!NOTE]
         >In the preceding code, `!` negates the expression. This tells OpenSSL to not use the following cipher suite.  

      1. Redémarrez SQL Server avec la commande suivante.

      ```bash
      sudo systemctl restart mssql-server
      ```

- Bases de données SQL Server 2014 sur Windows qui utilisent OLTP en mémoire ne peut pas être restaurées sur 2017 du serveur SQL sur Linux. Pour restaurer une base de données SQL Server 2014 qui utilise l’OLTP en mémoire, tout d’abord mettre à niveau les bases de données vers SQL Server 2016 ou 2017 du serveur SQL sur Windows avant de les déplacer vers SQL Server sur Linux via la sauvegarde/restauration ou de détachement et d’attachement.

- Autorisation de l’utilisateur **ADMINISTER BULK OPERATIONS** n’est pas pris en charge sous Linux pour l’instant.

#### <a name="networking"></a>Réseau

Fonctionnalités qui impliquent des connexions TCP sortantes à partir du processus sqlservr, tels que des serveurs liés ou des groupes de disponibilité peut ne pas fonctionnent si les deux conditions suivantes sont remplies :

1. Le serveur cible est spécifié comme un nom d’hôte et non une adresse IP.

1. L’instance source a IPv6 désactivée dans le noyau. Pour vérifier si votre système a IPv6 activée dans le noyau, les tests suivants doivent être validés :

   - `cat /proc/cmdline` Imprime la ligne de commande de démarrage du noyau actuel. La sortie ne doit pas contenir `ipv6.disable=1`.
   - Le répertoire /proc/sys/net/ipv6/ doit exister.
   - Un programme C qui appelle `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)` doit fonctionner - syscall doit retourner un fd ! = -1 et n’échoue pas avec EAFNOSUPPORT.

L’erreur exacte dépend de la fonctionnalité Pour les serveurs liés, cela se manifeste en tant qu’une erreur de délai d’attente de connexion. Pour les groupes de disponibilité, le DDL `ALTER AVAILABILITY GROUP JOIN` sur le serveur secondaire échoue après 5 minutes avec une erreur de délai d’attente de téléchargement de la configuration.

Pour contourner ce problème, effectuez l’une des opérations suivantes :

1. Utiliser des adresses IP au lieu des noms d’hôtes pour spécifier la cible de la connexion TCP.

1. Activation du protocole IPv6 dans le noyau en supprimant `ipv6.disable=1` à partir de la ligne de commande de démarrage. La façon de procéder dépend de la distribution de Linux et le chargeur de démarrage, tels que grub. Si vous ne souhaitez pas IPv6 doit être désactivée, vous pouvez le désactiver en définissant `net.ipv6.conf.all.disable_ipv6 = 1` dans les `sysctl` configuration (par exemple, `/etc/sysctl.conf`). Cela sera toujours empêcher l’obtention d’une adresse IPv6 de carte de réseau du système, mais vous pouvez autoriser les fonctionnement de fonctionnalités sqlservr.

#### <a name="network-file-system-nfs"></a>Network File System (NFS)
Si vous utilisez les partages distants **Network File System (NFS)** en production, notez les exigences de prise en charge suivantes :

- Utiliser la version NFS **4.2 ou ultérieure**. Les versions antérieures de NFS ne gèrent pas les fonctionnalités requises, telles que fallocate et la création de fichier sparse, courantes avec les systèmes de fichiers modernes.
- Positionnez uniquement les répertoires **/var/opt/mssql** sur le montage NFS. Les autres fichiers, tels que les fichiers binaires du système SQL Server, ne sont pas pris en charge.
- Assurez-vous que les clients NFS utilisent l’option 'nolock' lorsque qu'ils montent le partage distant.

#### <a name="localization"></a>Localisation

- Si vos paramètres régionaux ne sont pas anglais (fr_FR) lors de l’installation, vous devez utiliser l’encodage UTF-8 dans votre session d’interpréteur de commandes/terminal. Si vous utilisez l’encodage ASCII, vous pouvez voir une erreur semblable au suivant :

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Si vous ne pouvez pas utiliser le codage UTF-8, exécutez le programme d’installation à l’aide de la variable d’environnement MSSQL_LCID pour spécifier votre choix de la langue.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- Lorsque mssql-conf le programme d’installation en cours d’exécution et effectuez une installation non anglaise de SQL Server, incorrect des caractères étendus sont affichés après le texte localisé, « Configuration de SQL Server... ». Ou, pour les installations de base non Latin, la phrase peut être manquante complètement. La phrase manquante doit afficher la chaîne localisée suivante : « le PID de licence a été traité correctement.  La nouvelle édition est [\<nom\> édition] ». Cette chaîne est de sortie à titre d’information uniquement et prochaine mise à jour Cumulative SQL Server résoudre ce problème pour toutes les langues. Cela n’affecte pas la réussite de l’installation de SQL Server en aucune façon. 

#### <a name="full-text-search"></a>Recherche en texte intégral

- Pas de tous les filtres sont disponibles avec cette version, notamment les filtres pour les documents Office. Pour obtenir la liste de filtres pris en charge, consultez [installer de recherche de texte intégral SQL Server sur Linux](sql-server-linux-setup-full-text-search.md#filters).

#### <a id="ssis"></a> SQL Server Integration Services (SSIS)

- Le **mssql server est** package n’est pas pris en charge sur SUSE dans cette version. Il est actuellement pris en charge sur Ubuntu sur Red Hat Enterprise Linux (RHEL).

- SSIS lors de l’actualisation de Linux CTP 2.1 et versions ultérieures, les packages SSIS permet les connexions ODBC sur Linux. Cette fonctionnalité a été testée avec les pilotes ODBC MySQL sur le serveur SQL Server, mais il est également prévue pour fonctionner avec n’importe quel pilote ODBC Unicode qui respecte la spécification ODBC. Au moment du design, vous pouvez fournir une source de données ou une chaîne de connexion pour se connecter aux données ODBC ; Vous pouvez également utiliser l’authentification Windows. Pour plus d’informations, consultez la [blog de l’annonce prise en charge d’ODBC sur Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

- Les fonctionnalités suivantes ne sont pas pris en charge dans cette version lorsque vous exécutez des packages SSIS sur Linux :
  - Base de données catalogue de SSIS
  - Exécution de package planifié par l’Agent SQL
  - Authentification Windows
  - Les composants tiers
  - Capture de données modifiées (CDC)
  - Integration Services (SSIS) Scale Out
  - Azure Feature Pack pour SSIS
  - Prise en charge de Hadoop et HDFS
  - Microsoft Connector pour SAP BW

Pour obtenir la liste de composants SSIS intégrés qui sont actuellement pas en charge, ou qui sont pris en charge avec les limitations, consultez [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md#components).

Pour plus d’informations sur SSIS sur Linux, consultez les articles suivants :
-   [Annonce de billet de blog SSIS prise en charge de Linux](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/).
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)

#### <a name="-a-idssmsa-sql-server-management-studio-ssms"></a>< un id = « ssms » ></a> SQL Server Management Studio (SSMS)

Les limitations suivantes s’appliquent à SSMS sur Windows connectés à SQL Server sur Linux.

- Plans de maintenance ne sont pas pris en charge.

- L’entrepôt de données de gestion (MDW) et la collecte de données (Data Collector) dans SSMS ne sont pas pris en charge. 

- Les composants de SSMS qui utilisent l’authentification Windows ou les options du journal des événements Windows ne fonctionnent pas avec Linux. Vous pouvez toujours utiliser ces fonctionnalités avec d’autres options, telles que des connexions SQL. 

- Impossible de modifier le nombre de fichiers journaux à conserver.

## <a name="next-steps"></a>Étapes suivantes

Pour commencer, consultez les Démarrages rapides suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-ubuntu.md)
- [Approvisionner une machine virtuelle SQL dans Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=%2fsql%2flinux%2ftoc.json)
- [Exécuter et connecter - Cloud](quickstart-install-connect-clouds.md)

Pour obtenir des réponses aux questions fréquemment posées, consultez le [SQL Server sur le Forum aux questions sur Linux](sql-server-linux-faq.md).
