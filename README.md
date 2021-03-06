How to run Selenium tests without cloud solution?
-------------------------------------------------



**Read key values from build options. You should read browser type from this option**
System.getProperties().getProperty("browser")

**Example Selenium webdriver code block**


      public static void openBrowser(String url) {
	  if (System.getProperties().getProperty("browser").equals("FIREFOX")) {

            FirefoxProfile profile = new FirefoxProfile();
            profile.setAcceptUntrustedCertificates(true);
            driver = new FirefoxDriver(profile);
            driver.get(url);

        } else if (System.getProperties().getProperty("browser").equals("CHROME")) {
        ...
        ..


**Add Java Maven surefire plugin to your pom.xml for parallel test execution**


    <project>
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.19</version>
            <executions>
                <execution>
                    <goals>
                        <goal>test</goal>
                    </goals>
                    <configuration>
                        <skip>false</skip>
                    </configuration>
                </execution>
            </executions>
            <configuration combine.self="override">
                <forkCount>2</forkCount>
                <reuseForks>true</reuseForks>
                <argLine>-Xms512m -Xmx1024m -XX:MaxPermSize=512m</argLine>
                <includes>
                    <include>**\RunTestSuite1.java</include>
                    <include>**\RunTestSuite2.java</include>
                </includes>
                <skipTests>false</skipTests>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-site-plugin</artifactId>
                <version>3.3</version>
            </plugin>
        </plugins>
        </build>
    ..
    ..
    
    </project>

**Example terminal command to execute for CI integration like Jenkins:**
 "maven clean test -Dbrowser=FIREFOX"

**Example IntelliJ Usage for debugging and developing**
 Run-> Edit Configurations -> Defaults -> Junit -> VM options
-Dbrowser=FIREFOX
-Dbrowser=CHROME
-Dbrowser=IE
-Dbrowser=SAFARI

**There are different test running models. You should read the following article.** 
https://saucelabs.com/resources/articles/selenium-jenkins-how-to-do-it-yourself-and-the-sauce-labs-advantage


> Written with [StackEdit](https://stackedit.io/).

