
# UNIX Commands 


Global search and replace in VI editor
: `:%s/TODLSCS_D/TODLSCSMB_MB/g`



#### Reset ETL Control 

1. Cancel the jobstream 
: `can JSNAME`
2. Reset the ETL Control entries
: `. /apps/prod/common/release/etlcontrol.env && /apps/prod/common/release/Update_ETLControlBatch.ksh <JSNAME>`
3. Resubmit the jobstream
: `sbs JSNAME`


# ETL Control Cheat Sheet

#### Find All details 

Use this query (with a modified WHERE clause) to get details for a ETL flow

```
SELECT *
FROM ETLCONTROLJOB J
JOIN ETLCONTROLJOBSTREAM JS ON J.JOBSTREAMID = JS.JOBSTREAMID
JOIN ETLCONTROLSOURCEJOB SJ ON J.JOBID = SJ.JOBID
JOIN ETLCONTROLSOURCE S ON S.SYSTEMID = SJ.SYSTEMID AND S.SOURCEID = SJ.SOURCEID
JOIN ETLCONTROLSOURCESYSTEM SS ON S.SYSTEMID = SS.SYSTEMID;

```

# Mongo DB 

### Connection Details

##### Production
```
mongodb+srv://PTFPIMSPB_RO:PPSMIPFTP958527983@datahub-prod.lcdvl.mongodb.net/prod?authSource=admin&replicaSet=atlas-ei9j1f-shard-0&readPreference=primary&appname=MongoDB%20Compass%20Readonly&ssl=true
```

##### Development

```
datahub-dev-shard-00-02.gjbpj.mongodb.net:27017,datahub-dev-shard-00-00.gjbpj.mongodb.net:27017
```

### Segment Refresh in Mongo Collection


1. Update the ETL Control Entries in ME_AUDIT schema
: `UPDATE ME_AUDIT.ETL_SEGMENT_TO_TABLE_MAPPING SET READY_IND='Y' where SEGMENT_NAME in ('SST001');`


2. Run the below script to trigger full refresh using `infa` user in the Informatica server
: `/share/infa/Scripts/Common/Mongo_fullrefresh_exec.ksh teqda_imst SVR020 wf_FULL_SVR020`


# ODS Refresh
ODS Refresh

1. Set READY_IND = 'Y'

2. Run the below script to trigger full refresh using `dwh` user in the Informatica server
```
/home/dwh/etl/bin/etl_fullrefresh_exec.ksh me VR_AVL_HIST_AUDIT_TRAIL_T Y N WF_SVR021_VR_AVL_HIST_AUDIT_TRAIL_T
```

