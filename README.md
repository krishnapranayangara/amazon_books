amazon_books

![image](https://github.com/user-attachments/assets/a54b4a63-213e-4bc9-8071-32c4acd51350)

ETL Operation Overview: Fetching Amazon Books and Storing in PostgreSQL
This project outlines the implementation of an ETL (Extract, Transform, Load) pipeline designed to fetch book data from Amazon and store it in a PostgreSQL database. Utilizing Apache Airflow for workflow orchestration and Docker for environment consistency, this pipeline automates the data ingestion process, focusing on books relevant to Data Engineering and Machine Learning.

1. Data Extraction
The extraction process is executed through a Python script integrated into an Apache Airflow DAG (Directed Acyclic Graph). The get_amazon_data_books function sends requests to the Amazon search results page for machine learning books, leveraging the Beautiful Soup library to parse the HTML content. Key attributes extracted from the book listings include:

Title
Author
Price
Rating
To ensure data integrity, we maintain a set of seen titles to avoid duplicates during the extraction process. The extracted book data is stored as a list of dictionaries and then converted into a pandas DataFrame for further processing.

2. Data Transformation
In this implementation, the transformation phase is minimal, focusing primarily on data cleaning. The pipeline removes duplicates based on the title of the books, ensuring that only unique entries are loaded into the database. This transformation step is integral in maintaining a clean dataset that reflects accurate book information.

3. Data Loading
Once the data is transformed, it is loaded into a PostgreSQL database using the insert_book_data_into_postgres function. This function retrieves the book data from Airflow's XCom and uses the PostgresHook for seamless integration with the PostgreSQL database. The books table is created with the following schema:

id (Primary Key, Serial)
title (Text, Not Null)
authors (Text)
price (Text)
rating (Text)
The insertion of records is performed using parameterized SQL queries to prevent SQL injection and ensure data integrity.

4. Workflow Scheduling with Apache Airflow
The entire ETL process is orchestrated using Apache Airflow. The DAG is configured with the following tasks:

fetch_book_data: Executes the data extraction from Amazon.
create_table: Creates the books table in PostgreSQL if it does not already exist.
insert_book_data: Loads the extracted data into the books table.
This automated workflow is scheduled to run daily, ensuring that the database is consistently updated with the latest book data.

5. Containerization with Docker
To enhance the deployment process, the entire application is containerized using Docker. This approach ensures that all dependencies and configurations are encapsulated within the Docker container, providing a consistent environment for running the ETL pipeline across different systems.

Conclusion
This ETL operation effectively streamlines the process of gathering and storing book data from Amazon into a PostgreSQL database. By leveraging the capabilities of Apache Airflow for scheduling and Docker for environment management, we have created a reliable and maintainable data pipeline.

Future Enhancements
Incremental Data Loading: Implement mechanisms to fetch and load only new or updated book records to improve efficiency.
Data Quality Checks: Introduce validation steps to ensure data integrity and accuracy before loading into the database.
Broader Data Sources: Expand the pipeline to include data from other platforms, enhancing the dataset's breadth for research and analysis.
This structured ETL approach not only supports efficient data management but also enables further data analysis and insights within the domains of Data Engineering and Machine Learning.

![image](https://github.com/user-attachments/assets/fad37b7c-cbb7-4761-98d6-4644e3ca03b5)
