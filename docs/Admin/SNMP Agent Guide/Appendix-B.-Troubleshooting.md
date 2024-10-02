# Appendix B. Troubleshooting 

### FAQ

##### Question

When I run altisnmpd, I get the error message: "Error: Failed to connect to the agentx master agent: Unknown host (Connection refused)".

##### Answer

Verify that the snmpd daemon is running. 

If so, use the altisnmpd -x option to check whether the IP and port are correct.

##### Question

When I execute the snmpget and snmpwalk commands, I get the error message: "No Such Object available on this agent at this OID".

##### Answer

This error can occur if altisnmpd and snmpd are not communicating correctly. 

Verify that altisnmpd is running. If the user restarted snmpd, altisnmpd must also be restarted.

##### Question

When I execute the snmpset command, I get the error message: "Error in packet. Reason: notWritable (that object does not support modification)...".

##### Answer

This error can occur if the value for a read-only object has been set. If it is a read-write object, this is a network problem. In this case, refer to the manual. 

##### Question

When I execute the snmpget and snmpwalk commands, I get the error message: "No Such Instance currently exists at this OID". 

##### Answer

This checks whether the name for an OID or object is saved correctly. The user can view Altibase MIB information with the snmpwalk command.

```
ex> snmpwalk -v 2c -c private IP:PORT altibase
```

Also, verify that Altibase is running.

##### Question

When I execute the snmpget and snmpwalk commands, I get the error message : "Unknown Object Identifier (Sub-id not found: (top) -> xxx)"

##### Answer

Verify whether the OID name or the object name has been properly specified. 

If the ALTIBASE-MIB.txt file cannot be loaded, check whether the file exists in \$MIBDIRS and then check that $MIBS is set to ALL.

##### Question

 When I execute the snmpwalk command, the entire ALTIBASE MIB is not output.

```
ex> snmpwalk -v 2c -c private IP:PORT altibase
ALTIBASE-MIB::altiPropertyIndex.1 = INTEGER: 1
ALTIBASE-MIB::altiStatusIndex.1 = INTEGER: 1  
```

##### Answer

If the entire ALTIBASE MIB is not output, verify if Altibase is running.
