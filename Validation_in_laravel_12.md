**Validation**.
In Previous Part, we learned about Blade Templates — how to make our project cleaner using layouts, includes, loops, and conditions.

Now in this Part, we’ll add a very important feature: Validation.

👉 Validation means checking if the data entered by the user is correct before saving it in the database.

Think of it like a teacher checking an exam paper:

If all answers are filled properly → ✅ accepted.

If answers are missing or wrong → ❌ rejected with remarks.

In our Student Management System, when someone adds or edits a student, we must check:

Name should not be empty.

Email should be valid (like abc@gmail.com).

Class should not be empty.

Phone should be numeric and exactly 10 digits.

Without validation, people could enter:

Blank names

Wrong emails like hello@

Phone numbers like 123

And all this wrong data would go into our database 😱

That’s why validation is a must in web development.

**1. Where Do We Write Validation in Laravel?**

In Laravel, validation is usually written inside the Controller method where we handle form submissions.

Example: In our project → StudentController@store (for Add Student) and StudentController@update (for Edit Student).

**2. Basic Validation Syntax**

In a controller method:

$request->validate([
   'field_name' => 'rules'
]);


👉 Explanation:

$request->validate() → tells Laravel to check the data before saving.

'field_name' → which input field to validate (like name, email, phone).

'rules' → what conditions the input must follow (like required, email, numeric).

**1. Validation Rules in Laravel (Simple English)**

Laravel gives us easy rules to validate form fields:

| Rule                    | Meaning                              | Example                           |
| ----------------------- | ------------------------------------ | --------------------------------- |
| `required`              | Field cannot be empty                | Name required                     |
| `string`                | Must be text                         | Class should be text, not numbers |
| `max:100`               | Max characters allowed               | Name max 100 chars                |
| `email`                 | Must be a valid email                | `abc@gmail.com` ✅                 |
| `unique:students,email` | No duplicate email in students table | Stops same email twice            |
| `numeric`               | Only numbers allowed                 | Phone                             |
| `digits:10`             | Must be exactly 10 digits            | Phone must be 10 digits           |


**1. Validation in Controller (Step by Step)**

Let’s start with the store() method, which saves a new student.

📂 app/Http/Controllers/StudentController.php


```
public function store(Request $request)
{
    // ✅ Step 1: Validate the input
    $request->validate([
        'name'  => 'required|string|max:100',
        'email' => 'required|email|unique:students,email',
        'class' => 'required|string|max:50',
        'phone' => 'required|digits:10|numeric',
    ]);

    // ✅ Step 2: Save the student manually
    $student = new Student();
    $student->name  = $request->name;
    $student->email = $request->email;
    $student->class = $request->class;
    $student->phone = $request->phone;
    $student->save();

    // ✅ Step 3: Redirect back with success message
    return redirect('/students');
}
```

🧠 Explanation (Line by Line)

public function store(Request $request)
👉 This function runs when Add Student form is submitted.
👉 $request contains all form data.

$request->validate([...])
👉 This checks the data before saving.
👉 If any rule fails → user is sent back with error messages.

'name' => 'required|string|max:100'
👉 Name cannot be empty, must be text, max 100 characters.

'email' => 'required|email|unique:students,email'
👉 Email cannot be empty, must be valid format, and must not exist already in students table.

'class' => 'required|string|max:50'
👉 Class cannot be empty, must be text, max 50 characters.

'phone' => 'required|digits:10|numeric'
👉 Phone must be exactly 10 digits, numbers only.

$student = new Student();
👉 Create a new Student object.

$student->name = $request->name;
👉 Take the name from form and put it in the model.
👉 Repeat for email, class, phone.

$student->save();
👉 Save the student into database.

return redirect('/students'); → simply goes back to the student list page. (We’ll add success messages in the next part.)

**📘 Update Method (Edit Student)**

```
public function update(Request $request, $id)
{
    // ✅ Step 1: Find the student
    $student = Student::find($id);

    // ✅ Step 2: Validate input
    $request->validate([
        'name'  => 'required|string|max:100',
        'email' => 'required|email|unique:students,email,' . $student->id,
        'class' => 'required|string|max:50',
        'phone' => 'required|digits:10|numeric',
    ]);

    // ✅ Step 3: Update fields manually
    $student->name  = $request->name;
    $student->email = $request->email;
    $student->class = $request->class;
    $student->phone = $request->phone;
    $student->save();

    // ✅ Step 4: Redirect back
    return redirect('/students');
}
```
👉 Difference in update:
We use:
```
'unique:students,email,' . $student->id
```

This means the email must be unique, except for the student we’re editing.

**2. Updating the Forms (Blade Files) Showing Validation Errors in Forms**

Now let’s fix our forms to:

Show validation errors.

Keep old values after errors.

📂 resources/views/students/create.blade.php
```
@extends('layouts.app')

@section('title', 'Add Student')

@section('content')
   <h2>Add Student</h2>

   {{-- ✅ Show validation errors --}}
   @if ($errors->any())
      <div style="color: red;">
         <ul>
            @foreach ($errors->all() as $error)
               <li>{{ $error }}</li>
            @endforeach
         </ul>
      </div>
   @endif

   <form action="{{ url('/students') }}" method="POST">
      @csrf

      <label>Name:</label>
      <input type="text" name="name" value="{{ old('name') }}"><br><br>

      <label>Email:</label>
      <input type="email" name="email" value="{{ old('email') }}"><br><br>

      <label>Class:</label>
      <input type="text" name="class" value="{{ old('class') }}"><br><br>

      <label>Phone:</label>
      <input type="text" name="phone" value="{{ old('phone') }}"><br><br>

      <button type="submit">Save</button>
   </form>
@endsection
```



📂 resources/views/students/edit.blade.php
```
@extends('layouts.app')

@section('title', 'Edit Student')

@section('content')
   <h2>Edit Student</h2>

   {{-- ✅ Show validation errors --}}
   @if ($errors->any())
      <div style="color: red;">
         <ul>
            @foreach ($errors->all() as $error)
               <li>{{ $error }}</li>
            @endforeach
         </ul>
      </div>
   @endif

   <form action="{{ url('/students/' . $student->id) }}" method="POST">
      @csrf
      @method('PUT')

      <label>Name:</label>
      <input type="text" name="name" value="{{ old('name', $student->name) }}"><br><br>

      <label>Email:</label>
      <input type="email" name="email" value="{{ old('email', $student->email) }}"><br><br>

      <label>Class:</label>
      <input type="text" name="class" value="{{ old('class', $student->class) }}"><br><br>

      <label>Phone:</label>
      <input type="text" name="phone" value="{{ old('phone', $student->phone) }}"><br><br>

      <button type="submit">Update</button>
   </form>
@endsection
```


3. Old Helper (old()) Explained

old('name') → keeps what the user typed if validation fails.

old('name', $student->name) →

For Edit form → shows old input if validation fails,

Otherwise → shows existing student data.

**🔎 1. Where does $errors come from in Blade?**

When you use validation in Laravel like this:
```
$request->validate([
    'name' => 'required',
    'email' => 'required|email',
]);
```

👉 What happens behind the scenes?

If validation fails:

Laravel automatically redirects back to the same form page.

It also sends the error messages along with the redirect.

These errors are stored in a special object called an ErrorBag.

In Blade views, Laravel gives us a free global variable: $errors.

So when we write in Blade:
```
@if ($errors->any())
   <ul>
      @foreach ($errors->all() as $error)
         <li>{{ $error }}</li>
      @endforeach
   </ul>
@endif
```

👉 $errors->any() → checks if there are any validation errors.
👉 $errors->all() → gets all error messages as an array.

Example:
If you tried to save a student without a name and with a wrong email, $errors->all() will look like:
```
[
   "The name field is required.",
   "The email must be a valid email address."
]
```

Then Blade loops through and shows them.

So, $errors is automatically filled by Laravel’s validation system — we don’t need to manually pass it from the controller.



**4. Example in Action**

Try to add a student with no name → ❌ error shown.

Try to add invalid email → ❌ error shown.

Try to type phone with 5 digits → ❌ error shown.

Fill correctly → ✅ saved successfully.


🎯 Outro

So in Part 8B we:

Learned what validation is and why it’s needed.

Added validation rules in store() and update() methods.

Showed error messages inside our forms.

Made sure users can’t enter wrong data into the database.

👉 You might notice we didn’t yet show success messages after saving.
That’s because those messages are stored in something called a session, and I don’t want to confuse you now.

💡 In the next part (8C), we’ll explore Flash Messages — how to show “Student added successfully ✅” in a nice way and also understand sessions.

