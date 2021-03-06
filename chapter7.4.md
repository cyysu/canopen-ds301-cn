##7.4 **对象字典**
##7.4.1 **常规结构**
标准对象字典的整体布局定义于表41。  
<center/>表41：对象字典结构

|**索引**|**对象**|
|---|---|
|0000<sub>h</sub>|	未使用
|0001<sub>h</sub>–001F<sub>h</sub>|静态数据类型|
|0020<sub>h</sub>–003F<sub>h</sub>|复合数据类型|
|0040<sub>h</sub>–005F<sub>h</sub>|制造商指定复合数据类型|
|0060<sub>h</sub>–025F<sub>h</sub>|设备协议指定数据类型|
|0260<sub>h</sub>–03FF<sub>h</sub>|保留|
|0400<sub>h</sub>–0FFF<sub>h</sub>|保留|
|1000<sub>h</sub>–1FFF<sub>h</sub>|通信协议区|
|2000<sub>h</sub>–5FFF<sub>h</sub>|制造商指定协议区|
|6000<sub>h</sub>–67FF<sub>h</sub>|标准化协议区1<sup>st</sup>逻辑设备|
|6800<sub>h</sub>–6FFF<sub>h</sub>|标准化协议区2<sup>nd</sup>逻辑设备|
|7000<sub>h</sub>–77FF<sub>h</sub>|标准化协议区3<sup>rd</sup>逻辑设备|
|7800<sub>h</sub>–7FFF<sub>h</sub>|标准化协议区4<sup>th</sup>逻辑设备|
|8000<sub>h</sub>–87FF<sub>h</sub>|标准化协议区5<sup>th</sup>逻辑设备|
|8800<sub>h</sub>–8FFF<sub>h</sub>|标准化协议区6<sup>th</sup>逻辑设备|
|9000<sub>h</sub>–97FF<sub>h</sub>|标准化协议区7<sup>th</sup>逻辑设备|
|9800<sub>h</sub>–9FFF<sub>h</sub>|标准化协议区8<sup>th</sup>逻辑设备|
|A000<sub>h</sub>–AFFF<sub>h</sub>|标准网络变量区|
|B000<sub>h</sub>–BFFF<sub>h</sub>|标准系统变量区|
|C000<sub>h</sub>–FFFF<sub>h</sub>|保留|

对象字典容量65536，通过16位索引寻址，每个对象可包含多至256个子项，通过8位子索引号寻址。  
0001h到001Fh的静态数据类型包含标准的类型定义如BOOLEAN、INTEGE、UNSIGNED、浮点、字符串等。  
0020h到003Fh的复合数据类型是相对于标准数据类型而言的普适所有CANopen设备的预定义结构。  
0040h到005Fh的制造商指定复合数据类型是相对于标准数据类型而言的特定CANopen设备所定义的结构。  
设备协议可以定义附加数据类型作为其设备的类型。设备协议定义的静态数据类型和复合数据类型位列 0060h到025Fh。  
CANopen设备可选择支持只读索引区的复合数据类型(从0020h到005Fh和0060h到025Fh)结构。子索引0保存子索引数，后续子索引数据类型为表44规定的UNSIGNED16。
从1000h到1FFFh的通信协议区包含了通信相关参数。这些对象适用于所有的CANopen设备。  
从6000h到9FFFh的标准协议区包含可由网络读写的某类CANopen设备的所有数据对象。设备协议使用从6000h到9FFFh区域来描述功能和参数  
对象字典在概念上迎合了可选功能,这意味着生产商在其CANopen设备提供某些扩展功能，可以使用预定义方式。从2000h到5FFFh的空间正是为制造商的定制功能所留。  
从A0000h到AFFFh的网络变量区应包含输入变量和输出变量，这是可编程CANopen设备的一部分。这些网络变量的定义不在本规范的范围内，而是将来的协议和框架的一部分。  
B000h到BFFFh的系统变量应包含输入变量和输出变量，其为有层次感的基础CANopen网络的一部分。 这些系统变量的定义不在本规范的范围内，而是将来的协议和框架的一部分。 
###7.4.2 **索引和子索引的使用**
16位的索引可寻址对象字典的所有对象。简单的变量可由索引直接引用。记录和数组可由索引寻址其数据的整体机构。  
定义子索引来访问数据结构的具体元素。对于单一的对象字典对象如UNSIGNED8、BOOLEAN、INTEGER32等值，其子索引始终是00h。对于复合的对象字典对象如带有多个数据字段的数组或记录，子索引的引用域为主索引所指向的数据结构。子索引访问的字段可以是不同的数据类型。  
###7.4.3 **对象代码的使用**
对象代码表达了对象字典特定索引处的对象类型。使用到以下定义：
<center/>表42：对象字典对象定义

|对象名称|说明|对象代码|
|---|---|---|
|NULL|无数据字段对象|00<sub>h</sub>|
|DOMAIN|大体量数据，如可执行的程序代码|	02<sub>h</sub>|
|DEFTYPE|表示类型定义如BOOLEAN, UNSIGNED16, FLOAT等等|05<sub>h</sub>|
|DEFSTRUCT|定义了一种新的记录类型如在21h的PDO映射结构|06<sub>h</sub>|
|VAR|单值如UNSIGNED8、BOOLEAN、FLOAT、INTEGER16和VISIBLE STRING等|07<sub>h</sub>|
|ARRAY|多数据字段对象，每个数据字段都是一种简单相同的基本数据类型变量如UNSIGNED16的数组。子索引0类型为UNSIGNED8，并非数组的一部分。|08<sub></sub>|
|RECORD多数据字段对象，数据字段可以是任意简单变量的组合。子索引0是U|NSIGNED8，子索引255是UNSIGNED32,并非记录的一部分。|09<sub>h</sub>|

###7.4.4 **数据类型的使用**
对象的数据类型信息包括下列预定义类型：BOOLEAN、FLOAT、UNSIGNED、INTEGER、VISIBLE/OCTET   STRING、TIME_OF_DAY、TIME_DIFFERENCE和DOMAIN(见7.1)。它还包含预定义的复合数据类型PDO 映射和制造商、协议规范或应用协议规范所定义的其它类型。禁止定义记录的记录、记录数组或包含数组的记录。数组或记录对象内的一个子索引表示一个数据字段。  

###7.4.5 **访问权限的使用**
该属性定义了对象的访问权限。其视角是面对CANopen网络设备。  

有以下权限可选：
<center/>表43：数据对象访问权限属性

|**属性**|**描述**|
|---|---|
|rw|读写权限|
|wo|只写权限|
|ro|只读权限|
|const|只读并且值为常量  该值可以在NMT初始化态更改。其余状态不可变更。|

###7.4.6 **类别和条目类别的使用**
类别和条目类别定义了对象是强制性的、可选的还是条件的。CANopen设备必须支持强制对象，可以支持可选对象。如果设备执行特定的功能才会支持对应的对象。此种情况下，详细的对象规范会描述其中关系，并且该对象被定义为条件类别。

###7.4.7 **数据类型条目的使用**
####7.4.7.1 **简述**
静态数据类型放在对象字典里只是出于定义的目的。范围从0001h到0007h、0010h、从 0012h到 0016h，并从0018h到001Bh可以被映射进RPDO，以明确该CANopen设备不使用(无关)的空间。DEFTYPE和DEFSTRUCT不允许映射到 RPDOs中。
数据类型如下所示：

<center/>表44：对象字典的数据类型

|**索引**|**对象**|**名称**|
|---|---|---|
|0001<sub>h</sub>|DEFTYPE|BOOLEAN|
|0002<sub>h</sub>|DEFTYPE|INTEGER8|
|0003<sub>h</sub>|DEFTYPE|INTEGER16|
|0004<sub>h</sub>|DEFTYPE|INTEGER32|
|0005<sub>h</sub>|DEFTYPE|UNSIGNED8|
|0006<sub>h</sub>|DEFTYPE|UNSIGNED16|
|0007<sub>h</sub>|DEFTYPE|UNSIGNED32|
|0008<sub>h</sub>|DEFTYPE|REAL32|
|0009<sub>h</sub>|DEFTYPE|VISIBLE_STRING|
|000A<sub>h</sub>|DEFTYPE|OCTET_STRING|
|000B<sub>h</sub>|DEFTYPE|UNICODE_STRING|
|000C<sub>h</sub>|DEFTYPE|TIME_OF_DAY|
|000D<sub>h</sub>|DEFTYPE|TIME_DIFFERENCE|
|000E<sub>h</sub>|保留||
|000F<sub>h</sub>|DEFTYPE|DOMAIN|
|0010<sub>h</sub>|DEFTYPE|INTEGER24|
|0011<sub>h</sub>|DEFTYPE|REAL64|
|**索引**|**对象**|**名称**|
|0012<sub>h</sub>|DEFTYPE|INTEGER40|
|0013<sub>h</sub>|DEFTYPE|INTEGER48|
|0014<sub>h</sub>|DEFTYPE|INTEGER56|
|0015<sub>h</sub>|DEFTYPE|INTEGER64|
|0016<sub>h</sub>|DEFTYPE|UNSIGNED24|
|0017<sub>h</sub>|保留||
|0018<sub>h</sub>|DEFTYPE|UNSIGNED40|
|0019<sub>h</sub>|DEFTYPE|UNSIGNED48|
|001A<sub>h</sub>|DEFTYPE|UNSIGNED56|
|001B<sub>h</sub>|DEFTYPE|UNSIGNED64|
|001C<sub>h</sub>–001F<sub>h</sub>|保留||
|0020<sub>h</sub>|DEFSTRUCT|PDO_COMMUNICATION_PARAMETER|
|0021<sub>h</sub>|DEFSTRUCT|PDO_MAPPING|
|0022<sub>h</sub>|DEFSTRUCT|SDO_PARAMETER|
|0023<sub>h</sub>|DEFSTRUCT|IDENTITY|
|0024<sub>h</sub>–003F<sub>h</sub>|保留||
|0040<sub>h</sub>–005F<sub>h</sub>|DEFSTRUCT|制造商的复合数据类型|
|0060<sub>h</sub>–007F<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型1<sup>st</sup>的逻辑设备|
|0080<sub>h</sub>–009F<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型<sup>1st</sup>的逻辑设备|
|00A0<sub>h</sub>–00BF<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型2<sup>nd</sup>的逻辑设备|
|00C0<sub>h</sub>–00DF<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型2<sup>nd</sup>的逻辑设备|
|00E0<sub>h</sub>–00FF<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型3<sup>rd</sup>的逻辑设备|
|0100<sub>h</sub>–011F<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型3<sup>rd</sup>的逻辑设备|
|0120<sub>h</sub>–013F<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型4<sup>th</sup>的逻辑设备|
|0140<sub>h</sub>–015F<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型4<sup>th</sup>的逻辑设备|
|0160<sub>h</sub>–017F<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型5<sup>th</sup>的逻辑设备|
|0180<sub>h</sub>–019F<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型5<sup>th</sup>的逻辑设备|
|01A0<sub>h</sub>–01BF<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型6<sup>th</sup>的逻辑设备|
|01C0<sub>h</sub>–01DF<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型6<sup>th</sup>的逻辑设备|
|01E0<sub>h</sub>–01FF<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型7<sup>th</sup>的逻辑设备|
|0200<sub>h</sub>–021F<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型7<sup>th</sup>的逻辑设备|
|0220<sub>h</sub>–023F<sub>h</sub>|DEFTYPE|设备协议规范的标准数据类型8<sup>th</sup>的逻辑设备|
|0240<sub>h</sub>–025F<sub>h</sub>|DEFSTRUCT|设备协议规范的标准数据类型8<sup>th</sup>的逻辑设备|

数据类型使用详见7.1。每个CANopen设备不需要支持所有已定义的数据类型。CANopen设备仅需支持它在1000h到AFFF用到的数据类型。  
预定义的复合数据类型放在标准的数据类型之后。详见7.4.8。  
CANopen设备可选支持标准数据类型编码的数据长度(UNSIGNED32)。例如索引000Ch

(TIME_OF_DAY)包含值0000 0030<sub>h</sub>=48<sub>d</sub>使用48位序列作为TIME_OF_DAY的数据类型编码。如果长度是可变的(例如000F<sub>h</sub>=域)，对象条目包含0000 0000<sub>h</sub>。  
为支持复合数据类型，CANopen设备可选择提供该种数据类型结构。子索引00<sub>h</sub>值为索引支持最大子索引数，当然不计入00和FF<sub>h</sub>，表44的接下来的子索引值编码为UNSIGNED16(UNSIGNED8是旧的实现)。对象索引0020h描述了PDO通讯参数结构如下所示(见对象从1400<sub>h</sub>到15FF<sub>h</sub>)：

<center/>表45：复合数据类型示例

|子索引|值|(说明)|
|---|---|---|
|00<sub>h</sub>|04<sub>h</sub>|( 4个子索引)|
|01<sub>h</sub>|0007<sub>h</sub>|(UNSIGNED32)|
|02<sub>h</sub>|0005<sub>h</sub>|(UNSIGNED8)|
|03<sub>h</sub>|0006<sub>h</sub>|(UNSIGNED16)|
|04<sub>h</sub>|0005<sub>h</sub>|(UNSIGNED8)|

标准(简单)和复合的制造商特定数据类型可通过读取子索引01<sub>h</sub>来区分：复合数据类型对象该子索引是有值的，而标准数据类型对象该索引不存在，因此会得到中止SDO传输的反馈。  
请注意,某些条目数据类型为UNSIGNED32的对象是带有字符结构的(例如PDO COB-ID，见图67)。  

####7.4.7.2 **对象字典条目的组织结构**
如果某个对象字典对象包含几个分项，子索引00h描述了最大子索引，不计入FF<sub>h</sub>，其对象条目编码为UNSIGNED8。  
子索引FF<sub>h</sub>描述了对象类型及其结构。它被编码为UNSIGNED32且其组织如下：  
![图51：子索引FF<sub>h</sub>结构](./CANopen_DS301_CN_image/51.png)
图51：子索引FF<sub>h</sub>结构

可选支持子索引FF<sub>h</sub>。如果支持的话就要连整个对象字典的复合数据类型结构也一并提供，以使得可以上传整个对象字典条目结构。
###7.4.8 **预定义复合数据类型规范**
本节介绍用于通信的预定义复合数据类型结构。值的范围和含义的解释由使用这些类型的对象的详细描述给出。  
####7.4.8.1 **PDO通讯参数记录规格**
表46说明PDO通讯参数的记录。  

<center/>表46：PDO通讯参数记录

|索引|子索引|名称|数据类型|
|---|---|---|---|
|0020<sub>h</sub>|00h|最大子索引号|UNSIGNED8|
|''|01<sub>h</sub>|COB-ID|UNSIGNED32|
|''|02<sub>h</sub>|传输类型|UNSIGNED8|
|''|03<sub>h</sub>|抑制时间|UNSIGNED16|
|''|04<sub>h</sub>|保留|UNSIGNED8|
|''|05<sub>h</sub>|事件定时器|UNSIGNED16|
|''|06<sub>h</sub>|同步启动值|UNSIGNED8|
####7.4.8.2 **PDO映射参数记录规格**
表47表达PDO 映射参数记录。  
<center/>表47：PDO映射参数记录

|索引|子索引|名称|数据类型|
|---|---|---|---|
|0021<sub>h</sub>|00<sub>h</sub>|PDO映射对象数|UNSIGNED8|
|''|01<sub>h</sub>|第1个映射的对象|UNSIGNED32|
|''|02<sub>h</sub>|第2个映射的对象|UNSIGNED32|
|''|:|:::|::::::|
|''|40<sub>h</sub>|第64个映射的对象|UNSIGNED32|

####7.4.8.3 **SDO参数记录规格**
表48指定SDO参数记录。
<center/>表48：SDO参数记录

|索引|子索引|名称|数据类型|
|---|---|---|---|
|0022<sub>h</sub>|00<sub>h</sub>|最大子索引支持|UNSIGNED8|
|''|01<sub>h</sub>|COB-ID 客户端->服务器|UNSIGNED32|
|''|02<sub>h</sub>|COB-ID 服务器->客户端|UNSIGNED32|
|''|03<sub>h</sub>|SDO客户端应答服务器的Node-ID|UNSIGNED8|

####7.4.8.4 **身份记录规格**
表49指定身份的记录。  
<center/>表49：身份记录

|索引|子索引|名称|数据类型|
|---|---|---|---|
|0023<sub>h</sub>|00<sub>h</sub>|最大子索引支持|UNSIGNED8|
|''|01<sub>h</sub>|Vendor-ID|UNSIGNED32|
|''|02<sub>h</sub>|产品码|UNSIGNED32|
|''|03<sub>h</sub>|修订版本号|UNSIGNED32|
|''|04<sub>h</sub>|序列号|UNSIGNED32|
####7.4.8.5 **OS调试记录规格**
表50指定OS调试记录。  

<center/>表50：OS调试记录

|索引|子索引|名称|数据类型|
|---|---|---|---|
|0024<sub>h</sub>|00<sub>h</sub>|最大子索引支持|UNSIGNED8|
|''|01<sub>h</sub>|命令|OCTET_STRING|
|''|02<sub>h</sub>|状态<br/>00<sub>h</sub>-命令已完成-没有错误<br/>01<sub>h</sub>-命令完成-错误<br/>02<sub>h</sub>——预留<br/>:::::<br/>FE<sub>h</sub>——预留<br/>FF<sub>h</sub>-命令执行<br/>|UNSIGNED8|
|''|03<sub>h</sub>|回复|OCTET_STRING|
####7.4.8.6 **OS命令记录规格**
表51中指定OS命令的记录。
<center/>表51：OS命令的记录

|索引|子索引|名称|数据类型|
|---|---|---|---|
|0025<sub>h</sub>|00<sub>h</sub>|最大子索引支持|UNSIGNED8|
|''|01<sub>h</sub>|命令|OCTET_STRING|
|''|02<sub>h</sub>|状态<br/>00<sub>h</sub>-命令已完成-没有错误-未回复<br/>01<sub>h</sub>-命令已完成-没有错误-答复<br/>02<sub>h</sub>-命令完成-错误-未回复<br/>03<sub>h</sub>-命令完成-错误-回复<br/>04<sub>h</sub>-预留<br/>:::::<br/>FE<sub>h</sub>——预留<br/>FF<sub>h</sub>-命令执行|UNSIGNED8|
|''|03<sub>h</sub>|回复|OCTET_STRING|


