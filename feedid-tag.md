- [toc]

# 后台好友服务器说明
参数说明 | 参数值
---|---
服务主机地址 | 10.1.1.197
服务主机端口 | 22080
通信协议 | TCP

# 协议包结构说明
字段名 | 字节数 | 说明
---|---|---
pkg_header | 18 | 包头
pkg_body | 不定 | 包体

# 包头格式说明
字段名 | 类型 | 字节数 | 说明
---|---|---|---
pkg_len | Uint32 | 4 | 协议长度 
seq_num |	uint32|	4 | 包序列号，客户端填写，服务端原样返回
cmd_id | uint16| 2 | 协议号
status_code | uint32| 4 | 错误码
user_id | uint32 | 4 | 米米号

# 错误码说明
宏定义|错误码|描述
---|---|---
FEED_RELATION_SUCC|0|请求成功
FEED_RELATION_ERR_SYS|1001|服务内部内存错误
FEED_RELATION_ERR_PARA|1002|参数错误
FEED_RELATION_ERR_REDIS|1003|redis服务出错

# 协议说明
## 1. 添加最近更新文章的mimi号0xA101
**请求包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
time|	uint32|	4	|更新时间

## 2. 查询最近更新文章的mimi号0xA102
**请求包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
begin|	uint32|	4	|起始偏移量
end|	uint32|	4	|结束偏移量
cnt|	uint32|	4	|个数
**应答包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
cnt|	uint32|	4	|个数
mid|	uint32|	4	| 米米号 


## 3. 添加指定tag的feed流0xA103
**请求包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
time|	uint32|	4	|时间
tagcnt	|uint32	|4| tag数目
finfo | finfo | 不定 | feed信息
tag	|uint32[0]	|不定| 指定的tag


**finfo说明:**
字段名|	类型|	字节数	|说明
---|---|---|---
length|	uint32|	4	|feedid的长度
feedid|	char(0)	|不定	|feedid

**应答包体：**
无

## 4. 查询指定tag的feed流0xA104
**请求包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
tag	|uint32	|4| 指定的tag
begintime|	uint32|	4	|起始时间
endtime|	uint32|	4	|结束时间
cnt|	uint32|	4	|时间

**应答包体：**
**请求包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
cnt	|uint32	|4| 返回条数
finfo | finfo[0] | 不定 | feed信息

## 5. 删除指定tag的feed流0xA105
**请求包体：**
字段名|	类型|	字节数	|说明
---|---|---|---
tag	|uint32	|4| 指定的tag
finfo | finfo | 不定 | feed信息
**应答包体：**
无