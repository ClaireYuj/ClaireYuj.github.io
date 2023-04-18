---
title: 基于Python实现的PING功能
top: false
cover: false
author: DULULU oO
date: 2021-11-28 21:00:35
password:
summary:
tags: 
    - Python
categories: 计算机网络
---

## 什么是PING 
ping通常用于测试网络连通，再命令行输入ping 目标主机地址， 得到回复如下：

![ping](/img/posts/Ping/ping_cmd.jpg)

其中的数据内容如下：

bytes值：字节数，也就是数据包大小。

time值：响应时间，这个时间越小，说明连接速度越快。

TTL值：Time To Live,表示DNS记录在DNS服务器上存在的时间，它是IP协议包的一个值，告诉路由器该数据包何时需要被丢弃。可以通过Ping返回的TTL值大小，粗略地判断目标系统类型是Windows系列还是UNIX/Linux系列。

默认情况下，Linux系统的TTL值为64或255，WindowsNT/2000/XP系统的TTL值为128，Windows98系统的TTL值为32，UNIX主机的TTL值为255。

因此一般TTL值：

100~130ms之间，Windows系统 ；

240~255ms之间，UNIX/Linux系统。

## ping的步骤

完整代码见：[PING](https://github.com/ClaireYuj/PING-Traceroute/blob/main/ICMPPing.py)

解析主机并，进行多次ping操作，输出结果，判断最终结果并输出四个步骤
ping的代码如下

```Python
def ping(host, timeout=1):
    """
    do the ping use doOnePing method in a while loop, and show the details
    :param host:
    :param timeout:
    :return:
    """
    # 1. Look up hostname, resolving it to an IP address
    # 2. Call doOnePing function, approximately every second
    # 3. Print out the returned delay
    # 4. Continue this process until stopped
    try:
        destinationAddress = socket.gethostbyname(host)
    except Exception as e:
        print(e)
        print(" Error on extract the hostname")
        return
    print(" Ping {0} [{1}] with 32 bytes of data:".format(host, destinationAddress))
    lost = 0
    accept = 0
    timesum = 0.0
    count = 4
    times = []
    ttl = 0

    for i in range(count):

        sequence = i
        delay, ttl = doOnePing(destinationAddress, timeout)
        if delay < 0:
            if delay == -1:
                print(" %s The request is overtime ...... can not receive the icmpPacket " % delay)
                lost += 1
                times.append(delay * 1000)
            elif delay == -3:
                print("3 The destination is unreachable")
            elif delay == -11:
                print("11 Overtime")
            else:
                print("%s failed......." %delay)

        else:
            delay = delay * 1000
            print("reply from {0} : byte=32 seq = {1} time={2:.2f}ms ttl = {3} ".format(destinationAddress, sequence, delay, ttl))
            accept += 1
            timesum += delay
            times.append(delay) # all the time
        time.sleep(1)
    print('packet: send = {0}，received = {1}，loss= {2} ({3}% loss) \n\
	Estimated round trip time: min = {4:.2f}ms，max = {5:.2f}ms，average = {6:.2f}ms'.format(
        count, accept, lost, lost / (lost + accept) * 100, min(times),
        max(times), sum(times) // (lost + accept)
    ))

```

### 进行一次ping

ping命令共输出四次ping的结果，其中一次ping的结果如下
```Python

def doOnePing(destinationAddress, timeout):
    """
    create the ICMP socket, then call the sendOnePing and receiveOnePing method in sequence
    :param destinationAddress:
    :param timeout:
    :return totalDelay, ttl:
    """
    # 1. Create ICMP socket
    icmpSocket = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.getprotobyname("icmp"))
    dataID = os.getpid() & 0xFFFF

    # 2. Call sendOnePing function
    icmpPacket, sendtime = sendOnePing(icmpSocket, destinationAddress, dataID)

    # 3. Call receiveOnePing function
    totalDelay, ttl = receiveOnePing(icmpSocket, destinationAddress, dataID, timeout, sendtime)
    # 4. Close ICMP socket
    icmpSocket.close()

    # 5. Return total network delay
    return totalDelay, 
```

### 发出ping请求

每次ping需要发出请求以及接收请求两个步骤
```Python
  """
    build the icmp header, and pack the checksum in header
    record the time to send packet
    :param icmpSocket:
    :param destinationAddress:
    :param ID:
    :return icmpPacket, sendTime:
    """
def sendOnePing(icmpSocket, destinationAddress, ID):
  
    # 1. Build ICMP header
    ip = socket.gethostbyname(destinationAddress)
    icmpChecksum = 0
    icmpHeader = struct.pack(">BBHHH", ICMP_ECHO_REQUEST, 0, socket.htons(icmpChecksum), ID, 1) #htons - trans the byte sequence of host nto network byte order

    # 2. Checksum ICMP packet using given function
    data = struct.pack(">d", time.time())
    icmpChecksum = checksum(icmpHeader + data)

    # 3. Insert checksum into packet
    # icmpHeader = struct.pack("bbHHh", ICMP_ECHO_REQUEST, 0, socket.htons(icmpChecksum), ID, 1)
    icmpHeader = struct.pack(">BBHHH", ICMP_ECHO_REQUEST, 0, socket.htons(icmpChecksum), ID, 1) # B-unsigned char h -short
    icmpPacket = icmpHeader + data


    # 4. Send packet using socket
    icmpSocket.sendto(icmpPacket, (ip, 80))

    #  5. Record time of sending
    sendTime = time.time()
    return icmpPacket, sendTime

```

checksum是checksum意思是较验和，就是将一段已有长度的数据按照某种整数类型(通常是16位无符号整数)进行累加(进位部分再继续加到低位)，累加后的结果再取反以用于校验。具有校验合的数据包再次进行校验后结果则为0。

```Python
def checksum(string):
    csum = 0
    countTo = (len(string) // 2) * 2
    count = 0

    while count < countTo:
        thisVal = string[count + 1] * 256 + string[count]
        csum = csum + thisVal
        csum = csum & 0xffffffff
        count = count + 2

    if countTo < len(string):
        csum = csum + string[len(string) - 1]
        csum = csum & 0xffffffff

    csum = (csum >> 16) + (csum & 0xffff)
    csum = csum + (csum >> 16)
    answer = ~csum
    answer = answer & 0xffff
    answer = answer >> 8 | (answer << 8 & 0xff00)

    answer = socket.htons(answer)

    return answer
```

### 接收ping请求

```Python
"""
    wait the socket to receive the reply by select.select, calculate the time to receive the packet
    :param icmpSocket:
    :param destinationAddress:
    :param ID:
    :param timeout:
    :param startTime:
    :return delay, ttl:
    """
def receiveOnePing(icmpSocket, destinationAddress, ID, timeout, startTime):
    
    # 1. Wait for the socket to receive a reply
    while True:
        what_ready = select.select([icmpSocket], [], [], timeout)
        receivedTime = time.time()
        # 2. Once received, record time of receipt, otherwise, handle a timeout
        if what_ready[0] == []:
            return -1, 0
        # 3. Compare the time of receipt to time of sending, producing the total network delay
        else:
            delay = receivedTime - startTime

        # 4. Unpack the packet header for useful information, including the ID
        recPacket, addr = icmpSocket.recvfrom(1024)
        icmpHeader = recPacket[20: 28]
        # ip_type, code, checksum, packet_ID, sequence = struct.unpack("<bbHHh", icmpHeader)
        icmpType, icmpCode, icmpChecksum, icmpPacketID, icmpSequence = struct.unpack(">BBHHH", icmpHeader)

        ipversion, iptype, iplength, ipid, ipflags, ipttl, ipprotocol, ipchecksum, ipsrc_ip, ipdest_ip = struct.unpack(
            "!BBHHHBBHII", recPacket[:20])
        # 5. Check that the ID matches between the request and reply
        if icmpType == ICMP_ECHO_REPLY and icmpPacketID == ID:

            # 6. Return total network delay
            return delay, ipttl
        elif icmpType == ICMP_UNREACHED:
            return -3, 0
        elif icmpType == ICMP_OVERTIME:
            return -11, 0
        else:
            return -2, 0

```

