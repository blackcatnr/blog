---
title: OSI七层传输协议模型
date: 2023-09-01
tags: ['星海']
categories: 星海
id: OSI七层传输协议模型
---
<!-- more -->
# OSI七层传输协议模型
# 目录
  - [1. 应用层（Application Layer）](#section.1)<a name="context.1"> </a>
  - [2. 表示层（Presentation Layer）](#section.2)<a name="context.2"> </a>
  - [3. 会话层（Session Layor）](#section.3)<a name="context.3"> </a>
  - [4. 传输层（Transport Layer）](#section.4)<a name="context.4"> </a>
  - [5. 网络层（Network Layer）](#section.5)<a name="context.5"> </a>
  - [6. 数据链路层（Data Link Layer）](#section.6)<a name="context.6"> </a>
  - [7. 物理层（Physical Layer）](#section.7)<a name="context.7"> </a>
------------------------------------------------  

​        ==为了完成不同计算机或网络或架构之间的成功通信，国际标准化组织提出了OSI七层模型，该模型(从上到下)包括了应用层、表示层、会话层、传输层、网络层、数据链路层、物理层==。

<img src="C:\Users\倪瑞\AppData\Roaming\Typora\typora-user-images\image-20230901144204611.png" alt="image-20230901144204611" style="zoom: 80%;" />

​        ==每一层其实际上都是一个协议包==，比如说当我们要提起应用层的时候，并不仅仅指计算机的应用程序（谷歌、火狐等APP），还包含了大量的应用层协议，应用层协议能使应用层程序在网络中够正确的运行，记下来我们一起来看下OSI的七层模型。

### [1. 应用层（Application Layer）](#context.1)<a name="section.1"> </a>

​         应用层是由网络应用程序使用的，离用户最近的一层。应用层通过各种协议（FTP——文件传输协议；HTTP/S——网上冲浪协议；SMTP——邮件传输协议；Telnet——与虚拟端之间的通信协议），为网络应用提供服务（网络应用是指使用会互联网的计算机应用），==**执行用户活动**==。

<img src="https://pic3.zhimg.com/v2-f9bba83371f1c5e1f6dc038121874616_r.jpg" alt="img" style="zoom:80%;" />

### [2. 表示层（Presentation Layer）](#context.2)<a name="section.2"> </a>

表示层从应用层接受数据，这些数据是以字符和数字的形式出现的（如：Chinese、666），表示层将这些字符和数据，转换成机器能够理解的二进制格式（1001 0110），==表示层的这个功能称为“翻译”功能，即把人类的语言翻译成机器能理解的语言==。在传输数据之前，表示层减少了用来表示原始数据的比特数，也就是将原始数据进行了压缩，数据压缩减少了数据原始数据所需的空间，随着文件大小的减少，它就可以在很短的时间内到达目的地，数据压缩对实时视频和音频传输有很大的帮助，以保持完整性的数据传输前的数据加密。

<img src="https://pic1.zhimg.com/v2-cf8307be4afee49b6bcb661c3494ffc8_r.jpg" alt="img" style="zoom:80%;" />

与此同时，==在发送端，数据会在表示层被加密，然后在接受端，数据再在表示层进行解密操作，保证数据传输的安全型==。

<img src="https://pic3.zhimg.com/v2-c892fedc700269c1109bb35d61317ad2_r.jpg" alt="img" style="zoom:80%;" />

### [3. 会话层（Session Layor）](#context.3)<a name="section.3"> </a>

​         在讲会话层之前，我们首先假设下，如果你计划办一个聚会，为确保每一个活动能顺利进行，则需要建立一个流程（装扮环境、烧菜、清洁、告别）。

​          会话层的情况也是如此，会话层用于建立和管理连接、启用、发送和连接数据，就像你为聚会雇用助手一样，会话层也有自己的助手，也就是各种API或应用程序接口，还有NetBIOS，即网络基本输入输出系统，它允许不同计算机上的应用程序相互通信。

<img src="https://pic3.zhimg.com/v2-1ba9a4d55bc1e256cabc483b2db0792a_r.jpg" alt="img" style="zoom:80%;" />

在客户端与服务器建立会话之前，服务器会执行一项为身份验证的功能，比如你在客户端计算机输入了用户名和密码之后（指向服务器应用程序对于的API），客户端计算机和服务器之间将建立会话或链接。

<img src="https://pic1.zhimg.com/v2-9352a9270c4b8b6b3ccdfe6170a1f8d4_r.jpg" alt="img" style="zoom:80%;" />

​        通过身份验证后，服务器将检查用户授权身份验证和授权，授权是服务器来确定你是否有访问文件权限的过程，如果服务器确认你所登录的账号没有相应的权限，则客户端计算机就会收到一条消息，告诉你：您未被授权访问此页面。

​        ==身份验证和授权这两个功能都由会话层执行，会话层提供正在下载的文件的跟踪==。举个例子，我们上网浏览的网页包含了文本、图片等信息，这些文本和图片作为单独的文件存储在Web服务器上，当你在Web浏览器中请求网站时，你的Web浏览器将建立一个到Web服务器的单独会话，以便分别下载这些文本和图像信息文件。这些文件以数据包的形式被接收，与此同时，会话层也给出了下载下来的那个数据包属于哪个文件的轨迹，以确定是文本文件还是图像文件，并追踪他们收到数据包的位置，最终将文本和图片展示在浏览器中，这个就是会话层的会话管理功能。到这里，我们也知道了，浏览器会执行应用层、表示层和会话层所有的功能。

<img src="https://pic1.zhimg.com/v2-f5b86cebae9d9ca24f670aeccdf0f030_r.jpg" alt="img" style="zoom:80%;" />

==总结一下，会话层有三个方面的功能：会话管理、会话管理和授权==。

<img src="https://pic1.zhimg.com/v2-56fb685108ab139605830dc347ca61d0_r.jpg" alt="img" style="zoom:80%;" />

### [4. 传输层（Transport Layer）](#context.4)<a name="section.4"> </a>

在会话层之下时传输层。传输层通过分段（Segmentation）、流量控制（Flow Control）和差错控制（Error Control）来控制通信的可靠性。

（1）首先在分段中，传输层从会话层接收数据，并将数据分为称为“段”（Segment）的数据单元：

<img src="C:\Users\倪瑞\AppData\Roaming\Typora\typora-user-images\image-20230901145649183.png" alt="image-20230901145649183" style="zoom:50%;" />

​         且每个数据单元（Data Unit）包含一个源端口号、目的端口号和序列号，

<img src="https://pic3.zhimg.com/v2-51462f4237cf8c9a72cf7c0389c8801e_r.jpg" alt="img" style="zoom:80%;" />

这些端口号有助于引导每一个网段执行正确的应用程序。

<img src="https://pic2.zhimg.com/v2-a6ce88ff998a90672721a5143b71d185_r.jpg" alt="img" style="zoom:80%;" />

（2）其次，在流量控制中，传输层可以控制传输的数据量。假如一个移动设备连接到一个服务器，若服务器的最大可以传输100Mbps的数据，而我们移动端设备最大可以处理10Mbps的数据，现在我们正在从服务器下载一个文件，但是服务器开始以50Mbs的速度发送数据，这比手机所能处理的速率要高，所以手机在传输层的帮助下，可以告诉服务器将数据传输速率降低到10Mbps，这样就不会有数据丢失。

<img src="https://pic1.zhimg.com/v2-e06668381b5fc9aefb2c5991648ae3c0_r.jpg" alt="img" style="zoom:80%;" />

（3）最后一个是差错控制。传输层也可用于差错控制，在数据传输的过程中，如果某些数据出现丢失，目标传输层将使用自动重复请求方案，来重新传输丢失或损坏的数据，传输层向每个段添加一个称为“校验和”的位，以找出所接收到的传输层控制协议。

<img src="https://pic3.zhimg.com/v2-3dfb1b1238417dfc39fb35bddc397f7e_r.jpg" alt="img" style="zoom:80%;" />

传输层控制协议有面向连接的传输和面向无连接的传输，面向连接的传输通过TCP来实现的面向无连接的传输是通过UDP来实现的。

<img src="https://pic1.zhimg.com/v2-e24e713962fb63616bb093c44e81517c_r.jpg" alt="img" style="zoom:80%;" />

UDP比TCP更快，因为它不会提供任何关于数据是否真正交付的反馈，而TCP提供反馈，因此丢失的数据可以在TCP中重新传输。在是否接收到所有的数据并不重要的应用场景中，如流媒体电源、歌曲、游戏、IP语音、TFTP、DNS等可以使用UDP协议传输数据；而必须要进行完整的数据传输的场合，例如万维网、电子邮件、FTP等，就需要使用TCP协议。

==总结一下，传输层涉及到分段（Segmentation）、流量控制（Flow Control）、差错控制（Error Control）、面向连接（TCP）和无连接（UDP）的传输==。

### [5. 网络层（Network Layer）](#context.5)<a name="section.5"> </a>

传输层将数据传递到网络层，网络层用于将接收到的数据段从一台计算机传输到不同网络中的另一台计算机，网络层的数据单元称为数据包（Packets），网络层的功能是进行逻辑寻址（Logical Addressing）、路由（Rout）和路径确定（Path Determination）。

<img src="https://pic1.zhimg.com/v2-2b03009d119e245d9923a25ea9b3181c_r.jpg" alt="img" style="zoom:80%;" />

（1）首先是逻辑寻址（Logical Addressing）。在网络层进行的IP寻址称为逻辑寻址，网络中每一台计算机都有一个唯一的IP地址，网络层为每一个分段（Segment）都会被分配发送方和接收方的IP地址，并形成一个IP数据包，分配IP地址是为了确保每一个数据包到达正确的目的地。

<img src="https://pic1.zhimg.com/v2-9c5adadd09422e84b486cff00e1bb7b4_r.jpg" alt="img" style="zoom:80%;" />

（2）路由（Rout）。路由是一种将数据包从源端移动到目的端的方法，该方法建立子啊IPV4 & IPV6基础之上。

<img src="https://pic4.zhimg.com/v2-f0334aaeee4b3e31563a042a975efe3b_r.jpg" alt="img" style="zoom:80%;" />

现假如计算机A与网络1相连，计算机B与网络2相连，我们从B电脑请求访问Facebook网站，现在有了来自计算机B中Facebook服务器的回复，这些回复将以数据包的形式出现，而这个数据包只传送到计算机B，由于网络中每个设备都有一个唯一的IP地址，所以计算机A和计算机B都也有一个唯一的IP地址，而Facebook服务器给计算机B的回复已经在数据包中，且在数据包中已经添加了发送方的IP地址：192.168.3.1和接收方的IP地址192.168.2.1，假设子网掩码使用255.255.255.0，这个掩码表示IP地址的前三组数字代表网络，即计算机B所在的网段（Network 2）就是192.168.2，而IP地址的最后一个数就是计算机的编号，这样我们就可以采用IP地址和掩码的方式在网络层进程数据的传输。

<img src="https://pic4.zhimg.com/v2-13a3b3b4290eee0f277e4d8c9fa55eb3_r.jpg" alt="img" style="zoom:80%;" />

（3）接下来就是路径选择（Path Determination）的问题，计算机可以通过多种方式连接到Internet服务器，从源到目标的数据传递的最佳可能路径称为“路径选择”，关于路径选择的协议有OSPF、边界网关协议（BGP）、中间系统到中间系统协议（IS-IS），以确定数据传递的最佳可能路径。

<img src="https://pic1.zhimg.com/v2-3a8631f027f9810e20c0c5753cec52f8_r.jpg" alt="img" style="zoom:80%;" />

### [6. 数据链路层（Data Link Layer）](#context.6)<a name="section.6"> </a>

==数据链路层从网络层接收数据包，数据包包含了发送方和接受方的IP地址。有两种寻址方式：逻辑寻址和物理寻址==。逻辑寻址在网络层已经完成，即在数据段（Segment）中添加了发送方和接收方的IP地址，以形成IP数据包。而物理寻址就是在数据链路层中完成的，其方法就是在IP数据包中添加发送方计算机和接收方计算机的物理地址：MAC，从而形成一个数据帧。MAC地址是由计算机制造商嵌入到计算机的，也是唯一的，我们的手机也有一个唯一的MAC地址。

<img src="https://pic4.zhimg.com/v2-bec211c8eab3dcf7ea8e8742c46a16c7_r.jpg" alt="img" style="zoom:80%;" />

数据链路层中的数据单元称为“帧”数据，链路层作为计算机的软件网络接口卡嵌入，网卡提供经由本地介质将数据从一台计算机传送到另一台计算机的装置，本地介质包括铜线、光纤或无线电信号。

<img src="https://pic2.zhimg.com/v2-9f6282593f731095c8b01599ba5ad3f1_r.jpg" alt="img" style="zoom:80%;" />

所以，数据链路层执行两个基本功能：1.允许上层使用“帧封装”之类的各种技术访问介质；2.控制如何放置和接收来自介质的数据，并进行错误检测。

有时候可能会有多个设备连接到公共介质，如果两个或多个设备连接到同一个介质，并同时发送数据，那么这些消息可能会发生冲突，从而形成一个收件人无法理解的信息，为了避免这些情况，数据链路层会密切关注，并监视什么时候共享的媒介是空闲的，这样设备就可以为接收端传递正确的数据，这就是所谓的CSMA（载波监听多路访问）技术。

<img src="https://pic1.zhimg.com/v2-08c27e18439ac2e7bdf0f773c9c92c34_r.jpg" alt="img" style="zoom:80%;" />

### [7. 物理层（Physical Layer）](#context.7)<a name="section.7"> </a>

到现在为止，应用的“行为动作”已经通过传输层进行了分割，变成网络层的数据包和数据链路层的数据“帧”，现在是一种二进制序列了，最后在物理层将这些二进制序列转换成信号并在本地介质（铜缆、光纤、无线信号等）上传输，并在目标计算机应用层上显示数据。

<img src="https://pic3.zhimg.com/v2-1969916e8bb505609bcfa87aca5ca5a2_r.jpg" alt="img" style="zoom:80%;" />