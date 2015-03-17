# Akana API HOOK
![Image of Akana] 
(https://www.akana.com/img/formerlyLOGO8.png) 
[Akana.com](http://akana.com)

## SmartyStreets API 
![SmartyStreets API] 
(https://smartystreets.com/includes/home-feb-2015/img/logo.png)

### About the API
- Verify Addresses with SmartyStreet's USPS Address Verification API
- Home Page: [SmartyStreet] (https://smartystreets.com)
- API Documentation: [SmartyStreet API docs] (https://smartystreets.com/docs)

### Pre-Reqs
- Create an Account with [SmartyStreet] (https://smartystreets.com)
- Login to Obtain the Auth ID and Auth Token
- Using Firefox or any browser go to (https://api.smartystreets.com) and export the complete SSL certificate hierarchy as a .DER
- Import the certificates into Akana's Policy Manager by selecting the Configure tab -> Security -> Certificates and click "Add Trusted CA Certificate" * note this will take a couple of minutes to propagate to network director. 
- Install Authentication Custom Policies Follow instruction [Here] (https://bitbucket.org/akana/apihooks/src/f0a543b41c437a319e2a6573891813a01ff57f68/pso.experimental.extensions/?at=master)

### Getting Started Instructions
#### Download and Import
- Download SmartyStreetAPIHook.zip (https://github.com/lheritage/Demo/tree/master/dist)
- Download migration.properties and modify with your container key.
- Login to Akana's Policy Manager  example: http://localhost:9900
- Select the select the parent organization you want to import the API Hook into.  The import will create a whole new organization. 
  - Click on the "Import Package" from the Actions navigation window on the right side of the screen
  - Click on button to browse for the org_SmartyStreetOrg_export.zip archive file
  - Click on button to browse to your migration.properties file.
  - Click Okay.

#### Verify Import
- Expand the organization you imported into.  You should now see a new organization called SmartyStreetOrg.
- Expand the services folder of the SmartyStreetOrg.  You should see two physical and one virtual service.
- Expand Scripts.   You should see OpenAPI-Common
- Expand Policies -> Operational Policies.  You should see two defined.  
    1. SmartyStreet-HelloWorldConfig
    2. SmartyStreet_SecurityPolicy

#### Configure Security
- Expand Policies -> Operational Policies and click on SmartyStreet-HelloWorldConfig
- From the XML Policy window in the center pane, click on Modify
- Enter your auth-id and your auth-token for smartystreets.  
  - Get your Auth ID and Auth Token  by logging into [SmartyStreet] (https://smartystreets.com). 
  - On the left nav select API Keys
  - From the Secret Keys section get your Auth ID and your Auth Token
- Once you have entered your auth-id and auth-token in your policy.  Click Apply
- From the Policy Workflow pane on the right select Activate Policy


#### Policy Activation
- We also need to activate the SmartyStreet_SecurityPolicy.  
- Select SmartyStreet_SecurityPolicy
- From the Policy Workflow pane on the right select Activate Policy


#### Activate Anonymous Contract
- For this example there is already an anonymouse contract defined.
- Expand the Contracts -> Provided Contracts folders
- Select the SmartyStreet_Open v1.0 contract
- From the Contract Workflow pain on the right. Click Activate.

#### Verify Connectivity

To verify connectivity we created and installed a sample integration API called SmartyStreets_HelloWorldIntegaration_vs0.   You will find it in the services folder.
- This integration API uses the GET street-address of the SmartyStreets API to validate an address and then appends a success status to the result of the invocation of the SmartyStreets API.
- When viewing the details page of the SmartyStreets_HelloWorldIntegration_vs0 you will notice in the PolicyAttachment we have two operational policies attached.  
    - The first one is for auditing.  
    - The second one is the configurations we are supplying to the integration for authenticating to the SmartyStreets API.  
- You can view the integration in PolicyManager by selecting the SmartyStreets_HelloWorldIntegration_vs0, selecting Operation from the menu bar, clicking on GET /validateStreet and select Process

Try out the integration
- Using your browser http://{Your HOST example: localhost:9901}/smarty/v1/validateStreet?street=12100 Wilshire Blvd&city=Los Angeles&state=CA&zipcode=90025

The results should look like this.   You can change the address you want to verify. 
```
{"helloworld":"success",
 "helloWorld":[{"input_index":0,
                "candidate_index":0,
                "delivery_line_1":"12100 Wilshire Blvd",
                "last_line":"Los Angeles CA 90025-7120",
                "delivery_point_barcode":"900257120996",
                "components":{"primary_number":"12100",
                "street_name":"Wilshire",
                "street_suffix":"Blvd",
                "city_name":"Los Angeles",
                "state_abbreviation":"CA",
                "zipcode":"90025",
                "plus4_code":"7120",
                "delivery_point":"99",
                "delivery_point_check_digit":"6"},
                "metadata":{"record_type":"H",
                            "zip_type":"Standard",
                            "county_fips":"06037",
                            "county_name":"Los Angeles",
                            "carrier_route":"C026",
                            "congressional_district":"33",
                            "building_default_indicator":"Y",
                            "rdi":"Commercial",
                            "elot_sequence":"0074",
                            "elot_sort":"A",
                            "latitude":34.04348,
                            "longitude":-118.46757,
                            "precision":"Zip9",
                            "time_zone":"Pacific",
                            "utc_offset":-8,"dst":true},
                            "analysis":{"dpv_match_code":"D",
                                      "dpv_footnotes":"AAN1",
                                      "dpv_cmra":"N",
                                      "dpv_vacant":"N",
                                      "active":"Y",
                                      "footnotes":"H#"}}]}
 
```

### Create Your Own Integration with SmartyStreets
You are now able to create your own integrations with SmartyStreet API.   Use the SmartyStreets_HelloWorldIntegration_vs0 example integration api as your guide!

### Modify and Build
- RAML Changes
  - bla bla
- Security Changes
  - bla bla

### License
Put a link to an open source license

