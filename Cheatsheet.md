# Python classes: cheat sheet

## A simple class

Save this in a file called `user.py` (or open the ready-made copy: [`examples/user.py`](examples/user.py)):

```python
class User:
    """A user of a system."""

    def __init__(self, uid, name, role="student"):
        self.uid = uid          # data attributes
        self.name = name
        self.role = role
        self.logins = []        # start with an empty list

    def email(self):
        # a method that builds a value from the attributes and returns it
        return f"{self.uid}@example.ac.uk"

    def __str__(self):
        info_str = f"User {self.uid}: {self.name}\n"
        info_str += f"role: {self.role}\n"
        info_str += f"email: {self.email()}"
        return info_str

    def record_login(self, when):
        self.logins.append(when)
        print(f"{self.name} logged in at {when}")
```

## Using it

In another file in the same folder (e.g. `main.py`, also available as [`examples/main.py`](examples/main.py)):

```python
from user import User

u = User("ab123", "Ada")                # create an object (calls __init__)
v = User("cd456", "Grace", "staff")     # role given instead of the default

print(u.name)                           # read an attribute
u.name = "Ada Lovelace"                 # change an attribute
print(u.email())                        # call a method that returns a value
print(u)                                # calls __str__
u.record_login("09:30")                 # call a method
print(u.logins)
```

Output:

```text
Ada
ab123@example.ac.uk
User ab123: Ada Lovelace
role: student
email: ab123@example.ac.uk
Ada Lovelace logged in at 09:30
['09:30']
```

## The parts

| part | what it does |
|------|--------------|
| `class User:` | defines a new type of object. Class names start with a capital letter |
| `def __init__(self, ...)` | the **constructor**. Runs automatically when you write `User(...)`. Sets up the starting attributes |
| `self` | the object itself. It is always the first parameter of a method, but you never pass it when calling: `u.record_login("09:30")` |
| `self.name = name` | creates a **data attribute** stored inside the object. Without `self.` it is just a local variable that disappears |
| `role="student"` | a **default value**, used if the argument is left out |
| `def email(self)` | a method that **returns** a value worked out from the attributes (here the `uid`). Nothing extra is stored: the email is built each time you ask. Call it with brackets: `u.email()` |
| `def __str__(self)` | returns the text used by `print(obj)`. It must `return` a string, not print it. Inside the class, call other methods with `self.`, e.g. `self.email()` |
| `def record_login(self, when)` | an ordinary **method**: a function belonging to the class. It can read and change the object's attributes through `self` |
| `u = User(...)` | creates an **object** (an *instance* of the class). Each object has its own copy of the attributes |

## Common mistakes

- Forgetting `self` as the first parameter: `def record_login(when):` gives `TypeError: ... takes 1 positional argument but 2 were given`.
- Forgetting `self.` when storing or reading an attribute: writing `name = name` in `__init__` stores nothing.
- Forgetting the brackets when calling a method: `u.email` gives something like `<bound method User.email ...>` instead of the address.
- Forgetting `self.` when calling a method from inside the class: `email()` fails, it needs to be `self.email()`.
- Printing inside `__str__` instead of returning a string: `print(u)` then shows `None` or raises `TypeError: __str__ returned non-string`.
- Writing `__init__` or `__str__` with the wrong number of underscores. They are **two** before and **two** after.
- Using `User` without importing it. If the class is in `user.py` you need `from user import User`.
- Changing the class file and not saving it, or not re-running your script.
