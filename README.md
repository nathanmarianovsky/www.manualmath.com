<p align=center>
 <img src="/client/logo_with_text.png">
</p>


# Setting Up
In order to get started first copy the repository over to your local machine. Now I have to assume that you have npm and git installed, and so inside the root directory of the project as administrator run:
```js
npm install -g bower gulp
npm install gulp gulp-install
gulp
```
This will handle the installation of all node_modules, bower_components, and build the necessary gulp files.


# Changing the Content
### Physical Files
In order to change the content of the website, there are two main folders to look at:

1. At "/client/" we have "about.php" and "notation.php". The first one houses the content for the site's landing page and the second is the notation that is mentioned on this site. 

2. At "/content/" we have the rest. Inside there will be folders for each different subject. When looking at each subject there will be two html files associated that load the content of the subject alongside folders that represent the topics. Then looking inside any of the topics associated to the subject will have a single html file associated to the topic alongside folders that represent the sections. Finally looking inside any of the sections associated to the topic will have a single html file associated to the section alongside html files that represent the examples associated to the section. Sometimes there may also be image files inside the section folders, but these are just images used inside either the notes or examples.

### Changing the Database
To begin with, the database is setup according to the following ERR diagram:

<p align="center">
 <img width=500 height=500 src="/client/database_setup.png">
</p>

I provide a current build of mine that can be found in "/content/db". After setting up, add subjects, topics, sections, and examples into the database as needed. As far as names go inside the database, for any object they must match the file name and adhere to the rules:

* All " " spaces must be replaced with "_"
* All "-" characters must be replaced with "AND"
* All "'" characters must be replaced with "APOSTROPHE"
* All ":" characters must be replaced with "COLON"
* All "," characters must be replaced with "COMMA"
* Subject name, sname, is unique to a given subject

The id given to any object is completely arbitrary so long as the id is unique to that object, which essentially means that when looking at sections there can only be a single section with an id of 7, but there may exist a subject, topic, and even example that have the same id. The order is what helps provide a "natural" ordering to the objects as needed. 


# Styling
All styles associated to the website can be found inside the "/styles/dev" folder. This site is mobile-friendly and to make it easier to read, there exist different files for different screen widths. 


# Front-End Functionality
All of the functionality associated to the actual website can be found inside the "/scripts/front-end" folder. 


# Using the API
### Getting All Objects of a Certain Type
To use the API, there exist ways to extract the subjects, topics, sections, and examples from the database. So lets say that you want to get either all of the subjects, topics, sections, or examples that are available in the database. You would call on:
```
localhost/api/want
```
where "localhost" can remain if you are running a local build or replaced with the domain name and "want" represents what we want to get which can be one of four things:
* subjects
* topics
* sections
* examples

### Getting Specific Object(s)
Now what if we want to get a specific object given that we know some information that can be used to identify it? Overall I can summarize all of the calls into a single generalization:
```
localhost/api/want/param_type/param
```
where:

* want: This represents the object we want. Specifically this can be one of four things:
  * subject
  * topic
  * section
  * example
* param_type: This represents the type of data associated to the object we want. Specifically this can be one of two things:
  * id
  * name
* param: This represents the actual data value that is either the id or the name.

So for example, if I wanted to get the all the section(s) associated to the id 7 I would call:
```
localhost/api/section/id/7
```
Now if I wanted to get the example(s) whose name is "example_1":
```
localhost/api/example/name/example_1
```
Note that since example names are not unique, this will return all of the examples who have such a name. The only time when a name is going to be unique is for any subject, otherwise expect that you might get back more than one result. On the other hand, if you provide an id you are guaranteed to get a unique result.

### Getting File Contents
The API also has calls to get back the html content associated to any given subject, topic, section, or example:
```
localhost/api/want/file/id

```
where "want" is one of the four objects and "id" references the actual id of the object. Notice that unlike getting objects, you only allowed to use the id here as a parameter since the id guarantees a unique object.


# Running the Server

### Apache Server
Prior to running you need to modify the following code at "/api/config.php":
```php
$mysql_hostname = "";
$mysql_user = "";
$mysql_password = "";
$mysql_database = "";
```
and put in your credentials for the database. To configure the port you need to find the following lines inside the Apache "httpd.conf" file:
```sh
Listen 0.0.0.0:80
Listen [::0]:80
```
and change "80" to whichever port you wish to use. With Apache you may also have to change the default path so in the same configuration file find:
```sh
DocumentRoot "path"
```
and replace "path" with the project path. Lastly in the same file also change:
```sh
<Directory "path">
```
"path" once again to the desired path. While still on the topic of Apache configuration make sure that the following modules are turned on:
```sh
core_module
so_module
watchdog_module
http_module
log_config_module
logio_module
version_module
unixd_module
access_compat_module
alias_module
auth_basic_module
authn_core_module
authn_file_module
authz_core_module
authz_host_module
authz_user_module
autoindex_module
dir_module
env_module
filter_module
mime_module
mpm_prefork_module
negotiation_module
php5_module
rewrite_module
setenvif_module
status_module
deflate_module
```
Now we need to minify the necessary files so in the root directory run:
```sh
node minify.js
```
Finally you can run the Apache service!

### NodeJS Server
Prior to running you need to modify the following code at "/scripts/back-end/config.js":
```js
"host": "",
"user": "",
"password": "",
"database": ""
```
and put in your credentials for the database and at "/app.js":
```js
app.listen(8080, () => {
  console.log("The server is now listening!");
});
change "8080" to whichever port you wish to use. Now to minify all of the necessary documents and start the server run:
```js
node app.js
```
at the root directory. If you see:
```
The serve is now listening!
All html files in /client, CSS files in /styles/dev, and RequireJS have been minified!
```
the server has officially been launched and is listening on the port you provided.


# Version History

### v.0.0.4
* Logo Redesign
* Hamburger Button Fix for Mobile
* Official Documentation
* Consolidated the Initial Development Process into a Single Command
* Content++

### v.0.0.3
* Logo Redesign
* Math Formula Rendering Issue Fixed
* Images not Loading Fixed
* No More About Link on the Landing Page Menu
* Fixed Some of the Content

### v.0.0.2
* Mobile Support
* About Page - Nore More Home Page
* Subject Page - Gives a Brief Overview on a Subject and its Notation
* Topic Page - Gives a Brief Overview on a Topic
* Menu Selection Coloring
* Logo and Favicon

### v.0.0.1
* Initial Release


# Future Plans
In the near future I see a couple of things that I want to change about the website:
* Design a CMS, content management system, that will provide a user with the ability to control the contents of the website through a simple web interface rather than having to add content through a development environment
* Rewrite the API in Node.js
* Add the subjects (these represent the main ones of interest, over time the list may evolve):
 * Precalculus
 * Differential Calculus
 * Integral Calculus
 * Linear Algebra
 * Vector Calculus
 * Complex Variables
 * Probability

# License
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">manualmath.com</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/nathanmarianovsky/manualmath.com" property="cc:attributionName" rel="cc:attributionURL">nathanmarianovsky</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
