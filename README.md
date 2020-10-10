#reddit-wallscraper

Class: CS41\
Date: March 11, 2020\
Project Partners: Christopher Moffitt & Elizabeth Fitzgerald\
Google Drive URL for Presentation: https://drive.google.com/file/d/1G11pi4g7jpeK87NpjaQiVwEEr8vu6Rvx/view\
=========================================
##Brief summary
Reddit-Wallscraper scrapes reddit to find the current top 25 wallpapers and downloads them locally to a wallpapers directory\
Here is an example wallpaper that was scraped from reddit:
!(examples/WindowsVistaimg31NORTHERNLIGHTSDENALINATIONALPARKALASKA.png)


#Requirements:
-------------------------
Simply download the code as is, and then run the main python script entitled wallscraper.py and the command line argument (wallpapers).
ex:
python wallscraper.py wallpapers

For periodical running extension (much more complicated).
Save wallpapers.plist file to LaunchAgents folder.
Then enter the following in Terminal in order to start the launchd:
$ launchctl load ~/Library/LaunchAgents/wallpapers.plist
$ launchctl start wallpapers

=========================================
#Technical Details:
-------------------------
This project performs several tasks. It scrapes data, downloads that data, avoids downloading the same image twice, allows for command line utility, and periodically runs itself. Each of these tasks was made up of smaller parts that came together to make it whole.
-------------------------
##Task 1: Scraping Data
-------------------------
(A) Better familiarize ourselves with JSON objects and how to work with them
 --

(B) Write the query code to collect the JSON objects from reddit
 --

(C) Build a class for Reddit Posts that stores the most relevant JSON information as attributes
 -- In order to do this, we had to organize the collected JSON data into a single, neat dictionary in the __init__ function. The dictionary only collects certain attributes (those in the attr list). With these attributes in mind, the function goes through each post characteristic scraped from the JSON data, and stores only the desired attributes in a dictionary. If a post lacks a particular attribute, then the attribute is assigned the value of None.

-------------------------
##Task 2: Downloading Data
-------------------------
(A) Implement the download function in the RedditPost class (moderate)
 -- The download function only runs on posts whose urls contain the string ".jpg" or ".png"
 -- The download function sorts images into different folders based on their size, and titles them in the format "wallpapers/[image size]/[title].png".
 -- When creating this function, we had to deal with the case of the path to a folder not existing. The command "os.makedirs(path)" accounts for this.
 -- Once the path is known to exist, we use the requests package to collect the content of the post's url, and then write that to the existing filepath.

(B) Better familiarize ourselves with magic methods and implement the __str__(self) method
 --  This function simply allowed us to print out post data more cleanly by printing the object itself. It was fairly simple, and only required basic string concatenation.

(C) Test downloading one image
 -- We started by simply downloading the first collected image post, to confirm that it went to the right folder.

(D) Download all images generated by initial query
 -- Once we downloaded one image correctly, we simply ran all the RedditPost objects through a for loop in the main function.

-------------------------
##Task 3: Wallpaper Deduplication
-------------------------
(A) Keep track of previously seen images
 -- In order to keep track of previously seen images across different instances of the program, we used the pickle package.
 -- We created a list of seen wallpapers and saved it to the project folder as a pickle object. We then saved every new post's url content to the list.

(B) Check new images against those already seen
 -- With this object, we can load it any time we're saving a new post, scan through it to make sure there are no matching content, then save the new wallpaper's content to the list before dumping the list back into the pickle object.

-------------------------
##Task 4: Implementing Command Line Utility
-------------------------
(A) Allow the user to specify which subreddit posts to download through command line arguments
 -- We did this by importing the sys package
 -- We then just had to pass sys.argv[1] to our query function
-------------------------
Task 5: Configuring the script to run automatically
-------------------------
(A) Work with Mac LaunchAgents to have our wallscraper script run every hour
 -- Create a new .plist file for wallscraper in LaunchAgents and make the start interval for every hour
 -- Use terminal to start the launch
