---
title: "IBM DB2 database: Running commands for regular maintenance"
seo-title: "IBM DB2 database: Running commands for regular maintenance"
description: This document lists IBM DB2 commands that are recommended for regular maintenance of your AEM forms database. 
seo-description: This document lists IBM DB2 commands that are recommended for regular maintenance of your AEM forms database. 
uuid: 6895e8ba-279e-4332-9380-d43c7eb26376
contentOwner: admin
content-type: reference
geptopics: SG_AEMFORMS/categories/maintaining_the_aem_forms_database
products: SG_EXPERIENCEMANAGER/6.4/FORMS
discoiquuid: a0a8a79e-7c43-4ed2-8361-de32384f8916
index: y
internal: n
snippet: y
---

# IBM DB2 database: Running commands for regular maintenance{#ibm-db-database-running-commands-for-regular-maintenance}

The following IBM DB2 commands are recommended for regular maintenance of your AEM forms database. For detailed information about maintenance and performance tuning for your DB2 database, see *IBM DB2 Administration Guide*.

* **runstats:****** This command updates statistics that describe the physical characteristics of a database table, along with its associated indexes. Dynamic SQL statements generated by AEM forms automatically use these updated statistics, but static SQL statements built inside a database require that the `db2rbind` command be run as well. 
* **db2rbind****:** This command rebinds all the packages in the database. Use this command after running the `runstats` utility to revalidate all packages in the database.
* **reorg table**** or ****index****: **This command checks whether a reorganization of some tables and indexes is required.

  As your databases grow and change, recalculating table statistics is critical to improving database performance and should be done regularly. These commands can be run either manually by using scripts or by using a cron job.

<!--
Comment Type: remark
Last Modified By:
Last Modified Date:
<p>Bug 1522079:</p>
-->

>[!NOTE]
>
>Before you run the `runstats` command, the database must contain data, and at least one directory synchronization must have been performed.

For a small database, such as for 10,000 users or 2,500 groups, it is sufficient to invoke the `runstats` command to reduce the sync timings.

For larger databases, such as for 100,000 users or 10,000 groups, run the `reorg` command before you run the `runstats` command.

## Use the runstats command on your AEM forms database {#use-the-runstats-command-on-your-aem-forms-database}

Run the `runstats` command on the following AEM forms database tables and indexes.

<!--
Comment Type: remark
Last Modified By:
Last Modified Date:
<p>Bug 1909846:</p>
-->

>[!NOTE]
>
>The `*runstats*` command needs to be run only during the first database synchronization. However, it must be run twice during that process: once during the synchronization of Users and Groups and then during the synchronization of Group Members. Ensure that the script executes completely each time you run it.

For correct syntax and usage, see the database manufacturer’s documentation. Below, `<schema>` is used to denote the schema that is associated with your DB2 user name. If you have a simple default DB2 installation, this is the database schema name.

```as3
     TABLE <schema>.EDCPRINCIPALGROUPENTITY 
  
     TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY 
  
     TABLE <schema>.EDCPRINCIPALENTITY 
  
     TABLE <schema>.EDCPRINCIPALUSERENTITY 
  
     TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY 
  
     TABLE <schema>.EDCPRINCIPALENTITY FOR INDEXES ALL 
  
     TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY FOR INDEXES ALL 
  
     TABLE <schema>.EDCPRINCIPALUSERENTITY FOR INDEXES ALL 
  
     TABLE <schema>.EDCPRINCIPALGROUPENTITY FOR INDEXES ALL 
  
     TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY FOR INDEXES ALL
```

## Run the reorg command on your AEM forms database {#run-the-reorg-command-on-your-aem-forms-database}

Run the `reorg` command on the following AEM forms database tables and indexes. For correct syntax and usage, see the database manufacturer’s documentation.

```as3
     TABLE <schema>.EDCPRINCIPALGROUPENTITY 
  
     TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY 
  
     TABLE <schema>.EDCPRINCIPALENTITY 
  
     TABLE <schema>.EDCPRINCIPALUSERENTITY 
  
     TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY 
  
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALENTITY 
  
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALEMAILALIASENTITY 
  
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALUSERENTITY 
  
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALGROUPENTITY 
  
     INDEXES ALL FOR TABLE <schema>.EDCPRINCIPALGRPCTMNTENTITY
```
