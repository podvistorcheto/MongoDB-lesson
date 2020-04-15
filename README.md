In our last video, we learned how to use MongoDB from the command line using the Mongo shell.
In this video, we're going to see how we can do that programmatically using Python.
But first, we need to install some extra libraries so that the Python Mongo library will work.
So we can go to our terminal and type sudo pip3 install dnspython.
When that's installed, we can install the Python Mongo library, which is called pyMongo.
So to do that, sudo pip3 install pymongo, which will install the Python 3 version of the pyMongo library, which is the one that we want to use.
Now that we've done that, we can go back to our Atlas page and click on the connect button from the overview tab.
But this time, we don't want to connect using the Mongo shell.
We want to connect an application.
Again, we can use the Short SRV string because we're using the latest version of Mongo.
So we'll just click on copy to copy it to the clipboard.
Now we can go back to Cloud9.
We want to edit a file called .bashrc
The dot in front of the name indicates that it's a hidden file.
It contains commands that are run every time you open a new terminal.
We're going to put our Mongo URI in there so that we don't have to recopy and retype this every time we close down Cloud9.
The .bashrc file lives in our home directory, which is one level above where we are now.
So type cd .. to go up a level and then nano .bashrc to open the file in the nano editor.
Insert a blank line anywhere near the top and type export MONGO_URI.
Then open some quotes, paste in our connection string and close the quotes.
Now as we did before, we need to change the name of the database from test to our database name, which in my case was myTestDB.
Remember again, this is case sensitive.
This time, we actually need to supply the password.
So to do that, we'll just change the password placeholder to r00tUser.
When that's all done, we can press control X to exit, Y to save and enter to confirm the filename.
We need to close our terminal and reopen it for these changes to take effect.
When that's done, we can type echo $ MONGO_URI, and our connection string is printed back to us.
The reason we're doing this is because in production, when our code is live on a server, we don't want to go exposing these kinds of connection strings, particularly when they contain passwords.
We should never commit usernames, passwords, secret keys, and API keys to GitHub where they're visible to the whole world.
Use environment variables instead.
So let's go ahead and create a new Python file called mongo.py and open it up.
First we'll import pymongo.
Then we'll import os.
We're going to use the OS library to set a constant called MONGODB_URI by using the getenv() method to read in the environment variable that we just set.
We'll set another constant called DBS_name and give that the name of our database.
Then another constant called COLLECTION_NAME
We'll give that the name of our MongoDB collection.
Notice once again that the Python constants are all written in capital letters with underscores separating the words.
So now we'll create our mongo_connect() function.
So we'll define mongo_connect.
It's going to take url as an argument.
We'll do a try except block here.
So conn = pymongo.MongoClient(url).
If that works, then we'll print Mongo is connected to the screen.
We'll return our connection object, return conn
Now if we have an exception, if pymongo throws an error with a connection failure, then we shall print Could not connect to MongoDB.
We'll also print the error message.
So that's our basic Mongo connection string.
Let's get our indent level right here. We need to be right back at the beginning.
Let's call our function now, so conn = mongo_connect.
We send in MONGODB_URI as our argument.
Then we'll set our collection name.
coll = conn, and we'll pass in the database name and the collection name, so that will set our collection.
Now that we've done that, let's try printing everything that's in the database.
So we'll create a new variable and call it documents.
Very similar to how we did from the command line, we'll do coll.find.
We'll call the find() method.
That will be returned in documents.
Now, this returns a MongoDB object.
So to iterate through that, we'll do for doc in documents.
Then print the doc to the screen.
Let's try that: python3 mongo.py
Mongo is connected.
As we can see, we have the contents of our database printed to screen.
Each of the records that we added earlier through the command shell and through the web interface are there.
So that's how to print all of them.
What about if we wanted to insert a new record?
Let's creates a new variable. We'll call it new_doc.
Again, we just pass in the key value pairs.
We'll create Douglas Adams here.
Notice, again, that we're putting the field names in quotes.
I'm using single quotes here, just for the sake of consistency.
So we're going to supply all of the values.
Hair color was grey.
His occupation was a writer.
Nationality was English.
So we just create another dictionary with key value pairs.
We're assigning this to our variable new_doc.
When we've done that, again, just coll.insert(new_doc).
Now we're going to keep our find() method here as well and iterating through that so that we can print everything to screen again and see that what we've done has worked.
So we're going to define our new_doc, insert it, and then find all.
Let's just clear the screen here and run it again.
As we can see, our final record there has the occupation of writer, and it's Douglas Adams.
What if we wanted to insert more than one record like we did before?
Well, this is very similar.
Let's just delete everything that we've put here because we don't want to add more than one Douglas Adams into our database.
To do this, we're going to create a variable called new_docs.
We send an array of dictionaries.
So I'm going to add Sir Terry Pratchett here.
Then, when we've done that, we put a comma at the end of our first dictionary, and then we create our second dictionary in the array.
This time we'll create George RR Martin.
So we close off our dictionary, and then we close off our array.
This time, we use the insert_many() method.
So it'll send new_docs.
So instead of insert, it's now insert many.
We'll retain our find() method.
Let's just clear the screen.
Run it.
As we can see now, after Douglas Adams, we have Terry Pratchett and George RR Martin who have also been added.
So that's how we can create new data in our MongoDB database.
Let's just delete that.
What if we wanted to find specific data?
Well again, we use the find() method, as we did before.
We just send in our key value pairs in a dictionary of what it is that we're searching for.
So let's say that we want to find everyone in the database whose first name is Douglas.
Let's clear the screen.
As we can see, that's what is returned.
Now we can also delete data by passing in a string like that, so coll.remove({'first': 'douglas'}).
Let's add back our find() method again.
So we want to find everything.
We'll just take off the documents= on the front of coll.remove there.
Run it again.
Now we can see that Douglas Adams has indeed been removed from the database.
So that's how to remove. What about if we wanted to update some data in the database?
Well again, we can use the update_one() method.
The first argument that we send in is a search string.
So in this case, we're looking for all whose nationality is American.
We're then sending a key value pair, a dictionary, as our second argument again.
We put this $set keyword.
Then we create another key value pair dictionary.
So this time, if they're American, we want to set hair color to maroon.
Just make sure we have the closing brackets right on that.
Now let's just filter our database now in our find() method by sending in a dictionary there with the nationality of American.
I'll save that, clear it, and run it again.
We can see, just like you did before, that one of our records has changed. The first one in the database, hair color, to maroon.
George Martin's hair color has stayed the same.
So to change that, we can just change our method to update_many.
Let's clear it and run it again.
Now, indeed, you can see that all of the records have been changed.
So that's the basics of how to do CRUD, create, read, update, and delete using Python to access MongoDB.
In our next video, we're going to put all of this together and do a small walk-through project of our persons database.

SAMPLE Code


import pymongo
import os

MONGODB_URI = os.getenv("MONGO_URI")
DBS_NAME = "mytestdb"
COLLECTION_NAME = "myFirstMDB"

def mongo_connect(url):
    try:
        conn = pymongo.MongoClient(url)
        print("Mongo is connected!")
        return conn
    except pymongo.errors.ConnectionFailure as e:
        print("Could not connect to MongoDB: %s") % e
        
conn = mongo_connect(MONGODB_URI)

coll = conn[DBS_NAME][COLLECTION_NAME]

documents = coll.find()
for doc in documents:
    print(doc)




<img src="https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png" style="margin: 0;">

Welcome podvistorcheto,

This is the Code Institute student template for Gitpod. We have preinstalled all of the tools you need to get started. You can safely delete this README.md file, or change it for your own project.

## Gitpod Reminders

To run a frontend (HTML, CSS, Javascript only) application in Gitpod, in the terminal, type:

`python3 -m http.server`

A blue button should appear to click: *Make Public*,

Another blue button should appear to click: *Open Browser*.

To run a backend Python file, type `python3 app.py`, if your Python file is named `app.py` of course.

A blue button should appear to click: *Make Public*,

Another blue button should appear to click: *Open Browser*.

In Gitpod you have superuser security privileges by default. Therefore you do not need to use the `sudo` (superuser do) command in the bash terminal in any of the backend lessons.

## Updates Since The Instructional Video

We continually tweak and adjust this template to help give you the best experience. Here are the updates since the original video was made:

**February 2020:** The initialisation files now _do not_ auto-delete. They will remain in your project. You can safely ignore them. They just make sure that your workspace is configured correctly each time you open it. It will also prevent the Gitpod configuration popup from appearing.

**December 2019:** Added Eventyret's Bootstrap 4 extension. Type `!bscdn` in a HTML file to add the Bootstrap boilerplate. Check out the <a href="https://github.com/Eventyret/vscode-bcdn" target="_blank">README.md file at the official repo</a> for more options.

--------

Happy coding!
