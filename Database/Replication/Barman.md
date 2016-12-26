Barman 2.0, backup over streaming replication is the recommended setup for
PostgreSQL 9.4 or higher.

Traditional backup via rsync /SSH is available for all versions of PostgreSQL
starting from 8.3, and it is recommended in all cases where pg_basebackup
limitations occur (for example, a very large database that can benefit from
incremental backup and deduplication).  
The reason why we recommend streaming backup is that, based on our experience,
it is easier to setup than the traditional one. 

Two typical scenarios for backups
Scenario 1: Backup via streaming protocol (streaming-only setup)
In this scenario, you will need to configure:
1. a standard connection to PostgreSQL, for management, coordination, and
   monitoring purposes
2. a streaming replication connection that will be used by both pg_basebackup
   (for base backup operations) and pg_receivexlog (for WAL streaming)

Requirements for recovery
Remote recovery is definitely the most common way to restore a PostgreSQL
server with Barman.

Pratik:


ilgili paketler:
barman-cli: Client Utilities for Barman, Backup and Recovery Manager
for PostgreSQL
barman: Backup and Recovery Manager for PostgreSQL
pgespresso92: Optional Extension for Barman

bagimliliklar:
python-argcomplete
python-argh       
python-dateutil   
python-psycopg2   

