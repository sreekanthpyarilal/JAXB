✅ Created custom package-info.java files per JAXB package with @XmlSchema and @XmlNs annotations to explicitly define prefixes:

@jakarta.xml.bind.annotation.XmlSchema(
  namespace = "http://example.com/entities",
  elementFormDefault = jakarta.xml.bind.annotation.XmlNsForm.QUALIFIED,
  xmlns = {
    @jakarta.xml.bind.annotation.XmlNs(
      prefix = "ent",
      namespaceURI = "http://example.com/entities"
    )
  }
)
package com.example.generated.entities;

import jakarta.xml.bind.annotation.XmlNs;
import jakarta.xml.bind.annotation.XmlSchema;


✅ Placed those files in a custom folder like:

src/main/resources/jaxb-templates/com/example/generated/entities/package-info.java

✅ And finally, during build, overwrote the auto-generated ones using the maven-resources-plugin:

<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-resources-plugin</artifactId>
  <version>3.3.1</version>
  <executions>
    <execution>
      <id>override-package-info</id>
      <phase>generate-sources</phase>
      <goals><goal>copy-resources</goal></goals>
      <configuration>
        <outputDirectory>${project.build.directory}/generated-sources</outputDirectory>
        <resources>
          <resource>
            <directory>src/main/resources/jaxb-templates</directory>
            <includes><include>**/package-info.java</include></includes>
          </resource>
        </resources>
        <overwrite>true</overwrite>
      </configuration>
    </execution>
  </executions>
</plugin>
