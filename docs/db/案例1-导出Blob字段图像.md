## 需求：
有一张单表存储着图片（图片使用Blob字段）以及图片的相关信息，需求是从数据库中将这些Blob字段内容保存为图片文件，文件名为图片的相关信息。

## 环境
数据库：Oracle
本地：Windows
工具：Hutool-db模块

## 编码

### 数据库配置：`src/main/resources/config/db.setting`
```shell
#JDBC url，必须
url = jdbc:oracle:thin:@localhost:1521/orcl
#用户名，必须
user = test
#密码，必须，如果密码为空，请填写 pass = 
pass = test
```

### 代码：`PicTransfer.java`
```java
public class PicTransfer {
	public static void main(String[] args) throws SQLException {
		Db.use().find(
				ListUtil.of("NAME", "TYPE", "GROUP", "PIC"), 
				Entity.create("PIC_INFO").set("TYPE", 1),
				rs -> {
					while(rs.next()){
						save(rs);
					}
					return null;
				}
		);
	}
	
	private static void save(ResultSet rs) throws SQLException{
		String destDir = "f:/pic";
		String path = StrUtil.format("{}/{}-{}.jpg", destDir, rs.getString("NAME"), rs.getString("GROUP"));
		FileUtil.writeFromStream(rs.getBlob("PIC").getBinaryStream(), path);
	}
}
```

