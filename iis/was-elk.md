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

### logging with ELK

- [Using Docker and ELK to Analyze WebSphere Application Server SystemOut.log](http://www.stoeps.de/using-docker-to-analyze-websphere-application-server-systemout-log/)

 - So now we can start custimizing the view filter for event type, error code and so on. Have fun to start analyzing with ELK.
![was-elk1](http://www.stoeps.de/wp-content/uploads/2016/05/2016-05-28_16-39-36.png)
![was-elk2](http://www.stoeps.de/wp-content/uploads/2016/05/2016-05-28_16-47-58.png)

- [Monitor WebSphere with ELK and Nagios](https://kbild.ch/2016/08/monitor-websphere-with-elk-and-nagios/)

![charts](https://kbild.ch/wp-content/themes/website/data/php/timthumb.php?src=wp-content/uploads/2016/08/ELK-Nagios-Flow.001.jpg&q=90&w=648)

- [ELK hunting](https://www.linkedin.com/pulse/elk-hunting-charek-chen?forceNoSplash=true)
```
input {
file {
path => "/opt/WebSphere/CommerceServer70/instances/demo/httplogs/access_log"
        start_position => beginning
}
}
filter {
     grok {
          match => { 'message' => '\"%{NUMBER:epoch:int}\",\"%{WORD:method}\",\"%{URIPATHPARAM:url}\",\"(%{DATA:queryString})?\",\"%{NUMBER:responseCode:int}\",\"%{NUMBER:bytes:int}\",\"%{NUMBER:duration:int}\",\"%{HOSTPORT:wasserver}\"'}
}
```

![wasA](https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAVUAAAAJDA0MWI1NzMzLTM2ZjMtNDkzNS04NmVkLWZjODJmZWJlOWU1ZA.jpg)
![wasB](https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAWVAAAAJGUyZDk3YWE5LWUxOGYtNGNiNy05M2M3LTdmZTVhNjhmNGVhMw.jpg)


### [IBM WebSphere Application Server 系統日誌檔](http://www.ibm.com/support/knowledgecenter/zh-tw/SSZJPZ_9.1.0/com.ibm.swg.im.iis.productization.iisinfsv.install.doc/topics/wsadmin_was_log_files.html)

WebSphere® Application Server 日誌檔包含可用來監視 WebSphere Application Server 啟動及診斷錯誤的資訊。

下列日誌檔適用於診斷 IBM® InfoSphere® Information Server 問題：
SystemOut.log
傳至 STDOUT 的 WebSphere Application Server 訊息會重新導向至此檔案。
SystemErr.log
傳至 STDERR 的 WebSphere Application Server 訊息會重新導向至此檔案。


### [Log and trace settings]https://www.ibm.com/support/knowledgecenter/en/SSEQTP_8.5.5/com.ibm.websphere.base.doc/ae/utrb_logtrace.html?cp=SSEQTP_8.5.5)

- Diagnostic Trace
The diagnostic trace configuration settings for a server process determine the initial trace state for a server process. The configuration settings are read at server startup and used to configure the trace service. You can also change many of the trace service properties or settings while the server process is running.

- Java virtual machine (JVM) Logs
The JVM logs are created by redirecting the System.out and System.err streams of the JVM to independent log files. WebSphere Application Server writes formatted messages to the System.out stream. In addition, applications and other code can write to these streams using the print() and println() methods defined by the streams.

- Process Logs
WebSphere Application Server processes contain two output streams that are accessible to native code running in the process. These streams are the stdout and stderr streams. Native code, including Java virtual machines (JVM), might write data to these process streams. In addition, JVM provided System.out and System.err streams can be configured to write their data to these streams also.

- IBM Service Logs
The IBM service log contains both the WebSphere Application Server messages that are written to the System.out stream and some special messages that contain extended service information that is normally not of interest, but can be important when analyzing problems. There is one service log for all WebSphere Application Server JVMs on a node, including all application servers.


