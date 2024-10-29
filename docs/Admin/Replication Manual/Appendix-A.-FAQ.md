# Appendix A. FAQ

### Replication FAQ

##### **Question**

I want to know how to resolve conflicts.

##### **Answer**

Please refer to the section "Troubleshooting" in Chapter 2.

##### **Question**

Is replication possible between two servers located on different local networks?

##### **Answer**

Yes, it's possible. However, because of the physical distance between two servers, replication performance may downgrade somewhat in accordance with bandwidth and latency.

##### **Question**

Can I execute ADD COLUMN on a replication target table?

##### **Answer**

Yes, you many execute DDL statements on replication target tables.

First, set the following property settings: set the REPLICATION_DDL_ENABLE property to 1, and, using the ALTER SESSION SET REPLICATION command, set the REPLICATION property to some value other than NONE.

For more detailed information, please refer to Executing DDL Statements on Replication Target Tables.

##### **Question**

When one of two servers connected for replication goes down or offline and then comes back online, how can I check the current status of replication data to be sent to the other server? 

##### **Answer**

The replication gap, meaning the number of redo logs for which corresponding XLogs need to be sent but have not yet been sent, can be checked by querying the REP_GAP column in the V$REPGAP performance view. Performance views can also be used to check various other information related to replication execution.

##### **Question**

Is replication possible between two different kinds of servers? 

##### **Answer**

Yes, it's possible. The heterogeneous replication function of Altibase takes into account byte ordering, structure aligning, endian and bit count on both the Sender and Receiver in order to make replication between different kinds of servers possible.

To achieve this, when XLOGs are sent or received, the Sender thread adds data to be sent to a transmission buffer, and the Receiver thread receives data from a reception buffer in the same order in which it was sent by the Sender thread. 

However, when performing replication between heterogeneous servers, if the byte order is different, the necessary operation of changing the byte order will entail a reduction in performance.

##### **Question**

Can I add or delete tables while replication is active? 

##### **Answer**

This is impossible while replication is underway. To add or delete replication target tables, it is first necessary to stop replication.

##### **Question**

Can I perform replication between memory and disk tables? 

##### **Answer**

Yes, it's possible. 
