# SQL Murder Mystery 🕵

The SQL Murder Mystery is an interactive problem designed to test and improve your SQL querying skills. In this project, you play the role of a detective solving a murder by exploring a database filled with information about the crime, suspects, and evidence. The objective is to use SQL queries to find the murderer and the mastermind behind the crime.

Note: This project is based on the SQL Murder Mystery available on [SQL Murder Mystery]https://mystery.knightlab.com/

Disclaimer: This project was completed using the SQL Murder Mystery website and does not include a local database copy.

---

## Skills Tested ✅

* **Data Exploration**: Understand database structure and relationships between tables.
* **SQL Querying**: Apply joins, subqueries, aggregation, filtering, and `LIKE` operations.
* **Critical Thinking**: Use logic to interpret clues and solve the mystery.

---

## Investigation 🔍

### Step 1: Crime Scene Details

```sql
-- Get details of the murder case
SELECT * 
FROM crime_scene_report 
WHERE date = 20180115 
  AND city = 'SQL City' 
  AND type = 'murder';
```

| Date       | Type   | Description                                                                                                                                         | City     |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| 2018-01-15 | Murder | Security footage shows that there were 2 witnesses. First lives at last house on "Northwestern Dr". Second, named Annabel, lives on "Franklin Ave". | SQL City |

---

### Step 2: Identify Witnesses

#### Witness 1 (Northwestern Dr)

```sql
SELECT * 
FROM person
WHERE address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;
```

| id    | name           | license_id | address_number | address_street_name | ssn       |
| ----- | -------------- | ---------- | -------------- | ------------------- | --------- |
| 14887 | Morty Schapiro | 118009     | 4919           | Northwestern Dr     | 111564949 |

#### Witness 2 (Annabel, Franklin Ave)

```sql
SELECT * 
FROM person
WHERE name LIKE 'Annabel%' 
  AND address_street_name = 'Franklin Ave';
```

| id    | name           | license_id | address_number | address_street_name | ssn       |
| ----- | -------------- | ---------- | -------------- | ------------------- | --------- |
| 16371 | Annabel Miller | 490173     | 103            | Franklin Ave        | 318771143 |

---

### Step 3: Get Witness Interviews

```sql
SELECT * 
FROM interview
WHERE person_id IN (14887, 16371);
```

| person_id | transcript                                                                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 14887     | I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. Membership number started with "48Z". Only gold members have these bags. The man got into a car with a plate containing "H42W". |
| 16371     | I saw the murder happen and recognized the killer from my gym when working out last week on Jan 9th.                                                                                                          |

---

### Step 4: Find the Murderer

```sql
SELECT g.*, d.*
FROM get_fit_now_member AS g
JOIN person AS p ON g.person_id = p.id
JOIN drivers_license AS d ON p.license_id = d.id
WHERE g.membership_status = 'gold'
  AND g.id LIKE '48Z%'
  AND d.plate_number LIKE '%H42W%';
```

| id    | person_id | name          | membership_start_date | membership_status | id     | age | height | eye_color | hair_color | gender | plate_number | car_make  | car_model |
| ----- | --------- | ------------- | --------------------- | ----------------- | ------ | --- | ------ | --------- | ---------- | ------ | ------------ | --------- | --------- |
| 48Z55 | 67318     | Jeremy Bowers | 20160101              | gold              | 423327 | 30  | 70     | brown     | brown      | male   | 0H42W2       | Chevrolet | Spark LS  |

---

### Step 5: Interview of Murderer

```sql
SELECT * 
FROM interview
WHERE person_id = 67318;
```

| person_id | transcript                                                                                                                                                                                        |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 67318     | I was hired by a woman with a lot of money. I don’t know her name but she is 5'5"–5'7" (65–67"), has red hair, drives a Tesla Model S, and attended the SQL Symphony Concert 3 times in Dec 2017. |

---

### Step 6: Find the Mastermind

```sql
SELECT p.name, COUNT(*) AS concert_count, i.annual_income
FROM drivers_license AS d
JOIN person AS p ON d.id = p.license_id
JOIN facebook_event_checkin AS f ON p.id = f.person_id
JOIN income AS i ON p.ssn = i.ssn
WHERE f.event_name = 'SQL Symphony Concert'
  AND f.date BETWEEN 20171201 AND 20171231
GROUP BY p.id, p.name, i.annual_income
ORDER BY concert_count DESC
LIMIT 1;
```

| name             | concert_count | annual_income |
| ---------------- | ------------- | ------------- |
| Miranda Priestly | 3             | 310000        |

On Submitting this name to INSERT statement, we get the result as -

| value |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
|Congrats, you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne!|

🎉 Congratulations! 🎉 You've successfully solved the SQL Murder Mystery and identified the murderer!
---

## Summary of Investigation 🕵

1. **Crime Scene**: Murder on 2018-01-15 in SQL City. Witnesses identified: Morty Schapiro and Annabel Miller.
2. **Murderer**: Jeremy Bowers, identified via gym membership and vehicle details.
3. **Mastermind**: Miranda Priestly, identified by cross-referencing physical description, vehicle, and concert attendance.

---

## Key Takeaways

* Used **joins** to connect related tables (`person`, `drivers_license`, `get_fit_now_member`, `facebook_event_checkin`).
* Applied **filters** with `LIKE`, ranges, and aggregation (`COUNT`) to narrow down suspects.
* Combined **data analysis and logical reasoning** to solve the mystery.

---

## Signing Off

Detective: Yash Mavare
Date: Oct 17, 2025

