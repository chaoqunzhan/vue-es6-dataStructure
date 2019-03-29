**UDP 和TCP的区别：**
|  |  UDP|TCP  |
|--|--|--|
|  是否连接|  面向非连接|面向连接（三握事挥）|
|传输可靠性|不可靠|可靠|
|应用场合|传输少量数据|大量数据|
|速度|快|慢|

UDP:直接发送数据包，服务端接收到就返回一个确认；
TCP:经过三次握手和四次挥手的过程，SYN：同步序列编号（Synchronize Sequence Numbers）

**补充：**
网络传输可分为三个层次：网络层、传输层和应用层。  
网络层：有IP协议、ICMP协议、ARP协议、RARP协议和BOOTP协议。  
传输层：有TCP协议与UDP协议。  
应用层：有FTP、HTTP、TELNET、SMTP、DNS等协议。

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEwNDI0NDkxN119
-->