## 🏦 Bank Management System (C++ Project)

This project is a simple **Bank Management System** written in **C++**, designed to manage client data such as adding, deleting, updating, searching, and performing transactions (deposit/withdraw).
All client information is stored in a text file (`file.txt`) and managed through **Structs** and **Vectors**.

---

### 📁 Main Components

#### 🔹 1. Struct: `BankInfo`

A structure that defines a single bank client, containing:

* `BankAccount` — account number
* `PinCode` — client PIN code
* `Name` — client full name
* `Phone` — client phone number
* `Balance` — client current balance
* `flage` — helper variable

---

#### 🔹 2. Main Menu

The main interface is displayed through `ShowManue()`:

```
[1] Show Client List.
[2] Add New Client.
[3] Delete Client.
[4] Update Client Info.
[5] Find Client.
[6] Transaction.
[7] Exit.
```

---

### ⚙️ Core Functionalities

#### 🧾 Reading the File

* `readFileAndAppenditInVector()` loads data from the text file and stores it into a `vector<BankInfo>`.
* Helper functions:

  * `ConvertLineOFStringWithSepToVector()` splits each line using `#//#` separator.
  * `ConvertVectorToStruct()` converts the vector into a `BankInfo` struct.

---

#### 👥 Display Clients

* `ShowClients()` prints all clients in a formatted table using `setw()`.
* `Header()` prints the table header.

---

#### ➕ Add New Client

* `AddMoreClients()` allows adding one or more clients.
* `CheckBankAccountToGetNewID()` checks if the account already exists.
* `AddNewClient()` and `WriteFileByVstruct()` save new data to the file.

---

#### ❌ Delete Client

* `FindClientByBankAccount()` locates a client and displays details before deletion.
* `RemoveClientOfVector()` removes the client from the list.
* `DeleteClient()` performs the final deletion and updates the file.

---

#### ✏️ Update Client Info

* `stUpdateClient()` finds and edits a client’s data.
* `UpdateClient()` saves the updated information to the file.

---

#### 🔍 Find Client

* `FindClient()` displays client details based on account number.
* `FindClientFunc()` manages the user interface for searching.

---

### 💵 Transactions Section

#### Menu:

`ShowTransAction()` displays:

```
[1] Deposit.
[2] Withdraw.
[3] Total Balance.
[4] Main Menu.
```

#### Deposit:

* `Deposit()` calls `UpdateDeposit()`.
* The client is found using `IsBankAccountInBank()`, then balance is increased and written back to the file.

#### Withdraw:

* `WithDrow()` calls `UpdateWithDrow()`.
* Checks if the amount is valid (not greater than current balance) before updating.

#### Total Balance:

* `TotalBalance()` calculates and displays the total sum of all client balances.

---

### 🧩 Helper Functions

* `IsCharInSeparator()` and `ConvertLineOFStringWithSepToVector()` — handle string parsing.
* `WriteFileByVstruct()` — writes vector data back to the file.
* `UpdateStructInfo()` — updates a specific client inside the vector after changes.

---

### 🖥️ Main Function (`main()`)

* Loads all client data into memory using `readFileAndAppenditInVector()`.
* Calls `AllFunc()` to run the main program loop and handle user choices.

---

### ✅ Features

* File-based data persistence.
* Organized client management using Struct and Vector.
* Interactive console-based interface.
* Clear separation of logic (Add, Delete, Update, Search, Transactions).

---

### ⚠️ Notes

* This system uses a text file instead of a database.
* Designed for learning and practicing file handling & data management in C++.
* Compatible with **Visual Studio**, **Code::Blocks**, or any modern C++ IDE.

---

🧑‍💻 **Author:** Mustafa
📅 **Language:** C++
📂 **Project Type:** Console Application
📘 **Topic:** File Handling • Structs • Vectors • Banking Logic

