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
		
		Record selection decisions in a table in the iteration document: 
		| Selected design concept | Rationale | Discarded Alternatives |
		|---|---|---|

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
  	    
		Record instantiation decisions in a table in the iteration document: 
		| Instantiation decision | Rationale |
		|---|---|

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