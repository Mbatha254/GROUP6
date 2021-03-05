# GROUP6
**Introduction**

The objective of our project is for a first part, to set a risk index for every district in the greater Sydney region, based on the district population and the Covid-19 cases in the area, and secondly to route the way to a specific business based on a previous selection, so in the process will be:

- Setting the coordinates (x, y) for a random point in Qgis.
- Selecting the district (for the previously selected point), the population and Covid-19 cases from different tables and using them to calculate the risk index for the district selected.
- Displaying the business types in the area for the same district from the businesses table, then selecting a specific business after choosing the type.
- Setting the shortest path to the business in need from our previously selected starting point.

**Tools and data**

- QgsMapToolEmitPoint a tool that displays an (x, y) for every click on Qgis.
- psycopg2 a tool that creates a connection between Postgres to Python.

The programs that we will need are:

- Qgis, which is an open-source cross-platform desktop geographic information system application that supports viewing, editing and analysing of spatial data.
- PostgreSQL, also an open-source relational database managing system emphasizing extensibility and SQL compliance.
- A 3 or further version of Python, which is an interpreted, general purpose programming language.
- Visual Studio Code, a freeware source-code editor made by Microsoft. Support debugging, highlighting, intelligent code completion and more.

The program demo is done using windows, but the tools and programs used are also available for Mac or Linux users.

All the data is downloaded from the abs.gov.au , data.nsw.gov.au and stat.data.gov.au websites for the Australian New South Wale&#39;s region, as csv files.

**Tutorial**

To avoid any connectivity problems, download the population file, the districts file and join them using the (x, y). then the Covid-19 cases file, and later the businesses in the New South Wales&#39;s region file.

The next step is loading data into the database, so we create a new database inside our server in PgAdmin4, and we connect it with our user and password previously created when downloading Postgres. We load all the tables into our database and modify or visualise the geometry.

Connect python to our database using psycopg2 by coping the code and replacing the credentials (user, password, host, port, database).

Connect the database to Qgis and load it into the canvas, call the QgsMapToolEmitPoint tool previously insalled in the terminal, so that we can have (x, y) for every click on the map.

Copy paste the code and replace the (x, y) manually, run it. It will give us the first 2 querys:

SELECT population , sa2\_name , Postcode FROM public.&quot;GREATERSYD&quot; as g WHERE g.x=151.1093572616734 and g.y=-33.878140952011805 LIMIT 1

SELECT covid\_cases FROM public.&quot;covid\_cases&quot; as c WHERE c.x=151.1093572616734 and c.y=-33.878140952011805  LIMIT 1

Copy the population (result[0]) and Covid cases (covid\_result[0])

and paste it in the third query:

RI = ((int(covid\_result[0] \* 10) / 28937) + ((int(result[0]) \* 10) / 8164000)) / 2

Now that we have our Risk Index for the district we can copy it&#39;s postal code and (result[2]) and paste it in the fourth queryto have the businesses types in the area:

SELECT DISTINCT industry FROM public.&quot;businesses&quot; as b WHERE b.Postcode=2134

After seeing the types available we need to search for the type needed so we type it in the next query, for example &quot;gyms&quot;:

&quot;businesses&quot; as b WHERE b.industry= &#39;Gyms&#39;  and b.Postcode=2134

So now it will give us all the gyms in the district, next we choose a gym, copy the name and paste it in the next query, example &quot;Viva Health Club&quot;:

SELECT longitude , latitude FROM public.&quot;businesses&quot; as b WHERE b.name= &#39;Viva Health Club&#39;

Finally, we have the coordinates for our destination, all that&#39;s left is to copy it and paste it in the shortest path point to point tool in Qgis, our starting point would be the first selected point on the map. We add our open street map and run the tool.
