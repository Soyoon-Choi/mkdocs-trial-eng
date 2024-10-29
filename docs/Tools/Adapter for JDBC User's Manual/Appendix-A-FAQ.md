# Appendix A: FAQ

#### What do I have to do after modifying environment variables or properties?

If environment variables or properties are modified after jdbcAdapter has been started, jdbcAdapter should be restarted in order to apply the modifications.

#### What happens if data is not propery applied to Altibase DB?

If jdbcAdapter fails to apply data to Altibase DB, only log messages are left and the next data is applied. The log messages are written to a trace log file located in $JDBC_ADAPTER_HOME/trc directory.

