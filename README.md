VM

### test.py
* Please, cite all the improvements (no code is needed) that you would make to improve the code quality and overall possible reuse
  * Improvements are described in the [PR comments](https://github.com/AMNDL/Planeks-test/pull/1/files#diff-6140e1c7010a9a9ba28f498bab6d594b5442c2eeccb6f2415a50e209816d6710R312-R315)

* What verifications are missing in the function add_cat_code ?
  * Function needs purpose and params description in the docstring
  * Before attempting to load the Excel file, check if the file exists to avoid unnecessary exceptions
  * Verify that the specified sheet ("Codice") exists in the Excel file
  * Check if the expected columns ("Codice Esame SAP" and "ID prestazioni") exist in the loaded DataFrame.
  * After the merge, check for NaN values in the columns that were supposed to be filled. This can help identify issues with the merge.
  * Ensure that column names used for merging are consistent between the patient DataFrame and the "Codice" sheet. Consider converting column names to lowercase or using the same case to avoid potential issues.
  * Check if the resulting DataFrame (joined) is empty after the merge and log a warning.
* Is the logging chosen in the script scalable in terms of storage or how easy will it be to dig into the past results after running the script ?
  * Logger needs to use timestamps, so past results could be analyzed easily
  * To increase scalability, log rotation can be used. It would prevent log filed from becoming too large.
  * Logger can also use retention policy to remove very old log files.
  * I would also consider using tool like Grafana or CloudWatch that would help a lot with logs separation and maintaining
* How would you create a way to run the same script with different configuration files that would allow you to compare easily and efficiently the results of your script (ex: blue / green deployment) ? How would you create configuration schemas and composable config groups ?
  * I would use different config.ini files and ConfigParser to run the script with different settings.
  * Schema configuration can be declared in JSON format

## AWS

### __main__.py
* Can you list all the improvements (no code is needed) you would make to the code in this file  to enhance the code quality and increase its potential for reuse?
  * Improvements are described in the [PR comments](https://github.com/AMNDL/Planeks-test/pull/1/files#diff-6140e1c7010a9a9ba28f498bab6d594b5442c2eeccb6f2415a50e209816d6710R312-R315)

## process.py
* Can you list all the improvements you would make to the code in this file  to enhance the code quality and increase its potential for reuse?
  * Improvements are described in the [PR comments](https://github.com/AMNDL/Planeks-test/pull/1/files#diff-6140e1c7010a9a9ba28f498bab6d594b5442c2eeccb6f2415a50e209816d6710R312-R315)

## Overall
* We use Poetry for the management of the libraries in Python. Explain how to use a private repository as a library that you will use in another script and how you plan to avoid exposing the credentials of the private repositories in the files that constitutes your app.
  * I would use [tool.poetry.dependencies] and declare my module there using repository url and credentials
  * To hide credentials from repo, I would use [tool.poetry.scripts] and dotenv python module to avoid credentials usage directly in config file
* If we integrate all the modules and missing files, what modifications should be made to the code to ensure compatibility with processing requests from a separate AWS script?
  * We would need to create a web application to ensure communication with external AWS service, or use AWS Lambda. 
* In order for the code to access and utilize a PDF file, what modifications are necessary to ensure the file is downloaded and available on the virtual machine? Bear in mind security and resilience.
  * PDF file could be uploaded to S3 with strict access policies, so script would retrieve PDF from there,
* To enable simultaneous processing of the information from multiple patients in the overall system, how would you proceed ? How would you guarantee that the information that you generate will be available for the scripts in the VM? (see schema)
  * I would use Queues like AWS SQS to store list of patient information that needs to be processed and put script into AWS Lambda that would be triggered as soon as info is added to thje queue.
* How would you ensure security of the data that is transiting in between the systems ? How do you ensure over time the robustness of those measures ?
  * I would encrypt data that is being transisted, or use solutions like AWS KMS, IAM roles, or configure GateAway

## Database

* Without knowing the specific data but as a general description, how would you model the database required (no code needed, only high-level explanation) and how would you test that the models cover all the edge cases ? Please take into account that the scripts run following a complex orchestrated process where the scripts are launched with different frequency.
  * Didn't understand what exactly I'd have to model. But I would consider storing data retrieved from the external portal and information that was processed by VM. That would allow to understand what was the input information for the processing script, that could help in testing, debugging and changes overview. Maybe I would look at the direction of Event Sourcing.
## In general

* How do you plan to create documentation for the overall project more efficiently ?
  * I would use a tool like Guru, maybe Notion, plus more information in Readme files
How long would it take to implement all the suggestions made in the previous questions ?
  * 2.5 hours considering code review
