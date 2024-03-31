---
layout: post
title:  "Enhancing your Letterboxd experience"
date: 2024-03-30
description: Letterboxd already is the favorite platform for movie lovers, what if we could make the experience even better and free?  
image: "/assets/img/cinema.jpeg"
image2: "/assets/img/letterboxd.png"
display_image2: true  # change this to true to display the image below the banner 
---

### Wait! What is Letterboxd?
<img src="{{site.url}}/{{site.baseurl}}/assets/img/letterboxd.png"/>
Letterboxd has emerged as a <a href="https://www.washingtonpost.com/style/of-interest/2023/12/18/letterboxd-fans-movies/" target="_blank">cultural phenomenon</a>, captivating both avid moviegoers and casual viewers with its revolutionary platform. Its blend of practicality and charming format has reshaped the movie-watching experience. Nowadays, it's common to observe movie enthusiasts instinctively reaching for their phones post-screening to share their ratings and opinions. This behavior has not only garnered widespread attention but has also fueled the app's word-of-mouth promotion.
<img src="{{site.url}}/{{site.baseurl}}/assets/img/year-review.png" style="width:500px"/>

Beyond its functionality in tracking watched movies, ratings, and comments, Letterboxd serves as a vibrant social hub. Here, users can engage with friends and prominent content creators, gaining insights into their viewing habits and perspectives on films. In my view, Letterboxd stands as an indispensable tool for anyone passionate about cinema.


### Intro

As you might have guessed, I'm an avid fan of Letterboxd. And as a Data Science student, my love for statistics is undeniable. Surely, you've experienced the joy of receiving your personalized <a href="https://open.spotify.com/wrapped" target="_blank">Spotify Wrapped</a> at the year's end. Well, Letterboxd offers a <a href="https://letterboxd.com/2023/#title-page" target="_blank">similar treat</a>. Every first week of the year, they unveil your personalized page brimming with stats from the past year. But wouldn't it be thrilling to track your movie-watching stats throughout the year? Absolutely! Regrettably, this feature is exclusive to paid subscribers.

However, fear not! Armed with Python skills and the art of web scraping, there's a workaround. While I won't divulge my own findings here, I'm more than willing to impart the knowledge of how to delve into your movie data. Let's embark on this journey together, unlocking the potential of your cinematic adventures.
<hr>

### Tools
I used Python and web scraping since the website does not have API open to the public, since they are currently in closed beta. I read the terms and they even have an option to export your data and since we are not using this practice for commercial purposes we are good to get our own data from the website.

As a disclaimer all the steps were done in a mac computer, so if something looks different it might be because of the different systems.
<hr>

## Web Scraping

### Step 1: Knowing what you want 
Letterboxd keeps your data in 3 main places: Films(Watched), Diary, Reviews
<figure>
<img src="{{site.url}}/{{site.baseurl}}/assets/img/lists.png" allign="middle"/>
</figure>

All 3 will lead to different paths depending in what data you want to collect for me I wanted to answer 3 questions: What decade has the highest rating? What decade has the lowest rating? and What day of the week I watch most movies?

Based off these 3 questions "Films" will attend best my necessities since is where all the movies I've seen are storaged and I can answer these 3 questions.

If you are using the Films page I highly recommend change the view setting for large, that way you also have the last time you watched that movie variable.
<figure>
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/view.png" style="width:500px" allign="middle"/>

  <figcaption>On the left we have the regular view option and without the date. On the right with the large view option and now with the date</figcaption>
</figure>

### Step 2: Organizing 

These are the libraries I used for this project:
<div style="max-width: 800px; margin: 0 auto;">
    {% highlight python %}
    import pandas as pd
    from bs4 import BeautifulSoup
    from selenium import webdriver
    from selenium.webdriver.chrome.service import Service as ChromeService
    from webdriver_manager.chrome import ChromeDriverManager
    from selenium.webdriver.common.by import By
    from selenium.common.exceptions import NoSuchElementException, StaleElementReferenceException
    import time
    import re
    import datetime
    {% endhighlight %}
</div>

If you are new to web scraping the following part will be helpful, otherwise <a href="#step3">click here</a> to go to the next step.

#### The purpose of each library:

pandas (import pandas as pd):
This library is used for data manipulation and analysis. However, it doesn't seem to be used explicitly in the provided code. It might have been intended for further processing of extracted data outside the scope of this script.

BeautifulSoup (from bs4 import BeautifulSoup):
BeautifulSoup is a Python library for pulling data out of HTML and XML files. It provides functions to parse HTML content and extract desired information using various methods and selectors.

selenium (from selenium import webdriver):
Selenium is a powerful tool for web automation. It allows you to interact with web browsers programmatically, enabling tasks such as navigating pages, filling forms, and scraping dynamic content. In this script, Selenium is used to automate the web browser (Chrome) for navigating Letterboxd and extracting movie data.

to install selinium if you haven't, open terminal and run the following line
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
pip install selenium
{%- endhighlight -%}
Once installed you can use selenium on Python
</div>

ChromeService (from selenium.webdriver.chrome.service import Service as ChromeService):
This module is used to customize and control the Chrome browser service. In this script, it's used to initialize the Chrome browser service for Selenium.

ChromeDriverManager (from webdriver_manager.chrome import 
ChromeDriverManager):
ChromeDriverManager is a utility provided by the webdriver_manager package, which automatically downloads and manages the ChromeDriver executable required by Selenium. It ensures that the correct version of ChromeDriver is used for compatibility with the installed Chrome browser.

By (from selenium.webdriver.common.by import By):
By is an enumeration provided by Selenium for specifying the mechanism used to locate elements on a web page. It's used in conjunction with find_element() and find_elements() methods to locate elements by different criteria such as class name, XPath, etc.

NoSuchElementException, StaleElementReferenceException (from selenium.common.exceptions import NoSuchElementException, StaleElementReferenceException):
These are exceptions provided by Selenium to handle common errors encountered during web scraping. NoSuchElementException is raised when an element cannot be found on the page, while StaleElementReferenceException is raised when an element reference becomes stale due to a page refresh or navigation.

time (import time):
The time module provides functions for working with time-related tasks. In this script, it's used to introduce delays between page navigations to avoid overwhelming the website with requests.

re (import re):
The re module provides support for regular expressions in Python. It's used in this script to extract the release year from movie titles using regular expressions.

datetime (import datetime):
The datetime module provides classes for manipulating dates and times in Python. It's used in this script to parse and format dates extracted from the webpage.


<nav id="step3">
</nav>

### Step 3: Setting variables
Because we are web scraping we need to get all the data from a website and through the websriver is how we can access the website to read in our Python code and then get the information we need. The code to read the website is:
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))

# URL of the page we want to star scraping
url = 'https://letterboxd.com/razedori/films/by/date/size/large/'

# Load the initial page, this will make the page open so don't be scared once the page pops
driver.get(url)
{%- endhighlight -%}
</div>

now we can name the variables for the information we want to collect later
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
# Initialize lists to store data
movie_title = []
movie_year = []
movie_rating = []
movie_liked = []
# Last time I watched the movie
movie_watchdate = [] 
{%- endhighlight -%}
</div>

And Because we want to be able to scrape data from every page in the "Films" section and I currently have 100 pages we need to set our current page and last page because later when we do our loop this will make sure we go through each page in the process:
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
pagination = driver.find_element(By.XPATH, ".//div[contains(@class, 'paginate-pages')]/ul")
last_page = pagination.find_elements(By.XPATH, ".//li[@class='paginate-page']")[-1].text

current_page = 1
{%- endhighlight -%}
</div>

<figure>
<img src="{{site.url}}/{{site.baseurl}}/assets/img/html.png" style="width:800px" allign="middle"/>
<figcaption>here what we are looking for when searching for a specific part on a page</figcaption>
</figure>

To be able to find the XPATH, or in other words the path that will make the code know how go to the next page we need to inspect and witht he mouse find the sector where the pages are like in the image below and you'll see the name on the right an that's the name you use in the code.



### Step 4: Definitions

Due to some dificulties I had to use definitions to be able to scrape the data that I wanted each for a specific reason. The code:
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
def extract_release_year(movie):
    soup = BeautifulSoup(movie.get_attribute("outerHTML"), 'html.parser')
    year_tag = soup.find('span', class_='frame-title')

# checking if the movie has a year tag
    if year_tag:
        year_text = year_tag.get_text()
        
        match = re.search(r'\((\d{4})\)$', year_text)
        if match:
            year = match.group(1)  # Extract the year from the matched group
            return year
        else:
            return 'N/A'
    else:
        return 'N/A'

# converting the rating stars to numbers
def convert_rating_to_numeric(rating_text):
    
    rating_mapping = {
        '★': 1,
        '★★': 2,
        '★★★': 3,
        '★★★★': 4,
        '★★★★★': 5,
        '★½': 1.5,
        '★★½': 2.5,
        '★★★½': 3.5,
        '★★★★½': 4.5
    }
    
# Return the numerical value corresponding to the rating symbol
    return rating_mapping.get(rating_text, 'N/A')

    def extract_watch_date(movie):
    watch_date_element = movie.find_element(By.XPATH, ".//time[contains(@class, 'localtime-mmm-dd')]")
    watch_date_text = watch_date_element.get_attribute('datetime')
# Try parsing with the first format
    try:
        watch_date = datetime.datetime.strptime(watch_date_text, "%Y-%m-%dT%H:%M:%S.%fZ")
    except ValueError:
# If the first format fails, try the second format
        watch_date = datetime.datetime.strptime(watch_date_text, "%Y-%m-%d" + "T%H:%M:%S" + "Z")
# Format the date as 'Month Day, Year'
    return watch_date.strftime("%B %d, %Y")


def extract_watch_day_of_week(watch_date_text):
# Extract the day of the week (Monday, Tuesday, etc.)
    return datetime.datetime.strptime(watch_date_text, "%B %d, %Y").strftime("%A")

{%- endhighlight -%}
</div>

### Step 5: Looping

Here's where the magic happens, because in the same page we have different movies and we have multiple pages I created a loop to get all the data we need page by page, one advice I give you is to add delay because most of my problems were that the loop was faster than the reading so some movies got skipped in the process, but with the delay everything worked well.
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
while current_page <= int(last_page):
    container = driver.find_element(By.CLASS_NAME, 'poster-list.-p150.-grid.film-list.clear')
    movie_list_items = container.find_elements(By.CLASS_NAME, 'poster-container')

    for idx, movie in enumerate(movie_list_items):
        try:
            img_element = movie.find_element(By.TAG_NAME, 'img')
            alt_text = img_element.get_attribute('alt')
# Extracting title from alt text
            title = alt_text.split(' - ')[0]
            movie_title.append(title.strip())

# Extracting release year
            release_year = extract_release_year(movie)
            movie_year.append(release_year)

# Extracting rating
            try:
                rating_element = movie.find_element(By.XPATH, ".//span[contains(@class, 'rating')]")
                rating_text = rating_element.text.strip()
                # Convert rating to numeric value
                numeric_rating = convert_rating_to_numeric(rating_text)
                movie_rating.append(numeric_rating)
            except NoSuchElementException:
                movie_rating.append('N/A')

# Checking if a movie has a liked icon
            try:
                like_element = movie.find_element(By.XPATH, ".//span[contains(@class, 'like has-icon icon-liked liked-medium icon-16')]")
                liked = "Yes"
              except NoSuchElementException:
                liked = "No"
              movie_liked.append(liked)
            
# Extracting full watch date
            watch_date = extract_watch_date(movie)
            movie_watchdate.append(watch_date)
        except StaleElementReferenceException:
            # If the element becomes stale, re-locate it
            continue
# Click the next page button
    try:
        next_page_button = driver.find_element(By.CLASS_NAME, 'next')
        next_page_button.click()
    except NoSuchElementException:
        print("Next page button not found.")
        break
    except Exception as e:
        print("Exception occurred while clicking next page button:", e)
        break

    current_page += 1
    
# Introduce a delay before the next iteration
    time.sleep(delay)



# Close the webdriver
driver.quit()

{%- endhighlight -%}
</div>

This process might take a while, for me it took an average of 4 minutes, so depending of how many pages you have on your profile time may vary.

### Step 6: Final Data

Our last step is to organize the data we collected and also add a new column that will get the date we last watched the movie and convert to the day of the week, that way we can later to an analysis with the days of the week in a later post. 
<div style="max-width: 800px; margin: 0 auto;">
{%- highlight python -%}
df = pd.DataFrame({
    'Title': movie_title,
    'Release Year': movie_year,
    'Rating': movie_rating,
    'Liked': movie_liked,
    'Last Time Watched': movie_watchdate
})
df['watch_day_of_week'] = df['Last Time Watched'].apply(extract_watch_day_of_week)
# Here we the dataframe as csv file
df.to_csv('where you want to save', index=False)
{%- endhighlight -%}
</div>

the output should be similar to this:
 
<img src="{{site.url}}/{{site.baseurl}}/assets/img/table.png"/>

<hr>

### Tips & Final Thoughts

I spent a lot time playing around with the html code so one advice that I give you is to go element by element instead of trying to get all of them all at once. Also when testing put a low limitation for the last page, I used 5 to make testing easier.

This code I showed here can be used in any profile you basically need to change the url and as I mentioned at beggining put on the right page and view setting. 

In conclusion was a fun project and see that this can be helpful for people makes me happy too, I am excited to do an analysis with what we collected and see the results. I would guess that my highest decade is the 90s and the lowest the 2020s, but the results will come later this year.

I know web scraping is not easy, I myself spent hours doing the code but it's worth it to pratice with something you like. But if the website offer API I would suggest to use it instead of web scraping since it's easier and approved by the company that offers it.

