![alt text](https://github.com/brevinowens/solvemined/blob/main/Resources/solvemined.png?raw=true)

## Project Proposal:

### Question from the client: "Due to the recent decision by Princeton to increase the amount of financial aid offered to low income students, we would like to know what is the relationship, if any, between the enrollment of students by state and the revenue of public instiutions? Additionally, does financial aid impact enrollment henceforth affecting the revenue of these institutions?"  

### Data source 1:
National Center for Education Statistics (NCES)
#### Description:
link: [Revenue](https://nces.ed.gov/ipeds/TrendGenerator/app/build-table/6/13?rid=6&ridv=1%7C2%7C4%7C5%7C6%7C8%7C9%7C10%7C11%7C12%7C13%7C15%7C16%7C17%7C18%7C19%7C20%7C21%7C22%7C23%7C24%7C25%7C26%7C27%7C28%7C29%7C30%7C31%7C32%7C33%7C34%7C35%7C36%7C37%7C38%7C39%7C40%7C41%7C42%7C44%7C45%7C46%7C47%7C48%7C49%7C50%7C51%7C53%7C54%7C55%7C56&cid=25)

This table which we were able to download into a .csv file contains revenue data by state of the institution over the last 15 or so years. The data shows how these colleges were making revenue, for example whether the institution were recieving federal grants, local grants, etc. 

### Data source 2:
NCES
#### Description:
link: [Financial Aid](https://nces.ed.gov/ipeds/TrendGenerator/app/build-table/8/25?rid=6&ridv=1%7C2%7C4%7C5%7C6%7C8%7C9%7C10%7C11%7C12%7C13%7C15%7C16%7C17%7C18%7C19%7C20%7C21%7C22%7C23%7C24%7C25%7C26%7C27%7C28%7C29%7C30%7C31%7C32%7C33%7C34%7C35%7C36%7C37%7C38%7C39%7C40%7C41%7C42%7C44%7C45%7C46%7C47%7C48%7C49%7C50%7C51%7C53%7C54%7C55%7C56&cid=46)

This table is showing the number of full-time and first-time students that recieved financial aid, by state and year. It also shows which type of financial aid that they recieved.

### Data source 3:
NCES

#### Description: 
link: [Enrollment](https://nces.ed.gov/ipeds/TrendGenerator/app/build-table/2/2?rid=6&ridv=1%7C2%7C4%7C5%7C6%7C8%7C9%7C10%7C11%7C12%7C13%7C15%7C16%7C17%7C18%7C19%7C20%7C21%7C22%7C23%7C24%7C25%7C26%7C27%7C28%7C29%7C30%7C31%7C32%7C33%7C34%7C35%7C36%7C37%7C38%7C39%7C40%7C41%7C42%7C44%7C45%7C46%7C47%7C48%7C49%7C50%7C51%7C53%7C54%7C55%7C56&cid=9)

This last table shows the number of students enrolled in schools by state and student level.

### We will be using a nonrelational data base as we were able to combine all the data into one dataframe. One database seemed the most logical as there are not ids for every data entry, either by state, some form of student id, or yearly id. 

## Extract:
On the NCES website, we found under Data & Tools > Downloads Microdata/Raw Data > IPEDS Data Center > Data Trends, a trend generator that allowed us to select information from specific questions for further examination.  

The questions we selected were meant to help compare revenue received by public institutions by state and the amount of financial aid states and institutions gave to students. Based on this premis, the following questions were chosen:
1. Institutional Revenues: What are the revenues (in thousands) of public postsecondary institutions using GASB standards?
2. Financial Aid: How many full-time, first-time students were awarded financial aid?
3. Student Enrollment: How many students enroll in postsecondary institutions annually?

Once the question was selected, we were able to use the "build table" feature of the trend generator.  From there, we compared each state to the source of funds (for Revenue), type of aid (for Financial Aid), and student level (for Enrollment) and downloaded the results in the form of CSV files for all available years.

## Transform:
The original data files downloaded from the NCES were in the form of CSV files. They each had four "header" rows, only one of which needed to be read through and included in the data frames created.

We then started by looking at the revenue data specifically. We noticed a lot of redundancy in the columns and decided to drop some as they were not necessary for our analysis. We then dropped the last four rows of the data frame as they contained footers referencing the source information. After this, we dropped the total row in the data frame that appeared at the start of every year. To do this, we first set the index to “State” and were then able to perform a simple drop function. We then reset the index for further data cleaning. Next, we removed all dollar signs ($) signs and commas from the data frame by using the replace function and simultaneously converted the data type to an integer, except for the ‘State’ and ‘Year’ columns. We then reordered the columns and changed them to simpler column names while not losing any description that is necessary.

We then repeated this process for our financial aid and enrollment data. One extra step that we had to perform here was to drop two years of data in the financial aid dataset and three years of data for the enrollment dataset as it was not contained in the revenue dataset.

Our final step was to merge all three data sets to be used in our Mongo database by using a nested merge function.

## Load:
As stated previously, we chose to do a non-relational database (MongoDB) so loading the data frame into the database was pretty straightforward. First, we set up a connection to our local host. Then a connection to mongo client was established, after which we were able to create a new database and collection. Lastly, we inserted our data frame into the database in the form of a dictionary.