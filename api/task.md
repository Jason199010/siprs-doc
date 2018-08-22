# 6.2. task相关

* [获取流程的任务](#获取流程的任务)
* [获取单个任务](#获取单个任务)
* [任务调试](#任务调试)

### task

#### 获取流程的任务

请求方式：GET

请求路径：/task?processInstanceId=2910431

请求参数：

| 参数                | 必须   | 说明             |
| ----------------- | ---- | -------------- |
| processInstanceId | 是    | 流程ID（activiti） |

返回结果：

```
[{
        "id": "c91b3868-801e-43a3-b77a-9bddff26f671",
        "processInstanceId": "2910431",
        "taskId": "2912516",
        "name": "machine",
        "namespace": "machine",
        "stone": "MachineStone",
        "createTime": "2018-08-06 14:27:48",
        "endTime": "2018-08-06 14:28:31",
        "status": "monitor.success",
        "reason": null,
        "params": "{\"namespace\":\"machine\",\"outputParams\":\"[{\\\"serviceId\\\":\\\"68bd2483-3d2c-4112-9150-9770da000836\\\",\\\"serviceType\\\":\\\"Instance\\\",\\\"outputId\\\":\\\"OUTPUT19763363\\\"}]\",\"regionId\":\"701\",\"stackresult\":[{\"serviceType\":\"Instance\",\"resourceCode\":\"i-2E2B1F54\",\"serviceId\":\"68bd2483-3d2c-4112-9150-9770da000836\"}],\"userId\":\"2c5a9110-27d1-4834-84c7-95dfeb5ad73f\",\"template\":\"{\\\"AWSTemplateFormatVersion\\\":\\\"v1.0\\\",\\\"Description\\\":\\\"AwsCloud\\\",\\\"Resources\\\":{\\\"AWSEC2Instance19763363\\\":{\\\"Type\\\":\\\"AWS::EC2::Instance\\\",\\\"Properties\\\":{\\\"AvailabilityZone\\\":\\\"cc1\\\",\\\"SecurityGroups\\\":[\\\"default\\\"],\\\"Password\\\":\\\"LPP$o3P8\\\",\\\"KeyName\\\":\\\"\\\",\\\"ImageId\\\":\\\"ami-AE26FF8F\\\",\\\"StorageId\\\":\\\"storage-0FCF83D7\\\",\\\"StorageScheduleTags\\\":\\\"\\\",\\\"InstanceName\\\":\\\"Server_1\\\",\\\"HostName\\\":\\\"\\\",\\\"InstanceType\\\":\\\"m2.medium\\\",\\\"AssociatePublicIpAddress\\\":\\\"false\\\",\\\"ScheduleTags\\\":\\\"\\\",\\\"AllowNodes\\\":\\\"\\\",\\\"NetworkInterfaces\\\":[{\\\"PrivateIpAddress\\\":\\\"\\\",\\\"VpcId\\\":\\\"vpc-00176A87\\\",\\\"SubnetId\\\":\\\"subnet-15F62707\\\"}],\\\"UserData\\\":{\\\"Fn::Base64\\\":\\\"eyJ1c2VyZGF0YSI6ImFIUjBjRG92THpFd0xqSXdNaTQzTUM0eCIsImluc3RhbmNlSWQiOiJkMzljMDZkYi0zZWU2LTRlZDgtOTdlNy0yNjQ3MzRkY2ZkYzYiLCJzZWNyZXRLZXkiOiJNVGt6TXpkaE1qVXRNemN4TlMwMFkyVXhMVGd4TkRjdFpURTFOR1ZqTkdObU5qUmwiLCJhY2Nlc3NLZXkiOiIxOTIwMkMyMTk3NTg0OTY1QjA3Mzk2QzE0M0FENkI4QyIsImRlcGxveWVyVXJsIjoiaHR0cDpcL1wvMTAuMjAxLjc4LjEyOTo4MzAxIn0=\\\"},\\\"Tags\\\":[{\\\"Key\\\":\\\"service\\\",\\\"Value\\\":\\\"68bd2483-3d2c-4112-9150-9770da000836\\\"},{\\\"Key\\\":\\\"from\\\",\\\"Value\\\":\\\"sip\\\"},{\\\"Key\\\":\\\"order\\\",\\\"Value\\\":\\\"2018080600007\\\"}]}}},\\\"Outputs\\\":{\\\"OUTPUT19763363\\\":{\\\"Value\\\": {\\\"Ref\\\":\\\"AWSEC2Instance19763363\\\"}}}}\",\"queue\":\"MachineStone\"}",
        "outputs": "{\"strategyId\":\"[\\\"587165cf-a112-4cf1-a080-7039ebde2b46\\\",\\\"67e2a558-4ee2-461f-81e3-82d4176b3ce1\\\",\\\"6db44bd9-62e0-4144-ab83-9794deb221e6\\\",\\\"c626f605-7c87-4f5c-bf79-41d865d37431\\\",\\\"dc958448-55c4-40b3-ac82-1f475dd60b2a\\\",\\\"de2ccc70-d612-4872-827f-7ec486987d85\\\",\\\"f315ada8-3eb6-498e-9ad0-dffbb6774b22\\\"]\",\"machineId\":\"d39c06db-3ee6-4ed8-97e7-264734dcfdc6\",\"type\":\"MachineStone\"}",
        "progress": null
    }]
```

参数说明：

| 参数                | 说明                     |
| ----------------- | ---------------------- |
| id                | 唯一标识                   |
| processInstanceId | 流程ID（activiti）         |
| taskId            | 任务ID（activiti）         |
| name              | 任务名称                   |
| namespace         | 命名空间，适用于一个流程中有两个以上的执行器 |
| stone             | 执行器                    |
| createTime        | 创建时间                   |
| endTime           | 结束时间                   |
| status            | 状态                     |
| reason            | 失败原因                   |
| params            | 参数                     |
| outputs           | 输出                     |
| progress          | 进度                     |

任务状态：

| 状态               | 说明           |
| ---------------- | ------------ |
| produce.success  | 消息已发送,等待执行   |
| produce.failed   | 消息发送失败       |
| start.begin      | 开始执行         |
| start.failed     | 开始事件，执行失败    |
| start.success    | 开始事件成功       |
| monitor.begin    | 监控事件开始       |
| monitor.success  | 监控事件成功       |
| monitor.failed   | 监控事件失败       |
| rollback.begin   | 回滚事件开始       |
| rollback.success | 回滚事件成功       |
| rollback.failed  | 回滚事件失败       |
| delete.begin     | 删除事件开始       |
| delete.success   | 删除事件成功       |
| delete.failed    | 删除事件失败       |
| cancel.begin     | 取消事件开始       |
| cancel.success   | 取消事件成功       |
| cancel.failed    | 取消事件失败       |
| canceled         | 已取消          |
| mock.begin       | 模拟事件开始       |
| mock.success     | 模拟事件成功       |
| mock.passed      | 模拟事件跳过，不需要执行 |
| mock.failed      | 模拟事件失败       |

#### 获取单个任务

请求方式：GET

请求路径：/task/{taskId}

请求参数：

| 参数     | 必须   | 说明             |
| ------ | ---- | -------------- |
| taskId | 是    | 任务ID（activiti） |

返回结果：

[参考获取流程关联的任务](#获取流程的任务)

#### 任务调试

> 对于失败的，并且处于调试模式的流程，可以对任务进行调试操作。

请求方式：PUT

请求路径：/task/{taskId}/{operate}

请求参数：

| 参数      | 必须   | 说明                                       |
| ------- | ---- | ---------------------------------------- |
| taskId  | 是    | 任务ID（activiti）                           |
| operate | 是    | reRun：对于失败的任务，重新执行；resume：对于失败的任务，跳过调试模式，触发回滚。 |

返回结果：

[参考获取流程关联的任务](#获取流程的任务)

