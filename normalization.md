# Normalization Explained in Details

## **🔵 Section 3: Organizing Your Data (Like Tidying Your Home)**

**Goal:** Learn how to arrange information efficiently so you don't store the same thing multiple times.

> **Interactive Tutorial:** Before reading further, try the [Normalization Interactive Visualization](./visualisation/normalization-interactive.html). Open this file in your web browser and follow the 8-step guided tour to see these ideas come to life.

### **3.1 Why This Matters: The Coffee Shop Story**

Picture this: You're a regular at a coffee shop. Every time you buy coffee, the cashier writes down your order along with your complete home address on a paper slip.

Now imagine you move to a new apartment. The coffee shop would need to dig through hundreds of old receipts and update your address on every single one. That's exhausting and error-prone!

This problem is called an "Update Anomaly" - when changing one piece of information forces you to update it in many places.

**Normalization** is like organizing your home: instead of keeping your keys in random places throughout the house, you put them in one designated spot. Similarly, we organize data so each piece of information lives in exactly one place. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

***

### **3.2 The Three Rules of Good Organization**

Think of these as three levels of tidiness. Here's a simple way to remember them: [stackoverflow](https://stackoverflow.com/questions/2331838/normalization-in-plain-english)

**"Every fact should be about the key, the whole key, and nothing but the key."**

Let's break that down with real-world examples:

#### **Rule 1 (1NF): One Item Per Space - "No Grocery Bags in Cells"**

**Real-Life Analogy:** Imagine your closet. You wouldn't stuff 3 shirts onto a single hanger and write "blue shirt, red shirt, green shirt" on one label. Each shirt gets its own space. [red-gate](https://www.red-gate.com/blog/normalization-1nf-2nf-3nf)

**The Problem - Before 1NF:**

Your shopping list looks like this:

| Person | Favorite Fruits |
|--------|----------------|
| Sarah  | Apple, Banana, Orange |
| Mike   | Grape, Mango |

**What's wrong?** The "Favorite Fruits" cell contains multiple values crammed together. If you want to count how many people like apples, you'd have to split apart that cell manually.

**The Solution - After 1NF:**

| Person | Favorite Fruit |
|--------|---------------|
| Sarah  | Apple |
| Sarah  | Banana |
| Sarah  | Orange |
| Mike   | Grape |
| Mike   | Mango |

**Why it's better:** Now each cell contains exactly one piece of information. You can easily search, count, and organize. [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

***

#### **Rule 2 (2NF): Everything Relates to the Complete ID - "The Address Book Rule"**

**Real-Life Analogy:** Think of an address book. You wouldn't write someone's home address next to each phone call you made to them. The address belongs with the person's name, not with individual calls. [datacamp](https://www.datacamp.com/tutorial/normalization-in-sql)

**The Problem - Before 2NF:**

Imagine a library tracking who borrowed which books:

| Student ID | Student Name | Book ID | Book Title | Borrow Date |
|-----------|-------------|---------|-----------|-------------|
| 001 | Alice | B10 | Harry Potter | 2026-01-10 |
| 001 | Alice | B20 | Hobbit | 2026-01-12 |
| 002 | Bob | B10 | Harry Potter | 2026-01-11 |

**What's wrong?** Alice's name appears twice. If Alice changes her name (marriage, legal change), you'd have to update multiple rows. Also, "Book Title" really depends on "Book ID," not on the specific borrowing transaction.

**The Solution - After 2NF:**

Split into three tables:

**Students Table:**
| Student ID | Student Name |
|-----------|-------------|
| 001 | Alice |
| 002 | Bob |

**Books Table:**
| Book ID | Book Title |
|---------|-----------|
| B10 | Harry Potter |
| B20 | Hobbit |

**Borrowing Records:**
| Student ID | Book ID | Borrow Date |
|-----------|---------|-------------|
| 001 | B10 | 2026-01-10 |
| 001 | B20 | 2026-01-12 |
| 002 | B10 | 2026-01-11 |

**Why it's better:** Student information lives in one place, book information lives in one place. Each piece of information connects to its complete identifier. [guides.visual-paradigm](https://guides.visual-paradigm.com/a-comprehensive-guide-to-database-normalization-with-examples/)

***

#### **Rule 3 (3NF): No Chain Dependencies - "The Phone Directory Principle"**

**Real-Life Analogy:** In a phone directory, you look up a person's name to get their phone number. You don't look up their phone number to find their city, then use the city to find their zip code. That's a "chain" - one thing leading to another. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

**The Problem - Before 3NF:**

Your employee database looks like this:

| Employee ID | Employee Name | Department Code | Department Name | Department Manager |
|------------|--------------|-----------------|-----------------|-------------------|
| E001 | Alice | D10 | Sales | John Smith |
| E002 | Bob | D10 | Sales | John Smith |
| E003 | Carol | D20 | Marketing | Jane Doe |

**What's wrong?** "Department Name" and "Department Manager" don't really depend on the employee. They depend on the "Department Code." If the Sales manager changes from John Smith to Mary Johnson, you'd have to update every sales employee's row. [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

**Visual representation of the dependency chain:**
```
Employee ID → Department Code → Department Name
Employee ID → Department Code → Department Manager
```

This is called a "transitive dependency" - information depending on something that depends on something else. [datacamp](https://www.datacamp.com/tutorial/normalization-in-sql)

**The Solution - After 3NF:**

Split into two tables:

**Employees Table:**
| Employee ID | Employee Name | Department Code |
|------------|--------------|-----------------|
| E001 | Alice | D10 |
| E002 | Bob | D10 |
| E003 | Carol | D20 |

**Departments Table:**
| Department Code | Department Name | Department Manager |
|-----------------|-----------------|-------------------|
| D10 | Sales | John Smith |
| D20 | Marketing | Jane Doe |

**Why it's better:** When the Sales manager changes, you update ONE cell in the Departments table. All employees automatically reflect this change when you look up their department. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

***

### **🟢 Activity 3: The E-Commerce Deconstruction (Group Breakout - 20 Mins)**

Let's practice with a real online store example, going through all three rules step by step.

#### **Starting Point: The Messy Spreadsheet**

Imagine you're running an online store and currently track everything in one big table:

| OrderID | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate |
|---------|--------|----------|-----------|-----------|-------------|-----------|
| 100 | 10 | iPhone | 1000 | 1 | John | 2021-01-01 |
| 100 | 20 | iPad | 500 | 1 | John | 2021-01-01 |
| 200 | 30 | Macbook | 2000 | 1 | John | 2021-01-02 |
| 300 | 10 | iPhone | 1000 | 2 | Mary | 2021-01-03 |
| 300 | 30 | Macbook | 2000 | 2 | Mary | 2021-01-03 |

***

#### **Step 1: Applying Rule 1 (1NF) - Making Each Row Unique**

**The Problem:** Look at OrderID 100 - it appears in two rows (iPhone and iPad). Which row is "the order"? Both? This is confusing!

**Think of it like a receipt:** When you get a store receipt, it has ONE receipt number at the top, but multiple line items below. Each line item needs its own identifier.

**The Solution:** Add a line number to create a unique identifier for each row:

| OrderID | LineNumber | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate |
|---------|-----------|--------|----------|-----------|-----------|-------------|-----------|
| 100 | 1 | 10 | iPhone | 1000 | 1 | John | 2021-01-01 |
| 100 | 2 | 20 | iPad | 500 | 1 | John | 2021-01-01 |
| 200 | 1 | 30 | Macbook | 2000 | 1 | John | 2021-01-02 |
| 300 | 1 | 10 | iPhone | 1000 | 2 | Mary | 2021-01-03 |
| 300 | 2 | 30 | Macbook | 2000 | 2 | Mary | 2021-01-03 |

**Key Insight:** Now each row has a unique two-part ID: (OrderID + LineNumber). Just like "Building A, Apartment 301" uniquely identifies a location.

✅ **Rule 1 Complete!** Each row is now uniquely identifiable.

***

#### **Step 2: Applying Rule 2 (2NF) - Separating Order Info from Item Info**

**The Problem:** Look at John's information (CustomerID, CustomerName, OrderDate). Does this change between line items? No! Whether John bought an iPhone (line 1) or an iPad (line 2), he's still John, and the order date is still 2021-01-01.

**Visual Diagram - What depends on what:**

```
(OrderID + LineNumber) → ItemID, ItemName, ItemPrice ✓ (Needs both parts)
OrderID only → CustomerID, CustomerName, OrderDate ✓ (Doesn't need LineNumber)
```

The customer information only cares about OrderID, not LineNumber. This violates Rule 2. [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

**The Solution:** Split into two tables based on what the information really describes:

**Orders Table** (one row per order):

| OrderID | CustomerID | CustomerName | OrderDate |
|---------|-----------|-------------|-----------|
| 100 | 1 | John | 2021-01-01 |
| 200 | 1 | John | 2021-01-02 |
| 300 | 2 | Mary | 2021-01-03 |

**Order Line Items Table** (one row per item in an order):

| OrderID | LineNumber | ItemID | ItemName | ItemPrice |
|---------|-----------|--------|----------|-----------|
| 100 | 1 | 10 | iPhone | 1000 |
| 100 | 2 | 20 | iPad | 500 |
| 200 | 1 | 30 | Macbook | 2000 |
| 300 | 1 | 10 | iPhone | 1000 |
| 300 | 2 | 30 | Macbook | 2000 |

**Real-World Comparison:**
- **Orders Table** = The envelope with order details
- **Order Line Items Table** = The itemized list inside the envelope

✅ **Rule 2 Complete!** John's name now appears only once per order, not once per item.

***

#### **Step 3: Applying Rule 3 (3NF) - Creating a Product Catalog**

**The Problem:** Look at the iPhone - it appears in Order 100 and Order 300, both times with ItemName "iPhone" and ItemPrice 1000. That's duplicate information!

**Ask yourself:** Does the iPhone's name and price depend on which specific order purchased it? No! An iPhone is an iPhone regardless of who buys it. [freecodecamp](https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/)

**Visual Diagram - The Chain Dependency:**

```
(OrderID + LineNumber) → ItemID → ItemName, ItemPrice
                         ↑         ↑
                         |         |
                    This is the chain we need to break!
```

ItemName and ItemPrice depend on ItemID, not on the specific order. This is a "transitive dependency". [datacamp](https://www.datacamp.com/tutorial/normalization-in-sql)

**The Solution:** Create a separate Products catalog:

**Products Table** (one row per product):

| ItemID | ItemName | ItemPrice |
|--------|----------|-----------|
| 10 | iPhone | 1000 |
| 20 | iPad | 500 |
| 30 | Macbook | 2000 |

**Simplified Order Line Items Table:**

| OrderID | LineNumber | ItemID |
|---------|-----------|--------|
| 100 | 1 | 10 |
| 100 | 2 | 20 |
| 200 | 1 | 30 |
| 300 | 1 | 10 |
| 300 | 2 | 30 |

**Real-World Comparison:**
- **Products Table** = Your store's product catalog
- **Order Line Items** = References pointing to items in the catalog

**The Big Win:** If the iPhone price changes to $1,200, you update ONE cell in the Products table. Every query that asks "What's the current price of an iPhone?" automatically gets the new price.

✅ **Rule 3 Complete!** No more chain dependencies!

***

### **📊 Visual Summary: Before and After**

**BEFORE NORMALIZATION:**
```
One Big Messy Table (25 rows for 5 orders × 5 details)
❌ John's name repeated 3 times
❌ iPhone details repeated 2 times
❌ Must update multiple places when anything changes
```

**AFTER NORMALIZATION (1NF → 2NF → 3NF):**
```
Orders Table: 3 rows
Order Line Items: 5 rows
Products Table: 3 rows
Total: 11 rows (less than half!)

✅ Each fact stored exactly once
✅ Easy to update anything anywhere
✅ No contradictions possible
```

***

### **🟢 Workshop 3.2.3: Your Turn to Practice**

**Your Assignment:**

Visit [dbdiagram.io](https://dbdiagram.io/d) and create a visual diagram showing how these three tables connect to each other. Here's what to show:

1. Draw three boxes (one for Orders, one for Order Line Items, one for Products)
2. Show lines connecting them (Orders → Order Line Items → Products)
3. Mark which columns are IDs that link the tables together

**Hint:** The connecting lines represent "relationships" - like "Each Order has many Line Items" and "Each Line Item refers to one Product."

Share your diagram in Discord: [https://discord.com/channels/1165846570177150996/1457586759667028094](https://discord.com/channels/1165846570177150996/1457586759667028094)

***

### **3.3 Group Discussion: When Breaking Rules Makes Sense**

**Scenario:** Your normalized database is working beautifully. Products are stored once, orders reference them cleanly.

**Question:** "In our cleaned-up design, if the iPhone price increases to $1,200 next year, what happens to Mary's order from 2021?"

**Think about it:**
- In our normalized design, there's ONE iPhone price in the Products table
- Mary ordered in 2021 when the price was $1,000
- If we update the price to $1,200, does Mary's historical order suddenly show $1,200?

**The Answer:** Yes, it would! And that's **wrong** for accounting purposes. [stackoverflow](https://stackoverflow.com/questions/2331838/normalization-in-plain-english)

**The Solution - Intentional Denormalization:**

Add a "sold_price" column to Order Line Items:

| OrderID | LineNumber | ItemID | **Sold_Price** |
|---------|-----------|--------|---------------|
| 100 | 1 | 10 | **1000** |
| 300 | 1 | 10 | **1000** |

This way:
- **Products Table** shows the current price ($1,200)
- **Order history** preserves what was actually paid ($1,000)

**Real-Life Analogy:** This is like keeping old receipts. Even if a store raises prices today, your receipt from last year still shows what you paid back then. The store needs both: current prices (for new sales) and historical prices (for accounting records). [digitalocean](https://www.digitalocean.com/community/tutorials/database-normalization)

**Key Lesson:** Normalization is a powerful tool, but sometimes you intentionally keep duplicate data for good business reasons. This is called "denormalization," and it's a conscious design choice, not a mistake.

***

### **🎯 Quick Reference Card**

Print this and keep it handy:

| Rule | Remember It As | Ask Yourself |
|------|---------------|--------------|
| **1NF** | One item per space | Are there any cells with lists or multiple values? |
| **2NF** | The whole key | Does every column need the ENTIRE ID to make sense? |
| **3NF** | Nothing but the key | Does any column depend on another regular column? |

**The Golden Question:** "If I change this piece of information, how many places do I need to update?"
- **Many places?** You probably need more normalization.
- **One place?** You're doing it right! [stackoverflow](https://stackoverflow.com/questions/2331838/normalization-in-plain-english)

***

[Continue learning in Post-Class materials](./post-class.md)
