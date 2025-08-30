# 🏭 Factory Design Pattern (Java)

## 📌 What is Factory Pattern?
The **Factory Pattern** is a **Creational Design Pattern** that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.  

Instead of calling a constructor directly, you ask a **Factory** to give you the object.  
The factory decides which exact implementation to return.

---

## 📖 Key Concepts
1. **Interface / Abstract Class (Product)**  
   - Base element for which the objects are created.  
   - Example: `OperatingSystem` interface (implemented by `WindowsOperatingSystem`, `LinuxOperatingSystem`).

2. **Factory Class (Creator)**  
   - Responsible for creating objects.  
   - Example: `OperatingSystemFactory`.  
   - Hides the instantiation logic from the client.  
   - Makes it easier to extend — just add a new type (e.g., `MacOperatingSystem`).

---

## 💡 Advantages
- Encapsulation of object creation → Client doesn’t worry about `new`.  
- Extensibility → Easily add new object types without changing client code.  
- Centralized control over object creation.  
- Reduced coupling → Client depends only on interface/abstract class, not concrete implementations.  

---

## 📌 Real-world Java Examples
- `Calendar.getInstance()` → Returns different calendar implementations depending on locale/timezone.  
- `Class.forName()` → Reflection loads class dynamically at runtime.  
- **JDBC Drivers** → `DriverManager.getConnection()` gives you DB-specific connection.  

---

## 📝 Example Usage in this Project
```java
OperatingSystem windows = OperatingSystemFactory.getInstance("WINDOWS", "WIN10", "x64");
OperatingSystem linux   = OperatingSystemFactory.getInstance("LINUX", "UBUNTU", "x64");
➡️ Factory hides the new keyword.
➡️ If tomorrow we add MacOperatingSystem, the client code does not change.

🏗 UML Diagram (Conceptual)
graphql
Copy code
                OperatingSystem (Interface)
                       ▲
         ┌─────────────┴───────────────┐
         │                             │
 WindowsOperatingSystem       LinuxOperatingSystem

                OperatingSystemFactory
                        |
            getInstance(String type, ...)
🚀 How to Run
bash
Copy code
# Build the project
mvn clean install

# Run the application
java -cp target/Factory-Design-Pattern-1.0-SNAPSHOT.jar Main
🔎 Difference: Factory vs Abstract Factory vs Builder
Factory Method → Creates objects without exposing instantiation logic.

Abstract Factory → Factory of factories; creates families of related objects.

Builder → Step-by-step construction of a complex object.
📚 Reference
Check this great resource on [GoF Design Patterns in Java Core Libraries](https://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns-in-javas-core-libraries).
