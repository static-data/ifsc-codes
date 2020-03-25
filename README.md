# Static IFSC codes in sqlite

## Motivation

* Keep all static data in fast lookup files for easy access in applications
* Keep the static files in a repo which servers can download upon deployment
* Ligher services and databases like RDBMS

## Tools

* Install csvkit to work with different file formats and output csv first
** On Mac
   ```
     $brew install csvkit
     $brew install sqlite
   ```

## Data Preparation

### IFSC Codes

* Navigate to https://www.rbi.org.in/scripts/neft.aspx and get the consolidated list; Example:
$curl 'https://rbidocs.rbi.org.in/rdocs/content/docs/68774.xlsx' > 68774.xlsx && in2csv 68774.xlsx > ifsc_codes.csv

* Ensure you've created a database ifsc_codes.db in sqlite

```
$sqlite ifsc_codes.db
sqlite>
```

* On a different terminal import, import your csv file into the newly created database
```
$csvsql --db sqlite:///ifsc_codes.db  --insert ifsc_codes.csv
```

* Back on the sqlite terminal window, check if the table got created

```
sqlite> .schema
CREATE TABLE ifsc_codes (
	"BANK" VARCHAR NOT NULL,
	"IFSC" VARCHAR NOT NULL,
	"BRANCH" VARCHAR,
	"ADDRESS" VARCHAR,
	"CONTACT" VARCHAR,
	"CITY" VARCHAR,
	"DISTRICT" VARCHAR,
	"STATE" VARCHAR
);
```

* Create any indexes you want back on the sqlite terminal window

```
sqlite> CREATE INDEX ind_city_bank ON ifsc_codes(city, bank);
sqlite> CREATE UNIQUE INDEX ind_city_bank ON ifsc_codes(ifsc);
```

* Checkin the ifsc_codes.db file into github
