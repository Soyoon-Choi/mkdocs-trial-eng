# Appendix A. Installation Prerequisites

### Setting User Resource Limit Values

User resource limit values can be confirmed or changed with the OS command, "ulimit".

-   File Size  
    The maximum file size treatable by the process

-   Data Segment Size  
    The maximum size of logical memory the process can use (vsz field).

-   Max Memory Size  
    The maximum size of physical memory the process can use (RSS field)

-   Open Files (descriptor)  
    The Maximum number of files and sockets simultaneously accessible by the process.

-   Stack size
    The maximum size of the stack

-   Virtual memory  
    The maximum size of virtual memory usable by the process. 

Unix users are recommended to set the resource limit values of a user's account to "unlimited" (caution is required that the core file size is not set to "unlimited"). If the Altibase server crashes and dumps the core, it will store every memory database as a core file, so setting it to unlimited may cause disk shortage. Altibase client products must have a stack size of at least 70KB.

### Setting Kernel Parameters for Different Operating Systems (OS)

System Kernel parameter values can be confirmed or changed with a utility provided for each operating system. 

System kernel parameters can be classified into the following: 

-   Semaphore  
    Semaphore setting for IPC connection

-   File-cache  
    Settings for the prevention of memory insufficiency due to the operating system's file cache.

-   Other Settings

#### HP-UX

##### Setting Kernel Parameters on HP-UX

-   HP 11.31 or above: Kernel parameters can be set with the /usr/sbin/kcture utility.

##### Recommended Values

<table>
	<tr>
		<td>Classification</td>
		<td>Parameter Nam</td>
		<td>Recommended Value(bytes)</td>
	</tr>
	<tr>
		<td rowspan="3">Shared Memory</td>
		<td>shmmax</td>
		<td>2G+1</td>
	</tr>
	<tr>
		<td>shmmin</td>
		<td>500</td>
	</tr>
	<tr>
		<td>shmseg</td>
		<td>200</td>
	</tr>
	<tr>
		<td rowspan="9">Semaphore</td>
		<td>semmns</td>
		<td>8192</td>
	</tr>
	<tr>
		<td>semmns</td>
		<td>8192</td>
	</tr>
	<tr>
		<td>semmni</td>
		<td>5029</td>
	</tr>
	<tr>
		<td>semmsl</td>
		<td>2000</td>
	</tr>
	<tr>
		<td>semmap</td>
		<td>5024</td>
	</tr>
	<tr>
		<td>semmnu</td>
		<td>1024</td>
	</tr>
	<tr>
		<td>semopm</td>
		<td>512</td>
	</tr>
	<tr>
		<td>semume</td>
		<td>512</td>
	</tr>
	<tr>
		<td>semvmx</td>
		<td>32767</td>
	</tr>
	<tr>
		<td rowspan="2">File-Cache(HP recommends 20% for 8G or less, 10% for 8G or more)</td>
		<td>dbc_min_pct</td>
		<td>5%</td>
	</tr>
	<tr>
		<td>dbc_max_pct</td>
		<td>5~20%</td>
	</tr>
	<tr>
		<td rowspan="6">Others</td>
		<td>maxdsiz</td>
		<td>2GB</td>
	</tr>
	<tr>
		<td>maxdsiz_64bit</td>
		<td>The amount of memory which is predicted that Altibase will used</td>
	</tr>
	<tr>
		<td>max_thread_proc</td>
		<td>600 or more</td>
	</tr>
	<tr>
		<td>maxfiles</td>
		<td>2048 or more</td>
	</tr>
	<tr>
		<td>nproc</td>
		<td>6142</td>
	</tr>
	<tr>
		<td>maxusers</td>
		<td>124</td>
	</tr>
</table>

#### AIX

##### How to set

Kernel parameters can be set with the /usr/bin/smit utility

##### Recommended Values

The same values for shared memory and semaphore on HP-UX are also recommended for AIX.

###### Setting File-Cache

Depending on the file caching policy for AIX, the file system can swap-out memory from the application program heap, although the system has free memory, and use it as file-cache (this is called stealing).

For AIX 5.2 or later, kernel parameters can be set as below to prevent the system from stealing: 

```
minperm =  5%
lru_file_repage = 0 (AIX 5.2 ML4 or higher)
strict_maxclient = 0
```

###### Configuration of Posix AIO

AIX provides the Posix A/O interfaces for improved disk I/O improvement which must be manually activated. However, from AIX 6.1 and higher, the interfaces are activated by default.

Posix AIO can be activated as below:

-   Change "Configure Defined Asynchronous I/O" to Available through smit. 

-   Change "STATE to be configured at system restarted" to Available through smit. 

This Installation and startup of Altibase are impossible on AIX if the system is not set as above. 

#### LINUX

##### Setting Kernel Parameters on Linux

Kernel parameters can be set in the sem,shmmax, shmmni, swapiness files at the /proc/sys/kernel path. In Redhat 7.2 and later versions check the value of RemoveIPC in /etc/system/login.conf . 

##### Recommended Values

The same values for shared memory and semaphore on HP-UX are also recommended for Linux

However, sessions using the IPC connection can be abruptly cut off, if the Linux kernel version is earlier than 2.5. 

To set kernel parameters automatically when the server boots, add the following to the /etc/rc.d/rc/local file. 

```
/etc/rc.d/rc.local Add the following entry in the file.
echo 2147483648 > /proc/sys/kernel/shmmax
echo 4096 > /proc/sys/kernel/shmmni
echo 200 32000 512 5029 > /proc/sys/kernel/sem
echo 5 > /proc/sys/vm/swappiness
```

###### Setting in RemoveIPC

In RedHat 7.2 and later versions, it is recommended to set the RemoveIPC setting to 'no' (default is 'yes'). If RemoveIPC is set to 'yes', there is a shortage of semaphores and an abnormal termination may occur.

To change the setting, you need to set the RemoveIPC = no in etc/system/logind.conf and restart the OS. 

### Configuration of THP (Transparent Huge Pages)

THP is amemory management system provided by a Linux and its main purpose is to reduce the expenses for viewing TBL (Translation Lookaside Buffer) by increasing the size of memory pages.

However, unlike the original intention, THP causes memory allocation delay and fragmentation which eventually deteriorates the performance in most cases. Thus, THP option should be deactivated in order to use Altibase with its optimized performance. 

It is advised to set the THP option to 'never' in order to run the Altibase operation.

#### Verification method for THP configuration

The available options of THP are always, madvise, and never. Besides, the option inside the square brackets [] is the option currently applied.

-   madvise: This is an option in which THP is activated on a process explicitly requested through the madvise()function.
    
-   always: THP is always applied into all process

-   never: means deactivating THP on all process regardless of requesting  the madvise()function

How to verify for THP Configuration are follows: 

1. Execute the following command indicated below: 

   ```
   $ cat /sys/kernel/mm/transparent_hugepage/enabled
   ```

2. Execute the following command in RedHat Linux:

   ```
   $ cat /sys/kernel/mm/redhat_transparent_hugepage/enabled
   ```

3. Then, the following output is displayed: 

   ```
   $ [always] madvise never
   ```

#### Deactivation method for THP

It is advised to set the HTP option to never in order to run the Altibase operation.

1. Add transparent_hugepage=never to the end of the line kernel boot of /etc/grub.conf with the root account.
   
```
   .....
   kernel /vmlinuz-2.6.32-220.el6.x86_64 ro root=UUID=067b9803-90ca-4875-a018-ff043adde1ed rd_NO_LUKS LANG=ko_KR.UTF-8 rd_NO_MD quiet rhgb crashkernel=128M  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_LVM rd_NO_DM transparent_hugepage=never
   ......
```

2. Reboot the system.
3. Confirm whether the THP option is never or not. 

### Red Hat Enterprise Linux 8

For RHEL 8, symbolic links for libncurses.so.5 and libtinfo.so.5 must be created to run iSQL and iLoader. Starting from Altibase 7.1.0.6.3, the Altibase package installer automatically creates these symbolic links in the $ALTIBASE_HOME/lib directory, so users do not need to create them manually. If the symbolic links for libncurses.so.5 and libtinfo.so.5 are missing, users can create them using the following procedure:

1. Check the ncurses and tinfo library files.

   ```bash
   % ls -l /usr/lib64/| grep -e libncurses.so -e libtinfo.so
   -rw-r--r--   1 root root       31 Jan 16  2019 libncurses.so
   lrwxrwxrwx.  1 root root       17 Jan 16  2019 libncurses.so.6 -> libncurses.so.6.1*
   -rwxr-xr-x.  1 root root   216912 Jan 16  2019 libncurses.so.6.1*                 # ncurses library file
   lrwxrwxrwx   1 root root       13 Jan 16  2019 libtinfo.so -> libtinfo.so.6*
   lrwxrwxrwx.  1 root root       15 Jan 16  2019 libtinfo.so.6 -> libtinfo.so.6.1*
   -rwxr-xr-x.  1 root root   208616 Jan 16  2019 libtinfo.so.6.1*                   # tinfo library file
   ```

2. If libncurses.so.5 and libtinfo.so.5 files do not exist, create symbolic links in the $ALTIBASE_HOME/lib.

   ```bash
   % ln -s /usr/lib64/libncurses.so.6.1 $ALTIBASE_HOME/lib/libncurses.so.5
   % ln -s /usr/lib64/libtinfo.so.6.1 $ALTIBASE_HOME/lib/libtinfo.so.5
   ```

3. Check the created symbolic links.

   ```bash
   % ls -l $ALTIBASE_HOME/lib | grep -e libncurses.so.5 -e libtinfo.so.5
   lrwxrwxrwx   1 user user       17 May  7 16:44 libncurses.so.5 -> /usr/lib64/libncurses.so.6*
   lrwxrwxrwx   1 user user       15 May  7 16:51 libtinfo.so.5 -> /usr/lib64/libtinfo.so.6*
   ```

- If libncurses.so.5 file does not exist, the following error occurs when iSQL is executed.

  ```bash
  % isql
  isql: error while loading shared libraries: libtinfo.so.5: cannot open shared object file: No such file or directory
  ```

  ```bash
  % server create utf8 utf8
  /home/dev02/altibase_home/bin/isql: error while loading shared libraries: libncurses.so.5: cannot open shared object file: No such file or directory
  ```

- In RHEL 8, the ncurses (including tinfo) library version has been changed to 6.1. Altibase requires ncurses 5 version files. 
  The ncurses library guarantees both source-level compatibility (API) and binary compatibility (ABI) from ncurses 5 to ncurses 6.2. 

  Reference : [Announcing ncurses 6.2 (invisible-island.net)](https://invisible-island.net/ncurses/announce.html#h2-release-notes)

### Checking Disk Configuration

Redo log files and data files generally experience disk I/O in Altibase. To minimize performance loss due to disk I/O, we recommend that you segregate redo log files and data files onto a separate physical disk.

### OS Patch

#### Linux
There is a bug in glibc that could cause deadlock due to race conditions such as malloc/free, and it must be patched beyond the patch taht reflects the bug. In this case, it is recommended to be glib patched with glibc-2.12-1.166.el6_7.1 and/or higher version. (Reference: https://bugzilla.redhat.com/show_bug.cgi?id=1244002)

#### AIX

 When using Altibase on AIX, memory usage increases (hearpmin library bug). In this case, C/C++ compilers of the appropriate version must be patched from the IBM Support Portal (http://www-01.ibm.com/support/docview.wss?uid=swg21110831 ).

