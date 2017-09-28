# Capstone Project I - Clean-up raw geographical data
## Part A
This exercise requires two museum collections from the GBIF repository for two subspecies of house mice:

[0008658-160822134323880.csv](https://github.com/MSandoval-Powers/practice/blob/master/0008658-160822134323880.csv)

[0008659-160822134323880.csv](https://github.com/MSandoval-Powers/practice/blob/master/0008659-160822134323880.csv)

**Steps for Part A**

1. Made a **directory** called NAME_Project1 (replace name with own personal name) and moved into the directory:

`mkdir NAME_Project1`

2. Created two subdirectories for each dataset called **DOM** and **CAST**

`mkdir DOM`

`mkdir CAST`

3. Examined the CSV files to determine which one belonged to -domesticus subspecies or -castaneus subspecies. Copied -domesticus file to **DOM** and -castaneus file to **CAST**

`less -S ORIGINALFILE1.csv`

`less -S ORIGINALFILE2.csv`

`cp ORIGINALFILETHATMATCHESDOM.csv DOM`

`cp ORIGINALFILETHATMATCHESCAST.csv CAST`

## Part B

4. Used `head` command to get the header into a file named **DOM_header.txt**

`head -1 DOMFILE.csv > DOM_header.txt`

Opened original file in a text editor (_nano_) and removed the header line manually

`nano DOMFILE.csv` with ^K to delete first line

5. In **DOM** directory, sorted the csv file based on latitude coordinates. Removed duplicated records. Saved output to new file.

_NOTE: Opened csv file and counted to see that latitude coordinates were in 17th column_

`sort -k17n DOMFILE.csv | uniq > DOM_lat_uniq.txt`

6. Repeat previous step using **DOM_lat_uniq.txt** as input file. Sorted file based on longitude coordinates. Removed duplicated records and saved output to new file. 

_NOTE: Opened csv file and counted to see that longitude coordinates were in 18th column_

`sort -k18n DOM_lat_uniq.txt | uniq DOM_lat_long_uniq.txt`

7. Compared line count of original file (`wc -l DOMFILE.csv`) to new file (`wc -l DOM_lat_long_uniq.txt`) 

```DOMFILE.csv file lines = 8481

DOM_lat_long_uniq.txt file lines = 8389

8481 lines - 8389 lines = 92 lines difference 

92 lines difference/8481 total lines = 1.085% of the original file was duplicated lines of data
```

8. Used `grep` to extract records collected by Smithsonian only (USNM) and named to new file. 

`grep "USNM" DOM_lat_long_uniq.txt > DOM_USNM.txt`

9. Determined line counts in **DOM_USNM.txt** file

`wc -l DOM_USNM.txt`

```DOM_USNM.txt file lines = 4346

DOM_lat_long_uniq.txt file lines = 8389 

4346/8389 = 52% is the proportion of records that are part of Smithsonian collection
```

10. Used `awk` to produce file with only latitude and longitude coordinates

`awk 'FS="t" {print $17, $18}' DOM_USNM.txt > DOM_USNM_lat_long.txt`

11. Used `grep` to remove blank lines and send to new file

`grep -v "^\s*$" DOM_USNM_lat_long.txt > DOM_lat_long_cleaned.txt`

12. Repeated all steps in **Part B** on the CSV file in the **CAST** folder. Recorded line counts and proprotions calculated:

```CASTFILE.csv file lines = 1676

CAST_lat_long_uniq.txt file lines = 1676

_Line counts between the two files were the same = no duplicated records_

CAST_USNM.txt file lines = 1317

1317/1676 = 79% is the proportion of records that are part of Smithsonian collection
```

## Part C

13. Used `cat` to combine cleaned records from both *DOM* and *CAST* into one file in main directory *NAME_Project1*

`cat DOM/DOM_lat_long_cleaned.txt CAST/CAST_lat_long_cleaned.txt > Lat_Long_USNM_combined.txt`

14. Created README file to explain what was done!
