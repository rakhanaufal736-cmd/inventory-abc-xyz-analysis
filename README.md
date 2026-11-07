# Inventory ABC-XYZ Analysis

An inventory case study that answers a practical question: **which SKUs deserve management attention, and what safety stock, reorder point and ordering policy fit each group?**

The project combines ABC-XYZ classification, EOQ, safety stock, reorder point and a policy simulation, delivered as an interactive web explorer and a fully formula-driven Excel workbook.

**[Live demo](https://rakhanaufal736-cmd.github.io/inventory-abc-xyz-analysis/)** | **[Download the Excel workbook](analisis_inventory_abc_xyz_v2.xlsx)**

> Replace `YOUR-USERNAME` with your GitHub username once GitHub Pages is enabled.

---

## Business Problem

A company managing roughly 200 SKUs needs to decide where to focus its inventory control effort. Treating every item the same wastes effort on low-value items and leaves high-value, volatile items exposed to stockouts.

## Approach

1. **Classify** each SKU on two dimensions:
   - **ABC** by cumulative annual sales value (A = first 80%, B = next 15%, C = remaining 5%).
   - **XYZ** by the coefficient of variation (CV) of monthly demand (X ≤ 0.25, Y ≤ 0.50, Z above 0.50).
2. **Size the policy** for every SKU: EOQ, safety stock, reorder point and max level.
3. **Simulate** three policies over 12 months and compare fill rate and total cost:
   - A. EOQ without safety stock
   - B. EOQ with safety stock
   - C. Min-Max
4. **Recommend** a policy and review cadence for each of the nine ABC-XYZ classes.

## Key Findings

Results under the default assumptions (ordering cost IDR 75,000 per order, holding cost 25% of purchase price per year, 95% service level):

| # | Finding |
|---|---------|
| 1 | 60 class A SKUs (30% of items) generate 80% of annual sales value, while 77 class C SKUs generate only 5%. |
| 2 | 49 class Z SKUs (unstable demand) account for 34% of sales value but 59% of safety stock value. |
| 3 | Estimated inventory investment (safety stock plus half of EOQ) is IDR 296.3 million, of which safety stock is IDR 84.9 million. Inventory turnover is about 7.8x per year. |
| 4 | For the AZ class, fill rate is 91.1% under policy A, 97.7% under B and 98.7% under C. Min-Max also has the lowest total cost for this class. |
| 5 | The biggest lever is the AZ class (20 SKUs): cutting its lead time by 50% reduces its safety stock by about 29% (IDR 12.8 million) without lowering service level. |

### Policy Matrix

|  | **X** (stable) | **Y** (moderate) | **Z** (volatile) |
|---|---|---|---|
| **A** (high value) | EOQ ordering, small safety stock, weekly review | Standard safety stock, weekly review, monitor forecast | Higher safety stock, shorten lead time, weekly forecast review |
| **B** (medium) | Min-Max, biweekly review | Min-Max with standard safety stock, biweekly review | Min-Max with moderate safety stock, quarterly re-evaluation |
| **C** (low) | Simple monthly periodic ordering | Monthly periodic ordering with small buffer | Order on actual demand, consider discontinuing slow movers |

## Methodology

```
EOQ           = sqrt(2 x D x S / H)
Safety stock  = z x daily demand std dev x sqrt(lead time in days)
ROP           = demand during lead time + safety stock
Max level     = ROP + EOQ
```

`D` is annual demand in units, `S` is ordering cost per order, `H` is holding cost per unit per year and `z` is derived from the target service level assuming normally distributed demand.

## Repository Contents

| File | Description |
|------|-------------|
| `index.html` | Interactive explorer. Adjust the sliders and every metric recalculates in the browser. No build step or dependencies. |
| `analisis_inventory_abc_xyz_v2.xlsx` | Workbook with 8 sheets: Ringkasan, Asumsi, Data_Penjualan, Analisis_SKU, Matriks, Simulasi_Kebijakan, Simulasi_Per_Kelas and Simulasi_Per_SKU. |

In the workbook, change the inputs in the `Asumsi` sheet (blue cells) and the classification, EOQ and reorder points update automatically. The per-class and per-SKU simulation sheets hold computed values and do not refresh when assumptions change.

## Run Locally

No installation is needed. Open `index.html` in any modern browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Deployment

The site is static and hosted on GitHub Pages. In the repository, go to **Settings > Pages**, choose the `main` branch and the root folder, and save.

## Data and Limitations

- The data is **simulated**: 200 SKUs over 12 months, derived from a real company's data masked with random multipliers (0.7 to 1.2) and replaced item names and prices.
- Ordering cost, holding cost and lead times are assumptions. On real data they should be replaced with the company's own figures.
- The simulation uses the same 12 months as the analysis (in-sample), so results are optimistic compared with true out-of-sample performance.
- Demand is assumed to be normally distributed, and stockouts are treated as lost sales with no backorders.
- Stockout cost counts only lost margin. This makes the no-safety-stock policy look cheap despite its lower fill rate. If stockouts cost more in practice (lost customers, penalties), policies with higher fill rate become more attractive.

## Skills Demonstrated

Inventory management, ABC-XYZ segmentation, EOQ and safety stock modeling, discrete simulation, sensitivity analysis, Excel modeling and data visualization.
