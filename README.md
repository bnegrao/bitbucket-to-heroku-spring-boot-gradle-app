# Bitbucket to Heroku Spring Boot Gradle App
Project with the minimum setup required to deploy a Spring Boot + Gradle application from Bitbucket to Heroku.

The use case is when the repository is on Bitbucket, the application is a REST application built with 
Spring Boot(with Gradle(with Groovy)), and you want the application to be deployed to Heroku any time there is a push
to the repo at Bitbucket.

## Files
| File                                                                            | Used by                | Description                                                                                                                                             |
|---------------------------------------------------------------------------------|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| [./bitbucket-pipelines.yml](./bitbucket-pipelines.yml)                          | Bitbucket              | Defines the pipeline rules for Bitbucket.                                                                                                               |
| [./Procfile](./Procfile)                                                        | Heroku                 | Tells Heroku this application receives requests from the internet, and what is the command line to execute it.                                          |
| [./system.properties](./system.properties)                                      | Heroku                 | Sets what java sdk must be used to run this app.                                                                                                        |
| [./src/.../application.properties](./src/main/resources/application.properties) | Spring Boot and Heroku | It tells SpringBoot the application must listen in the port set by the `$PORT` environment variable. Heroku will want to set it to a random port number | 

## How the deployment works
1. Assuming you have checked out a branch from Bitbucket repository, you just need to push a change and that'll trigger the pipeline on Bitbucket.
2. The Bitbucket pipeline will just create a 'tgz' file with the application source code and send that package to Heroku
3. Heroku receives the package, builds the application, then deploys the executable 'jar' file.

## Heroku configuration
1. Create the application
2. Create an api key to be used by Bitbucket to invoke Heroku's API
3. Set the `heroku/gradle` buildpack in the application settings:
`Go to your application root page in Heroku -> Settings -> Buildpacks -> Add Buildpack -> 'heroku/gradle'`

## Bitbucket configuration
1. You need to enable pipeline execution in the repository configuration:  
`Bitbucket -> Repository root page -> Repository Settings -> Pipeline Settings -> check 'Enable Pipelines'`
2. Set the in the repository configuration the environment variables that are used in the [./bitbucket-pipelines.yml](./bitbucket-pipelines.yml) file:  
`Bitbucket -> Repository root page -> Repository Settings -> Pipeline Settings -> check 'Repository Variables'`.   
The variables are:
    * HEROKU_APP_NAME: the name your gave to your application in Heroku 
    * HEROKU_API_KEY: the api key you created in Heroku to allow api requests to your application 



