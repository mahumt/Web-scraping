
From the Edgar website itself: <br>
"The EDGAR indexes list the following information for each filing: Company Name, Form Type, CIK (Central Index Key), Date Filed, and File Name (including folder path).
<br>
Four types of indexes are available. The company, form, and master indexes contain the same information sorted differently.
<br>
company — sorted by company name <br>
form — sorted by form type <br>
master — sorted by CIK number  ----> this is being used in this exercise. It has a delimiter, makeing it easier to parse <br>
XBRL — list of submissions containing XBRL financial files, sorted by CIK number; these include Voluntary Filer Program submissions" <br>


This exercise/project is based on Sigma coding's sec web scraping series. 
Some modifications were done to make it more useful for my case,
some of which were taken from the youtube comment section for the SEC scraping series and some from Israel-Dryer's page. <br>

I modified the code to iterate over more than one year and quarter, then to specifcally output and store all master indexes, and then loop over them,
to find specific filings and companies (useful: if you are doing a top down approach - starting from the basic url and then pulling filings that are needed). 
Multiple years and quarters were added specfically for year-over-year and quarter-over-quarter analysis, that alot of analysts are interest in. <br>


Sources
https://www.sec.gov/edgar/searchedgar/accessing-edgar-data.htm <br>
https://github.com/areed1192/sigma_coding_youtube/blob/master/python/python-finance/sec-web-scraping/Web%20Scraping%20SEC%20-%20Daily%20Index.ipynb <br>
https://github.com/israel-dryer/Web-Scraping/blob/master/SEC-Read-IDX-Into-Pandas.ipynb <br>
