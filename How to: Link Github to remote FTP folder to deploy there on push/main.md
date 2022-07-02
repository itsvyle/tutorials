# How to: Link Github to remote FTP folder to deploy there on push

The source of this tutorial is: https://github.com/marketplace/actions/ftp-deploy?version=4.3.0

## Step 1: Create new Github repository
**Note**: You may want to make the repository private
Create a `README.md` file at the start, as it will be anoying to manually add files afterhand if you don't

## Step 2: Link it locally and create the `.gitignore` and `.gitattributes` files
Follow the instructions [here](/How%20to:%20Create%20new%20typescript%20project%20in%20VS2019%20remotely%20connected%20to%20github/main.md#how-to-create-new-typescript-project-in-vs2019-remotely-connected-to-github)
If the project is going to use typescript, then create the `tsconfig.json` file. Note you must then restart visual studio for the `tsconfig.json` to take effect

## Step 3: Setup Secrets in the repository
* Navigate to the `Settings` tab of the repo, and go the `Secrets->Actions`
* Add the 3 following secrets:
  * `FTP_SERVER`: the FTP uri of the remote server
  * `FTP_USERNAME`: the username with which you log in to the ftp server
  * `FTP_PASSWORD`: the password with which you log in to the ftp server
  * `FTP_UPLOAD_LOG_LEVEL`: the level of logging of the uploader. One of: 
   * `minimal`: only important info
   * `standard`: important info and basic file changes
   * `verbose`: print everything the script is doing
It should look like this:
![image](https://user-images.githubusercontent.com/65409906/177017532-fed61ad5-e436-4c69-958a-e4ab30bd5e45.png)



## Step 4: Setup action in the repository
* Go in the `Actions` tab of the repo:
![image](https://user-images.githubusercontent.com/65409906/177016515-a2308702-e11d-491f-b019-3767256cab37.png)
* Click on `Set up a workflow yourself`
* Check that the file position is `PROJECT_NAME/.github/workflows/main.yml`
![image](https://user-images.githubusercontent.com/65409906/177016548-09affcb2-4623-48c2-894c-623aa1288eb3.png)
* Paste in the following content:
```yml
# Documentation here: https://github.com/marketplace/actions/ftp-deploy?version=4.3.0
name: 🚀 Deploy website on push
on:
  push:
    branches: ["main"]
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:
jobs:
  web-deploy:
    name: 🎉 Deploy via FTP
    runs-on: ubuntu-latest
    steps:
    - name: 🚚 Get latest code
      uses: actions/checkout@v2
    
    - name: 📂 Sync files
      uses: SamKirkland/FTP-Deploy-Action@4.3.0
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        port: 21
        # Defines whether or not actual changes will be made | dry-run: true means that no changes will be made
        dry-run: false
        server-dir: "./"
        # minimal: only important info | standard: important info and basic file changes | verbose: print everything the script is doing
        log-level: ${{ secrets.FTP_UPLOAD_LOG_LEVEL }}
        # Must always be false:
        dangerous-clean-slate: false
        exclude: |
          **/.git*
          **/.git*/**
          **/node_modules/**
          README.md
```
* If the remote ftp is the source of a french OVH website, add the following lines to the `exclude` property, being careful that they are indented the same as the other lines of `exclude`:
```yml
          .forward
          .htaccess
          LISEZ-MOI
          ./sessions/**
          ./cgi-bin/**
          ./requetes/**
```
* If you don't want to upload typescript files (`.ts` files), add this to the `exclude` property, being careful that they are indented the same as the other lines of `exclude`:
```yml
          **/**.ts
```

## Step 5: Upload the current data on the website
* Using filezilla, download the following content of the ftp server contents to the LOCAL repository
 * If this is an OVH website, only download the `www` folder and the `.ovhconfig` file
* Pull data from the github repository
* Push all changes to the repo

## Seeing upload/actions logs
By going in the `Actions` tab of the repo, you should be able to see all the logs from the previous synchronisations

## Run upload manually or disable auto-upload
Go in the `Actions` tab of your repo
In the `Workflows` left panel, select the `🚀 Deploy website on push` workflow

**To run, just click on the `Run Workflow` button**:
![image](https://user-images.githubusercontent.com/65409906/177017352-b1611f1d-bedd-4c6e-82c6-46f8047bb037.png)

**To disable auto-upload, click on the three dots then on `Disable Workflow`**:
![image](https://user-images.githubusercontent.com/65409906/177017363-3c2c3674-49db-4333-ae58-86e2f01bf893.png)
