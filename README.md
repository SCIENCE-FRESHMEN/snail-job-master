# snail-job-master
🔥 灵活、可靠、高性能的分布式任务重试 & 分布式任务调度开源平台

## 项目简介
SnailJob 是一套一体化分布式任务治理平台，融合**分布式失败重试**、**定时任务调度**、**可视化工作流编排**三大核心能力，将重试、死信、流量管控、分片任务、DAG流程、告警监控等能力统一平台化管理，减少业务代码侵入，提升分布式系统数据一致性与运维效率。

项目采用分区架构设计，天然支持水平扩容、故障转移，调度精度可达秒级，具备完善的可视化控制台、全链路日志追踪、多渠道告警、权限管控等配套能力，适配中小到大规模分布式业务场景。

## 核心能力
### 一、分布式任务重试（核心一等能力）
- 多重试模式：本地重试、远程重试、混合重试
- 丰富重试策略：固定间隔、指数退避、CRON定时重试、自定义算法
- 重试流量治理：单机/跨服务限流、重试风暴防护、异常黑白名单
- 任务收敛机制：死信归档、执行完成回调、幂等ID防重复执行
- 多渠道通知：邮件、Webhook、主流办公机器人告警推送

### 二、高性能分布式调度
- 多种触发规则：CRON表达式、固定频率、秒级定时、一次性任务
- 多样化执行模式：集群执行、广播执行、静态分片、Map/MapReduce分片
- 任务运维：手动触发、暂停/恢复、强制终止、失败重试、断点续跑
- DAG可视化工作流：拖拽编排任务依赖，支持复杂业务流程串行/并行调度

### 三、平台运维配套
- 可视化管理控制台：任务、重试、执行器、日志、监控一站式操作
- 全链路日志：实时日志查看、历史执行记录检索、异常堆栈展示
- RBAC权限体系：多账号、多角色、资源隔离
- 开放REST API，支持第三方系统对接
- 多数据源适配，插件化存储扩展

### 四、跨语言客户端
原生Java客户端（本仓库核心），同步提供Python、Go独立SDK，无需Java Runtime间接调用，原生直连服务端执行任务。

## 项目模块结构
```
snail-job-master
├── .github/workflows        # GitHub CI/CD流水线配置
├── .gitee                   # Gitee平台配置文件
├── doc                      # 官方使用文档、部署文档
├── snail-job-common         # 公共常量、工具类、通用实体
├── snail-job-datasource     # 数据持久层、多数据库适配
├── snail-job-client         # Java客户端核心实现
├── snail-job-client-starter # SpringBoot快速接入starter
├── snail-job-server-interface # 服务端对外接口定义
├── snail-job-server-dispatcher # 调度分发核心服务
├── snail-job-server-starter # SpringBoot服务端启动器
├── .gitattributes
├── .gitignore
├── CONTRIBUTING.md          # 开源贡献规范
└── pom.xml                  # 项目根Maven配置
```

## 环境要求
- JDK 17 及以上版本
- Maven 3.6+ 构建工具
- MySQL 8.0+/PostgreSQL 等关系型数据库

## 快速开始
### 1. 克隆源码
```bash
git clone https://github.com/SCIENCE-FRESHMEN/snail-job-master.git
cd snail-job-master
```

### 2. 项目编译打包
```bash
mvn clean package -DskipTests
```

### 3. 服务端启动
引入 `snail-job-server-starter` 模块，SpringBoot项目直接启动，配置数据库连接即可运行调度控制台。

### 4. 客户端接入
SpringBoot项目引入 `snail-job-client-starter`，简单配置服务端地址，即可通过注解定义定时任务、重试任务。

示例定时任务：
```java
/**
 * 定时任务示例
 * @author opensnail
 * @since 1.5.1
 */
@SnailJob(cron = "0 0/1 * * * ?")
public void executeTask() {
    // 业务逻辑
}
```

示例失败重试：
```java
@SnailRetry(retryCount = 5, delay = 3000)
public void remoteCall() {
    // 远程调用，异常自动重试
}
```

## 贡献指南
详见 [CONTRIBUTING.md](./CONTRIBUTING.md)，简要流程如下：
1. Fork 本仓库至个人账号
2. 克隆Fork后的仓库到本地
3. 新建功能分支进行开发
4. 同步上游dev分支代码，避免冲突
5. 规范提交代码，提交格式：`#类型 #issue编号 中文描述`
   - 类型可选：bug / feature / enhancement
   - 示例：`enhancement #I7EIC7 增加路由剔除机制,提高节点的可达性`
6. 推送分支到个人Fork仓库，向原仓库dev分支提交PR
7. PR规范要求：
   - 所有PR必须关联至少一个Gitee Issue，无issue需自行创建
   - 新增功能必须补充单元测试，保证用例全部通过
   - 代码头部统一添加标准类注释，核心逻辑增加行内注释
   - 合并前压缩多次提交，保证提交记录简洁清晰

### 成为Committer条件
1. 熟读项目源码，理解核心调度、重试逻辑
2. 认领Issue完成3个有效PR并审核合并，即可申请成为Committer
3. 长期维护Committer要求：每月至少1个合并PR，活跃参与社区答疑、项目宣传

## 社区交流
- 文档地址：项目doc目录完整部署、使用、开发文档
- 问题反馈：Gitee Issues 提交bug、功能建议
- 社区讨论：官方Gitter交流房间

## 开源协议
本项目遵循开源开源协议，可自由学习、商用、二次开发，二次分发请保留源码版权注释。

## 项目状态
持续迭代开发中，欢迎广大开发者提交PR、Issue共建完善项目。
