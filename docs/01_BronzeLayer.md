# Bronze Layer – Incremental Ingestion (ADF)

## Objective
Ingest **incremental raw data** from Azure SQL into **ADLS Bronze** as **Parquet**, with **watermark (CDC) tracking**.

## Components
- **ADF** pipeline: `PL_Master_Incremental_Ingestion_Loop`
- **Datasets**: SQL source (generic), ADLS sink (dynamic), JSON for watermark
- **Alerting**: Logic App webhook on pipeline failure

## Flow
1. **Lookup** last watermark from `/bronze/{table}_cdc/cdc.json`
2. **Copy** only rows where `cdc_col > watermark`
3. **IfCondition**: if `dataRead > 0` → update watermark; else delete empty file
4. **SetVariable** to timestamp for file naming
5. **Web** activity triggers Logic App on failure

## Output Structure


## Notes
- Parameter list drives tables (`schema`, `table`, `cdc_col`, `from_date`)
- Translator/type conversion enabled for robustness

## Result
A scalable, table-driven, incremental ingestion layer.
