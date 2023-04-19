#  Github actions

<p>Working with the gitpod.yml file</p>

## gitpod.yml

### name  
Use to name your workflow 
```yml
name: Github actions practice 
```

### on  
Controls when the work flow will run  
```yml
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the actions tab
  workflow_dispatch:
```
### job
A workflow run is made up of one or more jobs that can run sequentially or in parallel  
```yml
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repositroy under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!
```
### Variables
There are a few ways to create and use variables

```yml
  variables:
    runs-on: ubuntu-latest

    steps:
    - name: 'Create and use env variables'
    # Use the env key
      env:
          First_Name: 'John'
    # And you can use them in the following ways
      run: |
        echo "First_Name $First_Name"
        echo 'Hello ${{ env.First_Name}}'

    - name: 'Another way'
      id: step_one
      run: |
        echo "current_state=ON" >> $GITHUB_ENV

    - name: "Use the value"
      id: step_two
      run: |
        echo "${{env.current_state}}" # This will output ON
```

To view the default variables  
```yml
  default-variables:
    runs-on: ubuntu-latest
    steps:
       run: env
```
To access secret variables set up on github  
```yml
  secrets:
    runs-on: ubuntu-latest
    steps:
     - name: 'Access secret'
       run: |
          echo "Secret value: ${{ secrets.CHAT_BOT }}"
```
Github action: uploand-artifact and download-artifact
```yml
  Artifact_upload:
    runs-on: ubuntu-latest
    steps:
     - name: 'upload_artifact'
       run: echo "Add this text content to a file" > file.txt
    
     - uses: actions/upload-artifact@v2
       with:
         name: file
         path: file.txt

  Artifact_download:
    runs-on: ubuntu-latest
    needs: upload_artifact
    steps:
     - name: 'download artifact'
    
     - uses: actions/upload-artifact@v2
       with:
         name: file
         path: file.txt
     - run: cat file.txt
```
