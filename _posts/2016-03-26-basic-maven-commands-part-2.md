---
title: "Basic Maven Commands Part 2"
description: ""
category: 
tags: [maven]
---
{% include JB/setup %}

Here are some basic maven commands for setting up your project:  
**JavaWebapp**
{% highlight shell %}
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-webapp -DgroupId=com.mycompany -DartifactId=JavaWebapp -Dversion=0.1-SNAPSHOT -Dpackage=com.mycompany -DinteractiveMode=false
{% endhighlight %}

**JavaProgram**  
maven commands  
create jar project  
{% highlight shell %}
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.mycompany -DartifactId=JavaProgram -Dversion=0.1-SNAPSHOT -Dpackage=com.mycompany -DinteractiveMode=false
{% endhighlight %}
create executable jar  
add to pom.xml  
{% highlight xml %}
<build>
	<finalName>${project.artifactId}</finalName>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-shade-plugin</artifactId>
			<version>${version.shaded.plugin}</version>
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>shade</goal>
					</goals>
					<configuration>
						<transformers>
							<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
								<mainClass>com.mycompany.App</mainClass>
							</transformer>
						</transformers>
						<filters>
							<filter>
								<artifact>*:*</artifact>
								<excludes>
									<exclude>META-INF/*.SF</exclude>
									<exclude>META-INF/*.DSA</exclude>
									<exclude>META-INF/*.RSA</exclude>
								</excludes>
							</filter>
						</filters>
					</configuration>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
{% endhighlight %}
