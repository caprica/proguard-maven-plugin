proguard-maven-plugin
=====================

Maven Plugin for ProGuard.

We are not the original author of this project, it is a fork of https://github.com/dingxin/proguard-maven-plugin
licensed under the Apache License, Version 2.0. 

What have we changed from the original version?

 - plugin compiled for source/target Java 6 rather than Java 9
 - changed Maven package and artifact names so as not to conflict with the original plugin
 - removed setting of default options when no options are provided even though a valid configuration file was specified
 - changed the name of the default configuration file `src/main/proguard/proguard.conf`
 
We gladly give credit to the original plug-in author at https://github.com/dingxin.

Usage
=====

Add plugin configuration into your project pom.xml:

```
<build>
    <plugins>
        <plugin>
            <groupId>uk.co.caprica.maven.proguard</groupId>
            <artifactId>proguard-maven-plugin</artifactId>
            <version>1.0.0</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>proguard</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
            ...
            </configuration>
        </plugin>
    </plugins>
</build>
```

Supported `<configuration>` children as below:

### Disables the plugin execution ###
```
<skip>false</skip>
```

### ProGuard options ###
```
<options>
	<option>-dontoptimize</option>
	<option>-keepattributes *Annotation*</option>
	...
</options>
```

No default options are provided.
 
You should specify options here, or via the configuration file (see below), or a combination of the two.

### ProGuard configuration file ###
```
<configFile>proguard.conf</configFile>
```

If you do not specify a configuration file, `src/main/proguard/proguard.conf` will be used by default if it exists.

### ProGuard Filters for the input jar ###
```
<inFilter>!module-info.class,!META-INF/maven/**</inFilter>
```

### ProGuard Filters for the output jar ###
```
<outFilter>!META-INF/maven/**</outFilter>
```

### Add dependency jars to -libraryjars arguments ###
```
<includeDependency>true</includeDependency>
```

### Add dependency jars to -injars arguments ###
```
<includeDependencyInjar>true</includeDependencyInjar>
```

### ProGuard Filters for dependency jars ###
```
<dependencyFilter>!module-info.class,!META-INF/**</dependencyFilter>
```

### Additional -libraryjars ###
```
<libs>
	<lib>${java.home}/jmods/java.base.jmod(!**.jar;!module-info.class)</lib>
</libs>
```

## Examples ##

### Example for Java SE 9 ###
```
<build>
	<plugins>
		<plugin>
			<groupId>uk.co.caprica</groupId>
			<artifactId>proguard-maven-plugin</artifactId>
			<version>1.0.0</version>
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>proguard</goal>
					</goals>
				</execution>
			</executions>
			<configuration>
				<libs>
					<lib>${java.home}/jmods/java.base.jmod(!**.jar;!module-info.class)</lib>
				</libs>
			</configuration>
		</plugin>
	</plugins>
</build>
```

### Example for Java SE 8 ###
```
<build>
	<plugins>
		<plugin>
			<groupId>uk.co.caprica</groupId>
			<artifactId>proguard-maven-plugin</artifactId>
			<version>1.0.0</version>
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>proguard</goal>
					</goals>
				</execution>
			</executions>
			<configuration>
				<libs>
					<lib>${java.home}/lib/rt.jar</lib>
					<lib>${java.home}/lib/jsse.jar</lib>
					<lib>${java.home}/lib/jce.jar</lib>
				</libs>
			</configuration>
		</plugin>
	</plugins>
</build>
```

### Example for war project ###
```
<build>
	<plugins>
		<plugin>
			<groupId>uk.co.caprica</groupId>
			<artifactId>proguard-maven-plugin</artifactId>
			<version>1.0.0</version>
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>proguard</goal>
					</goals>
				</execution>
			</executions>
			<configuration>
				<options>
					<option>-dontoptimize</option>
					<option>-keepattributes *Annotation*</option>
					<option>-keepattributes Signature</option>
					<option>-keepattributes InnerClasses</option>
					<option>-keepclassmembers class * { @**.* *; }</option>
					<option>-keep public class * { public protected *; }</option>
					<option>-dontwarn org.apache.logging.log4j.**</option>
				</options>
				<libs>
					<lib>${java.home}/jmods/java.base.jmod(!**.jar;!module-info.class)</lib>
				</libs>
			</configuration>
		</plugin>
	</plugins>
</build>
```

### Example with Custom Options ###
```
<build>
	<plugins>
		<plugin>
			<groupId>uk.co.caprica</groupId>
			<artifactId>proguard-maven-plugin</artifactId>
			<version>1.0.0</version>
			<executions>
				<execution>
					<phase>prepare-package</phase>
					<goals>
						<goal>proguard</goal>
					</goals>
				</execution>
			</executions>
			<configuration>
				<libs>
					<lib>${java.home}/jmods/java.base.jmod(!**.jar;!module-info.class)</lib>
				</libs>
			</configuration>
		</plugin>
	</plugins>
</build>
```
