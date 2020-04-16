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






ManipulateDataWithACustomUserInterface


In our final video, we're going to add find, update, and delete functionality to our app.
So that will give us the full range of CRUD operations, Create, Read, Update, and Delete in our menu-driven app.
So go ahead and add a new function after our add function.
This one's going to be called find_record.
So def find_record().
And now what we're going to do is define a variable, which gets the results of our get_record() function.
So remember that's a little helper function that we wrote to actually find the information.
And we have a cursor object here, which consists of the dictionary containing the results.
So if we do have some results, then we want to print a blank line first.
And then we're going to use a for loop to iterate through the keys and values.
So for k,v in doc.items().
So we're calling the items method here to step through each individual value in our dictionary.
The first thing we want to check is if the key is not equal to ID.
ID, if you remember, is that default key that is created by Mongo.
We don't want that to be displayed.
So if the key is not equal to ID, then let's print the key.
We will put the capitalized method on it, so that we get the first letter capitalized.
Plus a colon.
And then plus the value again with the first letter capitalized.
Remember that we stored our data in lowercase, so we want to capitalize it again, just to make it look nice.
So now all that's left to do here is to go to our option 2, take out our print statement, and call our find_record() function.
So let's go back to our command prompt and see if that works.
So open the terminal window.
Just clear this.
We'll run it again.
And what we should be able to do this time, then, is select option 2, which will prompt us for a first name and a last name.
So let's search for Saoirse Ronan again.
And here we can see that all of the data is returned: actress, name, gender, date of birth, nationality, and hair color.
Now our find functionality, as we said, will work for any record in the database.
And we'll try with Terry Pratchett.
You can see that we get his information back.
And because of the fact that we're using the lower() function, we can actually put in any mix of case here if we want, and it will still find the data correctly.
We've tested our find functionality.
We now know that that's working.
So let's go back into our project and add the functionality to edit a record.
So, again, after our find_record() function, we'll add in some blank lines here.
We want to define a new function called edit_record().
Again, we are going to store the results of our get_record() function in doc.
And we're going to check to see if there is something in the dictionary, so if doc:
And this time, we're going to create an empty dictionary called update_doc, and we're going to add to that dictionary. We're going to build that as we go on to iterate through our keys and our values.
That dictionary will form the basis of what we're going to use and insert into the database.
So we'll print another blank line.
Again, it's the same thing. We're going to iterate through using k,v in doc.items.
And yet again, we also want to filter out the ID field. We don't want to be editing that.
So what we're going to do this time, then, is that as we're iterating through, we're going to add our update_doc dictionary.
So we're going to provide the key for our update_doc dictionary.
And the value for that is going to be equal to our input here.
So we're going to capitalize the k as our prompt.
I'm going to put a + "["+ v +"]
And the reason that we're doing this is that we want the value to appear in these square brackets so that we can see what the current value of them is.
So k is the key, and v is the value.
We should get a prompt that contains both the key and what the value is currently set to.
Now, we don't always want to change every single piece of information.
So what we're going to do here is put in a check.
If we haven't actually entered anything for update_doc, if we've just left it blank, we don't actually want to delete the information that's in there.
We just want to leave it the same as it was before.
We're going to set update_doc[k] back to the value of v.
And that keeps it at its original setting.
So when we've done that, let's come right back out on the indent here.
I'm going to have a try except block again.
And we're going to call the coll.update_one() method.
It's the current document that we want to update.
We put the set keyword in there with the $ in front.
And then the dictionary that we're going to pass in is our update_doc dictionary that we just created.
Print a blank line.
And then say that our document was updated.
And now for except, we have an error, so we'll have our usual "Error accessing the database".
So from here, all we need to do now, then, is edit our menu.
We want to take out option 3, our print statement, and instead, we're going to put in our edit_record() function that we've just created.
Let's save that.
We'll go back to our terminal window, and we'll run our project and see what happens.
So this time, when we select option 3, we're asked again for a first name and a last name.
And now, as we can see, we get each of the keys and the values coming up.
Now, we've left the first two blank. kWe don't want to change those.
Let's say that she's starring in a new film, so this time she has to have brown hair.
So we'll change that.
And let's even say that she's changing her occupation. She's now a CEO.
Everything else stays the same.
So document has been updated.
Now if we select option 2 to find the record by name, so again Saoirse and Ronan, we can see indeed that her hair color and her occupation have been updated.
So when we left it blank, the default values were kept for the field.
We're coming up to the end then. All we need to do now is add delete functionality.
So while we're in our menu, we might as well change option 4 straightaway.
We're going to call our next function delete_record().
So we'll call that.
And then, after our edit_record() function, let's define it.
def delete_record():
And, as we're now familiar with, we're going to store the results of our get_record() function in the doc variable.
And we're going to check if any results were returned.
If they are, print a blank line.
Then, much as we did with our find() function, we're going to iterate through and print each of the values.
And the reason we're going to do this is we want to be sure that we're deleting the right document.
We don't just want to go ahead and delete it without asking and confirming.
So again, we'll filter out the ID.
We'll capitalize our key.
We'll have a colon separating the key and the value.
We'll capitalize the first letter of our value as well.
So now we've done that, we'll print a blank line.
And this time, we're going to create another variable called confirmation.
And that's going to store the results of an input statement.
And our input is going to prompt us to say 'Is this the document you want to delete?'.
We'll put a new line in there and then have Y or N, yes or no, and our little greater than sign as well to provide a prompt.
And, of course, another blank line underneath.
So we're asking the user for confirmation that this is the document they actually want to delete.
So we'll just move the screen up a bit.
If confirmation.lower() == 'y', then we're going to try coll.remove.
And we're going to remove the doc.
Then we'll print document deleted.
If that doesn't work, then we put our exception in place.
And we're going to print "Error accessing the database".
And finally, we'll just add an else statement here to our if.
So if we type anything other than Y, then it's just going to print "Document not deleted" and return to the main menu.
So that's our delete function that's now up and working as well.
Let's just clear the screen and test it.
So when we ask to delete a record, choosing option 4, then, yet again, we're asked for a first name and a last name.
So the name that we will provide this time is Terry Pratchett.
So yes, that finds him.
Is this a document you want to delete?
We won't type Y or N. We will type F, so we get "Document not deleted".
We'll try that again.
I'll enter Terry Pratchett.
We find him, and this time we will answer Y.
Document has been deleted.
So now when we try to find him with option 2 Terry Pratchett, we get no results found.
Well done for completing this module.
We've learned how to create a MongoDB database in the cloud and how to add data to it using the mLab web frontend.
We've also seen how to install Mongo's command-line tools and use them for the basic CRUD operations of creating, reading, updating, and deleting data.
Finally, we saw how to talk to a MongoDB database using the pymongo library in Python and brought all of that together in our walkthrough project at the end of this module.
In future lessons, we're going to look at how to get this displaying on your own website by integrating Python, pymongo, and the Flask mini framework.


import pymongo
import os

MONGODB_URI = os.getenv("MONGO_URI")
DBS_NAME = "myTestDB"
COLLECTION_NAME = "myFirstMDB"

def mongo_connect(url):
    try:
        conn = pymongo.MongoClient(url)
        print("Mongo is connected!")
        return conn
    except pymongo.errors.ConnectionFailure as e:
        print("Could not connect to MongoDB: %s") % e


def get_record():
    print("")
    first = input("Enter first name > ")
    last = input("Enter last name > ")

    try:
        doc = coll.find_one({'first': first.lower(), 'last': last.lower()})
    except:
        print("Error accessing the database")
    
    if not doc:
        print("")
        print("Error! No results found.")
    
    return doc


def show_menu():
    print("")
    print("1. Add a record")
    print("2. Find a record by name")
    print("3. Edit a record")
    print("4. Delete a record")
    print("5. Exit")

    option = input("Enter option: ")
    return option


def add_record():
    print("")
    first = input("Enter first name > ")
    last = input("Enter last name > ")
    dob = input("Enter date of birth > ")
    gender = input("Enter gender > ")
    hair_colour = input("Enter hair colour > ")
    occupation = input("Enter occupation > ")
    nationality = input("Enter nationality > ")

    new_doc = {'first': first.lower(), 'last': last.lower(), 'dob': dob,
               'gender': gender, 'hair_colour': hair_colour, 'occupation':
               occupation, 'nationality': nationality}
    
    try:
        coll.insert(new_doc)
        print("")
        print("Document inserted")
    except:
        print("Error accessing the database")


def find_record():
    doc = get_record()
    if doc:
        print("")
        for k, v in doc.items():
            if k != "_id":
                print(k.capitalize() + ": " + v.capitalize())


def edit_record():
    doc = get_record()
    if doc:
        update_doc = {}
        print("")
        for k, v in doc.items():
            if k != "_id":
                update_doc[k] = input(k.capitalize() + " [" + v + "] > ")

                if update_doc[k] == "":
                    update_doc[k] = v
        
        try:
            coll.update_one(doc, {'$set': update_doc})
            print("")
            print("Document updated")
        except:
            print("Error accessing the database")


def delete_record():

    doc = get_record()

    if doc:
        print("")
        for k, v in doc.items():
            if k != "_id":
                print(k.capitalize() + ": " + v.capitalize())
        
        print("")
        confirmation = input("Is this the document you want to delete?\nY or N > ")
        print("")

        if confirmation.lower() == 'y':
            try:
                coll.remove(doc)
                print("Document deleted!")
            except:
                print("Document not deleted")


def main_loop():
    while True:
        option = show_menu()
        if option == "1":
            add_record()
        elif option == "2":
            find_record()
        elif option == "3":
            edit_record()
        elif option == "4":
            delete_record()
        elif option == "5":
            conn.close()
            break
        else:
            print("Invalid option")
        print("")


conn = mongo_connect(MONGODB_URI)
coll = conn[DBS_NAME][COLLECTION_NAME]

main_loop()



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
