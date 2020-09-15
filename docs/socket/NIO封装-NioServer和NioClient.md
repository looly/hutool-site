## 由来

Hutool对NIO其进行了简单的封装。

## 使用

### 服务端

```java
NioServer server = new NioServer(8080);
server.setChannelHandler((sc)->{
	ByteBuffer readBuffer = ByteBuffer.allocate(1024);
	try{
		//从channel读数据到缓冲区
		int readBytes = sc.read(readBuffer);
		if (readBytes > 0) {
			//Flips this buffer.  The limit is set to the current position and then
			// the position is set to zero，就是表示要从起始位置开始读取数据
			readBuffer.flip();
			//eturns the number of elements between the current position and the  limit.
			// 要读取的字节长度
			byte[] bytes = new byte[readBuffer.remaining()];
			//将缓冲区的数据读到bytes数组
			readBuffer.get(bytes);
			String body = StrUtil.utf8Str(bytes);
			Console.log("[{}]: {}", sc.getRemoteAddress(), body);
			doWrite(sc, body);
		} else if (readBytes < 0) {
			IoUtil.close(sc);
		}
	} catch (IOException e){
		throw new IORuntimeException(e);
	}
});
server.listen();
```

```java
public static void doWrite(SocketChannel channel, String response) throws IOException {
	response = "收到消息：" + response;
	//将缓冲数据写入渠道，返回给客户端
	channel.write(BufferUtil.createUtf8(response));
}
```

### 客户端

```java
NioClient client = new NioClient("127.0.0.1", 8080);
client.setChannelHandler((sc)->{
	ByteBuffer readBuffer = ByteBuffer.allocate(1024);
	//从channel读数据到缓冲区
	int readBytes = sc.read(readBuffer);
	if (readBytes > 0) {
		//Flips this buffer.  The limit is set to the current position and then
		// the position is set to zero，就是表示要从起始位置开始读取数据
		readBuffer.flip();
		//returns the number of elements between the current position and the  limit.
		// 要读取的字节长度
		byte[] bytes = new byte[readBuffer.remaining()];
		//将缓冲区的数据读到bytes数组
		readBuffer.get(bytes);
		String body = StrUtil.utf8Str(bytes);
		Console.log("[{}]: {}", sc.getRemoteAddress(), body);
	} else if (readBytes < 0) {
		sc.close();
	}
});
client.listen();
client.write(BufferUtil.createUtf8("你好。\n"));
client.write(BufferUtil.createUtf8("你好2。"));
// 在控制台向服务器端发送数据
Console.log("请输入发送的消息：");
Scanner scanner = new Scanner(System.in);
while (scanner.hasNextLine()) {
	String request = scanner.nextLine();
	if (request != null && request.trim().length() > 0) {
		client.write(BufferUtil.createUtf8(request));
	}
}
```