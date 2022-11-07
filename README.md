# Advanced-Data-Manipulation-with-R-programming

COVID-19 Pandemic in the United States
The COVID-19 pandemic in the United States is the deadliest pandemic in its history. With 842,141 confirmed deaths in January of 2022, it has wrought many a change including the further polarization of American politics, increase in virtual work environments, manufacturing and supply chain complications, increase in online learning, greater engagement in video streaming and gaming, and many others.

The pandemic has negatively effected the economics of all countries including the United States. A table on the linked page presents a statistical summary with various data including job levels, umemployment rate, inflation rate. The data is given for February through August.

Scrape and Transpose Data

Here, I will scrape the data from the table in Wikipedia. For this, I will use rvest to pull the data and will use XPath as my selector type.

The data I just scraped is not oriented properly. The variable names are contained on the left, with the data stretching to the right. The variable names should be located at the top row, not the left-most column.

Transposing data can be tricky. To transpose data, at its simplest, use the function t(). If your data frame is named wiki_data, then you would do t(wiki_data) and save it as a new data frame. Unfortunately, transposing data is never that simple.

I recommend using the library data.table with its function transpose(). During the process of transposing the data, you will find the column headers are not the correct data. Instead, R inserted generic names. You will need to remove the header, and then convert the first row into the header. Additionally, by doing this, the indexing of the data frame will be changed and out of order. You will need to update the ordering. Below is some code showing how to perform these steps.

# Transpose the data

covid_data_t = transpose(covid_data)

# Extract the 1st row of data, to be used as header

names(covid_data_t) = covid_data_t[1,]

# Remove the first row of data, which is a duplicate now

covid_data_t <- covid_data_t[-1,]

# Adjust the indexing of the data

rownames(covid_data_t) <- 1:nrow(covid_data_t)

Be sure you do not have tidyverse loaded while performing these functions. Some of the base R functions that table.data relies on will be changed by tidyverse, leading to errors.

Once the conversion is done, you will need to add the month data back in. During the transpose process, that data was lost. You can use the function add_column() from the library tibble (more information here).

That is it! Your data is not ready for processing. On to the next section.

Rename Columns (4 pts.)
With the data oriented correctly, using dplyr, rename columns like so:

Jobs, level (000s) change to jobs_lvl

Jobs, monthly change (000s) change to jobs_mth

Unemployment rate % change to unemp_rate

Number unemployed (millions) change to unemp_mil

Employment to population ratio %, age 25–54 change to emp_pop

Inflation rate % (CPI-All) change to infl_rate

Stock market S&P 500 (avg. level) change to snp500_mean

Debt held by public ($ trillion) change to public_debt

For the column with months, it should be named month

At this point, you should have a completely transposed data frame. Be aware that transposing data removes data type. This means the numerical columns are not 
considered numerical; they are character. In addition, when you attempt to convert data from character to numeric, you will encounter an error. Be sure all commas are removed from the data you are converting. The numeric data type does not allow commas. Convert everything except jobs_mth to numeric when you are ready.

You can use str_remove_all() from the Tidyverse library stringr to remove the commas from the column jobs_lvl like so:

library(stringr)

covid_data_t$jobs_lvl = str_remove_all(covid_data_t$jobs_lvl, ',')

Then convert the data type from character to numeric except for jobs_mth. If you want more explanation, look at the R file.

Save the data file as a tab-dilimited file under the directory data. Name it scraped_data.txt. Link it in submission.md.

Data Wrangling and Manipulation 

It is time to practice your data wrangling skills with R. Read your newly created data file back into R as a new data frame. Please perform the following tasks. Take a screen capture of your output and link it in submission.md.

Select all columns that start with a j or contain an a (ignore case). Save it as a new data frame. 

Using your newly created data frame, select data in which jobs_lvl was greater than 135,000. 

Using pipes %>%, perform the previous two operations together and save it as a new data frame. This means you should select columns that start with a j or contain an a (ignore case) and select months in which jobs_lvl was greater than 135,000. 

Let's perform some other tasks. Ignore the queries you just performed.

Calculate the mean of jobs_lvl. 
Calculate the median of jobs_lvl. 
