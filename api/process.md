# 6.1. process相关

* [启动流程](#启动流程)
* [查询流程](#查询流程)
* [取消/删除流程](#取消/删除流程)

### process

#### 启动流程

请求方式：POST

请求地址：/process

请求包体：

```Json
{
    "modelId": "d4888443-857e-11e8-bef7-d00dd670a106-模型ID",
    "deploymentId": "模型部署ID",
    "key": "模型Key",
    "processName": "test",
    "variables": {
        "projectId": "11111",
        "regionId": "bcc",
        "vpcId": "vpc-509A87BA",
        "storageId": "storage-320AA6D8",
        "imageId": "ami-2F051345",
        "securityGroupName": "default",
        "expireTime": "-1",
        "keypairName": "tt6666",
        "disableApiTermination": "false",
        "slaveNum": "6",
        "buyNum": "1",
        "Bingo::RS::FRONT::RSBILLING": {},
        "master-1a": {
            "instanceType": "m2.large"
        },
        "slave-": {
            "instanceType": "m2.large"
        },
        "SipRS::Mock": true,
        "subnetId": "subnet-439A34CC"
    }
}
```

参数说明：

| 参数           | 必须                            | 说明              |
| ------------ | ----------------------------- | --------------- |
| modelId      | (modelId,deploymentId,key)三选一 | 模型的ID           |
| deploymentId | (modelId,deploymentId,key)三选一 | 模型的发布ID         |
| key          | (modelId,deploymentId,key)三选一 | 模型的key          |
| processName  | 是                             | 启动流程的名称         |
| variables    | 是                             | 流程的变量，以json方式传入 |

返回结果：

[参考查询流程](#查询流程)

#### 查询流程

请求方式 ：GET

请求路径：/process/{processInstanceId}

请求参数：

| 参数                | 必须   | 说明             |
| ----------------- | ---- | -------------- |
| processInstanceId | 是    | 流程ID（activiti） |

返回结果：

```
{
    "id": "43a9f3e4-8a85-4df0-b1cc-0333be9fc0c8",
    "processInstanceId": "2935001",
    "modelId": "d4888443-857e-11e8-bef7-d00dd670a106",
    "modelKey": null,
    "processName": "test",
    "processType": null,
    "processStatus": "debug",
    "processContent": "{\"projectId\":\"11111\",\"regionId\":\"bcc\",\"vpcId\":\"vpc-509A87BA\",\"storageId\":\"storage-320AA6D8\",\"imageId\":\"ami-2F051345\",\"securityGroupName\":\"default\",\"expireTime\":\"-1\",\"keypairName\":\"tt6666\",\"disableApiTermination\":\"false\",\"slaveNum\":\"6\",\"buyNum\":\"1\",\"Bingo::RS::FRONT::RSBILLING\":{},\"master-1a\":{\"instanceType\":\"m2.large\"},\"slave-\":{\"instanceType\":\"m2.large\"},\"SipRS::Mock\":true,\"subnetId\":\"subnet-439A34CC\"}",
    "startTime": "2018-08-13 14:15:24",
    "endTime": null,
    "activitiFileId": null,
    "outputs": null,
    "nativeOutputs": "",
    "rollbackProcessInstanceId": null,
    "cancelProcessInstanceId": null,
    "deleteProcessInstanceId": null
}
```

| 参数                        | 说明                                       |
| ------------------------- | ---------------------------------------- |
| Id                        | process唯一标识                              |
| processInstanceId         | 流程ID（activiti）                           |
| modelId                   | 模型ID                                     |
| modelKey                  | 模型key                                    |
| processName               | 流程名称                                     |
| processType               | 暂不支持                                     |
| processStatus             | 流程状态                                     |
| processContent            | 流程启动的参数                                  |
| startTime                 | 启动时间                                     |
| endTime                   | 结束时间                                     |
| activitiFileId            | 暂不支持                                     |
| outputs                   | 流程输出，根据流程参数模板配置生成                        |
| nativeOutputs             | 流程原生输出                                   |
| rollbackProcessInstanceId | 如果流程失败，触发回滚的回滚流程ID                       |
| cancelProcessInstanceId   | 如果流程被取消，取消流程的流程ID                        |
| deleteProcessInstanceId   | 如果流程被触发删除事件，删除流程的流程ID。该删除，是发起新的删除流程，用以触发流程中各个执行器（stone）的删除事件，可以清理产生的数据和资源。 |

#### 取消/删除流程

请求方式：PUT

请求路径：/process/{processInstanceId}/{operate}

请求参数：

| 参数                | 必须   | 说明                                       |
| ----------------- | ---- | ---------------------------------------- |
| processInstanceId | 是    | 流程ID（activiti）                           |
| operate           | 是    | cancel/delete;cancel: 取消一个正在运行中的流程；delete: 触发一个流程实例的删除流程，清理流程产生的数据和资源。 |

> 无论是cancel还是delete，都需要流程中使用的执行器(stone)，实现了相关的逻辑。

返回结果：

[参考查询流程](#查询流程)