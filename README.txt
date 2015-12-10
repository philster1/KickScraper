This web scraper scrapes project information from kickstarter.com

To use this please follow the steps below
1. run tableCreate() to create a sqlite3 database with the 
   two tables "Project Links" and "Project Data"
   This step only needs to be performed once
2. run collectLinks(NumberOfPages) to scrape project links 
   from kickstarter.com/discover/popular
   each page has a total of 20 links
3. run collectData() to scrape individual project data from 
   the project links that were collected in the previous step
   
The sqlite data can be best viewed through DB Brower for Sqlite - http://sqlitebrowser.org/
