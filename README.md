# consumer-economics-bigquery
## Data
Source: `bigquery-public-data.thelook_ecommerce` (synthetic e-commerce data, so findings illustrate method, not real consumer behaviour). Data queried on: [date]. Analysis window: [start date] to [cut-off date].

## Assumptions
- Revenue counts order items with status: [your choice, e.g. Complete and Shipped; excludes Cancelled and Returned]
- All values are in USD.
- A "customer" is a distinct `user_id` with at least one counted order.
- No ad spend is available, so CAC and ROAS are not calculated.

## Metric definitions

| Metric | Definition | Source tables |
|--------|------------|---------------|
| Revenue | Sum of `sale_price` for counted order items | order_items |
| Orders | Count of distinct `order_id` | order_items |
| AOV (average order value) | Revenue / orders | order_items |
| Items per order | Counted items / orders | order_items |
| Gross margin | `sale_price` minus product `cost` | order_items, products |
| Gross margin % | Gross margin / revenue | order_items, products |
| Return rate | Returned items / [your denominator, e.g. shipped items] | order_items |
| Cancellation rate | Cancelled items / all items | order_items |
| Session conversion rate | Sessions with a purchase event / all sessions | events |
| Funnel step conversion | Sessions reaching a step / sessions at the previous step (product, cart, purchase) | events |
| Cart abandonment rate | 1 minus (sessions with a purchase / sessions with a cart event) | events |
| Repeat purchase rate | Customers with 2+ orders / customers with 1+ orders | order_items |
| N-day retention (cohort) | Share of a first-order-month cohort who order again within N days of their first order | order_items |
| LTV proxy | Cumulative gross margin per customer, by months since first order and cohort | order_items, products |
| RFM segments | Recency (days since last order), frequency (orders), monetary (revenue), scored into groups | order_items |
| Revenue concentration | Share of revenue from the top 10% of customers | order_items |

## Funnel notes

- Funnel steps are product view, cart and purchase, counted on distinct sessions (1 Jan 2020 to 31 Oct 2026: 608,009 sessions, 390,907 with a cart event, 172,376 with a purchase). Almost every session has a product view, so the funnel has no meaningful top-of-funnel step.
- 71.6% of sessions have no `user_id` (435,275 sessions), and none of these record a purchase. Sessions are either fully anonymous or fully identified. Among identified sessions, 99.8% end in a purchase. Session conversion therefore largely reflects the share of identified sessions, which is a property of the synthetic data, so funnel results are treated as descriptive and not as benchmarks.
- 9 sessions record a purchase without a cart event and are excluded from the ordered funnel.
