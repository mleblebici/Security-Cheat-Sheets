# Table of Contents
[Fortify SCA](#p1)

[Analysis of Java Maven Projects](#p2)

[Analysis of Python Projects](#p3)

[Analysis of Android Projects](#p4)

[Analysis of iOS Projects](#p5)

[Analysis of Java Projects](#p6)

[Analysis of ASP.NET Projects](#p7)

[Analysis of Projects Built With Make](#p8)

[Fortify SCA Useful Commands](#p9)

<a name="p1"/>

# FORTIFY SCA
Fortify Source Code Analyzer (SCA) performs source code analysis of applications written in various programming languages. Regardless of the language(s) used in applications, Fortify SCA performs source code analysis in two main stages. First one is translation stage where source code belonging to application is converted to an intermediate language. After translation, scan stage is the actual stage where the weaknesses are revealed. Generally, translation stage requires more time and effort. Most of the problems occuring in this stage are due to the fact that SCA is not able to reach some libraries that are used in the application. 
Fortify Source Code Analyzer comes with an user interface where you can select your project and press Go. However, this way of doing source code analysis is not recommended, as build warnings generated during translations stage are ignored. Warnings in SCA should not be considered as similar to IDE warnings like unused method or indentation, SCA can give warnings when it cannot find a method or class. In such a case, other classes using these are covered partially. This affects code coverage and SCA's ability to trace code stack seriously. IDE Plugins like those for Visual Studio, Eclipse IDE and Intellij IDEA also ignores warnings and generates reports based on code that is left after warnings. Therefore, the recommended way to perform source code analysis is usage of sourceanalyzer command with logging and resolving all warnings. This document describes analysis of applications written in different languages with sourceanalyzer command.

<a name="p2"/>

## Analysis of Java Maven Projects
Project
Maven projects can be analyzed using Fortify SCA's maven plugin. To do so, maven must be installed on the analysis machine. Maven can be installed as follows:
* Download maven from [Apache Maven website](http://maven.apache.org/download.cgi). The version used in application should be downloaded.
* Extract the compressed file to "Program Files\Apache\Maven" folder.
* Create M2_HOME and MAVEN_HOME environment variables pointing to above path.
* On the command line, run mvn -version command to check if it is installed successfully. 

After installation of Maven, Fortify Maven plugin must be installed as follows:
* Fo to [Fortify Home]\Samples\advanced\maven-plugin.
* Install the plugin with mvn clean package install.

After installation of Fortify maven plugin, you can perform the source code analysis as follows:
* Go to the path of the project to be analyzed. 
* First, run mvn clean command.
* After that run the following command.

```
mvn install –Dmaven.test.skip=true –Dfortify.sca.source.version=[the java version of the project, e.g. 1.6, 1.7 or 1.8] –Dfortify.sca.buildId=[project build id] –Dfortify.sca.logfile=[optional logfile] com.fortify.ps.maven.plugin:sca-maven-plugin:translate
```
* The above command performs the translation of the project. Once translated, you should always check build warnings and resolve them to ensure full code coverage. Next, the following command will analyze the project source code and generate the report.

```
sourceanalyzer –b [project build id] –scan –f [project report name].fpr
```


### Notes
Generally, organizations use their own local repositories for libraries used in the project. For a complete source code analysis, you should ensure that these libraries are seen by the SCA. If SCA does not have access to project's private repositories or there is another problem causing missing library warnings, you can request all libraries and put them in a local maven repository and perform the source code analysis. 

For any reason, if you cannot use Fortify maven plugin, in order to make translation easy you can build the project with maven and add folders containing maven repositories to sourceanalyzer classpath command switch. Doing so enables you to gather jar files required for translation. 

<a name="p3"/>

## Analysis of Python Projects
**NOTE**: Currently Fortify SCA only supports versions up to Python 2.7. Projects written with Python 3.0 can be converted to Python 2.7 and only after that can be analyzed. However, this should be considered while auditing findings.

Python version which the project is built with should be installed on the analysis machine. Fortify SCA requires all Python packages that the project imports to be placed locally. Since it translates the imported packages as well, translation stage can take a relatively long time. Location of imported packages and python installation path should be given to --python-path command switch separated with ;. No additional command switches are required for translation and analysis and reporting is performed with -scan and -f command switches. 

<a name="p4"/>

## Analysis of Android Projects
Analysis of Android applications can be done with Fortify Android Studio plugin. In order to install Android Studio plugin, go to Android Studio/Preferences/Plugins. Click Install Plugin from disk and choose the file located under [Fortify Installation path]/plugins/intellij. Click Apply to install the plugin. After restarting Android Studio, you will see an additional Fortify tab. After installation of plugin, build the application with Android Studio. After successfully building the project, click Analyze Project(Scan Project) under Fortify tab. The report is generated in application root. Since application is analyzed with plugin, you should always check build warnings with sourceanalyzer -show-build-warnings command switch. In case of build warnings, resolve the warnings and repeat the above steps for more accurate analysis. 

<a name="p5"/>

## Analysis of iOS Projects

Fortify SCA supports XCode 7.3 and prior versions. Therefore, if analysis is to performed with XCode plugin, XCode 7.3 should be installed in analysis machine. iOS projects can be easily analyzed with XCode plugin like Android applications, however as always you should check build warnings after plugin finishes the analysis. 
You can also use the following command to translate the project source code: 
```
sourceanalyzer -b buildID -debug -logfile translation.log xcodebuild -project ProjectName.xcodeproj -sdk iphoneos clean build CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
```

OR

```
sourceanalyzer -b buildID -debug -logfile log.log  xcodebuild -scheme SCHEME_NAME -project PROJECT_NAME.xcodeproj -configuration Debug -destination 'platform=iOS Simulator,name=iPhone 6 Plus,OS=10.2' clean build RUN_CLANG_STATIC_ANALYZER=YES CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
```

After translation, you can perform the analysis and generate the report with the following command:
```
sourceanalyzer –b buildID –logfile scan.log –scan –f ProjectName.fpr 
```

<a name="p6"/>

## Analysis of Java Projects
Prior to source code analysis, all jar files that the project uses should be gathered in a dedicated folder and this folder's path information should be given to sourceanalyzer command with -cp command switch. After gathering all jar files and specifying them to sourceanalyzer command, you can run the following command to translate your Java project:

```
sourceanalyzer –b JavaProject –cp JarFiles\**\*.jar ProjectPath –logfile translation.log –jdk jdkVersion
```
As always, you can then run the following command to perform analysis and generate the report.
```
sourceanalyzer –b JavaProject –scan –f JavaProject.fpr –logfile scan.log
```

<a name="p7"/>

## Analysis of ASP.NET Projects
Analysis of ASP.NET projects can be easily done with Visual Studio Developer Command Prompt. Using developer command prompt significantly reduces the number of build warnings. You should go to the path where the sln file is located with developer command prompt. You should first build the project with the following command:

```
devenv <Solution_file>.sln /Rebuild Debug
```

You should resolve all errors or warnings generated after the above build attempt. After successfully building the project with above command, you can run following commands to clean the project and translate it.

```
devenv <Solution_file>.sln /Clean
```
```
sourceanalyzer –b slnProject –logfile translation.log devenv Project.sln /Rebuild Debug
```

Similar to other projects, you can then run the following command to analyze the project and generate the report.
```
sourceanalyzer –b slnProject –logfile scan.log –scan –f slnProject.fpr
```

<a name="p8"/>

## Analysis of Projects Built With Make
You should run the following commands on a Linux machine on which Fortify is installed to perform translation and the analysis.

```
./configure
sourceanalyzer –b Project make –logfile translation.log
sourceanalyzer –b Project –scan –f Project.fpr –logfile scan.log
```

<a name="p9"/>

## Fortify SCA Useful Commands
* To list all build projects:

```
sourceanalyzer –show-build-ids
```
* To list all files included by a specific project(build Id):

```
sourceanalyzer –b buildID –show-files
```
* To learn total line of code covered by a specific project(build Id):

```
sourceanalyzer –b buildID –show-loc
```
* To list all build warnings and errors associated with a build project:

```
sourceanalyzer –b buildID –show-build-warnings
```

* In order to perform the source code analysis on an external disc without generating intermediate files on the host machine:
```
sourceanalyzer -b buildID -Dcom.fortify.sca.ProjectRoot F:\
```

* To use a specific encoding schema during translation:
```
sourceanalyzer -b buildID -encoding UTF-8
```
