# Breaking the Monolith by using hapi 
## Background
Let me get the disclaimer out of the way: I am not an expert on Hapi
I started looking into Hapi's ability to break components out.
This is my attempt to follow other tutorials from a hello world to a true component system.
I have broken this down into the following steps

| Project  | Description | Link |
|---|---|---|
|hapi-tut-monolith-01|A simple hello world hapi project| [01](https://github.com/quapaw/hapi-tut-monolith-01)|
|hapi-tut-monolith-02a|Add services - customers and products| [02A](https://github.com/quapaw/hapi-tut-monolith-02a)|
|hapi-tut-monolith-02b|Adding Glue and externalizing config| [02B](https://github.com/quapaw/hapi-tut-monolith-02b)|
|hapi-tut-monolith-02c|Moving services into their own folders| [02C](https://github.com/quapaw/hapi-tut-monolith-02c)|
|hapi-tut-monolith-03-main|Moved service into own project. Instructions here| [03-main](https://github.com/quapaw/hapi-tut-monolith-03-main)|
|hapi-tut-monolith-03-customer|Just the customer service| [03-customers](https://github.com/quapaw/hapi-tut-monolith-03-customers)|
|hapi-tut-monolith-03-products|Just the produce service| [03-products](https://github.com/quapaw/hapi-tut-monolith-03-products)|
|hapi-tut-monolith-04a-customer|Movement of some files| [04a-customers](https://github.com/quapaw/hapi-tut-monolith-04a-customers)|
|hapi-tut-monolith-04b-customer|New methods| [04b-customers](https://github.com/quapaw/hapi-tut-monolith-04b-customers)|
|hapi-tut-monolith-04c-customer|Validation and Error Handling|[04c-customers](https://github.com/quapaw/hapi-tut-monolith-04c-customers)|
|**hapi-tut-monolith-04d-customer**|**Unit Testing**|**[04d-customers](https://github.com/quapaw/hapi-tut-monolith-04d-customers)**|


#HAPI Tutorial - Monolith - 4 - Move toward production
This part of the tutorial will move the customer plugin more toward a production plugin.
This step, 04d, will add unit testing.

* Unit Testing will use lab test framework.  Unit testin will also use labbable to manage the hapi server.
    The github project for lab is located here (https://github.com/hapijs/lab)
    The github project for labbable is located here (https://github.com/devinivy/labbable)
    
## Adding lab and labbable as development requirements

* Add lab to package.json by running ```npm install lab --save-dev```
* Add code to package.json by running ```npm install code --save-dev```
* Add labbable to package.json by running ```npm install labbable --save-dev```

## Adding test script to npm package.json

* Add the following configuration to your package.json
    This will call lab to run unit tests
    -L will run the linter to evaluate your code
    -v will turn on verbose test output
    -r tap -o stdout 
        Tells to run a tap report and send the output of tap to stdout
    -r html -o test.html
        Tells to run html report and send it to the file called test.html

```json
  "scripts": {
    "test": "lab -L -v -r tap -o stdout -r html -o test.html"
  }
```

* Change localServer.js
    Look at the copy of localServer.js in this project.  
    The logic has been changed to allow it to be used for unit testing by using labbable
    
* Add a directory called test
* Create a TestCustomer.js file in the test directory
    Look at our example TestCustomer.js in the project
    
    
* To run the unit test run the following command ```npm test```

* Tests may all pass but the lenting may fail.  
# Look at test.html 
Take a look at the lent errors on the files that have been running the the past tutorials
    
    
    ```
    index.js
    L1 - ERROR - strict - Use the global form of 'use strict'.
    L1 - WARNING - hapi/hapi-scope-start - Missing blank line at beginning of function.
    L8 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L9 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L10 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L11 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L12 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L13 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L14 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L15 - ERROR - indent - Expected indentation of 14 spaces but found 16.
    L16 - ERROR - indent - Expected indentation of 10 spaces but found 12.
    L19 - ERROR - object-curly-spacing - A space is required after '{'.
    L20 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L21 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L22 - ERROR - indent - Expected indentation of 4 spaces but found 17.
    L24 - ERROR - object-curly-spacing - A space is required after '{'.
    L25 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L26 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L27 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L29 - ERROR - indent - Expected indentation of 26 spaces but found 24.
    L30 - ERROR - indent - Expected indentation of 26 spaces but found 24.
    L31 - ERROR - indent - Expected indentation of 26 spaces but found 24.
    L32 - ERROR - indent - Expected indentation of 26 spaces but found 24.
    L35 - ERROR - indent - Expected indentation of 4 spaces but found 17.
    L37 - ERROR - object-curly-spacing - A space is required after '{'.
    L38 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L39 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L40 - ERROR - indent - Expected indentation of 8 spaces but found 18.
    L41 - ERROR - indent - Expected indentation of 22 spaces but found 20.
    L48 - ERROR - indent - Expected indentation of 4 spaces but found 17.
    L56 - WARNING - eol-last - Newline required at end of file but not found.
    lib/CustomerRoutes.js
    L8 - WARNING - hapi/hapi-scope-start - Missing blank line at beginning of function.
    L12 - WARNING - hapi/hapi-scope-start - Missing blank line at beginning of function.
    L17 - WARNING - hapi/hapi-scope-start - Missing blank line at beginning of function.
    L31 - WARNING - hapi/hapi-scope-start - Missing blank line at beginning of function.
    L32 - ERROR - prefer-const - 'payload' is never reassigned. Use 'const' instead.
    L40 - WARNING - eol-last - Newline required at end of file but not found.
    ```

    * Have fun cleaning up the lenting errors until you get this
    
    ```
    Linting Report
    
    0 0
    ```

    * Look at the code coverage report.
    The report will highlight lines of code that have not been tested with your unit test.
    See how close to 100% coverage you can get
    You can add the following code to TestCustomer.js to get to 100% coverage
    
   
    ```javascript
    test('get customers', (done) => {
    
            const options = {
                method: 'GET',
                url: '/customers'
            };
    
            server.inject(options, (res) => {
    
                expect(res.statusCode).to.equal(200);
                done();
            });
        });
    
        test('load customer', (done) => {
    
            const SampleCustomer = {
                id:     126,
                first:  'David',
                middle: 'H',
                last:   'Father',
                addressLine: '9 S. Ninth Street',
                city:        'Little Rock',
                state:       'AR',
                postalCode:  '72206'
            };
    
            const options = {
                method: 'POST',
                url: '/customers',
                payload: SampleCustomer
            };
    
            server.inject(options, (res) => {
    
                expect(res.statusCode).to.equal(200);
                done();
            });
        });
    
    ```