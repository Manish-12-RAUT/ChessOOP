ChessOOP
========

A simple GUI based chess game that implements basic OOP concepts in Java. The class design and requirement analysis of this project was done as a part of the Object Oriented Analysis and Design course while the actual implementation was done was a part of Paradigms of Programming Part I course.

Running the Game
----------------

This game can be played by simply executing binary in Executable Jar folder. The Executable bit of the Chess.jar needs to be set in order to execute the file. New players have to be created for the first time you run the program.

Features
--------

1. GUI Developed using Java Swing
2. Backtracking to determine all possible moves of a piece
3. Greedy Approach to determine if the move results in a potential win/loss
4. A number of GUI Event Handlers were implemented

Developing the Project
----------------------

The project can be imported in Eclipse and developers can start right away. The are two 2 main packages. The Piece package provides functionalities for each piece on the Chess Board. The chess package provides the major functionalies including the Main Class. The Time Class provides the timer related functions and the Player Class is the vitual avatar of a real player. The chessboard is made of 64 cells and each cell is modeled in Cell class. The GUI functions on each cell has also been provided.

The project has been well documented in 3 docs - Software Requirement Specification, Class Design, Project Documentation. Please go through it to get more insight about the working of the project.

Since this was one of our first project during our undergraduate, there are a lot of bugs. The coding style and standards might not be up to the mark. Please bear with us.

Project Contributors
--------------------

1. Ashish Kedia (ashish1294@gmail.com)
2. Adarsh Mohata (amohta163@gmail.com)





# Define function to create a local user
function CreateLocalUser {
    param (
        [string]$username,
        [string]$password
    )

    # Create a local user with specified username and password
    $securePassword = ConvertTo-SecureString -String $password -AsPlainText -Force
    New-LocalUser -Name $username -Password $securePassword -UserType "Local" -FullName $username -Description "Local User"
    Write-Output "Local user $username has been created."
}

# Define function to create an AD user
function CreateADUser {
    param (
        [string]$username,
        [string]$password,
        [string]$samaccountname,
        [string]$firstname,
        [string]$lastname
    )

    # Import the Active Directory module
    Import-Module ActiveDirectory

    # Define the domain and Organizational Unit (OU) where the user will be created
    $domain = "yourdomain.local"
    $ou = "OU=Users,DC=yourdomain,DC=local"

    # Create a secure password
    $securePassword = ConvertTo-SecureString -String $password -AsPlainText -Force

    # Create a new AD user with specified properties
    New-ADUser -Name "$firstname $lastname" -SamAccountName $samaccountname -UserPrincipalName "$samaccountname@$domain" -DisplayName "$firstname $lastname" -GivenName $firstname -Surname $lastname -AccountPassword $securePassword -Enabled $true -Path $ou
    Write-Output "AD user $samaccountname has been created."
}

# Prompt user to choose between local user and AD user creation
Write-Host "Choose user type to create:"
Write-Host "1. Local User"
Write-Host "2. AD User"

$userChoice = Read-Host "Enter 1 for Local User, 2 for AD User"

# Use a switch statement to create the chosen user type
switch ($userChoice) {
    1 { 
        $username = Read-Host "Enter local username"
        $password = Read-Host "Enter local user password" -AsSecureString
        CreateLocalUser -username $username -password $password
    }
    2 {
        $username = Read-Host "Enter AD username"
        $password = Read-Host "Enter AD user password" -AsSecureString
        $samaccountname = Read-Host "Enter SAM Account Name"
        $firstname = Read-Host "Enter First Name"
        $lastname = Read-Host "Enter Last Name"
        CreateADUser -username $username -password $password -samaccountname $samaccountname -firstname $firstname -lastname $lastname
    }
    default {
        Write-Host "Invalid choice. Please enter 1 or 2."
    }
}