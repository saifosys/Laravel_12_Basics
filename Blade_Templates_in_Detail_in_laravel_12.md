### Blade Templates in Detail (Layouts, Includes, Loops, Conditions, Echoing)

Till now, in our Student Management System, we’ve built full CRUD operations — we can add, edit, delete, and list students with their name, email, class, and phone.

If you look at our blade files right now (index.blade.php, create.blade.php, edit.blade.php), you’ll notice they only have the main content:

Index page → has the student table.

Create page → has the form to add student.

Edit page → has the form to update student.

### 👉 But in a real-world project, every page should also have common elements like:

A header (e.g., “Student Management System”)

A navigation bar (links like Home, Add Student)

A footer (e.g., copyright notice)

Now, we could just copy-paste this header, nav, and footer into each page. But imagine tomorrow we need to change the header text or add a new menu link. That means we’d have to open every single file (index, create, edit, and all future pages) and edit them one by one. That’s repetitive, messy, and not professional.

### 👉 And this is exactly why Laravel gives us Blade Templates.

### 1. What is Blade?

Blade is a template engine that Laravel uses for views.

Instead of writing messy PHP in HTML, Blade gives us shortcuts.

Example:

Normal PHP → ``` <?php echo $student->name; ?> ```

Blade → {{ $student->name }}

👉 Much cleaner!

All Blade files end with:

.blade.php


And live in:

resources/views/

Blade allows us to:

Create layouts → In Laravel layouts are like master templates that define the common strucure of your web pages (like header, footer etc). Instead of writing the same HTML code again and again for every page, we create one layout file and extend it in other views.

Use includes → for small reusable parts (like nav bar).

Add loops and conditions easily in views.

Echo (print) values in a clean way.

So today, in this section, we’ll take our project to the next level by making it more organized and professional using Blade.


### 3. Blade Layouts (Master Template)
💡 Analogy

Think of a school letterhead. Every certificate (fee receipt, mark sheet, TC) has the same header/footer, but the middle content changes.

That’s exactly what layouts do.

Main Directives
Directives are special blade commands that make your templates smarter they usually start with @ in Blade.
```
1. @extends

Meaning: It tells Blade that this view should inherit from a layout (master template).

Where to use: At the top of child views (like home.blade.php, about.blade.php).

When to use: Whenever you want your page to use a common layout instead of writing HTML again and again.


👉 Example:

@extends('layouts.app')

This means:
➡ “Use resources/views/layouts/app.blade.php as the base structure for this page.”


---

🔹 2. @yield

Meaning: Placeholder in the layout that says:
👉 “Here child view content will be inserted.”

Where to use: In the layout file (layouts/app.blade.php).

When to use: Whenever you want to define a “slot” where child views can inject content.


👉 Example in layouts/app.blade.php:

<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    <div class="container">
        @yield('content')
    </div>
</body>
</html>

Here @yield('title') and @yield('content') are slots for child views.


---

🔹 3. @section

Meaning: Defines the actual content for a section that was declared with @yield.

Where to use: In child views that @extends a layout.

When to use: To fill in the content for a @yield placeholder.


👉 Example in home.blade.php:

@extends('layouts.app')

@section('title', 'Home Page')   {{-- Short syntax for one-line section --}}

@section('content')
    <h1>Welcome to the Home Page</h1>
    <p>This is dynamic content.</p>
@endsection


---

🔹 4. @endsection

Meaning: Marks the end of a @section block.

Where to use: After you write all the content inside a section.

When to use: Always after @section (except when using the short one-line form like @section('title', 'Home')).


👉 Example:

@section('content')
    <h1>About Us</h1>
    <p>We are a Laravel company.</p>
@endsection


---

✅ How They Work Together (Flow)

1. Layout (layouts/app.blade.php)



<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    <header>My Website</header>

    <div class="main">
        @yield('content')
    </div>

    <footer>Footer here</footer>
</body>
</html>

2. Child View (home.blade.php)



@extends('layouts.app')   {{-- Inherit layout --}}

@section('title', 'Home Page')   {{-- Fill the @yield('title') slot --}}

@section('content')   {{-- Fill the @yield('content') slot --}}
    <h1>Welcome Home!</h1>
    <p>This is home page content.</p>
@endsection


---

🎯 Simple Analogy

@extends → “I want to use this master template.”

@yield → “Here is a blank space in the template where content will go.”

@section → “Here is the content I want to put into that blank space.”

@endsection → “I’m done writing content for this section.”



---

✅ Where & When to Use:

Use @extends → in child views (at the top).

Use @yield → in layouts (to define placeholders).

Use @section → in child views (to provide content for @yield).

Use @endsection → to close @section.
```
### Step 1: Master Layout

📂 resources/views/layouts/app.blade.php
```
<!DOCTYPE html>
<html>
<head>
   <title>@yield('title')</title>
</head>
<body>

   <h1>Student Management System</h1>
   <hr>

   {{-- Navigation --}}
   @include('layouts.nav')

   <hr>

   {{-- Page-specific content --}}
   @yield('content')

   <hr>
   <footer>
      <p>© {{ date('Y') }} My School</p>
   </footer>

</body>
</html>
```
### Step 2: Child Page (Students List)

📂 resources/views/students/index.blade.php
```
@extends('layouts.app')

@section('title', 'Students List')

@section('content')
   <h2>All Students</h2>

   <a href="{{ route('students.create') }}">+ Add New Student</a>

   <table border="1" cellpadding="8">
       <tr>
           <th>ID</th>
           <th>Name</th>
           <th>Email</th>
           <th>Class</th>
           <th>Phone</th>
           <th>Action</th>
       </tr>

       @foreach($students as $student)
       <tr>
           <td>{{ $student->id }}</td>
           <td>{{ $student->name }}</td>
           <td>{{ $student->email }}</td>
           <td>{{ $student->class }}</td>
           <td>{{ $student->phone }}</td>
           <td>
               <a href="{{ route('students.edit', $student->id) }}">Edit</a> |
               <form action="{{ route('students.destroy', $student->id) }}" method="POST" style="display:inline;">
                   @csrf
                   @method('DELETE')
                   <button type="submit">Delete</button>
               </form>
           </td>
       </tr>
       @endforeach

   </table>
@endsection
```
### 4. Blade Includes (Reusable Parts)
An Include is like a resuable partial file. It just inserts another blade file inside your view. good for repeating small parts like navbar, sidebar,form etc

📂 resources/views/layouts/nav.blade.php
```
<nav>
   <a href="{{ route('students.index') }}">Home</a> |
   <a href="{{ route('students.create') }}">Add Student</a>
</nav>
```

📂 In app.blade.php:
```
@include('layouts.nav')
```

👉 Update once, and it changes everywhere.

### 5. Loops in Blade (Where We Use Them)

We use loops whenever we need to show multiple items.

👉 In our project, loops are used for:

Students list table in index.blade.php.

Class dropdown in add/edit forms.

Example (dropdown):
```
<select name="class">
   @foreach($classes as $class)
       <option value="{{ $class }}">{{ $class }}</option>
   @endforeach
</select>
```

### 6. Conditions in Blade (Where We Use Them)

We use conditions to decide what to show based on data.

👉 In our project:

If no students exist → show “No students found.”

If students exist → show the count.

Example:
```
@if($students->isEmpty())
   <p>No students found. Please add one.</p>
@else
   <p>Total students: {{ $students->count() }}</p>
@endif
```

### 7. Echoing Data (Printing Values)

{{ $student->name }} → prints safely.

{!! $student->bio !!} → prints raw HTML (⚠ careful).

👉 In our project:

<p>Name: {{ $student->name }}</p>
<p>Email: {{ $student->email }}</p>
<p>Class: {{ $student->class }}</p>
<p>Phone: {{ $student->phone }}</p>

### 8. How Blade Directives Work Together (Step by Step Flow)

Browser → /students

Route → calls StudentController@index

Controller → return view('students.index', ['students' => $students]);

View (index.blade.php) → says @extends('layouts.app') → loads master layout

Master layout → already has header, nav, footer

Sections → fill in the placeholders

Final HTML → sent to browser 🎉

### 9. Why Blade is Powerful (Recap)

Layouts → School letterhead (same design everywhere).

Includes → School stamp/signature (reused).

Loops → Attendance register (repeat per student).

Conditions → Pass/Fail remark (depends on data).

Echo → Print student details.

👉 Without Blade → repetitive, messy.
👉 With Blade → neat, DRY, professional.
