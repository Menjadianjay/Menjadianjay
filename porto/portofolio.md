Data engineering portfolio · Yogyakarta, Indonesia (UTC+7)

# Joseph De Ornay

I build data pipelines that stop when the data is wrong, and I write down why each rule exists.

Informatics graduate of Sanata Dharma University (July 2026), looking for a data engineering internship. Below are three projects, with the numbers behind each one.

[GitHub](https://github.com/Menjadianjay) [LinkedIn](https://www.linkedin.com/in/joseph-aprillio-7i70a0a) [Medium](https://medium.com/@josephAprillio) `josephdeornay@gmail.com`

[Retail ETL pipeline](#retail) [Javanese speech thesis](#speech) [Laundry system](#laundry) [Skills](#skills)

Data engineering · team project

## Retail Analytics Platform

Jul – Oct 2026 Role: Data Engineer 101 of 104 commits [GitHub repository](https://github.com/Retail-Analytical-Platform/Retail-Analytics-Data)

A PostgreSQL pipeline takes a flawed retail star schema of 63,170 rows through four layers and publishes only data that passes its checks.

### How the data moves

public CSV load

**Source tables.** One fact table with 60,000 sales and four dimensions, reset from the CSV files on each full run.

staging etl.extract()

**Snapshot copy.** No logic here, so the stage is a true copy of the source. The original sale categories are loaded beside it for audit.

transformed etl.transform()

**Fixes and gates.** Applies the rules, then raises an exception instead of emitting bad data.

clean etl.load()

**Published schema.** Primary keys, foreign keys and CHECK constraints, a reporting view, and column comments that define revenue.

Rule tables read by transform

- product_category_map
- quantity_rules
- pricing_rules
- decision_log

Each stage is a PL/pgSQL function. The whole run starts with one command: `docker compose --profile etl run --rm etl`. Database credentials are Docker secrets, never environment variables in git.

### What the source data got wrong, and what the pipeline did

| Problem | Found in source | After the pipeline |
| --- | --- | --- |
| Month names in the date table | ✕**667** of 730 dates wrong | ✓Rebuilt from the date itself. The run fails, and a CHECK constraint rejects any mismatch. |
| Product cost above list price | ✕**313** of 400 products (78%) | ✓0 loss-making products. Prices come from a rules table, and a CHECK constraint blocks negative margin. |
| Revenue that disagrees with quantity × price × (1 − discount) | ✕Differs on **99.9%** of sales | ✓0 mismatches after the pricing repair. The official formula is stored as a column comment in the database. |
| Duplicate customers | ✕**5** records share an email and name with another | ✓Merged into the record with the most orders, and the orders are re-pointed to it. Same email with a different name goes to a review table instead of being merged. |

Source counts were re-checked on the CSV files in the repository. The “after” results come from the pipeline’s own run notices and the repository documentation (docs/dataAnalytics/PricingIssueFix.md).

### Decisions I wrote down

- RejectedDeriving each sale’s category from its product. That would have erased Apparel and Grocery, so the original 42 / 26 / 20 / 12 percent category split is kept and enforced as a gate.
- AcceptedThe discount is a constant 35% on every row. There is nothing to clean, so it is documented as a flat-rate assumption.
- FixedProduct prices, because 78% of products had a cost above their list price. Prices now come from a rules table, whose first values were marked as placeholders (median source cost × 1.60) until real market prices existed.

### Replacing invented prices with market prices

The source prices had no relationship to cost or product, so I wrote a Python scraper on the eBay Browse API. The first version guessed brands from listing titles and priced only **32 of 400** products. The second version asks eBay for its own Brand filter on each product, then records the median new-condition price per brand. It produced **529 brand-level prices across 387 brands**. Listings must match at least 60% of the product-name words, and cases, covers and spare parts are filtered out.

### Known limits and next steps

- **92 of 400 products** still carry the source floor price of 5.00, because their product name ran out of distinct brands.
- The rules table still holds placeholders. The next step is to seed it from the scraped prices so a fresh run produces good prices without the manual repair.
- eBay prices are US-only medians of new items, so they should be read with the low and high bands that the scraper also records.

- PostgreSQL 16
- PL/pgSQL
- Python
- pandas
- SQLAlchemy
- Docker Compose
- Docker secrets
- eBay Browse API
- Git & pull requests

Research · data pipeline · bachelor’s thesis

## Javanese speech recognition: TDNN vs TDNN-F

Sep 2025 – Jun 2026 Sanata Dharma University Thesis written in Indonesian

Two acoustic models were trained on the same cleaned data and compared by character error rate. TDNN-F scored lower in all ten paired runs.

### From raw recordings to model input

1. 185,076**Recordings** in the OpenSLR Javanese corpus
2. 18,501**Stratified 10% sample**, drawn proportionally from each speaker group
3. 18,383**Clean transcripts** after normalization, lowercasing, punctuation and accent removal, and English-word filtering−118
4. 17,880**Files with MFCC features** (39 values per frame). The rest were too short or in the wrong format.−503
5. 17,858**Aligned** to words and phonemes with the Montreal Forced Aligner, which gave 17.51 hours of active speech−22

before`Ing sawetara kÃ©wan gulu bisa tuwuh dawa upamanÃ© gulu jerapah`

after`ing sawetara kewan gulu bisa tuwuh dawa upamane gulu jerapah`

The sample is a transcript from the thesis’s preprocessing table. In the raw text, accented letters appear as garbled characters, and cleaning reduces them to plain letters. The cleaned data was split 80:20 into training and test sets.

### Results

TDNN-F has the lower error rate in every one of the 10 paired runs

TDNN TDNN-F

RunCharacter error rate (%)Gap

1

best 33.80

−12.09

2

−6.37

3

−10.06

4

−4.21

5

−10.28

6

−11.43

7

−6.11

8

−4.02

9

−3.53

10

best 39.50

−2.59

30 35 40 45

Character error rate on held-out test data, in percent. Lower is better. Each run is one random-search tuning configuration per model; the gap is in percentage points.

Best run

33.80% vs 39.50%TDNN-F vs TDNN

Average of 10 runs

35.99% vs 43.06%TDNN-F also varied less: 3.88 points from best to worst run, against 6.43 for TDNN.

Paired t-test

p = 0.000149t = 6.255 with 9 degrees of freedom. Both sets of results passed a Shapiro-Wilk normality check first.

Show the numbers as a table

| Run | TDNN % | TDNN-F % | Gap |
| --- | --- | --- | --- |
| 1 | 45.89 | 33.80 | 12.09 |
| 2 | 43.97 | 37.60 | 6.37 |
| 3 | 45.26 | 35.20 | 10.06 |
| 4 | 40.21 | 36.00 | 4.21 |
| 5 | 45.78 | 35.50 | 10.28 |
| 6 | 45.93 | 34.50 | 11.43 |
| 7 | 42.01 | 35.90 | 6.11 |
| 8 | 40.82 | 36.80 | 4.02 |
| 9 | 41.21 | 37.68 | 3.53 |
| 10 | 39.50 | 36.91 | 2.59 |

### What the data showed

- **51.6% of the 20,127 unique words appear only once** in the corpus (10,391 words), so most of the vocabulary has too few examples to learn from.
- **Javanese-specific sounds are rare.** /ch/, /dh/, /ny/ and /th/ each make up under 1% of phonemes. The thesis names this imbalance as a likely contributor to the high error rate.
- **The corpus is small.** 17.51 hours of speech, against roughly 740 to 755 hours in the Korean and Mandarin studies the thesis compares with, and no pre-trained Javanese model for the aligner.
- **Training was expensive.** One run took 303 to 2,544 minutes on a single NVIDIA RTX A4500, which is why each model got 10 random-search runs instead of a full grid.

- Python
- MFCC
- Montreal Forced Aligner
- LDA
- TDNN / TDNN-F
- N-gram language model
- Random search
- Paired t-test

Team web application · five members

## Podomoro Laundry Management System

Sep – Dec 2024 Laravel 11 · PHP 8.2 · MySQL / SQLite · Tailwind CSS · Alpine.js [Repository](https://github.com/Menjadianjay/Podomoro-Laundry)

A laundry business ran on paper notes. Our team replaced them with one web application for taking and managing orders, and added an attendance system for the manager.

- **Orders:** each order records the customer, service, weight and payment method, and the total price is calculated from the service rate, so taking and managing orders is faster and more efficient than on paper.
- **Attendance:** the manager had a hard time tracking employee attendance, so I built a staff attendance system. It allows one record per person per day, only during working hours (08:00–19:00, Asia/Jakarta time), with status, notes and an optional proof file (JPG, PNG or PDF, up to 2 MB) that managers can review, edit and delete.
- **Panels:** staff and managers log in to separate panels. The manager dashboard shows total income, transaction count and number of services, and the service catalog holds each service's type, rate and turnaround time.
- **Interface:** paginated, responsive lists with the newest records first, a redesigned login page, and interface text translated into English.
- **My part:** I wrote 38 of the repository's 51 commits (about 75%), merged through branches and pull requests, and contributed to the architecture and UI design. The data lives in five relational tables: users, customers, services, transactions and attendance.

Skills

## What I work with

### Data work

- ETL pipelines
- Data cleaning and validation
- Data quality gates
- Star schema modeling
- Feature extraction
- Statistical testing

### Languages and libraries

- Python
- pandas
- SQLAlchemy
- SQL
- PL/pgSQL
- Java
- JavaScript
- PHP

### Databases and tooling

- PostgreSQL
- MySQL
- SQLite
- Docker
- Docker Compose
- Git & GitHub
- Laravel

### Learning next

- AWS
- PySpark
- Airflow

I have also worked as a teaching assistant for Machine Learning (Mar – Jul 2026) and Object-Oriented Programming (Sep – Dec 2025) at Sanata Dharma University, which is where I practiced explaining technical work clearly.

Contact

Email **josephdeornay@gmail.com**, or find me on [GitHub](https://github.com/Menjadianjay) and [LinkedIn](https://www.linkedin.com/in/joseph-aprillio-7i70a0a).

Figures come from the project repository and its documentation, and from the thesis. Source-data counts for the retail project were re-checked on the repository’s CSV files.
