---
description: >-
  If you're scraping a Dataset we don't yet know about, it needs to exist in
  this table first.
---

# Submit or update Datasets

## 1. Define the supporting info for your Dataset.

> Tip: Make each of these queries in a new tab so you can keep them handy. You can use the DoltHub SQL editor, or run it locally.

### Agency ID

![](../../../.gitbook/assets/screen-shot-2021-05-01-at-12.39.30-pm.png)

```sql
--find agencies by city name
SELECT * FROM `agencies` WHERE `city` like "%Juneau%"

--find agencies by state
SELECT * FROM `agencies` WHERE `state_iso` = "NJ"

--find agencies by county
SELECT * FROM `agencies`
JOIN counties ON agencies.county_fips=counties.fips
WHERE `counties.name` LIKE "%Bibb%"
```

### Source Type

```sql
SELECT * from `source_types`
```

### Data Type

```sql
SELECT * FROM `data_types`
```

### Format Type

```sql
SELECT * FROM `format_types`
```

### Can Scrape

Enter `0` if this dataset cannot legally be scraped.

## 2. Submit the Dataset

### Dolt SQL Editor

You can use SQL `INSERT` to add new datasets right in Dolt. It's possible to generate these Insert statements from spreadsheets or write them manually.

1. Make a copy of the [Dataset Submission Template](https://docs.google.com/spreadsheets/d/1qh-6pb6KoIFSQ9qyyzd_bZIOosD74Sg21VPjbOQ5j3g/edit#gid=494854000) and populate information about new datasets as you work.
2. Navigate to the Query table and note that each row generates a new SQL query.
3. Paste the queries individually into the [DoltHub Datasets repo](https://www.dolthub.com/repositories/pdap/datasets) and run them.
4. When you're done, make a Pull Request and ask in \#datasets for someone to approve it.

![](../../../.gitbook/assets/screen-shot-2021-05-02-at-12.10.13-am.png)

### CLI

1. [Install Dolt](https://docs.dolthub.com/getting-started/installation).
2. Initialize the project locally.

   ```text
   dolt init
   dolt clone pdap/datasets && cd datasets
   dolt table export datasets > datasets.csv
   ```

3. Open the `datasets.csv` file, make changes, and save. Leave `id` blank—UUIDs are generated automatically. Make sure you're not adding a URL that already exists.

   ```text
   dolt branch <branch name e.g. add-CA-counties>
   dolt checkout <your branch>
   dolt table import -u datasets datasets.csv
   ```

4. Run this command to add your csv to your checked out branch.

   ```text
   dolt add .
   ```

5. Push the commit.

   ```text
   dolt commit -m “<message e.g. added 5 Alameda County datasets>”
   dolt push --set-upstream origin <your branch>
   ```

6. Head to [https://www.dolthub.com/repositories/pdap/datasets/](https://www.dolthub.com/repositories/pdap/datasets/)
7. Create a [Pull Request](https://docs.dolthub.com/dolthub/getting-started#pull-requests) to merge your dataset into master. Your branch should appear as an option.
