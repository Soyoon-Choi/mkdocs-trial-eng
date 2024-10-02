   # After all procedures are successfully completed, aku is terminated. 
   AKU run successfully.
   ```

### 4) If the situation in which Pod was force terminated before `aku -p end` command completed or terminated with the property AKU_REPLICATION_RESET_AT_END set to 0, continues for a long time<a name="cautions4"></a>

If the situation in which a Pod was force-terminated before the `aku -p end` command completed or terminated with the property AKU_REPLICATION_RESET_AT_END set to 0 continues for a long time, there is a possibility of uninitialized replication information remaining in the terminated Pod as well as in other Pods. When this happens, the other Pods do not delete the online log files that are required for replication to the terminated Pod. If the online log file accumulates a lot, it can lead to disk space exhaustion and result in Altibase server not being able to operate normally. To prevent this situation, if you notice that there are long periods of time in which a Pod was terminated before the `aku -p end` command completed, you should stop replication and initialize the replication information. Refer to the commands below.

~~~sql
ALTER REPLICATION replication_name STOP;
ALTER REPLICATION replication_name RESET;
~~~
Suppose *pod_name*-1 failed to complete `aku -p end` and and terminated abnormally while *pod_name*-0 and *pod_name*-1 are both operational. Let's check the XSN of SYSTEM_.SYS_REPLICATIONS_ on *pod_name*-0. You can see that the XSN value of the replication object AKU_REP_01 is not -1. It means that the replication information is not initialized. AKU_REP_01 is the replication object between *pod_name*-0 and *pod_name*-1.
~~~sql
iSQL> SELECT REPLICATION_NAME, XSN FROM SYSTEM_.SYS_REPLICATIONS_;
REPLICATION_NAME                XSN                  
--------------------------------------------------------
AKU_REP_03                      -1
AKU_REP_02                      -1
AKU_REP_01                      859070110
3 rows selected.
~~~
Execute the statements for stopping and resetting replication of the object(AKU_REP_01) on *pod_name*-0.
~~~sql
iSQL> ALTER REPLICATION AKU_REP_01 STOP;
Alter sucess.
iSQL> ALTER REPLICATION AKU_REP_01 RESET;
Alter sucess.
~~~
And then, let's check the XSN of SYSTEM_.SYS_REPLICATIONS_ on *pod_name*-0. The XSN value of the replication object AKU_REP_01 was changed to -1.
~~~sql
iSQL> SELECT REPLICATION_NAME, XSN FROM SYSTEM_.SYS_REPLICATIONS_;
REPLICATION_NAME                XSN                  
--------------------------------------------------------
AKU_REP_03                      -1
AKU_REP_02                      -1
AKU_REP_01                      -1
3 rows selected.
~~~

## Examples

This section introduces various examples of aku usage.

The aku log in the example codes includes the following information:

```
[AKU][Date and time][Thread number] [Message type][Code information][Target pod name][Replication name] Message
```

### aku -i

This is the result of running `aku -i` and displays the information set in aku.conf. A server with Server ID 0 is that of the first Pod created by the StatefulSet.

~~~bash
$ aku -i
  #########################
 [ Server ]
  Server ID        : 0
  Host             : AKUHOST-0.altibase-svc
  User             : SYS
  Password         : manager
  Port             : 20300
  Replication Port : 20301
  Max Server Count : 4
 #########################
 [ Replications ]
 #### Serve[ID:0] Replication list ####
  Replication Name : AKU_REP_01
  Replication Name : AKU_REP_02
  Replication Name : AKU_REP_03
 #### Serve[ID:1] Replication list ####
  Replication Name : AKU_REP_01
  Replication Name : AKU_REP_12
  Replication Name : AKU_REP_13
 #### Serve[ID:2] Replication list ####
  Replication Name : AKU_REP_02
  Replication Name : AKU_REP_12
  Replication Name : AKU_REP_23
 #### Serve[ID:3] Replication list ####
  Replication Name : AKU_REP_03
  Replication Name : AKU_REP_13
  Replication Name : AKU_REP_23
 #########################
 [ Replication Items ]
  #### Replication [Prefix:AKU_REP] item list ####
  User Name        : SYS
  Table Name       : T1
 
  User Name        : SYS
  Table Name       : T2
 
  User Name        : SYS
  Table Name       : T3
 #########################
~~~

### aku -p start on a Master Pod

This is an output of running `aku -p start` on a Master Pod (*pod_name*-0).

~~~bash
$ aku -p start
AKU started with START option.
[AKU][2024/03/18 12:34:58.136944][140708807235840] [INFO][akuRunStart:828][-][-] Start as MASTER Pod.
AKU run successfully.
~~~

The Description of the output is as below.

~~~bash
# Read aku.conf and create replication objects. 
# START option means that aku -p start is executed.
AKU started with START option.

# MASTER Pod indicates the first pod. 
[AKU][2024/03/18 12:34:58.136944][140708807235840] [INFO][akuRunStart:828][-][-] Start as MASTER Pod.

# After all procedures are successfully completed, aku is terminated. 
AKU run successfully.
~~~

### aku -p start on the 4th Pod

This is an output of `aku -p start` command on the 4th Pod (*pod_name*-3).

~~~bash
AKU started with START option.
[AKU][2024/03/18 14:01:59.604647][140678415444224] [INFO][akuRunStart:903][-][-] Start as SLAVE Pod.
[AKU][2024/03/18 14:02:01.005068][140678415444224] [INFO][akuRunStart:959][-][-] Truncate tables for replications.
[AKU][2024/03/18 14:02:01.025100][140678415444224] [INFO][akuRunStart:964][-][-] Table truncation has ended.
[AKU][2024/03/18 14:02:01.025877][140678415444224] [INFO][akuRunStart:975][-][-] Sync tables from MASTER Server.
[AKU][2024/03/18 14:02:05.045135][140678415444224] [INFO][akuRunStart:980][-][-] Replication sync has ended.
AKU run successfully.
~~~

The Description of the output is as below.

~~~bash
# Read aku.conf and create replication objects. 
# START option means that aku -p start is executed.
 AKU started with START option.

# SLAVE Pod indicates other pods but the first pod. 
[AKU][2024/03/18 14:01:59.604647][140678415444224] [INFO][akuRunStart:903][-][-] Start as SLAVE Pod.

# To prvent record conflict during SYNC processing, deletes all records on the target table.
[AKU][2024/03/18 14:02:01.005068][140678415444224] [INFO][akuRunStart:959][-][-] Truncate tables for replications.
[AKU][2024/03/18 14:02:01.025100][140678415444224] [INFO][akuRunStart:964][-][-] Table truncation has ended.

# MASTER Server indicates the Altibase server of the Master Pod. The data has been synchronized from the server to the local pod.
[AKU][2024/03/18 14:02:01.025877][140678415444224] [INFO][akuRunStart:975][-][-] Sync tables from MASTER Server.
[AKU][2024/03/18 14:02:05.045135][140678415444224] [INFO][akuRunStart:980][-][-] Replication sync has ended.

# After all procedures are successfully completed, aku is terminated. 
AKU run successfully.
~~~

### aku -p end on the 4th Pod

This is an output of `aku -p end` command with the property AKU_REPLICATION_RESET_AT_END set to 1 on the 4th Pod (*pod_name*-3). You can see that replication FLUSH and RESET commands are executed.

~~~bash
$ aku -p end
AKU started with END option.
[AKU][2024/03/18 14:02:49.246961][139626938108160] [INFO][akuRunEnd:1090][-][-] Start as SLAVE Pod.
[AKU][2024/03/18 14:02:49.247094][139626938108160] [INFO][akuRunEnd:1095][-][-] Flush replications.
[AKU][2024/03/18 14:02:49.247731][139626938108160] [INFO][akuRunEnd:1100][-][-] Replication flush has ended.
[AKU][2024/03/18 14:02:52.001848][139626938108160] [INFO][akuRunEnd:1114][-][-] Reset replications.
[AKU][2024/03/18 14:02:52.014300][139626938108160] [INFO][akuRunEnd:1119][-][-] Replication reset has ended.
AKU run successfully.
~~~

The Description of the output is as below.

```bash
# Read aku.conf, stop the replication and reset the replication. 
# END option means that aku -p end is executed.
AKU started with END option.

# SLAVE Pod indicates other pods but the first pod. 
[AKU][2024/03/18 14:02:49.246961][139626938108160] [INFO][akuRunEnd:1090][-][-] Start as SLAVE Pod.

# FLUSH unsent changes to other pods.
[AKU][2024/03/18 14:02:49.247094][139626938108160] [INFO][akuRunEnd:1095][-][-] Flush replications.
[AKU][2024/03/18 14:02:49.247731][139626938108160] [INFO][akuRunEnd:1100][-][-] Replication flush has ended.

# Reset all replication objects created on the aku of the local pod.
[AKU][2024/03/18 14:02:52.001848][139626938108160] [INFO][akuRunEnd:1114][-][-] Reset replications.
[AKU][2024/03/18 14:02:52.014300][139626938108160] [INFO][akuRunEnd:1119][-][-] Replication reset has ended.

# After all procedures are successfully completed, aku is terminated. 
AKU run successfully.
```



