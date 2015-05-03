based on jrtsplib , modified some code to adapat vlc server

关于RTSP的背景知识：《Specifications for a simple RTSP client》
jrtsplib：JAVA下的最小RTSP协议库实现，google code:http://code.google.com/p/rtsplib-java/

以下列出jrtsplib在移植过程中遇到的问题：
1、 修改位置：“RtcpClient.java->dataReceived(Transport, byte[.md](.md), int){...}”
> 原因：在发送DESCRIBE后，得到VLC Server的响应，包含SDP消息，但是通过wireshark察看到，却是malformedpacket,包不知为何被分开了，因此，原有的基础上需要修改，加入分包的判断，获取SDP消息内容。
2、修改位置：“MessageBuffer.java”，改变了byte数组存储方式，使用arraylist动态存储
> 原因：原先的代码会因为1中分包导致前一段包的数据被放在后面，不便于操作，同时，修改后，更加简洁
3、修改位置：在Header文件夹中添加"RangerHeader.java"
> 原因：缺少该部分header
4、修改位置：“RtspMessageFactory.java->incomingMessage(){}”
> 原因：见1
5、修改位置：“我的RTSP实现文件rtsp\_test.java”
> 原因：在SETUP的响应消息中，未加入对server\_port的获取。
6、修改位置："rtspclient.java"
> 原因：接受到SETUP的session的head中，包含了timeout字段，jrtsp会将timeout字段也包含在下一次发出的session中，导致vlc虽然返回200OK，但是没有数据流发送过来



RTSP background: "Specifications for a simple RTSP client"
jrtsplib: JAVA minimum RTSP protocol library google code: http://code.google.com/p/rtsplib-java/

The following lists jrtsplib problems encountered in the porting process:
1, modify the position: "RtcpClient.java-> dataReceived (Transport, byte [.md](.md), int) {...}"
     
 Reason: VLC Server response after sending DESCRIBE contains SDP message, but through the the wireshark back to, malformedpacket, the package I do not know why apart, therefore, need to be modified based on the original, adding the judgment of the subcontracting Get the SDP message content.

2, modify the position: "MessageBuffer.java change the byte array is stored to use arraylist dynamic storage
    
Reasons: the original code because 1 in sub-section of the package in the lead before data is placed at the back, and not easy to operate, at the same time, to modify, more concise

3 Modify Location: Add in the Header folder "RangerHeader.java"
    
Reasons: the part of the header is missing

4, modify the position: "RtspMessageFactory.java-> incomingMessage () {}"
    
Reason: see 1

5, modify the position: "My RTSP implementation file rtsp\_test.java"
    
Reason: SETUP response message, not added to acquisition of server\_port.

6, modify the position: "rtspclient.java"
    
Reasons: in the head received SETUP the session that contains the timeout field jrtsp will timeout field is also included in the next issue of the session, although vlc return 200OK, but not the data stream sent