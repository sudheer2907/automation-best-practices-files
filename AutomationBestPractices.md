# Automation Best Practices

**1. Implement some of the important tools which checks checkstyle issue, PMD/CPD and FindBugs issues into your code.**
Note: Before you enable these rules, make sure that you have full understanding about Checkstyle Issues, PMD/CPD and about FindBugs.

Let's start implementing these into your project. The important criteria to implement or to enable these into your automation project is that your project should of type maven.
Please follow these below steps to implemet the same:
1. Create a folder code-compliances into your project and keep these three files into it
    A. checkstyle-rules.xml
    B. code-formatter.xml
    C. code-formatter-intellij.xml
2. Add these below properties into your pom.xml

		<checksyle.rule.file>code-compliances/checkstyle-rules.xml</checksyle.rule.file>
		<checkstyle.check.skip>false</checkstyle.check.skip>
		<cpd.skip>false</cpd.skip>
		<pmd.check.skip>false</pmd.check.skip>
		<findbugs.check.skip>false</findbugs.check.skip>
		<checkstyle.failure>true</checkstyle.failure>
		<pmd.failure>true</pmd.failure>
		<fail.on.pmd.cmd.findbugs.error>true</fail.on.pmd.cmd.findbugs.error>

3. Add these below pluggins into your pom.xml

			<plugin>
				<artifactId>maven-pmd-plugin</artifactId>
				<groupId>org.apache.maven.plugins</groupId>
				<version>3.10.0</version>
				<configuration>
					<includeTests>true</includeTests>
					<targetJdk>${jre.level}</targetJdk>
					<printFailingErrors>true</printFailingErrors>
					<failOnViolation>${pmd.failure}</failOnViolation>
					<linkXRef>false</linkXRef>
				</configuration>
				<executions>
					<execution>
						<phase>compile</phase>
						<goals>
							<goal>pmd</goal>
							<goal>cpd</goal>
							<goal>cpd-check</goal>
							<goal>check</goal>
						</goals>
					</execution>
				</executions>
			</plugin>

			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>findbugs-maven-plugin</artifactId>
				<version>3.0.4</version>
				<configuration>
					<xmlOutput>true</xmlOutput>
					<xmlOutputDirectory>target/site</xmlOutputDirectory>
					<failOnError>${fail.on.pmd.cmd.findbugs.error}</failOnError>
				</configuration>
				<executions>
					<execution>
						<phase>compile</phase>
						<goals>
							<goal>check</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
      			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-checkstyle-plugin</artifactId>
				<version>3.1.0</version>
				<configuration>
					<configLocation>${checksyle.rule.file}</configLocation>
					<failOnViolation>${checkstyle.failure}</failOnViolation>
					<encoding>UTF-8</encoding>
					<consoleOutput>true</consoleOutput>
					<failsOnError>${failOnError}</failsOnError>
					<linkXRef>true</linkXRef>
				</configuration>
				<executions>
					<execution>
						<id>validate</id>
						<phase>validate</phase>
						<goals>
							<goal>check</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
      
4. Ones you are done with the changes, execute below command to get list of issues into your code - mvn clean compile
You will get result like this

* [INFO] BugInstance size is 0
* [INFO] Error size is 0
* [INFO] No errors/warnings found
* [INFO] ------------------------------------------------------------------------
* [INFO] BUILD SUCCESS
* [INFO] ------------------------------------------------------------------------
