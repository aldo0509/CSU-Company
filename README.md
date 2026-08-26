# CSU-Company
Odoo.sh 17.0+e (Enterprise Edition) Implementation


Flow CSU-Company
ROADMAP IMPLEMENTASI ODOO.SH 17.0+e
Enterprise Edition • PT Centrochem Solusindo Utama (CSU)

**Tujuan Dokumen**
Dokumen ini menyusun roadmap implementasi dan pengembangan Odoo.sh 17.0+e Enterprise Edition berdasarkan alur bisnis yang telah diidentifikasi di PT CSU. Roadmap disusun bertahap agar setiap modul memiliki fondasi master data, integrasi, pengujian, dan alur operasional yang jelas sebelum masuk ke modul Accounting.

1. Gambaran Besar Flow Sistem
1.1 Flow Sales
QUOTATION
    │
    ├── Approval Discount
    │
    ├── Email Notification Kevin
    │
    ▼
Customer Quotation
    │
    ├──────────────► CUSTOMER APPROVE
    │                     │
    ▼                     ▼
Selesai Penawaran      SALES ORDER
                           │
                           ├── Pricelist
                           ├── Repeat Order
                           ├── Delivery
                           ├── Invoice
                           └── Receipt
                                  │
                                  ▼
                            ACCOUNTING
                            (Tahap akhir)
 
1.2 Flow Product
PURCHASE
   │
   ▼
Raw Material / Sparepart
   │
   ▼
INVENTORY
   │
   ▼
BILL OF MATERIAL
   │
   ▼
MANUFACTURING ORDER
   │
   ├── Produksi
   ├── Serial Number
   └── Bulk Production
          │
          ▼
    Finished Goods Stock
          │
     ┌────┴─────┐
     ▼          ▼
  RENTAL      SALES

2. Phase 0 – Foundation & Analysis
Tujuan: menyiapkan fondasi sistem, master data, hak akses, dan audit terhadap konfigurasi existing.
•	☐ Audit seluruh modul existing di Odoo.sh 17.0+e.
•	☐ Identifikasi custom module CSU.
•	☐ Mapping module standard versus custom.
•	☐ Audit custom field dan konfigurasi Odoo Studio.
•	☐ Audit automated action dan server action.
•	☐ Audit approval workflow existing.
•	☐ Audit struktur inventory dan warehouse.
•	☐ Audit product category.
•	☐ Audit customer dan vendor master.
•	☐ Audit user dan access rights.
•	☐ Mapping role setiap user dan PIC.
Role Awal
User / Role	Tanggung Jawab
Alia	Quotation / Penawaran
Kevin	Approval Discount
Bupuji	Pricelist
Evi	Sales Order
Erni	Purchase / Manufacturing / Rental
Warehouse	Stock & Delivery
Technician	Installation & Maintenance
Accounting	Invoice, Receipt, Financial

3. Phase 1 – Modul Quotation / Penawaran
3.1 Target Flow
Alia Create Quotation
        │
        ▼
System Calculate Discount
        │
        ├── Discount <= Limit ──► Ready to Send
        │
        └── Discount > Limit
                 │
                 ▼
          Approval Required
                 │
                 ▼
              Kevin
             │     │
        Approve   Reject
             │       │
             ▼       ▼
       Ready Send  Back Revision
             │
             ▼
      Send Email Customer
             │
             ▼
      Customer Accept
             │
             ▼
       Convert to Sales Order
3.2 Quotation Creation
•	☐ Alia membuat quotation.
•	☐ Customer wajib dipilih.
•	☐ Product wajib tersedia.
•	☐ Quantity dan harga.
•	☐ Pricelist.
•	☐ Discount.
•	☐ Payment term jika diperlukan.
•	☐ Validity quotation.
3.3 Discount Approval
Business rule threshold discount perlu disepakati terlebih dahulu. Contoh:
0% – 10%     = Auto Approve
> 10%         = Approval Kevin
•	☐ Buat field discount threshold.
•	☐ Detect discount per line.
•	☐ Detect total discount quotation.
•	☐ Trigger approval jika melebihi batas.
•	☐ Status approval.
•	☐ Approve button untuk Kevin.
•	☐ Reject button dan reject reason.
•	☐ Lock quotation sebelum approval.
•	☐ Approval history.
Status yang disarankan:
Draft
↓
Waiting Approval
↓
Approved
↓
Sent
↓
Customer Accepted
↓
Sales Order
3.4 Email Notification
•	☐ Email notification kepada Kevin saat quotation membutuhkan approval.
•	☐ Email notification kepada Alia setelah quotation approved atau rejected.
•	☐ Email template quotation.
•	☐ Email template approval.
•	☐ Email template rejection.
•	☐ Activity notification.
•	☐ Link langsung ke quotation.
3.5 Send to Customer
•	☐ Generate PDF quotation.
•	☐ Email quotation kepada customer.
•	☐ Track status sent.
•	☐ Customer portal acceptance jika digunakan.
•	☐ Convert quotation menjadi Sales Order.
Deliverable Phase 1: Quotation, Discount Approval, Email Notification, Customer Sending, dan Quotation History.

4. Phase 2 – Sales Order
4.1 Pricelist Management
PIC: Bupuji
•	☐ Create dan update pricelist.
•	☐ Customer-specific pricelist.
•	☐ Minimum quantity price.
•	☐ Discount rule.
•	☐ Pricelist effective date.
4.2 Sales Order
PIC: Evi
•	☐ Create Sales Order.
•	☐ Customer selection.
•	☐ Product selection.
•	☐ Automatic pricelist.
•	☐ Quantity.
•	☐ Discount.
•	☐ Salesperson.
•	☐ Sales history.
4.3 Repeat Order
Requirement: Evi hanya dapat membuat repeat order untuk customer lama atau customer yang sebelumnya sudah melewati proses quotation.
Customer Selected
      │
      ▼
Check Customer History
      │
      ├── Existing Customer ──► Allow Repeat Order
      │
      └── New Customer ───────► Require Quotation
•	☐ Customer transaction history.
•	☐ Validation new customer.
•	☐ Repeat order button.
•	☐ Duplicate SO lines.
•	☐ Previous price reference.
4.4 Delivery / Surat Jalan
•	☐ Delivery Order customization.
•	☐ Surat Jalan PDF.
•	☐ Serial Number jika applicable.
•	☐ Customer signature jika diperlukan.
•	☐ Delivery history.
4.5 Invoice & Tanda Terima
•	☐ Invoice template.
•	☐ Invoice numbering.
•	☐ Payment registration.
•	☐ Receipt / Tanda Terima template.
•	☐ Customer payment history.
Deliverable Phase 2: Pricelist, Sales Order, Repeat Order Validation, Delivery Order, Surat Jalan, Invoice, dan Receipt.

5. Phase 3 – Purchase & Inventory
Phase ini harus selesai sebelum Manufacturing karena proses produksi bergantung pada ketersediaan bahan baku, struktur warehouse, dan stock tracking.
5.1 Purchase
•	☐ Purchase Requisition jika diperlukan.
•	☐ Request for Quotation Vendor.
•	☐ Purchase Order.
•	☐ Approval PO jika diperlukan.
•	☐ Vendor price.
•	☐ Lead time.
5.2 Inventory
•	☐ Warehouse structure.
•	☐ Raw Material location.
•	☐ Sparepart location.
•	☐ Finished Goods location.
•	☐ Rental Asset location.
•	☐ Scrap location.
•	☐ Maintenance material location.
 
6. Phase 4 – Manufacturing
6.1 Target Flow
Purchase Sparepart
       │
       ▼
Receive Inventory
       │
       ▼
Raw Material Available
       │
       ▼
Create BoM
       │
       ▼
Manufacturing Order
       │
       ▼
Input Production Qty
       │
       ▼
Generate Serial Number
       │
       ▼
Bulk Production
       │
       ▼
Finished Product
       │
       ▼
Stock Gudang
6.2 Bill of Materials
•	☐ Create BoM.
•	☐ Define component.
•	☐ Define quantity.
•	☐ Define operation.
•	☐ Define work center jika diperlukan.
6.3 Manufacturing Order
•	☐ Create MO.
•	☐ Input product.
•	☐ Input production quantity.
•	☐ Check raw material availability.
•	☐ Reserve components.
•	☐ Production tracking.
 
6.4 Serial Number
Requirement: setiap unit produk hasil produksi harus memiliki Serial Number.
Production = 100 Unit

SN-0001
SN-0002
SN-0003
...
SN-0100
•	☐ Serial Number mandatory.
•	☐ Automatic serial generation.
•	☐ Serial Number uniqueness.
•	☐ Production traceability.
•	☐ Component traceability.
•	☐ Serial history.
6.5 Bulk Production
MO Quantity: 100
        │
        ▼
Generate 100 Serial Number
        │
        ▼
Produce 100 Units
        │
        ▼
Validate Production
        │
        ▼
100 Finished Products
        │
        ▼
Finished Goods Stock
•	☐ Produksi massal dalam satu Manufacturing Order.
•	☐ Automatic generation Serial Number sesuai jumlah produksi.
•	☐ Validasi hasil produksi.
•	☐ Finished product otomatis masuk ke stok gudang.
Deliverable Phase 4: Purchase Component, BoM, Manufacturing Order, Bulk Production, Automatic Serial Number, dan Finished Product Inventory.

7. Phase 5 – Rental Management
Rental merupakan salah satu area paling kompleks karena berkaitan dengan asset, Serial Number, customer, kontrak, durasi rental, installation, maintenance, sparepart usage, biaya, dan kemungkinan produk dijual.
7.1 Product Classification
Pump
├── Rental
├── Sale
└── Both

Chemical
├── Sale
└── Rental
•	☐ Can be Sold.
•	☐ Can be Rented.
•	☐ Track by Serial Number.
•	☐ Maintenance Required.
•	☐ Rental Product.
7.2 Rental Order
PIC utama: Erni
Sales Request
      │
      ▼
Internal Request
      │
      ▼
Erni Check Availability
      │
      ▼
Create Rental Order
      │
      ▼
Select Customer
      │
      ▼
Select Product
      │
      ▼
Select Serial Number
      │
      ▼
Confirm Rental
•	☐ Rental customer.
•	☐ Rental product.
•	☐ Rental Serial Number.
•	☐ Availability validation.
•	☐ Rental start date.
•	☐ Rental end date.
•	☐ Rental duration.
•	☐ Rental contract.
•	☐ Delivery pump.
•	☐ Return pump.
 
7.3 Serial Number Availability
Available
Reserved
Installed
Rented
Maintenance
Returned
Sold
Scrapped
Sistem harus mencegah satu Serial Number digunakan untuk rental dan penjualan secara bersamaan.
7.4 Installation
•	☐ Installation service product.
•	☐ Installation cost.
•	☐ Technician assignment.
•	☐ Installation date.
•	☐ Installation completion.
7.5 Maintenance
Rental Active
      │
      ▼
Monthly Maintenance Schedule
      │
      ▼
Technician Visit
      │
      ▼
Maintenance Report
      │
      ├── Labor Cost
      ├── Sparepart Used
      └── Other Expense
•	☐ Maintenance schedule.
•	☐ Monthly recurring activity.
•	☐ Technician assignment.
•	☐ Maintenance worksheet.
•	☐ Sparepart consumption.
•	☐ Cost recording.
•	☐ Maintenance history per Serial Number.
7.6 Sparepart Used for Maintenance
•	☐ Inventory Out untuk sparepart yang digunakan.
•	☐ Maintenance Cost recording.
•	☐ Cost linked ke customer.
•	☐ Cost linked ke rental contract.
•	☐ Cost linked ke Pump Serial Number.
7.7 Pump Bisa Rental dan Dijual
Finished Product
      │
      ├── RENTAL
      │      │
      │      ▼
      │  Serial Number remains asset
      │
      └── SALE
             │
             ▼
      Ownership transferred
             │
             ▼
       Serial Number sold
•	☐ Rental availability.
•	☐ Sale availability.
•	☐ Prevent sold item from rental.
•	☐ Asset status.
•	☐ Ownership tracking.
 
8. Phase 6 – Cross Module Integration
Full CSU Flow
CUSTOMER
   │
   ▼
QUOTATION
   │
   ├── Discount Approval
   │
   ▼
CUSTOMER ACCEPT
   │
   ▼
SALES ORDER
   │
   ├───────────────┐
   │               │
   ▼               ▼
PRODUCT SALE    RENTAL REQUEST
   │               │
   ▼               ▼
DELIVERY      RENTAL ORDER
   │               │
   ▼               ▼
INVOICE       SERIAL NUMBER
                   │
                   ▼
             INSTALLATION
                   │
                   ▼
              MAINTENANCE
                   │
                   ▼
               RENTAL END

9. Phase 7 – Accounting
Accounting direkomendasikan dimulai setelah phase sebelumnya lolos UAT. Modul ini menerima dampak transaksi dari seluruh proses bisnis sehingga konfigurasi harus dibangun berdasarkan flow yang sudah tervalidasi.
9.1 Sumber Integrasi
•	Quotation.
•	Sales Order.
•	Purchase.
•	Inventory.
•	Manufacturing.
•	Rental.
•	Maintenance.
•	Sparepart Usage.
•	Invoice.
•	Payment.
9.2 Area yang Perlu Dibahas
Area	Kebutuhan Awal
Revenue	Product Sales Revenue, Rental Revenue, Installation Revenue, Maintenance Revenue
Cost	COGS, Manufacturing Cost, Sparepart Cost, Installation Cost, Maintenance Cost, Technician Cost
Asset	Pump as Rental Asset, Depreciation, Asset Sale, Asset Disposal
Financial	Customer Invoice, Vendor Bill, Payment, Receipt, Journal Entry, Analytic Accounting, Profitability

10. Prioritas Implementasi
Sprint	Scope
Sprint 1	Foundation, Audit Existing System, Master Data, Role & Access
Sprint 2	Quotation, Discount Approval, Email Notification, Customer Sending
Sprint 3	Pricelist, Sales Order, Repeat Order, Delivery
Sprint 4	Invoice, Receipt, Sales Integration
Sprint 5	Purchase, Warehouse, Inventory Structure
Sprint 6	BoM, Manufacturing Order, Serial Number, Bulk Production
Sprint 7	Rental Core, Serial Number Selection, Rental Duration, Rental Contract
Sprint 8	Installation, Maintenance, Technician, Sparepart Consumption
Sprint 9	Sale vs Rental Asset, Full Cross Module Integration, UAT
Sprint 10+	Accounting Analysis, Chart of Accounts, Costing Model, Asset Management, Financial Integration

11. Dependency Map
Modul	Bergantung pada
Quotation	Customer, Product, Pricelist
Sales Order	Quotation, Customer, Pricelist
Delivery	Sales Order, Inventory
Invoice	Sales Order, Delivery
Purchase	Product, Vendor
Manufacturing	Purchase, Inventory, BoM
Serial Number	Inventory, Manufacturing
Rental	Inventory, Serial Number, Customer
Maintenance	Rental, Inventory, Technician
Accounting	Seluruh modul dan alur bisnis

12. Urutan Implementasi Final
FOUNDATION → QUOTATION → SALES → INVENTORY/PURCHASE → MANUFACTURING → RENTAL → INTEGRATION/UAT → ACCOUNTING
13. Kesimpulan dan Fokus Prioritas
Prioritas utama implementasi CSU adalah membangun fondasi transaksi dari proses penawaran hingga pergerakan barang sebelum memasuki Accounting. Area dengan kompleksitas tertinggi adalah integrasi Manufacturing, Serial Number, Rental, Maintenance, dan pencatatan biaya. Kelima area tersebut sebaiknya dianalisis secara detail dan diuji melalui UAT sebelum konfigurasi Accounting difinalisasi.
