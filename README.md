# Assurity_Assessment
Contents of the Repository
1) Instructions to Execute the Script
2) Script
3) Data File
4) Output File (Remove this file to test its generation while executing the script)
5) A doc file Working Script Snapshots.docx
6) Raw Results file
7) HTML Report
8) Test Results Summary

******************************************Start:Instructions to Execute the Script******************************************
1) Script Name:- Asurity_Assessment.jmx
2) Data File Name:- p_Category_ID.csv

Place the script and the data file at the root folder of Jmeter "apache-jmeter-5.6.3\Assessment"
Start Jmeter from the bin directory - click on "ApacheJMeter.jar". This will start Jmeter in GUI Mode
Load the Script into Jmeter and Update the paths for the test data and output files at the below location
1) Dynamic User Defined Variables
     Update:- p_CategoryID_CSV_Path, p_CategoryID_Random_CSV_Path, p_OutputFile_Path -Instructions are also added to the script
2) Command to execute the script - (Please update the respective paths in the command below
     jmeter -n -t "{Path To Script Directory}\Asurity_Assessment.jmx" -l "{Path TO Results Directory}\results.jtl" -e -o "{Path TO Results Directory}\HTMLReportNew" -Jjmeter.save.saveservice.response_data.on_error=true

3) Command to pass run time varibles to the script if you want to test for more number of threads or anyother throughput. You can update the values for Threads, RampUp, LoopCount, StartUpdelay and Target Throughput. This command meets the ask for Modularity "The test execution parameters should allow for changing ramp-up, steady state, throughput and VUser (Thread) count."
   
   jmeter -n -t "{Path To Script Directory}\Asurity_Assessment.jmx" -l "{Path TO Results Directory}\results.jtl" -Jp_Threads=5 -Jp_RampUp=5 -Jp_LoopCount=2 -Jp_Duration=60 -Jp_StartUpDelay=0 -Jp_TargetThroughput=10

PS:- 
1) Delete the file "Categories_Promotions.csv" when you keep all the files at the Jmeter root location to see its generation. If you don't delete it will append to the existing file.
   
2) The script is readable and comments are added to each element within the scripit and should make it easy to make any changes and check why its been done in a particular way
   
3) The test has been configured to meet all the criteria listed below
   
   NFR-01	Test should support Vusers (Threads) half the count of Category IDs shared in Test Data
   NFR-02	The test should ramp up at one VUser (Thread) per second
   NFR-03	Test should achieve 10 API calls in total for the 1-minute Steady State duration
   NFR-04	90 percent of the times the API is expected to perform within 500 ms

5) The script meets all the functional requirements as stated below
   1) Functional Requirements
      API: https://api.tmsandbox.co.nz/v1/Categories/6327/Details.json?catalogue=false 
      Test Data: (Category ID: 6327, 6328, 6329, 6330, 6331, 6332, 6333, 6334, 6335, 6336)
   2) Test Assertion:
        a) Check for the Response Status
        b) On a successful response, validate the response with below criteria:
              Parameter Check: Category ID
              Text Check: "CanRelist": true
    3) Print following values in a csv file: Category ID, Name, Path, Promotion ID, Price
       Please print all Promotion IDs and respective Prices per Category ID

 6) Working Script Snapshots.docx file contains the proof of execution during the script development

******************************************END: Instructions to Execute the Script********************************************

******************************************Start: Test Results Summary********************************************************


