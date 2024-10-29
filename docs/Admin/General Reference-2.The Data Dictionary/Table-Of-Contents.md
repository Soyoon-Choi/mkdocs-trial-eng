# Table Of Contents

- [Preface](#preface)
  - [About This Manual](#about-this-manual)
- [1. The Data Dictionary](#1-the-data-dictionary)
  - [Meta Tables](#meta-tables)
  - [SYS_AUDIT\_](#sys_audit_)
  - [SYS_AUDIT_OPTS\_](#sys_audit_opts_)
  - [SYS_COLUMNS\_](#sys_columns_)
  - [SYS_COMMENTS\_](#sys_comments_)
  - [SYS_COMPRESSION_TABLES\_](#sys_compression_tables_)
  - [SYS_CONSTRAINTS\_](#sys_constraints_)
  - [SYS_CONSTRAINT_COLUMNS\_](#sys_constraint_columns_)
  - [SYS_CONSTRAINT_RELATED\_](#sys_constraint_related_)
  - [SYS_DATABASE\_](#sys_database_)
  - [SYS_DATABASE_LINKS\_](#sys_database_links_)
  - [SYS_DIRECTORIES\_](#sys_directories_)
  - [SYS_ENCRYPTED_COLUMNS\_](#sys_encrypted_columns_)
  - [SYS_GRANT_OBJECT\_](#sys_grant_object_)
  - [SYS_GRANT_SYSTEM\_](#sys_grant_system_)
  - [SYS_INDEX_COLUMNS\_](#sys_index_columns_)
  - [SYS_INDEX_PARTITIONS\_](#sys_index_partitions_)
  - [SYS_INDEX_RELATED\_](#sys_index_related_)
  - [SYS_INDICES\_](#sys_indices_)
  - [SYS_JOBS\_](#sys_jobs_)
  - [SYS_LIBRARIES\_](#sys_libraries_)
  - [SYS_LOBS\_](#sys_lobs_)
  - [SYS_MATERIALIZED_VIEWS\_](#sys_materialized_views_)
  - [SYS_PACKAGES\_](#sys_packages_)
  - [SYS_PACKAGE_PARAS\_](#sys_package_paras_)
  - [SYS_PACKAGE_PARSE\_](#sys_package_parse_)
  - [SYS_PACKAGE_RELATED\_](#sys_package_related_)
  - [SYS_PART_INDICES\_](#sys_part_indices_)
  - [SYS_PART_KEY_COLUMNS\_](#sys_part_key_columns_)
  - [SYS_PART_LOBS\_](#sys_part_lobs_)
  - [SYS_PART_TABLES\_](#sys_part_tables_)
  - [SYS_PASSWORD_HISTORY\_](#sys_password_history_)
  - [SYS_PASSWORD_LIMITS\_](#sys_password_limits_)
  - [SYS_PRIVILEGES\_](#sys_privileges_)
  - [SYS_PROCEDURES\_](#sys_procedures_)
  - [SYS_PROC_PARAS\_](#sys_proc_paras_)
  - [SYS_PROC_PARSE\_](#sys_proc_parse_)
  - [SYS_PROC_RELATED\_](#sys_proc_related_)
  - [SYS_RECYCLEBIN\_](#sys_recyclebin_)
  - [SYS_REPLICATIONS\_](#sys_replications_)
  - [SYS_REPL_HOSTS\_](#sys_repl_hosts_)
  - [SYS_REPL_ITEMS\_](#sys_repl_items_)
  - [SYS_REPL_OFFLINE_DIR\_](#sys_repl_offline_dir_)
  - [SYS_REPL_OLD_CHECKS_](#sys_repl_old_checks_)
  - [SYS_REPL_OLD_CHECK_COLUMNS_](#sys_repl_old_check_columns_)
  - [SYS_REPL_OLD_COLUMNS\_](#sys_repl_old_columns_)
  - [SYS_REPL_OLD_INDEX_COLUMNS\_](#sys_repl_old_index_columns_)
  - [SYS_REPL_OLD_INDICES\_](#sys_repl_old_indices_)
  - [SYS_REPL_OLD_ITEMS\_](#sys_repl_old_items_)
  - [SYS_REPL_TABLE_OID_IN_USE_](#sys_repl_table_oid_in_use_)
  - [SYS_REPL_RECOVERY_INFOS\_](#sys_repl_recovery_infos_)
  - [SYS_SECURITY\_](#sys_security_)
  - [SYS_SYNONYMS\_](#sys_synonyms_)
  - [SYS_TABLES\_](#sys_tables_)
  - [SYS_TABLE_PARTITIONS\_](#sys_table_partitions_)
  - [SYS_TABLE_SIZE\_](#sys_table_size_)
  - [SYS_TBS_USERS\_](#sys_tbs_users_)
  - [SYS_TRIGGERS\_](#sys_triggers_)
  - [SYS_TRIGGER_DML_TABLES\_](#sys_trigger_dml_tables_)
  - [SYS_TRIGGER_STRINGS\_](#sys_trigger_strings_)
  - [SYS_TRIGGER_UPDATE_COLUMNS\_](#sys_trigger_update_columns_)
  - [SYS_USERS\_](#sys_users_)
  - [DBA_USERS\_](#dba_users_)
  - [SYS_USER_ROLES\_](#sys_user_roles_)
  - [SYS_VIEWS\_](#sys_views_)
  - [SYS_VIEW_PARSE\_](#sys_view_parse_)
  - [SYS_VIEW_RELATED\_](#sys_view_related_)
  - [SYS_XA_HEURISTIC_TRANS\_](#sys_xa_heuristic_trans_)
  - [SYS_GEOMETRIES_](#sys_geometries_)
  - [SYS_GEOMETRY_COLUMNS_](#sys_geometry_columns_)
  - [USER_SRS_](#user_srs_)
  - [Performance Views](#performance-views)
  - [V\$ACCESS_LIST](#vaccess_list)
  - [V\$ALLCOLUMN](#vallcolumn)
  - [V\$ARCHIVE](#varchive)
  - [V\$BACKUP_INFO](#vbackup_info)
  - [V\$BUFFPAGEINFO](#vbuffpageinfo)
  - [V\$BUFFPOOL_STAT](#vbuffpool_stat)
  - [V\$CATALOG](#vcatalog)
  - [V\$DATABASE](#vdatabase)
  - [V\$DATAFILES](#vdatafiles)
  - [V\$DATATYPE](#vdatatype)
  - [V\$DBA_2PC_PENDING](#vdba_2pc_pending)
  - [V\$DBLINK_ALTILINKER_STATUS](#vdblink_altilinker_status)
  - [V\$DBLINK_DATABASE_LINK_INFO](#vdblink_database_link_info)
  - [V\$DBLINK_GLOBAL_TRANSACTION_INFO](#vdblink_global_transaction_info)
  - [V\$DBLINK_LINKER_CONTROL_SESSION_INFO](#vdblink_linker_control_session_info)
  - [V\$DBLINK_LINKER_DATA_SESSION_INFO](#vdblink_linker_data_session_info)
  - [V\$DBLINK_LINKER_SESSION_INFO](#vdblink_linker_session_info)
  - [V\$DBLINK_NOTIFIER_TRANSACTION_INFO](#vdblink_notifier_transaction_info)
  - [V\$DBLINK_REMOTE_STATEMENT_INFO](#vdblink_remote_statement_info)
  - [V\$DBLINK_REMOTE_TRANSACTION_INFO](#vdblink_remote_transaction_info)
  - [V\$DBMS_STATS](#vdbms_stats)
  - [V\$DB_FREEPAGELISTS](#vdb_freepagelists)
  - [V\$DB_PROTOCOL](#vdb_protocol)
  - [V\$DIRECT_PATH_INSERT](#vdirect_path_insert)
  - [V\$DISKTBL_INFO](#vdisktbl_info)
  - [V\$DISK_BTREE_HEADER](#vdisk_btree_header)
  - [V\$DISK_TEMP_INFO](#vdisk_temp_info)
  - [V\$DISK_TEMP_STAT](#vdisk_temp_stat)
  - [V\$DISK_UNDO_USAGE](#vdisk_undo_usage)
  - [V\$EVENT_NAME](#vevent_name)
  - [V\$EXTPROC_AGENT](#vextproc_agent)
  - [V\$FILESTAT](#vfilestat)
  - [V\$FLUSHER](#vflusher)
  - [V\$FLUSHINFO](#vflushinfo)
  - [V\$INDEX](#vindex)
  - [V\$INSTANCE](#vinstance)
  - [V\$INTERNAL_SESSION](#vinternal_session)
  - [V\$LATCH](#vlatch)
  - [V\$LIBRARY](#vlibrary)
  - [V\$LFG](#vlfg)
  - [V\$LOCK](#vlock)
  - [V\$LOCK_STATEMENT](#vlock_statement)
  - [V\$LOG](#vlog)
  - [V\$LOCK_WAIT](#vlock_wait)
  - [V\$MEMGC](#vmemgc)
  - [V\$MEMSTAT](#vmemstat)
  - [V\$MEMTBL_INFO](#vmemtbl_info)
  - [V\$MEM_BTREE_HEADER](#vmem_btree_header)
  - [V\$MEM_BTREE_NODEPOOL](#vmem_btree_nodepool)
  - [V\$MEM_RTREE_HEADER](#vmem_rtree_header)
  - [V\$MEM_RTREE_NODEPOOL](#vmem_rtree_nodepool)
  - [V\$MEM_TABLESPACES](#vmem_tablespaces)
  - [V\$MEM_TABLESPACE_CHECKPOINT_PATHS](#vmem_tablespace_checkpoint_paths)
  - [V\$MEM_TABLESPACE_STATUS_DESC](#vmem_tablespace_status_desc)
  - [V\$MUTEX](#vmutex)
  - [V\$NLS_PARAMETERS](#vnls_parameters)
  - [V\$NLS_TERRITORY](#vnls_territory)
  - [V\$OBSOLETE_BACKUP_INFO](#vobsolete_backup_info)
  - [V\$PKGTEXT](#vpkgtext)
  - [V\$PLANTEXT](#vplantext)
  - [V\$PROCTEXT](#vproctext)
  - [V\$PROCINFO](#vprocinfo)
  - [V\$PROPERTY](#vproperty)
  - [V\$REPEXEC](#vrepexec)
  - [V\$REPGAP](#vrepgap)
  - [V\$REPGAP_PARALLEL](#vrepgap_parallel)
  - [V\$REPLOGBUFFER](#vreplogbuffer)
  - [V\$REPOFFLINE_STATUS](#vrepoffline_status)
  - [V\$REPRECEIVER](#vrepreceiver)
  - [V\$REPRECEIVER_COLUMN](#vrepreceiver_column)
  - [V\$REPRECEIVER_PARALLEL](#vrepreceiver_parallel)
  - [V\$REPRECEIVER_PARALLEL_APPLY](#vrepreceiver_parallel_apply)
  - [V\$REPRECEIVER_STATISTICS](#vrepreceiver_statistics)
  - [V\$REPRECEIVER_TRANSTBL](#vrepreceiver_transtbl)
  - [V\$REPRECEIVER_TRANSTBL_PARALLEL](#vrepreceiver_transtbl_parallel)
  - [V\$REPRECOVERY](#vreprecovery)
  - [V\$REPSENDER](#vrepsender)
  - [V\$REPSENDER_PARALLEL](#vrepsender_parallel)
  - [V\$REPSENDER_SENT_LOG_COUNT](#vrepsender_sent_log_count)
  - [V\$REPSENDER_SENT_LOG_COUNT_PARALLEL](#vrepsender_sent_log_count_parallel)
  - [V\$REPSENDER_STATISTICS](#vrepsender_statistics)
  - [V\$REPSENDER_TRANSTBL](#vrepsender_transtbl)
  - [V\$REPSENDER_TRANSTBL_PARALLEL](#vrepsender_transtbl_parallel)
  - [V\$REPSYNC](#vrepsync)
  - [V\$RESERVED_WORDS](#vreserved_words)
  - [V\$SBUFFER_STAT](#vsbuffer_stat)
  - [V\$SEGMENT](#vsegment)
  - [V\$SEQ](#vseq)
  - [V\$SERVICE_THREAD](#vservice_thread)
  - [V\$SERVICE_THREAD_MGR](#vservice_thread_mgr)
  - [V\$SESSION](#vsession)
  - [V\$SESSION_EVENT](#vsession_event)
  - [V\$SESSION_WAIT](#vsession_wait)
  - [V\$SESSION_WAIT_CLASS](#vsession_wait_class)
  - [V\$SESSIONMGR](#vsessionmgr)
  - [V\$SESSTAT](#vsesstat)
  - [V\$SFLUSHER](#vsflusher)
  - [V\$SFLUSHINFO](#vsflushinfo)
  - [V\$SNAPSHOT](#vsnapshot)
  - [V\$SQLTEXT](#vsqltext)
  - [V\$SQL_PLAN_CACHE](#vsql_plan_cache)
  - [V\$SQL_PLAN_CACHE_PCO](#vsql_plan_cache_pco)
  - [V\$SQL_PLAN_CACHE_SQLTEXT](#vsql_plan_cache_sqltext)
  - [V\$STABLE_MEM_DATAFILES](#vstable_mem_datafiles)
  - [V\$STATEMENT](#vstatement)
  - [V\$STATNAME](#vstatname)
  - [V\$SYSSTAT](#vsysstat)
  - [V\$SYSTEM_CONFLICT_PAGE](#vsystem_conflict_page)
  - [V\$SYSTEM_EVENT](#vsystem_event)
  - [V\$SYSTEM_WAIT_CLASS](#vsystem_wait_class)
  - [V\$TABLE](#vtable)
  - [V\$TABLESPACES](#vtablespaces)
  - [V\$TIME_ZONE_NAMES](#vtime_zone_names)
  - [V\$TRACELOG](#vtracelog)
  - [V\$TRANSACTION](#vtransaction)
  - [V\$TRANSACTION_MGR](#vtransaction_mgr)
  - [V\$TSSEGS](#vtssegs)
  - [V\$TXSEGS](#vtxsegs)
  - [V\$UDSEGS](#vudsegs)
  - [V\$UNDO_BUFF_STAT](#vundo_buff_stat)
  - [V\$USAGE](#vusage)
  - [V\$VERSION](#vversion)
  - [V\$VOL_TABLESPACES](#vvol_tablespaces)
  - [V\$WAIT_CLASS_NAME](#vwait_class_name)
  - [V\$XID](#vxid)
- [2. Sample Schema](#2-sample-schema)
  - [Information about the Sample Schema](#information-about-the-sample-schema)
  - [E-R Entity-Relationship (ER) Diagram and Sample Data](#e-r-entity-relationship-er-diagram-and-sample-data)


