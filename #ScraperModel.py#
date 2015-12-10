import requests
from bs4 import BeautifulSoup
import sqlite3
from urllib import urlopen

pages = 1 #HOW MANY PAGES OF PROJECTS DO YOU WANT TO SCRAPE? (20 PROJECTS PER PAGE)

#setting up sqlite3
conn = sqlite3.connect('kickstarter-project-links.db')  #creates sqlite database
c = conn.cursor()   #starts creation of database
sqlLinks = "SELECT * FROM ProjectLinks;" # WHERE Link =?" #to read from database

#function for creating a table
def tableCreate():
    #ProjectLinks for scraped links && ProjectData for scraped project-specific data
    c.execute("CREATE TABLE ProjectLinks(Link TEXT)")
    c.execute("CREATE TABLE ProjectData(ProjectTitle TEXT, TotalBackers INT, TotalPledged INT, Goal INT, ProjectLength INT, EndDate TEXT, HoursLeft REAL, TotalUpdates INT, TotalComments INT, Description TEXT)")

##################################################LINK SCRAPING#####################################################

#Data Variables collected for ProjectLinks.db
link = ""

#function for adding data to ProjectLinks.db
def dataEntryLinks(link): 
    c.execute("INSERT INTO ProjectLinks(Link) VALUES (?)",
              (link,))
    conn.commit() #absolutely need to do this to save data to table

#Collecting all of the links from the 'Discover' page
def collectLinks():
    for i in range (1,pages + 1,1): 
        url = "https://www.kickstarter.com/discover/advanced?woe_id=0&sort=popularity&seed=2415707&page=" + str(i)
        r = requests.get(url)

        soup = BeautifulSoup(r.content, "html.parser") #open(filename) for html files

        projects_list = soup.find_all("h6", "project-title")

        for project in projects_list:
            for a in project.find_all('a', href=True):
                link = "https://www.kickstarter.com" + str(a['href'].replace('?ref=popular',''))
                dataEntryLinks(link)
        print "completed page" + str(i)

###########################################PROJECT SPECIFIC DATA SCRAPING############################################

#Data Variable collected for ProjectData.db
projectTitle = ""
backersNum = 0
totalPledged = 0
goal = 0
projLength = 0
endDate = ""
hrsLeft = 0
updatesNum = 0
commentsNum = 0
descriptionTxt = ""

#function for adding data to ProjectData.db
def dataEntryData(projectTitle, backersNum, totalPledged, goal, projLength, endDate, hrsLeft, updatesNum, commentsNum, descriptionTxt):
    c.execute("INSERT INTO `ProjectData`(ProjectTitle, TotalBackers, TotalPledged, Goal, ProjectLength, EndDate, HoursLeft, TotalUpdates, TotalComments, Description) VALUES (?,?,?,?,?,?,?,?,?,?)",
              (projectTitle, backersNum, totalPledged, goal, projLength, endDate, hrsLeft, updatesNum, commentsNum, descriptionTxt,))
    conn.commit() #absolutely need to do this to save data to table
            
#Collecting all of the data from the individual projects
def collectData():
    for row in c.execute(sqlLinks):
        projectUrl = str(row).replace("(u'","").replace("',)", "")
        r = requests.get(projectUrl)

        soup = BeautifulSoup(r.content, "html.parser")

        raw_data = soup.find_all("section")

        for data in raw_data:
            try: projectTitle = data.find("h2", {"class":"normal mb1"}).text #title
            except: pass
            try: backersNum = data.find("data", {"itemprop":"Project[backers_count]"}).text #total backers
            except: pass
            try: totalPledged = data.find("data", {"itemprop":"Project[pledged]"}).text #funding
            except: pass
            try: goal = data.find("span", {"class":"money usd no-code"}).text #goal
            except: pass
            try:
                for span_data in data.find_all('span', id=True):
                    projLength = span_data['data-duration'] #project length
                    endDate = span_data['data-end_time'] #project end date
                    hrsLeft = span_data['data-hours-remaining'] #hours left for funding
            except: pass
            try: updatesNum = data.find("a", {"data-content":"updates"}).text.replace("Updates\n(","").replace(")\n","") #number of updates
            except: pass
            try:
                for data in data.find_all('span', id=True):
                    commentsNum = data['data-comments-count'] #number of comments 
            except: pass
            try: descriptionTxt = data.find("div", {"class":"full-description"}).text.replace("\n","") #description text
            except: pass
            try:
                dataEntryData(projectTitle, backersNum, totalPledged, goal, projLength, endDate, hrsLeft, updatesNum, commentsNum, description)
            except: pass
        print str(row) + " Complete"
    print "DONE"
              
