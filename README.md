This is my first big web scraping project, so any and all pertinent information will be found in the README.md section. This will include a small intro to web scraping modules, how to go about it and then an intro to the SEC Web site and how to go about scraping it. <br>

Intro to webscraping:<br>
There some great websites out there that takes you through the entire process:  https://realpython.com/python-web-scraping-practical-introduction/ <br>

Two very helpful Reddit Users broke down all the modules/options that can be used to scrape a website, and which one is most useful for which need.<br>
OldSanJuan: "** If the information is being populated by an API call that's easily parsable, just use requests on the API to get all the information you need. No need to parse out html. Most likely the data is already returned in JSON format.**" <br>

Natural-Intelligence: "**Generally speaking, there are three options for web scraping for Python which you can actually mix if you want: Requests+Beautifulsoup, Selenium+Beautifulsoup and Scrapy. If the site is simple (no wild dynamic HTML with Javascript etc.) and your needs are simple, pick Requests+Beautifulsoup. If the site has dynamic HTML produced by Javascript but your needs are simple, you need Selenium+Beautifulsoup. If your needs are complicated (ie. building search engine spider), use Scrapy (+Selenium if site uses dynamic HTML in JS).
<br>
That being said, typically my scraping projects goes: study the site using Browser's inspect to find out where the data comes from, which HTTP method to use and what elements contains the data, then use Requests for getting the page and get the data from the request object (.json() if the page contains only JSON, or parse with XPath or Beautifulsoup if the data is in HTML) and then inspect what I need from there. After that, build a crawler either with some loops or with Scrapy.**""<br>


B. SEC website<br>
The SEC website has no API unlike the Federal reserve website.<br>
The website have some constants that can be leverage to make the parsing easier and that we can use to safeguard our code against future changes on the website: <br>

 1- Formulaic web address: For the SEC daily archives the address is https://www.sec.gov/Archives/edgar/daily-index, we can add "/2020/QTR1" for 2020Q1 filings for all companies.  https://www.sec.gov/Archives/edgar/daily-index/2020/QTR1.<br>
 2- On the https://www.sec.gov/Archives/edgar/daily-index and then "/2020" and then "/QTR1". Adding "/index.json" after each web address allows us to see the web address/ website in JSON format. This makes it easier to parse, since JSON uses a key:value structure that despite the changing HTML will not change <br>0
 3- Once we are at the point of  https://www.sec.gov/Archives/edgar/daily-index/2020/QTR1 we dont need to look into every  index (.idx) we only nned to look for the master index (there will be more than one). This master index, amongst other things, contains the name of the company, the CIK number, the filing type and most importantly it contains a link to the filing in .txt format<br>
 4- The actual link to the file follows a pattern as well: https://www.sec.gov/Archives/edgar/data  + CIK + accession number (a unique ID for each filing) + filings.txt  e.g. https://www.sec.gov/Archives/edgar/data/10048/0001104659-20-000118.txt <br>
<br>
<br>
So the way to go about scraping the SEC website in plain terms is as follow:<br>
  a- get the daily-index base url then add on the year and quarter that you want (if you want multiple years or quarters than make a year list and quarter list and loop over them, which i have done for 2018 and 2019 quarter 1).<br>
  b- grab the master indexes and either download them or loop over them online (Warning: if you make too many requests, the SEC website locks you out and you need to wait before making another request or the code dosent work)<br>
  c- Search in the master index for the company (using the CIK - since that is unique and more accurate than just the nmae) and the filing type. Grab the address that includes the accession-number. If you have multiple filings, store these URLs in a list<br>
  d- Go into the url (or incase of multiple URLs, loop over the list) and parse the document.<br>
  e- I wanted the 10-Q which is a quarterly documents with balance sheets, income statement etc. If the filing is new then there should be a "Financial_Report.xlsx" that we can download. Or else we need to go into the actual .txt file, grab the names and match them against our list to get ".htm" addresses that have the tables. We can download those and store them as excel files.
<br>
<br>
Sources: 
<br>
This exercise/project is based on Sigma coding's sec web scraping series. https://github.com/areed1192 & https://www.youtube.com/channel/UCBsTB02yO0QGwtlfiv5m25Q <br>
Modifications were done to make it more useful for my case, some of which were taken from the youtube comment section for the SEC scraping series and some from Israel-Dryer https://github.com/israel-dryer/.<br>
And a lot of stuff from various questions/answers on stackoverflow.com <br>
<br>
I modified the code to iterate over more than one year and quarter, then to specifcally output and store all master indexes, and then loop over them, to find specific filings and companies (useful: if you are doing a top down approach - starting from the basic url and then pulling filings that are needed). Multiple years and quarters were added specfically for year-over-year and quarter-over-quarter analysis, that alot of analysts are interest in. <br>
