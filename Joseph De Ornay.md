<h1 align="center">Joseph De Ornay</h1>

<p align="center">
  <b>I build data pipelines that stop when the data is wrong, and I write down why each rule exists.</b><br>
  Informatics graduate, Sanata Dharma University (July 2026) · Yogyakarta, Indonesia (UTC+7)<br>
  Looking for a data engineering internship
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/joseph-aprillio-7i70a0a"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-Joseph%20De%20Ornay-0A66C2?logo=linkedin&logoColor=white"></a>
  <a href="https://medium.com/@josephAprillio"><img alt="Medium" src="https://img.shields.io/badge/Medium-Articles-000000?logo=medium&logoColor=white"></a>
  <a href="mailto:josephdeornay@gmail.com"><img alt="Email" src="https://img.shields.io/badge/Email-josephdeornay%40gmail.com-EA4335?logo=gmail&logoColor=white"></a>
</p>

---

## About me

I like turning messy data into data people can trust. In my projects I check the source first, count what is wrong, fix it with documented rules, and add checks that fail loudly instead of passing bad data along.

- **Data engineering:** PostgreSQL ETL pipelines with staging layers and quality gates
- **Machine learning:** a Javanese speech recognition thesis, evaluated with statistical tests
- **Software:** a Laravel web application built with a team
- **Teaching:** Teaching Assistant for Machine Learning and Object-Oriented Programming
- **Learning next:** AWS, PySpark, Airflow

## Projects

| Project | What it is | Stack |
|---|---|---|
| [**Retail Analytics Platform**](#retail-analytics-platform) | A four-layer PostgreSQL ETL pipeline that publishes only data that passes its checks | PostgreSQL 16, PL/pgSQL, Python, Docker Compose |
| [**Javanese speech recognition**](#javanese-speech-recognition-tdnn-vs-tdnn-f) | Bachelor's thesis comparing two acoustic models, TDNN and TDNN-F | Python, MFCC, Montreal Forced Aligner, N-gram LM |
| [**Podomoro Laundry**](#podomoro-laundry-management-system) | A web application that replaced paper notes at a laundry business | Laravel 11, PHP 8.2, MySQL/SQLite, Tailwind CSS |

---

## Retail Analytics Platform

*Jul – Oct 2026 · Role: Data Engineer · 101 of 104 commits · [Repository](https://github.com/Retail-Analytical-Platform/Retail-Analytics-Data)*

A PostgreSQL pipeline takes a flawed retail star schema (63,170 rows: 60,000 sales and four dimension tables) through four layers and publishes only data that passes its checks.

**Pipeline:** `public` (source tables from CSV) → `staging` (etl.extract(), plain copy) → `transformed` (etl.transform(), fixes and quality gates) → `clean` (etl.load(), PK, FK and CHECK constraints). Rule tables (product_category_map, quantity_rules, pricing_rules, decision_log) feed the transform step.

Each stage is a PL/pgSQL function. The whole run starts with one command, and database credentials are Docker secrets, not environment variables in git:

```bash
docker compose --profile etl run --rm etl
```

### What the source data got wrong

| Problem | Found in source | After the pipeline |
|---|---|---|
| Month names in the date table | **667 of 730** dates wrong | Rebuilt from the date itself; a CHECK constraint rejects any mismatch |
| Product cost above list price | **313 of 400** products (78%) | 0 loss-making products; prices come from a rules table and a CHECK constraint blocks negative margin |
| Revenue vs quantity × price × (1 − discount) | Differs on **99.9%** of sales | 0 mismatches after the pricing repair; the formula is stored as a column comment |
| Duplicate customers | **5** records share an email and name | Merged into the record with the most orders; same email with a different name goes to a review table |

> Source counts were re-checked on the repository's CSV files. The "after" results come from the pipeline's run notices and the repository documentation.

### Decisions I wrote down

- **Rejected:** deriving each sale's category from its product, because that would erase Apparel and Grocery. The original 42 / 26 / 20 / 12 percent split is kept and enforced as a gate.
- **Accepted:** the discount is a constant 35% on every row, so there is nothing to clean. It is documented as a flat-rate assumption.
- **Fixed:** product prices, because 78% of products had a cost above list price. The rules table started with placeholder values until real market prices existed.

### Replacing invented prices with market prices

I wrote a Python scraper on the eBay Browse API. The first version guessed brands from listing titles and priced only **32 of 400** products. The second asks eBay for its own Brand filter on each product and records the median new-condition price per brand: **529 brand-level prices across 387 brands**. Listings must match at least 60% of the product-name words, and cases, covers and spare parts are filtered out.

### Known limits and next steps

- 92 of 400 products still carry the source floor price of 5.00.
- The rules table still holds placeholders. Next step: seed it from the scraped prices so a fresh run produces good prices without a manual repair.
- eBay prices are US-only medians of new items, so they should be read with the low and high bands the scraper also records.

---

## Javanese speech recognition: TDNN vs TDNN-F

*Sep 2025 – Jun 2026 · Bachelor's thesis, Sanata Dharma University (written in Indonesian)*

Two acoustic models were trained on the same cleaned data and compared by character error rate (CER). **TDNN-F scored lower in all ten paired runs.**

### From raw recordings to model input

| Step | Files | Dropped |
|---|---:|---:|
| OpenSLR Javanese corpus | 185,076 | |
| Stratified 10% sample | 18,501 | |
| Clean transcripts (normalization, accent removal, English-word filter) | 18,383 | 118 |
| MFCC features extracted (39 values per frame) | 17,880 | 503 |
| Aligned with the Montreal Forced Aligner (17.51 hours of speech) | 17,858 | 22 |

The cleaned data was split 80:20 into training and test sets with a fixed random seed.

### Results

| | TDNN-F | TDNN |
|---|---:|---:|
| Best run (CER) | **33.80%** | 39.50% |
| Average of 10 runs | **35.99%** | 43.06% |
| Spread, best to worst run | 3.88 points | 6.43 points |

A paired t-test gives **p = 0.000149** (t = 6.255, 9 degrees of freedom), after a Shapiro-Wilk normality check on both sets of results.

<details>
<summary>All ten runs</summary>

| Run | TDNN % | TDNN-F % | Gap (points) |
|---:|---:|---:|---:|
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

Each run is one random-search tuning configuration per model. Lower CER is better.

</details>

### What the data showed

- **51.6% of the 20,127 unique words appear only once**, so most of the vocabulary has too few examples to learn from.
- **Javanese-specific sounds are rare.** /ch/, /dh/, /ny/ and /th/ each make up under 1% of phonemes, which the thesis names as a likely contributor to the high error rate.
- **The corpus is small:** 17.51 hours, against roughly 740 to 755 hours in the Korean and Mandarin studies it compares with.
- **Training was expensive.** One run took 303 to 2,544 minutes on a single NVIDIA RTX A4500, which is why each model got 10 random-search runs instead of a full grid.

---

## Podomoro Laundry Management System

*Sep – Dec 2024 · Team of five · [Repository](https://github.com/Menjadianjay/Podomoro-Laundry)*

A laundry business ran on paper notes. Our team replaced them with one Laravel web application for taking and managing orders, and added an attendance system for the manager.

- **Orders:** each order records the customer, service, weight and payment method, and the total price is calculated from the service rate.
- **Attendance:** I built the staff attendance system after the manager struggled to track attendance. It allows one record per person per day, only between 08:00 and 19:00 (Asia/Jakarta time), with status, notes and an optional proof file (JPG, PNG or PDF, up to 2 MB) that managers can review, edit and delete.
- **Panels:** staff and managers log in to separate panels. The manager dashboard shows total income, transaction count and number of services, and the service catalog holds each service's type, rate and turnaround time.
- **Interface:** paginated, responsive lists with the newest records first, a redesigned login page, and interface text translated into English.
- **My part:** 38 of the repository's 51 commits (about 75%), merged through branches and pull requests, plus architecture and UI design. The data lives in five tables: users, customers, services, transactions and attendance.

`Laravel 11` `PHP 8.2` `MySQL / SQLite` `Blade` `Tailwind CSS` `Alpine.js` `Vite` `Git`

---

## Skills

| Area | What I use |
|---|---|
| **Data engineering** | ETL pipeline design, staging layers, data cleaning and validation, quality gates, star schema modeling, PostgreSQL 16 (PL/pgSQL, CTEs, window functions), MySQL, SQLite |
| **Machine learning** | MFCC features, forced alignment, LDA, TDNN / TDNN-F, N-gram language models, random-search tuning, Shapiro-Wilk and paired t-tests |
| **Languages** | Python (pandas, SQLAlchemy, psycopg2, NumPy), SQL, PL/pgSQL, Bash, Java, JavaScript, PHP |
| **Linux and tools** | Ubuntu, Arch Linux; shell navigation, file permissions, process management, user administration, log troubleshooting; Docker, Docker Compose, Kubernetes, systemd, Git and GitHub |
| **Web** | Laravel 11, Blade, Eloquent ORM, Tailwind CSS, Alpine.js, HTML5, CSS3 |

## Experience

- **Teaching Assistant, Machine Learning** · Sanata Dharma University · Mar – Jul 2026
- **Teaching Assistant, Object-Oriented Programming** · Sanata Dharma University · Sep – Dec 2025

## Contact

Email [josephdeornay@gmail.com](mailto:josephdeornay@gmail.com), or find me on [LinkedIn](https://www.linkedin.com/in/joseph-aprillio-7i70a0a) and [Medium](https://medium.com/@josephAprillio).

<sub>Figures come from the project repositories, their documentation and the thesis.</sub>
