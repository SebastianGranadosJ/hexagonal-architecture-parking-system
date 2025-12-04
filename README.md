# 🎬 Cinema Management API – Hexagonal Architecture (Ports & Adapters)

This project is a modular **Cinema Management API** developed in **TypeScript**, using **Express** as the web framework.  
The system integrates both **SQL database storage** and **local JSON persistence**, and additionally consumes external data from **SWAPI (Star Wars API)** for movie-related information.

The application is structured using a strict **Hexagonal Architecture** (Ports & Adapters), ensuring high separation of concerns, independence from frameworks, strong modularity, and maintainability.  
The system is divided into **four functional hexagons**, plus a dedicated **API hexagon** responsible for all external communication, and a **Shared Module** that provides utilities and components reused across all hexagons.

---

## 🧱 Hexagons Overview

### **1. Movies Hexagon**
Manages movie information. It handles local movie storage and also integrates external data sources such as **SWAPI** to fetch film details, characters, and related metadata.

### **2. Parking Hexagon**
Handles everything related to the cinema's parking system, including vehicle entry, exit, billing calculation, client benefits, and daily revenue reporting.

### **3. Inventory Hexagon**
Responsible for managing inventory items such as food, merchandise, or products sold at the cinema.  
Supports stock checking, item creation, and reservation handling.

### **4. Customer Hexagon**
Handles customer registration, lookup, and identification for benefits such as free parking or personalized services.  
Uses JSON-based local persistence.

### **5. API Hexagon**
Serves as the communication layer between the outside world and the system.  
Contains all Express routing, controllers, and transport-level logic, acting as the **Driver Adapter** for incoming HTTP requests.

### **6. Shared Module**
A cross-cutting module containing reusable utilities, domain abstractions, and infrastructure helpers shared by all hexagons.  
Includes database connectors, JSON managers, error handling, and common value objects.

---

## 🧩 Hexagonal Architecture Diagram


![Hexagonal Architecture](./hexa.png)


> **Note:** The diagram shows the structure of Domain, Application, and Infrastructure, along with Driver/Driven ports and adapters.

The architecture ensures **high modularity**, **easy testability**, and **clear separation of concerns** across the three main layers:

- **Domain**: Core business logic and entities  
- **Application**: Services and Use Cases (Driver Ports)  
- **Infrastructure**: Controllers, repositories, database, and adapters (Driven Ports)


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


