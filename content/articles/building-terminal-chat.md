
+++
categories = ["tutorials"]
date = "2018-10-23"
description = ""

featuredpath = "date"
linktitle = ""
slug = "building-terminal-chat-application"
title = "Building Termina Chat Application with Python and Pusher"
tags = ["medium","python","development"]
type = "post"
[ author ]
  name = "Ayan Bag"
+++





It is better to have a conversation

In our daily life we encounter with many chat application which a really awesome. It takes a lot effort to develop and maintain these application because it involves a two way communication between different user and logging each and every request provide by server.

Getting inspired by the chat application as we see in Die Hard 4, I created my own realtime chat application . And I am here to share how I made this interactive real-time chat application with quite concise Python code.It also feature an in-app file transfer system.So, lets get started.

### Prerequisites

Basic knowledge and understanding of python and its related concepts. Also need Python 3.x and pip installed and configured in your machine.

### Setting up your app:

I build the server with the help of a third party dependency , Pusher. Pusher is a hosted service that make super easy to add real-time data and functionality to web, console and mobile application.

**Step 1:** Create a account in pusher.com. I personally prefer to go with free account , if you use this project as a hobby project. In your free account , you will get *a daily message sending limit of 200,000* which will be enough for testing and using this chat app.

**Step 2:** After creating a the account, navigate to your dashboard.

![Your dashboard will look like this.](https://cdn-images-1.medium.com/max/2698/1*mKNvvH9LHrTofPS1AwTPzw.png)*Your dashboard will look like this.*

After you navigate to your dashboard , click the **Create New App **button.

**Step 3:** Now fill the required information i.e your application name , cluster info ( it is a physical location of server to handle your request ), your tech stack(front-end and back-end) and something about your app.

![](https://cdn-images-1.medium.com/max/2000/1*jho000oMW7YJCFQg92vqFw.png)

Then hit the Create My App button.

**Step 4:** After that, a another dashboard will appear. Here navigate to App Key column and take a note of all *the app credential *(app_id,key,secret,cluster).

### **Creating our chat application**

**Initial Stage:**

Firstly, it is preferred to create a isolated python environment for our app.It can be achieved by installing the package virtualenv. It is used to manage different environments in Python. This is so we do not end up with conflicting libraries which is different from project to projects.To install virtualenv , we have to run :

    sudo pip install virtualenv

for windows, (run in cmd or Powershell as admin)

    pip install virtualenv

Next, we need to create a virtual environment for our app

    virtualenv terminal_chat

Now we are done with setting up the environment,we move to the new directory created and the activate the enviroment.

    #Changing the directory
    cd terminal_chat
    #activating the enviroment
    source bin/activate

for windows user,

    #changing the directory
    cd terminal_chat
    #activating the eniviroment
    Scripts\activate

Now we need to install the following libraries which are required in our project.

* PyInquirer : A collection of common interactive command line user interfaces.

* termcolor : ANSII Color formating for printing color text in output terminal.

* progress : A animation package to create animated progression bar in terminal.

* pusher : The official Python library for interacting with the Pusher HTTP API.

* pysher : Python module for handling pusher WebSockets. This will handle event subscriptions using Pusher

* python-dotenv : Python module that reads the key, value pair from .env file and adds them to the environment variable.

* sql.connector : Allows Python to communicate with MySQL database.

To install, we need to run

    pip install termcolor pusher
    pip install git+https://github.com/nlsdfnbch/Pysher.git 
    pip install python-dotenv PyInquirer progress
    pip install mysql-connector

**Creating a entry point for our app:**

Now, we need to create .env file which holds our environmental variable which connect our app with Pusher. Create .env file in your working directory.

    app_id=your_api_id
    app_key=your_app_key
    app_secret=your_app_secret
    app_cluster=your_app_cluster

**Creating User Login and Registration System:**

Now create user login and registration authorization system named login_auth.py and add:

<iframe src="https://medium.com/media/af0c711f628658b9875baab96ac17abb" frameborder=0></iframe>

Before you run the script create a database named **user_info **and this database create a table named userwith columns:

* used_id : a unique id for each user

* name : stores the name of each user

* username : stores the username

* passwod : stores the password

You can create the database in Structured Query Language or in MySQL Workbench.
> # **If you don’t know Structure Query Language(SQL) then go for MySQL Workbench, it provides GUI based environment for creating a database without any knowledge of SQL.**

**Creating Chatroom Database:**

After you create user login system, its time to setup the chatroom for our users.Now , create a file named chatrooms_server.py and add:

<iframe src="https://medium.com/media/d003eca20742ddebc59ae775dd380646" frameborder=0></iframe>

Again before you run this script, create a table named user under same database user_info with columns

* server_rooms : stores all registered chatroom

* id : unused variable in the program but still required

**Creating the File Transfer System :**

Now we will setup our FTP to transfer file in-app.Here, in my app I used DriveHQ FTP Service (Free Version).Only you have to sign up in [www.drivehq.com](https://www.drivehq.com) as free service user.You can use any FTP Service you want.

    **Cloud FTP Server Details of DriveHQ**:(for free user)
    FTP SERVER NAME: ftp.drivehq.com
    IP ADDRESS : 66.220.9.50

Now create a script called ftpserver.py and add:

<iframe src="https://medium.com/media/6ff074db9505ea46187e4d63a01fdf7b" frameborder=0></iframe>

Above code, is used to upload the file provided by user in cloud and to download the file requested by user.

Here, ftplib is a File Transfer Protocol module that implements the client side of the FTP protocol.

**Creating the main app and its UI:**

Now create a file called terminalchat.py and add:

<iframe src="https://medium.com/media/1a6bcadc737be6e572f92df8ec0e8ede" frameborder=0></iframe>

Let explain you what is happening in the above code.

We import termcolor to print color text, PyInquirer to print formatted menu in the console, progress to do animation in our console and finally load_env to load environment variables from .env file.

Here, we also imported ftpserver.py and user_login_reg.py so that we can use the functionality of those script in our terminalchat.py script.

The TerminalChat class then defined, with some properties

* pusher : this property will hold the Pusher server instance once it is available.

* channel: this property will hold the Pusher instance of the channel subscribed to.

* clientPusher: this property will hold the Pusher client instance once it is available.

* user: this property will hold the details of the currently logged in user.

* users: this property holds a static list of users who can log in, with their values as the password. In a real-world application, this would usually be gotten from some database

* chatrooms: this property holds a list of all available chat-rooms one can join.

Now let describe you the defined functions in terminalchat.py :

* main : This a entry point into our script. Here,we call welcome function , the login function and the chat selector function.After that, we have a while loop with getInput function so the function will be always running and we will always have a input to type new message to the terminal.

* logincred : It is a login function. In this function we request username and password from user and calls login_auth_reg.login() function for authorizing the login.

* chatselector: It also the user to create or enter into a specific chatroom. It will check whether the input chatroom exists or not. If it exists , the call initPusher , which initialized and setup Pusher to send and receive our message

* getInput : It shows an input with the logged in user’s name in front, waiting for the user to enter a message and send

**NOTE**: In terminalchat.py is have used os.system('cls') as my development environment is Windows. If you running this script in Linux, replace os.system('cls') to os.system('clear') .

**Connecting to the Pusher server:**

Now we will connect the server to our client i.e our chat app. So to implement that we need import the following packages in terminalchat.py .

<iframe src="https://medium.com/media/b5474e30e8551d9e28165f67ac50b2b8" frameborder=0></iframe>

After this add this segment of code in terminalchat.py :

<iframe src="https://medium.com/media/ecd95c53e9a3f67d763a771f1a5c9aad" frameborder=0></iframe>

In initPusher function, we initialize a Pusher instance to a variable named pusher , passing in our app_id ,app_key, app_secret and app_cluster . After that we initialize a new Pysher client for Pusher. Here we Pysher as client library for Pusher because default library of Pusher only allows triggering of event and not subscribing them. We bind to the connection , the pusher:connection_established is pass to connectHandler function as its callback.After that, we call connect on clientPusher .

In connectHandler , we receive an argument as data , which comprise of connection data that comes from established connection between Pusher WebSockets. We subscribe to the channel, which has been chosen with Pusher, then bind to an event called newmessage, passing in the pusherCallback function as it’s callback.

In the pusherCallback method, we receive an argument called message, which returns the object of the new message received from Pusher. Here, we convert the message to a readable JSON format for Python, then check if the message isn't for the currently logged in user before printing the message to the screen alongside the sender’s name. We also print the logged in user’s name to the screen, with a colon in its front, so the user knows he can still type.

Now this segement of code at the end of getInput function of TerminalChat class:

    self.pusher.trigger(self.chatroom, u'newmessage', {"user": self.user, "message": message})

After receiving a message, we trigger a newmessage event to current chat-room,which pass the current user and message sent.

## Putting all pieces of Code into one file

Here is what our terminalChat.py looks like:

<iframe src="https://medium.com/media/d017cb8fb90fd923c7d8399bf83113b1" frameborder=0></iframe>

## Demo

The demo of our Terminal Chat App :

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/wy43hNxlfKg" frameborder="0" allowfullscreen></iframe></center>

Finally, thanks for bearing with me and reading till the last character! I applaud your patience 🙂.

Ref. : [https://www.pusher.com/](https://www.pusher.com/tutorials)

P.S: For my other projects, check my [GitHub](https://github.com/ayanbag) Profile.

---
## Author
>
>Hey, I’m Ayan, a backend software engineer. I write about what I know to help viewers like you. please consider supporting what I do!
>
>[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/B0B81M1SZ)

