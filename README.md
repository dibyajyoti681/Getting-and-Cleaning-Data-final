The script assumes it has in it's working directory the following files and folders:
* activity_labels.txt
* features.txt
* test/
* train/

The output is created in working directory with the name of tidy2.txt

*Note:* the R script is built to run without including any libraries for the purpose of this course.

## run_analysis.R walkthrough
It follows the goals step by step.

* Step 1:
* Read all the test and training files: y\_test.txt, subject\_test.txt and X_test.txt.
* Combine the files to a data frame in the form of subjects, labels, the rest of the data.

* Step 2:
* Read the features from features.txt and filter it to only leave features that are either means ("mean()") or standard deviations ("std()"). The reason for leaving out meanFreq() is that the goal for this step is to only include means and standard deviations of measurements, of which meanFreq() is neither.
* A new data frame is then created that includes subjects, labels and the described features.

* Step 3:
- * asd 
  * Read the activity labels from activity_labels.txt and replace the numbers with the text.
 
 * Step 4:
  * Make a column list (includig "subjects" and "label" at the start)
  * Tidy-fy the list by removing all non-alphanumeric characters and converting the result to lowercase
  * Apply the now-good-columnnames to the data frame
  
 * Step 5:
  * Create a new data frame by finding the mean for each combination of subject and label. It's done by `aggregate()` function
  
 * Final step:
  * Write the new tidy set into a text file called tidy.txt, formatted similarly to the original files

