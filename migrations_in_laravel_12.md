We created our database in phpmyadmin and connected it to laravel using the .env file. Now Lets see how to create Tables  and columns in Laravel 121 

### 1 - Problem
Normally, when we want a table, we go to phpMyAdmin, click create table, and add columns manually. That works fine if I’m working alone.  

But imagine a team of 5 developers – each person has to create the same table manually on their computer. This can cause mistakes, and it becomes difficult to keep everyone’s database the same

### 2 - To solve this, Laravel provides Migrations.  

Think of migrations like a blueprint for your database tables.  

Instead of manually creating tables in phpMyAdmin, we write small PHP files that describe the table structure – which table to create, and which columns it should have.  

Then, with a single command, Laravel automatically creates the table in the database. And if another developer runs the same command, they get the exact same table.  

So migrations make teamwork easy and keep our database consistent.  


### 3 — Where are Migrations Stored?

All migration files are kept in:

database/migrations/

Laravel creates some default migration files when you make a new project (like users and password_resets tables).

### 4 — Migration File Naming

Migration file names have a timestamp followed by a description:

2025_08_15_103500_create_students_table.php


The timestamp ensures migrations run in the order they were created.

The description explains what the migration does.

### 5 — How to Create a Migration (Windows Command)

To create a new migration for our students table:

php artisan make:migration create_students_table


This will create a new file in database/migrations with two methods inside:

public function up()  
{  
    // Code to create table  
}  

public function down()  
{
    // Code to delete table  
}  

### 6 — Understanding up() and down() Methods

up() → This method runs when we apply the migration. It usually contains code to create or modify a table. In the up() function, we tell Laravel what table and columns we want.  

down() → This method runs when we rollback (undo) the migration. It usually contains code to drop (delete) the table or reverse the change.

### 7 — Example: Creating Students Table

Here’s an example migration for our project:

public function up()  
{  
    Schema::create('students', function (Blueprint $table) {  
        $table->id(); // Creates an ID column that Auto-increments      
        $table->string('name'); //creates a column for student names  
        $table->string('email')->unique(); //creates an email column that must be unique   
        $table->string('class'); //creats a column for class  
        $table->timestamps(); // Adds two extra columns, created_at & updated_at automatically managed by Laravel.  
    });  
}  

public function down()  
{  
    Schema::dropIfExists('students');  
}  
Here laravel gives us a special class called Schema. with it, we say: create a table named students.
so here we are creating a table named students with these columns: id, name, email, class, created_at and updated_at

### There are lot of column types and column modifiers in Laravel you can find and read what each one is from the official website https://laravel.com/docs/12.x/migrations  

### 8 — Running Migrations

Once your migration is ready, run:

php artisan migrate

Laravel will:

Look at all migration files that haven’t been run yet

Create those tables in your database  

This will create the students table in our database. Let’s check in phpMyAdmin… and yes, here’s our students table with all the columns we defined

### 9 — Rolling Back Migrations

If you made a mistake and want to undo the last migration, you don't have to delete the table manually. Laravel gives us a rollback option:

php artisan migrate:rollback


If you want to reset all migrations:

php artisan migrate:reset


If you want to drop all tables and run everything from scratch:

php artisan migrate:fresh

 

### 10 — Changing Tables with Migrations

If you already have a table and want to add a new column:  

php artisan make:migration add_phone_to_students_table --table=students


Inside migration:

public function up()  
{  
    Schema::table('students', function (Blueprint $table) {  
        $table->string('phone')->nullable();  
    });  
}  

public function down()  
{  
    Schema::table('students', function (Blueprint $table) {  
        $table->dropColumn('phone');  
    });  
}  

### 11 — Best Practices for Migrations

Never edit a migration that’s already been run in production — instead, create a new migration to change the table.  

Name your migrations clearly so others know what it does.  

Use rollback often in development to fix mistakes quickly.  

<img width="2048" height="2048" alt="migrations-in-laravel-12" src="https://github.com/user-attachments/assets/920d0f6f-bb8f-4008-b977-a33201a3d773" />
