# Hotel Pricing System - Iteration 2

This document tracks the progress of the 2nd iteration of the architecture design process for the Hotel Pricing System, following the Attribute-Driven Design (ADD) methodology.

## Iteration Goal

According to the Iteration Plan, the goal of Iteration 2 is to:

**Implement performance optimization and reliability patterns to meet strict 100ms price publication SLA and ensure 100% successful price delivery to external systems.**

## Drivers to Address

The following drivers will be addressed in this iteration:

- QA-1: Performance - 100ms price publication requirement for base rate changes (High priority)
- QA-2: Reliability - 100% successful price publication to external systems (High priority)
- QA-4: Scalability - Support 100k-1M daily price queries with <20% latency impact (High priority)
- QA-8: Monitorability - Collect performance and reliability metrics for price publication (Medium priority)
- HPS-2: Change Prices - Core functionality that drives performance and reliability requirements
- CON-2: Cloud hosting environment
- CON-5: REST APIs for external system integration
- CON-6: Cloud-native approach
- CRN-4: Avoid technical debt while implementing performance solutions

## ADD Process Steps

### Step 1: Review Inputs

**Design Purpose**: Address high-priority performance and reliability requirements for the Hotel Pricing System.

**Primary Functional Requirements**:
- HPS-2: Change Prices - Core business function requiring 100ms publication performance (QA-1)

**Quality Attribute Scenarios**:
- QA-1: Performance - 100ms price publication requirement for base rate changes
- QA-2: Reliability - 100% successful price publication to external systems
- QA-4: Scalability - Support 100k-1M daily price queries with minimal latency impact
- QA-8: Monitorability - Collect performance and reliability metrics for price publication

**Design Constraints**:
- CON-2: Cloud hosting environment
- CON-5: REST APIs for external system integration
- CON-6: Cloud-native approach

**Architectural Concerns**:
- CRN-4: Avoid introducing technical debt
- CRN-5: Set up continuous deployment infrastructure

The inputs are sufficient and build upon the foundation established in Iteration 1. The performance requirement (QA-1) is particularly challenging with the 100ms publication SLA, and reliability (QA-2) requires 100% success rate for price changes.

### Step 2: Establish goal for the iteration by selecting drivers

The goal and drivers for this iteration have been defined above, focusing on implementing performance optimization and reliability patterns for critical business operations.

The selected drivers address:
- QA-1: Strict 100ms publication SLA for price changes that directly impact business operations
- QA-2: 100% reliability requirement for price delivery to prevent financial losses
- QA-4: Scalability to support growing query volumes with minimal performance degradation
- QA-8: Monitoring capabilities to track performance and reliability metrics
- HPS-2: Core price change functionality that drives these quality requirements
- CON-2/5/6: Technical platform constraints that enable the performance solutions
- CRN-4: Architectural concern to avoid technical debt in performance implementations

### Step 3: Choose one or more elements of the system to refine

Based on the Iteration 1 foundation and the focus on performance and reliability, the following elements will be refined in this iteration:

1. **Pricing Service**: This element handles core price change operations and is critical for meeting the 100ms publication SLA (QA-1) and 100% reliability (QA-2). The current design from Iteration 1 needs optimization for performance.

2. **Query Service**: This element must support high-volume price queries (QA-4) and requires caching strategies and performance optimization to handle 100k-1M daily queries.

3. **Message Broker Integration**: The event-driven communication layer needs reliability patterns to ensure 100% successful price delivery to external systems (QA-2).

4. **Monitoring Infrastructure**: To address QA-8 (Monitorability), we need to implement performance and reliability metric collection for price publication operations.

### Step 4: Choose one or more design concepts that satisfy the selected drivers

| Selected design concept | Rationale | Discarded Alternatives |
|---|---|---|
| **In-Memory Caching (Redis)** | - Provides sub-millisecond read performance for price queries (QA-4)<br>- Reduces database load for high-volume queries<br>- Supports cache invalidation patterns for price updates<br>- Cloud-native and scalable (CON-6) | - **Database-level caching**: Limited performance gains and database dependency<br>- **Application-level caching**: Memory constraints and cache coherency issues |
| **Message Queue with Dead Letter Queue (RabbitMQ)** | - Ensures reliable message delivery with retry mechanisms (QA-2)<br>- Supports asynchronous processing for 100ms SLA (QA-1)<br>- Provides delivery guarantees and fault tolerance<br>- Enables monitoring of message flow (QA-8) | - **Direct HTTP calls**: No reliability guarantees and synchronous blocking<br>- **Simple message broker**: Lacks advanced reliability features |
| **Database Read Replicas** | - Offloads read traffic from primary database (QA-4)<br>- Improves query performance for price lookups<br>- Supports horizontal scaling for read operations<br>- Cloud-native implementation (CON-6) | - **Database sharding**: Overly complex for current scale requirements<br>- **Additional database instances**: Higher cost and management overhead |
| **Performance Monitoring with Metrics Collection** | - Enables real-time performance tracking (QA-8)<br>- Provides visibility into 100ms SLA compliance (QA-1)<br>- Supports reliability monitoring (QA-2)<br>- Cloud-native monitoring integration | - **Log-based monitoring**: Delayed insights and complex analysis<br>- **Custom monitoring solution**: Higher development and maintenance cost |
| **Circuit Breaker Pattern** | - Prevents cascading failures in external system integrations (QA-2)<br>- Improves system resilience and reliability<br>- Provides graceful degradation when external systems fail<br>- Supports monitoring of integration health (QA-8) | - **Retry-only approach**: Can exacerbate failures and increase latency<br>- **Timeout-only approach**: Limited failure handling capabilities |

### Step 5: Instantiate architectural elements, sketch views, allocate responsibilities, and define interfaces

Based on the selected design concepts, we will now refine the architectural elements identified in Step 3 and allocate specific responsibilities to each component.

#### Pricing Service Refinement

| Instantiation decision | Rationale |
|---|---|
| **Price Calculation Engine** | Optimized component for fast rate calculations using pre-compiled business rules to meet 100ms SLA (QA-1) |
| **Price Change Orchestrator** | Coordinates price change workflow with asynchronous processing and immediate cache updates for performance |
| **Circuit Breaker for External Systems** | Prevents cascading failures when publishing to external systems, ensuring system reliability (QA-2) |

#### Query Service Refinement

| Instantiation decision | Rationale |
|---|---|
| **Redis Cache Cluster** | In-memory caching layer for price data to support high-volume queries with low latency (QA-4) |
| **Cache Manager** | Handles cache invalidation and synchronization when prices change, maintaining data consistency |
| **Query Optimizer** | Routes queries to appropriate data sources (cache vs database) based on performance requirements |

#### Message Infrastructure Refinement

| Instantiation decision | Rationale |
|---|---|
| **RabbitMQ with DLQ Configuration** | Message broker with dead letter queues for reliable price publication with retry mechanisms (QA-2) |
| **Message Reliability Service** | Ensures message delivery guarantees and handles failed publication attempts |

#### Monitoring Infrastructure

| Instantiation decision | Rationale |
|---|---|
| **Performance Metrics Collector** | Collects timing data for price publication operations to monitor 100ms SLA compliance (QA-1, QA-8) |
| **Reliability Dashboard** | Tracks successful/failed price publications and external system integration health (QA-2, QA-8) |

#### Component Responsibilities

**Price Calculation Engine**:
- Execute business rules for rate calculations with sub-10ms performance
- Pre-compile calculation rules for optimal execution speed
- Handle concurrent price calculations for multiple rates

**Price Change Orchestrator**:
- Coordinate the complete price change workflow
- Update cache immediately after price calculation (before database commit)
- Trigger asynchronous publication to external systems
- Monitor end-to-end timing for SLA compliance

**Redis Cache Cluster**:
- Store current price data with sub-millisecond access
- Support cache partitioning by hotel and date ranges
- Handle cache warming during system startup
- Provide failover capabilities for high availability

**Cache Manager**:
- Invalidate cache entries when prices change
- Handle cache synchronization across multiple instances
- Manage cache TTL and eviction policies
- Monitor cache hit rates and performance

**RabbitMQ with DLQ**:
- Guarantee message delivery to external systems
- Retry failed publications with exponential backoff
- Route permanently failed messages to dead letter queue for manual intervention
- Provide message durability and persistence

**Performance Metrics Collector**:
- Track price publication timing from request to external system confirmation
- Monitor cache hit rates and query performance
- Alert on SLA violations and performance degradation
- Integrate with cloud-native monitoring services

#### Interface Definitions

1. **Optimized Pricing API** (REST)
   - `POST /api/v2/prices/changes` - Submit price change with immediate cache update
   - `GET /api/v2/prices/changes/{id}/metrics` - Get performance metrics for price change

2. **High-Performance Query API** (REST)
   - `GET /api/v2/prices/cached?hotelId={id}&date={date}` - Query prices from cache with fallback
   - `GET /api/v2/prices/bulk` - Bulk price query with optimized response format

3. **Cache Management API** (Internal)
   - `POST /internal/cache/invalidate` - Invalidate cache entries for specific hotels/dates
   - `GET /internal/cache/stats` - Get cache performance statistics

4. **Monitoring API** (Internal)
   - `POST /internal/metrics/publication` - Record price publication timing and success metrics
   - `GET /internal/metrics/sla` - Get SLA compliance statistics

5. **Message Reliability API** (Internal)
   - `POST /internal/messages/retry/{messageId}` - Manually retry failed message delivery
   - `GET /internal/messages/dlq` - List messages in dead letter queue

### Step 6: Record design decisions

| Driver | Decision | Rationale |
|---|---|---|
| **QA-1 (Performance)** | Implement Redis caching with immediate cache updates during price changes | Provides sub-millisecond query performance and enables meeting 100ms publication SLA by updating cache before database commit |
| **QA-2 (Reliability)** | Use RabbitMQ with Dead Letter Queues and Circuit Breaker pattern | Ensures 100% successful price delivery with retry mechanisms and prevents cascading failures in external system integrations |
| **QA-4 (Scalability)** | Deploy Redis cluster and database read replicas | Supports 1M+ daily queries with minimal latency impact through horizontal scaling of read operations |
| **QA-8 (Monitorability)** | Implement performance metrics collection and reliability dashboard | Provides real-time visibility into SLA compliance and publication success rates for operational monitoring |
| **HPS-2 (Change Prices)** | Optimize Price Calculation Engine with pre-compiled business rules | Reduces calculation time to meet 100ms SLA while maintaining complex rate calculation capabilities |
| **CON-2/6 (Cloud-native)** | Use managed Redis and RabbitMQ services | Leverages cloud provider reliability and scalability while minimizing operational overhead |
| **CRN-4 (Technical Debt)** | Implement proper cache invalidation and circuit breaker patterns | Avoids short-term solutions that would create maintenance issues and reliability problems |

### Step 7: Perform analysis of current design and review iteration goal and achievement of design purpose

In this step, we analyze if the design decisions made during the iteration were sufficient to address the drivers associated with the iteration goal. The following table summarizes the analysis:

| Driver | Analysis result |
|---|---|
| **QA-1: Performance (100ms publication)** | **Satisfied**: Redis caching with immediate updates, optimized calculation engine, and asynchronous processing enable sub-100ms price publication. Cache updates occur before database commit to meet SLA. |
| **QA-2: Reliability (100% successful publication)** | **Satisfied**: RabbitMQ with DLQ provides delivery guarantees, circuit breaker prevents cascading failures, and manual retry capability ensures eventual success for all price changes. |
| **QA-4: Scalability (100k-1M daily queries)** | **Satisfied**: Redis cluster and database read replicas support horizontal scaling for read operations, with cache hit optimization maintaining <20% latency impact at high volumes. |
| **QA-8: Monitorability (performance/reliability metrics)** | **Satisfied**: Performance metrics collector and reliability dashboard provide comprehensive monitoring of SLA compliance and publication success rates. |
| **HPS-2: Change Prices** | **Satisfied**: Enhanced with performance optimizations while maintaining core functionality and simulation capabilities from Iteration 1. |

#### Overall Iteration Assessment

1. **Key Achievement**: Successfully addressed the challenging 100ms publication SLA through immediate cache updates and optimized calculation engine
2. **Key Achievement**: Implemented comprehensive reliability patterns with guaranteed message delivery and failure prevention mechanisms
3. **Key Achievement**: Established scalable infrastructure capable of handling 1M+ daily queries with minimal performance degradation

**Remaining Risks/Issues**:
- Cache coherency risk between immediate cache updates and database commits requires careful transaction management
- Dead letter queue management introduces operational overhead for failed message handling
- Performance monitoring adds slight overhead to price publication operations

**Recommendation for Next Iteration**:
Proceed to Iteration 3 focusing on availability (QA-3) and deployability (QA-7) requirements, with specific attention to:
- High-availability patterns for all services
- Deployment pipeline automation and environment configuration
- Testing strategy implementation for the performance and reliability patterns established in this iteration

The design decisions made in this iteration successfully build upon the microservices foundation from Iteration 1 while specifically targeting the critical performance and reliability drivers that impact business operations.