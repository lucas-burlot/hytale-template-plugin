# Hytale Plugin Template

A minimal, ready-to-use template for creating Hytale plugins with modern build tools and automated testing.

> **✨ Builds immediately without any changes!** Clone and run `./gradlew build` to get a working plugin JAR.

## Quick Start

### Prerequisites

- **Java 25 JDK** - [Download here](https://www.oracle.com/java/technologies/downloads/)
- **IDE** - IntelliJ IDEA, VSCode/Cursor, or other Java IDE
- **Git** - [Download here](https://git-scm.com/)

### IDE Setup

To get autocompletion and code navigation for Hytale code, you need to configure your IDE to reference `HytaleServer.jar`.

#### IntelliJ IDEA

1. **File → Project Structure** (Ctrl + Alt + Shift + S)
2. **Modules**
3. **Dependencies** tab
4. Click **+**
5. **JARs or directories**
6. Select: `libs/HytaleServer.jar` (or the path to your HytaleServer.jar)
7. **Scope → Compile**
8. **OK / Apply**

#### VSCode / Cursor (or other IDEs)

**Tip:** To get all decompiled classes like in IntelliJ:

1. Place `HytaleServer.jar` and `cfr.jar` in the `libs/` folder of your project
2. Decompile the JAR with the following command:

```bash
java -jar libs/cfr.jar libs/HytaleServer.jar --outputdir libs/out
```

3. The `.classpath` and `.vscode/settings.json` files are already configured to reference `libs/HytaleServer.jar`

**Configuration files:**
- `.classpath` - Eclipse/VSCode classpath configuration
- `.vscode/settings.json` - VSCode configuration to reference libraries

After decompilation, you'll have access to all Hytale classes in `libs/out/` with full autocompletion.


### 1. Build Immediately (No Changes Needed!)

The template works out-of-the-box:

```bash
# Windows
gradlew.bat build

# Linux/Mac
./gradlew build
```

Your plugin JAR will be in: `build/libs/TemplatePlugin-1.0.0.jar`

**Automatic deployment:** If you've configured `serverDir` in `gradle.properties`, the plugin JAR will be automatically copied to your server's `plugins/` folder after the build.

### 2. Customize Your Plugin (Optional)

When ready to customize, edit these files:

**`settings.gradle.kts`:**
```kotlin
rootProject.name = "your-plugin-name"
```

**`gradle.properties`:**
```properties
# Project Information
pluginGroup=com.yourname
pluginVersion=1.0.0
pluginDescription=Your plugin description

# Gradle Configuration
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.daemon=true
org.gradle.jvmargs=-Xmx2048m -XX:MaxMetaspaceSize=512m

# Server path to copy mod in server
serverDir=C:/Path/To/Your/Server  # Path to your Hytale server (optional)
```

**Note:** The `serverDir` property allows automatically copying the plugin JAR to the server's `plugins/` folder whenever it's built. If this property is set, the plugin will be automatically copied after each build.

**`src/main/resources/manifest.json`:**
```json
{
  "Group": "YourName",
  "Name": "YourPluginName",
  "Main": "com.yourname.yourplugin.YourPlugin"
}
```

**Rename the main plugin class:**
- Rename `src/main/java/com/example/templateplugin/TemplatePlugin.java`
- Update package name to match your `pluginGroup`

### 3. Build Your Plugin

```bash
# Windows
gradlew.bat build

# Linux/Mac
./gradlew build
```

Your plugin JAR will be in: `build/libs/YourPluginName-1.0.0.jar`

**Automatic deployment:** If you've configured `serverDir` in `gradle.properties`, the plugin JAR will be automatically copied to your server's `plugins/` folder after the build.

### 4. Test Your Plugin (Automated!)

```bash
# Windows
gradlew.bat runServer

# Linux/Mac
./gradlew runServer
```

## Development Workflow

### Building

```bash
# Compile only
./gradlew compileJava

# Build plugin JAR (and automatically deploy if serverDir is configured)
./gradlew build

# Clean and rebuild
./gradlew clean build
```

**Automatic deployment:** If you've configured `serverDir` in `gradle.properties`, the plugin JAR will be automatically copied to your server's `plugins/` folder after each build.

### Testing

```bash
# Run server with your plugin
./gradlew runServer

# Run unit tests
./gradlew test

# Clean test server
rm -rf run/
```

### Debugging

```bash
# Run server in debug mode
./gradlew runServer -Pdebug

# Then connect your IDE debugger to localhost:5005
```

---

## Customization

### Adding Dependencies

Edit `build.gradle.kts`:

```kotlin
dependencies {
    // Hytale API (provided by server)
    compileOnly(files("libs/hytale-server.jar"))
    
    // Your dependencies (will be bundled)
    implementation("com.google.code.gson:gson:2.10.1")
    
    // Test dependencies
    testImplementation("org.junit.jupiter:junit-jupiter:5.10.0")
}
```

### Configuring Server Testing

**Run Hytale Server** - A Gradle plugin to download and run a Hytale server for development and testing purposes. The server files will be located in the `run/` directory of the project. Before starting the server it will compile (build task) and copy the plugin jar to the server's `plugins/` folder.

**Usage:**

Edit `build.gradle.kts`:

```kotlin
runHytale {
    jarUrl = "url to hytale server jar"
}
```

Run the server with:

```bash
# Windows
gradlew.bat runServer

# Linux/Mac
./gradlew runServer
```