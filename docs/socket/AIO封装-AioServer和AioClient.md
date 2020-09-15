## 由来

在JDK7+后，提供了异步Socket库——AIO，Hutool对其进行了简单的封装。

## 使用

### 服务端

```java
AioServer aioServer = new AioServer(8899);
aioServer.setIoAction(new SimpleIoAction() {
	
	@Override
	public void accept(AioSession session) {
		StaticLog.debug("【客户端】：{} 连接。", session.getRemoteAddress());
		session.write(BufferUtil.createUtf8("=== Welcome to Hutool socket server. ==="));
	}
	
	@Override
	public void doAction(AioSession session, ByteBuffer data) {
		Console.log(data);
		
		if(false == data.hasRemaining()) {
			StringBuilder response = StrUtil.builder()//
					.append("HTTP/1.1 200 OK\r\n")//
					.append("Date: ").append(DateUtil.formatHttpDate(DateUtil.date())).append("\r\n")//
					.append("Content-Type: text/html; charset=UTF-8\r\n")//
					.append("\r\n")
					.append("Hello Hutool socket");//
			session.writeAndClose(BufferUtil.createUtf8(response));
		}else {
			session.read();
		}
	}
}).start(true);
```

### 客户端

```java
AioClient client = new AioClient(new InetSocketAddress("localhost", 8899), new SimpleIoAction() {
	
	@Override
	public void doAction(AioSession session, ByteBuffer data) {
		if(data.hasRemaining()) {
			Console.log(StrUtil.utf8Str(data));
			session.read();
		}
		Console.log("OK");
	}
});

client.write(ByteBuffer.wrap("Hello".getBytes()));
client.read();

client.close();
```