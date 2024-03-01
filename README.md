Thank you for visiting my profile! Find below a sample of the analytics projects I have worked on, either independently or through my Master's in Business Analytics program.

### [Project 1: IMDb Top 500 Rated Movies Web Scraping Project](https://github.com/sohampatki/Portfolio/tree/main/WebScraping)

<b>Objective</b>: Scraping information about the top 500 most popular movies on IMDb from the year 2018-2020 and identifying basic trends in viewer preferences. <br>
<b>Tools Used</b>: Python (Beautiful Soup, Pandas, Matplotlib), Alteryx <br>
<b>Source</b>: [IMDb.com](https://www.imdb.com/search/title/?at=0&sort=num_votes,desc&start=1&title_type=feature&year=2018,2020)<br><br>
<b>Procedure</b>:
 1. Collecting information on 8 parameters: movie ID, title, rank, release year, movie
runtime, rating, number of votes, and genres.
2. Storing the information in a sturctured (.csv) format
3. Cleansing and transforming data using Alteryx to remove unwanted characters (leading and trailing whitespaces, characters, punctuation) from numerical variables
4. Changing datatypes to the correct format and joining the dataset with an existing dataset of older rankings using Alteryx.
5. Using python to visualize data and find underlying trends in viewer preferences
 
View Python code [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/Python_Code.ipynb) <br>
View project description [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/Project%20Description.pdf) <br>
View scraped data [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/IMDBTop500.csv) <br>
View data visualizations [here](https://github.com/sohampatki/Portfolio/blob/main/WebScraping/IMDB_Vizzes.ipynb)
### [Project 2: Hotel Reservations Relational Database Design Project](https://github.com/sohampatki/Portfolio/tree/main/Hotel%20Reservations%20Database%20Design)

<b>Objective</b>: Designing a relational database to store hotel room reservations, promotional offers and discounts <br>
<b>Tools Used</b>: MySQL, LucidChart <br>
<b>Procedure</b>:<br>

1. Determining the business value of hotels providing customers the option of making reservations directly with them instead of through a third party vendor like Expedia.
2. Defining the project's objective and identifying business requirements for the database to effectively capture relevant information, taking into account the complexities of managing room reservations at a large hotel. 
3. Establishing relevant entities, their attributes and the cardinalities of relationships between different entities.
4. Developing an ERD and Database Schema, ensuring it is normalized to 3NF.
5. Writing SQL code to implement the database using sample data for customers, hotel rooms, reservations, billing information etc.
6. Demonstrating how the database can support routine business operations and provide detailed analytical insights using SQL queries.
![download](https://github.com/sohampatki/Soham-Patki-s-Portfolio/assets/133144327/203138e8-e26f-4339-a439-b98f8e6eb154)


View SQL file [here](https://github.com/sohampatki/Portfolio/blob/main/Hotel%20Reservations%20Database%20Design/Hotel%20Reservations%20Relational%20Database%20(MySQL)) <br>
View slide deck [here](https://github.com/sohampatki/Portfolio/blob/main/Hotel%20Reservations%20Database%20Design/Database%20Design%20Slide%20Deck.pdf) <br>
view project description [here](https://github.com/sohampatki/Portfolio/blob/main/Hotel%20Reservations%20Database%20Design/Design%20Process%20Description.pdf)

### [Project 3: Speciality Chocolate Competitor Analysis Dashboard](https://github.com/sohampatki/Portfolio/tree/main/Competitive%20Analysis%20Dashboard%20(Tableau)%20)

To practice my data visualization skills, I developed a strategic dashboard aimed to support the business decision-making of a specialty chocolate brand. The dashboard was created with the aim of helping decision makers track the ratings of competing brands and each of their product offerings. The dashboard also tracked the prices and quality of beans for each bean exporting country, the average ratings of different chocolate formulations, and the frequency with which different cocoa percentages were sold in the market. The dashboard is designed to be interactive, and filtering queries can be applied to it by clicking on different visual elements. All the visualizations in the dashboard are linked to each other and dynamically update based on the user's filtering. Steps involved in creating the dashboard were:

1. Identifying data sources for chocolate bar ratings, cocoa bean quality ratings by country and cocoa bean prices by country.
2. Using R to manipulate raw data and make joining them possible using common parameters.
3. Transforming variables to make interpretation and visual analysis more intuitive.
4. Creating a slide deck explaining different aspects of the dashboard and demonstrating examples of effective usage.
<img width="1412" alt="Dashboard Screenshot" src="https://github.com/sohampatki/Portfolio/assets/133144327/d3379130-2075-4380-bfd0-09b5afcc0f68">

View interactive dashboard [here](https://public.tableau.com/app/profile/sohampatki/viz/SpecialtyChocolateBrandandBeanQualityTracker/Dashboard1) <br>
View slide deck [here](https://github.com/sohampatki/Portfolio/blob/main/Dashboard_Slide_deck.pdf)


### [Project 4: Data Profiling and Cleaning Project](https://github.com/sohampatki/Portfolio/blob/main/Data%20Profiling%20and%20Cleaning%20Project.md)

As part of my Master's coursework, this project provided a systematic procedure for exploring the characteristics of raw data and preparing it for analysis. The steps involved were:

1. Determining the dimensions of the dataset, the data types of each variable, unique values, null values and duplicated rows.
2. Using summary statistics to get a better understanding of the different variables in the dataset.
3. Cleaning the dataset by fixing formatting issues, handling null values, modifying data types and removing duplicated rows. 
