ChannelBuffer的模型图如下：

 +-------------------+------------------+------------------+
 
 | PrependableBytes  |  ReadableBytes   |   WritableBytes  |
 
 |                   |     (CONTENT)    |                  |
 
 +-------------------+------------------+------------------+
 
 |                   |                  |                  |
 
 0      <=      readPos_   <=   writePos_    <=    capacity_




如上图所示，一个ChannelBuffer被划分为三个部分：

1.Prependable：表示已经读过的内容缓冲区

2.readable：表示可读的内容缓冲区

3.writable：表示可写的内容缓冲区

ChannelBuffer的这三个缓冲区由2个内部控制指针来控制：

readPos_：控制读缓冲区首地址

writePos_：控制写缓冲区首地址

Prependable=readPos_

Readable=writePos_-readPos_

writable=size()-writeIndex

iovec

I/O vector，与readv和wirtev操作相关的结构体。

成员iov_base指向一个缓冲区，这个缓冲区是存放readv所接收的数据或是writev将要发送的数据。

成员iov_len确定了接收的最大长度以及实际写入的长度。

// 第一块缓冲区，指向可写空间
 
 vec[0].iov_base = begin() + writerIndex_;
 
 vec[0].iov_len = writable;
 
 // 第二块缓冲区，指向栈空间
 
 vec[1].iov_base = extrabuf;
 
 vec[1].iov_len = sizeof extrabuf;
 
 /*
 
 通过writev函数可以将分散保存在多个（writev的第三个参数表示缓冲区的数量）buff的数据一并进行发送，
 
 通过readv可以由多个buff分别接受数据，适当的使用这两个函数可以减少I/O函数的调用次数
 
 */
 
 // 如果可写空间大于65536，那么把数据直接写到缓冲区即可，否则就一个额外的栈空间
 
 const int iovcnt = (writable < sizeof extrabuf) ? 2 : 1;
 
 const ssize_t n = sockets::readv(fd, vec, iovcnt);
 
 if (n < 0)
 
 {
 
 *savedErrno = errno;
 
 }
 
 // 第一块缓冲区足够容纳数据
 
 else if (implicit_cast<size_t>(n) <= writable)
 
 {
 
 // 向后移动writerIndex
 
 writerIndex_ += n;
 
 }
 
 // 第一块缓冲区空间不够，数据被接收到第二块缓冲区extrabuf，将其append至buffer
 
 else
 
 {
 
 // 更新当前writerIndex（到buffer的末尾）
 
 writerIndex_ = buffer_.size();
 
 // 将额外空间的数据写入到缓冲区中
 
 append(extrabuf, n - writable);
 
 }
