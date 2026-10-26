# ER Diagram Workshop – Submission Template

## Objective
To understand and apply ER modeling concepts by creating ER diagrams for real-world applications.

## Purpose
Gain hands-on experience in designing ER diagrams that represent database structure including entities, relationships, attributes, and constraints.

---

# Scenario A: City Fitness Club Management

**Business Context:**  
FlexiFit Gym wants a database to manage its members, trainers, and fitness programs.

**Requirements:**  
- Members register with name, membership type, and start date.  
- Each member can join multiple programs (Yoga, Zumba, Weight Training).  
- Trainers assigned to programs; a program may have multiple trainers.  
- Members may book personal training sessions with trainers.  
- Attendance recorded for each session.  
- Payments tracked for memberships and sessions.

### ER Diagram:

<img width="1280" height="720" alt="1" src="https://github.com/user-attachments/assets/fe695b5f-4320-4294-9df4-6d9f0bb4d753" />

<img width="1128" height="836" alt="image" src="https://github.com/user-attachments/assets/16f2e663-4345-4183-a406-66c5a00e1eca" />


### Entities and Attributes

| Entity | Attributes (PK, FK) | Notes |
|--------|--------------------|-------|
| Member | **MemberID (PK)**, Name, MembershipType, StartDate | Registered gym member |
| Program | **ProgramID (PK)**, ProgramName, Fee | Yoga, Zumba, Weight Training, etc. |
| Trainer | **TrainerID (PK)**, Name, Specialization, Phone | Staff who teach programs and give personal training |
| Session | **SessionID (PK)**, MemberID (FK), TrainerID (FK), SessionDate, Time | A personal training session booked by a member |
| Attendance | **AttendanceID (PK)**, SessionID (FK), Status, CheckInTime | Records whether the member attended a session |
| Payment | **PaymentID (PK)**, MemberID (FK), SessionID (FK, nullable), Amount, PaymentType, PaymentDate | PaymentType = Membership or Session; SessionID is empty for membership payments |
| Enrollment *(junction)* | **MemberID (PK, FK)**, **ProgramID (PK, FK)**, JoinDate | Resolves Member–Program M:N |
| Program_Trainer *(junction)* | **ProgramID (PK, FK)**, **TrainerID (PK, FK)** | Resolves Trainer–Program M:N |

### Relationships and Constraints

| Relationship | Cardinality | Participation | Notes |
|--------------|------------|---------------|-------|
| Member joins Program | M:N | Member: optional, Program: optional | Implemented through Enrollment |
| Trainer is assigned to Program | M:N | Trainer: total, Program: total | Every program has at least one trainer; implemented through Program_Trainer |
| Member books Session | 1:M | Member: optional, Session: total | A session must belong to one member |
| Trainer conducts Session | 1:M | Trainer: optional, Session: total | A session must have one trainer |
| Session has Attendance | 1:M | Session: optional, Attendance: total | Attendance cannot exist without a session |
| Member makes Payment | 1:M | Member: optional, Payment: total | Every payment is made by one member |
| Payment pays for Session | M:1 | Payment: optional, Session: optional | Only session payments link to a session |

### Assumptions
- A member has exactly one membership type at a time (e.g., Monthly, Quarterly, Annual).
- A personal training session is one member with one trainer.
- Membership payments are linked only to the member; session payments are linked to both member and session.

---

# Scenario B: City Library Event & Book Lending System

## Business Context

The Central Library wants to manage book lending, cultural events, room bookings, and overdue fines.

## Requirements

- Members borrow books, with loan and return dates tracked.
- Each book has title, author, and category.
- Library organizes events; members can register.
- Each event has one or more speakers/authors.
- Rooms are booked for events and study.
- Overdue fines apply for late returns.

## ER Diagram
<img width="1280" height="720" alt="2" src="https://github.com/user-attachments/assets/33d0a3d3-e26e-44d9-9012-9fef0313cea8" />

<img width="1270" height="868" alt="Screenshot 2026-07-28 122037" src="https://github.com/user-attachments/assets/fb449ec6-49bc-4651-bd8b-31a7a9e8a594" />

---

## Entities and Attributes

| Entity | Attributes (PK, FK) | Notes |
|--------|----------------------|-------|
| **Member** | **MemberID (PK)**, Name, Phone | Stores library member details. |
| **Book** | **BookID (PK)**, Title, ISBN, CategoryID (FK) | Stores books available in the library. |
| **Category** | **CategoryID (PK)**, CategoryName, Description | Classifies books into categories. |
| **Loan** | **LoanID (PK)**, MemberID (FK), BookID (FK), LoanDate, DueDate, ReturnDate | Tracks borrowed books. |
| **Fine** | **FineID (PK)**, LoanID (FK), Amount, Status | Records overdue fines. |
| **Event** | **EventID (PK)**, EventName, EventDate | Stores library event details. |
| **Speaker** | **SpeakerID (PK)**, Name, Specialization | Stores event speaker details. |
| **EventRegistration** | **RegID (PK)**, MemberID (FK), EventID (FK), RegDate | Records member registrations for events. |
| **Room** | **RoomID (PK)**, RoomName, Capacity | Stores study/event room details. |
| **RoomBooking** | **BookingID (PK)**, RoomID (FK), BookingDate, Purpose | Tracks room bookings. |

---

## Relationships and Constraints

| Relationship | Cardinality | Participation | Notes |
|--------------|------------|---------------|-------|
| Member → Loan | 1 : M | Total (Loan) | A member can borrow multiple books. |
| Book → Loan | 1 : M | Partial | A book may be borrowed multiple times over its lifetime. |
| Loan → Fine | 1 : 0..1 | Partial | A fine exists only for overdue loans. |
| Category → Book | 1 : M | Partial | Each category contains multiple books. |
| Event → EventRegistration | 1 : M | Total (EventRegistration) | Members register for events. |
| Member → EventRegistration | 1 : M | Partial | A member can register for multiple events. |
| Event → Speaker | M : N | Partial | Events may have multiple speakers and speakers may participate in multiple events. |
| Room → RoomBooking | 1 : M | Partial | Rooms can be booked multiple times. |

---

## Assumptions

- Each book belongs to one category.
- A fine is generated only when the return date exceeds the due date.
- Members can register for multiple events.
- Rooms may be booked either for events or study purposes.
- A speaker can participate in multiple events.

---

# Scenario C: Restaurant Table Reservation & Ordering

## Business Context

A popular restaurant wants to manage reservations, food ordering, billing, and waiter assignments efficiently.

## Requirements

- Customers can reserve tables or walk in.
- Each reservation includes the date, time, and number of guests.
- Customers place food orders linked to reservations.
- Each order contains multiple dishes.
- Dishes belong to categories such as Starter, Main Course, and Dessert.
- Bills are generated for each reservation, including food and service charges.
- Waiters are assigned to serve reservations.

## ER Diagram
<img width="1280" height="720" alt="3" src="https://github.com/user-attachments/assets/b1ca0f2b-cbbb-4dd8-a800-425ad7561a0a" />

<img width="1253" height="859" alt="Screenshot 2026-07-28 122057" src="https://github.com/user-attachments/assets/2085f3a6-ad55-43aa-813f-41f6c3f052ef" />

---

## Entities and Attributes

| Entity | Attributes (PK, FK) | Notes |
|--------|----------------------|-------|
| **Customer** | **CustomerID (PK)**, CustomerName, Phone, Email | Stores customer information. |
| **Table** | **TableID (PK)**, TableNumber, Capacity, Status | Represents restaurant tables. |
| **Waiter** | **WaiterID (PK)**, WaiterName, Phone | Stores waiter details. |
| **Reservation** | **ReservationID (PK)**, CustomerID (FK), TableID (FK), WaiterID (FK), ReservationDate, ReservationTime, NumberOfGuests, ReservationType | Represents table reservations or walk-in customers. |
| **FoodOrder** | **OrderID (PK)**, ReservationID (FK), OrderTime | Stores orders placed for a reservation. |
| **OrderItem** | **OrderItemID (PK)**, OrderID (FK), DishID (FK), Quantity, SubTotal | Represents individual dishes in an order. |
| **Dish** | **DishID (PK)**, DishName, Price, CategoryID (FK) | Stores menu items. |
| **DishCategory** | **CategoryID (PK)**, CategoryName | Categorizes dishes (Starter, Main Course, Dessert). |
| **Bill** | **BillID (PK)**, ReservationID (FK), FoodAmount, ServiceCharge, TotalAmount, PaymentStatus | Stores billing information for each reservation. |

---

## Relationships and Constraints

| Relationship | Cardinality | Participation | Notes |
|--------------|------------|---------------|-------|
| Customer → Reservation | 1 : M | Total (Reservation), Partial (Customer) | A customer can have multiple reservations. |
| Table → Reservation | 1 : M | Partial | A table can be reserved multiple times on different dates/times. |
| Waiter → Reservation | 1 : M | Partial | One waiter can serve multiple reservations. |
| Reservation → FoodOrder | 1 : M | Total (FoodOrder) | Each food order belongs to one reservation. |
| FoodOrder → OrderItem | 1 : M | Total (OrderItem) | Each order consists of one or more order items. |
| Dish → OrderItem | 1 : M | Partial | A dish can appear in many order items. |
| DishCategory → Dish | 1 : M | Partial | Each dish belongs to one category. |
| Reservation → Bill | 1 : 1 | Total | Every reservation generates exactly one bill. |

---

## Assumptions

- A reservation is associated with exactly one customer, one table, and one waiter.
- Walk-in customers are also recorded as reservations with `ReservationType = Walk-in`.
- A reservation may contain multiple food orders.
- Each food order contains one or more order items.
- Each dish belongs to exactly one category.
- Every reservation generates exactly one bill after service.
- Service charges are included in the final bill.

---

## Instructions for Students

1. Complete **all three scenarios** (A, B, C).  
2. Identify entities, relationships, and attributes for each.  
3. Draw ER diagrams using **draw.io / diagrams.net** or hand-drawn & scanned.  
4. Fill in all tables and assumptions for each scenario.  
5. Export the completed Markdown (with diagrams) as **a single PDF**
