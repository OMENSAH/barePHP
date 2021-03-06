
# Documentation

## structure
`application folder` : is where the whole framework files are created.

`public folder` : contains all static files like images, css, js, etc.

## application folder sub-structure
`404 folder`: Any incoming URL which are not associated with any controller action reroute to this folder to inform the user that the requested page does not exist in this application.
        
`Config folder`: This folder houses the configuration file. The file has information concerning database connection settings, URLROOT, APPROOT and the name for the project.

`Controllers Folder`: The C in the MVC happens in this folder. All controllers associated with an application are created here.

`Core Folder`: This contains file called Core.php that loads controllers, format incoming URL’s into MVC friendly URL. Also, it contains BaseController.php file that helps to load views and models. The remaining file in this folder is Database.php that provides an abstract layer for database operations.

` Helpers Folder`: This is where other utilities needed by developers to perform specific functions are created and housed.

`Models Folder`: The folder will house all files needed to perform and maintain data and business logic.

`Views Folder`: This folder serves as the directory where all files needed to be served to the client are housed.

` .htaccess File`: The .htaccess file helps to map the incoming URL as a querying string. With the help of .htaccess file in the root directory the domain name appends public to the URL to help get access to the starting file. For instance if the domain name is “http://oliver.com”, it becomes “http://oliver.com/public”. The .htaccess file in this directory then helps to reverse the newly created URL to the old URL.

` bootstrap.php File`: This file loads all the files and instantiate the class needed to start the entire project.
## public folder sub-structure
This is where most of the framework structure are housed and that is where developers will be using a lot to build their applications. It consists of other folders, .htaccess file and a bootstrap file. These provide utilities encompassing the entire framework, loading the files needed to start the framework;


# Important Constants 
`URLROOT ` : helps to create dyanmic links to pages, images, css, js files.
  ``` php
        <script src="<?php echo URLROOT;?>/js/main.js"></script>
  ```
  ### APPROOT  : 
  Helps to include other php files in our pages.It is used in place of PHP require/include.
  ```php
      <?php require APPROOT. '/views/inc/header.php';?>
   ```
  
# Steps to Use  barePHP

### Download the Repository.

### Rename the project folder (optional)

### Rename RewriteBase in public/.htacccess 
If you renamed the project folder, let's say the project is renamed to "goodApp" then the RewriteBase will be /goodApp/public.
If your are putting this in a server without any folder to contain the files then it should just be /public

### Rename Database info in the config/config,php to suit the information about your database
```php
    define("DB_HOST", 'YOUR SERVER HOST');
    define("DB_USER", 'YOUR USERNAME TO DATABASE');
    define("DB_PASS", 'YOUR PASSWORD TO DATABASE');
    define("DB_NAME", 'YOUR DATABASE NAME');
```
### Rename URLROOT 
```php
    define("URLROOT", "YOUR BASE URL");
```
if you have a folder to hold the framework files then the base url will be http://hostname/folder such as  http://localhost/goodApp. if you are just hosting on your domain without creating any folder to hold the files then http://domainName
### Rename SITE NAME 
```php
    define("SITENAME", "YOUR SITE NAME");
```
### Default Controller
```php
    define("DEFAULTCONTROLLER", "Pages");
```
You can rename the default controller to any controller you want but make sure rename that controller file name(Pages) to the newly used name. 

### Creating Models
The model class you will create mostly communicate with a data source and  here we are using mysql database.The  class has an property of a database which is instantiated in the models' constructor. This makes it easier to use the methods of the database in our model class.The methods of the database are:
*   query :  make a query to the database using PDO
*   bind :  binds value of passed parameters 
*   resultSet : returns an array of objects from the query
*   single : returns a single object from the query
*   rowCount: returns the number of rows affected by a query;

Example.
Here we are retrieving data from users table
```php
    class User{
        private $db;
        public function __construct(Database $database){
            $this->db = $database;
        }
        public function getUsers(){
            $this->db->query("SELECT * FROM users");
            $this->db->resultSet();
        }
    }
```
### Creating  Controllers 
Controller brings together a model and and a view. The controller extends to a base controller called BaseController which makes it easier to use its methods to work with views and models.
Example:
Here we are using our model to get our data and pass that to our view;

```php
    <?php 
       class Users extends BaseController{
        public function __construct(){ 
            $this->modelDB = $this->loadModel("User")
        }
        public function index(){
            $users = $this->postModel->getUsers();
            $data = [
                "title" =>  "My Users",
                "users" => $users
            ]
            $this->loadView('pages/index', $data);
        }
    }
```

### Working with Views. 
With the views, I have a created inc folder that holds all header and footer contents for your pages. You can choose to use it.

Example  showing how to use data from our controller in our view.
```php
    <?php require APPROOT. '/views/inc/header.php';?>
        <h1><?php echo $data['title']; ?></h1>
         <?php foreach($data['users'] as $user): ?>
            <li><h1><?php echo $user->name; ?></h1></li>
         <?php endforeach; ?>    
    <?php require APPROOT. '/views/inc/footer.php';?>
```
    


