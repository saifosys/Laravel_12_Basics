Let’s begin with the basic foundation of any Laravel app:

Route (Receives the request)

Controller (Handles the logic)

View (Shows the final web page)

And today we’ll learn exactly how to write code for these three things — with full syntax explanations so that even beginners can understand.

1️⃣ ROUTE – Syntax and Structure

💡 What is a Route?

A Route in Laravel listens to the URL typed in the browser and decides what to do next.

📚 Real-Time Example:
Imagine a visitor walks into a school and goes to the reception desk. They say, "I want to talk to the Head Teacher." The receptionist checks their register and says, “Okay, please go to Room 102.”

That’s exactly what a Laravel Route does. It listens to incoming requests and sends them to the correct place — like a controller.

📁 Where is the Route file?

routes/web.php

This is where we write all our web routes.

📘 Route Syntax

Route::get('/url-path', function () {
    return 'Message or View here';
});

🧠 Explanation:

Route::get() → This is the method for a GET request. (Browser visits are GET requests.)

'/url-path' → This is the route/path in the browser (like /about, /contact)

function () { ... } → Anonymous function (closure) that runs when the route is matched

return 'text'; → Sends a message or a view back to the browser

✅ Example:

Route::get('/about', function () {
    return 'This is About Page';
});

But this becomes messy when the project grows. That’s why we move logic to a controller.

👉 Controller-Based Route:

Route::get('/about', [PageController::class, 'about']);

This means: “When someone visits /about, run the about() method from PageController.”

You must also write this at the top of the file:

use App\Http\Controllers\PageController;

🔁 Route Methods:

Laravel supports multiple HTTP request types. Here are some:

Route::get() – for displaying pages (read)

Route::post() – for submitting forms

Route::put() – for updating records

Route::delete() – for deleting records

✅ So the flow becomes:

Route catches request

Sends it to controller method

That method returns a view

2️⃣ CONTROLLER – Syntax and Structure

💡 What is a Controller?

A Controller is where your main logic lives. It’s like the Head Teacher in the school — the person who decides what to do when a request comes.

📚 Real-Time Example:
Let’s say a student wants a bonafide certificate. They go to reception (Route), who then forwards them to the Head Teacher (Controller). The Head Teacher checks if the student is eligible and gives the approval.

That decision-making part is done by the Controller.

📁 Where is the Controller stored?

app/Http/Controllers/

🛠️ How to create a Controller?

Use this command in your Windows terminal (CMD or PowerShell):

php artisan make:controller PageController

✅ What does this command do?

php → Runs the PHP interpreter

artisan → Laravel’s built-in command line tool

make:controller → Tells Laravel to generate a controller file

PageController → This is the name of your new controller

After running it, Laravel will create this file for you:

app/Http/Controllers/PageController.php

📘 Controller Syntax

<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class PageController extends Controller
{
    public function about()
    {
        return view('about');
    }
}

🧠 Explanation:

namespace App\Http\Controllers; → This tells Laravel the folder location of this file

use Illuminate\Http\Request; → This allows handling data sent from forms, etc. (We’ll use it later)

class PageController extends Controller → Defines our custom controller class

public function about() → A method (function) that runs when the /about route is visited

return view('about'); → Laravel will show the about.blade.php file from the resources/views folder

You can create multiple methods in the same controller:

public function home() {
    return view('home');
}

public function contact() {
    return view('contact');
}

🧠 Controller Naming Tips:

Controller names should end with Controller (like StudentController, PageController)

Method names usually match the action or page (like index, create, store, edit, etc.)

✅ So the controller’s job is to act as the traffic manager — deciding which page (view) to show.

✨ A Little About Models

📚 Real-Time Example:
Let’s say the Head Teacher needs a student’s marks before signing the bonafide certificate. They’ll ask the clerk to get the student’s file.

That clerk is the Laravel Model.

So the Model talks to the database and brings data to the Controller.

🧠 In short:

Route = Receptionist (listens)

Controller = Head Teacher (decides)

Model = Clerk (fetches records)

View = Certificate/Notice (shown to student)

We’ll explore Models more deeply in Part 4.

3️⃣ VIEW – Syntax and Structure

💡 What is a View?

View is the final HTML page shown to the user — like the printed notice or certificate the visitor finally receives.

📚 Real-Time Example:
After the Head Teacher approves a certificate, the office staff prints it and gives it to the student. That printed paper is what the student actually sees. That’s the View in Laravel.

📁 Where are Views stored?

resources/views/

🔖 Laravel uses Blade

Blade is Laravel’s templating engine. Views in Laravel are Blade files and end with .blade.php

Blade lets you write clean, short, and reusable HTML with dynamic data.

✅ File Name Example:

home.blade.php

📘 Blade File Syntax (Basic HTML for now)

<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Welcome to Student Management System</h1>
    <p>This is the homepage.</p>
</body>
</html>

Just plain HTML for now.

Later we’ll use special Blade tags like:

{{ $name }} → To display student name or data

@foreach → For listing students

@if → For checking conditions

@extends, @section → For layout system

But don’t worry — we’ll learn them step-by-step later. For now, just basic static pages.

🧠 How Controller sends View:

return view('home');

Laravel automatically looks for:

resources/views/home.blade.php

⚠️ View Naming Tips:

No capital letters or spaces in view filenames

Use underscores or hyphens if needed (e.g., student_list.blade.php)

✅ Benefits of Blade:

Easy to use

Cleaner than writing raw PHP inside HTML

Can use loops and conditions like in programming
