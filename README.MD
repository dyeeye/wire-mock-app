
# Wire Mock application on CloudHub

To improve the quality of code as early as possible, we should be able to test services against 3rd party services. From my experience, we do not often get mocking services. 
Using this project, we can introduce quickly and easily new mocks with powerful request matching and response templating. With not that big effort we can introduce scenarios for our tests.

## Wire Mock 

Wire Mock is flexible API mocking for unit, integration, and performance testing. The core is open source and embedded within this MuleSoft application.
More about wiremock you can read [here](https://wiremock.org/#open-source-get-started)

## Mappings

The empty mock server is not really usable. We need to upload mock scenarios. The best here is that mappings are not hardcoded, and you can change them on the fly. I prepared flow 'upload-mappings-scheduled-flow'
that reads sample mapping from my GitHub gist account and uploads it into wire mock. If you store this in a different place, fill free to change the logic of reading the mapping json file.

The scheduler is set up to refresh every 24h, but in case you can run on-demand, the scheduler to refresh earlier your mocking scenarios. Isn't it that amazing?

Did you know that you can make a REST callout to upload mappings? Load Wire Mock Module.postman_collection and check to Add Mapping to Wire Mock CloudHub request. 

### Request matching

In this single application, you should put all the mocks for different 3rd parties. I suggest using the following path pattern to group them.

``/{service}/{path}``

for example

``/experian-crosscore/decisioning``

How to prepare request matching you can find [here](https://wiremock.org/docs/request-matching/). Remember that this is advanced matching, and you can later reuse in the response values provided in the request.

### Response templating

To generate response, you can use Handlebars templating language. You can find [here](https://wiremock.org/docs/request-matching/) more about how to do it.

## Build

If you can just run the application and check how it works with sample mapping you can directly get **wire-mock-app-1.0.0-mule-application.jar** from **bin** folder and upload it to Cloudhub.
If you like to build it on your own follow the steps:
1. Download Wire Mock Extension (for mulesoft application) from [here](https://github.com/dyeeye/wire-mock-module)
2. Change in pom groupid and either push the extension to Anypoint Exchange or install it locally (in your maven)
3. In Wire Mock App in pom.xml update the dependency with artificatId wire-mock-module
4. Apply your changes like uploading mock mappings
5. Build and deploy to CloudHub

## Usage

To use mocking service, give the user hostname, which is the application cloud hub name and the prefix of the application I suggested earlier, for example

http://wire-mock-app.us-e2.cloudhub.io/experian-crosscore/decisioning

## To Do
- Enable http performance tuning
- Enable https support