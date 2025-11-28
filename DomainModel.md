
# Hotel Pricing System - Domain Model

## Domain Model Class Diagram

```mermaid
classDiagram
    class User {
        <<Aggregate Root>>
        -String userId
        -String username
        -String email
        -List~Permission~ permissions
        +authenticate(credentials)
        +hasPermission(hotelId, action)
    }

    class Hotel {
        <<Aggregate Root>>
        -String hotelId
        -String name
        -String location
        -List~RoomType~ roomTypes
        -List~Rate~ availableRates
        -TaxRate taxRate
        +addRoomType(roomType)
        +removeRoomType(roomTypeId)
        +addRate(rate)
        +removeRate(rateId)
    }

    class RoomType {
        <<Entity>>
        -String roomTypeId
        -String name
        -String description
        -int capacity
    }

    class Rate {
        <<Aggregate Root>>
        -String rateId
        -String name
        -RateType rateType
        -BusinessRule calculationRule
        -boolean isFixed
        +calculatePrice(basePrice, date)
    }

    class Price {
        <<Entity>>
        -String priceId
        -String hotelId
        -String roomTypeId
        -String rateId
        -LocalDate date
        -BigDecimal amount
        +updateAmount(newAmount)
    }

    class PriceChange {
        <<Aggregate Root>>
        -String changeId
        -String userId
        -String hotelId
        -LocalDate effectiveDate
        -List~PriceUpdate~ priceUpdates
        -ChangeStatus status
        +simulate()
        +apply()
        +publishToExternalSystems()
    }

    class PriceUpdate {
        <<Value Object>>
        -String rateId
        -BigDecimal newAmount
        -PriceUpdateType type
    }

    class BusinessRule {
        <<Value Object>>
        -String ruleId
        -String expression
        -Map~String, Object~ parameters
        +evaluate(basePrice, context)
    }

    class TaxRate {
        <<Value Object>>
        -BigDecimal percentage
        -String taxCode
        -String description
    }

    class Permission {
        <<Value Object>>
        -String hotelId
        -List~String~ allowedActions
        -PermissionLevel level
    }

    class RateType {
        <<enumeration>>
        PUBLIC
        DISCOUNT
        CORPORATE
        PACKAGE
    }

    class ChangeStatus {
        <<enumeration>>
        DRAFT
        SIMULATED
        APPLIED
        PUBLISHED
    }

    class PriceUpdateType {
        <<enumeration>>
        BASE_RATE
        FIXED_RATE
    }

    class PermissionLevel {
        <<enumeration>>
        READ_ONLY
        READ_WRITE
        ADMIN
    }

    User "1" -- "*" Permission : has
    Hotel "1" -- "*" RoomType : contains
    Hotel "1" -- "*" Rate : offers
    Hotel "1" -- "1" TaxRate : has
    Rate "1" -- "1" BusinessRule : uses
    PriceChange "1" -- "*" PriceUpdate : contains
    PriceChange "1" -- "1" User : created by
    PriceChange "1" -- "1" Hotel : affects
    Price "*" -- "1" Hotel : belongs to
    Price "*" -- "1" RoomType : for
    Price "*" -- "1" Rate : for
```

## Domain Model Elements Description

| Element | Type | Description |
|---------|------|-------------|
| User | Aggregate Root | Represents system users with authentication and authorization capabilities. Manages user permissions across hotels. |
| Hotel | Aggregate Root | Represents a hotel property with its room types, available rates, and tax configuration. Serves as a boundary for price management. |
| RoomType | Entity | Defines different types of rooms available in a hotel (e.g., Standard, Suite, Deluxe). |
| Rate | Aggregate Root | Represents pricing rates with associated business rules for calculation. Can be base rates or fixed rates. |
| Price | Entity | Represents the actual price for a specific room type, rate, and date combination. |
| PriceChange | Aggregate Root | Manages the process of changing prices, including simulation and publication to external systems. |
| PriceUpdate | Value Object | Represents a single price change operation within a PriceChange aggregate. |
| BusinessRule | Value Object | Encapsulates the calculation logic for derived rates from base rates. |
| TaxRate | Value Object | Represents the tax configuration for a hotel. |
| Permission | Value Object | Defines user permissions for specific hotels and actions. |
| RateType | Enumeration | Categorizes different types of rates (Public, Discount, Corporate, Package). |
| ChangeStatus | Enumeration | Tracks the state of price change operations (Draft, Simulated, Applied, Published). |
| PriceUpdateType | Enumeration | Distinguishes between base rate and fixed rate updates. |
| PermissionLevel | Enumeration | Defines different levels of user access (Read-only, Read-write, Admin). |

## Key Domain Concepts

- **Price Management**: The core domain capability centered around changing and calculating prices
- **Rate Calculation**: Business rules that determine how prices are derived from base rates
- **Authorization**: User permissions tied to specific hotels
- **External Integration**: Publishing price changes to external systems
- **Simulation**: Ability to preview price changes before applying them

The domain model focuses on the core pricing functionality while maintaining clear boundaries between aggregates to support the high-performance, reliability, and scalability requirements.