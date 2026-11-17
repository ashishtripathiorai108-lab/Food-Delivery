# Food Delivery Order and Tracking System

A modular, menu-driven order management simulation written in C. It lets you place food delivery orders, move them through a delivery workflow, track them, calculate delivery partner bonuses and penalties, and generate a performance report. Orders can be saved to and loaded from a binary file.

## Features

- **Add orders** with order ID, customer name, restaurant, expected delivery time and bill amount
- **Update order status** following a strict flow with validation
- **Track an order** by its ID
- **View all orders** in a formatted table
- **View delivery history** (delivered and cancelled orders only)
- **Performance report** with averages, on-time/late counts, bonuses, penalties and total revenue
- **Save / load** orders to `orders.dat` (auto-loaded on startup, auto-saved on exit)

## Concepts Used

| Concept | Where it appears |
|---|---|
| Structures and arrays of structures | `Order`, `OrderList` |
| Pointers and dynamic memory | `malloc`, `realloc`, `free` in the list management functions |
| Loops and decision-making | Status transitions, delay/bonus logic, menu loop |
| Functions | Modular design, one function per feature |
| File handling | `fopen`, `fread`, `fwrite` in `save_orders` / `load_orders` |
| Enums | `Status` for the order flow |

## Requirements

- A C compiler such as GCC (C99 or later)

## Compile and Run

```bash
gcc food_delivery.c -o food_delivery
./food_delivery
```

On Windows (MinGW):

```bash
gcc food_delivery.c -o food_delivery.exe
food_delivery.exe
```

## Menu Options

```
===== FOOD DELIVERY TRACKER =====
 1. Add new order
 2. Update order status
 3. Track an order
 4. View all orders
 5. View delivery history
 6. Performance report
 7. Save orders to file
 8. Load orders from file
 0. Exit
```

## Order Status Flow

```
Placed -> Preparing -> Out for Delivery -> Delivered
   \          \
    +----------+--> Cancelled
```

Rules enforced by the program:

- Status must move forward one step at a time.
- An order can be **cancelled** only before it is *Out for Delivery*.
- *Delivered* and *Cancelled* are final states and cannot be changed.
- When marking an order *Delivered*, you enter the actual delivery time in minutes.

## Bonus and Penalty Rules

Calculated by comparing actual and expected delivery time:

| Condition | Result |
|---|---|
| Delivered 10 or more minutes early | Bonus of Rs. 50 |
| Delivered more than 15 minutes late | Penalty of Rs. 30 |
| Delivered 1 to 15 minutes late | Slightly late, no penalty |
| Delivered on time (or less than 10 min early) | On time |

These values can be changed through the constants at the top of the source file:

```c
#define BONUS_THRESHOLD 10
#define BONUS_AMOUNT    50.0f
#define LATE_THRESHOLD  15
#define PENALTY_AMOUNT  30.0f
```

## Performance Report

The report shows:

- Total, active, delivered and cancelled orders
- Average delivery time
- Number of on-time/early and late deliveries
- Total bonuses paid and penalties applied
- Total revenue (from delivered orders only)

## Sample Session

```
Enter choice: 1
Order ID: 101
Customer name: Rahul
Restaurant: Pizza Hub
Expected delivery time (minutes): 30
Order amount (Rs.): 450
Order 101 placed successfully.

Enter choice: 2
Order ID to update: 101
Current status: Placed
Select new status:
  1. Placed
  2. Preparing
  3. Out for Delivery
  4. Delivered
  5. Cancelled
Choice: 2
Order 101 is now 'Preparing'.
```

## Data Storage

Orders are stored in a binary file named `orders.dat` in the working directory:

1. The first bytes hold the order count (`int`).
2. The remaining bytes hold the `Order` structures.

Note: the file is not portable across different compilers or architectures, because it stores raw structs.

## Code Structure

| Section | Purpose |
|---|---|
| Input helpers | `read_line`, `read_int`, `read_float` for safe input with validation |
| List management | `init_list`, `grow_list` (doubles capacity), `free_list`, `find_order` |
| Core features | `add_order`, `update_status`, `track_order`, view and report functions |
| File handling | `save_orders`, `load_orders` |
| Menu | `show_menu` and `main` |

## Possible Improvements

- Store orders in a text or CSV format for portability
- Add order items and itemized billing
- Add delivery partner assignment
- Support searching by customer or restaurant
- Add sorting of orders by time or amount

## License

Free to use for learning and academic purposes.
