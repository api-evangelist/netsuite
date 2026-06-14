# Oracle NetSuite GraphQL Schema

## Overview

This conceptual GraphQL schema represents the Oracle NetSuite cloud ERP data model, derived from the SuiteTalk REST Web Services API, REST Record API, SuiteAnalytics Connect, and the SuiteQL query layer. NetSuite does not currently expose a native GraphQL endpoint; this schema captures the underlying data model in GraphQL SDL form to facilitate tooling, documentation, federation mapping, and integration planning.

## Source

- Provider: Oracle NetSuite
- REST API: https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/book_1559132836.html
- REST API Browser: https://system.netsuite.com/help/helpcenter/en_US/APIs/REST_API_Browser/record/v1/2023.1/index.html
- SuiteAnalytics: https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/chapter_1541138949.html
- GitHub: https://github.com/oracle-netsuite

## Domain Coverage

The schema covers the following NetSuite functional areas:

### Financial Management
Account, AccountingPeriod, JournalEntry, JournalLine, Currency, ExchangeRate, RevenueRecognition, AdvancedBilling, Contract, ContractLine, Revenue, Term, PaymentMethod, PaymentTerm

### Entities / Relationships
Entity, Customer, Vendor, Employee, Contact, Subsidiary, Department, Location, Classification

### Transactions
Transaction, Invoice, Bill, PurchaseOrder, SalesOrder, Estimate, CreditMemo, VendorCredit, Check, Deposit

### Inventory and Supply Chain
Inventory, InventoryItem, InventoryDetail, InventoryLocation, ItemFulfillment, ItemReceipt

### Manufacturing
WorkOrder, WorkOrderCompletion, BOM, BOMRevision, BOMComponent

### Pricing and Tax
PricingGroup, PriceLevel, TaxItem, TaxCode, TaxGroup

### Platform / Customization
CustomRecord, RecordType, CustomField, SearchDefinition, SavedSearch, ReportDefinition, Role, Permission, Script, ScriptDeployment, WorkflowDefinition, Integration, Project

## Key Relationships

- `Customer`, `Vendor`, `Employee`, and `Contact` all extend the base `Entity` type
- `Invoice`, `Bill`, `SalesOrder`, `PurchaseOrder`, `Estimate`, `CreditMemo`, `VendorCredit`, `Check`, and `Deposit` all implement the `Transaction` interface
- `JournalEntry` contains one or more `JournalLine` records
- `BOM` (Bill of Materials) references `BOMRevision` which references `BOMComponent` items
- `WorkOrder` references both `BOM` and `InventoryItem`
- `InventoryDetail` links `InventoryItem` to `InventoryLocation`
- `Contract` contains `ContractLine` records and is linked to `Revenue` and `RevenueRecognition`
- `CustomRecord` is an instance of a `RecordType`, with fields defined by `CustomField`
- `Script` is deployed via `ScriptDeployment` and can be attached to `WorkflowDefinition`

## Type Count

75 named types are defined in `netsuite-schema.graphql`.

## Usage Notes

- Internal IDs in NetSuite are represented as `ID` scalars.
- Dates use the `String` scalar with ISO 8601 format (NetSuite's REST API returns dates as strings).
- Monetary amounts are represented as `Float`.
- Enum values map to NetSuite list/record reference values.
- Many NetSuite records support custom fields; these are captured via the `customFields` relationship to `CustomField`.
- SuiteQL queries can span multiple record types and are represented at the query root via `suiteQL`.
