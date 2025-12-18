# ZillowETLProject

![ETL Diag](https://github.com/user-attachments/assets/7fc083bf-3637-4242-834a-21c406c2436a)

This is an ETL project using AWS and automated using Apache Airflow.
This project integrates Zillow's data through RapidAPI to streamline the ETL (Extract, Transform, Load) process. Zillow provides us with real estate data which we will further transform and analyze.


First of all, we initialize our EC2 instance so that can run apache airflow on it. This will help us with our automation and orchestrating the workflows. It ensures each step in the pipeline runs smoothly and handles dependencies, retries, and scheduling. We will create a DAG(Directed acyclic graphs) to make the process efficient  

![Screenshot 2024-11-16 214609](https://github.com/user-attachments/assets/b720f47a-2a74-4933-a0e0-c37359c9b1af)

![Screenshot 2024-11-16 214818](https://github.com/user-attachments/assets/a471ef66-d518-43d9-bf39-29542fb1ca63)


Data is fetched from Zillow's API using the script analytics.py. This script is responsible for collecting the required information and preparing it for further processing. The data is collected in a csv format.
   
 ![Screenshot 2024-11-16 215145](https://github.com/user-attachments/assets/4d8fb853-015c-49d7-bd81-89883a695200)



We create a landing S3 bucket for the extracted data. The "landing" Amazon S3 bucket, serves as the first checkpoint for raw data.

  ![Screenshot 2024-11-16 215413](https://github.com/user-attachments/assets/47b09143-fc4c-4986-b384-d75025c6f5d4)



We create another S3 'intermediate' bucket for our data. This will be the midpoint for our data. We can add security checks on our data here.

![Screenshot 2024-11-16 215408](https://github.com/user-attachments/assets/e42db1a4-028b-4f37-8def-c29e962c5d61)

  
A Lambda Function Trigger is created for the intermediate bucket.
An AWS Lambda function is triggered whenever new data lands in the S3 bucket. This function processes the raw data and transfers it to an intermediate bucket. The intermediate bucket holds pre-processed data, ready for further transformations or validations.

![Screenshot 2024-11-16 215332](https://github.com/user-attachments/assets/7aedafe8-f0ec-4425-a31d-1dc9389d0fb2)


The final s3 bucket is created which holds the cleaned, and formatted data, which is now ready for loading into the target database.

![Screenshot 2024-11-16 215404](https://github.com/user-attachments/assets/c69e2f00-f69b-4e9f-9df3-656973e68ad1)

We create a lambda trigger which takes the data from the intermediate bucket, performs any necessary transformations, and loads the cleaned data into the "transformed data" S3 bucket. We use the pandas library to clean and transform data

![Screenshot 2024-11-16 215340](https://github.com/user-attachments/assets/7a59bde9-5210-4eda-9d78-f92d44a61364)

The transformed data from the final cleaned bucket is then loaded into Amazon Redshift, which acts as the data warehouse. Redshift ensures efficient querying and analysis of the processed data. We create a redshift cluster of the node type 'dc2.large' having number of nodes as 1.


![Screenshot 2024-11-16 193807](https://github.com/user-attachments/assets/f2250eae-1e0f-4f21-bbce-a85c4ac5e384)

![Screenshot 2024-11-16 214642](https://github.com/user-attachments/assets/728c8b1b-1959-4247-bed0-a3867d2983c4)

We write an sql query in redshift to get the Data.

![Screenshot 2024-11-16 214622](https://github.com/user-attachments/assets/673d0f5e-f8e1-40dc-8be2-242ef8ffb158)

Finally, Amazon QuickSight is used to create dashboards and visualizations, enabling stakeholders to gain insights from the data.
![Screenshot 2024-11-16 214119](https://github.com/user-attachments/assets/5fb9be1f-9c60-4cba-adc7-a63b8635c087)

![Screenshot 2024-11-16 214156](https://github.com/user-attachments/assets/6e65acc0-e31e-4613-a81c-18fba0467840)

![Screenshot 2024-11-16 214536](https://github.com/user-attachments/assets/b104f6dc-cd23-4c33-b997-0639ff29ddf7)

![Screenshot 2024-11-16 214422](https://github.com/user-attachments/assets/048b1042-9c05-481d-a234-50a6f9c2d0f8)











