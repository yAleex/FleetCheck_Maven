# FleetCheck – Build Systems Lab

This project is intentionally incomplete. Follow the worksheet in the order given.

Expected final application output:

FleetCheck 1.0
Vehicles loaded: 4
Vehicles requiring service: 2
Average mileage: 37000 km

Do not copy the solution POM. The objective is to observe how each build change alters the result.

---

## Evidence

### Evidence 1 – Missing dependency (Maven)

Command: `mvn clean package` (JDK 21, Maven 3.9.12)

[ERROR] /home/alex/IdeaProjects/FleetCheck_Starter/src/main/java/pt/upt/fleetcheck/App.java:[4,38] package com.fasterxml.jackson.databind does not exist

Cause: the import `com.fasterxml.jackson.databind.ObjectMapper` (line 4 of App.java) fails because pom.xml does not declare the jackson-databind dependency.

### Evidence 4 – What the Shade plugin changed

Before (default JAR): `java -jar target/fleetcheck-1.0.0.jar` fails with
"no main manifest attribute". The default JAR contains only the project's own
classes and a manifest without Main-Class; Jackson is not included.

After (Shade): `target/fleetcheck-1.0.0-all.jar` is a fat JAR. Shade writes
Main-Class: pt.upt.fleetcheck.App into the manifest and copies the classes of
jackson-databind, jackson-core and jackson-annotations into the JAR, so it
runs standalone with `java -jar`. The normal JAR is kept alongside it.