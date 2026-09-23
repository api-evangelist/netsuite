# Oracle NetSuite GraphQL Schema

## Overview

This conceptual GraphQL schema represents the Oracle NetSuite ERP platform, covering financials, accounting, order management, inventory, CRM, HR, and professional services automation. The schema is derived from the SuiteTalk REST Web Services API (record and SuiteQL query endpoints) documented at https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/.

NetSuite does not natively expose a GraphQL endpoint. This schema is a conceptual mapping of the REST API's record types and relationships into GraphQL types, suitable for use in API gateway layers, schema-stitching implementations, or documentation tooling.

## Authentication

NetSuite uses OAuth 2.0 for all API access. Supported flows:

- Authorization Code (user-interactive)
- Client Credentials / Machine-to-Machine (M2M)
- Token-Based Authentication (TBA)

All requests target an account-specific base URL:

```
https://{accountId}.suitetalk.api.netsuite.com/services/rest/record/v1
```

## Schema Source

- SuiteTalk REST Record API: https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/book_1559132836.html
- SuiteQL Query API: https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/book_1559132836.html
- Authentication Guide: https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/chapter_4369651411.html

## Type Summary

The schema defines 73 named GraphQL types covering:

| Domain | Types |
|---|---|
| General Ledger / Financials | Account, AccountingPeriod, AccountType, SubsidiaryDetail, CurrencyDetail, JournalEntry, JournalLine, GLBudget, GLForecast |
| Customers | Customer, CustomerBilling, CustomerShipping, CustomerStatus |
| Vendors | Vendor, VendorBilling, VendorTerm |
| Employees / HR | Employee, EmployeePayroll, Department, LocationDetail, ClassDetail |
| Transactions | Invoice, InvoiceLine, Bill, BillLine, SalesOrder, SalesOrderLine, PurchaseOrder, POLine, CreditMemo, CreditLine |
| Payments | Payment, CustomerPayment, VendorPayment, Check |
| Inventory / Items | InventoryItem, ItemGroup, AssemblyItem, ServiceItem, NonInventoryItem, ItemLocation, ItemPrice, ItemBOM, BOM |
| Manufacturing | WorkOrder, WorkOrderItem |
| Shared Transaction | TransactionLine |
| Expenses | Expense, ExpenseReport |
| Projects / Time | Project, ProjectBudget, Resource, TimeEntry |
| Fulfillment / Receipts | ShipItem, ItemFulfillment, ItemReceipt |
| Transfers | InventoryTransfer, TransferOrder |
| Access Control | Role, Permission |
| Scripting / Automation | Script, ScriptDeployment |
| Security / Sessions | APIKey, Token, Session |

## Query Capabilities

The root `Query` type exposes per-record-type fetch (by internal ID) and list (paginated) operations, plus a `suiteQL` query for SQL-like ad-hoc reporting.

## Mutation Capabilities

The root `Mutation` type exposes create, update, and delete operations for all major record types that support write access in the SuiteTalk REST API.

## File

See `oracle-netsuite-schema.graphql` for the full type definitions.
