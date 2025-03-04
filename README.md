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


1. caregiver Table
SELECT DISTINCT caregiver.*
FROM mimic_iv_subset.caregiver
JOIN mimic_iv_subset.chartevents ON caregiver.caregiver_id = chartevents.caregiver_id;

2. d_items Table:
SELECT DISTINCT d_items.*
FROM mimic_iv_subset.d_items
JOIN mimic_iv_subset.chartevents ON d_items.itemid = chartevents.itemid;

3. chartevents Table
WITH year_filter AS (
    SELECT subject_id, anchor_year
    FROM mimic_iv_subset.patients
    WHERE anchor_year_group = '2011 - 2013'
)

SELECT * 
FROM mimic_iv_subset.chartevents
JOIN year_filter ON chartevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM chartevents.charttime) = year_filter.anchor_year;

4. datetimeevents Table
SELECT * 
FROM mimic_iv_subset.datetimeevents
JOIN year_filter ON datetimeevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM datetimeevents.charttime) = year_filter.anchor_year;

5. icustays Table

SELECT * 
FROM mimic_iv_subset.icustays
JOIN year_filter ON icustays.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM icustays.intime) = year_filter.anchor_year;

6. ingredientevents Table
SELECT * 
FROM mimic_iv_subset.ingredientevents
JOIN year_filter ON ingredientevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM ingredientevents.starttime) = year_filter.anchor_year;

7. inputevents Table
SELECT * 
FROM mimic_iv_subset.inputevents
JOIN year_filter ON inputevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM inputevents.starttime) = year_filter.anchor_year;

8. outputevents Table
SELECT * 
FROM mimic_iv_subset.outputevents
JOIN year_filter ON outputevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM outputevents.charttime) = year_filter.anchor_year;

9. procedureevents Table
SELECT * 
FROM mimic_iv_subset.procedureevents
JOIN year_filter ON procedureevents.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM procedureevents.starttime) = year_filter.anchor_year;



```


```
omr table:
subject_id,chartdate,seq_num,result_name,result_value

provider table:
provider_id

admissions table
subject_id, hadm_id, admittime, dischtime, deathtime,admission_type,admit_provider_id,admission_location, discharge_location,insurance, language, marital_status, ethnicity,edregtime, edouttime,hospital_expire_flag


d_hcpcs table:
code,category,long_description, short_description

d_icd_diagnoses table:
icd_code, icd_version,long_title

d_icd_procedures table:
icd_code, icd_version,long_title

d_labitems table:
itemid,label,fluid,category

diagnoses_icd table:
subject_id,hadm_id,seq_num,icd_code, icd_version

drgcodes table:
subject_id,hadm_id,drg_type,drg_code,description,drg_severity, drg_mortality

emar table:
subject_id,hadm_id,emar_id, emar_seq,poe_id,pharmacy_id,enter_provider_id,charttime,medication,event_txt,scheduletime,storetime

emar_detail table:
subject_id, emar_id, emar_seq, parent_field_ordinal, administration_type, pharmacy_id, barcode_type, reason_for_no_barcode, complete_dose_not_given, dose_due, dose_due_unit, dose_given, dose_given_unit, will_remainder_of_dose_be_given, product_amount_given, product_unit, product_code, product_description, product_description_other, prior_infusion_rate, infusion_rate, infusion_rate_adjustment, infusion_rate_adjustment_amount, infusion_rate_unit, route, infusion_complete, completion_interval, new_iv_bag_hung, continued_infusion_in_other_location, restart_interval, side, site, non_formulary_visual_verification

hpcsevents table:
subject_id,hadm_id,chartdate,hcpcs_cd,seq_num,short_description


labevents tables:
labevent_id,subject_id,hadm_id,specimen_id,itemid,order_provider_id,charttime,storetime,value, valuenum,valueuom,ref_range_lower, ref_range_upper,flag
priority,comments

microbiologyevents table:
microevent_id,subject_id,hadm_id,micro_specimen_id,order_provider_id,chartdate, charttime,spec_itemid, spec_type_desc,test_seq,storedate, storetime
test_itemid, test_name,org_itemid, org_name,isolate_num,ab_itemid, ab_name,dilution_text, dilution_comparison, dilution_value,interpretation,comments


patients table:
subject_id,gender,anchor_age, anchor_year, anchor_year_group,dod

pharmacy table:
subject_id,hadm_id,pharmacy_id,poe_id,starttime, stoptime,medication,proc_type

poe table:
poe_id,poe_seq,subject_id,hadm_id,ordertime,order_type,order_subtype

poe_detail table:
poe_id,poe_seq,subject_id,field_name,field_value


prescriptions table:
subject_id,hadm_id,pharmacy_id,poe_id, poe_seq,order_provider_id,starttime, stoptime,drug_type

procedures_icd table:
subject_id,hadm_id,seq_num,chartdate,icd_code, icd_version

services tables:
subject_id,hadm_id,transfertime,prev_service, curr_service

transfers table
subject_id, hadm_id, transfer_id,eventtype,careunit,intime, outtime


caregiver table:
caregiver_id

d_items table:
itemid,label, abbreviation,linksto,category,unitname,param_type,lownormalvalue, highnormalvalue

chartevents table:
subject_id, hadm_id, stay_id,caregiver_id,charttime, storetime,itemid,value, valuenum,valueuom,warning

datetimeevents table:
subject_id, hadm_id, stay_id,caregiver_id,charttime, storetime,itemid,value,valueuom,warning

ICU stays:
subject_id, hadm_id, stay_id,FIRST_CAREUNIT, LAST_CAREUNIT,INTIME, OUTTIME,LOS

Ingredientevents table:

subject_id, hadm_id, stay_id,caregiver_id,starttime, endtime,storetime,itemid,amount, amountuom,rate, rateuom,orderid, linkorderid,statusdescription
originalamount,originalrate

Inputevents table:
subject_id, hadm_id, stay_id,caregiver_id,starttime, endtime,storetime,itemid,amount, amountuom,rate, rateuom,orderid, linkorderid,ordercategoryname, secondaryordercategoryname, ordercomponenttypedescription, ordercategorydescription,patientweight,totalamount, totalamountuom
isopenbag,continueinnextdept,statusdescription,originalamount,originalrate

outputevents table:
subject_id, hadm_id, stay_id,caregiver_id,charttime,storetime,itemid,value, valueuom

procedureevents table:
subject_id, hadm_id, stay_id,caregiver_id,starttime, endtime,storetime,itemid,value,valueuom,location , locationcategory
orderid, linkorderid,ordercategoryname, ordercategorydescription,patientweight,isopenbag,continueinnextdept,statusdescription,originalamount, originalrate


```

```
import psycopg2

# Database connection parameters
SOURCE_DB = {
    "dbname": "source_database",
    "user": "source_user",
    "password": "source_password",
    "host": "source_host",
    "port": "5432"
}

TARGET_DB = {
    "dbname": "target_database",
    "user": "target_user",
    "password": "target_password",
    "host": "target_host",
    "port": "5432"
}

# List of tables and their corresponding date columns
TABLES = {
    "omr": "chartdate",
    "admissions": "admittime",
    "diagnoses_icd": "admittime",
    "drgcodes": "admittime",
    "hpcsevents": "chartdate",
    "labevents": "charttime",
    "microbiologyevents": "chartdate",
    "pharmacy": "starttime",
    "poe": "ordertime",
    "prescriptions": "starttime",
    "procedures_icd": "chartdate",
    "services": "transfertime",
    "transfers": "intime",
    "chartevents": "charttime",
    "datetimeevents": "charttime",
    "icustays": "intime",
    "ingredientevents": "starttime",
    "inputevents": "starttime",
    "outputevents": "charttime",
    "procedureevents": "starttime"
}

# SQL template for filtering and inserting data
SQL_TEMPLATE = """
WITH year_filter AS (
    SELECT subject_id, anchor_year
    FROM mimic_iv_subset.patients
    WHERE anchor_year_group = '2011 - 2013'
)
CREATE TEMP TABLE temp_filtered_data AS
SELECT {table_name}.*
FROM mimic_iv_subset.{table_name}
JOIN year_filter ON {table_name}.subject_id = year_filter.subject_id
WHERE EXTRACT(YEAR FROM {table_name}.{date_column}) = year_filter.anchor_year;

CREATE TABLE IF NOT EXISTS mimic_iv_filtered.{table_name} (LIKE mimic_iv_subset.{table_name} INCLUDING ALL);

INSERT INTO mimic_iv_filtered.{table_name}
SELECT * FROM temp_filtered_data;

DROP TABLE temp_filtered_data;
"""

def execute_query(query, conn):
    """Executes a given SQL query."""
    try:
        with conn.cursor() as cursor:
            cursor.execute(query)
        conn.commit()
        print(f"✅ Successfully processed table: {query.split()[8]}")
    except Exception as e:
        print(f"❌ Error processing table: {query.split()[8]} - {e}")
        conn.rollback()

def transfer_data():
    """Transfers filtered data from source to target database."""
    try:
        # Connect to both source and target databases
        src_conn = psycopg2.connect(**SOURCE_DB)
        tgt_conn = psycopg2.connect(**TARGET_DB)
        print("🚀 Connected to databases!")

        for table, date_column in TABLES.items():
            query = SQL_TEMPLATE.format(table_name=table, date_column=date_column)
            execute_query(query, src_conn)  # Execute query in source database

        print("✅ Data successfully transferred!")
    
    except Exception as e:
        print(f"❌ Error connecting to databases: {e}")
    
    finally:
        if src_conn:
            src_conn.close()
        if tgt_conn:
            tgt_conn.close()
        print("🔌 Disconnected from databases!")

if __name__ == "__main__":
    transfer_data()
```
