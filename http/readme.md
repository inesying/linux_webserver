<!--
 * @Author       : inesying
 * @Date         : 2020-10-06
 * @copyleft GPL 2.0
--> 
HTTP
关于HTTP请求GET和POST的区别

 

1.GET提交，请求的数据会附在URL之后（就是把数据放置在HTTP协议头＜request-line＞中），以?分割URL和传输数据，多个参数用&连接;例如：login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0 %E5%A5%BD。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

  POST提交：把提交的数据放置在是HTTP包的包体＜request-body＞中。上文示例中红色字体标明的就是实际的传输数据

  因此，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变
3、GET请求，将参数直接写在访问路径上。操作简单，不过容易被外界看到，安全性不高，地址最多255字节；

4、POST请求，将参数放到body里面。POST请求操作相对复杂，需要将参数和地址分开，不过安全性高，参数放在body里面，不易被捕获。

POST方法有两种格式，一种是基本的格式，一般用于发送文字信息。
Post请求基本格式如下：
POST /login.asp HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 35
username=wantsoft&password=password //post的数据
另一种是multipart/form-data，格式如下
POST /upload_file/UploadFile HTTP/1.1
Accept: text/plain, /
Accept-Language: zh-cn
Host: www.example.com
Content-Type:multipart/form-data;boundary=—————————7d33a816d302b6
User-Agent: Mozilla/4.0 (compatible; OpenOffice.org)
Content-Length: 424
Connection: Keep-Alive —————————–7d33a816d302b6
Content-Disposition:form-data;
name=”userfile”;
filename=”userfile”
Content-Type: application/octet-stream abbXXXccc
—————————–7d33a816d302b6
Content-Disposition: form-data;
name=”text1” foo
—————————–7d33a816d302b6
Content-Disposition: form-data;
name=”password1” bar
—————————–7d33a816d302b6–
看起来比较复杂，其实就是把请求数据通过分界线也就是Boundary分开，至于开头的一些内容，很多都是默认，无需考虑。

表单格式：application/x-www-form-urlencoded

知识点：form表单默认的数据格式类型

表达格式又叫form格式，或者x-www-form-urlencoded格式，表单格式是由键值对组成，键和值之间用等号（=）连接。多个键值对之间用&符合连接，键和值不需要引号

如：name=Tom&age=19