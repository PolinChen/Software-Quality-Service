## was log file 

>
IBM Websphere Application Server creates the following log files trace.log ,SystemOut.log , and SystemErr.log , activity.log, StartServer.log , stopServer.log , native_stdout.log , native_stderr.log. Let us see the above log files in details .

### log description

- [was log](http://websphereprof.blogspot.tw/2013/04/ibm-was-log-files-and-its-path.html)
- [was detail](https://www.ibm.com/developerworks/community/files/basic/anonymous/api/library/d966e390-ef28-4941-9aed-b406e45ffbd3/document/04a2df1d-e99f-4613-8859-f19bdcf9781d/media/WAS_Logfiles.pdf) pdf

1. Diagnostic Trace Service logs - Contains all output of System.out and System.err streams of the JVM for the application server process and other details. Can be used for the Diagnostic purpose. The default file used to capture the logs is trace.log . Location (given below) and Name of the file can be changed.

2. Java virtual machine (JVM) logs : Contains Standard JVM output and error log. The JVM logs are created by redirecting the System.out and System.err streams of the JVM to independent log filesSystemOut.log and SystemErr.log respectively. Thease files contain the output of the System.out and System.err streams for the application server process. The data is written by the user program using the statement System.out.println(), System.err.print() and calling a JVM function, such as Exception.printStackTrace(). The System.out JVM log also contains system messages( message events) written by the application server. The default file names (SystemOut.log and SystemErr.log) and location (given below ) can be changed.

3. Process Logs : WAS processes contain two output streams stdout and stderr which are accessible to native code running in the process. Native code, including JVM, might write data to these process streams. In addition, System.out and System.err streams of JVM can be configured to write their data to these streams also. The default files used for writing this logs are native_stdout.log , native_stderr.log

4. IBM Service Logs : The default file used for this logs is activity.log . Maintains a history of activities of the Websphere Application Server. The IBM Service log is maintained in a binary format can be viewed by Log and Trace Analyzer.

5. StartServer.log and stopServer.log : Logs generated during application Server's start &amp; stop process are captured in these log files.

### path 

- linux log path:  /opt/IBM/WebSphere/AppServer/profiles/AppSrv01_nodename/logs/nodename
