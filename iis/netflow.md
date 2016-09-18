## sample for netfow with ELK

## [NetFlow Cookbook](https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1)

![netf1](http://www.ren-isac.net/img/googlecode/FlowHierarchy.jpg)
![netf2](http://www.ren-isac.net/img/googlecode/MotivatingExamplev2.jpg)

## [NetFlow](http://blog.xuite.net/vulcan.lee/it/3398786-NetFlow與網管之關係與應用)

### NetFlow 簡介

　　網路活動的相關資訊可以幫助網管人員瞭解所管轄的網路發生了什麼事、如何發生以及是什麼原因造成的，未來如何預防同樣的事情再次發生，並在網路安全事件發生時提供資訊，協助管理者找出攻擊來源或中毒主機。傳統上網路管理者通常透過SNMP (Simple Network Management Protocol) 協定的工具從支援SNMP的網路設施蒐集網路流量數據，雖然透過此種方式取得資訊不會造成處理上過重的負擔，但是SNMP如同其名提供的只是粗糙、簡略的資料。對網管人員而言，SNMP的資訊只能提供有關網路流量的等級及變化，例如不正常的流量增加，但是要讓網管人員能夠判斷所管轄的網路是否有問題並進一步找出問題所在，則需要更詳細的網路活動相關資訊。

　　在這方面SNMP所提供的資訊只能讓管理者發現問題，卻無法進一步解決問題。因此有另外一種技術的發展，希望能提供更詳細的網路資訊，以協助管理者解決問題，所以封包監聽器(packet sniffer) 或是類似的網路封包監聽工具開始被用來部署在網路設備上捕捉流過的封包並將封包加以解碼，找出封包標頭中欄位的相關資訊，並進一步分析其內容以取得更詳細的資訊。

    雖然透過封包監聽工具可以取得更詳細的網路資訊，但因為封包監聽工具通常專注在單一網路封包的內容，而不是整體網路活動的狀態，所以網路管理者很難從封包監聽工具所提供的資訊來掌握整體網路的狀態。此外分析封包非常耗費時間和運算資源，而且封包監聽所儲存的資料量也非常驚人，對於資源非常珍貴的路由器等網路設備，除了執行路由運算外還要額外付出資源在分析封包上，這樣的限制在某些環境中要進行封包監聽是不可行的，例如大型繁忙的網路，甚至可能因為網路太過繁忙而導致系統崩潰，因此封包監聽工具通常用來偵測特定事件而不是監控整個網路的運作狀況。

　　因此對網管人員而言，他們需要一套能夠協助其有效掌握整體網路資訊的工具，在此需求下，NetFlow便成為了近來網管人員中相當熱門的工具，因為NetFlow能提供比SNMP更詳細的網路狀況資訊來協助管理者掌握整個網路，而且NetFlow的分析方式也能來避免造成網路頻寬及運算資源過重的負擔。

### NetFlow 運作機制：
　　NetFlow 是由 Cisco公司的Darren Kerr 和Barry Bruins 在1996年發展的一套網路流量監測技術，NerFlow 目前已內建在大部分 Cisco 路由器上，同時Juniper、Ex-treme 等其他網路設備供應商也支援NetFlow 技術，使其逐漸成為大家都能接受的標準。

　　NetFlow本身主要是一套網路流量統計協定，其背後主要的原理是根據網路封包傳輸時，連續相鄰的封包通常是往相同目的地IP 位址傳送的特性，配合cache快取機制，當網路管理者開啟路由器介面的NetFlow功能時，路由器介面會在接受當網路封包時分析其封包的標頭部分來取得流量資料，並將所接到的封包流量的資訊彙整成一筆一筆的Flow，在NetFlow協定中Flow是被定義為兩端點間單一方向連續的封包流，這意味著每一個網路的連結都會被分別紀錄成兩筆 Flow 資料，其中一筆記錄從客戶端連到伺服器端，另外隨著一筆紀錄從伺服器端連回到客戶端的資訊。

　　路由器透過以下欄位來加以區分每一筆 Flow：來源 IP 位址(source IP address)、 來源埠號(source port number)、目的IP 位址(destination IP address)、目的埠號 (destination port number)、協定種類(protocol type)、服務種類(type of service) 、及路由器輸入介面(router input interface)，任何時間當路由器接受到新的封包時， 路由器會檢視這七個欄位來判斷這個封包是否屬於任何已記錄的Flow，有的話則將新收集到的封包的相關流量資訊整合到對應的Flow記錄中，如果找不到封包對應的Flow 記錄，便產生一個新的 Flow 記錄來儲存相關的流量資訊。由於路由器內快取記憶體的空間有限，無法無限制的容納持續增加的 Flow 紀錄，所以 NetFlow 協定也定義了終結 Flow 記錄的機制，來維持網路設備中儲存 Flow 資訊的空間。

　　雖然大部分的網路硬體供應商都有支援 NetFlow，但是各個廠商分別實作自己版本的 NetFlow，其中 NetFlow Version 5 是常見的 Netflow 資料格式，包含以下幾個欄位：

欄位名稱 (說明)
```
1. Source IP Address (來源主機之 IP 位址)
2. Destination IP Address (目的主機之 IP 位址)
3. Source TCP/UDP Port (來源主機所使用的埠號)
4. Destination TCP/UDP Port (目的主機所使用的埠號)
5. Next Hop Address (下一個端點的位址)
6. Source AS Number (來源主機所屬的 AS 編號)
Destination AS number (目的主機所屬的 AS 編號)
Source Prefix Mask (來源主機所屬網域的子網路遮罩)
Destination Prefix Mask (目的主機所屬網路的子網路遮罩)
Protocol (使用之通訊協定)
TCP Flag (封包控制旗標)
Type of Service (QoS需求參數)
Start sysUpTime (起始時間)
End sysUpTime (終止時間)
Input ifIndex (資訊流流入介面編號)
Output ifindex (資訊流流出介面編號)
Packet Count (封包數量)
Byte Count (Byte數量)
```

　　支援 NetFlow 功能的網路設備將其所收集到的 Flow 資訊以 UDP 封包送往預先設置 好的流量接收主機，配合 NetFlow 相關收集軟體，如 Cisco 的 NetFlow，FlowCollector 或公開原始碼軟體 Flow-tools，將這些原始流量資料作適當的處理、儲存以提供後續的 相關應用，在接下來的段落中我們接著介紹這些蒐集到的 Flow 資料有哪些應用。


## Process NetFlow with nProbe and Elasticsearch
- [Part 1:](http://www.secureict.info/2015/11/process-netflow-with-nprobe-and.html)
- [Part 2:](http://www.secureict.info/2015/11/process-netflow-with-nprobe-and_13.html)
- [Part 3:](http://www.secureict.info/2015/11/process-netflow-with-nprobe-and_91.html)

![newflow3](http://1.bp.blogspot.com/-H-MYESxr1AA/Vj3HlQNflJI/AAAAAAABBWs/LzGU4LPA6TM/s640/ELK%2BNetflow%2BProcessing.png)
![newflow1](http://2.bp.blogspot.com/-VFyW5YRv12M/Vkgv9NNp9BI/AAAAAAABBjg/iHkXDZb8-zs/s1600/2015-11-14%2B23_10_41-Discover%2B-%2BKibana.jpg)
![newflow2](http://3.bp.blogspot.com/-1AoD3NHgb9E/VkWicfjrHzI/AAAAAAABBcs/xzssLNlFuOI/s640/netflow01.jpg)

## [Step-by-Step Setup of ELK for NetFlow Analytics](http://blogs.cisco.com/security/step-by-step-setup-of-elk-for-netflow-analytics)

logstash-staticfile-netflow.conf
```
  input {
    udp {
      port => 9995
      codec => netflow {
        definitions => "/home/administrator/logstash-1.4.2/lib/logstash/codecs/netflow/netflow.yaml"
        versions => [5]
      }
    }
  }
```

## [Open Source Flow Collecting with Elastic, Logstash, and Kibana](https://developer.wordpress.com/2016/02/08/open-source-netflow-with-elastic-logstash-kibana/)

- The map is useful to see how anycast routing is performing.
![map1](https://developer.files.wordpress.com/2016/01/mapview.png?w=522&h=329&crop=1)

- setup a Denial of Service dashboard that tracks spikes in suspicious traffic, such as DNS, SYN floods, NTP, or any other UDP spikes.
![map2](https://developer.files.wordpress.com/2016/01/ddos-dash.png?w=898&h=449)

- In this example there was a spike in UDP traffic.  
![map3](https://developer.files.wordpress.com/2016/01/dash-zoomed.png?w=780)


## [ELK For Network Operations](http://operational.io/elk-for-network-operations/)

 Below are some screenshots showing real-time dashboards that would be useful in a NOC environment.
- realtime dashboard
![map4](http://i.imgur.com/lydtCwn.png)

- Denial of Service Attack
![map5](http://i.imgur.com/plyVY1g.png)

- Attempted DNS DDoS Participation
![map6](http://i.imgur.com/AQmrdqH.png)

- VOIP provider accidentally routed all voice traffic into our network
![map7](http://i.imgur.com/ikBKGi8.png)

## [NetFlow Analysis using ElasticSearch & Kibana](http://www.bulutsal.com/2014/03/netflow-analysis-using-elasticsearch.html)

![map8](http://1.bp.blogspot.com/-Exgc70P0_L4/Ux456wPJjyI/AAAAAAAABl8/sgm3XdRkfbY/s1600/Kibana+3+++NetFlow2.png)

```
Netflow captures following fields that can be used to analyze various aspects of the network :
Timestamp
Duration
Protocol
Source IP
Source port
Destination IP
Destination port
Bytes
Packets
TCP Flags
Interface
```

![map9](http://4.bp.blogspot.com/-Kb-j6xgpW78/Ux5CnWVGKCI/AAAAAAAABms/oL8vq5hbH-4/s1600/histogram3.png)
- [github source](https://github.com/bulutsal/networkanalysis)

## [Network Security with Netflow and IPFIX: Big Data Analytics for Information](https://books.google.com.tw/books?id=dySMCgAAQBAJ&pg=PT171&lpg=PT171&dq=netflow+elk&source=bl&ots=t1vWgKnFZV&sig=PYpLiD7Xj77VQMb9hM2Lw0UbA88&hl=zh-TW&sa=X&ved=0ahUKEwiG0b7ciJbPAhUFlJQKHQ-dBoQ4ChDoAQg9MAI#v=onepage&q=netflow%20elk&f=false) 

the whole book is good

## [Big Data Analysis for Network Security](http://163.28.17.129/plan5.html#)

系統架構圖
![big1](http://163.28.17.129/css/images/slider5.png)

### 校園網路管理的”Big data”
網路管理者在維護管理各式各樣網路設備、資安設備、及重要伺服器時，最即時且真實反映網路或服務狀況的資料來源，就是設備產生的各種記錄，例如：
- 網路路由器/交換器的syslog，netflow，以及經過的所有封包
- 網頁防火牆等資安設備對於經過的成功或失敗連線session的記錄
- VPN設備記錄所有成功失敗的帳號登入記錄及來源IP位址等相關資訊
- DNS伺服器記錄所有domain name查詢，可藉此判斷對惡意網域查詢的中毒電腦
- 郵件伺服器記錄所有信件進出記錄，可藉此及早察覺異常寄信或帳號被盜用情況
- 利用DPI技術處理經過網路設備的封包，擷取出網路流量最真實完整的參數，進行更深入的特性分析，及早察覺可能的異常流量

![big2](http://163.28.17.129/css/images/plan5-1.png)

