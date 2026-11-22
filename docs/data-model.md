# Data model

Star/hub schema, import mode. All CSV loads go through parameter `DataFolder`.

## Tables

| Table | Grain | Rows (approx.) | Source |
|---|---|---|---|
| Orders | order | 99 441 | olist_orders_dataset.csv (+ CustomerUniqueId merged from Customers) |
| OrderItems | order line | 112 650 | olist_order_items_dataset.csv |
| Payments | payment part | 103 886 | olist_order_payments_dataset.csv |
| Reviews | review | 99 224 | olist_order_reviews_dataset.csv (comment texts dropped, `HasComment` kept) |
| Customers | customer_id (1 per order) | 99 441 | olist_customers_dataset.csv |
| Sellers | seller | 3 095 | olist_sellers_dataset.csv (+ SellerStateName, SellerRegion merged from States) |
| Products | product | 32 951 | olist_products_dataset.csv + product_category_name_translation.csv |
| States | Brazilian state | 27 | inline `#table` in M: name, Czech region, capital lat/lng |
| Date | day | 1 096 | DAX `CALENDAR(2016-01-01, 2018-12-31)`, marked as date table |
| _Measures | – | 1 | DAX `{0}`, hidden column, holds all measures |

## Relationships (many → one, single direction unless noted)

| From | To | Note |
|---|---|---|
| Orders[CustomerId] | Customers[CustomerId] | |
| Orders[PurchaseDate] | Date[Date] | |
| OrderItems[OrderId] | Orders[OrderId] | **both directions** – Products/Sellers filter order-level measures (delivery, reviews, payments) |
| OrderItems[ProductId] | Products[ProductId] | |
| OrderItems[SellerId] | Sellers[SellerId] | |
| Payments[OrderId] | Orders[OrderId] | |
| Reviews[OrderId] | Orders[OrderId] | |
| Customers[StateCode] | States[StateCode] | Sellers deliberately not related to States (would create ambiguous path) |

## Calculated columns (DAX)

| Column | Logic |
|---|---|
| Orders[DeliveryDays] | DATEDIFF purchase → delivered to customer, DAY |
| Orders[DelayDays] | DATEDIFF estimated → delivered, DAY (> 0 = late) |
| Orders[DeliveryStatus] | Včas / Pozdě / Nedoručeno (sort: DeliveryStatusOrder) |
| Orders[DelayBucket] | 7+ dní dříve … 8+ dní pozdě (sort: DelayBucketOrder) |
| Orders[PurchaseHour] | HOUR(PurchaseTimestamp) |
| OrderItems[ItemTotal] | Price + Freight |
| Reviews[Sentiment] | 4–5 Pozitivní, 3 Neutrální, 1–2 Negativní (sort: SentimentOrder) |
| Products[WeightClass] | weight buckets (sort: WeightClassOrder) |

## Measures (`_Measures`, by display folder)

- **Sales**: Revenue, Freight, GMV, Order Count, Items Sold, Average Order Value, Average Item Price, Freight Share
- **Customers**: Customer Count (distinct CustomerUniqueId), Repeat Customers, Repeat Customer Rate, Orders per Customer
- **Time Intelligence**: Revenue PY, Revenue YoY %, Revenue YTD, Revenue Running Total, Orders PY, Orders YoY %
- **Delivery**: Delivered Orders, Late Orders, On-Time Rate, Late Rate, Avg Delivery Days, Avg Delay of Late Orders
- **Reviews**: Review Count, Avg Review Score, Positive Review Rate, Negative Review Rate, Review Gap On-Time vs Late
- **Payments**: Payment Value, Avg Installments, Payment Share
- **Ranking**: Category Rank, Seller Rank
- **UI**: Selection Title (dynamic subtitle), Selected Category (drillthrough header), On-Time Color, Review Color (conditional formatting, hex)

Currency format: `R$ #,0` (BRL).

## Data notes
- Orders span 2016-09 … 2018-10; months before 2017-01 and after 2018-08 are nearly empty → use `Date[IsAnalysisPeriod] = TRUE` for trends.
- 610 products without category → "(Neuvedeno)"; 2 categories lack translation → prettified Portuguese name.
- Some orders have more than one review and some reviews cover more than one order (no unique key on Reviews).
- Customers: `customer_id` is per order; repeat buyers identified by `CustomerUniqueId` (repeat rate ≈ 3 %).
