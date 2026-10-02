# Home Depot Scraper - Price & Inventory by ZIP

## Current source limitation (2 October 2026)

Home Depot can return HTTP 206 with a service-error envelope instead of stores. The repaired runtime was verified on 2 October 2026 in owner run `wNFgEfOn9quZqfYZ1` (build `0.45.61`): product `206577650`, ZIP `95050`, three actual California stores, three useful rows and no failures. Santa Clara, Campbell and Kifer Rd reported 16, 11 and 32 units respectively at $114. The ZIP-specific browser context HTTP route recovered from the direct client's service errors. The documentation revision keeps that runtime unchanged. This bounded observation does not guarantee source availability across other products, ZIPs or future runs. Earlier candidates failed; one resolved an unrelated default store and delivered no inventory. That path was removed. Source failures are not evidence of zero stock. Start with a small matrix and inspect `OUTPUT.status`, `isFinal`, `pendingZipCodes` and dataset reason codes before expanding or scheduling.

The repair validates the requested product ID, preserves unknown prices and stock flags as `null`, and keeps incremental `OUTPUT` checkpoints for delivered rows. A failed dataset write is not counted as a delivered result. An interrupted run preserves its observed rows and unfinished ZIPs. If a store lookup returns a service error, the current anonymous browser context's HTTP client may query the ZIP-specific store service. Product requests always use the resolved store ID and requested ZIP. Null-store product responses are not used to select stores for a ZIP: they can expose a default store in another state. The repair does not invent store IDs or bypass an access challenge. Source order does not establish nearest-store ranking.

## Start with a three-store pickup check

[Compare Home Depot pickup stock across three stores](https://apify.com/kamerozkan/home-depot-bulk-inventory-by-zip/examples/check-home-depot-stock-across-zip-codes)

Use this capped starter to compare one known product near ZIP 95050. Duplicate it, replace the product or ZIP, and inspect three store rows before increasing volume. It uses the current input fields and caps the matrix at three rows.

```json
{
  "products": [
    "206577650"
  ],
  "zipCodes": [
    "95050"
  ],
  "storesPerZip": 3,
  "maxMatrixRows": 3,
  "requestTimeoutSecs": 45,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": [
      "RESIDENTIAL"
    ],
    "apifyProxyCountry": "US"
  }
}
```

Read the dataset for store name, local price, digital stock, pickup and curbside flags, and checkedAt. Read the OUTPUT record for successfulRows, failedRows, stockRange and bestStockStore. A missing source field is not zero stock. These are point-in-time digital observations; they do not reserve stock or prove physical shelf availability.

At the FREE price tier, three successful rows plus one Actor start cost $0.014 in event charges: 3 x $0.003 + $0.005. The saved starter uses 1024 MB and a 300-second timeout. Failed rows have no inventory-result charge; a start event can still apply. Check the live pricing tab and set a maximum total charge before expanding the matrix.

[Ask for help or report an issue](https://console.apify.com/actors/tIDN1NdAFp95JAQiv/info/issues): include the public product ID, ZIP, run ID and reason code so the result can be investigated. After using the Actor, [share an honest rating or review](https://console.apify.com/actors/tIDN1NdAFp95JAQiv/info/reviews). Positive and critical feedback are both welcome.

Home Depot product scraper, stock checker, and price lookup by ZIP code: enter Home Depot Internet IDs, product URLs, or Store SKUs, add US ZIP codes, and get one structured row for every product x nearby-store check - store-local price, exact inventory count when Home Depot exposes it, pickup and curbside availability, bulk and clearance pricing - plus a decision-ready summary of the lowest-price and best-stock store.

Local price and inventory can differ between stores, but a difference is not guaranteed for every product or location. The Actor reports the live values it finds, whether they differ or match, and never converts a missing value into zero.

This Actor is an independent data automation tool and is not affiliated with, endorsed by, or sponsored by Home Depot.

## What you can build with it

### 🧺 1. A basket check: which store has every item ready for pickup?

Enter a multi-product basket and one or more ZIP codes. The `OUTPUT` summary lists `storesWithAllItems`: checked stores that report pickup availability for every item at the observation time. This digital signal does not reserve the basket or guarantee physical stock:

```json
{
  "products": ["206577650", "sku:1001619357", "https://www.homedepot.com/p/319360335"],
  "zipCodes": ["10001"],
  "storesPerZip": 3
}
```

### 🗺️ 2. A regional price and stock matrix

One product across several markets with one nearby store per ZIP: compare store-local prices, bulk-price tiers (the historical row below shows a $114 list price with a $102.60 bulk tier at quantity 2), clearance flags, and stock counts. Ideal for resellers, arbitrage teams, and pricing analysts.

```json
{
  "products": ["206577650"],
  "zipCodes": ["10001", "60601", "75201"],
  "storesPerZip": 1
}
```

### 🏬 3. Local availability before the trip

Check pickup, curbside pickup, `maxPickupQuantity`, and `limitedQuantity` for the exact product at the exact stores near a customer, before dispatching a driver or promising a delivery date.

### 📅 4. Scheduled inventory watch with a result cap

Save the input as a Task and schedule it. `maxMatrixRows` caps inventory-result rows and their event charges per run. Failed rows have no inventory-result charge; a start event can still apply. Set a maximum total charge for the run. This limits event charges; it is not a cap on the developer's platform usage or proxy costs. Export the dataset to CSV or Excel, or let a Make, Zapier, or n8n workflow read it after each run.

## Pricing and cost examples

FREE-tier event prices last checked on 2 October 2026:

| Event | Price | Notes |
|---|---:|---|
| Inventory result | $0.003 per successful product-store row | Failed, blocked and NO_STORE_DATA rows have no result charge |
| Actor start | $0.005 once per run after a valid Home Depot session | This is one event, not a per-GB price |

Event-charge examples at this tier:

- Starter: 3 successful rows plus one start = $0.014.
- Basket check: 120 successful rows plus one start = $0.365.
- Regional watch: 50 successful rows plus one start = $0.155 per run, or $4.65 for 30 such runs.
- Large matrix: 500 successful rows plus one start = $1.505.

Plan discounts and future pricing can differ. The live pricing tab is authoritative. These examples do not assume a full set of successful rows when a source returns partial data.

## Related Actors

- [Walmart Product Scraper - Price & Stock by ZIP](https://apify.com/kamerozkan/walmart-multi-zip-monitor) - the same store-verified, multi-ZIP approach for Walmart, with change detection across runs
- [Google Ads Transparency Scraper - Creative Monitor](https://apify.com/kamerozkan/google-ads-verified-change-monitor) - competitor ad changes for the brands you price-track
- [Lieferando Menu Scraper - Restaurant Menus, Prices & Delivery](https://apify.com/kamerozkan/german-delivery-menu-price-intelligence) - menu and delivery price intelligence for the German market

## Try it first

The default **Try for free** input uses one direct Home Depot Internet ID, one
ZIP code, and three nearby stores. This keeps the first matrix to three store
checks while still making the comparison useful:

```json
{
  "products": ["206577650"],
  "zipCodes": ["95050"],
  "storesPerZip": 3
}
```

## Compare three ZIP codes

To compare one product across three US markets, use one nearby store per ZIP:

```json
{
  "products": [
    "206577650"
  ],
  "zipCodes": [
    "10001",
    "60601",
    "75201"
  ],
  "storesPerZip": 1
}
```

This exact input ran live on 2026-07-28. Manhattan reported 2 units and Chicago 5, both at the same price, and the Dallas store timed out and was delivered as an uncharged FAILED row instead of disappearing:

```json
[
  {
    "status": "SUCCESS",
    "zipCodeRequested": "10001",
    "storeName": "Manhattan West 23rd St",
    "price": 114,
    "bulkPrice": 102.6,
    "bulkQuantityRequired": 2,
    "stockQuantity": 2,
    "inStock": true,
    "pickupAvailable": true,
    "curbsidePickupAvailable": false,
    "distanceMiles": null
  },
  {
    "status": "SUCCESS",
    "zipCodeRequested": "60601",
    "storeName": "South Loop",
    "price": 114,
    "bulkPrice": 102.6,
    "bulkQuantityRequired": 2,
    "stockQuantity": 5,
    "inStock": true,
    "pickupAvailable": true,
    "curbsidePickupAvailable": false,
    "distanceMiles": null
  },
  {
    "status": "FAILED",
    "zipCodeRequested": "75201",
    "storeName": "Lemmon Avenue",
    "reasonCode": "UNEXPECTED_ERROR",
    "errorMessage": "page.goto: Timeout 20000ms exceeded. (call log shortened here)"
  }
]
```

The run's `OUTPUT` record answered the comparison honestly: no price difference to act on, a real stock difference of 3 units:

<details>
<summary>Show the full JSON example</summary>

```json
{
  "priceRange": {
    "minimum": 114,
    "maximum": 114,
    "difference": 0,
    "hasDifference": false
  },
  "stockRange": {
    "minimum": 2,
    "maximum": 5,
    "difference": 3,
    "hasDifference": true
  },
  "lowestPriceStore": {
    "zipCodeRequested": "60601",
    "storeId": "1950",
    "storeName": "South Loop",
    "storeAddress": "1300 S Clinton Street, Chicago, IL, 60607",
    "distanceMiles": null,
    "productId": "206577650",
    "productName": "Prep Series 1/2 HP Continuous Feed Garbage Disposal with Power Cord and Universal Mount",
    "price": 114,
    "stockQuantity": 5,
    "inStock": true,
    "pickupAvailable": true,
    "curbsidePickupAvailable": false
  },
  "bestStockStore": {
    "zipCodeRequested": "60601",
    "storeId": "1950",
    "storeName": "South Loop",
    "storeAddress": "1300 S Clinton Street, Chicago, IL, 60607",
    "distanceMiles": null,
    "productId": "206577650",
    "productName": "Prep Series 1/2 HP Continuous Feed Garbage Disposal with Power Cord and Universal Mount",
    "price": 114,
    "stockQuantity": 5,
    "inStock": true,
    "pickupAvailable": true,
    "curbsidePickupAvailable": false
  }
}
```

</details>

## What it helps answer

- Which checked store reports the lowest local price for each product?
- Which checked store reports the highest available stock?
- Are pickup and curbside pickup available at a specific location?
- Which stores have every item in a multi-product basket ready for pickup?
- Did Home Depot expose clearance, limited-quantity, or bulk-pricing details?

## Input

The Actor accepts Home Depot Internet IDs, product URLs, and Store SKUs.
Bare 10-digit Store SKUs beginning with `100` and legacy 6-7 digit Store SKUs
are detected automatically. Prefix any Store SKU with `sku:` when you want to
make the identifier type explicit.

The maximum matrix size is controlled by `maxMatrixRows`. A request with 20
products, 2 ZIP codes, and 3 stores per ZIP can produce 120 rows.

## Output

The default dataset starts with the comparison fields customers usually need:
ZIP, store, stock, price, pickup, curbside pickup, distance, and product. It
then includes identifiers, raw metadata, and machine-readable error details.

Re-verified on 2026-09-02 (run `rZmxfWmkMwktjn3i6`): the same product at Manhattan West 23rd St showed `stockQuantity: 1` and at Chicago South Loop `stockQuantity: 7`, both at $114 list with the $102.60 bulk tier - a live example of stock differing by store while price does not. This is a complete, unedited row from a live run on 2026-07-28 (note the bulk price tier Home Depot publishes for this product):

```json
{
  "status": "SUCCESS",
  "productInput": "206577650",
  "productId": "206577650",
  "storeSkuNumber": "1001619357",
  "productName": "Prep Series 1/2 HP Continuous Feed Garbage Disposal with Power Cord and Universal Mount",
  "modelNumber": "GXP50C",
  "productUrl": "https://www.homedepot.com/p/MOEN-Prep-Series-1-2-HP-Continuous-Feed-Garbage-Disposal-with-Power-Cord-and-Universal-Mount-GXP50C/206577650",
  "zipCodeRequested": "10001",
  "storeId": "6175",
  "storeName": "Manhattan West 23rd St",
  "storeNameRaw": "Manhattan West 23rd St",
  "storeAddress": "40 West 23rd Street, New York, NY, 10010",
  "distanceMiles": null,
  "price": 114,
  "originalPrice": null,
  "clearancePrice": null,
  "clearancePercentageOff": null,
  "bulkPrice": 102.6,
  "bulkQuantityRequired": 2,
  "stockQuantity": 2,
  "inStock": true,
  "limitedQuantity": false,
  "pickupAvailable": true,
  "curbsidePickupAvailable": false,
  "maxPickupQuantity": null,
  "availabilityType": "Shared",
  "reasonCode": null,
  "errorMessage": null,
  "checkedAt": "2026-07-28T21:09:05.889Z"
}
```

The `OUTPUT` key-value-store record contains:

- `priceRange` and `stockRange` for a single-product run
- `lowestPriceStore` and `bestStockStore` for a single-product run
- `productComparisons` with the same decision fields calculated separately for
  every product in a multi-product run
- `storesWithAllItems` for pickup-ready basket availability
- totals, identifier mappings, resolved stores, and failure counts
- `isFinal`, `runInterrupted`, `completedZipCodes` (processed ZIPs), `pendingZipCodes` and `activeZipCode` for progress and interruption handling
- `storeCoverage` for requested versus resolved store counts; fewer stores than requested makes a useful result `PARTIAL`
- `storeDiscovery` for the observed discovery route and scope; the ZIP-specific lookup route does not establish complete geographic coverage

Top-level price and stock decisions are `null` for multi-product runs because
prices from different products should not be compared with each other. Use
`productComparisons` in that case.

## Billing

When pay-per-event pricing is enabled, the Actor uses two transparent events:

- `actor-start`: $0.005 once per run after a valid Home Depot session, at the FREE tier checked on 30 September 2026
- `inventory-result`: $0.003 per successful matrix row at that tier

Failed rows have no inventory-result charge; a start event can still apply. The Actor respects the caller's maximum charge
limit and stops producing paid rows when that limit is reached.


This Actor is also exposed to AI agents through Apify's MCP server
(mcp.apify.com): an agent can discover it by search and run it with the same
pay-per-event billing, with no separate integration.

## Reliability and data semantics

Inventory is live storefront data and can change after the run. A `null` value
means Home Depot did not expose that field; it is never converted to zero.
`FAILED` rows include a machine-readable `reasonCode` and `errorMessage`.

Unknown basket prices remain `totalPrice: null` with `priceComplete: false`. Explicit zero stock remains a useful source observation. A price-only or stock-only observation may be useful while other fields remain unknown. Inspect the actual fields required by your workflow.

A real failure row from a live run, structured and uncharged:

```json
{
  "status": "FAILED",
  "productInput": "319360335",
  "productId": "319360335",
  "zipCodeRequested": "95050",
  "reasonCode": "NO_STORE_DATA",
  "errorMessage": "Home Depot returned no fulfillment data for product 319360335 at store 0630 (call log shortened here)",
  "checkedAt": "2026-07-28T14:59:56.433Z"
}
```

A US residential proxy is strongly recommended and is enabled by default.
Apify compute and proxy usage depend on the selected pricing configuration.

## Legal

This Actor is an independent data automation tool and is not affiliated with,
endorsed by, or sponsored by Home Depot. Use it responsibly and comply with
applicable laws and website terms.
