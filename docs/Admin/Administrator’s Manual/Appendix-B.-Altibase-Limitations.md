# Appendix B. Altibase Limitations

### Maximum Altibase Values

The following table lists the maximum possible values that various Altibase objects can have. 

<table>
    <tr>
    	<th>Identification</th>
        <th>Maximum Values of Objects</th>
        <th>Remarks</th>
    </tr>
    <tr>
    	<td>DB_NAME Length</td>
        <td>128</td>
        <td>The maximum length of
the database name</td>
    </tr>
     <tr>
    	<td>OBJECT Length</td>
        <td>40</td>
        <td>The maximum length of
database object names</td>
    </tr>
     <tr>
    	<td>The Number of
Tablespaces</td>
        <td>64K (Including System Tablespaces)</td>
        <td>The maximum number
of tablespaces in one DB</td>
    </tr>
     <tr>
    	<td rowspan="2">The Number of Data
Files</td>
        <td>1,024</td>
        <td>The maximum number
of data files in one
tablespace</td>
    </tr>
     <tr>
        <td>67,108,864 (64K * 1024)</td>
        <td>The maximum number
of data files in one DB</td>
    </tr>
     <tr>
    	<td>The Number of Users </td>
        <td>2,147,483,638 (Including System Users) </td>
        <td>The maximum number
of users in one DB</td>
    </tr>
     <tr>
    	<td>The Number of Tables</td>
        <td>2,097,151(Including Meta Tables) </td>
        <td>The maximum number
of tables in one DB</td>
    </tr>
     <tr>
    	<td>The Number of Indexes</td>
        <td>64</td>
        <td>The maximum number
of indexes per table</td>
    </tr>
     <tr>
    	<td rowspan="2">The Number of
Columns</td>
        <td>1,024</td>
        <td>The maximum number
of columns per table</td>
    </tr>
     <tr>
        <td>32</td>
        <td>The maximum number
of key columns per index</td>
    </tr>
     <tr>
    	<td>The Number of Rows </td>
        <td>Unlimited </td>
        <td>The maximum number
of row per table</td>
    </tr>
     <tr>
    	<td>The Number of Partitions</td>
        <td>2,147,483,638 (In the Entire System)</td>
        <td>The maximum number
of partitions in one DB</td>
    </tr>
     <tr>
    	<td>The Number of Constraints</td>
        <td>2,147,483,638 (In the Entire System)</td>
        <td>The maximum number
of constraints in one DB</td>
    </tr>
</table>
