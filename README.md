# Factory Design Pattern - UML Diagrams

## 1. Factory Pattern - Class Diagram

```mermaid
classDiagram
    class Shape {
        <<interface>>
        +draw()*
    }
    
    class Circle {
        +draw()
    }
    
    class Rectangle {
        +draw()
    }
    
    class Square {
        +draw()
    }
    
    class ShapeFactory {
        +getShape(shapeType: String): Shape
    }
    
    class FactoryDemo {
        +main(args: String[])$
    }
    
    Shape <|.. Circle : implements
    Shape <|.. Rectangle : implements
    Shape <|.. Square : implements
    ShapeFactory ..> Shape : creates
    ShapeFactory ..> Circle : creates
    ShapeFactory ..> Rectangle : creates
    ShapeFactory ..> Square : creates
    FactoryDemo ..> ShapeFactory : uses
    FactoryDemo ..> Shape : uses
```

---

## 2. Sequence Diagram - Creating Shapes

```mermaid
sequenceDiagram
    participant Client as FactoryDemo
    participant Factory as ShapeFactory
    participant Product as Shape (Circle/Rectangle/Square)
    
    Client->>Factory: new ShapeFactory()
    activate Factory
    Factory-->>Client: factory instance
    deactivate Factory
    
    Note over Client,Factory: Request Circle
    Client->>Factory: getShape("circle")
    activate Factory
    Factory->>Product: new Circle()
    activate Product
    Product-->>Factory: circle instance
    deactivate Product
    Factory-->>Client: Shape (circle)
    deactivate Factory
    
    Client->>Product: draw()
    activate Product
    Product-->>Client: "Drawing a Circle"
    deactivate Product
    
    Note over Client,Factory: Request Rectangle
    Client->>Factory: getShape("rectangle")
    activate Factory
    Factory->>Product: new Rectangle()
    activate Product
    Product-->>Factory: rectangle instance
    deactivate Product
    Factory-->>Client: Shape (rectangle)
    deactivate Factory
    
    Client->>Product: draw()
    activate Product
    Product-->>Client: "Drawing a Rectangle"
    deactivate Product
```

---

## 3. Factory Pattern Flow

```mermaid
graph TB
    subgraph "Factory Pattern Object Creation Flow"
        A[Client<br/>FactoryDemo] -->|1. Requests Shape| B[Factory<br/>ShapeFactory]
        B -->|2. Decides which class| C{Shape Type?}
        C -->|"circle"| D[Create Circle]
        C -->|"rectangle"| E[Create Rectangle]
        C -->|"square"| F[Create Square]
        C -->|null/unknown| G[Return null]
        D --> H[Return Shape]
        E --> H
        F --> H
        G --> H
        H -->|3. Returns to Client| A
    end
    
    style A fill:#e1f5ff
    style B fill:#fff4e1
    style C fill:#ffe1f5
    style H fill:#e8f5e9
```

---

## 4. Factory Decision Logic

```mermaid
flowchart TD
    Start([Client calls<br/>getShape]) --> Check{shapeType<br/>== null?}
    
    Check -->|Yes| ReturnNull[Return null]
    Check -->|No| Switch{switch<br/>shapeType}
    
    Switch -->|"circle"| CreateCircle[new Circle]
    Switch -->|"rectangle"| CreateRect[new Rectangle]
    Switch -->|"square"| CreateSquare[new Square]
    Switch -->|default| ReturnNull2[Return null]
    
    CreateCircle --> Return[Return Shape]
    CreateRect --> Return
    CreateSquare --> Return
    ReturnNull --> End([End])
    ReturnNull2 --> End
    Return --> End
    
    style CreateCircle fill:#4CAF50,color:#fff
    style CreateRect fill:#2196F3,color:#fff
    style CreateSquare fill:#FF9800,color:#fff
    style Return fill:#9C27B0,color:#fff
```

---

## 5. Object Creation Comparison

```mermaid
graph TB
    subgraph "Without Factory Pattern"
        W1[Client Code]
        W2["Shape s1 = new Circle()<br/>Shape s2 = new Rectangle()<br/>Shape s3 = new Square()"]
        W3[❌ Client knows all concrete classes<br/>❌ Tight coupling<br/>❌ Hard to maintain]
        W1 --> W2 --> W3
    end
    
    subgraph "With Factory Pattern"
        B1[Client Code]
        B2["Shape s1 = factory.getShape('circle')<br/>Shape s2 = factory.getShape('rectangle')<br/>Shape s3 = factory.getShape('square')"]
        B3[✅ Client only knows interface<br/>✅ Loose coupling<br/>✅ Easy to extend]
        B1 --> B2 --> B3
    end
    
    style W3 fill:#f44336,color:#fff
    style B3 fill:#4CAF50,color:#fff
```

---

## 6. Design Explanation

### What is the Factory Pattern?

**Factory Pattern** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. It encapsulates object creation logic.

### Key Components

1. **Product Interface (Shape)**:
   - Defines the interface for objects the factory creates
   - Common interface: `draw()` method
   - All concrete products implement this interface

2. **Concrete Products (Circle, Rectangle, Square)**:
   - Implement the Product interface
   - Provide specific implementations of `draw()`
   - Client doesn't need to know about these classes

3. **Factory (ShapeFactory)**:
   - Contains the creation logic
   - Method: `getShape(String shapeType)`
   - Decides which concrete class to instantiate
   - Returns Product interface type

4. **Client (FactoryDemo)**:
   - Uses factory to create objects
   - Works with Product interface only
   - Doesn't know about concrete implementations

---

## 7. How Your Code Works

### Step-by-Step Process

```mermaid
stateDiagram-v2
    [*] --> ClientStart: Client needs a shape
    ClientStart --> CreateFactory: new ShapeFactory()
    CreateFactory --> RequestShape: getShape("circle")
    RequestShape --> CheckType: Factory checks type
    CheckType --> InstantiateCircle: Type is "circle"
    InstantiateCircle --> ReturnShape: Return Circle as Shape
    ReturnShape --> ClientUse: Client calls draw()
    ClientUse --> Output: "Drawing a Circle"
    Output --> [*]
    
    note right of CheckType
        Switch statement
        matches shapeType
    end note
    
    note right of ReturnShape
        Returns Shape interface
        Not Circle class
    end note
```

---

## 8. Factory Pattern Structure

```mermaid
graph LR
    subgraph "Factory Pattern Components"
        Client[Client<br/>FactoryDemo]
        Factory[Factory<br/>ShapeFactory]
        Interface[Product Interface<br/>Shape]
        P1[Concrete Product<br/>Circle]
        P2[Concrete Product<br/>Rectangle]
        P3[Concrete Product<br/>Square]
    end
    
    Client -->|uses| Factory
    Client -->|knows only| Interface
    Factory -->|creates| P1
    Factory -->|creates| P2
    Factory -->|creates| P3
    P1 -.->|implements| Interface
    P2 -.->|implements| Interface
    P3 -.->|implements| Interface
    
    style Client fill:#e1f5ff
    style Factory fill:#fff4e1
    style Interface fill:#ffe1f5
    style P1 fill:#e8f5e9
    style P2 fill:#e8f5e9
    style P3 fill:#e8f5e9
```

---

## 9. Advantages & Disadvantages

### ✅ Advantages

```mermaid
mindmap
    root((Factory Pattern<br/>Advantages))
        Loose Coupling
            Client independent of concrete classes
            Only depends on interface
            Easy to test
        Single Responsibility
            Creation logic in one place
            Easy to maintain
            Clear separation of concerns
        Open/Closed Principle
            Open for extension
            Closed for modification
            Add new products easily
        Code Reusability
            Centralized creation logic
            No duplicate code
            Consistent object creation
```

### ❌ Disadvantages

| Disadvantage | Description |
|-------------|-------------|
| **Complexity** | Adds extra layer of abstraction |
| **More Classes** | Need to create factory class |
| **String-based** | Type safety issues with string parameters |
| **Null Returns** | Must handle null for unknown types |

---

## 10. When to Use Factory Pattern

```mermaid
flowchart TD
    Start{Need to create<br/>objects?}
    
    Start --> Q1{Type determined<br/>at runtime?}
    Q1 -->|No<br/>Known at compile time| Direct[Use Direct<br/>Instantiation]
    Q1 -->|Yes| Q2{Many similar<br/>classes?}
    
    Q2 -->|No<br/>Only 1-2 classes| Direct2[Use Direct<br/>Instantiation]
    Q2 -->|Yes| Q3{Want to hide<br/>creation logic?}
    
    Q3 -->|No| Direct3[Use Direct<br/>Instantiation]
    Q3 -->|Yes| Factory[✅ Use Factory<br/>Pattern]
    
    Factory --> Benefits[✅ Loose coupling<br/>✅ Easy to extend<br/>✅ Centralized logic]
    
    style Factory fill:#4CAF50,color:#fff,stroke:#2E7D32,stroke-width:3px
    style Benefits fill:#81C784,color:#fff
    style Start fill:#FF9800,color:#fff
```

---

## 11. Real-World Examples

### Common Use Cases

```mermaid
mindmap
    root((Factory Pattern<br/>Use Cases))
        Java APIs
            Calendar.getInstance
            NumberFormat.getInstance
            Cipher.getInstance
            DocumentBuilderFactory
        Frameworks
            Spring Bean Factory
            JDBC DriverManager
            Log4j LoggerFactory
        UI Libraries
            Button Factory
            Widget Factory
            Theme Factory
        Database
            Connection Factory
            DAO Factory
            Entity Factory
        Testing
            Mock Object Factory
            Test Data Factory
```

---

## 12. Extension Example - Adding New Shape

### Adding Triangle to Your Factory

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Triangle as Triangle Class
    participant Factory as ShapeFactory
    participant Client as FactoryDemo
    
    Note over Dev,Triangle: Step 1: Create new product
    Dev->>Triangle: Create Triangle class
    Triangle-->>Dev: implements Shape
    
    Note over Dev,Factory: Step 2: Update factory
    Dev->>Factory: Add "triangle" case
    Factory-->>Dev: Updated switch statement
    
    Note over Dev,Client: Step 3: No client changes needed!
    Dev->>Client: Client code unchanged
    Client->>Factory: getShape("triangle")
    Factory->>Triangle: new Triangle()
    Triangle-->>Client: triangle instance
    
    Note over Client: Client uses same interface
    Client->>Triangle: draw()
    Triangle-->>Client: "Drawing a Triangle"
```

### Code to Add Triangle

```java
// 1. Create Triangle class
public class Triangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a Triangle");
    }
}

// 2. Update ShapeFactory - Add one case
public class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType == null) return null;
        
        switch (shapeType.toLowerCase()) {
            case "circle":
                return new Circle();
            case "rectangle":
                return new Rectangle();
            case "square":
                return new Square();
            case "triangle":  // NEW CASE
                return new Triangle();
            default:
                return null;
        }
    }
}

// 3. Client code - NO CHANGES NEEDED!
// Just use the new shape name
Shape shape4 = shapeFactory.getShape("triangle");
shape4.draw(); //
