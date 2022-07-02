# How to: Link Github to remote FTP folder to deploy there on push

The source of this tutorial is: https://github.com/marketplace/actions/ftp-deploy?version=4.3.0

## Step 1: Create new Github repository
**Note**: You may want to make the repository private
Create a `README.md` file at the start, as it will be anoying to manually add files afterhand if you don't

## Step 2: Link it locally and create the `.gitignore` and `.gitattributes` files
Follow the instructions [here](/How%20to:%20Create%20new%20typescript%20project%20in%20VS2019%20remotely%20connected%20to%20github/main.md#how-to-create-new-typescript-project-in-vs2019-remotely-connected-to-github)
If the project is going to use typescript, then create the `tsconfig.json` file. Note you must then restart visual studio for the `tsconfig.json` to take effect

## Step 3: Setup action in the repository
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
