# Automated the analysis of patient experience from feedback collected from NHS patients across different platforms using natural language processing, data science and data analysis techniques.
Analyzing patient experience involves identifying patient feedback, which healthcare organizations use to evaluate their services. Manually analyzing such data would be extremely
difficult, time-consuming, and expensive hence the need to automate this process. This research looks at collecting feedback in the form of online reviews from the National Health
Service, in England, across its website and google, from hospitals and General Practices. The
feedback was then analyzed using data analysis techniques and topic modeling to identify
key themes, trends, and anomalies. These experiments provided valuable insights towards
finding the different topics mentioned by patients from which we could infer which specific
service struggled or performed well and how: firstly, the services compared to each other
and secondly, how the platforms compared against each other. We were also able to identify
where the potential problem areas in England corresponding to the negative words associated with certain topics were found and the respective areas corresponding to positive patient
experience, using a choropleth representation.

In this readme we will walk you through the structure of the project. A combination of **python scripts** and **interactive python notebooks** were used to create this project. 

**The ipynb notebooks were the main souce of our code**.

**Notion**, which is an online notebook was also used to write notes, tutorial of how code works using code snippets. We will link the open notion's page whereever applicable.

Also, the scripts have been tailored and tested to test a graph and python notebooks for experiments. The process to get the data was explained in the report, where we used a combination of sub python script for basic work such as json files manipulation, counting, merging,data style manipulation, etc... Excel was also extensively used for the data part. The data is available on a linked google drive link, and the structure is explained at the end.


## **Packages for the python scripts**
```console
(venv) kxm874> pip list
Package    Version
---------- -------
pip        20.2.3
setuptools 49.2.1


kxm874:~$ pip install scrapy 
kxm874:~$ pip install selenium
kxm874:~$ pip install pandas
kxm874:~$ pip install beautifulsoup4
kxm874:~$ pip install plotly
kxm874:~$ pip install openpyxl
kxm874:~$ pip install geojson_rewind
```

The project can be broken down into 
- data collection
- data analysis
- topic modelling 


## **1. Data collection**

The project involved collecting data in the form of online reviews about the National Health Service. Data scraping was used to scrape those reviews and we collected data from two platforms, i.e. google(google reviews as they appear when you search for a place, and in our case the places were hospitals, clinics and General Practices) and the NHS's official website(services, i.e. hospitals, clinics and General Practices found on https://www.nhs.uk)

This process is fully explained in the following notion page:
[https://pickle-word-f76.notion.site/1-NHS-REVIEWS-COLLECTION-f080c0a68b8d482cb041b481b5345134](https://www.notion.so/kavishmuthoora/Analyzing-Patient-Experience-through-reviews-collected-from-the-NHS-across-Different-Platforms-using-a0dae8f5d95a41019049efdb2e1265ea?pvs=4)

The notion page might contain an older version of our script below but the idea remained the same.

### **1.1 Collecting reviews from  https://www.nhs.uk**


A scrapy spider was used for this part

We will provide a test section of a couple of services. The resulting .json files corresponding to each hospital or gp will be found in data and named as per the name of that entity.

The file containing the concatenation of the results will be in the directory below with the name as per below

First make sure to be in the right directory.
```console
kxm874:~  cd scrapy-nhsreviews\reviewscraper\reviewscraper
```

For a test subset run
```console
kxm874:~ scrapy crawl test-spidey -O testreviews.json
```
tests three hospitals: airedale general hospital, arrowe park hospital and alder heys childrens

scrape count should be around 171- depends if new reviews get posted in the meantime or existing ones get taken down due to moderation

For the full code on getting reviews run:
```console
kxm874:~$ cd scrapy-nhsreviews\reviewscraper\reviewscraper
kxm874:~$ scrapy crawl hospital-spider -O "instructor-hospital-reviews-test.json"
kxm874:~$ scrapy crawl gps-spider -O "instructor-gps-reviews-test.json"
```

### **1.2 Collecting reviews from Google**


We need to change directory to 

```console
kxm874:~$ cd selenium-bs4
```

Make sure you have a chrome crowser and that the version is compatible with chromedriver version for more info and on how to update. We are using a chrome version of Version 100.0.4896.127 (Official Build) (64-bit) and a corresponding chromedriver which can be found at https://chromedriver.storage.googleapis.com/index.html?path=100.0.4896.60/

For more info:
https://chromedriver.chromium.org/downloads

Below you can see a test version of the main script which takes days to run. The script only fetches data from a single place and stores the reviews in a csv file.


```console
kxm874:~$ google-reviews-scraper.py
```

However this file will show the automation of navigating through a page, scrolling a looking at the reviews. **Google recently updated their data policy and did not make the reviews publicly available anymore and updated their website which can lead to elements path changes which are static in this script** However, the script should demonstrate how the algorithm works, how the infinite loop scroll was implemented to reach the end. 

The hospitals-google-scaper.py file demonstrates the addition of manualle getting the names and address of hospitals from a csv and searching for the service on maps, collecting its reviews along the way. We also record the hospitals we might have missed and go over them again by changing the input file to the "...missed.csv"

The gps had a direct link, so the fist version described in the test file is used, only passing a list of start urls instead of one and iteratively going through all of them.

Throughout the files we use batches as checkpoints in case an error occurs to save a csv file of the results every 100 or so visited places. The batching was manually implemented.

Note these take a long time to run, to manually close them through the terminal after you have seen enough.

To run the scripts use,
```console
kxm874:~$ hospitals-google-scraper.py
kxm874:~$ gps-google-scraper.py
```
