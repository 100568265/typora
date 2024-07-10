# 数据库



## Channel表

**主表**

channels.db

| 字段名           | 数据类型     | 描述                           | 备注         |
| ---------------- | ------------ | ------------------------------ | ------------ |
| channel_id       | INT          | 自增，主键                     | 通道唯一ID   |
| channel_number   | INT          | 唯一                           | 通道号       |
| channel_name     | VARCHAR(255) | 非空                           | 通道名       |
| channel_type     | INT          | 非空, 1代表问答式，2代表全双工 | 通道类型     |
| channel_protocol | VARCHAR(255) |                                | 通道规约     |
| channel_interval | INT          |                                | 发送间隔(ms) |
| port_type        | INT          | 非空                           | 端口类型     |
| auto_open        | INT          | 1或0                           | 自动打开通道 |
| device_count     | INT          |                                | 设备数量     |
| create_time      | INTEGER      |                                | 创建时间     |
| update_time      | INTEGER      |                                | 更新时间     |



```sqlite
CREATE TABLE channels(
    channel_id INTEGER PRIMARY KEY AUTOINCREMENT,
  	channel_number INT NOT NULL UNIQUE,
    channel_name VARCHAR(255) NOT NULL,
    channel_type INT NOT NULL,
    channel_protocol VARCHAR(255),
    channel_interval INT,
    port_type INT,
    auto_open INT,
    device_count INT,
  	create_time INTEGER,
  	update_time INTEGER
);
```



**串口通道表**

serial_channels.db

| 字段名        | 数据类型    | 描述       | 备注       |
| ------------- | ----------- | ---------- | ---------- |
| channel_id    | INT         | 主键，外键 | 通道唯一ID |
| serial_number | INT         |            | 串口号     |
| baud_rate     | INT         |            | 波特率     |
| parity        | VARCHAR(10) | N,O,E      | 校验位     |
| data_bits     | INT         |            | 数据位     |
| stop_bits     | INT         |            | 停止位     |

```sql
FOREIGN KEY (channel_id) REFERENCES channels(channel_id)
```

channel_id作为channels表的外键



```sql
CREATE TABLE serial_channel(
    channel_id INT PRIMARY KEY,
    serial_number INT,
    baud_rate INT,
    parity VARCHAR(10),
    data_bits INT,
    stop_bits INT,
    FOREIGN KEY (channel_id) REFERENCES channels(channel_id)
);
```





TCP服务端通道表

tcp_server_channel.db

| 字段名        | 数据类型     | 描述       | 备注       |
| ------------- | ------------ | ---------- | ---------- |
| channel_id    | INT          | 主键，外键 | 通道唯一ID |
| local_port    | INT          |            | 本机端口   |
| local_address | VARCHAR(100) |            | 本机IP     |

```sql
FOREIGN KEY (channel_id) REFERENCES channels(channel_id)
```



TCP客户端通道表

tcp_client_channel.db

| 字段名         | 数据类型     | 描述       | 备注       |
| -------------- | ------------ | ---------- | ---------- |
| channel_id     | INT          | 主键，外键 | 通道唯一ID |
| remote_address | VARCHAR(100) |            | 远方IP     |
| remote_port    | INT          |            | 远方端口   |

```sql
FOREIGN KEY (channel_id) REFERENCES channels(channel_id)
```



channel通道类型(port_type)枚举对应表

| 通道类型  | 枚举量 |
| --------- | ------ |
| 串口通道  | 1      |
| TCP客户端 | 2      |
| TCP服务端 | 3      |









## Devices表

| 字段名               | 数据类型     | 描述       | 中文名       | 备注                      |
| -------------------- | ------------ | ---------- | ------------ | ------------------------- |
| device_id            | INTEGER      | 主键，自增 | 设备id       |                           |
| channel_number       | INT          |            | 所属通道号   |                           |
| device_number        | INT          | UNIQUE     | 设备号       |                           |
| device_name          | VARCHAR(255) |            | 设备名       |                           |
| device_address       | INT          |            | 设备地址     |                           |
| device_addressex     | INT          |            | 扩展地址     |                           |
| device_group         | VARCHAR(255) |            | 设备组       | 替换xml中的DeviceTypeID   |
| device_template      | VARCHAR(255) |            | 设备子模板   | 替换xml中的DeviceTypeName |
| device_serial_type   | VARCHAR(255) |            | 设备子系列   |                           |
| device_protocol_file | VARCHAR(255) |            | 设备规约文件 |                           |
| device_protocol_name | VARCHAR(255) |            | 设备规约名   |                           |
| resend_count         | INT          |            | 重发次数     |                           |
| break_count          | INT          |            | 中断次数     |                           |
| run                  | INT          |            | 设备投运     |                           |
| device_count         | INT          |            | 设备数量     |                           |
| channel_id           | INT          | 外键       | 所属通道ID   |                           |

```sql
FOREIGN KEY (channel_number) REFERENCES channels(channel_number)
```





































# 接口文档



## channel页面



### **添加一个通道**

路由：/channelconfig/addchannel

方法：POST

json格式：

串口通道：

```json
{
    "channel_name": "串口通道1",
    "channel_number": 1,
    "channel_type": 1,
    "channel_protocol": "COM",
    "channel_interval": 30,
    "port_type": 1,
    "auto_open": 1,
    "device_count": 10,
  	"serial_number": 2,
  	"baud_rate": 9600,
  	"parity": "none",
  	"data_bits": 8,
  	"stop_bits": 1
}
```



### 删除一个通道

路由：/channelconfig/deletechannel

方法：DELETE

json格式:

```json
{
    "channel_number":1
}
```





### 更新一个通道

路由：/channelconfig/updatechannel

方法：PUT

json格式：

```json
{
    "channel_name": "串口通道1",
    "channel_number": 1,
    "channel_type": 1,
    "channel_protocol": "COM",
    "channel_interval": 3000,
    "port_type": 1,
    "auto_open": 1,
    "device_count": 10,
  	"serial_number": 2,
  	"baud_rate": 115200,
  	"parity": "none",
  	"data_bits": 8,
  	"stop_bits": 1
}
```





### 查询所有通道

路由：/channelconfig/query

方法：GET

BODY：无

json格式：

```json
[
    {
        "auto_open": 1,
        "baud_rate": 9600,
        "channel_interval": 30,
        "channel_name": "串口通道1",
        "channel_number": 1,
        "channel_protocol": "COM",
        "channel_type": 1,
        "data_bits": 8,
        "device_count": 10,
        "parity": 0,
        "port_type": 1,
        "serial_number": 2,
        "stop_bits": 1
    },
    {
        "auto_open": 1,
        "baud_rate": 9600,
        "channel_interval": 30,
        "channel_name": "串口通道1",
        "channel_number": 2,
        "channel_protocol": "COM",
        "channel_type": 1,
        "data_bits": 8,
        "device_count": 10,
        "parity": 0,
        "port_type": 1,
        "serial_number": 2,
        "stop_bits": 1
    }
]
```











