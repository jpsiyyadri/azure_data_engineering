- what's the file structure suggested by microsoft for batch data lake

`{subjectmatter}/raw/year=2020/month=1`
reports -> doctor group

`{subjectmatter}/transformed/year=2020/month=1`

billing - finance, doctor

service group
    -> doctors-sg: read-patients-report, write-patients-report
        *doctor1
        * doctor3
    -> accountants-sg
    -> nurses-sg
    -> marketing-sg

doctor

``

- what's the file structure suggested by microsoft for streaming

- how to manage data between teams and give them access

- what's the importance of `surrogate key`

- why do you need spark pool at all?

- `Slowly Changing Dimensions` for frequently changed  data

    `users` -> unique: user_id
    `surrogateKey` -> id, row_id

  - SCD 1 : do not maintain any history
  - SCD 2 : set the flag -> `isActive` column for recent change and filter based on that
  - SCD 3 : maintain previous history not the entire history add a new column `prevValue`
  - SCD 4 : create a new table fast changing data

- What's `temporal tables`

  - for auditing/forensic you have to maintain history

    - How to create temporal tables

    ```sql
        CREATE TABLE YourTemporalTable
        (
            ID INT PRIMARY KEY,
            Name NVARCHAR(50) NOT NULL,
            ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL,
            ValidTo DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL,
            PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
        )
        WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.YourTemporalTableHistory));
    ```

    how do you get latest data if you store all the history, derive it by ValidTo column year which will be `9999`

- Layers

- LandingZone
- TransormationZone
  - USE `round robin` splits data equally
  - load the data from landing zoe
  - transform it
  - write into serving zone

- serving zone
  - indexing: hashing
  - execute queries and return the results

replicated tables: across distributions
