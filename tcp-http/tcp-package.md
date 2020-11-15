### TCP粘包以及拆包

  - 粘包与拆包的概念

    * 在socket通信中，如果一端连续发送多条数据，tcp协议会把多条数据进行打包为一个tcp报文发送出去，这个就是粘包；
    * 如果一端发送的数据包超过一次TCP所能传输的最大值时，会将一个数据包拆分为多个最大tcp长度的报文进行分开传输，这就是拆包；
    * 拆包或者粘包只会发生的TCP协议中，因为UDP中有消息保护机制；

  - 一些基础的概念
 
    * MSS
       - 指的是TCP建立连接后，双方约定可以传输的最大的TCP报文长度，是TCP用来限制应用层可发送的最大字节数，
       - 如果底层的MTU为1500byte，那么MSS = 1500 - 20(IPHeader) - 20(TCPheader) = 1460byte

    * MTU
       - 通信协议中最大的传输单元，指的是TCP/IP四层协议中数据链路层的最大传输单元，普遍使用的以太网的MTU是1500;
       
    * 图示
      - ![图片](https://github.com/havenBoy/havenboy-java-Interview/blob/master/image/5.jpg)

    * 出现粘包的原因
      - 发送的数据小于TCP发送缓冲区的大小，TCP将多次写入缓冲区的数据一次发送出去；
      - 接收应用层没有及时读取缓冲区中的数据；
      - 数据发送过快，数据包堆积在缓冲区多个数据后才发送出去；
    * 解决方法
      - 在发送端发送数据之前，将自己要发生的字节流的数据大小让接收端知道，然后接收端设置一个死循环来接受完所有的数据；
      - 在字节流加上自定义固定的报文长度，报文中包含字节流的长度，在接收端接收时，先从缓存取出自定义长度的报头，然后再收取到真实的数据；
      