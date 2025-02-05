<!--

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.

-->

> 下面是 IoTDB 生成或使用的文件
>
> 持续更新中。..

## 单机模式

### 配置文件
> conf 目录下
1. iotdb-datanode.properties
2. logback.xml
3. datanode-env.sh
4. jmx.access
5. jmx.password
6. iotdb-sync-client.properties
    + 只有 Sync 工具会使用

> 在 basedir/system/schema 目录下
1. system.properties
    + 记录的是所有不能变动的配置，启动时会检查，防止系统错误

### 状态相关的文件

#### 元数据相关文件
> 在 basedir/system/schema 目录下

##### 元数据
1. mlog.bin
    + 记录的是元数据操作
2. mtree-1.snapshot
    + 元数据快照
3. mtree-1.snapshot.tmp
    + 临时文件，防止快照更新时，损坏旧快照文件

##### 标签和属性
1. tlog.txt
    + 存储每个时序的标签和属性
    + 默认情况下每个时序 700 字节

#### 数据相关文件
> 在 basedir/data/目录下

##### WAL
> 在 basedir/wal 目录下
1. {StroageName}-{TsFileName}/wal1
    + 每个 memtable 会对应一个 wal 文件

##### TsFile
> 在 basedir/data/sequence or unsequence/{DatabaseName}/{DataRegionId}/{TimePartitionId}/目录下
1. {time}-{version}-{mergeCnt}.tsfile
    + 数据文件
2. {TsFileName}.tsfile.mods
    + 更新文件，主要记录删除操作

##### TsFileResource
1. {TsFileName}.tsfile.resource
    + TsFile 的概要与索引文件
2. {TsFileName}.tsfile.resource.temp
    + 临时文件，用于避免更新 tsfile.resource 时损坏 tsfile.resource
3. {TsFileName}.tsfile.resource.closing
    + 关闭标记文件，用于标记 TsFile 处于关闭状态，重启后可以据此选择是关闭或继续写入该文件

##### Version
> 在 basedir/system/databases/{DatabaseName}/{DataRegionId}/{TimePartitionId} or upgrade 目录下
1. Version-{version}
    + 版本号文件，使用文件名来记录当前最大的版本号

##### Upgrade
> 在 basedir/system/upgrade 目录下
1. upgrade.txt
    + 记录升级进度

##### Merge
> 在 basedir/system/databases/{DatabaseName}/目录下
1. merge.mods
    + 记录合并过程中发生的删除等操作
2. merge.log
    + 记录合并进展
3. tsfile.merge
    + 临时文件，每个顺序文件在合并时会产生一个对应的 merge 文件，用于存放临时数据

##### Authority
> 在 basedir/system/users/目录下是用户信息
> 在 basedir/system/roles/目录下是角色信息

##### CompressRatio
> 在 basedir/system/compression_ration 目录下
1. Ration-{compressionRatioSum}-{calTimes}
    + 记录每个文件的压缩率

