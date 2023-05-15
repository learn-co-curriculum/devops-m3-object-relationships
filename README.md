# Object Relationships

## Learning Goals

- Practice modeling one-to-one, one-to-many, and many-to-many relationships
  in Python.

## Introduction

Now that we know all the ways we can associate classes together in Python, let
us revisit the relationships we learned in SQL:

- one-to-one
- one-to-many
- many-to-many

In this lesson, we will learn how to model these relationships in Python. Let's
take a look!

## One-to-One Relationships

Consider the one-to-one relationship in the ER diagram below:

![student-idcard-one-to-one](https://curriculum-content.s3.amazonaws.com/pe-mod-3/object-relationships/student-idcard-er-diagram.png)

Each `Student` has one `IdCard` and each `IdCard` belongs to one `Student`.

We already know how to model this in a database, but how would we relate these
two in Python? Let's create two classes: a `Student` class and an `IdCard`
class!

```py
class Student:
    def __init__(self, name, birthday):
        self.name = name
        self.birthday = birthday
        
class IdCard:
    def __init__ (self, id, active):
        self.id = id
        self.active = active

```

Currently, these two classes are not associated with each other. In order to
create a relationship between them, we'll need to add an `IdCard` instance to
the `Student` class.

```py
class Student:
    def __init__(self, name, birthday, idCard):
        self.name = name
        self.birthday = birthday
        self.idCard = idCard
        
class IdCard:
    def __init__ (self, id, active):
        self.id = id
        self.active = active

```

We can create this one-to-one relationship by adding an `IdCard` instance to
the `__init__` method.

### Property Decorator

We'll also go ahead and use the `@property` decorator. The `@property` decorator
is a Python way to define a getter and setter method for an attribute. This will
make it easy to retrieve and set a certain attribute! Let's see this in use with
the `IdCard` instance we added to the `Student` class:

```py
class Student:
    def __init__(self, name, birthday, idCard):
        self.name = name
        self.birthday = birthday
        self.idCard = idCard

    @property
    def idCard(self):
        return self._idCard

    @idCard.setter
    def idCard(self, value):
        if isinstance(value, IdCard):
            self._idCard = value
        else:
            raise TypeError("IdCard must be an instance of IdCard")    

class IdCard:
    def __init__ (self, id, active):
        self.id = id
        self.active = active       
```

In the above code, we use the `@property` decorator to get the `idCard`
attribute, and we use the `@idCard.setter` decorator to set the `idCard`
attribute. This means, if we call `student.idCard`, it would actually call
the property method `idCard(self)`. And when instantiating the `Student`
object, when the code executes `self.idCard = idCard`, it will actually call the
property's setter method `idCard(self, value)`. To demonstrate, consider
stepping through the Python Tutor Visualizer embedded below:

<iframe width="800" height="500" frameborder="0" src="https://pythontutor.com/iframe-embed.html#code=class%20Student%3A%0A%20%20%20%20def%20__init__%28self,%20name,%20birthday,%20idCard%29%3A%0A%20%20%20%20%20%20%20%20self.name%20%3D%20name%0A%20%20%20%20%20%20%20%20self.birthday%20%3D%20birthday%0A%20%20%20%20%20%20%20%20self.idCard%20%3D%20idCard%0A%0A%20%20%20%20%40property%0A%20%20%20%20def%20idCard%28self%29%3A%0A%20%20%20%20%20%20%20%20return%20self._idCard%0A%0A%20%20%20%20%40idCard.setter%0A%20%20%20%20def%20idCard%28self,%20value%29%3A%0A%20%20%20%20%20%20%20%20if%20isinstance%28value,%20IdCard%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20self._idCard%20%3D%20value%0A%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%20TypeError%28%22IdCard%20must%20be%20an%20instance%20of%20IdCard%22%29%20%20%20%20%0A%0Aclass%20IdCard%3A%0A%20%20%20%20def%20__init__%20%28self,%20id,%20active%29%3A%0A%20%20%20%20%20%20%20%20self.id%20%3D%20id%0A%20%20%20%20%20%20%20%20self.active%20%3D%20active%0A%20%20%20%20%0Aid%20%20%3D%20IdCard%28123456789,%20True%29%0A%0Astudent%20%3D%20Student%28%22Dustin%20Henderson%22,%20%2205-29-1971%22,%20id%29%0A%0Aprint%28student.idCard%29&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=7&heapPrimitives=nevernest&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>

## One-to-Many Relationships

There are a couple of ways we could model a one-to-many relationship.

### One Way One-to-Many

Consider the one-to-many relationship in the ER diagram below:

![cook-recipe-one-to-many](https://curriculum-content.s3.amazonaws.com/pe-mod-3/object-relationships/cook-recipe-er-diagram.png)

Each `Cook` has many `Recipe`s and, for our example, each `Recipe` belongs to
only one `Cook`.

Let's create two classes: a `Cook` class and a `Recipe` class!

```py
class Cook:
    def __init__(self, name):
        self.name = name

class Recipe:
    def __init__ (self, name):
        self.name = name
```

Currently, these two classes are not associated with each other. To create a
relationship between them, the `Cook` class will need to have a list attribute
to store the related `Recipe` objects. This is a similar example to the
`Shopper` and `GroceryItem` example we looked at in the last lesson:

```py
class Cook:
    def __init__(self, name):
        self.name = name
        self.recipes = []

class Recipe:
    def __init__ (self, name):
        self.name = name
```

In this example, it does not make sense for the `Recipe` to know about the
`Cook`. We could refer to this as a one-way one-to-many relationship. We could
continue to add some code to actually use these two classes:

```py
# Create a cook and some recipes
cook = Cook("Julia Child")
recipe1 = Recipe("Crepes")
recipe2 = Recipe("Coq au Vin")

# Add the recipes to the cook's recipe list
cook.recipes.append(recipe1)
cook.recipes.append(recipe2)

# Print the cook's recipes
for recipe in cook.recipes:
    print(recipe.name)
```

The above would produce the following output:

```text
Crepes
Coq au Vin
```

### Two Way One-to-Many

The more common one-to-many relationship we may see is a two-way one-to-many
relationship. Consider the following ER diagram:

![cat-ER-diagram](https://curriculum-content.s3.amazonaws.com/spring-mod-2/testing/cat-rescue-er-diagram.png)

In this relationship, notice that a rescue can have many cats but a cat can
only belong to one rescue. Let's create two classes: a `Rescue` class and a
`Cat` class:

```py
class Rescue:
    def __init__(self, name, address, city, state, website_url, phone_number):
        self.name = name
        self.address = address
        self.city = city
        self.state = state
        self.website_url = website_url
        self.phone_number = phone_number

class Cat:
    def __init__ (self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age
```

Currently, these two classes are not related. To associate them, the `Rescue`
class will need to have a list attribute to store the related `Cat` objects. We
will also add a `Rescue` instance to the `Cat` class this time too. This is
because a `Cat` may need to know which `Rescue` it is associated with. A good
reason for this might be if a potential adopter sees a picture of a cat on a
website, like Petfinder. The potential adopter will need to know which `Rescue`
to go to in order to adopt the cat. Here is what these two classes may look
like after we form the association:

```py
class Rescue:
    def __init__(self, name, address, city, state, website_url, phone_number):
        self.name = name
        self.address = address
        self.city = city
        self.state = state
        self.website_url = website_url
        self.phone_number = phone_number
        self._cats = []

    @property
    def cats(self):
        return self._cats
    
    def add_cat(self, cat):
        if isinstance(cat, Cat):
            self._cats.append(cat)
            cat.rescue = self
        else:
            raise TypeError("Cat must be an instance of Cat")    

class Cat:
    def __init__ (self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age
        self._rescue = None

    @property
    def rescue(self):
        return self._rescue

    @rescue.setter
    def rescue(self, value):
        if isinstance(value, Rescue):
            self._rescue = value
        else:
            raise TypeError("Rescue must be an instance of Rescue")    
```

Here we can see that we have a protected attribute, `_cats` in the `Rescue`
class and a protected attribute `_rescue` in the `Cat` class. When we add a
`Cat` object to the `Rescue`, it will append the `Cat` to the list of cats and
set the `_rescue` attribute in the `Cat` class. To step through this code with
an example, consider the embedded visualizer below:

<iframe width="800" height="500" frameborder="0" src="https://pythontutor.com/iframe-embed.html#code=class%20Rescue%3A%0A%20%20%20%20def%20__init__%28self,%20name,%20address,%20city,%20state,%20website_url,%20phone_number%29%3A%0A%20%20%20%20%20%20%20%20self.name%20%3D%20name%0A%20%20%20%20%20%20%20%20self.address%20%3D%20address%0A%20%20%20%20%20%20%20%20self.city%20%3D%20city%0A%20%20%20%20%20%20%20%20self.state%20%3D%20state%0A%20%20%20%20%20%20%20%20self.website_url%20%3D%20website_url%0A%20%20%20%20%20%20%20%20self.phone_number%20%3D%20phone_number%0A%20%20%20%20%20%20%20%20self._cats%20%3D%20%5B%5D%0A%0A%20%20%20%20%40property%0A%20%20%20%20def%20cats%28self%29%3A%0A%20%20%20%20%20%20%20%20return%20self._cats%0A%20%20%20%20%0A%20%20%20%20def%20add_cat%28self,%20cat%29%3A%0A%20%20%20%20%20%20%20%20if%20isinstance%28cat,%20Cat%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20self._cats.append%28cat%29%0A%20%20%20%20%20%20%20%20%20%20%20%20cat.rescue%20%3D%20self%0A%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%20TypeError%28%22Cat%20must%20be%20an%20instance%20of%20Cat%22%29%20%20%20%20%0A%0Aclass%20Cat%3A%0A%20%20%20%20def%20__init__%20%28self,%20name,%20breed,%20age%29%3A%0A%20%20%20%20%20%20%20%20self.name%20%3D%20name%0A%20%20%20%20%20%20%20%20self.breed%20%3D%20breed%0A%20%20%20%20%20%20%20%20self.age%20%3D%20age%0A%20%20%20%20%20%20%20%20self._rescue%20%3D%20None%0A%0A%20%20%20%20%40property%0A%20%20%20%20def%20rescue%28self%29%3A%0A%20%20%20%20%20%20%20%20return%20self._rescue%0A%0A%20%20%20%20%40rescue.setter%0A%20%20%20%20def%20rescue%28self,%20value%29%3A%0A%20%20%20%20%20%20%20%20if%20isinstance%28value,%20Rescue%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20self._rescue%20%3D%20value%0A%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%20TypeError%28%22Rescue%20must%20be%20an%20instance%20of%20Rescue%22%29%20%20%20%20%0A%0A%23%20Create%20a%20rescue%20and%20some%20cats%0Arescue%20%3D%20Rescue%28%22Safe%20Place%20for%20Pets%22,%20%22100%20Main%20Street%22,%20%22Colorado%20Springs%22,%20%22Colorado%22,%20%22https%3A//safeplacepets.org%22,%20%22555-123-4567%22%29%0Acat1%20%3D%20Cat%28%22Leo%22,%20%22Domestic%20Long%20Hair%22,%204%29%0Acat2%20%3D%20Cat%28%22Owen%22,%20%22Domestic%20Short%20Hair%22,%2010%29%0A%0A%23%20Add%20the%20cats%20to%20the%20rescue's%20list%20of%20cats%0Arescue.add_cat%28cat1%29%0Arescue.add_cat%28cat2%29%0A%0A%23%20Print%20the%20list%20of%20available%20cats%20for%20adoption%20at%20the%20rescue%0Afor%20cat%20in%20rescue.cats%3A%0A%20%20%20%20print%28cat.name,%20cat.breed,%20cat.age%29&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=0&heapPrimitives=nevernest&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>

## Many-to-Many Relationships

### Many-to-Many Without Intermediary Class

Consider the many-to-many relationship in the ER diagram below:

![employee-shift-many-to-many](https://curriculum-content.s3.amazonaws.com/pe-mod-3/object-relationships/employee-shift-er-diagram.png)

Each `Employee` can work many `Shift`s and each `Shift` has many `Employee`s.

Let's create two classes: an `Employee` class and a `Shift` class!

```py
class Employee:
    def __init__(self, name):
        self.name = name


class Shift:
    def __init__(self, start_time, end_time):
        self.start_time = start_time
        self.end_time = end_time
```

In the code above, these two classes are not related. Now, if we remember in
SQL, when we implement a many-to-many relationship, we usually need to create a
joining table. In this example, we'll show how we can still model a many-to-many
relationship without needing to create an intermediary class that stores
references to both objects:

```py
class Employee:
    def __init__(self, name):
        self.name = name

    def shifts(self):
        return [shift for shift in Shift.all if self in shift.employees]

    def add_shift(self, shift):
        if isinstance(shift, Shift):
            shift.add_employee(self)
        else:
            raise TypeError("Shift must be an instance of Shift")        


class Shift:
   
    all = []

    def __init__(self, start_time, end_time, employees=None):
        self.start_time = start_time
        self.end_time = end_time
        self._employees = []
        if employees:
            for employee in employees:
                self.add_employee(employee)
        Shift.all.append(self)        

    @property
    def employees(self):
        return self._employees

    def add_employee(self, employee):
        if isinstance(employee, Employee):
            self._employees.append(employee)
        else:
            raise TypeError("Employee must be an instance of Employee")  
```

Note that while the two classes are now related to each other in the same way
(an employee can work for many shifts and a shift can have many employees), we
took different approaches to accessing the related objects in each class. Lets
breakdown a little further what we did here:

- We choose one class to be the "owner" of the relationship. In this example, we
  chose the `Shift` class to store the relationship.
  - We could easily have chosen the `Employee` class to store the relationship
    too if we wanted to.
- In the `Shift` class, we add an `employees` list attribute and initialize it
  to an empty list in the `__init__` method.
  - If a list of `Employee` objects is passed in, we'll add each `Employee` to
    the list attribute.
- We'll add a getter method in the `Shift` class using the `@property`
  decorator.
- We also created a method `add_employee(self, employee)` to add an employee to
  the `employees` list attribute.
- In the `Employee` class, we'll also add a method `add_shift(self, shift)` that
  will call the `add_employee(self, employee)` method.
- We'll also add a getter in the `Employee` class to get the list of shifts an
  employee works. Notice the logic in the `shifts(self)` method since the
  `Employee` class does not have a list attribute of the `shifts`.


To use these two classes, we could use the code here:

```py
# Create some employees
employee1 = Employee("Marshall")
employee2 = Employee("Lily")

# Create some shifts
import datetime
shift1 = Shift(datetime.datetime(2023, 5, 12, 7, 0, 0), datetime.datetime(2023, 5, 12, 3, 0, 0))
shift2 = Shift(datetime.datetime(2023, 5, 13, 9, 0, 0), datetime.datetime(2023, 5, 13, 5, 0, 0))

# Add the employees to the shifts
employee1.add_shift(shift1)
employee1.add_shift(shift2)
employee2.add_shift(shift2)

# Print the employees in the shifts
print("In shift 1, the employees are:")
for employee in shift1.employees:
    print(employee.name)      

print("In shift 2, the employees are:")
for employee in shift2.employees:
    print(employee.name)

print()

# Print the shifts each employee works
print("Employee 1 works the following shifts that start at: ")
for shift in employee1.shifts():
    print(shift.start_time)

print("Employee 2 works the following shifts that start at: ")
for shift in employee2.shifts():
    print(shift.start_time)   
```

Feel free to step through the code
[here in the Python Tutor Visualizer](https://pythontutor.com/visualize.html#code=class%20Employee%3A%0A%20%20%20%20def%20__init__%28self,%20name%29%3A%0A%20%20%20%20%20%20%20%20self.name%20%3D%20name%0A%0A%20%20%20%20def%20shifts%28self%29%3A%0A%20%20%20%20%20%20%20%20return%20%5Bshift%20for%20shift%20in%20Shift.all%20if%20self%20in%20shift.employees%5D%0A%0A%20%20%20%20def%20add_shift%28self,%20shift%29%3A%0A%20%20%20%20%20%20%20%20if%20isinstance%28shift,%20Shift%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20shift.add_employee%28self%29%0A%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%20TypeError%28%22Shift%20must%20be%20an%20instance%20of%20Shift%22%29%20%20%20%20%20%20%20%20%0A%0A%0Aclass%20Shift%3A%0A%20%20%20%0A%20%20%20%20all%20%3D%20%5B%5D%0A%0A%20%20%20%20def%20__init__%28self,%20start_time,%20end_time,%20employees%3DNone%29%3A%0A%20%20%20%20%20%20%20%20self.start_time%20%3D%20start_time%0A%20%20%20%20%20%20%20%20self.end_time%20%3D%20end_time%0A%20%20%20%20%20%20%20%20self._employees%20%3D%20%5B%5D%0A%20%20%20%20%20%20%20%20if%20employees%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20for%20employee%20in%20employees%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20self.add_employee%28employee%29%0A%20%20%20%20%20%20%20%20Shift.all.append%28self%29%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%40property%0A%20%20%20%20def%20employees%28self%29%3A%0A%20%20%20%20%20%20%20%20return%20self._employees%0A%0A%20%20%20%20def%20add_employee%28self,%20employee%29%3A%0A%20%20%20%20%20%20%20%20if%20isinstance%28employee,%20Employee%29%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20self._employees.append%28employee%29%0A%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%20TypeError%28%22Employee%20must%20be%20an%20instance%20of%20Employee%22%29%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%23%20Create%20some%20employees%0Aemployee1%20%3D%20Employee%28%22Marshall%22%29%0Aemployee2%20%3D%20Employee%28%22Lily%22%29%0A%0A%23%20Create%20some%20shifts%0Aimport%20datetime%0Ashift1%20%3D%20Shift%28datetime.datetime%282023,%205,%2012,%207,%200,%200%29,%20datetime.datetime%282023,%205,%2012,%203,%200,%200%29%29%0Ashift2%20%3D%20Shift%28datetime.datetime%282023,%205,%2013,%209,%200,%200%29,%20datetime.datetime%282023,%205,%2013,%205,%200,%200%29%29%0A%0A%23%20Add%20the%20employees%20to%20the%20shifts%0Aemployee1.add_shift%28shift1%29%0Aemployee1.add_shift%28shift2%29%0Aemployee2.add_shift%28shift2%29%0A%0A%23%20Print%20the%20employees%20in%20the%20shifts%0Aprint%28%22In%20shift%201,%20the%20employees%20are%3A%22%29%0Afor%20employee%20in%20shift1.employees%3A%0A%20%20%20%20print%28employee.name%29%20%20%20%20%20%20%0A%0Aprint%28%22In%20shift%202,%20the%20employees%20are%3A%22%29%0Afor%20employee%20in%20shift2.employees%3A%0A%20%20%20%20print%28employee.name%29%0A%0Aprint%28%29%0A%0A%23%20Print%20the%20shifts%20each%20employee%20works%0Aprint%28%22Employee%201%20works%20the%20following%20shifts%20that%20start%20at%3A%20%22%29%0Afor%20shift%20in%20employee1.shifts%28%29%3A%0A%20%20%20%20print%28shift.start_time%29%0A%0Aprint%28%22Employee%202%20works%20the%20following%20shifts%20that%20start%20at%3A%20%22%29%0Afor%20shift%20in%20employee2.shifts%28%29%3A%0A%20%20%20%20print%28shift.start_time%29&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false).

If we run the program, it would produce the following output:

```text
In shift 1, the employees are:
Marshall
In shift 2, the employees are:
Marshall
Lily

Employee 1 works the following shifts that start at:
2023-05-12 07:00:00
2023-05-13 09:00:00
Employee 2 works the following shifts that start at:
2023-05-13 09:00:00
```

### Many-to-Many With Intermediary Class

Let us look at another many-to-many example:

![camper-activity-many-to-many](https://curriculum-content.s3.amazonaws.com/pe-mod-3/object-relationships/camper-activity-many-to-many-er-diagram.png)

Campers can sign up for activities. A `Camper` can sign up for many activities
and an `Activity` can have many campers. Therefore, this is a many-to-many
relationship. This time, there is a `Signup` table that will join these two
entities together to form an intermediary class.

Let's create our classes:

```py
class Camper:
    def __init__(self, name, age ):
        self.name = name
        self.age = age

class Activity:
    def __init__(self, name, difficulty):
        self.name = name
        self.difficulty = difficulty

class Signup:
    def __init__(self, time):
        self.time = time
```

Currently, these classes are not related. Let's look at how we can associate
them:

```py
class Camper:
    def __init__(self, name, age ):
        self.name = name
        self.age = age
        self._activities = []
        self._signups = []

    def activities(self, new_activity=None):
        if new_activity and isinstance(new_activity, Activity):
            self._activities.append(new_activity)
        return self._activities

    def signups(self, new_signup=None):
        if new_signup and isinstance(new_signup, Signup):
            self._signups.append(new_signup)
        return self._signups         

class Activity:
    def __init__(self, name, difficulty):
        self.name = name
        self.difficulty = difficulty
        self._signups = []
        self._campers = []

    def signups(self, new_signup=None):
        if new_signup and isinstance(new_signup, Signup):
            self._signups.append(new_signup)
        return self._signups

    def campers(self, new_camper=None):
        if new_camper and isinstance(new_camper, Camper):
            self._campers.append(new_camper)
        return self._campers           

class Signup:
    def __init__(self, time, camper, activity):
        self.time = time
        self.camper = camper
        self.activity = activity

        camper.activities(activity)
        camper.signups(self)

        activity.campers(camper)
        activity.signups(self)

    @property
    def camper(self):
        return self._camper
    
    @camper.setter
    def camper(self, camper):
        if isinstance(camper, Camper):
            self._camper = camper
        else:
            raise TypeError("Camper must be an instance of Camper")    

    @property
    def activity(self):
        return self._activity

    @activity.setter
    def activity(self, activity):
        if isinstance(activity, Activity):
            self._activity = activity
        else:
            raise TypeError("Activity must be an instance of Activity")
```

Let us breakdown this code a little bit:

- The `Camper` class now has two list attributes: `_activities` and `_signups`
  to keep track of which activities a camper might sign up for.
- We have two methods that we added in the `Camper` class:
  - `activities(self, activity)`
    - The `activities()` method takes in an optional parameter. If there is no
      `Activity` passed in, then it acts as a get method to retrieve the list
      attribute.
    - If an `Activity` is passed in, it will validate that the object passed in is
      an instance of `Activity` before adding it to `_activities`. It will then
      return the list attribute.
  - `signups(self, signup)`
    - The `signups()` method also takes in an optional parameter. If there is no
      `Signup` passed in, then it acts as a get method to retrieve `_signups`.
    - If a `Signup` is passed in, it will validate that the object passed in is
      an instance of `Signup` before adding it to the list of signups. It will
      then return the list attribute.
- The `Activity` class also now has two list attributes: `_campers` and
  `_signups` to keep track of which campers have signed up for a certain
  activity.
- We have also added two methods to the `Activity` class:
  - `campers(self, camper)`
    - The `campers()` method takes in an optional parameter. If there is no
      `Camper` object passed in, then the method simply acts as a get method.
    - If a `Camper` object is passed in, it will validate that it is indeed a
      `Camper` instance before adding it to the list attribute before returning
      the list of `_campers`.
  - `signups(self, signup)`
    - The `signups()` method also takes in an optional parameter. If there is no
      `Signup` object passed in, then it acts as a get method.
    - If a `Signup` is passed in, it will validate that the object passed in is
      an instance of `Signup` before adding it to `_signups`. It will then return
      the list of signups.
- The `Signup` class takes in two additional parameters in the `__init__`
  method: a `Camper` instance and an `Activity` instance.
  - We'll set the attributes using the properties' setter methods.
  - We'll add the activity and signup to the `Camper`.
  - We'll add the camper and signup to the `Activity`.

We'll add some code now to show that the classes are now associated with each
other:

```py
camper1 = Camper("Dustin Henderson", 14)
camper2 = Camper("Suzie Bingham", 14)

activity1 = Activity("Arts and Crafts", 1)
activity2 = Activity("Puzzles", 4)

import datetime
signup1 = Signup(datetime.datetime(2023, 5, 15, 11, 0, 0), camper1, activity1)
signup2 = Signup(datetime.datetime(2023, 5, 15, 11, 0 , 0), camper2, activity1)
signup3 = Signup(datetime.datetime(2023, 5, 15, 5, 0, 0), camper2, activity2)


# Print the activities for Camper 2
for activity in camper2.activities():
    print(activity.name, activity.difficulty)

# Print the signups for Camper 1
for signup in camper2.signups():
    print(signup.time)

print()

# Print the campers for Activity 1
for camper in activity1.campers():
    print(camper.name, camper.age)

# Print the signups for Activity 1
for signup in activity1.signups():
    print(signup.time)

print()

# Print the Camper for Signup 3 and Activity for Signup 3
print(signup3.camper.name, signup3.camper.age)
print(signup3.activity.name, signup3.activity.difficulty)
```

The output of the code above would produce:

```text
Arts and Crafts 1
Puzzles 4
2023-05-15 11:00:00
2023-05-15 05:00:00

Dustin Henderson 14
Suzie Bingham 14
2023-05-15 11:00:00
2023-05-15 11:00:00

Suzie Bingham 14
Puzzles 4
```

## Conclusion

We have now seen how to model various real-world relationships using
object-oriented programming concepts. These relationships provide a way to
define how different objects interact and communicate with each other in our
code.

## Resources

- [Python @property decorator](https://www.programiz.com/python-programming/property)
- [Python's property(): Add Managed Attributes to Your Classes](https://realpython.com/python-property/)
- [StackOverflow: Infinite Recursion](https://stackoverflow.com/questions/29539475/infinite-recursion-in-python3-3-setter/29539756#29539756)
