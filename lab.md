# NLC App Instructions

## Pre-requisites
* Complete the [Dev Env Setup](https://github.com/Bluemix-Watson-Labs/Dev-Env-Setup) steps.

## Lab

1. 	Fork & clone the app using the Blue “Create Toolchain” button at [this GitHub repo](https://github.com/Bluemix-Watson-Labs/natural-language-classifier-toolchain-template).

  ![Create-Toolchain](./Create-Toolchain.png)

2. Before clicking the “Create” button on the Toolchain page, make sure you click on the *GitHub* tool and then the *Authorize* button to give Toolchains access to create/clone repositories in your GitHub account.

  ![Authorize-GitHub-Image](./Authorize-GitHub-Create-Toolchain.png)

3.	Once you click the “Create” button, click on the “Pipeline” tile to watch the progress of the deployment.

  ![Pipeline-Tile-Toolchain-Overview](./Pipeline-Tile-Toolchain-Overview.png)

4.	As part of the pipeline deploy script, your Natural Language Classifier service will be created, but the service credentials won’t be. Once your pipeline has deployed your application, click the back button on the page to go back to your toolchain.

  ![Deploy-App](./Deploy-App.png)

5.	Once you’re back on the toolchain overview page, click the “Connections” tab to see the Application that was deployed by the Pipeline tool in your toolchain. Click the App card to get taken to the Application Overview page on Bluemix.

  ![App-Card](./App-Card.png)

6.	Once on the Application Overview page on Bluemix, you’ll see a “Connections” tab on the left there too. Click that tab to see all the Connections your running Application has on Bluemix.

  ![Natural-Language-Classifier-App](./Natural-Language-Classifier-App.png)

7.	Once of the Connections should be to a “natural-language-classifier” service instance that was created by the pipeline for you as well. Click the card to go to the service instance page.

  ![Natural-Language-Classifier-Service-Instance](./Natural-Language-Classifier-Service-Instance.png)

8.	Next click the “Service Credentials” tab at the top and click the “New Credential” button, then “Add”. You now have a set of credentials that your application can use.

  ![Natural-Language-Credentials](./Natural-Language-Credentials.png)

9. Copy the `username` and `password` from the Credentials you just created.  You will need them in the next step.

10. Open your Terminal (Git Bash on Windows) and run the following:

    ```
    NLC_USERNAME=<username you copied from credentials above>
    NLC_PASSWORD=<password you copied from credentials above>
    mkdir -p ~/tmp
    cd ~/tmp
    wget "https://www.ibm.com/watson/developercloud/doc/nl-classifier/resources/weather_data_train.csv"
    curl -i -u "${NLC_USERNAME}":"${NLC_PASSWORD}" -F training_data=@./weather_data_train.csv -F training_metadata="{\"language\":\"en\",\"name\":\"TutorialClassifier\"}" "https://gateway.watsonplatform.net/natural-language-classifier/api/v1/classifiers"
    ```
  * Retrieve Classifier ID after the training is `DONE`, takes about 6 minutes. Copy and paste the classifier-ID key.

11.	To configure the app.js file to use your classifier, export the classifier ID as an environment variable. We can do this through the Bluemix Graphical User Interface (GUI) or the command line shown here:

  ```
  $ cf set-env <application-name> CLASSIFIER_ID <classifier-id>
  ```

12.	Finally, restage the application to ensure the environment variable is set.

  ```
  $ cf restage <application-name>
  ```
