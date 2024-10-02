# Appendix A. Error Codes

### Error Code Table

오류 The Log Analyzer error codes and the cause of each kind of error are set forth in the following table.코드와 발생 원인을 설명한다.

#### FATAL Error

<table>
<tbody>
<tr>
<th>Error Code</th><th>Description</th><th>Can be returned by</th>
</tr>
<tr>
<td>
<p>0x50008</p>
</td>
<td>
<p>Returned in response to an attempt to
begin a transaction that is already active</p>
</td>
<td>
<p>ALA_ReceiveXLog<br />ALA_GetXLog</p>
</td>
</tr>
<tr>
<td>
<p>0x5000A</p>
</td>
<td>
<p>Failed to initialize a mutex</p>
</td>
<td>
<p>ALA_CreateXLogCollector<br />ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x5000B</p>
</td>
<td>
<p>Failed to remove a mutex</p>
</td>
<td>
<p>ALA_Handshake<br />ALA_DestroyXLogCollector</p>
</td>
</tr>
<tr>
<td>
<p>0x5000C</p>
</td>
<td>
<p>Failed to lock a mutex </p>
</td>
<td rowspan="2" >
<p>ALA_AddAuthInfo<br />ALA_RemoveAuthInfo<br />ALA_Handshake<br />ALA_ReceiveXLog<br />ALA_GetXLog<br />ALA_SendACK<br />ALA_FreeXLog<br />ALA_DestroyXLogCollector<br />ALA_GetXLogCollectorStatus</p>
</td>
</tr>
<tr>
<td>
<p>0x5000D</p>
</td>
<td>
<p>Failed to unlock a mutex</p>
</td>
</tr>
</tbody>
</table>


#### ABORT Error

<table>
<tbody>
<tr>
<th>Error Code</th><th>Description</th><th>Can be returned by</th></tr>
<tr>
<td>
<p>0x51006</p>
</td>
<td>
<p>Failed to allocate memory</p>
</td>
<td>
<p>All Log Analysis API functions</p>
</td>
</tr>
<tr>
<td>
<p>0x5101E</p>
</td>
<td>
<p>Failed to allocate memory in pool</p>
</td>
<td>
<p>ALA_ReceiveXLog</p>
</td>
</tr>
<tr>
<td>
<p>0x5101F</p>
</td>
<td>
<p>Failed to free memory in pool </p>
</td>
<td>
<p>ALA_Handshake<br />ALA_ReceiveXLog<br />ALA_FreeXLog<br />ALA_DestroyXLogCollector</p>
</td>
</tr>
<tr>
<td>
<p>0x51020</p>
</td>
<td>
<p>Failed to initialize the memory pool</p>
</td>
<td>
<p>ALA_CreateXLogCollector</p>
</td>
</tr>
<tr>
<td>
<p>0x51021</p>
</td>
<td>
<p>Failed to remove the memory pool</p>
</td>
<td>
<p>ALA_DestroyXLogCollector</p>
</td>
</tr>
<tr>
<td>
<p>0x51013</p>
</td>
<td>
<p>Failed to initialize the network environment</p>
</td>
<td rowspan="3">
<p>ALA_Handshake<br />ALA_ReceiveXLog<br />ALA_SendACK</p>
</td>
</tr>
<tr>
<td>
<p>0x51019</p>
</td>
<td>
<p>NFailed to remove a network protocol</p>
</td>
</tr>
<tr>
<td>
<p>0x5101A</p>
</td>
<td>
<p>Failed to finalize the network environment</p>
</td>
</tr>
<tr>
<td>
<p>0x51017</p>
</td>
<td>
<p>The network session has already been
terminated.</p>
</td>
<td>
<p>ALA_ReceiveXLog<br />ALA_SendACK</p>
</td>
</tr>
<tr>
<td>
<p>0x51018</p>
</td>
<td>
<p>The network protocol is unfamiliar. </p>
</td>
<td rowspan="2">
<p>ALA_Handshake<br />ALA_ReceiveXLog</p>
</td>
</tr>
<tr>
<td>
<p>0x51016</p>
</td>
<td>
<p>Failed to read from the network</p>
</td>
</tr>
<tr>
<td>
<p>0x5101B</p>
</td>
<td>
<p>Failed to write to the network</p>
</td>
<td rowspan="2">
<p>ALA_Handshake<br />ALA_SendACK</p>
</td>
</tr>
<tr>
<td>
<p>0x5101C</p>
</td>
<td>
<p>Failed to flush the network</p>
</td>
</tr>
<tr>
<td>
<p>0x51015</p>
</td>
<td>
<p>A network timeout (and thus a probable
network error) has occurred.</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x5102C</p>
</td>
<td>
<p>Failed to add a network session</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x51024</p>
</td>
<td>
<p>Protocol versions are mismatched.</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x51027</p>
</td>
<td>
<p>Failed to allocate a link</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x51028</p>
</td>
<td>
<p>Failed to listen for a link</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x51029</p>
</td>
<td>
<p>Failed to wait for a link</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x5102A</p>
</td>
<td>
<p>Failed to accept a link</p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x5102B</p>
</td>
<td>
<p>Failed to set a link </p>
</td>
<td>
<p>ALA_Handshake</p>
</td>
</tr>
<tr>
<td>
<p>0x51022</p>
</td>
<td>
<p>Failed to shut down a link </p>
</td>
<td rowspan="2">
<p>ALA_Handshake<br />ALA_DestroyXLogCollector</p>
</td>
</tr>
<tr>
<td>
<p>0x51023</p>
</td>
<td>
<p>Failed to free a link</p>
</td>
</tr>
<tr>
<td>
<p>0x51012</p>
</td>
<td>
<p>The meta information does not exist.</p>
</td>
<td>
<p>ALA_Handshake<br />ALA_GetXLog<br />ALA_GetReplicationInfo<br />ALA_GetTableInfo<br />ALA_GetTableInfoByName</p>
</td>
</tr>
<tr>
<td>
<p>0x5103F</p>
</td>
<td>
<p>The table information does not exist. </p>
</td>
<td>
<p>ALA_GetXLog</p>
</td>
</tr>
<tr>
<td>
<p>0x51040</p>
</td>
<td>
<p>The column information does not exist.</p>
</td>
<td>
<p>ALA_GetXLog</p>
</td>
</tr>
</tbody>
</table>


#### INFO Error

| Error Code | Description                                                  | Can be returned by                                           |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 0x52034    | Log Analysis API Environment Create Failed                   | ALA_InitializeAPI                                            |
| 0x52035    | Log Analysis API Environment Remove Failed                   | ALA_DestroyAPI                                               |
| 0x52000    | Log Manager Initialization Failure                           | ALA_EnableLogging                                            |
| 0x52001    | Log File Open Failure                                        | ALA_EnableLogging                                            |
| 0x52004    | Log Manager Lock Failure                                     | All Log Analysis API functions                               |
| 0x52005    | Log Manager Unlock Failure                                   | All Log Analysis API functions                               |
| 0x52003    | Log Manager Remove Failure                                   | ALA_DisableLogging                                           |
| 0x52002    | Log File Close Failure                                       | ALA_DisableLogging                                           |
| 0x52009    | Not an active transaction                                    | ALA_GetXLog                                                  |
| 0x5200E    | The linked list is not empty                                 | ALA_Handshake<br />ALA_DestroyXLogCollector                  |
| 0x52033    | XLog Pool is emepty                                          | ALA_ReceiveXLog                                              |
| 0x5200F    | NULL Parameter                                               | All Log Analysis API functions                               |
| 0x5201D    | Invalid Parameter                                            | All Log Analysis API functions                               |
| 0x52014    | Network Timeout (can be retried)                             | ALA_ReceiveXLog                                              |
| 0x52026    | A socket type that is not supported.                         | ALA_Handshake                                                |
| 0x52025    | A socket type that is not supported.                         | ALA_Handshake                                                |
| 0x5202F    | The socket type does not support the corresponding Log Analysis API. | ALA_AddAuthInfo<br />ALA_RemoveAuthInfo                      |
| 0x5202D    | The XLog Sender name is different.                           | ALA_Handshake                                                |
| 0x52030    | There is only one piece of authentication information available. | ALA_RemoveAuthInfo                                           |
| 0x52031    | No more authentication information can be added.             | ALA_AddAuthInfo                                              |
| 0x52032    | There is no authentication information available for a peer. | ALA_Handshake                                                |
| 0x52010    | Invalid Role                                                 | ALA_Handshake                                                |
| 0x52011    | Invalid Replication Flags                                    | ALA_Handshake                                                |
| 0x52007    | Geometry Endian Conversion Failure                           | ALA_GetXLog                                                  |
| 0x52036    | Unable to obtain the MTD module.                             | ALA_GetXLog<br />ALA_GetAltibaseText<br />ALA_GetAltibaseSQL |
| 0x52037    | Failed to create text with the MTD module.                   | ALA_GetAltibaseText                                          |
| 0x52038    | CMT Initialization Failure                                   | ALA_GetODBCCValue                                            |
| 0x52039    | CMT End Failure                                              | ALA_GetODBCCValue                                            |
| 0x5203A    | Analysis Header Create Failed (ODBC Conversion)              | ALA_GetODBCCValue                                            |
| 0x5203B    | Analysis Header Remove Failure (ODBC Conversion)             | ALA_GetODBCCValue                                            |
| 0x5203C    | Failed to convert from MT to CMT.                            | ALA_GetODBCCValue                                            |
| 0x5203D    | Failed to convert from CMT to ulnColumn.                     | ALA_GetODBCCValue                                            |
| 0x5203E    | Failed to convert from ulnColumn to ODBC C.                  | ALA_GetODBCCValue                                            |



