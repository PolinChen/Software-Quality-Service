## sample for netfow with ELK

## [NetFlow Cookbook](https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1)


## [NetFlow](http://blog.xuite.net/vulcan.lee/it/3398786-NetFlow與網管之關係與應用)

### NetFlow 簡介

　　網路活動的相關資訊可以幫助網管人員瞭解所管轄的網路發生了什麼事、如何發生以及是什麼原因造成的，未來如何預防同樣的事情再次發生，並在網路安全事件發生時提供資訊，協助管理者找出攻擊來源或中毒主機。傳統上網路管理者通常透過SNMP (Simple Network Management Protocol) 協定的工具從支援SNMP的網路設施蒐集網路流量數據，雖然透過此種方式取得資訊不會造成處理上過重的負擔，但是SNMP如同其名提供的只是粗糙、簡略的資料。對網管人員而言，SNMP的資訊只能提供有關網路流量的等級及變化，例如不正常的流量增加，但是要讓網管人員能夠判斷所管轄的網路是否有問題並進一步找出問題所在，則需要更詳細的網路活動相關資訊。

　　在這方面SNMP所提供的資訊只能讓管理者發現問題，卻無法進一步解決問題。因此有另外一種技術的發展，希望能提供更詳細的網路資訊，以協助管理者解決問題，所以封包監聽器(packet sniffer) 或是類似的網路封包監聽工具開始被用來部署在網路設備上捕捉流過的封包並將封包加以解碼，找出封包標頭中欄位的相關資訊，並進一步分析其內容以取得更詳細的資訊。 

> 雖然透過封包監聽工具可以取得更詳細的網路資訊，但因為封包監聽工具通常專注在單一網路封包的內容，而不是整體網路活動的狀態，所以網路管理者很難從封包監聽工具所提供的資訊來掌握整體網路的狀態。此外分析封包非常耗費時間和運算資源，而且封包監聽所儲存的資料量也非常驚人，對於資源非常珍貴的路由器等網路設備，除了執行路由運算外還要額外付出資源在分析封包上，這樣的限制在某些環境中要進行封包監聽是不可行的，例如大型繁忙的網路，甚至可能因為網路太過繁忙而導致系統崩潰，因此封包監聽工具通常用來偵測特定事件而不是監控整個網路的運作狀況。

　　因此對網管人員而言，他們需要一套能夠協助其有效掌握整體網路資訊的工具，在此需求下，NetFlow便成為了近來網管人員中相當熱門的工具，因為NetFlow能提供比SNMP更詳細的網路狀況資訊來協助管理者掌握整個網路，而且NetFlow的分析方式也能來避免造成網路頻寬及運算資源過重的負擔。

![netf1](http://www.ren-isac.net/img/googlecode/FlowHierarchy.jpg)

### NetFlow 運作機制：


　　NetFlow 是由 Cisco公司的Darren Kerr 和Barry Bruins 在1996年發展的一套網路流量監測技術，NerFlow 目前已內建在大部分 Cisco 路由器上，同時Juniper、Ex-treme 等其他網路設備供應商也支援NetFlow 技術，使其逐漸成為大家都能接受的標準。

![netf2](http://www.ren-isac.net/img/googlecode/MotivatingExamplev2.jpg)

　　NetFlow本身主要是一套網路流量統計協定，其背後主要的原理是根據網路封包傳輸時，連續相鄰的封包通常是往相同目的地IP 位址傳送的特性，配合cache快取機制，當網路管理者開啟路由器介面的NetFlow功能時，路由器介面會在接受當網路封包時分析其封包的標頭部分來取得流量資料，並將所接到的封包流量的資訊彙整成一筆一筆的Flow，在NetFlow協定中Flow是被定義為兩端點間單一方向連續的封包流，這意味著每一個網路的連結都會被分別紀錄成兩筆 Flow 資料，其中一筆記錄從客戶端連到伺服器端，另外隨著一筆紀錄從伺服器端連回到客戶端的資訊。

　　路由器透過以下欄位來加以區分每一筆 Flow：來源 IP 位址(source IP address)、 來源埠號(source port number)、目的IP 位址(destination IP address)、目的埠號 (destination port number)、協定種類(protocol type)、服務種類(type of service) 、及路由器輸入介面(router input interface)，任何時間當路由器接受到新的封包時， 路由器會檢視這七個欄位來判斷這個封包是否屬於任何已記錄的Flow，有的話則將新收集到的封包的相關流量資訊整合到對應的Flow記錄中，如果找不到封包對應的Flow 記錄，便產生一個新的 Flow 記錄來儲存相關的流量資訊。由於路由器內快取記憶體的空間有限，無法無限制的容納持續增加的 Flow 紀錄，所以 NetFlow 協定也定義了終結 Flow 記錄的機制，來維持網路設備中儲存 Flow 資訊的空間。

　　雖然大部分的網路硬體供應商都有支援 NetFlow，但是各個廠商分別實作自己版本的 NetFlow，其中 NetFlow Version 5 是常見的 Netflow 資料格式，包含以下幾個欄位：

欄位名稱 (說明)
```
- Source IP Address (來源主機之 IP 位址)
- Destination IP Address (目的主機之 IP 位址)
- Source TCP/UDP Port (來源主機所使用的埠號)
- Destination TCP/UDP Port (目的主機所使用的埠號)
- Next Hop Address (下一個端點的位址)
- Source AS Number (來源主機所屬的 AS 編號)
- Destination AS number (目的主機所屬的 AS 編號)
- Source Prefix Mask (來源主機所屬網域的子網路遮罩)
- Destination Prefix Mask (目的主機所屬網路的子網路遮罩)
- Protocol (使用之通訊協定)
- TCP Flag (封包控制旗標)
- Type of Service (QoS需求參數)
- Start sysUpTime (起始時間)
- End sysUpTime (終止時間)
- Input ifIndex (資訊流流入介面編號)
- Output ifindex (資訊流流出介面編號)
- Packet Count (封包數量)
- Byte Count (Byte數量)
```


### NetFlow在網路安全上相關的應用：

　　由於 NetFlow 只有單純分析封包的標頭，因此透過 NetFlow 所得到的資訊來監控網路異常的行為，所以 NetFlow 的紀錄只包含了流量的相關資訊而沒有網路階層架構中較高層的資訊，但是 NetFlow 仍然能夠提供足夠的資訊來協助網路管理者掌握所管轄網路中異常的網路行為，而且由於 NetFlow 並未對封包內容進行分析，減輕網路設備運算處理的負擔，所以 NetFlow 的效率會比傳統的方式更好，這樣讓 NetFlow 很適合來分析高速、忙碌的網路環境。而且由於 NetFlow 資料來源是網路中的核心的元件-路由器，所以透過從路由器所蒐集到的 NetFlow 資訊可以協助掌握整體網路的情況，而且透過適當的分析 NetFlow 資訊，可以協助管理者在蠕蟲爆發或不正常網路行為的初期快速的偵測出網路的問題。

### 從網路層的角度進行分析：

　　一般來說，網路攻擊行為會存在著某些可供辨識的特徵，例如針對某個特定埠或利用某些特定網路的 IP 位址，我們可以透過這些特徵來與所獲得的 NetFlow 資料進行比對，進而找出可能的異常行為，我們可以透過分析 NetFlow 資料中目的主機所使用埠號欄位，例如 SQL Slammer 就是利用 1234 port 進行感染，而 W32.Sasser 則是利用 445port，我們可以利用目的主機所使用埠號這個欄位等於某個特定埠號，來過濾 NetFlow資料找出相對應的攻擊，另外我們也可以利用不合邏輯的來源或目的 IP 位址來找出異常，例如從我們所管轄網路對外流出的流量中如果來源 IP 位址不是我們所管轄網路的 IP 位址或是從外部網路流進我們所管轄網路的流量中如果來源 IP 位址是我們所管轄網路的 IP 位址

   此外網際網路位址指派機構 (Internet Assigned Numbers Authority , IANA) 將下列三段 IP 位址保留給私有網路使用 10.0.0.0 ~ 10.255.255.255、172.16.0.0 ~ 172.31.255.255 及 192.168.0.0 ~ 192.168.255.255，這幾段網路的位址不能出現在外在網路環境中，但由於當初網路設計的缺陷，路由器對於所接收封包的來源位址欄位並不會進行驗證，所以攻擊者可利用這個缺陷偽造來源 IP 位址 (IP Spoofing )來發動攻擊，避免被追蹤到攻擊來源，所以我們可以比對我們所接受到 NetFlow 資料中來源主機所使用的 IP 位址(Source IP Address)欄位，找出偽造來源位址的流量，再利用 NetFlow 資料中資訊流流入介面編號(Input IFindex)欄位的資訊，找出連接這個介面的上游路由器，請他們協助調查或是處理。

　　某些異常行為可能會連到某個或某些特定位址，而像在 2001 年造成嚴重網路擁塞的 Code Red 蠕蟲，我們分析所收集到的 NetFlow 資料便可發現，此蠕蟲的攻擊行為有一個特性，每筆 Flow 的 destination TCP/UDP port 欄位值會等於 80，Packet Count 欄位值等於 3，Byte Count 欄位值等於 144bytes，網路管理者可以撰寫程式分析所蒐集的NetFlow 資料，找出具此特徵的 Flow 資料，便可找出管轄網路內有可能感染 Code Red 蠕蟲的主機，來進行下線或封鎖以降低蠕蟲造成的危害。利用已收集到攻擊的特徵與 NetFlow 資訊中的相關欄位進行比對找出可能的攻擊，可以在攻擊造成網路嚴重傷害之前，做適當的反應措施來降低形成嚴重問題的可能性。

### 從傳輸層的角度進行分析：

　　我們可以透過 NetFlow 資料找出網路中建立 session 數目最多的主機，因為如果一台主機對特定主機產生不正常的大量連結需求，這可能代表著新的蠕蟲、阻斷服務攻擊、網路掃瞄等的可能性，因為一個正常的主機對外連結會有一定正常的頻率，如果正常的主機感染了蠕蟲，就可能會開始產生異常的網路行為，開始產生對外大量的連結需求來找尋下一個感染的對象，因此我們可以從感染蠕蟲的主機的 NetFlow 資訊中發現到大量的對外連結需求，例如 SQL slammer 我們就可以從感染的主機上發現大量對外 1434 port 的連結需求。
    同樣的原理，如果所管轄網路中的使用者從網路上下載阻斷服務攻擊之工具程式企圖對外發動攻擊時，或是使用者利用 Nmap 之類的掃瞄工具掃瞄特定網址，以找出目標主機所可能存在弱點或是漏洞時，我們都可以從 NetFlow 資料中發現從網域中某個特定位址送出的大量 session。除了偵測網路的攻擊外，我們也可以透過分析 session 的方式找出網路濫用的行為，例如分析 NetFlow 資料中目的主機所使用埠號的資訊，透過分析對外 25 port 連結的相關資訊，若某一台主機對外 25 port 連結的數目在某個特定時間內超出正常值過多，我們便可合理懷疑這台主機被利用來散發廣告信或透過 e-mail感染蠕蟲，同樣原理我們也可以應用來分析像 emule 等 peer-to-peer 檔案分享軟體常用之 TCP 4662 / UDP 4672 port，找出網路濫用的行為，並進行適當處置以降低其所造成的傷害。

### 利用 TCP 的控制旗標過濾出可疑的 Flow：

　　但對於一些大型網路，攻擊的相關 NetFlow 資訊可能會被其他正常的 NetFlow 資訊所稀釋，例如感染病毒的初期或是謹慎的駭客，可能會利用正常的流量來掩護其異常行為，所以我們在找出建立最多 session 或傳輸最大量的搜尋 NetFlow 資訊中尋異常流量時，可能無法發現真正異常的網路流量 NetFlow。另外，當我們遇到新的攻擊手法或是病毒時，可能無法在第一時間掌握其 Flow 特徵，也無法透過特徵比對的方式找出異常流量。為了更快速有效的偵測出異常的流量，我們試著對 TCP 的控制旗標進行分析，希望縮小需要進一步分析的 NetFlow 資料量，以及早發現異常流量。對蠕蟲而言，由於複製散佈的本質，所以主機在感染任何一種蠕蟲後，都會盡力的找尋可能的受害主機，來進行感染散佈，因此一般來說，蠕蟲會在很短的時間內盡全力探測可能的感染目標，而且大部分的蠕蟲都是透過 TCP 協定來傳輸散佈，所以我們可以從 TCP 的控制旗標中發現到一些蛛絲馬跡，做為我們縮小可疑名單的根據。

　　以正常的 TCP 連結建立過程而言，用戶端會先送出一個 SYN 封包給目的端主機，接著目的端主機會回應一個 SYN/ACK 封包，用戶端在接收到這樣的封包後，再送回給目的端主機 ACK 封包完成連結的建立，但由於蠕蟲通嘗試透過內建的名單或是隨機產生感染的目標，所以並不是每一次都能順利的建立連結建立，由於 NetFlow 會將每個 session 中所有傳輸時的 TCP 控制旗標全部儲存在封包控制旗標 (TCP Flag) 這個欄位中，因此我們可以透過這個欄位中的資訊來協助我們推測特定主機連線的特性，若某個 Flow 正常的建立 TCP 連結後，其封包控制旗標 (TCP Flag) 欄位會記錄的包含 ACK、SYN、FIN 等控制旗標，但是如果蠕蟲進行感染的動作時，由於隨機選取的主機並不一定存在，或是即使存在但目標主機沒有開放蠕蟲所要感染的 TCP port，在這種情況下，NetFlow 資訊中由受感染主機對外連線所產生的 Flow 封包控制旗標(TCP Flag) 欄位會只存在 SYN 這個TCP 控制旗標，根據這種特性網路管理者可以先將其 NetFlow 資料中封包控制旗標 (TCP Flag)欄位只有存在 SYN 控制旗標的 Flow 資料過濾出來，透過這種方式我們可以把大部分正常的流量排除，這時候我們要從可疑的資料中找出真正異常流量的難度就會降低許多，能快速的找出問題，也可以避免運算資源無謂的浪費。

### 利用ICMP的訊息協助過濾出可疑的Flow：

　　某些蠕蟲或網路攻擊也會利用 ICMP 來進行，例如 W32.Welchia、W32.Blaster 都會利用 ICMP 來掃瞄其他電腦，所以感染這類型蠕蟲的主機會對外送出大量的 ICMP 封包，也會伴隨大量的 ICMP port/host/network unreachable 回應訊息，我們可以從 NetFlow 資料中過濾出有異常行為的主機，首先找出通訊協定 (protocol) 欄位值為 1 的 Flow，代表所使用的通訊協定為 ICMP，再根據目的主機之埠號 (destination TCP/UDP port)欄位值分析出所代表的 ICMP 訊息，例如目的主機之埠號 (destination TCP/UDP port) 欄位值為 2048，轉化成八進位為 800，第一位代表位數字代表的是 ICMP 的類型，後兩碼為這個 ICMP 類型中的編碼，整體的意思是 ICMP echo 請求，但如果欄位值為 769，轉化為八進位則為 301，這個編碼代表得是 ICMP host unreachable，如果欄位值是 771則代表 ICMP port unreachable，欄位值是 768 則代表 ICMP network unreachable，我們可以先找出所使用通訊協定為 ICMP 的 Flow，進一步過濾出其中目的主機所使用埠號為 768、769、771 的 Flow，再進一步分析找出可能的異常行為。透過這種方式從大量 NetFlow 資料中過濾出可疑名單，再對名單內的 Flow 資料進行進一步的分析，這樣可以幫助網路管理者快速地在異常發生的初期找出問題所在。


##  利用ELK 建構網路的大資料的收集和分析

> 網路管理者面對的網路使用記錄數量繁多且格式各異，若能建立一個具規模彈性（scalable）的雲端巨量資料收集分析架構，便可集中收集分析各種網路、資安、伺服器設備的使用記錄，並進而交叉比對找出異常使用，不僅減輕管理者分析網路資料的負荷，也可以加強整合分析各式網路使用記錄的深度及廣度。ELK stack 可隨時橫向擴充處理能力，不受限於傳統資料庫擴充的限制，對於需要處理大量資料且持續增加的網路管理者而言，是非常適合的工具。

###  利用ELK 建構網路的大資料的系統架構

系統架構圖

![big1](http://163.28.17.129/css/images/slider5.png)

網路管理者在維護管理各式各樣網路設備、資安設備、及重要伺服器時，最即時且真實反映網路或服務狀況的資料來源，就是設備產生的各種記錄，例如：

- 網路路由器/交換器的syslog，netflow，以及經過的所有封包
- 網頁防火牆等資安設備對於經過的成功或失敗連線session的記錄
- VPN設備記錄所有成功失敗的帳號登入記錄及來源IP位址等相關資訊
- DNS伺服器記錄所有domain name查詢，可藉此判斷對惡意網域查詢的中毒電腦
- 郵件伺服器記錄所有信件進出記錄，可藉此及早察覺異常寄信或帳號被盜用情況
- 利用DPI技術處理經過網路設備的封包，擷取出網路流量最真實完整的參數，進行更深入的特性分析，及早察覺可能的異常流量

##  利用ELK 實施的網路監控和分析範例

- Network activity monitoring 
![map8](http://1.bp.blogspot.com/-Exgc70P0_L4/Ux456wPJjyI/AAAAAAAABl8/sgm3XdRkfbY/s1600/Kibana+3+++NetFlow2.png)

- IP traffic  monitoring 
![map9](http://4.bp.blogspot.com/-Kb-j6xgpW78/Ux5CnWVGKCI/AAAAAAAABms/oL8vq5hbH-4/s1600/histogram3.png)

- realtime dashboard
![map4](http://i.imgur.com/lydtCwn.png)

- Dashboard for tracks spikes in suspicious traffic, such as DNS, SYN floods, NTP, or any other UDP spikes.

![map2](https://developer.files.wordpress.com/2016/01/ddos-dash.png?w=898&h=449)

- Spike in UDP traffic.  

![map3](https://developer.files.wordpress.com/2016/01/dash-zoomed.png?w=780)

- Denial of Service Attack
![map5](http://i.imgur.com/plyVY1g.png)

- Attempted DNS DDoS Participation
![map6](http://i.imgur.com/AQmrdqH.png)

- VOIP provider accidentally routed all voice traffic into our network
![map7](http://i.imgur.com/ikBKGi8.png)


## [NetFlow Analysis using ElasticSearch & Kibana](http://www.bulutsal.com/2014/03/netflow-analysis-using-elasticsearch.html)

![map8](http://1.bp.blogspot.com/-Exgc70P0_L4/Ux456wPJjyI/AAAAAAAABl8/sgm3XdRkfbY/s1600/Kibana+3+++NetFlow2.png)


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

