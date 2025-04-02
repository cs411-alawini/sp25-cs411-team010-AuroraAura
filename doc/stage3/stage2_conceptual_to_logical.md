Here is the conceptual database design to the logical design, following the format from canvas
(Table-Name(Column1:Domain [PK], Column2:Domain [FK to table.column], Column3:Domain,...)
### Watch Brands
- **Brand_Name**: VARCHAR(50) [PK]
- **HQ_City**: VARCHAR(50)
- **HQ_Country**: VARCHAR(50)
- **Year_Founded**: YEAR
- **Founder**: VARCHAR(200)

### Watches
- **Reference**: VARCHAR(30) [PK]
- **Brand_Name**: VARCHAR(50) [FK to Watch_Brands.Brand_Name]
- **Model_Name**: VARCHAR(100)
- **Complication**: VARCHAR(100)
- **Case_Material**: VARCHAR(100)
- **Bracelet_Material**: VARCHAR(100)
- **Dial_Color**: VARCHAR(100)
- **Hour_Markings**: BOOL
- **Lunette_Material**: VARCHAR(100)
- **Retail_Price**: FLOAT(30,2)

### Customers
- **Customer_ID**: INT [PK]
- **First_Name**: VARCHAR(50)
- **Last_Name**: VARCHAR(50)
- **Email**: VARCHAR(60)
- **User_ID**: VARCHAR(50)
- **Year_Joined**: YEAR
- **Age**: INT

### Reviews
- **Review_ID**: INT [PK]
- **Customer_ID**: INT [FK to Customers.Customer_ID]
- **Review_Text**: VARCHAR(500)
- **Stars**: FLOAT(2,1)
- **Author_ID**: INT

### Watch Ownership
- **Ownership_ID**: INT [PK]
- **Customer_ID**: INT [FK to Customers.Customer_ID]
- **Brand_Name**: VARCHAR(50) [FK to Watch_Brands.Brand_Name]
- **Model_Name**: VARCHAR(100)
- **Watch_Collection_Type**: VARCHAR(50)
- **Reference**: VARCHAR(30) [FK to Watches.Reference]

### Brand Market Data
- **Transaction_ID**: INT [PK]
- **Brand_Name**: VARCHAR(50) [FK to Watch_Brands.Brand_Name]
- **Price**: FLOAT(30,2)
- **Production_Year**: YEAR
