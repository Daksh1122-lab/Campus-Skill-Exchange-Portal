# Campus Skill Exchange Portal

A core-Java (no external libraries) console application where students can **share technical skills, mentor peers, and collaborate on learning activities**.

## Features

| Module | What students can do |
|---|---|
| Accounts | Register / log in (salted SHA-256 password hashes), profile with department and year |
| Skill sharing | List skills they can teach (with level) and skills they want to learn |
| Mentor discovery | Search mentors by skill (case-insensitive, partial match), get suggestions based on skills they want, best-rated first |
| Mentorship | Send a request -> mentor accepts/declines -> mentee marks session complete and rates 1-5 -> mentor's average rating updates |
| Study groups | Create / browse / join / leave groups with a size limit, a discussion board (messages and resource links), and a shared checklist of learning activities |
| Persistence | All data is saved automatically to `data/portal.dat` (Java serialization) |

## Project structure

```
src/com/campusskillexchange/
  Main.java, DemoData.java      entry point and optional sample data
  model/                        Student, MentorshipRequest, StudyGroup (entities)
  storage/DataStore.java        in-memory collections + save/load to file
  service/                      StudentService, MentorshipService, StudyGroupService,
                                PortalException (business rules and validation)
  ui/ConsoleUI.java             menu-driven console interface
test/.../PortalSmokeTest.java   dependency-free tests (15 checks)
```

Layers: **UI -> Service -> Storage/Model**. The UI never touches data directly; every rule (duplicate email, full group, only the mentor can accept, etc.) lives in the service layer and is reported with `PortalException`.

## How to run (JDK 11 or newer)

Linux / macOS:
```bash
mkdir -p out
javac -d out $(find src -name "*.java")
java -cp out com.campusskillexchange.Main --demo
```

Windows (Command Prompt):
```bat
dir /s /b src\*.java > sources.txt
mkdir out
javac -d out @sources.txt
java -cp out com.campusskillexchange.Main --demo
```

`--demo` is only needed the first time: it creates three sample students, a pending mentorship request and a study group.
Demo logins: `aarav@campus.edu`, `meera@campus.edu`, `rohan@campus.edu` (password `pass1234`).

Run the tests:
```bash
javac -d out $(find src test -name "*.java")
java -cp out com.campusskillexchange.PortalSmokeTest
```

## Java concepts demonstrated

Encapsulation and immutable fields, enums, nested classes, collections (`Map`, `Set`, `List`, `TreeMap` with a case-insensitive comparator), streams and lambdas, custom exceptions, object serialization, `MessageDigest` hashing with `SecureRandom` salt, layered architecture.

## Ideas to extend it

- Swing/JavaFX GUI, or a Spring Boot REST API with a web front end
- Replace file serialization with MySQL/PostgreSQL through JDBC (the service layer would stay the same)
- Stronger password hashing (BCrypt/PBKDF2), hidden password input
- Skill endorsements, session scheduling, notifications, admin moderation
