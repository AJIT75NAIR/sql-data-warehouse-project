SELECT
    sls_ord_num,
    sls_prd_key,
    sls_cust_id,
    sls_order_dt,
    sls_ship_dt,
    sls_due_dt,
    sls_Sales,
    sls_quantity,
    sls_price
FROM bronze.crm_sales_details
WHERE sls_cust_id NOT IN (SELECT cst_id
FROM silver.crm_cust_info)

--CHECK FOR INVALID DATES

SELECT
    sls_order_dt
FROM bronze.crm_sales_details
WHERE sls_order_dt <= 0

SELECT
    NULLIF(sls_order_dt,0) AS sls_order_dt
FROM bronze.crm_sales_details
WHERE sls_order_dt <=0 OR LEN(sls_order_dt) != 8 OR sls_order_dt > 20500101
    OR sls_order_dt < 19900101

SELECT
    sls_ord_num,
    sls_prd_key,
    sls_cust_id,
    CASE WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
    END AS sls_order_dt,
    CASE WHEN sls_ship_dt = 0 OR LEN(sls_ship_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_ship_dt AS VARCHAR) AS DATE)
    END AS sls_ship_dt,
    CASE WHEN sls_due_dt = 0 OR LEN(sls_due_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_due_dt AS VARCHAR) AS DATE)
    END AS sls_due_dt,
    sls_sales,
    sls_quantity,
    sls_price
FROM bronze.crm_sales_details
WHERE sls_cust_id NOT IN (SELECT cst_id
FROM silver.crm_cust_info)

--CHECK FOR INVALID DATE ORDERS

USE DataWarehouse

SELECT
    *
FROM bronze.crm_sales_details
WHERE sls_order_dt > sls_ship_dt OR sls_order_dt > sls_due_dt

SELECT
    sls_ord_num,
    sls_prd_key,
    sls_cust_id,
    CASE WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
    END AS sls_order_dt,
    CASE WHEN sls_ship_dt = 0 OR LEN(sls_ship_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_ship_dt AS VARCHAR) AS DATE)
    END AS sls_ship_dt,
    CASE WHEN sls_due_dt = 0 OR LEN(sls_due_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_due_dt AS VARCHAR) AS DATE)
    END AS sls_due_dt,
    sls_sales,
    sls_quantity,
    sls_price
FROM bronze.crm_sales_details

SELECT DISTINCT
    CASE WHEN sls_sales IS NULL OR sls_sales <= 0 OR sls_sales != sls_quantity
* ABS(sls_price)
THEN sls_quantity * ABS(sls_price) ELSE sls_sales 
END AS sls_sales,
    sls_quantity,
    CASE WHEN sls_price IS NULL OR sls_price <= 0 
THEN sls_sales / NULLIF(sls_quantity, 0) ELSE sls_price
END AS sls_price
FROM bronze.crm_sales_details
WHERE sls_sales != sls_quantity * sls_price
    OR sls_sales IS NULL OR sls_quantity IS NULL OR sls_price IS NULL
    OR sls_sales <= 0 OR sls_quantity <= 0 OR sls_price <= 0
ORDER BY sls_sales, sls_quantity, sls_price

IF OBJECT_ID('silver.crm_sales_details', 'U') IS NOT NULL
DROP TABLE silver.crm_sales_details;
CREATE TABLE silver.crm_sales_details
(
    sls_ord_num NVARCHAR(50),
    sls_prd_key NVARCHAR(50),
    sls_cust_id INT,
    sls_order_dt DATE,
    sls_ship_dt DATE,
    sls_due_dt DATE,
    sls_sales INT,
    sls_quantity INT,
    sls_price INT,
    dwh_create_date DATETIME2 DEFAULT GETDATE()
);

INSERT INTO silver.crm_sales_details
    (
    sls_ord_num,
    sls_prd_key,
    sls_cust_id,
    sls_order_dt,
    sls_ship_dt,
    sls_due_dt,
    sls_sales,
    sls_quantity,
    sls_price
    )

SELECT
    sls_ord_num,
    sls_prd_key,
    sls_cust_id,
    CASE WHEN sls_order_dt = 0 OR LEN(sls_order_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_order_dt AS VARCHAR) AS DATE)
    END AS sls_order_dt,
    CASE WHEN sls_ship_dt = 0 OR LEN(sls_ship_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_ship_dt AS VARCHAR) AS DATE)
    END AS sls_ship_dt,
    CASE WHEN sls_due_dt = 0 OR LEN(sls_due_dt) != 8 THEN 
    NULL ELSE CAST(CAST(sls_due_dt AS VARCHAR) AS DATE)
    END AS sls_due_dt,
    sls_sales,
    sls_quantity,
    sls_price
FROM bronze.crm_sales_details

SELECT *
FROM silver.crm_sales_details

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    bdate,
    gen
FROM bronze.erp_cust_az12
WHERE cid LIKE '%AW00011000%'

SELECT *
FROM silver.crm_cust_info

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    bdate,
    gen
FROM bronze.erp_cust_az12
WHERE CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END NOT IN (SELECT DISTINCT cst_key
FROM silver.crm_cust_info)

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    bdate,
    gen
FROM bronze.erp_cust_az12

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    bdate,
    gen
FROM bronze.erp_cust_az12
WHERE bdate < '1924-01-01' OR bdate > GETDATE()

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    CASE WHEN bdate > GETDATE() THEN NULL ELSE bdate 
    END AS bdate,
    gen
FROM bronze.erp_cust_az12
ORDER BY bdate

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    CASE WHEN bdate > GETDATE() THEN NULL ELSE bdate 
    END AS bdate,
    gen
FROM bronze.erp_cust_az12

--DATA STANDARDIZATION & CONSISTENCY

SELECT DISTINCT
    gen,
    CASE WHEN UPPER(TRIM(gen)) IN ('F', 'Female') THEN 'Female' 
    WHEN UPPER(TRIM(gen)) IN ('M', 'Male') THEN 'Male' 
    ELSE 'NA' 
END AS gen
FROM bronze.erp_cust_az12

SELECT
    cid,
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    CASE WHEN bdate > GETDATE() THEN NULL ELSE bdate 
    END AS bdate,
    CASE WHEN UPPER(TRIM(gen)) IN ('F', 'Female') THEN 'Female' 
    WHEN UPPER(TRIM(gen)) IN ('M', 'Male') THEN 'Male' 
    ELSE 'NA' 
    END AS gen
FROM bronze.erp_cust_az12


INSERT INTO silver.erp_cust_az12
    (
    cid,
    bdate,
    gen
    )

SELECT
    CASE WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid)) ELSE  
    cid
    END cid,
    CASE WHEN bdate > GETDATE() THEN NULL ELSE bdate 
    END AS bdate,
    CASE WHEN UPPER(TRIM(gen)) IN ('F', 'Female') THEN 'Female' 
    WHEN UPPER(TRIM(gen)) IN ('M', 'Male') THEN 'Male' 
    ELSE 'NA' 
    END AS gen
FROM bronze.erp_cust_az12

SELECT
    cid,
    cntry
FROM bronze.erp_loc_a101;

SELECT cst_key
FROM silver.crm_cust_info;

SELECT
    REPLACE(cid, '-', '') AS cid,
    cntry
FROM bronze.erp_loc_a101;

SELECT
    REPLACE(cid, '-', '') AS cid,
    cntry
FROM bronze.erp_loc_a101
WHERE REPLACE(cid, '-', '') NOT IN 
(SELECT cst_key
FROM silver.crm_cust_info)

--DATA STANDARDIZATION & CONSISTENCY

SELECT DISTINCT cntry
FROM bronze.erp_loc_a101
ORDER BY cntry

SELECT
    REPLACE(cid, '-', '') AS cid,
    CASE WHEN TRIM(cntry) = 'DE' THEN 'Germany'
    WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States' 
    WHEN TRIM(cntry) = '' OR cntry IS NULL THEN 'N/A' ELSE
    TRIM(cntry) END AS cntry
FROM bronze.erp_loc_a101;

SELECT DISTINCT
    cntry AS old_cntry,
    CASE WHEN TRIM(cntry) = 'DE' THEN 'Germany'
    WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States' 
    WHEN TRIM(cntry) = '' OR cntry IS NULL THEN 'N/A' ELSE
    TRIM(cntry) END AS cntry
FROM bronze.erp_loc_a101
ORDER BY cntry

INSERT INTO silver.erp_loc_a101
    (cid, cntry)
SELECT
    REPLACE(cid, '-', '') AS cid,
    CASE WHEN TRIM(cntry) = 'DE' THEN 'Germany'
    WHEN TRIM(cntry) IN ('US', 'USA') THEN 'United States' 
    WHEN TRIM(cntry) = '' OR cntry IS NULL THEN 'N/A' ELSE
    TRIM(cntry) END AS cntry
FROM bronze.erp_loc_a101;

SELECT DISTINCT cntry
FROM silver.erp_loc_a101
ORDER BY cntry;

SELECT *
FROM silver.erp_loc_a101;

SELECT
    id,
    cat,
    subcat,
    maintenance
FROM bronze.erp_px_cat_g1v2;

--CHECK FOR UNWANTED SPACES

SELECT *
FROM bronze.erp_px_cat_g1v2
WHERE cat != TRIM(cat) OR subcat != TRIM(subcat) OR
    maintenance != TRIM(maintenance);

--DATA STANDARDIZATION & CONSISTENCY

SELECT DISTINCT
    cat
FROM bronze.erp_px_cat_g1v2;


SELECT DISTINCT
    subcat
FROM bronze.erp_px_cat_g1v2;


SELECT DISTINCT
    maintenance
FROM bronze.erp_px_cat_g1v2;

SELECT maintenance
FROM bronze.erp_px_cat_g1v2
WHERE UPPER(TRIM(maintenance)) != maintenance;

INSERT INTO silver.erp_px_cat_g1v2
    (id, cat, subcat, maintenance)
SELECT
    id,
    cat,
    subcat,
    maintenance
FROM bronze.erp_px_cat_g1v2;

SELECT *
FROM silver.erp_px_cat_g1v2;

CREATE OR ALTER PROCEDURE silver.load_silver
AS
BEGIN
        PRINT '>>TRUNCATING TABLE:silver.crm_cust_info';
        TRUNCATE TABLE silver.crm_cust_info;
        PRINT '>>INSERTING DATA INTO: silver.crm_cust_info';
        INSERT INTO silver.crm_cust_info
            (
            cst_id,
            cst_key,
            cst_first_name,
            cst_last_name,
            cst_material_status,
            cst_gender,
            cst_create_date
            )
        SELECT
            cst_id,
            cst_key,
            TRIM(cst_first_name) AS cst_first_name,
            cst_last_name,
            CASE WHEN UPPER(TRIM(cst_material_status)) = 'S' THEN 'Single' 
            WHEN UPPER(TRIM(cst_material_status)) = 'M' THEN 'Married'
            ELSE 'N/A' END AS 
            cst_marital_status,
            CASE WHEN UPPER(TRIM(cst_gender)) = 'F' THEN 'Female'
            WHEN UPPER(TRIM(cst_gender)) = 'M' THEN 'Male'
            ELSE 'N/A' END AS cst_gender,
            cst_create_date
        FROM (SELECT *,
        ROW_NUMBER () OVER (PARTITION BY cst_id 
        ORDER BY cst_create_date DESC) AS flag_last 
        FROM bronze.crm_cust_info WHERE 
        cst_id IS NOT NULL
        ) t 
        WHERE flag_last = 1;
END 

EXEC silver.load_silver;

EXEC bronze.load_bronze;
