# Data Notice

## Purpose

This repository is a technical sample for the [Home Depot Bulk Inventory by ZIP Actor](https://apify.com/kamerozkan/home-depot-bulk-inventory-by-zip). It demonstrates one exact public Example Task snapshot, two current-schema recipes, and three privacy-minimized live dataset rows.

It is not a continuous real-time feed, a nationwide inventory dataset, a physical shelf count, a reservation, or a price guarantee.

## July 2026 audit snapshot

The following state was verified through the public Apify API, public Store page, and authenticated owner console on 2026-07-28:

| Item | Verified value |
|---|---|
| Actor | `kamerozkan/home-depot-bulk-inventory-by-zip` |
| Actor ID | `tIDN1NdAFp95JAQiv` |
| Public | `true` |
| Current latest build | `0.45.46`, build ID `noqvnLk8lg9geR9iw`, status `SUCCEEDED` |
| Saved tasks | 10 total, 1 public |
| Public Example Task | `FTah87WeK1E6sodev`, slug `check-home-depot-stock-across-zip-codes`, 8 runs |
| Latest successful public-task run inspected | `NVxiOqH6VtdamV4rn`, build `0.45.42` |
| Inspected dataset | `oeu1jXpJSQiInAWfr`, 3 records |

The current `0.45.46` build finished after the inspected public-task run. This repository does not claim runtime validation of `0.45.46`.

## Input provenance

- [`01_public_store_example_legacy_input.json`](01_public_store_example_legacy_input.json) is an exact owner-console snapshot of the sole public Example Task input. Its successful inspected run used build `0.45.42`.
- That task snapshot uses the legacy names `productInputs` and `maxRows`.
- The current Store input schema uses `products` and `maxMatrixRows`. Use input 02 or 03 as the current-schema pattern for a new configuration.
- [`02_three_zip_comparison_recipe_input.json`](02_three_zip_comparison_recipe_input.json) and [`03_two_product_basket_recipe_input.json`](03_two_product_basket_recipe_input.json) are schema-valid recipes prepared for this repository. They are not additional public Example Tasks and were not run as part of this audit.

## Output provenance

All three output files are complete dataset rows from successful public-task run `NVxiOqH6VtdamV4rn`:

- [`01_live_campbell_inventory_output.json`](01_live_campbell_inventory_output.json)
- [`02_live_kifer_rd_inventory_output.json`](02_live_kifer_rd_inventory_output.json)
- [`03_live_santa_clara_inventory_output.json`](03_live_santa_clara_inventory_output.json)

The run checked Home Depot Internet ID `206577650` for requested ZIP `95050`. The three rows reported three nearby store contexts at different observation times. They are historical evidence of the row contract, not current availability.

## Privacy minimization

The files preserve public product facts, public store names, public business addresses, prices, stock signals, fulfillment signals, and observation timestamps because they are necessary to explain the dataset.

The repository excludes:

- account and customer data
- cookies and session material
- API tokens
- proxy credentials and proxy session identifiers
- IP addresses and user-agent strings
- request headers, signatures, and raw request logs
- raw upstream payloads

The run ID and dataset ID document provenance. They do not grant access to private owner data.

## Interpretation limits

- `stockQuantity`, price, pickup, and curbside pickup are point-in-time digital storefront observations.
- A digital count is not a physical shelf guarantee or reservation.
- A value can change after `checkedAt`.
- A `null` value means the source did not expose or confirm that field. It must not be converted to zero or false.
- `SUCCESS` applies only to the product, store context, and observation time represented by that row.
- A `FAILED` row must remain a failure. It must not be interpreted as out of stock.
- ZIP resolution selects nearby stores. It does not establish complete geographic coverage.
- Repeated runs are polling, not continuous real-time monitoring.
- Website changes, throttling, timeouts, and access defenses can create partial or failed results.
- This independent Actor is not affiliated with, endorsed by, or sponsored by Home Depot.
- No uptime, freshness, accuracy, completeness, inventory, or price SLA is provided by this repository.

Users are responsible for reviewing applicable law, platform terms, database rights, privacy rules, retention rules, and downstream use requirements.

## September 2026 repair evidence

Additional samples were collected by authenticated owner-started cloud tests on 2026-09-20. Their inputs contain only public product identifiers and US ZIP codes. The 04 output is one row from run `fntUOrdgJ4atjG1yC`; the 05 output is the three-row dataset from run `pNtXCQkTGPDWw1Xo3`; the 06 output is the four-row dataset from run `GoGYzf2p9ou8cxDoE`. All three used build `0.45.52`.

The mixed-availability case contains two `NO_STORE_DATA` failures for product `205909852`. These are retained as failures, with null inventory fields. They are not converted into successful rows or zero-stock claims. Failure rows are not billed as inventory-result events.

The startup defect was separately reproduced in run `aeJI3haRZaxfkw3SF` on build `0.45.50`. Removing only the maintenance block was insufficient: run `nalMPS80R1Yr1tkO3` on build `0.45.51` reported platform success despite zero successful data rows. Its OUTPUT status was FAILED. The subsequent repair corrects that mismatch.

Public 30-day run statistics exclude runs started by the Actor owner and include historical failures. Owner smoke tests do not reset or improve that public metric. This repository does not claim that the historical failure percentage is repaired retroactively or that all product-store combinations yield data.

### Released build

Build `0.45.53` (`hYpld3g8jp4HpzYUV`) was promoted to `latest` after run `niagodKIHIztJFpNC` produced three successful rows with no failed rows. The 07 output file contains that dataset. The default timeout was changed from 120 to 300 seconds; memory stayed at 1024 MB. A fresh Actor API read verified both settings after the update.

The final test used direct GraphQL store resolution. The browser GraphQL fallback was added to the release but was not exercised by this final successful run. Eight local regression checks passed, including startup syntax and failure exit semantics. The public row schema is unchanged; the added samples preserve the same contract.

## Listing update on September 30, 2026

The Store title, description and search metadata were checked against the owned Actor and synchronized with this repository. This documentation update does not alter executable code, input or output schemas, recorded test outputs, artifact hashes, billing or runtime builds. Existing examples retain their original dates and validation limits. A public listing is not evidence of successful output, network acceptance or an achieved search ranking.
