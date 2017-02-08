
# Getting started with Liberty on Bluemix
To get started, we'll take you through a sample hello world app and help you set up a development environment.

## Prerequisites
{: #prereqs}

You'll need [Git](https://git-scm.com/downloads){: new_window}, [Cloud Foundry CLI](https://github.com/cloudfoundry/cli#downloads){: new_window}, [Maven](https://maven.apache.org/download.cgi){: new_window} and a [{{site.data.keyword.Bluemix}} account](https://console.ng.bluemix.net/registration/),

## 1. Clone the sample app
{: #clone}

Now you're ready to start working with the app. Clone the repo and change the directory to where the sample app is located.
  ```bash
  git clone https://github.com/IBM-Bluemix/get-started-java
  cd get-started-java
  ```
  {: pre}

## 2. Run the app locally using command line
{: #run}

Use Maven to install dependencies and build the .war file.

  ```
  mvn clean install
  ```
  {: pre}

Run the app locally on Liberty.
  ```
  mvn install liberty:run-server
  ```
  {: pre}

View your app at: http://localhost:9080/JavaHelloWorldApp


## 4. Deploy to Bluemix using command line
{: #deploy}

To deploy to {{site.data.keyword.Bluemix_notm}} using command line, it can be helpful to set up a manifest.yml file. The manifest.yml includes basic information about your app, such as the name, the location of your app, how much memory to allocate for each instance, and how many instances to create on startup. This is also where you'll choose your URL. [Learn more...](/docs/manageapps/depapps.html#appmanifest)

The manifest.yml is provided in the sample.

  ```
  applications:
  - path: target/JavaHelloWorldApp.war
    memory: 512M
    instances: 1
    name: your-appname-here
    host: your-appname-here
  ```
  {: codeblock}

Change both the *name* and *host* to a single unique name of your choice. Note that the *host* value will be used in your public url, for example, http://your-appname-here.mybluemix.net. If you already created an app from the {{site.data.keyword.Bluemix_notm}} UI but haven't pushed your code to it, you can use the same name value. Make sure the path points to the built application, for this example the location is `target/JavaHelloWorldApp.war`.

Choose your API endpoint
   ```
   cf api <API-endpoint>
   ```
   {: pre}

Replace the *API-endpoint* in the command with an API endpoint from the following list.
* https://api.ng.bluemix.net # US South
* https://api.eu-gb.bluemix.net # United Kingdom
* https://api.au-syd.bluemix.net # Sydney

Login to your {{site.data.keyword.Bluemix_notm}} account
  ```
  cf login
  ```
  {: pre}

Push your application to Bluemix.
  ```
  cf push
  ```
  {: pre}

This can take around two minutes. If there is an error in the deployment process you can use the command `cf logs <Your-App-Name> --recent` to troubleshoot.

## 5. Developing and Deploying using Eclipse
{: #eclipse}

IBM® Eclipse Tools for {{site.data.keyword.Bluemix_notm}} provides plug-ins that can be installed into an existing Eclipse environment to assist in integrating the developer's integrated development environment (IDE) with {{site.data.keyword.Bluemix_notm}}.

1. Download and install  [IBM Eclipse Tools for Bluemix](https://developer.ibm.com/wasdev/downloads/#asset/tools-IBM_Eclipse_Tools_for_Bluemix).

2. Import this sample into Eclipse using `File` -> `Import` -> `Maven` -> `Existing Maven Projects` option.

3. Create a Liberty server definition:
  - In the `Servers` view right-click -> `New` -> `Server`
  - Select `IBM` -> `WebSphere Application2 Server Liberty`
  - Choose `Install from an archive or a repository`
  - Enter a destination path (/Users/username/liberty)
  - Choose `WAS Liberty with Java EE 7 Web Profile`
  - Continue the wizard with default options to Finish

4. Run your application locally on Liberty:
  - Right click on the `JavaHelloWorldApp` sample and select `Run As` -> `Run on Server` option
  - Find and select the localhost Liberty server and press `Finish`
  - In a few seconds, your application should be running at http://localhost:9080/JavaHelloWorldApp/

5. Create a {{site.data.keyword.Bluemix_notm}} server definition:
  - In the `Servers` view, right-click -> `New` -> `Server`
  - Select `IBM` -> `IBM Bluemix` and follow the steps in the wizard.\
  - Enter your credentials and click `Next`
  - Select your `org` and `space` and click `Finish`

6. Run your application on {{site.data.keyword.Bluemix_notm}}:
  - Right click on the `JavaHelloWorldApp` sample and select `Run As` -> `Run on Server` option
  - Find and select the `IBM Bluemix` and press `Finish`
  - A wizard will guide you with the deployment options. Be sure to choose a unique `Name` for your application
  - In a few minutes, your application should be running at the URL you chose.

Now you have your code running locally and on the cloud!

The `IBM Eclipse Tools for Bluemix` provides many powerful features such as incremental updates, remote debugging, pushing packaged servers, etc. [Learn more](/docs/manageapps/eclipsetools/eclipsetools.html)


## 6. Add a database
{: #adddatabase}

Next, we'll add a NoSQL database to this application and set up the application so that it can run locally and on Bluemix.

1. Log in to Bluemix in your Browser. Select your application and click on `Connect new` under `Connections`.
2. Select `Cloudant NoSQL DB` and Create the service.
3. Select `Restage` when prompted. Bluemix will restart your application and provide the database credentials to your application using the `VCAP_SERVICES` environment variable. This environment variable is only available to the application when it is running on Bluemix.

## 7. Use the database
{: #usedatabase}
We're now going to update your local code to point to this database. We'll store the credentials for the services in a properties file. This file will get used ONLY when the application is running locally. When running in Bluemix, the credentials will be read from the VCAP_SERVICES environment variable.

1. In Eclipse, open the file src/main/resources/cloudant.properties:
  ```
  cloudant_url=
  ```
  {: pre}

2. In your browser open the Bluemix UI, select your App -> Connections -> Cloudant -> View Credentials

3. Copy and paste just the `url` from the credentials to the `url` field of the `cloudant.properties` file.

4. Your Liberty server in Eclipse should automatically pick up the changes and restart the application.

  View your app at: http://localhost:9080/JavaHelloWorldApp/. Any names you enter into the app will now get added to the database.

  Make any changes you want and re-deploy to Bluemix!
