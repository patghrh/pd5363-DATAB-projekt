## Wyniki zapytań SQL

### Zapytanie 2: Testy, w których uczestniczyło więcej niż 30 pacjentów oraz liczbę pacjentów, która je wykonała

```sql
SELECT
    t.test_type,
    COUNT(DISTINCT pt.patient_id) AS patient_count
FROM tests t
JOIN patient_test pt ON t.test_id = pt.test_id
GROUP BY t.test_type
HAVING COUNT(DISTINCT pt.patient_id) > 30;
```

| test\_type | patient\_count | 
| --- | --- | 
| NGS-panel | 38 |
| SNP Array | 39 |

### Zapytanie 3: Nazwy testów i obliczona średnia liczby wariantów dla każdego testu

```sql
SELECT
    t.test_type,
    AVG(v.variant_count) AS average_variant_count
FROM tests t
JOIN (
    SELECT
        test_id,
        COUNT(*) AS variant_count
    FROM results
    GROUP BY test_id) v ON t.test_id = v.test_id
GROUP BY t.test_type;
```

| test\_type | average\_variant\_count | 
| --- | --- | 
| SNP Array | 3.0000 |
| NGS-panel | 3.6857 |
| WES | 3.5294 |
| WGS | 3.6071 |
