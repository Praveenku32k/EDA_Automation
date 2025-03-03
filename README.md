# EDA_Automation
EDA_Automation
```
WITH year_filter AS (
    SELECT subject_id, anchor_year
    FROM mimic_iv_subset.patients
    WHERE anchor_year_group = '2011 - 2013'  -- Adjust based on required real-world timeframe
)

-- Extracting one year's data from all tables

-- 1. OMR Table
SELECT * FROM mimic_iv_subset.omr
JOIN year_filter ON omr.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM omr.chartdate) = year_filter.anchor_year;

-- 2. Admissions Table
SELECT * FROM mimic_iv_subset.admissions
JOIN year_filter ON admissions.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM admissions.admittime) = year_filter.anchor_year;

-- 3. Diagnoses_ICD Table
SELECT * FROM mimic_iv_subset.diagnoses_icd
JOIN year_filter ON diagnoses_icd.subject_id = year_filter.subject_id
JOIN mimic_iv_subset.admissions ON diagnoses_icd.hadm_id = admissions.hadm_id
WHERE EXTRACT(YEAR FROM admissions.admittime) = year_filter.anchor_year;

-- 4. DRGCodes Table
SELECT * FROM mimic_iv_subset.drgcodes
JOIN year_filter ON drgcodes.subject_id = year_filter.subject_id
JOIN mimic_iv_subset.admissions ON drgcodes.hadm_id = admissions.hadm_id
WHERE EXTRACT(YEAR FROM admissions.admittime) = year_filter.anchor_year;

-- 5. HPCSEvents Table
SELECT * FROM mimic_iv_subset.hpcsevents
JOIN year_filter ON hpcsevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM hpcsevents.chartdate) = year_filter.anchor_year;

-- 6. Labevents Table
SELECT * FROM mimic_iv_subset.labevents
JOIN year_filter ON labevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM labevents.charttime) = year_filter.anchor_year;

-- 7. MicrobiologyEvents Table
SELECT * FROM mimic_iv_subset.microbiologyevents
JOIN year_filter ON microbiologyevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM microbiologyevents.chartdate) = year_filter.anchor_year;

-- 8. Pharmacy Table
SELECT * FROM mimic_iv_subset.pharmacy
JOIN year_filter ON pharmacy.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM pharmacy.starttime) = year_filter.anchor_year;

-- 9. POE Table
SELECT * FROM mimic_iv_subset.poe
JOIN year_filter ON poe.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM poe.ordertime) = year_filter.anchor_year;

-- 10. Prescriptions Table
SELECT * FROM mimic_iv_subset.prescriptions
JOIN year_filter ON prescriptions.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM prescriptions.starttime) = year_filter.anchor_year;

-- 11. Procedures_ICD Table
SELECT * FROM mimic_iv_subset.procedures_icd
JOIN year_filter ON procedures_icd.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM procedures_icd.chartdate) = year_filter.anchor_year;

-- 12. Services Table
SELECT * FROM mimic_iv_subset.services
JOIN year_filter ON services.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM services.transfertime) = year_filter.anchor_year;

-- 13. Transfers Table
SELECT * FROM mimic_iv_subset.transfers
JOIN year_filter ON transfers.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM transfers.intime) = year_filter.anchor_year;

-- 14 d_icd_diagnoses table:
WITH year_filter AS (
    SELECT subject_id, anchor_year
    FROM mimic_iv_subset.patients
    WHERE anchor_year_group = '2011 - 2013'
)

SELECT DISTINCT d_icd_diagnoses.*
FROM mimic_iv_subset.d_icd_diagnoses
JOIN mimic_iv_subset.diagnoses_icd ON d_icd_diagnoses.icd_code = diagnoses_icd.icd_code
JOIN mimic_iv_subset.admissions ON diagnoses_icd.hadm_id = admissions.hadm_id
JOIN year_filter ON admissions.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM admissions.admittime) = year_filter.anchor_year;

--15.d_icd_procedures

WITH year_filter AS (
    SELECT subject_id, anchor_year
    FROM mimic_iv_subset.patients
    WHERE anchor_year_group = '2011 - 2013'
)

SELECT DISTINCT d_icd_procedures.*
FROM mimic_iv_subset.d_icd_procedures
JOIN mimic_iv_subset.procedures_icd ON d_icd_procedures.icd_code = procedures_icd.icd_code
JOIN mimic_iv_subset.admissions ON procedures_icd.hadm_id = admissions.hadm_id
JOIN year_filter ON admissions.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM procedures_icd.chartdate) = year_filter.anchor_year;


```
