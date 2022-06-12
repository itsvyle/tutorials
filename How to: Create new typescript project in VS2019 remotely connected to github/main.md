# How to: Create new typescript project in VS2019 remotely connected to github

## Requirements:
* Visual studio 2019
* The `Github Extension for Visual Studio`, which can be installed via the `Visual Studio installer` or later, by going to **Extensions -> Manage Extensions -> Online -> Searching for 'Github Extension for Visual Studio'**
* The `Typescript SDK` for visual studio. 

## Step 1: Create repo on github
**Note**: No `.gitignore` template required, however it's better to require the creation of a `README.md`

## Step 2: Connect repo locally

1. ### **Clone or check out code**
![image](https://user-images.githubusercontent.com/65409906/173238674-ec3dccc8-e192-4821-a1a1-280c377583b3.png)

2. ### **Browse a repository -> Github -> Select the name of your newly created repo**
![image](https://user-images.githubusercontent.com/65409906/173238731-9acb8c37-b78d-4c60-a3f9-9c83427c3966.png)

3. ### **Set Local Path to C:\Repos\\\<your project name>**
![image](https://user-images.githubusercontent.com/65409906/173238811-a74da9e6-2275-4aad-b675-65016ab30775.png)

4. ### Clone

## Step 2: Initialize the project
If not done before, show the team explorer window: `View -> Team Explorer`
Press on the `Home` button in the team explorer
If done correctly, it should look something like this:
![image](https://user-images.githubusercontent.com/65409906/173239280-b8a04fe0-793b-401c-8e71-885d2850d072.png)

1. ### Create the `.gitignore` and `.gitattributes` files
* In the team explorer window, go to `Settings -> Repository settings -> Ignore & Attributes Files`
![image](https://user-images.githubusercontent.com/65409906/173239379-beea06eb-a6bc-4a96-8b46-8446317335d0.png)

* Click on add for both of the files. The result should look something like this:
![image](https://user-images.githubusercontent.com/65409906/173239409-c3e6f441-ec57-4a8f-9477-33c802d90e39.png)

2. ### Push the files to github
* Go back to the `Home` of the Team Explorer
* Go in `Changes`
* Enter the commit message: **Initial commit**
* Click on the arrow next to the `Commit staged` Button, and do `Commit and Push`:
![image](https://user-images.githubusercontent.com/65409906/173239523-2d2c31ca-1a17-43c2-8b98-51214f67f248.png)

## Step 3: Prepare project for typescript coding:

1. ### Add a `tsconfig.json` file:
* Go in `Solution Explorer -> Right Click the current repo -> Add -> New File` and set the file name to `tsconfig.json`
