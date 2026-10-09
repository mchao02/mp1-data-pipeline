# Pipeline 
The pipeline begins by loading in the input file provided by the user which comes in through the data_loaders.py file and takes 3 different file types (CSV, JSON, and YAML). For this pipeline, the input dataset is a CSV file and the YAML file which provides the validation and cleaning instructions. Next is data_validating which checks for valid column names by comparing the given column names in the datafile with the required names from the YAML file. The validatiding also checks 
tge configured numeric columns, and removed rows with data that cannot be converted to numbers. After that is data cleaning which happens in the data_processor file in which duplicates, rows or colums with missing data, and outliers (using the iqr method or zscore depending on the YAML instructions) are removed. The final cleaned dataset is then saved as a CSV file through the  data_output file and a cleaning report is printed. The modules are organized in the src folder, the settings are in config, the sample data files are in fixtures, and generated results are saved in output. The pipeline.py file connects all these steps while utils.py hanldes the logging setup and checks that input file exist.

## Running the Pipeline
``` BASH 
python pipeline.py --input fixtures/sample_data.csv --output output/clean.csv --config config/config.yaml --verbose
```

### Output

```text
2026-10-09 19:44:40,401 DEBUG    __main__ — Arguments parsed: input=fixtures/sample_data.csv, output=output/clean.csv, config=config/config.yaml
2026-10-09 19:44:40,401 INFO     src.utils — Input file validated: fixtures/sample_data.csv
2026-10-09 19:44:40,401 INFO     src.utils — Input file validated: config/config.yaml
2026-10-09 19:44:40,404 INFO     src.data_loaders — Load CSV file: fixtures/sample_data.csv (100 rows)
2026-10-09 19:44:40,405 INFO     src.data_loaders — loaded YAML file: config/config.yaml
2026-10-09 19:44:40,406 WARNING  src.data_validator — Invalid numeric value in rating, value: not_available
2026-10-09 19:44:40,406 WARNING  src.data_validator — Invalid numeric value in rating, value: error
2026-10-09 19:44:40,408 DEBUG    src.data_validator — Valid rows: 98, rows removed: 2
2026-10-09 19:44:40,408 INFO     __main__ — validation complete: 100 to 98 rows
2026-10-09 19:44:40,411 DEBUG    src.data_processor — Number of rows removed: 2
2026-10-09 19:44:40,412 DEBUG    src.data_processor — removed 2 rows
2026-10-09 19:44:40,413 DEBUG    src.data_processor — rating removed, threshold = 1.5, removed=2, method=iqr
2026-10-09 19:44:40,414 INFO     __main__ — Processing: 98 to 92 rows
2026-10-09 19:44:40,415 DEBUG    src.data_output — Saved 92 rows to output/clean.csv
2026-10-09 19:44:40,415 INFO     __main__ — Saved cleaned data to output/clean.csv
{'rows_before': 98, 'rows_after': 92, 'rows_removed': 6, 'columns_before': 5, 'columns_after': 5, 'columns_removed': 0}
```