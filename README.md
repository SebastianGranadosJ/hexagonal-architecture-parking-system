# 🚗 Parking Management System – Hexagonal Architecture (Ports & Adapters)

This project implements a **Parking Management System** following a strict **Hexagonal Architecture** (Ports & Adapters).  
The system was developed based on the functional requirements designed for the automated parking solution of Movies UPB, a fictional cinema created for academic purposes.
The architecture ensures **high modularity**, **easy testability**, and **clear separation of concerns** across the three main layers:

- **Domain**: Core business logic and entities  
- **Application**: Services and Use Cases (Driver Ports)  
- **Infrastructure**: Controllers, repositories, database, and adapters (Driven Ports)

---

## 🧱 Architecture Diagram

Below is the conceptual model used for the project:

![Hexagonal Architecture](./hexa.png)

> **Note:** The diagram shows the structure of Domain, Application, and Infrastructure, along with Driver/Driven ports and adapters.

---

# 📘 Functionalities Implemented

The system fully satisfies the requirements described in the project specification (You can find it in the /docs folder.).  
Below is the list of **Functional Requirements (RF)** and how they are implemented:

---

## ✅ **RF-01: Vehicle Entrance Registration**
- Exposes an endpoint for the entry machine.  
- Receives the vehicle license plate and type (car/motorcycle).  
- Stores:
  - Plate  
  - Vehicle type  
  - Exact timestamp of entry  
- Capacity limits:
  - **38 car spots**  
  - **26 motorcycle spots**

### Example Endpoint  
`POST /api/v1.0/estacionamiento/ingresos`

---

## ✅ **RF-02: Exit and Payment Calculation**
The system processes the vehicle exit through an endpoint that:

1. Receives the vehicle plate  
2. Validates if the vehicle belongs to a **store client**:
   - If **store client → payment = $0**
3. If not a store client:
   - Calculates total time in minutes
   - Applies the corresponding per-minute rate
   - Adds **19% tax (IVA)**
4. Marks the parking session as **finalized**
5. Registers exit timestamp

### Example Endpoint  
`POST /api/v1.0/estacionamiento/salidas`

---

## ✅ **RF-03: Service Rates**
Rates are configured as:

| Vehicle Type | Cost per Minute |
|--------------|-----------------|
| Car          | **$88**         |
| Motorcycle   | **$66**         |

These values are applied dynamically by the domain logic.

---

## ✅ **RF-04: Daily Revenue Report**
The system provides an endpoint to compute a **daily financial report**, including:

- Total revenue collected during the day  
- A detailed list of all vehicles that completed their stay, including:  
  - License plate  
  - Vehicle type  
  - Whether the driver was a store client  
  - Final payment amount  

### Example Endpoint  
`GET /api/v1.0/estacionamiento/reportes/balance-diario?fecha=YYYY-MM-DD`

---

# 🏗️ Non-Functional Requirements

## ⚙️ **RNF-01: Clean Architecture (Ports & Adapters)**
No external libraries for business logic.  
Pure domain models and use cases.  
Frameworks only in Infrastructure.

## 🖥️ **RNF-02: External Devices Simulation**
The entry machine and auto-payment terminal are simulated via:
- Postman  
- cURL  
- Any HTTP client

## 🗄️ **RNF-03: Data Persistence**
Supports relational databases MySQL.


