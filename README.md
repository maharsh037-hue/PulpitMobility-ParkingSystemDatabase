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
