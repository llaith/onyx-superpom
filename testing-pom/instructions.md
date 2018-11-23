# Instructions

The logging base should be applied via the following approach:

1. Create a property for the import
2. Add the deps (for the exclusions) 
3. Import the BOM (for the dep management) 
4. Scan the mixin (for the properties)
5. Add the SLF4J API
6. (Optional) Add a top-level output plugin


### Step 1: Create a Property

Create a property to keep things in sync

```xml
<properties>
    ...
    <version.onyx-testing-pom>1.0</version.onyx-testing-pom>
    ...
</dependencies>
```

### Step 2: Logging Deps

Add the deps to the dependencies section for resolution via the normal transitive dependencies system

```xml
<dependencies>
    ...
    <dependency>
        <groupId>org.llaith.onyx</groupId>
        <artifactId>onyx-testing-pom</artifactId>
        <version>${version.onyx-testing-pom}</version>
    </dependency>
    ...
</dependencies>
```

### Step 3: Dependency Management Import

Import the dependency-management section via a BOM import

```xml
<dependencyManagement>
    <dependencies>
        ...
        <dependency>
            <groupId>org.llaith.onyx</groupId>
            <artifactId>onyx-testing-pom</artifactId>
            <version>${version.onyx-testing-pom}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    ...
    </dependencies>    
</dependencyManagement>
```

### Step 4: Import the properties via a mixin

Import the properties via the mixin strategy

```xml
<build>
    <plugins>
        ...
        <plugin>
            <groupId>com.github.odavid.maven.plugins</groupId>
            <artifactId>mixin-maven-plugin</artifactId>
            <version>0.1-alpha-23</version>
            <extensions>true</extensions>
            <configuration>
                <mixins>
                    <mixin>
                        <groupId>org.llaith.onyx</groupId>
                        <artifactId>onyx-testing-pom</artifactId>
                        <version>${version.onyx-testing-pom}</version>
                    </mixin>
                </mixins>
            </configuration>
        </plugin>
        ...
    </plugins>
</build>
```

### Step 5: Adding the slf4j api dependency

Add the slf4j api dependency as normal.

```xml
<dependencies>
    ...
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
    </dependency>
    ...
</dependencies>
```        

### Step 6: (Optional) Adding a 'top-level' ouput plugin

If the project in question is a 'top-level' project, add the actual slf4j logger in the dependencies section.

```xml
<dependencies>
    ...
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-jdk14</artifactId>
    </dependency>
    ...
</dependencies>
```

Then to see the output in the IntelliJ console nicely, add the following property to the JVM Settings:

```
-Djava.util.logging.SimpleFormatter.format="%1$tY-%1$tm-%1$td %1$tH:%1$tM:%1$tS %4$s %2$s %5$s%6$s%n"
```

### Tweaks: Change versions

Note, to change the versions of the included libraries, override the properties that have been imported.

In this case, that is the following:

```xml
<properties>
    ...
    ...
</properties>
```  

Done.

 