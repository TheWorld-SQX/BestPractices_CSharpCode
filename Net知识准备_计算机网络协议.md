## 分层模型
OSI模型（Open Systems Interconnection model）总共有7层，从最底层到最顶层分别是：

1. **物理层（Physical Layer）：**
   - 定义硬件设备和传输媒体的规范，例如电缆、光纤、无线传输等。

2. **数据链路层（Data Link Layer）：**
   - 负责通过物理地址（MAC地址）来传输帧。这一层通常包括以太网、WiFi等协议。

3. **网络层（Network Layer）：**
   - 提供网络寻址、路由选择和分包等功能。IP协议运行在这一层。

4. **传输层（Transport Layer）：**
   - 主要负责端到端的通信和数据流控制。TCP（Transmission Control Protocol）和UDP（User Datagram Protocol）工作在这一层。

5. **会话层（Session Layer）：**
   - 提供建立、管理和终止会话的手段。负责建立、管理和终止会话连接。

6. **表示层（Presentation Layer）：**
   - 处理数据的表示、编码和加密，确保一个系统中的应用程序能够理解另一个系统中的数据。

7. **应用层（Application Layer）：**
   - 提供用户接口和网络服务，包括各种应用程序，例如Web浏览器、电子邮件客户端等。

在这个模型中，UDP和TCP位于传输层。它们是两种不同的传输协议，用于在网络上可靠地传输数据。其中：

- **TCP（Transmission Control Protocol）：** 提供可靠的、面向连接的通信。它确保数据的可靠性、有序性和完整性，但可能会引入一些延迟。

- **UDP（User Datagram Protocol）：** 是一种面向无连接的传输协议，它不提供可靠性，但具有较低的延迟。UDP适用于一些对实时性要求较高，可以容忍少量数据丢失的应用场景，例如音频和视频传输。


## 除了七层 还有另种被大家接受的
除了OSI模型之外，还有另一种网络体系结构模型，即TCP/IP模型（Transmission Control Protocol/Internet Protocol）。TCP/IP模型是实际互联网使用最广泛的体系结构，并且在实践中更为常见。

TCP/IP模型将网络协议分为四个层次，从低到高分别是：

1. **链路层（Link Layer）：**
   - 对应OSI模型的物理层和数据链路层。

2. **网络层（Internet Layer）：**
   - 对应OSI模型的网络层。

3. **传输层（Transport Layer）：**
   - 对应OSI模型的传输层。

4. **应用层（Application Layer）：**
   - 对应OSI模型的会话层、表示层和应用层。

在TCP/IP模型中，UDP和TCP仍然位于传输层，提供相同的功能，即端到端的通信。这两种模型都为网络协议提供了一种层次化的结构，以便更好地理解和管理网络通信。在实际应用中，TCP/IP模型被广泛采用，而OSI模型通常更多地用于学术和理论讨论。


## 是说平常我们发的消息被封装到一层一层的协议帧里去了?
是的，通信协议通常会将用户数据封装在一层一层的协议帧（Frame）中进行传输。每一层协议都有其特定的格式和字段，负责不同的功能，从物理层到应用层逐层封装。

以Modbus协议为例，它可以在不同的物理层上传输，比如在串口通信中使用RS-232或RS-485，或者在以太网通信中使用TCP/IP。在这些物理层上，Modbus协议将用户数据封装在协议帧中，这通常包括：

1. **起始位和停止位：** 在串口通信中，这是物理层的一部分，用于标识数据帧的开始和结束。

2. **地址字段：** 标识主站和从站的地址。

3. **功能码：** 指示从站应该执行的操作，比如读取寄存器或写入寄存器。

4. **数据字段：** 包含实际的用户数据。

5. **校验字段：** 用于检测或纠正传输中可能发生的错误。

这种分层的协议设计有助于提高通信的灵活性和可扩展性。不同的协议层负责不同的任务，使得可以在不同的物理层上使用相同的高层协议，同时能够适应各种不同的通信需求。

总体而言，通信协议的层次结构帮助确保在通信过程中数据的可靠性、完整性和安全性。



## 下层的协议会把上层用的特定格式帧 封装进去吧我记得有些是封装到头部或是尾部或是数据位里
是的，通信协议的分层结构中，通常会将上层协议的帧封装到下层协议的数据中。这个过程被称为协议栈的封装。

具体封装的方式取决于协议的设计和实现，但常见的做法包括：

1. **头部封装：** 上层协议的帧被添加到下层协议的头部。这样，接收方在解析数据时首先识别并去除头部信息。

2. **尾部封装：** 上层协议的帧被添加到下层协议的尾部。接收方解析数据时先读取尾部信息。

3. **数据位封装：** 上层协议的帧被嵌套在下层协议的数据位中。这通常涉及到将上层帧嵌套在下层数据包的特定字段中。

这种封装的方式有助于将不同层次的协议进行有效地组合，同时确保数据在通信链路上的正确传输。每个协议层负责自己的任务，而底层的传输协议则负责将整个数据包传送到目的地。

在实际应用中，TCP/IP协议栈是一个典型的例子。在这个协议栈中，数据从应用层（如HTTP）传输到传输层（TCP），然后通过网络层（IP）和链路层（Ethernet）最终发送到物理介质上。每一层协议都有自己的头部和尾部信息，以便在接收端正确解析和处理。


## HttpContent
"MultipartFormDataContent"是一个类，通常在使用C#编写的应用程序中用于创建HTTP请求的主体部分。在这个类中，可以添加多个HttpContent对象作为不同部分的内容，从而实现HTTP请求的多部分传输。

通常情况下，当需要向服务器发送包含多个部分的数据（例如上传文件和其他表单数据）时，就会使用这种方式构造HTTP请求。每个部分都可以是不同类型的数据，例如文本、二进制文件等。

以下是一个简单的示例，展示了如何使用C#创建一个包含多个部分的HTTP请求主体：

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        // 创建一个包含多个部分的HTTP请求主体
        var content = new MultipartFormDataContent();

        // 添加文本部分
        var textContent = new StringContent("Hello, World!");
        content.Add(textContent, "text");

        // 添加文件部分
        var fileContent = new ByteArrayContent(System.IO.File.ReadAllBytes("example.jpg"));
        fileContent.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/jpeg");
        content.Add(fileContent, "file", "example.jpg");

        // 创建HTTP请求客户端
        using (var client = new HttpClient())
        {
            // 发送POST请求
            var response = await client.PostAsync("http://example.com/upload", content);

            // 处理响应
            if (response.IsSuccessStatusCode)
            {
                Console.WriteLine("Upload successful.");
            }
            else
            {
                Console.WriteLine("Upload failed. Status code: " + response.StatusCode);
            }
        }
    }
}
```

在这个示例中，我们创建了一个MultipartFormDataContent对象，并向其中添加了两个部分：一个文本部分和一个文件部分。然后，我们使用HttpClient类发送了一个POST请求，将这个多部分内容作为请求主体发送到服务器。

在实际应用中，根据需要可能会添加更多的部分，并设置适当的内容类型、文件名等信息。
