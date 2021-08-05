[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.5.3-green.svg)](https://projects.spring.io/spring-boot/)
[![Spring Boot](https://img.shields.io/badge/Default%20JDK-11-green.svg)](https://projects.spring.io/spring-boot/)
[![Spring Boot](https://img.shields.io/badge/Support%20JDKs-9%2C11%2C14-green.svg)](https://projects.spring.io/spring-boot/)
[![Proguard Maven Plugin](https://img.shields.io/badge/Proguard%20Maven%20Plugin-7.0.0-blue.svg)](https://sourceforge.net/projects/proguard/)

## proguard-spring-boot-example
Proguard Obfuscate Spring Boot Maven Plugin Example

For big spring boot projects we can't obfuscate all files because for example may be will break spring autowiring 
(by default spring autowiring search classes by variable name) and other blocks. So we must check every obfuscation 
rules by running .jar file. 

Most of the common rules specify in the pom.xml.

## Decompiling files by Intellij
We must unzip .jar and put package files in the project/target directory. Then we can open target directory in Intellij 
project-structure window and open class. This will be real decompiled class.

## Before use
First you must change the setting ```<java.version>11</java.version>``` in the **pom.xml** to match your JDK version,
for example:
  - for JDK 9 change to ```<java.version>9</java.version>```
  - for JDK 14 change to ```<java.version>14</java.version>```

## After building
After building you can run ```spring.boot.jar``` in the **target** directory, for example:
```shell script
java -jar spring.boot.jar
```

## Supporting JDK
**Currently supported JDK 9, JDK 11 and JDK 14 (maybe also JDK 9, JDK 10 and JDK 13 - I've not tested them)!**

JDK after 1.8 (9, 10, 11 etc.) now using modular structure, so they don't contain files 
**rt.jar** and **jce.jar**.

I remove all libraries from the **libs** section in pom (empty block **<libs></libs>**). If your application want
them you can add system libraries manually, for example:
```
<libs>
    <lib>${java.home}/lib/rt.jar</lib>
    <lib>${java.home}/lib/jce.jar</lib>
</libs>
```
For JDK 11 you can try to add all modules from **<jdk-path>/jmods** directory to the **libs** section in maven pom.

## How it works
Obfuscation step must go before packaging **.jar** so in pom file you can see right order of steps.

We use **proguard-maven-plugin**. In the configuration, you can pass any options from the original **proguard project**,
but some options may cause failure so recheck any changes. See [proguard documentation](https://www.guardsquare.com/en/products/proguard/manual/examples#application)
for all available options and descriptions.

## Run JDKs support tests
To run tests from the root directory run command:
```shell script
docker-compose -f src/test/docker/docker-compose.yml up
```
After all containers startup successfully we can stop all by command:
```shell script
docker-compose -f src/test/docker/docker-compose.yml down
```

## Troubleshooting
  - If application can't start try remove some classes/packages from obfuscation (option **-keep**)
  - We use option **-dontoptimize**, but you can try to use optimization but recheck required after every changes
  - If maven build failed check your JDK version (**currently supports only JDK 9, JDK 11 and JDK 14**). See also error 
    output, maybe you compile under JDK 11 and not removed libraries from the **libs** block so **proguard**
    can't find **rt.jar** and others.
  - Some options from the **proguard project** may not work properly, recheck any new option

#### Used banner font
[https://devops.datenkollektiv.de/banner.txt/index.html](https://devops.datenkollektiv.de/banner.txt/index.html)

Font: larry3d
