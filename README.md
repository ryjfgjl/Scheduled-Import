# Scheduled Import of Excel into Database


Earlier, we introduced the method of [importing Excel into the database with one click](https://medium.com/@ryjfgjl.zhang/one-click-importing-excel-data-into-a-database-61728fdf5241), which eliminates the cumbersome steps of importing Excel into the database and solves various problems that may be encountered in the process. We also introduced the method of [bulk importing multiple Excel files into the database](https://dev.to/ryjfgjl/batch-import-of-multiple-excel-files-into-the-database-45ac), which enables unattended bulk import.
Now we will introduce how to achieve scheduled import and achieve full automation.

## Example
As shown in the figure, we have a table called Product Information, which records all the product information of the company.  When there are new products, data will be added to the table.  When there are changes in product information, the table data may also be updated.
Now, we need to synchronize the data in this table with the database, and when the Excel data is updated, we can update the data in the database on a regular basis.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mfctc5zusy3q50focykc.png)



Step 1: Create a product information table in the database
Use the ExcelToDatabase tool to [import Excel to the database with one click](https://dev.to/ryjfgjl/one-click-importing-excel-data-into-a-database-2j02).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iexypk4qovavw97zksvu.png)



![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9t2wnawz7hbw4f3lj8w2.png)


Step 2: Decide on the update method
There are generally two ways to update data: full update and incremental update
1. Full update: This involves deleting all previously imported data from the database table and then importing all the data from the Excel table. This approach is simple and robust, ensuring consistency between the Excel and database data. However, it can consume more resources and time when the Excel data volume is large or updates are frequent. It is generally suitable for scenarios where the data volume is small or updates are infrequent or where historical data retention is not required.
2. Incremental update: Based on the existing data in the database table, only the newly added or modified data in Excel is updated. This method requires a fixed unique identifier for each row of data to distinguish between data, such as the product ID here.
Now we will introduce two update methods.

**Full update**
We will first save the import configuration we just created and name it product information.  The target table can be the table name we just imported and generated.  The import mode can be selected as overwrite. Click save

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xhe4heqhfaft1swd9izz.png)



In the software toolbar - scheduled tasks - add a new task, add a scheduled task called product information - full update (here we leave the default settings for the scheduled task and add them later)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4vpo8lf13shbx0he3257.png)



Edit the Excel file by deleting, adding, and modifying a data record to test the effect.
The image below shows the data in the database table before modification. We will delete Product 10, change the unit price of Product 1 to 99, and add a new record for Product 11.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ldsu9daa523ofcdh10us.png)


Check the database table data again, it has been updated

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o7ya5p367e6d5xupv58g.png)



**Incremental Update**
If a full update is in progress, stop it first.

Here, we choose the product id as the unique identifier for the data and set it as the primary key in the database table.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2dqqdzklcutzxshuks5s.png)


Select the update option for import mode and click Save

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/03nnhxze3ehtavrtfsp2.png)


Add a new scheduled task called Product Information - Incremental Update

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3f8c5cykox1w73481441.png)


Modify a data record to test the update. Here, we'll change the price for Product 105 to 85 and add a new record for Product 20.
Before modification, the database table looks like this:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gs45kl24fio8jlb93ug3.png)



Modify and save the excel as shown in the figure

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/p5pmaiob96zqvksinnln.png)


Check the database table again, it has been updated

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cdf6cl9988e6gh7ren3c.png)


**Scheduled Task Settings**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z4ccjy08vdldufkdols4.png)


Sub Task Type: Select the saved task or configuration.
Start/End Time: Specify the effective period of the scheduled task.
Month: Can be filled from 1 to 12.
Week: Can be filled from 1 to 7, representing Monday to Sunday.
Day: Can be filled from 1 to 31.
Hour: Can be filled from 0 to 23.
Minute: Can be filled from 0 to 59.
Second: Can be filled from 0 to 59.

Using minutes as an example:
Every Minute: The task runs every minute, i.e., it runs at every minute from 0 to 59.
Every n Minutes: Enter 3 to run the task every 3 minutes, for example, it runs at the 2nd, 5th, 8th, 11th, etc., minutes.
At the nth Minute: Specify which minute the task should run, such as running at the 0th minute.

For example, to run the task every day at 9:00 AM:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e1raclzl2pdech61e2ig.png)




You can view the basic information of real-time scheduled tasks in the scheduled tasks interface:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6dqdkwy0jpcw7i0oncni.png)


Note: The scheduled task requires the software to run all the time.  When you click x to close the software interface, the software will default to immediately exit and stop the scheduled task. You can check Hide in tray area instead of directly exiting the program in the software menu bar - Tools - Settings. After hiding, you can right-click the program icon in the tray area and choose Open main interface or Exit program.


# How to add ExcelToDatabase to boot
Applications that require running scheduled tasks are usually placed on servers that run 24 hours a day without interruption, but if the scheduled tasks are run on your computer, you may need to restart the computer every day. In this case, we can add the program to the startup process, and the scheduled tasks will automatically run after the program is started.
Step 1: Create an exe shortcut

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/35f11qz5nyjlgxsp2mb4.png)


Step 2: Place the shortcut in the Windows startup folder, and the program under this file will start up with the computer.
My computer path is: C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0a9kazkksb4yyrqqopdt.png)

# Other timing methods
Using the built-in scheduled tasks in ExcelToDatabase requires that the program be kept open.  Exiting the program will automatically stop the scheduled task from running. If you want to run the scheduled task in the background without opening the program, you can have other specialized scheduled task programs call the API provided by ExcelToDatabase.
For example, the task scheduler that comes with Windows provides a background running mode without a graphical interface and can be automatically launched when the computer is turned on.

The usage of API can refer to the following description


## Introduction and Download of ExcelToDatabase
- [ExcelToDatabase - Automation tool for batch importing Excel files into database](https://github.com/ryjfgjl/ExcelToDatabase/blob/master/README.md)
 
