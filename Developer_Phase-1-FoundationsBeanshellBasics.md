# 🖋️ SailPoint Beanshell Scripting Basics 🚀

This guide covers **variables, loops, functions, conditionals, and working with IdentityIQ objects** in Beanshell scripting for SailPoint IIQ.  

---

## 1️⃣ Variables

```java
// String variable
String firstName = "Alice";

// Integer variable
int employeeID = 1001;

// Boolean variable
boolean isActive = true;

// List variable
List<String> roles = new ArrayList<>();
roles.add("HR_Manager");
roles.add("Payroll");

// Logging
log.info("User " + firstName + " has ID: " + employeeID);
```

##2️⃣ Loops

```java
List<String> users = new ArrayList<>();
users.add("Alice");
users.add("Bob");
users.add("Charlie");

for(int i = 0; i < users.size(); i++){
    log.info("User: " + users.get(i));
}


```

##Enhanced for loop:

```java
for(String user : users){
    log.info("User: " + user);
}

```

## While loop:

```java

int count = 0;
while(count < 3){
    log.info("Count: " + count);
    count++;
}
```
``` java
int count = 0;
while(count < 3){
    log.info("Count: " + count);
    count++;
}

```

## 3️⃣ Conditional Statements

```java
int employeeID = 1001;

if(employeeID == 1001){
    log.info("Employee is Alice");
} else if(employeeID == 1002){
    log.info("Employee is Bob");
} else {
    log.info("Unknown employee");
}
```

## 4️⃣ Functions / Methods

```java

// Function to greet a user
String greetUser(String name){
    return "Hello, " + name + "!";
}

// Call the function
String message = greetUser("Alice");
log.info(message);

```

##5️⃣ Working with Identity Objects

```java
// Suppose 'identity' is passed to the rule
String first = identity.getAttribute("firstName");
String last = identity.getAttribute("lastName");

// Generate username
String username = first.toLowerCase() + "." + last.toLowerCase();
identity.setName(username);

log.info("Generated username: " + username);
```

##6️⃣ Working with Lists & Loops
```java
List<Role> roles = identity.getRoles();
for(Role role : roles){
    log.info("Assigned role: " + role.getName());
}

```

##7️⃣ Logging and Debugging
```java
log.info("Debugging info: " + variableName);
log.warn("Warning example");
log.error("Error example");

```
>💡 Tip: Always use log.info() to see output when testing scripts in IIQ.






