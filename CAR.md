
# Shared Taxi — User Story Map

## Overview

This project models a shared-taxi booking system where multiple riders can share a vehicle. The system allows riders to plan trips, match with co-riders, confirm bookings, manage no-shows, recalculate fares, and generate final receipts.

The user story map is organized around the complete rider journey and separates **MVP functionality** from later enhancements.

## User Journey

The main journey is:

**Plan Ride → Match Co-Riders → Confirm Booking → Take Shared Ride → Recalculate, Notify & Collect Fares**

## User Story Map

```mermaid
flowchart TB

    subgraph Plan["1. Plan Ride"]
        direction TB

        P1["MVP: Each rider enters their own pickup point, drop-off point, and seats needed"]
        P2["Save frequent places"]
        P3["Schedule a ride for later"]

        P1 --> P2 --> P3
    end

    subgraph Match["2. Match Co-Riders"]
        direction TB

        M1["MVP: View a proposed shared taxi route that combines two or more rider pickup and drop-off points"]
        M2["Show rider profiles and ratings"]
        M3["Suggest alternate matches"]

        M1 --> M2 --> M3
    end

    subgraph Confirm["3. Confirm Shared Booking"]
        direction TB

        C1["MVP: Join the ride and accept the estimated distance-based split for the rider's route"]
        C2["MVP: Cancel before pickup; remove the rider and recalculate the route and estimates for remaining riders"]
        C3["Add payment method"]
        C4["Chat with co-riders"]

        C1 --> C2 --> C3 --> C4
    end

    subgraph Travel["4. Take the Shared Ride"]
        direction TB

        T1["MVP: Follow the pickup and drop-off sequence; record when each rider boards and exits"]
        T2["MVP: At the scheduled pickup point, notify the rider and start a 5-minute wait period"]
        T3["Before no-show: Rider A pickup → Rider B pickup → Rider C pickup → Rider A drop-off → Rider B drop-off → Rider C drop-off"]
        T4["MVP: After 5 minutes, the driver confirms Rider B is absent"]
        T5["After no-show: Remove Rider B pickup and drop-off; Rider A pickup → Rider C pickup → Rider A drop-off → Rider C drop-off"]
        T6["MVP: Determine the revised final trip fare from the updated route"]
        T7["Live driver location and arrival alerts"]
        T8["In-app safety tools"]

        T1 --> T2 --> T3 --> T4 --> T5 --> T6 --> T7 --> T8
    end

    subgraph Settle["5. Recalculate, Notify & Collect Fares"]
        direction TB

        S1["MVP: Exclude Rider B from active riders and set their travelled distance to 0"]
        S2["MVP: Measure Rider A and Rider C travelled distances on the revised route"]
        S3["MVP Example: Revised final fare = $20.00; Rider A = 6 km; Rider C = 4 km; total active distance = 10 km"]
        S4["MVP: Rider share = revised final fare × rider distance ÷ total active distance"]
        S5["Example: Rider A pays $12.00; Rider C pays $8.00"]
        S6["MVP: Notify Riders A and C with the updated route, old estimate, revised estimate, and reason for the change"]
        S7["MVP: Require each remaining rider to acknowledge the revised fare before the trip continues"]
        S8["MVP: Apply the $5.00 no-show fee to Rider B"]
        S9["MVP: Add a $1.00 platform fee per charged rider and 10% tax on each rider's subtotal"]
        S10["MVP: Generate final receipts"]

        RA["Receipt — Rider A<br/>Active rider; 6 km travelled<br/>Trip share: $12.00<br/>Platform fee: $1.00<br/>Tax (10%): $1.30<br/>Amount charged: $14.30"]

        RB["Receipt — Rider B<br/>No-show; 0 km travelled<br/>Trip fare share: $0.00<br/>No-show fee: $5.00<br/>Platform fee: $1.00<br/>Tax (10%): $0.60<br/>Amount charged: $6.60"]

        RC["Receipt — Rider C<br/>Active rider; 4 km travelled<br/>Trip share: $8.00<br/>Platform fee: $1.00<br/>Tax (10%): $0.90<br/>Amount charged: $9.90"]

        S11["Support tips, discounts, and disputes"]

        S1 --> S2 --> S3 --> S4 --> S5 --> S6 --> S7 --> S8 --> S9 --> S10

        S10 --> RA
        S10 --> RB
        S10 --> RC

        RA --> S11
        RB --> S11
        RC --> S11
    end

    P1 --> M1 --> C1 --> T1 --> S1

    T6 --> S1
    T5 -->|Updated route| S2

    P2 -.-> M2 -.-> C2 -.-> T2 -.-> S3
    P3 -.-> M3 -.-> C3 -.-> T3 -.-> S4

    classDef mvp fill:#f0fdf4,stroke:#4ade80,color:#052e16,stroke-width:3px
    classDef later fill:#f0fdfa,stroke:#2dd4bf,color:#042f2e

    class P1,M1,C1,C2,T1,T2,T3,T4,T5,T6,S1,S2,S3,S4,S5,S6,S7,S8,S9,S10,RA,RB,RC mvp
    class P2,P3,M2,M3,C3,C4,T7,T8,S11 later
```

## MVP Scope

The MVP focuses on the complete end-to-end shared-ride journey:

1. **Plan the ride**
2. **Match co-riders**
3. **Confirm the shared booking**
4. **Take the shared ride**
5. **Handle a no-show**
6. **Recalculate the route**
7. **Recalculate rider fares**
8. **Notify remaining riders**
9. **Apply the no-show fee**
10. **Generate final receipts**

## Fare Calculation

The MVP uses a distance-based allocation:

```text
Rider Share =
Revised Final Fare × Rider Distance
÷ Total Active Rider Distance
```

### Example

```text
Revised final fare = $20.00

Rider A = 6 km
Rider C = 4 km

Total active distance = 10 km

Rider A:
$20 × 6 ÷ 10 = $12.00

Rider C:
$20 × 4 ÷ 10 = $8.00
```

## No-Show Scenario

If Rider B does not arrive within the **5-minute waiting period**:

* Rider B is removed from the active riders.
* Rider B's travelled distance becomes `0 km`.
* Rider B's pickup and drop-off are removed from the route.
* The route is recalculated for Riders A and C.
* The final fare is recalculated.
* Riders A and C receive updated fare information.
* Rider B receives a `$5.00` no-show fee.
* Final receipts are generated for all riders.

## Future Enhancements

Features outside the MVP include:

* Saving frequent places
* Scheduled rides
* Rider profiles and ratings
* Alternative co-rider matching
* Payment methods
* In-app chat
* Live driver location
* Arrival alerts
* In-app safety tools
* Support for tips, discounts, and disputes

## Key Agile / SAD Concepts Demonstrated

This user story map demonstrates:

* **User Story Mapping**
* **MVP / Walking Skeleton**
* **End-to-End User Journey**
* **Acceptance Criteria**
* **Business Rules**
* **Event-driven thinking**
* **Continuous value delivery**
* **Requirements decomposition**
* **Vertical slicing**
* **Fare recalculation**
* **Exception handling**

## Technology / Documentation

The diagram uses **Mermaid**, which can be rendered directly by GitHub inside a Markdown file.
