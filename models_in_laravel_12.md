### Introduction to Models

“Till now, we have created our database and also learned how to create tables using migrations. But how do we use those tables in Laravel? How do we insert, update, delete, or fetch data from them?
That’s where Models come in.”

In simple words:

A Model is like a bridge between your database table and your Laravel code.

Each model usually represents one database table.

Example: If we have a students table, we create a Student model to work with it.


### Where are Models Stored?

By default, Laravel stores models in the app/Models folder.
When you create a model, it automatically appears there.


### Creating a Model

In Laravel, we create a model using the artisan command.  

php artisan make: model Student  

This will create a new file inside the app/Models folder.

Inside the file, you’ll see something like this:  

"<?php  
namespace App\Models;  

use Illuminate\Database\Eloquent\Factories\HasFactory;  
use Illuminate\Database\Eloquent\Model;  

class Student extends Model  
{  
    use HasFactory;  
}"  

The file will look empty, but don’t worry — Laravel already gives it all the basic features to work with the table.

— Breaking Down the Model Code
Line by Line:

namespace App\Models;
→ Tells Laravel this class is inside the app/Models folder.

use Illuminate\Database\Eloquent\Model;
→ We are extending Laravel’s base Model class (Eloquent ORM).

class Student extends Model
→ This defines our custom Student model that connects to the students table.

use HasFactory;
→ This trait allows us to quickly create fake test data (we’ll see later).


By default, the model name is singular (Student) and Laravel will assume the table name is plural (students).

### Connecting Model with Database

“By default, Laravel is smart. If the model name is Student, it looks for students table.
But if our table name is different, we need to tell Laravel like this:”

protected $table = 'my_students';

You add this line inside the Student model.

This tells Laravel the exact table name.

### Using Models in Practice (Theory Only)

Introduction

“Now we know what a model is. Let’s understand how we can use it to work with our database. 
I will show you the code and explain what each part does. Don’t worry about running it now — later we will see the practical implementation using Controller + Route + View.”


### 1. Insert Data

$student = new Student();  
$student->name = "Saif";  
$student->email = "saif@saifosys.com";  
$student->class = "10A";  
$student->phone = "1234567890";  
$student->save();   

Explanation:  

$student = new Student(); → Creates a new student object.  

$student->name = "Saif"; → Assigns the name.  
$student->email = "saif@saifosys.com"; → Assigns the email.   
$student->class = "10A"; -> Assigns the class.  
$student->phone = "1234567890"; -> Assigns the phone.  


$student->save(); → Saves this record to the database.  

### 2. Fetch All Data  

$students = Student::all();  

Explanation:  

Student::all() → Retrieves all records from the students table.  

You can then display them in the browser or use in your code.  


### 3. Fetch Specific Data by ID  

$student = Student::find(1);  

Explanation:  

find(1) → Fetches the student with ID = 1.  


### 4. Update Data  

$student = Student::find(1);  
$student->name = "Updated Name";  
$student->save();  

Explanation:  

First we fetch the student.  

Then we change the name.  

Finally, we save it back to the database.  


### 5. Delete Data  

$student = Student::find(1);  
$student->delete();  

Explanation:  

Finds the student by ID.  

Deletes the record from the database.  

### Summary (End of Class)

Models represent database tables.  

Naming convention: Model = singular, Table = plural.  

We use model methods like all(), find(), save(), delete() to work with database.  

Now we will see the practical session of CRUD in laravel 12 using a route, controller, model and views.   
crud means create,red,update,delete. these are the basic operations we perform on data operations.  


