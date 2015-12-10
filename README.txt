This web scraper scrapes project information from kickstarter.com

To use this please follow the steps below
1. run tableCreate() to create a sqlite3 database with the two tables "Project Links" and "Project Data"
   This step only needs to be performed once
2. run collectLinks(NumberOfPages) to scrape project links kickstarter.com/discover/popular in
   each page has a total of 20 links
3. run collectData() to scrape individual project data from the project links that were collected in the previous step
   Note: this is where the current issue is, as the project links that were scraped and are now in the database are not being iterated through
   
