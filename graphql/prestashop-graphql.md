# PrestaShop GraphQL Schema

## Overview

PrestaShop is an open-source e-commerce platform that exposes its data through a REST/Webservice API and a newer Admin API (PrestaShop 9+). There is no native GraphQL endpoint in PrestaShop's official release. This conceptual GraphQL schema represents the full domain model of the PrestaShop platform, derived from its 60+ Webservice resources and Admin API objects.

Developers who want to add a GraphQL layer to PrestaShop can implement it using:
- A custom GraphQL server (e.g., graphql-php, Apollo Server) that wraps the PrestaShop Webservice API
- A PrestaShop module that exposes a `/api/graphql` endpoint backed by the core ObjectModel layer
- An API gateway or BFF (Backend for Frontend) pattern that translates GraphQL queries to REST calls

## Schema Source

- **API Type**: REST/Webservice (conceptual GraphQL)
- **Source**: Derived from [PrestaShop Webservice API](https://devdocs.prestashop-project.org/9/webservice/) and [Admin API](https://devdocs.prestashop-project.org/9/admin-api/)
- **GitHub**: https://github.com/PrestaShop/PrestaShop
- **Resources modeled**: 55+ named types covering products, orders, customers, catalog, CMS, tax, shipping, and administration

## Root Types

### Query

Read operations across all PrestaShop resources:

- `product(id: ID!)` / `products(filter: ProductFilter, pagination: Pagination)` — catalog items
- `category(id: ID!)` / `categories(...)` — product tree navigation
- `order(id: ID!)` / `orders(filter: OrderFilter, pagination: Pagination)` — sales orders
- `customer(id: ID!)` / `customers(...)` — registered shoppers
- `cart(id: ID!)` / `carts(...)` — active and abandoned carts
- `carrier(id: ID!)` / `carriers(...)` — shipping methods
- `stock(productId: ID!, combinationId: ID)` — real-time stock levels
- `cms(id: ID!)` / `cmsPages(...)` — content pages
- `configuration(key: String!)` / `configurations(...)` — shop settings

### Mutation

Write operations:

- `createProduct` / `updateProduct` / `deleteProduct`
- `createOrder` / `updateOrderStatus`
- `createCustomer` / `updateCustomer`
- `addToCart` / `removeFromCart` / `applyCartRule`
- `createVoucher` / `updateVoucher`
- `updateStock` / `createStockMovement`
- `createAddress` / `updateAddress` / `deleteAddress`

## Type Summary

| Category | Types |
|---|---|
| Catalog | Product, ProductFeature, ProductFeatureValue, ProductAttribute, ProductAttributeGroup, ProductCombination, Category, Manufacturer, Supplier, Image, Attachment, Tag, SearchTag |
| Pricing | SpecificPrice, CartRule, Voucher, Tax, TaxRule, TaxRuleGroup |
| Inventory | Stock, StockAvailability, StockMovement, Warehouse |
| Orders | Order, OrderDetail, OrderHistory, OrderState, OrderSlip, OrderInvoice, OrderCarrier |
| Customers | Customer, CustomerGroup, CustomerMessage, CustomerThread |
| Addresses | Address, Country, State, Zone |
| Cart | Cart, CartProduct, CartRule |
| Shipping | Carrier, CarrierGroup |
| Payments | Currency, CurrencyRestriction |
| Internationalization | Language, Translation |
| CMS | CmsCategory, CmsPage |
| Administration | Shop, ShopGroup, Employee, Profile, Permission, Module, Configuration, Hook |
| Communication | Contact, Customization |

## Authentication

The PrestaShop Webservice API uses HTTP Basic Auth with a store-generated API key. A GraphQL implementation would carry the same authentication model, passing the key as a Bearer token or Basic Auth header.

The Admin API (PrestaShop 9+) uses OAuth 2.0 Client Credentials. A GraphQL wrapper targeting the Admin API would accept an `Authorization: Bearer <token>` header.

## Pagination

All list queries accept a `Pagination` input:

```graphql
input Pagination {
  page: Int
  limit: Int
  sortBy: String
  sortOrder: SortOrder
}

enum SortOrder {
  ASC
  DESC
}
```

## References

- Webservice Docs: https://devdocs.prestashop-project.org/9/webservice/
- Admin API Docs: https://devdocs.prestashop-project.org/9/admin-api/
- GitHub: https://github.com/PrestaShop/PrestaShop
- Community: https://www.prestashop.com/forums/
