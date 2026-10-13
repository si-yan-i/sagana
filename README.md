# SAGANA — Prelim Source Code

An Intelligent Delivery Route Optimization and Spoilage Monitoring System for
Perishable Farm Produce.

This is the **Prelim Term** deliverable: the Login/Logout, Dashboard, Farm &
Harvest (Batches), Batch Details, and a queue-based Near-Spoilage Alerts
screen, built on three custom linear data structures.

## Group Members
- Francine Danielle A. Biñas
- Jhazel J. De Guzman
- Cianne Greece S. Ilon

## Java Version and Requirements
- **JDK 21** (or any JDK 17+ should work; developed and tested on OpenJDK 21.0.12)
- No external libraries — uses only the Java Standard Library (`java.swing`, `java.awt`, `java.time`)
- Any IDE (IntelliJ IDEA, Eclipse, VS Code with Java extensions) or the command line

## Project Structure

```
sagana-prelim/
└── src/com/sagana/
    ├── Main.java              // Entry point
    ├── ui/                    // Swing GUI screens
    │   ├── LoginFrame.java
    │   ├── DashboardFrame.java
    │   ├── FarmHarvestFrame.java
    │   ├── BatchDetailsFrame.java
    │   └── AlertQueueFrame.java
    ├── model/                 // Data model (classes + a record)
    │   ├── Batch.java
    │   ├── BatchEvent.java    // record
    │   ├── EventType.java     // enum
    │   └── FreshnessStatus.java // enum
    ├── datastructures/        // Custom linear data structures
    │   ├── ResizableArray.java
    │   ├── SinglyLinkedList.java
    │   └── CustomQueue.java
    ├── service/                // Application/service logic
    │   ├── HarvestService.java
    │   ├── AlertQueueService.java
    │   └── AppContext.java
    ├── util/                   // Supporting utilities
    │   ├── Constants.java
    │   ├── IdGenerator.java
    │   └── ValidationUtil.java
    └── test/                   // Manual data structure tests
        └── DataStructureTest.java
```

## Build and Run Instructions

From the `sagana-prelim/` folder (the one containing `src/`):

```bash
# Compile
javac -d bin $(find src -name "*.java")

# Run the application (opens the Login screen)
java -cp bin com.sagana.Main

# Run the data structure tests (optional, no GUI)
java -cp bin com.sagana.test.DataStructureTest
```

On Windows (PowerShell), replace the `find` command with:
```powershell
javac -d bin (Get-ChildItem -Recurse -Filter *.java src | ForEach-Object { $_.FullName })
java -cp bin com.sagana.Main
```

## Sample Login

| Field | Value |
|---|---|
| Username | `admin` |
| Password | `sagana2026` |

(Defined in `com.sagana.util.Constants` — change there if needed.)

## Sample Usage Walkthrough

1. Log in with the credentials above.
2. From the Dashboard, click **Farm & Harvest**.
3. Click **Register Harvest** and fill in, e.g.:
   - Farm name: `Dela Cruz Farm`
   - Crop type: `Tomato`
   - Category: `Vegetable`
   - Quantity: `500`
   - Shelf life (hours): `168`
4. Select the new batch and click **View Batch Details** to see its live
   freshness status and event timeline. Use **Log Dispatched / Delayed /
   Arrived** to add events and watch the timeline update.
5. Click **Recheck Freshness / Flag Alert** — if the batch's freshness status
   is Near Spoilage, it is enqueued into the alert queue.
6. Back on the Dashboard, click **Near-Spoilage Alerts (Queue)** to see
   pending alerts and click **Process Next Alert** to dequeue them in the
   exact order they were flagged (FIFO).

## Data Structures Implemented (Chapter 1, Section 5)

| Structure | File | Used For |
|---|---|---|
| Custom Resizable Array | `datastructures/ResizableArray.java` | Storing all registered batches (`HarvestService`) |
| Singly Linked List | `datastructures/SinglyLinkedList.java` | Each batch's chronological event history (`Batch.eventHistory`) |
| Custom FIFO Queue | `datastructures/CustomQueue.java` | Near-spoilage alert processing order (`AlertQueueService`) |

No stack is included — see Section 5.3 of the proposal for the rationale.

## Current Limitations (Prelim Scope)

- Freshness status is computed with a simple time-elapsed-vs-shelf-life rule; the full multi-variable weighted spoilage model (travel time, storage temperature, handling duration) is a Final Term feature.
- The Key Metrics panel (kilograms rescued, meals recovered, etc.) is a placeholder — real values require the Food Rescue Matching feature planned for the Final Term.
- Data is stored in memory only and is not persisted between runs (no database yet).
- Route Planning/Delivery Monitoring and full Near-Spoilage Recovery (organization matching) screens are Midterm/Final Term features and are not yet implemented.
- No stack is included, as no Prelim feature requires last-in-first-out behavior.

## Testing

`com.sagana.test.DataStructureTest` contains manual assertion-based tests
(29 checks) verifying the correctness of `ResizableArray` (including the
resize/doubling trigger), `SinglyLinkedList` (append order, traversal), and
`CustomQueue` (FIFO enqueue/dequeue order). Run it directly — no JUnit
dependency required.
