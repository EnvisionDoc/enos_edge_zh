# 边缘计算

对于模型点，往往需要做一些预处理，或者执行某些简单计算，这些可以直接利用EnOS Edge边缘计算模块来执行。

Edge中所有边缘计算都是依附模板而存在，模板是Edge与设备进行通讯的一个驱动，不同的设备类型需要使用不同的模板进行通讯，即如果将针对某一种设备类型的计算逻辑编辑在对应的模板上，即可实现对所有该种类型的设备进行统一的计算处理。

在**Edge网关 > 模板配置 > 模板编辑**的部分，用户可以使用以下边缘计算功能：

- 设置点映射，将将设备端的采集点经过一定运算加工之后，映射到预先定义的模型测点上；
- 使用Groovy脚本对数据进行处理。

## 点映射

EnOS提供了一系列公式，用以将设备端的采集点经过一定运算加工之后，映射到预先定义的模型测点上。

EnOS预置的公式列表如下。为了说明方便，在列表中，模型测点用y表示;采集点用 x(i) 表示，其中，i代表采集点被添加的顺序。

.. csv-table::

   "公式名称", "说明"
   "NO_MAPPING", "不对此模型测点做映射"
   "EQUAL", "模型测点的值等于采集点的值，即y=x"
   "SUM", "求和，将添加到本模型测点的采集点值加总求和，y=x(1)+x(2)+...+x(i)"
   "PRODUCT", "相乘，将添加到本模型测点的采集点相乘求积，可以配置一个可配系数y=a* x(1) * x(2) *...* x(i) *"
   "CROSS_PRODUCT", "内积：用于计算被添加到本模型测点的各采集点的内积，并乘以一个可配系数a（即“操作数”参数），注意采集点被添加进来的顺序很重要。即y=a(x(1) * x(2)+x(3) * x(4)+...+x(i-1) * x(i)"
   "RATIO", "相除：用于计算被添加到本模型测点的2个采集点的比率，注意采集点被添加进来的顺序很重要，即y=x(1)/x(2)"
   "LOGICAL_OR", "对添加到本模型测点的采集点求逻辑或，y=(x(1)|x(2)|...|x(i))"
   "RATIO_AGAINST_SUM", "对添加到本模型测点的三个采集点做如下运算：y=x(1)/(x(2)+x(3))"
   "BIT_N", "将一个AI类型的采集点的指定比特取出，复制到一个新模型点上，包含一个参数“操作数”表示取出的是AI点的第几个比特位。例如，操作数为0表示取出的AI点的第1位，操作数为15则表示取出的AI点的第16位"
   "BITS_M_TO_N", "取连续多位赋值公式，可将一个AI点的多个连续的比特取出赋值到一个新模型点上，包含2个参数：操作数M，高比特位；和操作数N，低比特位，M>N。例如，M=7,N=0，则指取出采集点第8到1位赋值到新模型点上去"
   "IF_EQUAL", "包含3个操作数，记操作数1=a，操作数2=b，操作数3=c，则此公式的运算逻辑为：if x == a, then y== b, else y==c"
   "MULTICHANNEL", "将多个采集点分别映射到一个数组类型模型点的各组元上。即y为数组：y={y[1], y[2], …, y[i]}, 且y[1]=x(1), y[2]=x(2), …, y[i]=x(i), i<=32"
   "MULTIBIT", "y为int32数组，y={y[1],y[2]...,y[i]}，其中y[1].bit0=x(1).bit0, y[1].bit1=x(2).bit0, …, y[1].bit31=x(32).bit0,y[2].bit0=x(33).bit0,y[2].bit1=x(34).bit0,…,y[2].bit31=x(64).bit0,…,y[i].bit0=x(32(i-1)+1).bit0,y[i].bit1=x_(32(i-1)+2).bit0,…,y[i].bit31=x_(32(i-1)+32).bit0,i<=32"

### 非array型模型测点适用公式

对于非array的模型属性，EnOS支持下列公式：

- NO_MAPPING
- INVALID
- EQUAL
- SUM
- PRODUCT
- CROSS_PRODUCT
- RATIO
- LOGICAL_OR
- RATION_AGAINST_SUM
- BIT_N
- BITS_M_TO_N
- IF_EQUAL

### array型模型测点适用公式

- NO_MAPPING
- INVALID
- MULTICHANNEL
- MULTIBIT

## 数据处理脚本

Groovy语言的语法参见www.groovy-lang.org。但Groovy一些高级功能，如定义一个新类，在EnOS中被禁用。

为了能够使用Groovy脚本来完成设备数据的实时处理，EnOS提供了一些可以在脚本中使用的方法，用户可以使用这些方法完成模型点数据的读取、设置，以及设备属性的读取等。

下列一些最为常用的方法：

.. csv-table::
   
   "方法", "说明"
   "Number input(String domain-point-name)", "读取一个模型的值。以模型测点名为参数，且模型点名必须匹配在设备模型中的定义。返回值为int或float型，参照模型点在设备模型中的定义"
   "void output(String domain-point-name, Number value)", "设置一个模型点的值（即为一个模型点产生一条数据）。模型点名必须匹配在设备模型中的定义。需要设置的值为int或float类型。在设置时，EnOS会将值转换为匹配模型点定义的值类型。"
   "Boolean attrExists(String attribute-name)", "-	判断一个设备领域属性是否存在。以领域属性Key值为参数，且必须已经定义在设备模型中。"
   "String attrString(String attribute-name)", "-	读取一个设备领域属性值作为字符串。以领域属性Key值为参数，且必须已经定义在设备模型中。"

### 脚本示例

- 例如某厂家的电表，其电压值U=U(0)*10^(P-4)， U(0)和P都是采集到的原始值，需要经过此运算后才能得到最终的电压值U。可编写脚本如下：
   ```groovy
   output("METER3X.UA", input("METER3X.UA_tmp") * (10 ** (input("METER3X.DPT") - 4)))
   ```

- 某厂家有一台逆变器，每当逆变器上报特定错误码（28或38），EnOS为该逆变器INV.Fault模型测点产生一条值为1的数据；每当逆变器上报其他错误码的时候，EnOS为该逆变器INV.Fault模型测点产生一条值为0的数据。其脚本如下：
  ```groovy
  if (input("INV.FaultCode")==28 || input("INV.FaultCode"=38)
  {
    output("INV.Fault", 1)
  }
  else
  {
    output("INV.Fault", 0)
  }

  ```

- 某厂家领域属性`invType`设置为`STRING`的设备，每次接收到多路模型属性INV.BranchCurIn数据后，EnOS都会求取其离散率并乘以100，设置到INV.Disperse模型属性中，`invType`不满足条件的设备则不执行计算。脚本如下：
  ```groovy
  if (attrExists("invType")&&attrString("invType") == "STRING") {
    output("INV.Disperse", mcCoV("INV.BranchCurIn") * 100.0)
  }

  ```
