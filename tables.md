// Creating the tables

CREATE TABLE Watch_Brands (
  brand_name VARCHAR(50) NOT NULL,
  hq_city VARCHAR(50),
  hq_country VARCHAR(50),
  year_founded YEAR,
  founder VARCHAR(200),
  PRIMARY KEY (brand_name)
);

CREATE TABLE Watches (
  reference VARCHAR(30) NOT NULL,
  brand_name VARCHAR(50) NOT NULL,
  model_name VARCHAR(100),
  complication VARCHAR(100),
  case_material VARCHAR(100),
  bracelet_material VARCHAR(100),
  dial_color VARCHAR(100),
  hour_markings BOOL,
  lunette_material VARCHAR(100),
  retail_price FLOAT(30,2),
  PRIMARY KEY (reference),
  FOREIGN KEY (brand_name) REFERENCES Watch_Brands(brand_name) ON DELETE CASCADE
);

CREATE TABLE Customers (
  customer_id INT AUTO_INCREMENT,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  email VARCHAR(60) UNIQUE,
  user_id VARCHAR(50) UNIQUE,
  year_joined YEAR,
  age INT,
  PRIMARY KEY (customer_id)
);

CREATE TABLE Reviews (
  review_id INT AUTO_INCREMENT,
  customer_id INT NOT NULL,
  review_text VARCHAR(500),
  stars FLOAT(2,1),
  author_id INT NOT NULL,
  PRIMARY KEY (review_id),
  FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE,
  FOREIGN KEY (author_id) REFERENCES Customers(customer_id) ON DELETE CASCADE
);

CREATE TABLE Watch_Ownership (
  ownership_id INT AUTO_INCREMENT,
  customer_id INT NOT NULL,
  brand_name VARCHAR(50) NOT NULL,
  model_name VARCHAR(100),
  watch_collection_type VARCHAR(50),
  reference VARCHAR(30) NOT NULL,
  PRIMARY KEY (ownership_id),
  FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE,
  FOREIGN KEY (brand_name) REFERENCES Watch_Brands(brand_name) ON DELETE CASCADE,
  FOREIGN KEY (reference) REFERENCES Watches(reference) ON DELETE CASCADE
);

CREATE TABLE Brand_Market_Data (
  transaction_id INT AUTO_INCREMENT,
  brand_name VARCHAR(50) NOT NULL,
  price FLOAT(30,2),
  production_year YEAR,
  PRIMARY KEY (transaction_id),
  FOREIGN KEY (brand_name) REFERENCES Watch_Brands(brand_name) ON DELETE CASCADE
);
