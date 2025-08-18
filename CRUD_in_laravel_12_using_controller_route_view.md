# Using Route, Controller, View, Model to perform CRUD (Create, Read, Update, Delete) operations in Laravel 12

### Step 1: Create a Controller

 php artisan make:controller StudentController

This creates app/Http/Controllers/StudentController.php.

All CRUD operations will go here.

### Step 2: Define Routes

In routes/web.php, add:  

```use App\Http\Controllers\StudentController;  

Route::get('/students', [StudentController::class, 'index']); // Show all students  
Route::get('/students/create', [StudentController::class, 'create']); // Show form to add student  
Route::post('/students', [StudentController::class, 'store']); // Save new student  
Route::get('/students/{id}/edit', [StudentController::class, 'edit']); // Show form to edit student  
Route::post('/students/{id}', [StudentController::class, 'update']); // Update student data   
Route::get('/students/{id}/delete', [StudentController::class, 'destroy']); // Delete student    
```
Each route connects a URL to a controller method.  

### Step 3: Controller Methods

a) Show All Students

```use App\Models\Student;  

public function index() {  
    $students = Student::all();  
    return view('students.index', compact('students'));  
}  
```
> index() → Fetches all students → sends to students/index.blade.php.  
> compact('students') is just a PHP function. It creates an array where the key is the variable name and the value is the variable’s content.
> Here’s what happens internally:compact('students') → turns into: ['students' => $students]
> So it’s exactly the same as writing: return view('students.index', ['students' => $students]); 

b) Show Create/Add Form

```
public function create() {  
    return view('students.create');  
}  
```
> create() → Opens a form to add a new student.


c) Store New Student in database

```
public function store(Request $request) {  
    $student = new Student();  
    $student->name = $request->name;  
    $student->email = $request->email;
    $student->class = $request->class;
    $student->phone = $request->phone; 
    $student->save();  
    return redirect('/students');  
}
```
> store() → Saves new student in DB.

d) Show Edit Form

```
public function edit($id) {  
    $student = Student::find($id);  
    return view('students.edit', compact('student'));  
}  
```
> edit() → Opens edit form with existing student data.

e) Update Student in database

```
public function update(Request $request, $id) {  
    $student = Student::find($id);  
    $student->name = $request->name;  
    $student->email = $request->email;  
    $student->class = $request->class;  
    $student->phone = $request->phone;   
    $student->save();  
    return redirect('/students');  
}  
```
> update() → Updates student record in DB.

f) Delete Student

```
public function destroy($id) {  
    $student = Student::find($id);  
    $student->delete();  
    return redirect('/students');  
}  
```
> destroy() → Deletes student record.

### Step 4: Create Views

Create views in resources/views/students/:  
> “Right now, our app has only one module — Students. But in real projects, there can be many modules like Teachers, Courses, Products, Orders, and so on.”
> “If we put all the Blade files directly inside the views folder, everything will get mixed up and confusing. We won’t know which file belongs to which module.”
> “To keep our project organized, Laravel developers follow a folder structure.
> We create a separate folder for each module inside resources/views/.
> For example:
> students/ → all student-related views (list, add, edit, etc.)
> teachers/ → all teacher-related views
> courses/ → all course-related views”
> “So now we’ll create a folder named students inside resources/views/.
> Inside that folder, we’ll put files like:
> index.blade.php (list all students)
> create.blade.php (form to add a student)
> edit.blade.php (form to edit student)”
> “That’s why, in our controller, we call:
> return view('students.index');
> This tells Laravel: go inside the students folder and open the index.blade.php file.


a) index.blade.php (List students)  

  ```html
  <h1>Students List</h1> 
  <a href="/students/create">Add Student</a>   
  <table border="1">  
    <tr>  
        <th>ID</th><th>Name</th><th>Email</th><th>Class</th><th>Phone</th><th>Actions</th>  
    </tr>  
    @foreach($students as $student)  
    <tr>  
        <td>{{ $student->id }}</td>  
        <td>{{ $student->name }}</td>  
        <td>{{ $student->email }}</td>
        <td>{{ $student->class }}</td>
        <td>{{ $student->phone }}</td>
        <td>  
            <a href="/students/{{ $student->id }}/edit">Edit</a>  
            <a href="/students/{{ $student->id }}/delete">Delete</a>  
        </td>  
    </tr>  
    @endforeach  
</table>
```
>Explanation:  

>Displays all students in a table.
>@foreach loops through each student.
>Action links: Edit & Delete


b) create.blade.php (Add new student)  

```<h1>Add Student</h1>  
<form action="/students" method="POST">  
    @csrf  
    Name: <input type="text" name="name"><br>  
    Email: <input type="email" name="email"><br>
    Class: <input type="text" name="class"><br>
    Phone No: <input type="text" name="phone"><br>   
    <button type="submit">Save</button>  
</form>
```

>Explanation:  
>Form for adding a new student. @csrf → security token (prevents CSRF attack). On submit → goes to store() method in controller.
>When we submit a form in Laravel (like adding a new student), we write: @csrf
>The @csrf generates a hidden token (a random unique key).This token is sent with the form request to the server.
>“Why do we need this? Imagine a hacker creates a fake form and tricks you into clicking it. Without CSRF protection, that fake form could also send data to our website.
>But because of this CSRF token, Laravel checks: If the request has a valid token → Accept it.
>If not → Reject it.
>So basically, @csrf is like a security guard that ensures only requests from our own forms are accepted.”
>“For now, you don’t have to worry much — just remember:👉 Every time you make a POST, PUT, or DELETE form, always add @csrf.Otherwise, Laravel will throw an error.”

c) edit.blade.php (Edit student)  

```<h1>Edit Student</h1>  
<form action="/students/{{ $student->id }}" method="POST">  
    @csrf  
    Name: <input type="text" name="name" value="{{ $student->name }}"><br>  
    Email: <input type="email" name="email" value="{{ $student->email }}"><br>
    Class: <input type="text" name="class" value="{{ $student->class }}"><br>
    Phone No: <input type="text" name="phone" value="{{ $student->phone }}"><br>  
    <button type="submit">Update</button>  
</form>  

```

>Explanation:  
>Form for editing an existing student. Fields are pre-filled with current values. On submit → goes to update() method in controller.

### Step 5: Test Everything

1. Run server:

php artisan serve  

2. Open browser → http://127.0.0.1:8000/students  

3. Test:

Add new student

Edit student

Delete student

See all students

### Overall Flow for Students

1. Route → decides which controller method to call.


2. Controller → performs logic (fetch, save, update, delete).


3. Model → connects to database table.


4. View → shows result in browser.

> “Whenever we type a URL, the request passes Route → Controller → Model → View → back to browser.
