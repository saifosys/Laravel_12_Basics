✅ 1. What is Laravel?
Laravel is a framework that helps us build websites and web applications easily using PHP.

📘 What is a Framework?
A framework is like a ready-made structure. Instead of building everything from zero, we use this structure to save time and write less code.

Laravel is written in PHP. PHP is a server-side programming language used to build dynamic websites.

💡 Real-life example:
Imagine you are building a house:

Normal PHP is like starting with bricks, cement, and sand.
Laravel is like starting with the full structure ready — you just add your furniture and decoration.
Laravel already gives you:

Login system
Registration
Database connection
Email sending
And much more — all ready-made.
✅ 2. Why Use Laravel?
Laravel helps developers in many ways:

✔️ Easy to Use
Code is clean and simple. Even beginners can understand it.

✔️ Saves Time
Many features are already built-in. You don’t need to write everything from scratch.

✔️ Secure
Laravel protects your website from hackers (like SQL Injection, CSRF attacks, etc.).

✔️ MVC Structure
Laravel follows MVC (Model-View-Controller), which separates your logic and design. This makes code more organized.

✅ 3. What Do You Need to Work with Laravel 12?
Before you can work with Laravel, you need to install:

Tool	Use
XAMPP	Local server (Apache + MySQL)
VS Code	Code editor to write and manage code
Composer	PHP package manager to install Laravel
✅ 4. What is XAMPP? (Windows)
Laravel needs a local server to run. In real life, websites run on real servers like Hostinger or GoDaddy.

But on your own computer, we use XAMPP to create a local server.

🧰 XAMPP includes:
Apache: runs your website locally
MySQL: stores your website’s data (like user details, orders, etc.)
PHP: the language Laravel is based on
🔧 How to Install XAMPP (Step-by-Step):
Visit: https://www.apachefriends.org/index.html
Download XAMPP for Windows (with PHP 8 or above)
Install it like normal software (Next → Next → Finish)
Open XAMPP Control Panel
Click Start next to:
Apache
MySQL
✅ Once both are green, your local server is ready!

✅ 5. What is VS Code (Visual Studio Code)?
VS Code is a free code editor. It is like Microsoft Word — but for coding.

You will use it to:

Write Laravel code
Open Laravel project folders
Run commands in terminal
Organize your files easily
🔧 How to Install VS Code:
Visit: https://code.visualstudio.com/
Download and install it
Open it and install helpful extensions:
PHP Extension Pack
Laravel Snippets (optional)
✅ 6. What is Composer?
Composer is a tool for PHP that helps you:

Install Laravel
Manage Laravel dependencies (packages)
Keep your project updated
💡 Real-life example:
Composer is like a shopping app for developers. You search and install packages (tools) directly into your Laravel project.

✅ 7. How to Install Composer on Windows
Go to: https://getcomposer.org/download/
Download the Composer-Setup.exe file
Run the installer
When asked to select PHP, browse to this path: C:\xampp\php\php.exe (This is where XAMPP installs PHP on Windows)
Complete the installation
✅ To Check if Composer is Working:
Press Windows + R, type cmd, and press Enter
In Command Prompt, type: composer -V
You should see: Composer version 2.x.x
Now Composer is ready!

✅ 8. How to Install Laravel 12 (Windows Command)
Let’s now install Laravel and create your first project.

🛠 Step-by-Step:
Open Command Prompt (cmd)
Go to the folder where you want to create your project (example: Desktop) cd Desktop
Now create your Laravel project using Composer: composer create-project laravel/laravel laravelapp
Here:

laravelapp is your project name — you can choose any name you want.
⏳ Wait a few minutes while it downloads all the files.

✅ 9. Open Laravel Project in VS Code
After Laravel is installed:

Open Visual Studio Code
Click File > Open Folder
Select the folder: laravelapp
Your Laravel project is now open in VS Code
✅ 10. How to Start Laravel Server (Windows Command)
To run your Laravel website, use Laravel’s built-in server.

🛠 Steps:
In VS Code, click on Terminal > New Terminal
In terminal, type: php artisan serve
It will show something like: Starting Laravel development server: http://127.0.0.1:8000
Open your browser and visit: http://127.0.0.1:8000
✅ You should see the Laravel Welcome Page!

🎉 Congratulations! You have successfully set up Laravel 12 on Windows.

✅ 11. Common Errors and Fixes
❗ Error: “php not recognized”
✅ Fix:
Make sure XAMPP’s php.exe is added to your system PATH during Composer install.


check out my full Laravel 12 tutorials on [saifosys](https://saifosys.com)
