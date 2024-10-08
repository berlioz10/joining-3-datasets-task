# Assignement

In this assignment, we will try to merge 3 different datasets which contains details about different companies. The datasets are 3 csvs as it follows:
 - google dataset.csv
 - facebook dataset.csv
 - website dataset.csv

## Problems and how they were solved:

### Reading the datasets:
 - The datasets contained these characters **\\"**, which was really a bit difficult to solve it, but finding a solution by changing the characters with a keyword, and at the end the keyword is replaced with only a quote (**"**).
 - The website dataset alo had a format issue, because a lot of texts started with one quote but finished with two or three quote marks, which was, again, really annoying to solve. I replaced the quote marks with only a single one by using a simple regex to find them.
 - a funny error, all of my phone numbers were considered float, which I did not think it would be a really good idea, so I changed them to string, which is easier to use from some perspectives, and also it contains zeros, pluses, paranthesis, and I am not sure how those are represented.

### Merging the datasets
 - in the datasets, firstly I started by analysing the sitation and how it would be better to be approached
 - by looking at the infos (I used pandas library, info() function shows me the columns, their types and also how many non-nulls elements I have) for each table, instantly I saw that the only element which is found in all of the datasets is domain, so I thought that the element maybe would be useful for joining the tables.
 - for the other columns which I considered important (category, name, and phone number), I made an analysis to see what can be obtained from them:
    - category: facebook had the most detailed one, because instead of one element, it has a list of elements of categories. Website dataset contained the most of the categories, and also all the categories from google and facebook were found entirely in website categories.
    - name: some companies in the datasets do not have names, or there are duplicates name (makes sense with the example of Volkswagen, which can be found with the same name, but in different countries)
    - phone number: as it can be saved in different formats, I removed the space, paranthesis, lines and plus sign for having a better perspective. There were missing phone numbers and also duplicated phone numbers (probably a person made two companies, or the phone numbers are from different region, which I highly doubt).
    - domain: facebook and website have all unique domains and all of them are non-null (except one from website, which is an empty line). Only 13 of the domains from wesite dataset were not found in facebook, but also they were strange formated, so those 13 rows were ignored. Also, google dataset has no different domains from the ones seen in Facebook dataset, but 15 thousand of them are duplicated. 
 - Continuing with the merging of the datasets, I considered that the best solution would be to use the domain column. Firstly, only those who are unique in all of the three tables, and after that the ones from facebook and website dataset which are duplicates in google dataset. We also made a similarity test, but only with name and phone number, to identify if there is a company from google dataset which can be similar with the one from facebook or website dataset. After that, I took the rest from the google dataset which have duplicated domains and are not seen in facebook and website, and I put them as they are.
 - The conflicts of each element were solved by a simple majority vote, which determined which one is the most suitable for being chosen. The only conflicts I met were at the unique domains from facebook and website dataset but duplicated at google dataset, in which I only chose to create two rows instead of one, due to not being similar. 
 - For the similarity of data, even though I did not implement anything in that manner, I think that a solution, in the cases of the name of the company, is to use the longest name, as it gives more details about the company, but it is really difficult to choose. For example, there were three names something like "X", "X ltd" and "X ltd.". All of them are fine by me, but I think the most suitable one would be "X ltd." as it defines that it is a limited company (I am not sure about the dot though).

### Conclusions
 - From the perspective of accuracy, these were not taken into consideration:
    - the 13 rows from website dataset.
    - the duplicates of a company in the last step with the google dataset.
    - the ones from the first step which had the same domain: they were not tested with a similarity score, which obviously could decrease the accuracy
 - If I could not have used the domain column, i probably would have made a combination between country + name + phone number, and I would have observed which one is better suited to be considered the company of the other.
 - Ofc, I would have tried to solve it with a simple:
    > "SELECT * FROM D1, D2, D3 INNER JOIN domain1 ON D1.domain = D2.domain INNER JOIN domain2 on D2.domain = D3.root_domain"
    
    but it would have been impossible to increase the accuracy after, so I consider that I made my merging manually.
 - From my perspective, the task was really interesting, I really understand better how  difficult it is to work with more datasets from different perspectives, as each one has different view on how to save specific elements.
 - I used Jupyter Notebook with Python 3.8, and the main library used is `pandas`.
