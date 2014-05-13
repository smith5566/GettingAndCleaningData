##run_analysis.R script
=======

This script reads files from the Samsung data set as downloaded from the 
*Getting and Cleaning Data* Course Project page. The script uploads the tables into R,
merges the tables from the train and test data sets, labels the columns with the
measurements descriptions, and adds columns to label the activity and subject.
The means and standard deviation measurements are extracted.
The activities are transformed from the numerical value to the activity description.
The measurements are transformed to a more descriptive label and altered to
match R formatting guidelines **according to class lectures**. The averages 
are calculated for each measurement for each subject and activity. In the final steps
the script writes the tidy data set to a .txt file.

This markdown file can be read in conjunction with the run_analysis.R script. Each 
sub-heading below will correspond to a hashtag commentary heading preceding a theme 
of scripts. 

Create objects below are in **bold**.

Install/Upload packages:
------------

The first steps of the code install the reshape2 packages for the necessary melt()
and dcast() functions. 

Read relevant files into R
------------
The second section of script reads the neccessary files into R:

	*x_train.txt: Contains the data from the train group. Creates a data frame (**x_train**) of 7352
	observations of 561 variables.
	*x_test.txt: Contains data from the test group. Creates a data frame (**x_test**) of 2947
	observations of 561 variables.
	*y_train.txt: Contains the numerical value of the activity for each row. Corresponds 
	to the number of rows in **x_train.txt**. Creates a data frame (**y_train**) of 7352 observations of 1 variable.
	*y_test.txt: Contains the numerical value of the activity for each row. Corresponds
	to the number of rows in **x_test.txt**. Creates a data frame (**y_test**) of 2947 observations of 1 variable. 
	*features.txt: This file contains the names of the measures to be used as columns
	names. Number of columns matches both **x_train.txt** and **x_test.txt**. Creates a data frame
	(**features**) of 561 observations of 1 variable.
	*subject_train.txt: Contains the subject value for each row. Corresponds to the
	number of rows in the **x_train.txt** file. Creates a data frame (**subject_train**) of 7352 observations of 1 variable.
	*subject_test.txt: Contains the subject value for each row. Corresponds to the
	number of rows in the **x_test.txt** file. Creates a data frame (**subject_test**) of 2947 observations of 1 variable.
	
Create merged file
------------
The third section of script merges the **x_train** and **x_test** data frames. The rbind() functions
appends the **x_test** data frame to the **x_train** data frame by rows. Creates a data frame (**data**)
of 10299 observations of 561 variables (**x_train**: 7352 rows + **x_test**: 2947 rows = 10299 rows).

Label features
------------
The fourth section of script uses the features listed in the **features** data frame to create
column labels for the **data** data frame. The features in **features** are first extracted
to a character vector **features_names**.

Extract mean and standard deviation columns
------------
The fifth section of script extracts the variables of the means and standard deviation as per
the assignment requirements from the Course Project page. Any column name containing the string
"mean" or "std" was selected. First creates a list of columns meeting this criteria (**extract_columns**)
then creates a data frame of the extracted columns (**extract_data**).

Clean features names
------------
The sixth section of script transforms the column names to match R formatting guidelines as described
in the *Getting and Cleaning Data* lectures. The transformed labels are added as column names to the **extract_data**
data frame.

	* Parentheses are removed.
	* String "-std" is replaced with "standard deviation".
	* Hyphens are removed.
	* All column names are transformed to lower case letters.
	
Label activity
------------
The seventh section of script transforms adds a column from the activities code in the
**y_test** and **y_train** data frames. First a numerical vector (**activity_labels_number**) is created from a combination
of **y_train** and **y_test** using the rbind() function. An empty character vector (**activity_labels_list**)
is created. Then a loop function runs through **activity_labels_number** and transforms
the numerical values according to the following code, determined from the activity_labels.txt file.
The output of the loop feeds into the **activity_labels_list** vector.

	*1 is transformed to "walking"
	*2 is transformed to "walkingupstairs"
	*3 is transformed to "walkingdownstairs"
	*4 is transformed to "sitting"
	*5 is transformed to "standing"
	*6 is transformed to "laying"

Add activity labels to extract data
------------
The eight section of script adds the **activity_labels_list** vector as a column of the
**extract_data** data frame.

Add subject lines
------------
The ninth section of script creates a vector (**subject_list**) combining the **subject_train** and **subject_test**
data frame. The data frame is then appended to the **extract_data** data frame.

Average by subject and activity
-------------
The tenth section of script uses melt() and dcast() to create a new data frame. melt()
reshapes the data frame into a new data frame (**data_melt**) using the variables of subject
and activity and the features and the variable measures. dcast() then creates a new data frame
(**tidy_data**) of the averages of the features broken down by each subject and each activity.
For each subject there are six rows of averages, one for each activity.

Write to .txt file
------------
The last section of script writes the **tidy_data** data frame to a .txt file, tidy_data.txt,
into the working directory.
	


	



