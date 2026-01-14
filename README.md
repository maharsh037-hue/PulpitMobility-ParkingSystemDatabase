# Smart Urban Parking System – Database Design
(PulpitMobility Internship Challenge | Project 3)

## 1. Problem Understanding

From a mobility platform’s perspective, parking is not just about allocating empty slots—it is a
revenue-generating operational component that directly affects user experience and city traffic
flow.

I designed this database to represent a **smart urban parking system** that can operate across
multiple parking hubs within a city. The focus of the design is on enabling **revenue visibility,
operational control, and scalable expansion**, rather than solving a single-location parking
problem.

The system supports advance bookings as well as walk-in parking, different slot categories
(including EV and reserved slots), time-based pricing, and penalty handling. These elements allow
the platform to track parking usage patterns and evaluate revenue performance at both location
and slot-type levels.

---

## 2. System Assumptions & Scope

### Key Assumptions

- I assume that a city can contain multiple parking locations managed under the same platform.
- Each parking location is divided into zones, and each zone consists of multiple parking slots.
- A user may register multiple vehicles, but a vehicle can occupy only one parking slot at a time.
- Parking slots are categorized (Regular, EV, Handicapped, Reserved), with pricing varying by
  category and location.
- Parking access can occur either through pre-booking or direct walk-in entry.
- Every completed booking results in exactly one payment record.
- Overstay and rule violations are treated as revenue-impacting events and are tracked separately.

### Scope Definition

Included in the design:
- Slot allocation and availability management
- Booking lifecycle tracking
- Revenue and payment records
- Entry and exit logging
- Violation and penalty tracking

Explicitly excluded:
- Physical hardware integration (sensors, cameras)
- Real-time vehicle tracking systems
- External traffic management systems

## 3. Entity Identification and Design Rationale

While designing the database, I first identified the core business objects that directly impact
parking operations, revenue tracking, and scalability. Each entity below exists to answer a
specific operational or analytical requirement.

### City
I introduced a City entity to allow city-level expansion and to enable revenue analysis across
different urban regions.

Attributes:
- city_id (PK)
- city_name
- state

### Parking_Location
Each parking location represents a revenue-generating unit managed by the platform.

Attributes:
- location_id (PK)
- city_id (FK)
- location_name
- address
- total_capacity

### Parking_Zone
Large parking locations are divided into zones to support granular utilization and operational
control.

Attributes:
- zone_id (PK)
- location_id (FK)
- zone_name
- floor_level

### Slot_Type
Slot types are separated into their own entity to allow flexible pricing and future expansion.

Attributes:
- slot_type_id (PK)
- slot_type_name (Regular, EV, Reserved, etc.)
- base_hourly_rate

### Parking_Slot
This entity represents the physical parking slots available within each zone.

Attributes:
- slot_id (PK)
- zone_id (FK)
- slot_type_id (FK)
- slot_number
- is_active

### User
Users represent customers interacting with the parking platform.

Attributes:
- user_id (PK)
- full_name
- phone_number
- email

### Vehicle
Vehicles are linked to users to support multiple vehicle ownership and accurate booking records.

Attributes:
- vehicle_id (PK)
- user_id (FK)
- registration_number
- vehicle_type

### Booking
A booking represents a single parking event and serves as the primary source of revenue
generation.

Attributes:
- booking_id (PK)
- user_id (FK)
- vehicle_id (FK)
- slot_id (FK)
- booking_start_time
- booking_end_time
- booking_type (Pre-booked / Walk-in)
- booking_status

### Entry_Exit_Log
This entity records actual entry and exit times to ensure accurate billing.

Attributes:
- log_id (PK)
- booking_id (FK)
- entry_time
- exit_time

### Payment_Method
Payment methods are stored separately to support payment-based revenue analysis.

Attributes:
- payment_method_id (PK)
- method_name

### Payment
Each completed booking results in one payment transaction.

Attributes:
- payment_id (PK)
- booking_id (FK)
- payment_method_id (FK)
- amount_paid
- payment_time
- payment_status

### Violation
Violations capture overstay or rule breaches that may lead to penalties.

Attributes:
- violation_id (PK)
- booking_id (FK)
- violation_type
- penalty_amount
- violation_time


## 5. Normalization Process and Design Reasoning

The database was normalized step by step to eliminate redundancy, avoid data anomalies, and ensure
that the system can scale while supporting accurate operational and revenue analytics. Each
normalization stage reflects a deliberate design decision rather than a mechanical transformation.

---

### 5.1 Unnormalized Form (UNF)

At the initial stage, parking data can exist in a single flat structure similar to spreadsheets
used in early or manual systems.

```text
UNF_PARKING_DATA

city_name
state
location_name
location_address
zone_name
floor_level
slot_number
slot_type_name
base_hourly_rate
user_name
phone_number
email
vehicle_registration_number
vehicle_type
booking_start_time
booking_end_time
booking_type
booking_status
entry_time
exit_time
payment_method
amount_paid
payment_status
violation_type
penalty_amount



5.2 First Normal Form (1NF)
To achieve First Normal Form:
All attributes were made atomic
Repeating groups were removed
Each row was made uniquely identifiable using a primary key
