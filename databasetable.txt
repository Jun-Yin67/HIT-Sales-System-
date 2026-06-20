
DROP TABLE PurchaseDetail CASCADE CONSTRAINTS;
DROP TABLE OrderDetail CASCADE CONSTRAINTS;
DROP TABLE Delivery CASCADE CONSTRAINTS;
DROP TABLE Orders CASCADE CONSTRAINTS;
DROP TABLE Purchase CASCADE CONSTRAINTS;


DROP TABLE Product CASCADE CONSTRAINTS;
DROP TABLE Vendor CASCADE CONSTRAINTS;
DROP TABLE Customer CASCADE CONSTRAINTS;
DROP TABLE Employee CASCADE CONSTRAINTS;



-- Customer Table
CREATE TABLE Customer (
    Cust_ID VARCHAR2(8) NOT NULL,
    Cust_Type VARCHAR2(20) CHECK (Cust_Type IN ('customer', 'agent', 'dropshipper')),
    Cust_Name VARCHAR2(25) NOT NULL,
    Cust_Email VARCHAR2(24),
    Cust_Contact VARCHAR2(15),
    Cust_Add VARCHAR2(30),
    CONSTRAINT customer_cust_id_pk PRIMARY KEY (Cust_ID)
);

-- Employee Table
CREATE TABLE Employee (
    Emp_ID VARCHAR2(8) NOT NULL,
    Emp_Name VARCHAR2(30) NOT NULL,
    Emp_Contact VARCHAR2(15),
    CONSTRAINT employee_emp_id_pk PRIMARY KEY (Emp_ID)
);

-- Orders Table
CREATE TABLE Orders (
    Ord_ID VARCHAR2(8) NOT NULL,
    Cust_ID VARCHAR2(8) NOT NULL,
    Ord_Date DATE DEFAULT SYSDATE,
    Ord_CommRate NUMBER(5,2),
    OD_PayMethod VARCHAR2(20) CHECK (OD_PayMethod IN ('CreditCard', 'TnG', 'BankTransfer')),
    CONSTRAINT orders_ord_id_pk PRIMARY KEY (Ord_ID),
    CONSTRAINT orders_cust_id_fk FOREIGN KEY (Cust_ID) REFERENCES Customer(Cust_ID)
);


-- Delivery Table
CREATE TABLE Delivery (
    Del_ID VARCHAR2(10) NOT NULL,
    Ord_ID VARCHAR2(8) NOT NULL,
    Emp_ID VARCHAR2(8) NOT NULL,
    Del_Fee NUMBER(5,2) DEFAULT 0,
    Del_Method VARCHAR2(20),
    CONSTRAINT delivery_del_id_pk PRIMARY KEY (Del_ID),
    CONSTRAINT delivery_ord_id_fk FOREIGN KEY (Ord_ID) REFERENCES Orders(Ord_ID),
    CONSTRAINT delivery_emp_id_fk FOREIGN KEY (Emp_ID) REFERENCES Employee(Emp_ID)
);

-- Product Table
CREATE TABLE Product (
    Prod_ID VARCHAR2(15) NOT NULL,
    Prod_Type VARCHAR2(30) CHECK (Prod_Type IN ('Sharifah Ready to Eat Products', 'Biodegradable Packaging Items')),
    Prod_SubCategory VARCHAR2(16),
    Prod_Info VARCHAR2(40),
    Prod_Qty NUMBER(10) DEFAULT 0,
    Prod_QtyPerUnit NUMBER(10),
    Prod_Price NUMBER(8,2) DEFAULT 0,
    Prod_Weight NUMBER(10,2) DEFAULT 0,
    CONSTRAINT product_prod_id_pk PRIMARY KEY (Prod_ID)
);

-- OrderDetail Table
CREATE TABLE OrderDetail (
    OD_ID VARCHAR2(10) NOT NULL,
    Ord_ID VARCHAR2(8) NOT NULL,
    Prod_ID VARCHAR2(15) NOT NULL,
    OD_Qty NUMBER(8) DEFAULT 1,
    O_Disc NUMBER(5,2) DEFAULT 0,
    Ord_Price NUMBER(7,2) DEFAULT 0,
    CONSTRAINT orderdetail_od_id_pk PRIMARY KEY (OD_ID),
    CONSTRAINT orderdetail_ord_id_fk FOREIGN KEY (Ord_ID) REFERENCES Orders(Ord_ID),
    CONSTRAINT orderdetail_prod_id_fk FOREIGN KEY (Prod_ID) REFERENCES Product(Prod_ID)
);


-- Vendor Table
CREATE TABLE Vendor (
    Vend_ID VARCHAR2(8) NOT NULL,
    Vend_Category VARCHAR2(21) CHECK (Vend_Category IN ('Sharifah Food Sdn Bhd', 'Other vendors')),
    Vend_Name VARCHAR2(30) NOT NULL,
    Vend_Contact VARCHAR2(15),
    Vend_BD NUMBER(10),
    CONSTRAINT vendor_vend_id_pk PRIMARY KEY (Vend_ID)
);


-- Purchase Table
CREATE TABLE Purchase (
    P_ID VARCHAR2(8) NOT NULL,
    Vend_ID VARCHAR2(5) NOT NULL,
    P_Date DATE DEFAULT SYSDATE,
    CONSTRAINT purchase_p_id_pk PRIMARY KEY (P_ID),
    CONSTRAINT purchase_vend_id_fk FOREIGN KEY (Vend_ID) REFERENCES Vendor(Vend_ID)
);

-- PurchaseDetail Table
CREATE TABLE PurchaseDetail (
    P_ID VARCHAR2(8) NOT NULL,
    Prod_ID VARCHAR2(15) NOT NULL,
    P_Qty NUMBER(8) DEFAULT 0,
    P_Rec NUMBER(7,2) DEFAULT 0,
    CONSTRAINT purchasedetail_pk PRIMARY KEY (P_ID, Prod_ID),
    CONSTRAINT purchasedetail_p_id_fk FOREIGN KEY (P_ID) REFERENCES Purchase(P_ID),
    CONSTRAINT purchasedetail_prod_id_fk FOREIGN KEY (Prod_ID) REFERENCES Product(Prod_ID)
);



-- Product Table --

-- Sharifah Ready to Eat Products --

-- Ready to Eat Products
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('PR001', 'Sharifah Ready to Eat Products', 'NASI BRIYANI GAM AYAM', 'Ready to Eat', 500, 1, 13.90, 250),
('PR002', 'Sharifah Ready to Eat Products', 'SAMBAL SOTONG KERING', 'Ready to Eat', 500, 1, 12.90, 180),
('PR003', 'Sharifah Ready to Eat Products', 'RENDANG DENDENG (200G)', 'Ready to Eat', 500, 1, 23.90, 240),
('PR004', 'Sharifah Ready to Eat Products', 'SAMBAL TUMIS IKAN BILIS', 'Ready to Eat', 500, 1, 6.90, 140),
('PR005', 'Sharifah Ready to Eat Products', 'NASI GORENG KAMPUNG', 'Ready to Eat', 500, 1, 11.90, 250),
('PR006', 'Sharifah Ready to Eat Products', 'NASI GORENG UDANG', 'Ready to Eat', 500, 1, 11.90, 250),
('PR007', 'Sharifah Ready to Eat Products', 'NASI GORENG IKAN MASIN', 'Ready to Eat', 500, 1, 11.90, 250),
('PR008', 'Sharifah Ready to Eat Products', 'NASI GORENG AYAM', 'Ready to Eat', 500, 1, 11.90, 250),
('PR009', 'Sharifah Ready to Eat Products', 'NASI GORENG DAGING', 'Ready to Eat', 500, 1, 11.90, 250),
('PR010', 'Sharifah Ready to Eat Products', 'KARI AYAM', 'Ready to Eat', 500, 1, 8.90, 140),
('PR011', 'Sharifah Ready to Eat Products', 'NASI PUTIH', 'Ready to Eat', 500, 1, 4.50, 250),
('PR012', 'Sharifah Ready to Eat Products', 'RENDANG DENDENG MINI (120G)', 'Ready to Eat', 500, 1, 14.90, 120);

-- Ready to Cook Products
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('PR013', 'Sharifah Ready to Eat Products', 'NASI BRIYANI GAM 535G', 'Ready to Cook', 500, 1, 21.90, 535),
('PR014', 'Sharifah Ready to Eat Products', 'NASI BRIYANI HERBA 395G', 'Ready to Cook', 500, 1, 21.90, 395),
('PR015', 'Sharifah Ready to Eat Products', 'NASI BRIYANI BUKHARI 500G', 'Ready to Cook', 500, 1, 21.90, 500);

-- Paste Products
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('PR016', 'Sharifah Ready to Eat Products', 'PES KARI KEPALA IKAN 120G', 'Paste', 500, 1, 5.90, 120),
('PR017', 'Sharifah Ready to Eat Products', 'PES NASI GORENG KAMPUNG 120G', 'Paste', 500, 1, 5.90, 120),
('PR018', 'Sharifah Ready to Eat Products', 'PES NASI BRIYANI GAM 120G', 'Paste', 500, 1, 5.90, 120),
('PR019', 'Sharifah Ready to Eat Products', 'PES MASAK KERUTUK 120G', 'Paste', 500, 1, 5.90, 120),
('PR020', 'Sharifah Ready to Eat Products', 'PES ASAM PEDAS 120G', 'Paste', 500, 1, 5.90, 120),
('PR021', 'Sharifah Ready to Eat Products', 'PES SAMBAL TUMIS 120G', 'Paste', 500, 1, 5.90, 120),
('PR022', 'Sharifah Ready to Eat Products', 'PES MASAK TIGA RASA 120G', 'Paste', 500, 1, 5.90, 120),
('PR023', 'Sharifah Ready to Eat Products', 'PES MIE GORENG 120G', 'Paste', 500, 1, 5.90, 120);

-- Biodegradable Packaging Items --

-- Carry Bag
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('HIT-CB-001', 'Biodegradable Packaging Items', 'E20 BIO SINGLET 12x12', 'Carry Bag', 500, 50, 7.50, NULL),
('HIT-CB-002', 'Biodegradable Packaging Items', 'E30 BIO SINGLET 15x15', 'Carry Bag', 500, 50, 11.50, NULL),
('HIT-CB-003', 'Biodegradable Packaging Items', 'E40 BIO SINGLET 17x19', 'Carry Bag', 500, 40, 13.00, NULL),
('HIT-CB-004', 'Biodegradable Packaging Items', 'E48 BIO SINGLET 18x22', 'Carry Bag', 500, 40, 15.50, NULL),
('HIT-CB-005', 'Biodegradable Packaging Items', 'E55 BIO SINGLET 22x24', 'Carry Bag', 500, 40, 23.50, NULL),
('HIT-CB-006', 'Biodegradable Packaging Items', 'E60 BIO SINGLET 24x30', 'Carry Bag', 500, 40, 35.00, NULL),
('HIT-CB-007', 'Biodegradable Packaging Items', 'PAPER BAG WITH BASE SOS NO.4', 'Carry Bag', 500, 1000, 80.00, NULL),
('HIT-CB-008', 'Biodegradable Packaging Items', 'PAPER BAG WITH BASE SOS NO.6', 'Carry Bag', 500, 1000, 90.00, NULL),
('HIT-CB-009', 'Biodegradable Packaging Items', 'PAPER BAG WITH BASE SOS NO.8', 'Carry Bag', 500, 1000, 100.00, NULL);

-- Containers
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('HIT-CTR-001', 'Biodegradable Packaging Items', '2 Compartment Lunch Box (600ml)', 'Container', 500, 400, 285.00, NULL),
('HIT-CTR-002', 'Biodegradable Packaging Items', '4oz corn starch sauce cup', 'Container', 500, 1000, 225.00, NULL),
('HIT-CTR-003', 'Biodegradable Packaging Items', '2oz corn starch sauce cup', 'Container', 500, 1000, 205.00, NULL),
('HIT-CTR-004', 'Biodegradable Packaging Items', 'Corn Starch 800ml bowl', 'Container', 500, 300, 265.00, NULL),
('HIT-CTR-005', 'Biodegradable Packaging Items', 'Corn Starch 400ml bowl', 'Container', 500, 300, 195.00, NULL),
('HIT-CTR-006', 'Biodegradable Packaging Items', 'Corn Starch 450ml bowl', 'Container', 500, 300, 205.00, NULL),
('HIT-CTR-007', 'Biodegradable Packaging Items', 'Corn Starch 280ml bowl', 'Container', 500, 300, 170.00, NULL),
('HIT-CTR-008', 'Biodegradable Packaging Items', '4 compartment lunch box', 'Container', 500, 200, 257.00, NULL),
('HIT-CTR-009', 'Biodegradable Packaging Items', '3 compartment lunch box', 'Container', 500, 200, 250.00, NULL);

-- Cups
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('HIT-CUP-001', 'Biodegradable Packaging Items', '16oz Corn starch Cup', 'Cup', 500, 1000, 320.00, NULL),
('HIT-CUP-002', 'Biodegradable Packaging Items', '16oz Cornstarch Cold Cup Lid', 'Cup', 500, 1000, 150.00, NULL),
('HIT-CUP-003', 'Biodegradable Packaging Items', '90mm paper lid - white', 'Cup', 500, 2000, 235.00, NULL),
('HIT-CUP-004', 'Biodegradable Packaging Items', '90mm PLA lid', 'Cup', 500, 1000, 270.00, NULL),
('HIT-CUP-005', 'Biodegradable Packaging Items', '80 mm paper based lid white', 'Cup', 500, 1000, 225.00, NULL),
('HIT-CUP-006', 'Biodegradable Packaging Items', '12oz Bio Bagasse Cup (Without Lid)', 'Cup', 500, 1000, 605.00, NULL),
('HIT-CUP-007', 'Biodegradable Packaging Items', '12oz RIPPLE WRAP PAPER CUP', 'Cup', 500, 500, 195.00, NULL),
('HIT-CUP-008', 'Biodegradable Packaging Items', '8oz RIPPLE WRAP PAPER CUP', 'Cup', 500, 1000, 145.00, NULL),
('HIT-CUP-009', 'Biodegradable Packaging Items', 'Paper Cup 16oz', 'Cup',500, 500, 230.00, NULL);

-- Utensil
INSERT INTO Product (Prod_ID, Prod_Type, Prod_Info, Prod_SubCategory, Prod_Qty, Prod_QtyPerUnit, Prod_Price, Prod_Weight) VALUES
('HIT-UTS-001', 'Biodegradable Packaging Items', 'Wooden Stirrer', 'Utensil', 500, 10000, 125.00, NULL),
('HIT-UTS-002', 'Biodegradable Packaging Items', 'Spoon Corn Starch', 'Utensil', 500, 1000, 180.00, NULL),
('HIT-UTS-003', 'Biodegradable Packaging Items', 'Fork Corn Starch', 'Utensil', 500, 1000, 180.00, NULL),
('HIT-UTS-004', 'Biodegradable Packaging Items', 'Wooden Spoon', 'Utensil', 500, 2500, 320.00, NULL),
('HIT-UTS-005', 'Biodegradable Packaging Items', 'Wooden Fork', 'Utensil', 500, 2500, 320.00, NULL),
('HIT-UTS-006', 'Biodegradable Packaging Items', '3 in 1 Cutlery', 'Utensil', 500, 500, 225.00, NULL);



-----Customer Table-----

-- Regular Customers
INSERT INTO Customer (Cust_ID, Cust_Type, Cust_Name, Cust_Email, Cust_Contact, Cust_Add) VALUES
('C001', 'customer', 'Ali Baba', 'alibaba@gmail.com', '0123456789', '10 Jalan Bunga Raya, KL'),
('C002', 'customer', 'Siti Liza', 'sitilizzo@gmail.com', '0172345678', '21 Jalan Teratai, Seremban');

-- Agents
INSERT INTO Customer (Cust_ID, Cust_Type, Cust_Name, Cust_Email, Cust_Contact, Cust_Add) VALUES
('A001', 'agent', 'Nurul Aini', 'nurulaini@gmail.com', '0134567890', '88 Jalan Damai, Shah Alam'),
('A002', 'agent', 'Faiz Saduk', 'faizsaduk@gmail.com', '0168765432', '31 Jalan Ria, Ipoh');

-- Dropshippers
INSERT INTO Customer (Cust_ID, Cust_Type, Cust_Name, Cust_Email, Cust_Contact, Cust_Add) VALUES
('D001', 'dropshipper', 'Jason Derulo', 'jasonderulo@gmail.com', '0145678901', '55 Lorong Anggerik, Penang'),
('D002', 'dropshipper', 'Alicia Keys', 'aliciakeys@gmail.com', '0189988776', '19 Taman Bukit, Johor Bahru');



-----Employee Table-----
INSERT INTO Employee (Emp_ID, Emp_Name, Emp_Contact)
VALUES ('E001', 'Thomas Tan', '0112233445');



-----Orders & OrderDetails Table-----

--------------------------------------------------------------------------------
-- Order for Regular Customer (Ali) - Sharifah Ready to Eat Products
-- No discount (Ord_CommRate = 0)
--------------------------------------------------------------------------------

INSERT INTO Orders (Ord_ID, Ord_Date, Ord_CommRate, OD_PayMethod, Cust_ID)
VALUES ('O001', TO_DATE('01-05-2025', 'DD-MM-YYYY'), 0, 'CreditCard', 'C001');

-- PR001: 2 units @ 13.90 = 27.80
-- PR003: 1 unit @ 23.90 = 23.90
INSERT INTO OrderDetail (OD_ID, OD_Qty, O_Disc, Ord_Price, Ord_ID, Prod_ID) VALUES
('OD001', 2, 0, 27.80, 'O001', 'PR001'),
('OD002', 1, 0, 23.90, 'O001', 'PR003');

UPDATE Product SET Prod_Qty = Prod_Qty - 2  WHERE Prod_ID = 'PR001';
UPDATE Product SET Prod_Qty = Prod_Qty - 1  WHERE Prod_ID = 'PR003';


--------------------------------------------------------------------------------
-- Order for Regular Customer (Siti) - Biodegradable Packaging Items
-- No discount
--------------------------------------------------------------------------------

INSERT INTO Orders (Ord_ID, Ord_Date, Ord_CommRate, OD_PayMethod, Cust_ID)
VALUES ('O002', TO_DATE('02-05-2025', 'DD-MM-YYYY'), 0, 'BankTransfer', 'C002');

-- HIT-CTR-001: 3 units @ 285.00 = 855.00
-- HIT-CB-001: 2 units @ 7.50 = 15.00
INSERT INTO OrderDetail (OD_ID, OD_Qty, O_Disc, Ord_Price, Ord_ID, Prod_ID) VALUES
('OD003', 3, 0, 855.00, 'O002', 'HIT-CTR-001'), 
('OD004', 2, 0, 15.00, 'O002', 'HIT-CB-001');

UPDATE Product SET Prod_Qty = Prod_Qty - 3 WHERE Prod_ID = 'HIT-CTR-001';
UPDATE Product SET Prod_Qty = Prod_Qty - 2 WHERE Prod_ID = 'HIT-CB-001';


--------------------------------------------------------------------------------
-- Order for Agent (Aini) - Sharifah Ready to Eat Products
-- 10% discount on each item
--------------------------------------------------------------------------------

INSERT INTO Orders (Ord_ID, Ord_Date, Ord_CommRate, OD_PayMethod, Cust_ID)
VALUES ('O003', TO_DATE('03-05-2025', 'DD-MM-YYYY'), 10, 'BankTransfer', 'A001');

-- PR002: 10 units @ 12.90 = 129.00 - 10% = 116.10
-- PR005: 50 units @ 11.90 = 595.00 - 10% = 535.5
INSERT INTO OrderDetail (OD_ID, OD_Qty, O_Disc, Ord_Price, Ord_ID, Prod_ID) VALUES
('OD005', 10, 12.90, 116.10, 'O003', 'PR002'),  
('OD006', 50, 59.50, 535.5, 'O003', 'PR005');

UPDATE Product SET Prod_Qty = Prod_Qty - 10 WHERE Prod_ID = 'PR002';
UPDATE Product SET Prod_Qty = Prod_Qty - 50 WHERE Prod_ID = 'PR005';


--------------------------------------------------------------------------------
-- Order for Agent (Faiz) - Biodegradable Packaging Items
-- 3% discount
--------------------------------------------------------------------------------

INSERT INTO Orders (Ord_ID, Ord_Date, Ord_CommRate, OD_PayMethod, Cust_ID)
VALUES ('O004', TO_DATE('04-05-2025', 'DD-MM-YYYY'), 3, 'TnG', 'A002');

-- HIT-CTR-005: 20 units @ 195.00 = 3900.00 - 3% = 3783.00
-- HIT-UTS-003: 100 units @ 180.00 = 18000.00 - 3% = 17460.00
INSERT INTO OrderDetail (OD_ID, OD_Qty, O_Disc, Ord_Price, Ord_ID, Prod_ID) VALUES
('OD007', 20, 117.00, 3783.00, 'O004', 'HIT-CTR-005'),  
('OD008', 100, 540.00, 17460.00, 'O004', 'HIT-UTS-003');

UPDATE Product SET Prod_Qty = Prod_Qty - 20 WHERE Prod_ID = 'HIT-CTR-005';
UPDATE Product SET Prod_Qty = Prod_Qty - 100 WHERE Prod_ID = 'HIT-UTS-003';


--------------------------------------------------------------------------------
-- Order for Dropshipper (Jason) - Sharifah Ready to Eat Products
-- 10% discount
--------------------------------------------------------------------------------

INSERT INTO Orders (Ord_ID, Ord_Date, Ord_CommRate, OD_PayMethod, Cust_ID)
VALUES ('O005', TO_DATE('05-05-2025', 'DD-MM-YYYY'), 10, 'CreditCard', 'D001');

-- PR001: 15 units @ 13.90 = 208.50 - 10% = 187.65
-- PR006: 10 units @ 11.90 = 119.00 - 10% = 107.10
INSERT INTO OrderDetail (OD_ID, OD_Qty, O_Disc, Ord_Price, Ord_ID, Prod_ID) VALUES 
('OD009', 15, 20.85, 187.65, 'O005', 'PR001'), 
('OD010', 10, 11.90, 107.10, 'O005', 'PR006');

UPDATE Product SET Prod_Qty = Prod_Qty - 15 WHERE Prod_ID = 'PR001';
UPDATE Product SET Prod_Qty = Prod_Qty - 10 WHERE Prod_ID = 'PR006';


--------------------------------------------------------------------------------
-- Order for Dropshipper (Alicia) - Biodegradable Packaging Items
-- 3% discount
--------------------------------------------------------------------------------

INSERT INTO Orders (Ord_ID, Ord_Date, Ord_CommRate, OD_PayMethod, Cust_ID)
VALUES ('O006', TO_DATE('06-05-2025', 'DD-MM-YYYY'), 3, 'BankTransfer', 'D002');

-- HIT-CTR-002: 30 units @ 225.00 = 6750.00 - 3% = 6547.50
-- HIT-CB-005: 20 units @ 23.50 = 470.00 - 3% = 455.90
INSERT INTO OrderDetail (OD_ID, OD_Qty, O_Disc, Ord_Price, Ord_ID, Prod_ID) VALUES
('OD011', 30, 202.50, 6547.50, 'O006', 'HIT-CTR-002'),
('OD012', 20, 14.10, 455.90, 'O006', 'HIT-CB-005');

UPDATE Product SET Prod_Qty = Prod_Qty - 30 WHERE Prod_ID = 'HIT-CTR-002';
UPDATE Product SET Prod_Qty = Prod_Qty - 20 WHERE Prod_ID = 'HIT-CB-005';




----Delivery Table-----

-- Delivery for Order O001 (Ali Ahmad) - HIT's in-house service
INSERT INTO Delivery (Del_ID, Del_Fee, Del_Method, Ord_ID, Emp_ID)
VALUES ('D001', 15.00, 'HIT In-house Service', 'O001', 'E001'); 

-- Delivery for Order O002 (Siti Aminah) - Grab
INSERT INTO Delivery (Del_ID, Del_Fee, Del_Method, Ord_ID, Emp_ID)
VALUES ('D002', 20.00, 'Grab', 'O002', 'E001');  

-- Delivery for Order O003 (Nurul Aini) - Lalamove
INSERT INTO Delivery (Del_ID, Del_Fee, Del_Method, Ord_ID, Emp_ID)
VALUES ('D003', 25.00, 'Lalamove', 'O003', 'E001'); 

-- Delivery for Order O004 (Mohd Faiz) - HIT's in-house service
INSERT INTO Delivery (Del_ID, Del_Fee, Del_Method, Ord_ID, Emp_ID)
VALUES ('D004', 18.00, 'HIT In-house Service', 'O004', 'E001');  

-- Delivery for Order O005 (Jason Lim) - Grab
INSERT INTO Delivery (Del_ID, Del_Fee, Del_Method, Ord_ID, Emp_ID)
VALUES ('D005', 12.00, 'Grab', 'O005', 'E001');  

-- Delivery for Order O006 (Alicia Wong) - Lalamove
INSERT INTO Delivery (Del_ID, Del_Fee, Del_Method, Ord_ID, Emp_ID)
VALUES ('D006', 10.00, 'Lalamove', 'O006', 'E001');  





-----Vendor Table-----

--Sharifah Food Sdn Bhd Vendor
INSERT INTO Vendor (Vend_ID, Vend_Category, Vend_Name, Vend_Contact, Vend_BD)
VALUES ('V001', 'Sharifah Food Sdn Bhd', 'Sharifah Food Sdn Bhd', '0123456789', '20230501');

-- Other Vendor
INSERT INTO Vendor (Vend_ID, Vend_Category, Vend_Name, Vend_Contact, Vend_BD) VALUES
('V002', 'Other vendors', 'Eco Pack Supplies', '0198765432', '20230415'),
('V003', 'Other vendors', 'Green Packaging Ltd', '0181122334', '20230510'),
('V004', 'Other vendors', 'Fresh Supplies Co.', '0174433221', '20230425');





-----Purchase & Purchase Detail Table-----
--------------------------------------------------------------------------------
-- Purchase for Vendor Sharifah Food Sdn Bhd (V001)
INSERT INTO Purchase (P_ID, P_Date, Vend_ID)
VALUES ('P001',TO_DATE('01-05-2025', 'DD-MM-YYYY'), 'V001');  

-- Purchase Detail for Purchase P001 (Sharifah Food Sdn Bhd)
INSERT INTO PurchaseDetail (P_ID, Prod_ID, P_Qty, P_Rec) VALUES
('P001', 'PR001', 50, 500.00), 
('P001', 'PR002', 50, 300.00), 
('P001', 'PR003', 50, 600.00);

-- Update Product quantities for P001 purchase
UPDATE Product
SET Prod_Qty = Prod_Qty + 50 
WHERE Prod_ID IN ('PR001', 'PR002', 'PR003');
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
-- Purchase for Vendor Eco Pack Supplies (V002)
INSERT INTO Purchase (P_ID, P_Date, Vend_ID)
VALUES ('P002', TO_DATE('02-05-2025', 'DD-MM-YYYY'), 'V002');  

-- Purchase Detail for Purchase P002 (Eco Pack Supplies)
INSERT INTO PurchaseDetail (P_ID, Prod_ID, P_Qty, P_Rec) VALUES
('P002', 'HIT-CUP-001', 200, 1000.00),  
('P002', 'HIT-CTR-001', 200,1500.00),  
('P002', 'HIT-CB-001', 200, 50.00);  

-- Update Product quantities for P002 purchase
UPDATE Product
SET Prod_Qty = Prod_Qty + 200  
WHERE Prod_ID IN ('HIT-CUP-001', 'HIT-CTR-001', 'HIT-CB-001');
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
-- Purchase for Vendor Green Packaging Ltd (V003)
INSERT INTO Purchase (P_ID, P_Date, Vend_ID)
VALUES ('P003', TO_DATE('03-05-2025', 'DD-MM-YYYY'), 'V003');  

-- Purchase Detail for Purchase P003 (Green Packaging Ltd)
INSERT INTO PurchaseDetail (P_ID, Prod_ID, P_Qty, P_Rec) VALUES
('P003', 'HIT-CUP-002', 150, 1200.00),
('P003', 'HIT-CTR-005', 150, 1000.00);  

-- Update Product quantities for P003 purchase
UPDATE Product
SET Prod_Qty = Prod_Qty + 150  
WHERE Prod_ID IN ('HIT-CUP-002', 'HIT-CTR-005');
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
-- Purchase for Vendor Fresh Supplies Co. (V004)
INSERT INTO Purchase (P_ID, P_Date, Vend_ID)
VALUES ('P004', TO_DATE('04-05-2025', 'DD-MM-YYYY'), 'V004');  

-- Purchase Detail for Purchase P004 (Fresh Supplies Co.)
INSERT INTO PurchaseDetail (P_ID, Prod_ID, P_Qty, P_Rec) VALUES
('P004', 'HIT-UTS-001',100 , 500.00),  
('P004', 'HIT-CB-004',100, 700.00);  

-- Update Product quantities for P004 purchase
UPDATE Product
SET Prod_Qty = Prod_Qty + 100 
WHERE Prod_ID IN ('HIT-UTS-001', 'HIT-CB-004');
--------------------------------------------------------------------------------



-- Formatting Settings
SET LINESIZE 250
SET PAGESIZE 100

-- Customer Table
COLUMN CUST_ID        FORMAT A8
COLUMN CUST_TYPE      FORMAT A14
COLUMN CUST_NAME      FORMAT A18
COLUMN CUST_EMAIL     FORMAT A30
COLUMN CUST_CONTACT   FORMAT A15
COLUMN CUST_ADD       FORMAT A30

-- Employee Table
COLUMN EMP_ID         FORMAT A8
COLUMN EMP_NAME       FORMAT A30
COLUMN EMP_CONTACT    FORMAT A15

-- Orders Table
COLUMN ORD_ID         FORMAT A8
COLUMN CUST_ID        FORMAT A8
COLUMN ORD_DATE       FORMAT A14
COLUMN ORD_COMMRATE   FORMAT 990.99
COLUMN OD_PAYMETHOD   FORMAT A15

-- Delivery Table
COLUMN DEL_ID         FORMAT A10
COLUMN ORD_ID         FORMAT A8
COLUMN EMP_ID         FORMAT A8
COLUMN DEL_FEE        FORMAT 9990.99
COLUMN DEL_METHOD     FORMAT A22

-- Product Table
COLUMN PROD_ID          FORMAT A15
COLUMN PROD_TYPE        FORMAT A35
COLUMN PROD_SUBCATEGORY FORMAT A20
COLUMN PROD_INFO        FORMAT A40
COLUMN PROD_QTY         FORMAT 99999999
COLUMN PROD_QTYPERUNIT  FORMAT 99999999
COLUMN PROD_PRICE       FORMAT 99990.99
COLUMN PROD_WEIGHT      FORMAT 99990.99

-- OrderDetail Table
COLUMN OD_ID        FORMAT A10
COLUMN ORD_ID       FORMAT A8
COLUMN PROD_ID      FORMAT A15
COLUMN OD_QTY       FORMAT 9999
COLUMN O_DISC       FORMAT 990.99
COLUMN ORD_PRICE    FORMAT 99990.99

-- Vendor Table
COLUMN VEND_ID       FORMAT A8
COLUMN VEND_CATEGORY FORMAT A25
COLUMN VEND_NAME     FORMAT A30
COLUMN VEND_CONTACT  FORMAT A15
COLUMN VEND_BD       FORMAT 9999999999

-- Purchase Table
COLUMN P_ID          FORMAT A8
COLUMN VEND_ID       FORMAT A8
COLUMN P_DATE        FORMAT A12
COLUMN PURCHASE_ID     FORMAT A12
COLUMN PURCHASE_DATE   FORMAT A14

-- PurchaseDetail Table
COLUMN P_ID          FORMAT A8
COLUMN PROD_ID       FORMAT A15
COLUMN P_QTY         FORMAT 9999
COLUMN P_REC         FORMAT 99990.99






