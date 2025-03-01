# 3NF Proof for Each Table
We assume each table’s primary key (PK) is as shown in the schema.

## 1. Brand Market Data
**Schema**: `Brand_Market_Data(transaction_id [PK], brand_id [FK], transaction_date, price, ...)`

- **FDs**: \( \text{transaction_id} \to \{\text{brand_id, transaction_date, price, ...}\} \)
- **Justification**: `transaction_id` is the PK and determines all other attributes in this table. Thus, condition (1) holds for all non-key attributes.

## 2. Watch Brands
**Schema**: `Watch_Brands(brand_id [PK], brand_name, country, ...)`

- **FDs**: \( \text{brand_id} \to \{\text{brand_name, country, ...}\} \)
- **Justification**: `brand_id` is the PK and determines all other attributes. Hence, condition (1) is satisfied.
  
## 3. Watches
**Schema**: `Watches(reference_no [PK], brand_id [FK], model_name, movement_type, ...)`

- **FDs**: \( \text{reference_no} \to \{\text{brand_id, model_name, movement_type, ...}\} \)
- **Justification**: `reference_no` is the PK and determines all other attributes. Condition (1) is met.

## 4. Customers
**Schema**: `Customers(customer_id [PK], first_name, last_name, email, ...)`

- **FDs**: \( \text{customer_id} \to \{\text{first_name, last_name, email, ...}\} \)
- **Justification**: `customer_id` is the PK and determines all other attributes. Condition (1) holds.

## 5. Watch Ownership (Collections)
**Schema**: `Watch_Ownership(ownership_id [PK], customer_id [FK], reference_no [FK], purchase_date, ...)`

- **FDs**: \( \text{ownership_id} \to \{\text{customer_id, reference_no, purchase_date, ...}\} \)
- **Justification**: `ownership_id` is the PK and determines all other attributes in this table. Condition (1) is satisfied.

## 6. Reviews
**Schema**: `Reviews(review_id [PK], customer_id [FK], review_text, rating, ...)`

- **FDs**: \( \text{review_id} \to \{\text{customer_id, review_text, rating, ...}\} \)
- **Justification**: `review_id` is the PK and determines all other attributes. Condition (1) is met.

All tables have their non-key attributes fully functionally dependent on their primary keys, with no partial or transitive dependencies beyond the PK. Therefore, each table satisfies 3NF.
