# c8oprj-demo-dynamicEndpoint

This Mobile Builder demo project demonstrates the ability to dynamically set a new server endpoint address that differs from the one the application platform was built with.

## Background

When you build a Mobile Builder application for a platform (iOS, Android), the endpoint Url is **hardcoded** in the project's package. By default, it comes from the url address you typed to access the Test Platform of the project, or it can be set by the **Convertigo server endpoint** property of the **MobileApplication** object in your project. This address is used to perform the Flash Update of your application and to connect to the project's sequences or transactions. It looks like:

`http<s>://<convertigo_address><:port>/convertigo`

## Dynamically set server endpoint url

It is possible to override the server endpoint url at application runtime.\
Copy the **setEndpoint** sharedAction component from the demo project to your own project.\
In an event, use the sharedAction providing it the **endpoint** variable value set to an absolute Convertigo project url:

`http<s>://<convertigo_address><:port>/convertigo/projects/<project_name>`

Note that it needs to point up to the project's name unlike the **Convertigo server endpoint** property.
This means that you can connect to any other Convertigo server and Back Office project, but:

1. The new endpoint is not persistent, meaning you have to use the sharedAction each time the application starts or each time you need to switch to a new endpoint.
2. The Flash Update feature only relies on the hardcoded original server endpoint. To update your application you must deploy it on the original Convertigo server for the UI and eventually to the target Convertigo server for the Back Office parts.

## demo description and usage

The demo project uses a symbol named **dynamicEndpoint.from**.
Set it to **STUDIO**, for example, in your Studio Administration Console.

In the first page of the appplication, there are 4 buttons:

1. **ENDPOINT DEV** button sets the endpoint url to your local Studio address (http://localhost:18080/convertigo/projects/dynamicEndpoint). To test on a specific platform and device use your IP address. Then, it calls the **SQA** sequence and should display a toast message **STUDIO**
2.  **ENDPOINT PROD** button sets the endpoint url to a Convertigo Cloud Server project (https://confirmed.convertigo.net/convertigo/projects/dynamicEndpoint) that has the same project (for simplicity but not necessary) deployed and the **dynamicEndpoint.from** symbol set to **SERVER**. Then, it calls the **SQA** sequence and should display a toast message **SERVER**
3. **NEXT PAGE** button only roots to a new page to demonstrate that the new server endpoint is global to the application.
4. **SYNC & GET** button creates a document server-side with id **test_endpoint** and key/value pair 

    `data: "<dynamicEndpoint.from symbol value (STUDIO or SERVER)>"`

    It synchronizes with the new fullsync database endpoint and creates locally a new IndexDB database with a hash suffix based on the endpoint value (to avoid data mix and need to perform a reset on the same Fullsync DB name) and pull the document. The document **data** value is displayed in the right corner of the footer. The **RESET** button only resets the displayed value.

In the second page of the appplication, there are 2 buttons:

1. **CALL SEQUENCE** button calls the sequence on the new server endpoint address. For simplicity of the demo, the same project and the same sequence name are used. It should display a toast message **STUDIO** or **SERVER** depending on the button clicked on the previous page to set the new server enpoint url.
2.  **PREVIOUS PAGE** button only roots back to previous page to test again the endpoint switch.