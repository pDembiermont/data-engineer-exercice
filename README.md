# Data Engineer Test :hammer:

The purpose of this exercice is to run through some of the basic knowledge needed when doing data engineering as your day-to-day job. It's by no way exhaustive, just what appears to me as basic concepts needed for the job.

# Part I

In this part we will see some data managing as well as some data manipulation.

## Step 1 : Download file in python

* Download and save csv files in **utf-8** encoding from [Base de donnée publique du médicament (Bdpm)](http://base-donnees-publique.medicaments.gouv.fr/telechargement.php).
* You will only need the *composition* and *specialties* files (`Fichier des spécialités`, `Fichier des compositions`)
<br><br>
**Tips**: Files are **tab** (`\t`) seprated and encoded using `cp1252`

## Step 2 : Targz

Open the archive `bdpm_headers.tgz` (in `exercice/`) to access the headers for the Bdpm's csv.

## Step 3 : Dataframes Basics

Open the Bdpm's csv in pandas and add the corresponding headers to them.

## Step 4 : Mysql Basics

* Pull a docker image of `mysql` and run it ! (Create a `dockerfile` in `results/mysql/` showing what you did).
* Connect to it from within your previously created script (the one with the pandas dataframes) then add the dataframe to the `mysql` instance (one dataframe is a separated table in the database named respectively `CIS_bdpm` and `COMPO_bdpm`).
* You can view your newly created `mysql` database using a GUI tool like TablePlus (mac/windows) or Sequel Pro (mac only) or simply using CLI.
* Make a dump of the created database, name it `bdpm.sql`, compress its and drop it in `results/mysql/`.

## Step 5 : Sql requests

* In `results/mysql/` create a `bdpm_request.sql` file.
* In this file we will write the `sql` request to join `CIS_bdpm` and `COMPO_bdpm`.
* (There is usefull resources to learn how to do a join in mysql [here](https://www.w3schools.com/sql/sql_join.asp)).
* We will join `COMPO_bdpm` to `CIS_bdpm` on the equivalence of the foreign key `cis`.
<br><br>
* In a new python script, using pandas, execute the previously created sql query.
* You can print the 5 first lines of the resulting pandas dataframe.

## Step 6 : Manipulating Data

* In the resulting dataframe we want to keep only active substances, so you'll need to filter it by `sub_nature` = `SA`.
* In the same dataframe, we want to create a new column called `normalized_longname` that is a copy of the `longname` column except we want the values to be lowered and cleared of any accents. In addition we want to get rid of the portion corresponding to the drug form after the coma. Ex: `ANASTROZOLE ACCORD 1 mg, comprimé pelliculé` -> `anastrozole accord 1 mg`
<br><br>
**Tips**: We want to convert the coma between digits to a dot.

## Step 7 : Filter in SQL

* To avoid unwanted data loading, we can filter the active substances directly in sql.
* Redo `Step 6` but this time don't filter (on `sub_nature`= `SA`) in pandas but directly by amending `bdpm_request.sql` (practically create a new file called `filtered_bdpm_request.sql` in the same directory)

# Part II

As structured data is not the most common source of data on the world wide web, this section aims at gaining basic knowledge on how to harvest and then query unstructured data.

## Step 1 : Data harvesting from the web

* In a new python file / notebook, load this html web page [Zopiclone - Wikipédia](https://fr.wikipedia.org/wiki/Zopiclone).
* From the harvested page, extract `p` and `h2`.
* We end up with a list of selectors, we want to merge `p` until we meet a `h2` then start merging again.
* Merge the `h2` text content to the corresponding paragraph.

**Tips**: use the `xpath` selector to query `p` and `h2` from the root of the div `#mw-content-text`.

## Step 2 : Searching the content using Elasticsearch

* Now that we have some clean paragraphs (almost), we need to allow full text query in them. For this part we will use Elasticsearch.
* Use the `docker-compose` file from [here](https://github.com/deviantony/docker-elk) and start the Elasticsearch, Kibana, Logstash (ELK) stack.
* You will need to use a way to index documents from python into elasticsearch. You can use [this package](https://elasticsearch-py.readthedocs.io/en/master/).
* When documents are indexed, take a look at [kibana](localhost:5601) to check everything is correctly in.
* Now create a function that, given a string, queries Elasticsearch and returns the result.
* Print the result then save it as a json file in `results/elasticsearch/`.

