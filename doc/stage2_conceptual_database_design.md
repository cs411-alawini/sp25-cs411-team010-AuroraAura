# Assumptions for Database Design

Below are the key assumptions and reasoning behind each entity and relationship in our schema.

---

## 1. Brand Market Data
- **Purpose:** This table stores historical transactions or pricing data related to each watch brand.
- **Why an Entity (vs. Attribute?):**  
  - We need potentially thousands of records (one for each transaction or price record), so storing this in a separate table keeps it scalable.  
  - Data points such as transaction date, price, region, or volume would be too large and repetitive if folded into the `Watch_Brands` table.  
- **Cardinality & Relationships:**  
  - Each **brand** can have **many** market data entries (1–M).  
  - `Brand Market Data` has a foreign key referencing `Watch_Brands(brand_id)`.

**Assumption:**  
- Each row in `Brand_Market_Data` corresponds to a single recorded transaction or summarized price record for a specific brand at a point in time.

---

## 2. Watch Brands
- **Purpose:** Holds core information about each watch brand (e.g., brand name, country, founding year).
- **Why an Entity?:**  
  - We need a master list of all brands for the entire watch dataset.  
  - This entity can be referenced by multiple tables (e.g., `Brand_Market_Data`, `Watches`).
- **Cardinality & Relationships:**  
  - One **brand** can be associated with **many** watches (1–M).  
  - One **brand** can be associated with **many** brand market data entries (1–M).

**Assumption:**  
- A brand is a standalone entity because it can exist before any watch is created or any transaction is recorded.

---

## 3. Watches
- **Purpose:** Stores detailed information about individual watch models (e.g., reference number, model name, movement type).
- **Why an Entity?:**  
  - Each watch model (e.g., Rolex Submariner reference #, Omega Speedmaster reference #) may have unique attributes.  
  - Potentially thousands of watch entries from different brands.
- **Cardinality & Relationships:**  
  - One **brand** can have **many** watch models (1–M), so `watches.brand_id → watch_brands.brand_id`.  
  - One **watch** can be part of **many** user collections, and a user can own **many** watches, thus a M–M relationship resolved by `Watch_Ownership`.

**Assumption:**  
- Each row in `Watches` represents a distinct model or reference number, not necessarily an individual physical watch.

---

## 4. Customers
- **Purpose:** Represents users in our platform (the watch owners and/or reviewers).
- **Why an Entity?:**  
  - We need to store standard user attributes (e.g., username, email, registration date).  
  - The user entity can be referenced by other tables (e.g., `Watch_Ownership`, `Reviews`).
- **Cardinality & Relationships:**  
  - One **customer** can have **many** watch ownership records (1–M via the bridging table).  
  - One **customer** can write **many** reviews (1–M).

**Assumption:**  
- Each user is uniquely identified (e.g., `customer_id`) and can exist even if they currently own no watches or have not yet posted a review.
- **Deletion Constraint:** On deletion of a user (i.e., `Customers` record), all associated reviews and watch ownership instances are also deleted (cascading delete).

---

## 5. Watch Ownership (Collections)
- **Purpose:** Captures which user owns which watch(es).
- **Why an Entity?:**  
  - Resolves the many-to-many relationship between `Customers` and `Watches`.  
  - We can track additional details (e.g., purchase date, condition, or personal notes).
- **Cardinality & Relationships:**  
  - A single record links **one** user to **one** watch, but each user can own many watches, and each watch can be owned by many users (over time or concurrently if we allow multiple owners).  
  - `watch_ownership.customer_id → customers.customer_id`  
  - `watch_ownership.reference_no → watches.reference_no`

**Assumption:**  
- A single watch entry in `Watches` can appear multiple times in `Watch_Ownership` for different users if the business rules allow.

---

## 6. Reviews
- **Purpose:** Users can post reviews reflecting their overall ownership experience for all watches they own (a “collection” review).
- **Why an Entity?:**  
  - We need to store textual content, rating, date, and which user authored it.
- **Cardinality & Relationships:**  
  - One **customer** can have **many** reviews. Each review belongs to exactly one user (1–M).  
  - These reviews are conceptualized as “overall collection reviews,” so there is **no** direct link to individual watch ownership records.

**Assumption:**  
- A single review is a user’s reflection on their entire watch collection at that moment, rather than on a specific watch.
- If we ever decide to have watch-specific reviews, we could add a foreign key to `Watch_Ownership` or `Watches`.

---

## Additional Assumptions & Constraints

1. **Uniqueness & Keys:**  
   - `Watch_Brands(brand_id)` is the PK.  
   - `Watches(reference_no)` is the PK (assuming each watch model has a unique reference number).  
   - `Customers(customer_id)` is the PK.  
   - `Watch_Ownership(ownership_id)` is the PK, with FKs to `customers` and `watches`.  
   - `Reviews(review_id)` is the PK, with an FK to `customers`.

2. **Data Integrity:**  
   - Deletions: If a brand is deleted, its watch entries and brand market data should also be removed (or restricted, depending on business logic).  
   - Updates: A brand name can be updated, and references in `Watches` and `Brand_Market_Data` remain consistent.

3. **Size & Growth:**  
   - Each table can scale independently (especially `Brand_Market_Data`, which may accumulate thousands of rows over time).  
   - `Watch_Ownership` can handle repeated references to the same watch from different users if the business rules allow.

4. **Review Scope:**  
   - We store one review per user for their entire collection. (This is an application design choice that could change if we want watch-specific reviews in the future.)
