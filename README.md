# Assurity_Assessment
Contents of the Repository
1) Instructions to Execute the Script
2) Script
3) Data File
4) Output File (Remove this file to test its generation while executing the script)
5) A word document file-  "Working Script Snapshots.docx"
6) Raw Results file
7) HTML Report
8) Test Results Summary

## Start: Instructions to Execute the Script
1) Script Name:- Assurity_Assessment.jmx
2) Data File Name:- p_Category_ID.csv

About the Script:-    
The script has a thread group named  "Assessment", within this threadgroup we have two Transaction Controller.
1) ${__threadGroupName}_00_CategoryID_API_E2E (Enabled) and
   
2) ${__threadGroupName}_00_CategoryID_${p_Category_ID}_API_E2E_Debug (Disabled)
   
The debug transaction was created to identiy the failing Categorty ID's.

Place the script and the data file at the root folder of Jmeter "apache-jmeter-5.6.3\Assessment".

1) Start Jmeter from the bin directory - click on "ApacheJMeter.jar". This will start Jmeter in GUI Mode.
   
   Load the Script into Jmeter and Update the paths for the test data and output files at the below location.

   For Dynamic User Defined Variables:

   Update paths for:- p_CategoryID_CSV_Path, p_CategoryID_Random_CSV_Path, p_OutputFile_Path - Instructions are also added to the script.
   
2) Command to execute the script - (Please update the respective paths in the command below and navigate to Jmeter bin location using cmd and then enter the below command):
   
     jmeter -n -t "{Path To Script Directory}\Asurity_Assessment.jmx" -l "{Path To Results Directory}\results.jtl" -e -o "{Path TO Results Directory}\HTMLReport"

4) Command to pass run time varibles to the script if you want to test for more number of threads or any other throughput. You can update the values for Threads, RampUp, LoopCount, StartUpdelay and Target Throughput. This command meets the dynamic load profile selection requirement "The test execution parameters should allow for changing ramp-up, steady state, throughput and VUser (Thread) count."
   
   jmeter -n -t "{Path To Script Directory}\Asurity_Assessment.jmx" -l "{Path To Results Directory}\results.jtl" -e -o "{Path To Results Directory}\HTMLReport" -Jp_Threads=5 -Jp_RampUp=5 -Jp_LoopCount=2 -Jp_Duration=60 -Jp_StartUpDelay=0 -Jp_TargetThroughput=10

PS:- 
1) Delete the file "Categories_Promotions.csv" when you keep all the files at the Jmeter root location to see its generation. If you don't delete it will append to the existing file.
   
2) The script is readable and comments are added to each element within the script and should make it easy to make any changes and check why its been done in a particular way.
   
3) The test has been configured to meet all the criteria listed below:
   
   NFR-01	Test should support Vusers (Threads) half the count of Category IDs shared in Test Data
   
   NFR-02	The test should ramp up at one VUser (Thread) per second
   
   NFR-03	Test should achieve 10 API calls in total for the 1-minute Steady State duration
   
   NFR-04	90 percent of the times the API is expected to perform within 500 ms

4) The script meets all the functional requirements as stated below:
   
   1) Functional Requirements
      
      API: https://api.tmsandbox.co.nz/v1/Categories/6327/Details.json?catalogue=false
      
      Test Data: (Category ID: 6327, 6328, 6329, 6330, 6331, 6332, 6333, 6334, 6335, 6336)
      
   2) Test Assertion:
      
        a) Check for the Response Status.
      
        b) On a successful response, validate the response with below criteria:
              Parameter Check: Category ID
              Text Check: "CanRelist": true
      
    3) Print following values in a csv file: Category ID, Name, Path, Promotion ID, Price
       Please print all Promotion IDs and respective Prices per Category ID.

 5) Working Script Snapshots.docx file contains the proof of execution during the script development.


## Performance Test Summary – CategoryID API
Test run information

Test start time: 25/05/26, 3:59 PM

Test end time: 25/05/26, 4:00 PM

Summary:

A JMeter performance test was executed against the CategoryID REST API using 5 virtual users and 10 Category IDs. Each virtual user invoked the Category details endpoint, driving IDs from a CSV file. The test was run once over a short period, with requests distributed evenly and response times and errors captured into a JTL file (results.jtl). The results were analysed via the JMeter HTML dashboard. Assertions were configured to validate HTTP status codes and key response fields (e.g. CanRelist and CategoryId).

Two samples failed assertions:

Category ID 6327 returned HTTP 304 – Not Modified. This is a valid HTTP response indicating that the resource has not changed; it is not necessarily an application error but caused our “expect 200” assertion to fail.

Category ID 6331 failed the CanRelist assertion, meaning the actual value did not match the expected (true) value configured in the test for the assertion check. There might be some fuctional, data or configurational difference for this category Id.

Scope of Testing: https://api.tmsandbox.co.nz/v1/Categories/6327/Details.json?catalogue=false

Total requests executed: 10

Results Statistics From HTML Report:

Total samples: 10

Average response time: 128.5 ms

Minimum / Maximum response time: 69 ms / 514 ms

Median response time: 83.5 ms

90th percentile: 474.5 ms

95th / 99th percentile: 514 ms / 514 ms

Throughput: ~1 transaction/second

Error rate: 20% (2/10 samples), caused by assertion/response-code expectations, not HTTP 4xx/5xx server errors.

SLA evaluation:

SLA: 90% of responses must be ≤ 500 ms.
Actual: 90th percentile = 474.5 ms.

Conclusion: 

The HTTP 304 for Category ID 6327 appears to be expected behaviour from the API when content is unchanged. For performance reporting, 304 should be treated as a successful outcome, not a failure.

The CanRelist failure for Category ID 6331 may indicate a genuine issue. Recommendation would be to confirm the intended CanRelist behaviour for Category ID 6331 with the product/BA team.

SLA is met for this run, noting that the result is based on a small sample size (10 requests). Further runs with more samples are recommended for stronger confidence.
