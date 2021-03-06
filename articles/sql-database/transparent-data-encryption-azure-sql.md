---
title: Transparent Data Encryption per il database SQL di Azure e Data Warehouse | Microsoft Docs
description: Una panoramica di Transparent Data Encryption per il database SQL e Data Warehouse. Il documento illustra i vantaggi e le opzioni per la configurazione, che include crittografia TDE gestita dal servizio e Bring Your Own Key.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 04/19/2019
ms.openlocfilehash: 8ed7d144b886cc29592418007b9103b4aa94e8ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60331078"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Transparent Data Encryption per il database SQL e Data Warehouse

La funzionalità Transparent Data Encryption (TDE) consente di proteggere il database SQL di Azure, Istanza gestita di database SQL di Azure e Azure Data Warehouse dalla minaccia di attività dannose. Esegue in tempo reale la crittografia e la decrittografia del database, dei backup associati e dei file di log delle transazioni inattivi, senza richiedere modifiche dell'applicazione. Per impostazione predefinita, la tecnologia TDE è abilitata per tutti i database SQL di Azure appena distribuiti. Transparent Data Encryption non può essere usata per crittografare il database **master** logico nel database SQL.  Il database **master** contiene gli oggetti necessari a eseguire le operazioni di Transparent Data Encryption nei database utente.

Transparent Data Encryption deve essere attivata manualmente per Istanza gestita di database SQL di Azure, database di versioni precedenti del Database SQL di Azure o Azure SQL Data Warehouse.  

Transparent Data Encryption esegue la crittografia dell'archiviazione di un intero database usando una chiave simmetrica denominata "chiave di crittografia del database". Questa chiave di crittografia del database è protetta dalla protezione TDE. La protezione è un certificato gestito dal servizio (TDE gestita dal servizio) o una chiave asimmetrica archiviata in Azure Key Vault (Bring Your Own Key). È possibile impostare la protezione di Transparent Data Encryption a livello di server per Database SQL di Azure e Data Warehouse e a livello di istanza per Istanza gestita di database SQL di Azure. Salvo diversa indicazione, in questo documento il termine *server* fa riferimento sia al server che all'istanza.

All'avvio del database, la chiave di crittografia del database crittografato viene decrittografata e quindi usata per decrittografare e ricrittografare i file di database nel processo di motore di database di SQL Server. TDE esegue la crittografia e la decrittografia I/O in tempo reale dei dati a livello di pagina. Ogni pagina viene decrittografata quando letta in memoria e quindi crittografata prima di essere scritta su disco. Per una descrizione generale della funzionalità TDE, vedere [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

SQL Server eseguito in una macchina virtuale di Azure può anche usare una chiave asimmetrica di Key Vault. I passaggi della configurazione sono diversi rispetto all'uso di una chiave asimmetrica nel database SQL e in Istanza gestita di database SQL di Azure. Per altre informazioni, vedere [Extensible Key Management tramite Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Transparent Data Encryption gestita dal servizio

In Azure, l'impostazione predefinita per TDE prevede che la chiave di crittografia del database sia protetta da un certificato server integrato. Il certificato server integrato è univoco per ogni server. Se un database è in una relazione di replica geografica, il database primario e la replica geografica secondaria sono protetti dalla chiave primaria del server padre del database. Se due database sono connessi allo stesso server, condividono anche lo stesso certificato predefinito. Microsoft ruota automaticamente questi certificati in conformità con i criteri di sicurezza interni e la chiave radice è protetta da un archivio segreti interno di Microsoft.

Microsoft inoltre sposta e gestisce le chiavi senza problemi in base alle esigenze per la replica geografica e ne esegue il ripristino.

> [!IMPORTANT]
> Tutti i nuovi database SQL vengono crittografati per impostazione predefinita tramite TDE gestita del servizio. I database dell'istanza gestita di database SQL di Azure, i database SQL esistenti prima di maggio 2017 e i database SQL creati tramite il ripristino, la replica geografica e la copia di database non sono crittografati per impostazione predefinita.

## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Transparent Data Encryption gestita dal cliente: Bring Your Own Key

La [crittografia TDE con chiavi gestite dal cliente in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md) consente di crittografare la chiave DEK (Database Encryption Key) con una chiave asimmetrica gestita dal cliente chiamata protezione TDE.  Questo è anche comunemente noto come supporto Bring Your Own Key (BYOK) per Transparent Data Encryption. Nello scenario BYOK, la protezione TDE è archiviata in un'istanza di [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), il sistema di gestione delle chiavi esterne basato sul cloud di Azure, che viene gestita dal cliente ed è di sua proprietà. Può essere la protezione TDE [generate da key vault o trasferita in key vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) da un dispositivo HSM locale on. La chiave DEK della crittografia TDE, archiviata nella pagina di avvio di un database, viene crittografata e decrittografata dalla protezione TDE, che rimane archiviata in modo permanente nell'insieme di credenziali delle chiavi in Azure Key Vault.  Per decrittografare e crittografare la chiave DEK, il database SQL deve possedere le autorizzazioni di accesso all'insieme di credenziali delle chiavi del cliente. Se le autorizzazioni del server SQL per l'insieme di credenziali delle chiavi vengono revocate, un database non è accessibile e tutti i dati rimangono crittografati. Per il database SQL di Azure, la protezione TDE è impostata a livello di server logico SQL e viene ereditata da tutti i database associati al server. Per la [istanza gestita SQL di Azure](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance) (modalità BYOK alla funzionalità in anteprima), la protezione TDE è impostata a livello di istanza e viene ereditata da tutti *crittografato* database in tale istanza. Salvo diversa indicazione, in questo documento il termine *server* fa riferimento sia al server che all'istanza.

Grazie all'integrazione di TDE con Azure Key Vault, gli utenti possono controllare le attività di gestione delle chiavi, tra cui le rotazioni, i backup e le autorizzazioni dell'insieme di credenziali, nonché abilitare il controllo o il reporting per tutte le protezioni TDE usando Azure Key Vault. Questa funzionalità offre la gestione centralizzata delle chiavi, sfrutta il vantaggio dei moduli di protezione hardware (HSM) accuratamente monitorati e consente di separare i compiti di gestione delle chiavi e dei dati per contribuire a rispettare la conformità ai criteri di sicurezza.
Per altre informazioni su transparent data encryption con l'integrazione di Azure Key Vault (supporto Bring Your Own Key) per Database SQL di Azure, istanza gestita di SQL (funzionalità BYOK in anteprima) e Data Warehouse, vedere [Transparent data encryption con chiavi di Azure L'integrazione dell'insieme di credenziali](transparent-data-encryption-byok-azure-sql.md).

Per iniziare a usare la crittografia TDE con integrazione di Azure Key Vault (supporto Bring Your Own Key), vedere la procedura dettagliata [Abilitare Transparent Data Encryption usando la propria chiave di Key Vault con PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Spostare un database protetto con TDE

Non è necessario decrittografare i database per le operazioni all'interno di Azure. Le impostazioni di TDE nel database di origine o nel database primario vengono ereditate in modo trasparente nella destinazione. Le operazioni incluse fanno riferimento a:

- Ripristino geografico
- Ripristino temporizzato self-service
- Ripristino di un database eliminato
- Replica geografica attiva
- Creazione di una copia del database
- Ripristino di un file di backup per Istanza gestita di database SQL di Azure

> [!IMPORTANT]
> L'esecuzione di un backup manuale di sola copia di un database crittografato con tecnologia TDE gestita dal servizio non è consentita in Istanza gestita di database SQL di Azure perché il certificato usato per la crittografia non è accessibile. Usare la funzionalità di ripristino temporizzato per spostare i database di questo tipo in un'altra istanza gestita.

Quando si esporta un database protetto con TDE, il contenuto esportato del database non viene crittografato. Questo contenuto esportato viene archiviato in file BACPAC non crittografati. Assicurarsi di proteggere i file BACPAC nel modo appropriato e abilitare TDE al termine dell'importazione del nuovo database.

Ad esempio, se il file BACPAC viene esportato da un'istanza di SQL Server in locale, non viene crittografato automaticamente il contenuto importato del nuovo database. Allo stesso modo, se il file BACPAC viene esportato da un'istanza di SQL Server in locale, non viene crittografato automaticamente neppure il nuovo database.

L'unica eccezione riguarda l'esportazione da e verso un database SQL. TDE è abilitata nel nuovo database, ma il file BACPAC non viene comunque crittografato.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Gestire TDE nel portale di Azure

Per configurare TDE usando il portale di Azure, è necessario essere connessi come il proprietario, collaboratore o gestore della sicurezza SQL di Azure.

Attivare e disattivare TDE a livello di database. Per abilitare TDE in un database, andare al [portale di Azure](https://portal.azure.com) e accedere con l'account amministratore o di collaboratore di Azure. Trovare le impostazioni di TDE nel database utente. Per impostazione predefinita, viene usata TDE gestita dal servizio. Un certificato TDE viene generato automaticamente per il server che contiene il database. Per Istanza gestita di database SQL di Azure, usare T-SQL per attivare e disattivare TDE in un database.

![Transparent Data Encryption gestita dal servizio](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Impostare la chiave master di TDE, nota anche come protezione TDE, a livello di server. Per usare TDE con supporto Bring Your Own Key e proteggere i database con una chiave dall’insieme di credenziali delle chiavi, aprire le impostazioni di TDE nel server.

![TDE con supporto Bring Your Own Key](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Gestire TDE usando PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Il modulo Azure PowerShell per Resource Manager è ancora supportato dal Database SQL di Azure, ma i progetti di sviluppo future è per il modulo Az.Sql. Per questi cmdlet, vedere [azurerm. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Gli argomenti per i comandi nel modulo Az e nei moduli AzureRm sono sostanzialmente identici.

Per configurare TDE usando PowerShell, è necessario essere connessi come il proprietario, collaboratore o gestore della sicurezza SQL di Azure.

### <a name="cmdlets-for-azure-sql-database-and-data-warehouse"></a>Cmdlet per database SQL di Azure e Data Warehouse

Utilizzare i seguenti cmdlet per database SQL di Azure e Data Warehouse:

| Cmdlet | DESCRIZIONE |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |Abilita o disabilita TDE per un database|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |Ottiene lo stato di TDE per un database |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |Controlla l'avanzamento della crittografia per un database |
| [Add-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |Aggiunge una chiave di Key Vault a un'istanza di SQL Server |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |Fornisce le chiavi dell’insieme di credenziali delle chiavi per un server di database SQL di Azure  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |Imposta la protezione TDE per un istanza di SQL Server |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |Ottiene la protezione TDE |
| [Remove-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |Rimuove una chiave di Key Vault da un'istanza di SQL Server |
|  | |

> [!IMPORTANT]
> Per Istanza gestita di database SQL di Azure, utilizzare il comando T-SQL [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) per attivare e disattivare TDE a livello di database e controllare lo [script PowerShell di esempio](transparent-data-encryption-byok-azure-sql-configure.md) per gestire TDE a livello di istanza.

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Gestire TDE usando Transact-SQL

Connettere il database usando un account di accesso di un amministratore o di un membro del ruolo **dbmanager** nel database master.

| Comando | DESCRIZIONE |
| --- | --- |
| [ALTER DATABASE (database SQL di Azure)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | SET ENCRYPTION ON/OFF consente di crittografare o decrittografare un database |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Restituisce informazioni sullo stato di crittografia di un database e delle chiavi di crittografia associate al database |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Restituisce informazioni sullo stato di crittografia di un nodo di data warehouse e delle chiavi di crittografia associate al database |
|  | |

È possibile trasmettere la protezione TDE a una chiave di Key Vault usando Transact-SQL. Usare PowerShell o il portale Azure.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Gestire TDE usando API REST

Per configurare TDE usando API REST, è necessario essere connessi come il proprietario, collaboratore o gestore della sicurezza SQL di Azure.
Utilizzare i seguenti set di comandi per database SQL di Azure e Data Warehouse:

| Comando | DESCRIZIONE |
| --- | --- |
|[Crea o aggiorna il server](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Aggiunge un'identità di Azure Active Directory a un'istanza di SQL Server (usata per concedere l'accesso a Key Vault)|
|[Crea o aggiorna la chiave del server](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Aggiunge una chiave di Key Vault a un'istanza di SQL Server|
|[Elimina la chiave del server](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Rimuove una chiave di Key Vault da un'istanza di SQL Server|
|[Ottieni la chiave del server](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Ottiene una chiave di Key Vault da un'istanza di SQL Server|
|[Elenca le chiavi del server dal server](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Ottiene una chiave di Key Vault per un'istanza di SQL Server |
|[Crea o aggiorna la protezione della crittografia](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Imposta la protezione TDE per un istanza di SQL Server|
|[Ottieni la protezione della crittografia](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Ottiene la protezione TDE per un istanza di SQL Server|
|[Elenca le protezioni di crittografia dal server](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Ottiene le protezioni TDE per un istanza di SQL Server |
|[Crea o aggiorna la configurazione di Transparent Data Encryption](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Abilita o disabilita TDE per un database|
|[Ottieni la configurazione di Transparent Data Encryption](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Ottiene la configurazione di TDE per un database|
|[Elenca i risultati della configurazione di Transparent Data Encryption](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Ottiene il risultato della crittografia per un database|

## <a name="next-steps"></a>Passaggi successivi

- Per una descrizione generale della funzionalità TDE, vedere [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Per altre informazioni su TDE con supporto Bring Your Own Key per database SQL di Azure, Istanza gestita di database SQL di Azure e Data Warehouse, vedere [TDE con supporto Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md).
- Per iniziare a usare TDE con supporto Bring Your Own Key, vedere la guida [Abilitare Transparent Data Encryption usando la propria chiave di Key Vault con PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
- Per altre informazioni sugli insiemi di credenziali delle chiavi, vedere [Pagina di documentazione di Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
