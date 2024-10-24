# Object-Oriented Programming Concepts

Object-Oriented Programming (OOP) is a programming paradigm centered around objects that can contain both data (attributes) and functions (methods). It allows us to model real-world entities using classes and objects, focusing on making code more modular, reusable, and easier to maintain.

Here's a brief overview of OOP concepts using JavaScript:

### 1. **Classes and Objects**

- **Class**: A blueprint for creating objects. It defines the structure and behavior an object will have.
- **Object**: An instance of a class. It represents a specific entity created using the class blueprint.

```javascript
class Car {
  constructor(brand, model) {
    this.brand = brand;
    this.model = model;
  }

  drive() {
    console.log(`${this.brand} ${this.model} is driving.`);
  }
}

// Create an object (instance)
const myCar = new Car('Toyota', 'Corolla');
myCar.drive(); // Output: Toyota Corolla is driving.
```

### 2. **Encapsulation**

- Encapsulation is the practice of bundling data (attributes) and methods that operate on the data within the same object. In JavaScript, we can use classes and private fields (using `#`) to restrict access to internal object data.

```javascript
class User {
  #password;  // Private field

  constructor(username, password) {
    this.username = username;
    this.#password = password;
  }

  // Method to change password
  setPassword(newPassword) {
    this.#password = newPassword;
  }
}

const user = new User('chan', '1234');
user.setPassword('newpassword');
```

### 3. **Inheritance**

- Inheritance allows a class (child) to inherit properties and methods from another class (parent). It promotes code reuse.

```javascript
class Animal {
  speak() {
    console.log('The animal speaks.');
  }
}

class Dog extends Animal {
  speak() {
    console.log('The dog barks.');
  }
}

const myDog = new Dog();
myDog.speak(); // Output: The dog barks.
```

### 4. **Polymorphism**

- Polymorphism means "many forms." In JavaScript, this typically refers to methods in a child class overriding methods in a parent class, allowing for different behavior based on the object type.

```javascript
class Shape {
  area() {
    console.log('Calculating area...');
  }
}

class Circle extends Shape {
  area() {
    console.log('Calculating area of a circle.');
  }
}

const shape = new Shape();
shape.area(); // Output: Calculating area...

const circle = new Circle();
circle.area(); // Output: Calculating area of a circle.
```

### 5. **Abstraction**

- Abstraction hides complex implementation details and exposes only the necessary parts of an object. In JavaScript, there is no direct abstraction support, but it can be implemented using abstract classes or interfaces (though JavaScript doesn’t support abstract classes directly).

In the context of MongoDB, understanding OOP principles helps in modeling data as objects, where MongoDB documents (JSON-like) can be thought of as objects, with fields representing object attributes. This mindset is useful when using Object Document Mappers (ODMs) like Mongoose in JavaScript.