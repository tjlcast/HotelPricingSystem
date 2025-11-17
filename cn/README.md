# 酒店价格管理系统文档

此仓库包含酒店价格管理系统的全面架构文档，该解决方案专为AD&D Hotels设计，用于管理跨多个渠道的酒店价格。文档遵循属性驱动设计过程，并实施C4模型来可视化软件架构。

## 项目结构

```
hotel-pricing-system/
├── Design/                 # 架构设计文档
│   ├── Architecture.md     # 包含详细设计的主架构文档
│   ├── DomainModel.md      # 领域模型定义
│   ├── Iteration1.md       # 第一次ADD迭代文档
│   ├── Iteration2.md       # 第二次ADD迭代文档
│   ├── Iteration3.md       # 第三次ADD迭代文档
│   ├── Iteration4.md       # 第四次ADD迭代文档
│   ├── Iteration5.md       # 第五次ADD迭代文档
│   ├── Iteration6.md       # 第六次ADD迭代文档
│   └── IterationPlan.md    # ADD迭代规划文档
├── Process/                # 流程文档
│   └── AttributeDrivenDesign.md  # ADD流程描述
└── Requirements/           # 系统需求
    └── ArchitecturalDrivers.md   # 架构驱动因素文档
```

## 文档概述

### 需求

- **ArchitecturalDrivers.md**：定义驱动酒店价格管理系统架构决策的需求、约束和质量属性。包括用户故事、质量属性场景、约束和架构关注点。

### 流程

- **AttributeDrivenDesign.md**：描述用于开发架构的属性驱动设计（ADD）方法论，包括每次迭代涉及的步骤。

### 设计

- **Architecture.md**：主架构文档，提供酒店价格管理系统的全面文档，包括：
  - 上下文和容器图
  - 每个服务的组件图
  - 关键场景的序列图
  - 接口定义
  - 事件定义
  - 带有理由的设计决策

- **DomainModel.md**：定义酒店价格管理系统的核 心领域模型，包括实体、关系及其描述。

- **Iteration1.md 至 Iteration6.md**：记录通过ADD过程的六次迭代的架构演进，重点关注不同方面：
  - 迭代1：初始系统分解和结构
  - 迭代2：安全架构
  - 迭代3：性能和可靠性
  - 迭代4：可用性和可扩展性
  - 迭代5：可部署性
  - 迭代6：可修改性、可监控性和可测试性

- **IterationPlan.md**：概述ADD迭代计划，将架构驱动因素映射到特定迭代。

## 架构的关键特性

架构文档涵盖了酒店价格管理系统的多个方面：

1. **微服务架构**：定义了具有明确服务边界的领域驱动微服务方法。

2. **云原生设计**：指定利用容器化和托管服务的云原生实现。

3. **API网关模式**：详细说明用于路由、协议转换和安全执行的API网关。

4. **事件驱动通信**：描述服务间基于事件的通信以确保数据一致性。

5. **CQRS模式**：实现命令查询责任分离以优化读写操作。

6. **安全架构**：指定与云身份服务的OAuth 2.0/OIDC集成。

7. **监控和测试基础设施**：使用OpenTelemetry定义全面的监控，使用TestContainers进行测试。

## 如何使用本文档

- 从**ArchitecturalDrivers.md**开始了解驱动架构的需求。
- 查看**AttributeDrivenDesign.md**了解设计过程。
- 探索**Architecture.md**了解系统设计的全面视图。
- 查看迭代文档（**Iteration1.md**至**Iteration6.md**）了解架构如何演进。

## 关于本项目

这是一个仅包含文档的项目，旨在演示属性驱动设计在开发全面软件架构中的应用。文档包括Mermaid语法的详细图表、接口定义以及带有理由的设计决策。

酒店价格管理系统专为AD&D Hotels设计，用于管理跨多个分销渠道的酒店房价，支持价格更改、价格查询、酒店管理、费率管理和用户管理等操作。