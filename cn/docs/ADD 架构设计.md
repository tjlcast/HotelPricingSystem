## ADD 架构驱动设计的样例
	- [HotelPricingSystem](https://github.com/otrebmuh/HotelPricingSystem)
- ## DDD 领域模型设计
	- 请参考@ArchitecturalDrivers.md文档，采用领域驱动设计（DDD）方法为系统创建领域模型。将此领域模型创建在@Design文件夹下的DomainModel.md文档中。使用Mermaid格式的类图来表示领域模型。需包含一个表格，描述领域模型的每个元素及其类型（聚合根、实体、值对象）。在类图中使用构造型为类分配元素类型。
	- Consider the @ArchitecturalDrivers.md and create a domain model for the system using DDD. Create this domain model in a DomainModel.md document in the @Design folder. Represent the domain model using a class diagram using mermaid format. Include a table that describes each element of the domain model and their type (Aggregate Root, Entity, Value Object). Use stereotypes in the class diagram to assign the type of element to the classes.
- ### ADD 架构迭代计划
	- 请参考@ArchitecturalDrivers.md中的需求优先级和@DomainModel.md中的领域模型，根据@AttributeDrivenDesign.md描述的设计流程，制定系统的迭代设计计划。请为每次迭代描述其主要目标。首次迭代应侧重于系统的初始结构搭建，后续迭代则应聚焦于解决高优先级的驱动因素。务必确保早期迭代能处理直接支撑业务的需求。请以表格形式呈现迭代计划，表格应包含迭代编号、迭代目标以及待解决的驱动因素列表。将迭代计划输出到@Design文件夹下的IterationPlan.md文件中。
	- Consider the requirement priorities in @ArchitecturalDrivers.md and the domain model @DomainModel.md. Create an iteration plan for the design of the system, according to the @AttributeDrivenDesign.md process. Describe, for each iteration, what the main goal will be. The first iteration should be focused on initially structuring the system. The following iterations should be focused on addressing high priority drivers. Be sure to address requirements that directly support the business in early iterations. Present the iteration plan as a table with iteration number, goal and list of drivers to be addressed. Output the iteration plan to an IterationPlan.md file in the @Design folder.
- ## ADD 架构设计流程
	- ### Step 1：Review Inputs（审查设计输入）
		- **说明**：在每次迭代开始前，先审查本次迭代所依赖的设计输入，包括：
			- 设计目的（Design Purpose）——例如：“本次迭代的目标是建立基础架构框架”、“验证某个质量属性达成情况”等。
			- 主要功能需求（Primary Functional Requirements）
			- 质量属性情景（Quality Attribute Scenarios，如可用性、性能、可修改性、安全性等）, 通常通过包含具体指标的场景来描述。
			- 设计约束（Constraints，例如技术栈、部署平台、法规要求等）
			- 架构关注点（Architectural Concerns，例如日志、安全、缓存、可扩展性等）, 指那些通常不属于需求范畴、但需要做出设计决策的开发维度（例如系统结构、错误日志记录）。
		- **结果产出**：确认这些输入是否充分、是否需要补充或澄清，为下一步奠定基础。
	- ### Step 2：Establish Iteration Goal and Select Inputs（确定迭代目标并选择驱动因素）
		- **说明**：基于 Step 1 审查的输入，明确本次迭代的 **迭代目标（Iteration Goal）**，即本次要解决的问题、实现的成果、要满足的关键驱动（drivers）。同样，需要从输入中挑选出 **本次迭代重点考虑的驱动因素/架构驱动（architectural drivers）**，比如优先的质量属性、关键功能、制约条件等。
		- **结果产出**：
			- 明确的迭代目标描述（例如：“本次迭代聚焦于提高系统的可用性至 99.9%”）
			- 本次迭代所选择的输入驱动清单（功能、质量属性、约束……）
	- ### Step 3：Choose One or More Elements to Refine（选择一个或多个系统元素作为细化对象）
		- **说明**：在明确了迭代目标后，选择系统中一个或多个 **元素（elements）**（可以是系统本身或系统内的子系统/模块/Service）作为本次迭代要细化的对象。你要决定 “本次迭代我要改进哪个系统元素” 或 “我要细化哪个子系统” 等。当设计新系统时，首次迭代会将整个系统本身视作待细化的单一构件。
		- **结果产出**：
			- 所选元素的名称／标识（例如 “订单服务模块” 或 “用户认证子系统”）
			- 该元素与驱动之间的关联（为何选这个元素、它承担哪些驱动）
			- 若该元素已有结构，可附上其当前状态简介
	- ### Step 4：Choose One or More Design Concepts That Satisfy the Selected Drivers（选择一个或多个设计概念以满足所选驱动）
		- **说明**：在此步骤中，需选择相应的设计概念以实现迭代目标，这些设计概念可包括：
			- 设计模式：含参考架构、架构模式、设计模式及部署模式
			- 外部开发组件：包括各类框架或特定的云资源
			- 设计策略：经实践验证的、用于实现特定质量属性的方法
		- **结果产出**：
			- 所选的设计模式。
			- 选择该设计模式的理由。
			- 被舍弃的替代方案
	- ### Step 5：Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces（实例化架构元素、分配职责、定义接口）
		- **说明**：将 Step 4 中选定的设计概念具体化，作用于 Step 3中所选需要细化的元素。你可能要：
			- 创建或调整元素（模块、组件、服务等）
			- 给这些元素分配职责（谁负责什么）
			- 定义它们之间的接口（通道、协议、数据格式、边界）
			- 同时，更新架构文档中的视图（例如逻辑视图、部署视图等）来反映这些变更。
		- **结果产出**：
			- 实例化决策（创造/调整模块、组件、服务）
			- 实例化决策的理由
			- 给这些实例化的元素分配职责
			- 接口定义
	- ### Step 6：Record Design Decisions（记录设计决策）
		- **说明**：在元素实例化后，记录此次迭代所做的关键设计决策（包括为什么选、为什么弃、可能的风险、仍待决策项等）。
		- **结果产出**：
			- 驱动因素、设计决策、理由
	- ### Step 7: Perform analysis of current design and review iteration goal and achievement of design purpose（分析当前设计并复查迭代目标与达成情况）
		- **说明**：对本次迭代的设计成果进行 **回顾性分析**，核查：本次迭代是否达成了在 Step 2 所定的目标？所选驱动是否已被适当处理？
		- **结果产出**：
			- 迭代目标达成状态（已达、部分达成、未达）
			- 剩余风险／问题列表
			- 对下次迭代的建议（是否继续、下一次聚焦点是什么）
- ## ADD 迭代文档整合
	- ### 创建架构文档骨架
		- ```markdown
		  Create an skeleton of the architecture document in the @Design folder, the
		  structure of the document is as follows and inside there are instructions of what
		  you should include for this initial version:
		  
		  1.- Introduction
		  Create a description of the document
		  
		  2.- Context diagram
		  Include the context diagram from the @ArchitecturalDrivers.md document. Include a
		  paragraph at the beginning that describes what this diagram shows.
		  
		  3.- Architectural drivers
		  Include a summary of the drivers described in @ArchitecturalDrivers.md, including
		  their priorities. You should separate user stories, quality attribute scenarios,
		  concerns and constraints in separate tables.
		  
		  4.- Domain model
		  Include the domain model you created in @DomainModel.md
		  
		  5.- Container diagram
		  This section contains the main container diagram, according to the C4 approach.
		  Containers include high-level applications or data stores that run within your
		  system, including frontends, databases, message queues, web applications
		  microservices. Create an empty diagram, include a paragraph at the beginning that
		  describes what this diagram is.
		  This section should also include a table with the name of the container and its
		  responsibilities.
		  
		  6.- Component diagrams
		  Only include a paragraph that explains that for each container from the previous
		  section that we will develop, we will include a subsection with a component diagram
		  that will detail the internal design of the container. Each component diagram should
		  have an associated table with the name of the components and their responsibilities.
		  Don't include anything else in the document right now.
		  
		  7.- Sequence diagrams
		  For each use case or quality attribute scenario we will create a sequence diagram.
		  Create a subsection for each of the use case and quality attribute scenario drivers
		  that are mentioned in the @IterationPlan.md.
		  Include empty sequence diagrams for the moment being.
		  
		  8.- Interfaces
		  This section will include details about contracts, leave empty for the moment being.
		  
		  9.- Design decisions
		  This section describes the relevant design decisions that resulted in this design.
		  The section should only include an empty table with the columns driver, decision,
		  rationale and discarded alternative.
		  
		  ```