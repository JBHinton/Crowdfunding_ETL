# Crowdfunding_ETL

## Overview:

This repository examines the following csv files:

<ol>
  <li><a href="code/Resources/contacts.xlsx">Contacts</a></li>
  <li><a href="code/Resources/crowdfunding.xlsx">Crowdfunding</a></li>
</ol>

We utilize the notebook [Start_Code](code
/Start_Code_SWoodbury_JHinton_MAshby.ipynb) To transform the data using Pandas, Python and Regular Expressions.

## Part 1: Create the Category and Subcategory DataFrames

In this section, we use Pandas and Python to extract and transform data from the [crowdfunding](code/Resources/crowdfunding.xlsx) file for export into the [category.csv](code/Resources/category.csv) and [subcategory.csv](code/Resources/subcategory.csv) files. 

The crowdfunding data is read into a pandas dataframe. From this dataframe a category and subcategory are split into seperate columns from the existing category & subcategory column using <code>crowdfunding_info_df['category & sub-category'].str.split('/', n=2, expand=True)</code>. From here, each unique category and subcategory were cast to a list, then assigned category/subcategory ID's with the list comprehensions: 

<ul>
  <li><code>cat_ids = ["cat" + str(x) for x in category_ids]
</code></li>
  <li><code>scat_ids = ["subcat" + str(x) for x in subcategory_ids]
</code></li>
</ul>

Each resultant list with id and name (category and subcategory) were assigned to a dataframe and exported to the [category.csv](code/Resources/category.csv) and [subcategory.csv](code/Resources/subcategory.csv) files. 

## Part 2: Campaign Dataframe:

In this section, we used Pandas and Python to ETL from the crowdfunding.xlsx file. First, the columns: "blurb", "launched_at" and "deadline" were renamed to: "description", "launched_date" and "end_date", respectively, ultilizing the following code:

<code>campaign_df.rename(columns={"blurb":"description",\
            "launched_at":"launched_date", "deadline":"end_date"},inplace=True)</code>

Next, the 'goal' and 'pledged' columns were cast as float64:

<code>campaign_df[['goal','pledged']] = campaign_df[['goal','pledged']].astype('float')
</code>

The 'launched_date' and 'end_date' columns were converted to datetime:

<code>campaign_df["launched_date"] = pd.to_datetime(campaign_df["launched_date"], unit='s').dt.strftime('%Y-%m-%d') 
campaign_df["end_date"] = pd.to_datetime(campaign_df["end_date"], unit='s').dt.strftime(</code>

Finally, the category and subcategory files were merged with the campaign_df, after which, the 'staff_pick','spotlight','category & subcategory', 'category' and 'subcategory' columns were dropped, and the resultant dataframe was exported to [campaign.csv](code/Resources/campaign.csv)

## Part 3: Extract contacts.xlsx Data

In this section, two approaches were used to extract and transform data from contacts.xlsx. The first approach utilized Pandas, the second extraction using RegEx. In both cases the following process was followed:

<ol>
  <li>The contacts DataFrame was created</li>
  <li>The 4 digit contact_id was extracted, converted to int64 and assigned to a column within the DataFrame</li>
  <li>The name and Email were extracted and assigned to a column within the DataFrame</li>
  <li>The name column is split to first_name and last_name, assigning each a separate column in the DataFrame</li>
  <li>Columns were reordered, the name column was dropped, and the resultant DataFrame was exported to <a href="code/Resources/contacts.csv">contacts.csv</a></li>
</ol>

## Part 4: SQL Database

In this section, we used the cleaned csv files from the above sections to create a Postgresql Database. 

First, an [ERD](sql_schema_table_screenshot/ERD_sql.png) was created to visualize the relationships between the entities. The SQL from this ERD was exported to: [crowdfunding_db_schema.sql](sql_schema_table_screenshots/crowdfunding_db_schema.sql).

This SQL file was uploaded into the campaign_db Database. The CSV files from Parts 1-3 were imported into the schema, and the database was queried with SELECT statements, resulting in the following:

<ul>
  <li><a href="sql_schema_table_screenshots/catrgoy_table.png">category_table</a></li>
  <li><a href ="sql_schema_table_screenshots/subcategory_table.png">subcategory_table</a></li>
  <li><a href = "sql_schema_table_screenshots/contacts_table.png">contacts_table</a></li>
  <li><a href = "sql_schema_table_screenshots/campaign_table.png">campaign_table</a></li>
</ul>



