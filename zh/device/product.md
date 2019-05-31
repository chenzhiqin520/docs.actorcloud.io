# 产品管理

产品为设备的一个抽象集合，设备为产品的一个具体实例，一个设备必须对应唯一一个产品，不能直接删除设备非空的产品。

进入设备管理->产品管理，可查看产品列表：

- 点击**设备数量**查看产品下设备列表；

- 点击右上角的**新建**按钮可进行产品新建操作，请按照产品接入协议选择相应的**云端协议**。


点击产品卡片查看产品详情，产品详情有**产品信息**、**设备管理**、**功能点**、**数据流**、**指标管理**、**聚合数据**、**代理订阅**、**应用模板**、**资源定义**等几个基础模块。不同产品根据接入协议显示功能模块有所区别。




### 属性列表

**LWM2M** 协议特殊性，**LWM2M** 接入产品无需定义功能点与数据流，
可在协议范围内定义支持的属性，新建属性时请根据协议手册输入对象 ID 与 属性 ID 进行属性查找与定义。

![product_item_list](_assets/product_item_list.png)



### 功能点

功能点为产品/设备具备的最细粒度的能力或用途，一个产品可以包含多个功能点，点击产品详情页中的功能点即可查看该产品功能点列表。


功能点定义关键信息如下：

- 功能点标识：该功能数据在传输中使用的 JSON key 值 (JSON 数据类型)、二进制数据类型转换后的转换键值；

- 数据类型：有基础类型选择与功能类型（如地理位置）可选，根据实际上报/下发数据定义

  - JSON：该功能数据的格式，支持数值，枚举，字符串，故障，布尔值等，业务中必须传输 JSON 类型数据；

  - 二进制：由于二进制没有进行键-值编码，二进制传输信息的设备必须定义二进制功能点，下表是二进制数据类型定义指南：

| 类型标识 | 名称        | 转换后类型   | 长度必填   |
| ---- | --------- | ------- | ------ |
| 11   | 定长字符串     | String  | 是      |
| 12   | 定长字符串     | String  | 否      |
| 13   | 定长 binary | Integer | 是      |
| 14   | 变长 binary | Integer | 否      |
| 15   | 有符号整型     | Integer | 是      |
| 16   | 无符号整型     | Integer | 是      |
| 17   | 单精度浮点型    | Float   | 是      |
| 18   | 双精度浮点型    | Float   | 是      |
| 18   | bit 类型    | --      | 数据集自定义 |

- 枚举

 某些功能点不同数值如开关状态、噪声等级等有特定的含义，可以枚举出来，这类功能点可以添加枚举选项。

   - 实际值：实际上报、下发的值；

   - 显示值：显示在 ActorCloud 界面上的值。

定义示例：

- 某温度传感器需要定时上报温度信息，其上报 JSON 数据如下，`temperature` 即为功能点标识：

```
{
  "temperature": 77
}
```

该功能可按照下图定义：

![product_data_point](_assets/product_data_point.png)



### 数据流

数据流为产品/设备能够产生或消费的业务信息报文。
一条数据流包含一个或多个功能点，一个产品可以定义多个数据流，但是同一产品下的数据流主题不能重复，点击产品详情页中的数据流即可查看该产品数据流列表。



数据流定义关键信息如下：

- 功能点：该数据流包含的功能点，可多选；

- 数据流主题：数据流用于通信的主题；

- 流类型：可选设备数据上报、设备数据下发，数据的方向；

- 数据格式：可选二进制与 JSON 。选定二进制之后，通过该主题上报的二进制数据流将按照转换规则转换：

  - 通过选定数据序号与功能点进行转换规则定义，数据序号为选定功能点在二进制流中的序号；

  - bit 类型的功能点需要定义完整的 8bit 中的每一位。

  

定义示例：

- 温湿度传感器需要上报温度与湿度信息，则数据流名称可以为温湿度数据流，功能点包含温度与湿度，主题为 `temp_hum`：

![product_stream_create](_assets/product_stream_create.png)





### 设备列表

显示当前产品下的设备列表，操作同 **设备管理** -> **设备列表**。




### 指标管理

指标管理具有统计功能，用于聚合统计一个周期内数值类型的功能点、产品属性等信息数据并图表展示。

指标使用统计方法处理数据，对于特定的功能点可以统计其一段时期内的最大值、最小值、平均值、累计值数据，如果数据需要转换，如将 `英里` 转换为 `千米`, 可以使用公式指标。

指标管理统计的数据将在 **设备详情** -> **指标数据** 中展示。

- 图表类型：需要展现的图表类型；

- 视图展示：该指标是否展示图表；

- 聚合方式：周期内统计的方式，支持最大值、最小值、平均值、累计值等展示。




### 聚合数据

图表展示不同时间维度下当前产品中指标数据的聚合值。




### 代理订阅

设备连接后将自动订阅的主题列表，可以将上报的数据流主题纳入代理订阅列表。




### 应用模板

应用模板将一条数据流/产品属性中的某个值定义为一个操作卡片，其他值需要有一个默认值，以实现点击操作卡片一键下送控制指令的功能。

应用模板只支持下发数据流与数据传输类型包含**可下发**的功能点，对于 **LWM2M** 设备，仅支持**E**(执行)、**W**(写入)的产品属性。



模板格式对应可视化操作的卡片表现形式：

  - 数值、字符、时间类型：下发时需要指定数值；

  - 枚举类型：点击枚举值按钮可以将相关指令下发至设备；

  - 开关：点击开关可以下发切换设备状态的指令；

  - 布尔：点击 `true` `false` 按钮可以将相应指令下发至设备。

    

![app_templates](_assets/app_templates.png)




  > 每个卡片可以拖拽改变位置、点击右下角拖放改变尺寸。设备详情->控制台 页面将会按照该页面设置的模板与顺序展现。



### 资源定义

将设备上报某些属性、功能点关联到设备资源。

目前支持序列号、软件版本、硬件版本、制造商四个设备资源定义，即设备上报了指定功能点后，该数值会被同步更新到设备属性。

