```
title: BEAST2 Phylogeography
```

# BEAST2 Phylogeography

## Outline

1. Download WHO case data.

## Steps

### Download WHO Case Data

- This project uses the database constructed by [[xu2019HistoricalGenomicData|Xu et al. (2019)]].
- Convert the SI xlsx database from [[xu2019HistoricalGenomicData]] to a nice Lat,Lon file for Palladio.

```bash
echo -e "ID\tLatLon\tDate" > xu2019HistoricalGenomicData_palladio.tsv;
tail -n+2 xu2019HistoricalGenomicData_SI.txt | while read line;
do
  ID=`echo "$line" | cut -f 1`;
  Date=`echo "$line" | cut -f 4 | cut -d "." -f 1`;
  LatLon=`echo "$line" | awk -F "\t" '{print $3","$2}'`;
  echo -e "$ID\t$LatLon\t$Date";
done >> xu2019HistoricalGenomicData_palladio.tsv
```

1. Create a new project: ```1-ORI``` 
1. Construct a SQL query to select just #Clade/1-ORI  samples.
	```bash
	SELECT * FROM BioSample 
		WHERE 
		BioSampleBranch LIKE "%1.ORI%" 	
		AND 
		BioSampleComment LIKE "%KEEP%Assembly MODERN%"
	```
This selects 112 rows.

1. Construct a new multiple alignment.
	```
	workflow/scripts/project_clean.sh results rm
	workflow/scripts/project_load.sh results ../plague-phylogeography-projects/1-ORI rsync
	```

## Tools

```yaml
BEAST2: 2.6.3
GEO-SPHERE: 
spreaD3
```