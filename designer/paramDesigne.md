# 4.2.参数设计

假设你已经设计好了一个流程模型，那么它已经能够通过API传入一份json结构的参数数据，以启动流程，到这里已经足够了。

但是如果还需要通过申请页面来收集流程的启动参数，则需要通过参数模板的定义来动态渲染参数页面。

![img](..\img\paramRender.png)

| 属性                    | 必填   | 说明                                       |
| --------------------- | ---- | ---------------------------------------- |
| Key                   | Yes  | 参数名称                                     |
| DisplayName           | Yes  | 显示名称                                     |
| Type                  | Yes  | String,Number,及其他各类动态数据类型                |
| NoEcho                | No   | 如果为 TRUE，则使用 aws cloudformation describe-stacks时，参数值将使用星号 (*****) 进行遮蔽。默认为FALSE。 |
| AllowedValues         | No   | 包含参数允许值列表的阵列。 格式为数组，如：["vpc", 'region', "project"] |
| AllowedPattern        | No   | String 约束条件。对参数值所允许的格式进行校验的正则表达式。 注意：这是字串，转换特殊字符请使用 “\\”，如：“\\.” 可以匹配字符 “.” |
| MaxLength             | No   | String 约束条件。决定参数字符串值中最大字符数的整数值。          |
| MinLength             | No   | String 约束条件。决定参数字符串值中最小字符数的整数值。          |
| MaxValue              | No   | Number 约束条件。决定参数最大允许数值的数值。               |
| MinValue              | No   | Number 约束条件。决定参数最小允许数值的数值。               |
| Description           | No   | 用于说明参数的 String 类型（最多为 4000 个字符）。         |
| ConstraintDescription | No   | 用于解释发生约束违例时显示的约束要求的 String 类型。           |
| AllowedCustomization  | No   | 如果为TRUE,则允许用户修改。默认为TRUE                  |
| DefaultValue          | No   | 默认的参数值。如果为Ref:开头，则为引用类型，从当前作用域中取值        |
| NameSpace             | No   | 参数的作用域，不同作用域的参数互相隔离                      |

参数模板可以直接用json数组进行维护，例如：

```
[
  {
    "Key": "projectId",
    "Type": "Bingo::Project::Id",
    "DefaultValue": "11111",
    "DisplayName": "项目",
    "Fn::Source": {
      "userMode": "buy",
      "showRegion": true
    }
  },
  {
    "Key": "regionId",
    "Type": "Bingo::Cloud::Region",
    "DefaultValue": "bcc",
    "DisplayName": "区域",
    "Fn::Source": {}
  },
  {
    "Key": "vpcId",
    "Type": "Bingo::EC2::Vpc::Id",
    "DefaultValue": "",
    "DisplayName": "网络",
    "Fn::Source": {
      "regionId": "Ref:regionId",
      "vdcId": "Ref:projectId"
    }
  }]
```

#### 动态数据源

如果参数类型为动态数据源，则可以用`Fn::Source`来定义其参数；可以用`Ref：`来引用当前收集页面的其他参数，形成级联关系。

#### 关于outputs

输出参数同样用json数组定义：

```
[
  {
    "Key": "port",
    "DisplayName": "端口",
    "Value": "${parameters.port}"
  }
]
```

这里的输出，会是一个流程实例的最终输出结果，其中的Value可以引用流程上下文的所有变量，包括输入和输出。