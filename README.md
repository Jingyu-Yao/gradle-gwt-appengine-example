# Example Gradle + GWT + Google App Engine project

## Dependencies

- JDK 8 with JAVA_HOME set
- Eclipse (optional)
- Chrome browser (optional)

## How to...

### Eclipse (optional)

The following setup is tested on Eclipse Mars 2 for Java Developers.

We will import this project into Eclipse using [BuildShip](http://gradle.org/eclipse/),
the official Gradle plugin for Eclipse. Eclipse Mars 2 should come bundled with the plugin already.
If not, you can download it in the Eclipse Marketplace.

We can use the plugin simply by starting an `Import...` wizard and then select
`Gradle` as the type of project we like to import. Then just fill in any necessary
information like the location of the project. All optional settings can be left in
their default state.

Buildship will automatically create the Eclipse project files as well as copying the build
dependencies from Gradle into Eclipse. We can now start Gradle tasks in Eclipse's console.
However, starting tasks in Eclipse is not preferred since the Gradle plugin only allows
running one task at a time. Also, the project in Eclipse must be refreshed using
`Refresh Gradle Project` command whenever a change is made to Gradle. You can find this
command using `Quick Access` or by right clicking on the project and select Gradle.

### Development mode

Run the following commands in daemon mode or as separate processes:

    >>>gradlew appengineRun
    >>>gradlew gwtSuperDev

These commands boots up two local servers:

- SuperDevMode at `http://localhost:9876` which listens to requests to recompile your code
- AppEngine at `http://localhost:8080` which serves the app itself

Go to SuperDevMode server in your browser and follow the instructions to save the bookmarks that allow you to recompile your app.
Now you can go to the AppEngine server to view your app.
If you don't see anything, click on the `Dev Mode On` bookmark you saved earlier and then hit `Compile` on the opened modal.
This will sent a request to SuperDevMode server to initiate a recompile and reload your page.

You can avoid the modal by bookmarking the `Compile` button for one-click compile + reload.

Note: it is also recommended to use Chrome browser to debug your applications as it fully supports
source maps used by GWT.

### Deploy

    >>>gradlew appengineUpdate

This uploads the app to App Engine. It will override the version (defined in `appengine-web.xml`) on App Engine if it exists or else it will create that version.

If the uploaded app has a new version, run:

    >>>gradlew appengineSetDefaultVersion

to tell App Engine to start serving the new version on production.

## Project structure

### Layout

- `src/main/java/webapp/` contains web application sources as specificed by the `war` plugin for Gradle.
- `src/main/java/com/jingyu/example/` contains all the GWT and App Engine application logic.

### Important files

- `src/main/java/com/jingyu/example/Example.gwt.xml` this is the main GWT configuration file.
- `src/main/webapp/WEB-INF/web.xml` this is the main url routing file.
- `src/main/webapp/WEB-INF/appengine-web.xml` configuration file for App Engine.

### Ignored files and folders

- Generated by Buildship:
    - `.project`: Main Eclipse project file
    - `.classpath`: Classpath information for Eclipse
    - `.settings/`: Misc settings
- Generated by Gradle:
    - `.gradle/`: Gradle settings
    - `build/`: Compiled Javascripts and resources
    - `war/`: Mirrors `src/main/java/webapp/`
- Generated by Eclipse:
    - `bin/`: Temporary build files
    
## Some common errors

- `the import com.google.appengine cannot be resolved`
    - Problem: You are trying to use App Engine in the client code.
    - Solution: Don't do it.
    
- `Rebind result 'com.something.something.SomeServiceAsync' must be a class`
    - Problem: You are trying to create an async service from an async class.
    - Solution: You have to create the async service from the synchronous version of the service.
    The GWT compiler does some runtime magic to mash the interfaces.
    
- Getting `404`s for RPCs?
    - Did you map the servlets to url in `web.xml`?
    - Did you restart the App Engine server after you made changes server side?

## Resources

### Official links

- https://gradle.org/getting-started-gradle-java/
- http://www.gwtproject.org/
- https://cloud.google.com/appengine/docs/java/
- https://github.com/GoogleCloudPlatform/gradle-appengine-plugin
- https://github.com/steffenschaefer/gwt-gradle-plugin

### Other helpful guides (may not be compatible with this one)

- http://www.blueesoteric.com/blog/2015/05/eggg-eclipse-gwt-gae-and-gradle
- https://examples.javacodegeeks.com/core-java/gradle/gradle-gwt-integration-example/

## Todo...

- [x] Eclipse integration
- [ ] Remote debugging in Eclipse
- [ ] Port over more example code and comments from the example project generated by Google Plugin for Eclipse
- [ ] More example code for both GWT and App Engine