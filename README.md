> **Live API:** [Run Home Depot Bulk Inventory by ZIP on Apify](https://apify.com/kamerozkan/home-depot-bulk-inventory-by-zip)

# Home Depot Bulk Inventory by ZIP: Samples and JSON Schema

[![Apify Actor](https://img.shields.io/badge/Apify-Run%20Actor-00c7b7?logo=apify)](https://apify.com/kamerozkan/home-depot-bulk-inventory-by-zip)
![JSON Schema](https://img.shields.io/badge/schema-JSON%20Schema%202020--12-4c1)
![Samples](https://img.shields.io/badge/samples-3%20live%20rows-2f855a)
![License](https://img.shields.io/badge/license-MIT-blue)

Check known Home Depot product identifiers against a bounded set of US ZIP codes and nearby stores. Each dataset row records a point-in-time store context, price, digital inventory count when exposed, pickup signals, bulk pricing, or an explicit failure.

This repository contains three input examples, three privacy-minimized live output rows, and the row contract in [`dataset_record.schema.json`](dataset_record.schema.json).

## Start here

1. Open the [Actor on Apify](https://apify.com/kamerozkan/home-depot-bulk-inventory-by-zip).
2. Use input 02 or 03 for the current schema.
3. Start with a small product, ZIP, and store matrix.
4. Treat every result as a point-in-time digital storefront observation.

At the 2026-07-28 audit, the Actor was public and its latest build `0.45.46` had completed successfully. The Store exposed one public Example Task. Its newest inspected successful run used build `0.45.42` and produced the three live rows below. See [`DATA_NOTICE.md`](DATA_NOTICE.md) for the exact provenance and version boundary.

## Input examples

<details>
<summary><strong>01. Public Store Example Task snapshot</strong> - exact legacy input</summary>

[`01_public_store_example_legacy_input.json`](01_public_store_example_legacy_input.json)

```json
{
  "productInputs": [
    "206577650"
  ],
  "zipCodes": [
    "95050"
  ],
  "storesPerZip": 3,
  "maxRows": 10,
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

This is the exact owner-console snapshot of the sole public Example Task input. It uses legacy field names and is preserved as provenance. For a new configuration, use the current-schema fields shown in input 02 or 03.

</details>

<details>
<summary><strong>02. Compare one product across three ZIP codes</strong> - current-schema recipe</summary>

[`02_three_zip_comparison_recipe_input.json`](02_three_zip_comparison_recipe_input.json)

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
  "storesPerZip": 1,
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

This runnable recipe follows the current input schema. It is not a public Example Task and was not run during this repository audit.

</details>

<details>
<summary><strong>03. Check a two-product basket across two ZIP codes</strong> - current-schema recipe</summary>

[`03_two_product_basket_recipe_input.json`](03_two_product_basket_recipe_input.json)

```json
{
  "products": [
    "206577650",
    "205909852"
  ],
  "zipCodes": [
    "95050",
    "10001"
  ],
  "storesPerZip": 3,
  "maxMatrixRows": 12,
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

This runnable recipe follows the current input schema. It is not a public Example Task and was not run during this repository audit.

</details>

## Live output examples

All three rows below came from the same successful public Example Task run. The requested ZIP was `95050`; the Actor resolved three nearby store contexts. Historical inventory and price values are not current guarantees.

<details>
<summary><strong>01. Campbell store observation</strong> - 9 digitally reported units</summary>

[`01_live_campbell_inventory_output.json`](01_live_campbell_inventory_output.json)

```json
{
  "status": "SUCCESS",
  "productInput": "206577650",
  "productId": "206577650",
  "storeSkuNumber": "1001619357",
  "productName": "Prep Series 1/2 HP Continuous Feed Garbage Disposal with Power Cord and Universal Mount",
  "modelNumber": "GXP50C",
  "productUrl": "https://www.homedepot.com/p/MOEN-Prep-Series-1-2-HP-Continuous-Feed-Garbage-Disposal-with-Power-Cord-and-Universal-Mount-GXP50C/206577650",
  "zipCodeRequested": "95050",
  "storeId": "0642",
  "storeName": "Campbell",
  "storeNameRaw": "Campbell",
  "storeAddress": "480 E Hamilton Ave, Campbell, CA, 95008",
  "distanceMiles": 3.968629320997109,
  "price": 114,
  "originalPrice": null,
  "clearancePrice": null,
  "clearancePercentageOff": null,
  "bulkPrice": 102.6,
  "bulkQuantityRequired": 2,
  "stockQuantity": 9,
  "inStock": true,
  "limitedQuantity": false,
  "pickupAvailable": true,
  "curbsidePickupAvailable": true,
  "maxPickupQuantity": null,
  "availabilityType": "Shared",
  "reasonCode": null,
  "errorMessage": null,
  "checkedAt": "2026-07-28T14:35:37.586Z"
}
```

</details>

<details>
<summary><strong>02. Kifer Rd store observation</strong> - 23 digitally reported units</summary>

[`02_live_kifer_rd_inventory_output.json`](02_live_kifer_rd_inventory_output.json)

```json
{
  "status": "SUCCESS",
  "productInput": "206577650",
  "productId": "206577650",
  "storeSkuNumber": "1001619357",
  "productName": "Prep Series 1/2 HP Continuous Feed Garbage Disposal with Power Cord and Universal Mount",
  "modelNumber": "GXP50C",
  "productUrl": "https://www.homedepot.com/p/MOEN-Prep-Series-1-2-HP-Continuous-Feed-Garbage-Disposal-with-Power-Cord-and-Universal-Mount-GXP50C/206577650",
  "zipCodeRequested": "95050",
  "storeId": "0640",
  "storeName": "Kifer Rd",
  "storeNameRaw": "Kifer Rd",
  "storeAddress": "680 Kifer Rd, Sunnyvale, CA, 94086",
  "distanceMiles": 4.09905467505703,
  "price": 114,
  "originalPrice": null,
  "clearancePrice": null,
  "clearancePercentageOff": null,
  "bulkPrice": 102.6,
  "bulkQuantityRequired": 2,
  "stockQuantity": 23,
  "inStock": true,
  "limitedQuantity": false,
  "pickupAvailable": true,
  "curbsidePickupAvailable": true,
  "maxPickupQuantity": null,
  "availabilityType": "Shared",
  "reasonCode": null,
  "errorMessage": null,
  "checkedAt": "2026-07-28T14:35:41.387Z"
}
```

</details>

<details>
<summary><strong>03. Santa Clara store observation</strong> - 14 digitally reported units</summary>

[`03_live_santa_clara_inventory_output.json`](03_live_santa_clara_inventory_output.json)

```json
{
  "status": "SUCCESS",
  "productInput": "206577650",
  "productId": "206577650",
  "storeSkuNumber": "1001619357",
  "productName": "Prep Series 1/2 HP Continuous Feed Garbage Disposal with Power Cord and Universal Mount",
  "modelNumber": "GXP50C",
  "productUrl": "https://www.homedepot.com/p/MOEN-Prep-Series-1-2-HP-Continuous-Feed-Garbage-Disposal-with-Power-Cord-and-Universal-Mount-GXP50C/206577650",
  "zipCodeRequested": "95050",
  "storeId": "0630",
  "storeName": "Santa Clara",
  "storeNameRaw": "Santa Clara",
  "storeAddress": "2435 Lafayette St, Santa Clara, CA, 95050",
  "distanceMiles": 1.0824222734465827,
  "price": 114,
  "originalPrice": null,
  "clearancePrice": null,
  "clearancePercentageOff": null,
  "bulkPrice": 102.6,
  "bulkQuantityRequired": 2,
  "stockQuantity": 14,
  "inStock": true,
  "limitedQuantity": false,
  "pickupAvailable": true,
  "curbsidePickupAvailable": true,
  "maxPickupQuantity": null,
  "availabilityType": "Shared",
  "reasonCode": null,
  "errorMessage": null,
  "checkedAt": "2026-07-28T14:35:43.782Z"
}
```

</details>

## Data contract

Validate a dataset row with any JSON Schema 2020-12 implementation:

```javascript
import Ajv2020 from "ajv/dist/2020.js";
import addFormats from "ajv-formats";
import schema from "./dataset_record.schema.json" with { type: "json" };
import row from "./01_live_campbell_inventory_output.json" with { type: "json" };

const ajv = new Ajv2020({ allErrors: true });
addFormats(ajv);
if (!ajv.validate(schema, row)) throw new Error(ajv.errorsText());
```

Important semantics:

- `stockQuantity = null` means no count was confirmed. It does not mean zero.
- `SUCCESS` describes one checked product and store context at `checkedAt`.
- `FAILED` rows preserve a `reasonCode` and diagnostic instead of inventing stock or price.
- Pickup and curbside fields are digital storefront signals, not reservations.
- Multi-product runs should be compared per product. Do not compare prices across different products.

## API example

```bash
curl -X POST \
  "https://api.apify.com/v2/acts/kamerozkan~home-depot-bulk-inventory-by-zip/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d @02_three_zip_comparison_recipe_input.json
```

Set a maximum run charge in Apify before increasing the matrix. Pricing and platform usage can change.

## Scope and legal boundary

This is an independent, unofficial data automation sample. It is not affiliated with, endorsed by, or sponsored by Home Depot.

Home Depot storefront values can change after a run. No uptime, freshness, inventory, price, accuracy, completeness, or physical shelf guarantee is provided. Review applicable law, website terms, database rights, and downstream use requirements before operating at scale.

## License

Repository code, examples, and schema are available under the [MIT License](LICENSE). Third-party product names and data remain subject to their respective rights and terms.
