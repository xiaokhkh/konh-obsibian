Server Software:        BWS/1.1   //被测试的服务器所用的软件信息，这里使用的是百度自己开发的反向代理Baidu Front End,类似nginx。

Server Hostname:        www.baidu.com   //被测主机名

Server Port:            443 //被测主机的服务端口号，一般http请求的默认端口号是80，https默认使用443端口

SSL/TLS Protocol:       TLSv1.2,ECDHE-RSA-AES128-GCM-SHA256,2048,128         //加密协议

Document Path:          /index.html//请求的具体文件(本次请求的是根目录)

Document Length:        227 bytes //请求的文件index.html大小

Concurrency Level:      100 //并发级别，也就是并发数，请求中－c参数指定的数量

Time taken for tests:   3.781 seconds //本次测试总共花费的时间

Complete requests:      1000 //本次测试总共发起的请求数量

Failed requests:        0 //失败的请求数量。因网络原因或服务器性能原因，发起的请求并不一定全部成功，通过该数值和Complete requests相除可以计算请求的失败率，作为测试结果的重要参考。

Total transferred:      1081868 bytes//总共传输的数据量，指的是ab从被测服务器接收到的总数据量，包括index.html的文本内容和请求头信息。

HTML transferred:       22700 bytes //从服务器接收到的index.html文件的总大小，等于Document Length＊Complete requests＝227 bytes＊100＝22700 bytes

Requests per second:    264.50 [#/sec](mean) //平均(mean)每秒完成的请求数：QPS，这是一个平均值，等于Complete requests/Time taken fortests=1000/3.781= 264.50

Time per request:       378.068 [ms] (mean)   //从用户角度看，完成一个请求所需要的时间（因用户数量不止一个，服务器完成100个请求，平均每个用户才接收到一个完整的返回，所以该值是下一项数值的10倍。）

Time per request:       3.781 [ms] (mean, across all concurrent requests)// 服务器完成一个请求的时间。

Transfer rate:         279.45 [Kbytes/sec] received//网络传输速度。对于大文件的请求测试，这个值很容易成为系统瓶颈所在。要确定该值是不是瓶颈，需要了解客户端和被测服务器之间的网络情况，包括网络带宽和网卡速度等信息。
