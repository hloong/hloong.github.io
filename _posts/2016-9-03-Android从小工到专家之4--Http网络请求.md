## HTTP请求原理

HTTP实际上是基于TCP的应用层协议，在更高的层次封装了TCP的使用细节，使网络请求操作更加易用。

TCP协议对应于传输层，而HTTP协议对应于应用层，从本质上来说，二者没有可比性。Http协议是建立在TCP协议基础之上的，当浏览器需要从服务器获取网页数据的时候，会发出一次Http请求。Http会通过TCP建立起一个到服务器的连接通道，当本次请求需要的数据完毕后，Http会立即将TCP连接断开，这个过程是很短的。所以Http连接是一种短连接，是一种无状态的连接。

## HTTP的请求方式
HTTP有7种请求方式：GET，POST,DELETE,PUT,HEAD,TRACE,OPTIONS
### GET，POST之前的区别
![image](https://pic3.zhimg.com/ad46b512903694303423954df74aafd6_r.jpg)

## HTTP与HTTPS

| HTTP协议       | HTTPS协议       |
| ------------ | ------------- |
| HTTP （应用层）   | HTTP （应用层）    |
| -            | TSL或SSL （安全层） |
| TCP （传输层）    | TCP （传输层）     |
| IP （网络层）     | IP （网络层）      |
| 网络接口 （数据链路层） | 网络接口 （数据链路层）  |

