# About Me
Welcome to my portfolio! I love working with data to discover meaningful business insights and enjoy presenting complex ideas through accessible visualizations. My professional background is in analytics and IT implementation, you can find my resume [here](https://github.com/sohampatki/Soham-Patki-s-Portfolio/blob/main/Resume_SohamPatki_Feb24.pdf). Find below a sample of the analytics projects I have worked on, either independently or through my Master's in Business Analytics program.

# Contents
* [About Me](https://github.com/sohampatki/Soham-Patki-s-Portfolio/tree/main?tab=readme-ov-file#about-me)
* [Projects](https://github.com/sohampatki/Soham-Patki-s-Portfolio/tree/main?tab=readme-ov-file#projects)
    * [Speciality Chocolate Competitor Analysis Dashboard](https://github.com/sohampatki/Soham-Patki-s-Portfolio/tree/main?tab=readme-ov-file#project-1-speciality-chocolate-competitor-analysis-dashboard)
    * [Hotel Reservations Relational Database Design Project](https://github.com/sohampatki/Soham-Patki-s-Portfolio/tree/main?tab=readme-ov-file#project-2-hotel-reservations-relational-database-design-project)
    * [IMDb Top 500 Rated Movies Web Scraping Project](https://github.com/sohampatki/Soham-Patki-s-Portfolio/tree/main?tab=readme-ov-file#project-3-imdb-top-500-rated-movies-web-scraping-project)
    * [Data Profiling and Cleaning Project](https://github.com/sohampatki/Soham-Patki-s-Portfolio/tree/main?tab=readme-ov-file#project-4-data-profiling-and-cleaning-project)
* [Contact Me](https://github.com/sohampatki/Soham-Patki-s-Portfolio/blob/main/README.md#contact-me)

# Projects
## [Project 1: Speciality Chocolate Competitor Analysis Dashboard](https://github.com/sohampatki/Portfolio/tree/main/Competitive%20Analysis%20Dashboard%20(Tableau)%20)

<b>Objective</b>: Developing a strategic dashboard to support business decision-making for a fictional specialty chocolate brand. Designed to help decision makers track competitors' ratings across products, survey their sourcing practices, track bean prices and gauge popularity of different chocolate formulations. As an interactive dashboard, all data is relationally linked and can be filtered by clicking different visual elements.<br>
<b>Tools</b>: Tableau Desktop, R<br>
<b>Skills</b>: Data transformation, joins, data visualization, dashboard design, business strategy development<br>
<b>Sources</b>: [Manhattan Chocolate Society](https://flavorsofcacao.com/chocolate_database.html), [International Cocoa Organization](https://www.icco.org/fine-or-flavor-cocoa/)<br>

<b>Procedure</b>:

1. Identifying data sources for chocolate bar ratings, cocoa bean quality ratings by country and cocoa bean prices by country.
2. Using R to manipulate raw data and make joining them possible using common parameters.
3. Transforming variables to make interpretation and visual analysis more intuitive.
4. Creating a slide deck explaining different aspects of the dashboard and demonstrating examples of effective usage.<br><br>
<img width="1412" alt="Dashboard Screenshot" src="https://github.com/sohampatki/Portfolio/assets/133144327/d3379130-2075-4380-bfd0-09b5afcc0f68"><br>

- Download Tableau workbook [here](https://github.com/sohampatki/Soham-Patki-s-Portfolio/blob/main/Specialty%20Chocolate%20Dashboard/Specialty%20Chocolate%20Brand%20and%20Bean%20Quality%20Tracker.twbx) <br>
- View interactive dashboard [here](https://public.tableau.com/app/profile/sohampatki/viz/SpecialtyChocolateBrandandBeanQualityTracker/Dashboard1) <br>
- View slide deck [here](https://github.com/sohampatki/Portfolio/blob/main/Dashboard_Slide_deck.pdf)<br>


## [Project 2: Hotel Reservations Relational Database Design Project](https://github.com/sohampatki/Portfolio/tree/main/Hotel%20Reservations%20Database%20Design)

<b>Objective</b>: Designing a relational database to store hotel room reservations, promotional offers and discounts <br>
<b>Tools Used</b>: MySQL, LucidChart <br>
<b>Skills Used</b>: Defining business requirements, ERD design, schema design, normalization, database implementation <br>
<b>Procedure</b>:

1. Determining the business value of hotels providing customers the option of making reservations directly with them instead of through a third party vendor like Expedia.
2. Defining the project's objective and identifying business requirements for the database to effectively capture relevant information, taking into account the complexities of managing room reservations at a large hotel. 
3. Establishing relevant entities, their attributes and the cardinalities of relationships between different entities.
4. Developing an ERD and Database Schema, ensuring it is normalized to 3NF.
5. Writing SQL code to implement the database using sample data for customers, hotel rooms, reservations, billing information etc.
6. Demonstrating how the database can support routine business operations and provide detailed analytical insights using SQL queries.<br>
![download](https://github.com/sohampatki/Soham-Patki-s-Portfolio/assets/133144327/203138e8-e26f-4339-a439-b98f8e6eb154)<br>
- View SQL file [here](https://github.com/sohampatki/Portfolio/blob/main/Hotel%20Reservations%20Database%20Design/Hotel%20Reservations%20Relational%20Database%20(MySQL)) <br>
- View slide deck [here](https://github.com/sohampatki/Portfolio/blob/main/Hotel%20Reservations%20Database%20Design/Database%20Design%20Slide%20Deck.pdf) <br>
- View project description [here](https://github.com/sohampatki/Portfolio/blob/main/Hotel%20Reservations%20Database%20Design/Design%20Process%20Description.pdf)<br>

## [Project 3: IMDb Top 500 Rated Movies Web Scraping Project](https://github.com/sohampatki/Portfolio/tree/main/WebScraping)

<b>Objective</b>: Scraping information about the top 500 most popular movies on IMDb from the year 2018-2020 and identifying basic trends in viewer preferences. <br>
<b>Tools Used</b>: Python (Beautiful Soup, Pandas, Matplotlib), Alteryx <br>
<b>Skills Used</b>: Web scraping, data cleaning, joins, data visualization <br>
<b>Source</b>: [IMDb.com](https://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020)<br>
<b>Procedure</b>:
 1. Collecting information on 8 parameters: movie ID, title, rank, release year, movie
runtime, rating, number of votes, and genres.
2. Storing the information in a sturctured (.csv) format
3. Cleansing and transforming data using Alteryx to remove unwanted characters (leading and trailing whitespaces, characters, punctuation) from numerical variables
4. Changing datatypes to the correct format and joining the dataset with an existing dataset of older rankings using Alteryx.
5. Using python to visualize data and find underlying trends in viewer preferences

- View Python code [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/Python_Code.ipynb) <br>
- View project description [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/Project%20Description.pdf) <br>
- View scraped data [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/IMDBTop500.csv) <br>
- View data visualizations [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/IMDB_Vizzes.ipynb)<br>
 
## [Project 4: Data Profiling and Cleaning Project](https://github.com/sohampatki/Portfolio/blob/main/Data%20Profiling%20and%20Cleaning%20Project.md)

As part of my Master's coursework, this project provided a systematic procedure for exploring the characteristics of raw data and preparing it for analysis. The steps involved were:
1. Determining the dimensions of the dataset, the data types of each variable, unique values, null values and duplicated rows.
2. Using summary statistics to get a better understanding of the different variables in the dataset.
3. Cleaning the dataset by fixing formatting issues, handling null values, modifying data types and removing duplicated rows.

- View Python code [here](https://github.com/sohampatki/Soham-Patki-s-Portfolio/blob/main/Data%20Profiling%20and%20Cleaning%20Project.md)

# Contact Me
Email: patki.s@northeastern.edu<br>
LinkedIn: https://www.linkedin.com/in/soham-patki/
