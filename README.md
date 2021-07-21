# How to write your own tutorial

To get a full setup of Google Codelabs you can follow this tutorial https://medium.com/@zarinlo/publish-technical-tutorials-in-google-codelab-format-b07ef76972cd. 


## Write your tutorial

Thanks for considering contributing a tutorial or fixing a bug!

1. Fork the repo from https://github.com/nodnarboen/apac-techlabs and clone it to your local machine.

1. Find all tutorial located in the `/site/workshops` folder.

1. Start your tutorial by taking a look at the `template.md` file in this folder. You can just duplicate the file and adopt it to get started easily. The contents of the file should be self-explanatory. Make sure to change the "ID" as this will be the folder the generated files of your new tutorial will be go into.

1. Write the tutorial - make sure you use some of the features that are described in the `tutorial-template.md` file such as `duration` information or hightlights of some sections.


## Generate contents and test locally

1. Generate ONLY ONE tutorial and test it locally

    ```
    ./builder.sh my-tutorial.md

1. You can now start a local webserver take a look at your tutoral (you'll find the `claat` binary in the `./bin` folder, please copy it in your path)

    ```
    claat serve
    ```

1. Alernatively, generate ALL tutorials and test it locally.
    ```
    ./builder.sh 

    claat serve
    ```

  
### Optional: Preview overview site 

- To generate not only the local preview for the tutorials themselves but also the overview page of all tutorials, execute the following command and navigate to the URL that is given to you after executing the command (you need to install `gulp` first, take a look at the prerequisites):
    ```
    gulp serve 
    ```

- If you want to generate the content without starting the server for preview, execute the following command:
    ```
    gulp dist
    ```

## Contribute you tutorial

Create a PR with your changes. 
Please **do not** include the generated `*.html` file, but **do include** the `codelab.json` file that has been generated.

# Need help?

If you need help, please reach out to us @ APAC-SE-Central at-sign dynatrace.com
