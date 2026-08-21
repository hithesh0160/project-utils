# Java Selenium TestNG

This stack is useful for browser automation, web testing, and simple scheduled web tasks.

## Used In

- `Naukri` - Selenium resume upload automation.
- `Shopping` - Selenium/TestNG plus product scraping and notification flow.
- `ActitimeV2`, `Actitime01` - older Selenium-style automation.

## Maven Dependencies

```xml
<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-java</artifactId>
  <version>4.34.0</version>
</dependency>
<dependency>
  <groupId>org.testng</groupId>
  <artifactId>testng</artifactId>
  <version>7.11.0</version>
</dependency>
```

## Surefire TestNG Setup

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-surefire-plugin</artifactId>
  <version>3.0.0-M7</version>
  <configuration>
    <suiteXmlFiles>
      <suiteXmlFile>testng.xml</suiteXmlFile>
    </suiteXmlFiles>
  </configuration>
</plugin>
```

## Commands

```bash
mvn clean test
mvn test -Dstyle.color=never -Dsurefire.printSummary=true -Dsurefire.useFile=false
```

## Checklist

- Browser setup works headless in CI.
- Locators are stable.
- Credentials are secrets.
- Test output is saved when CI runs.
- Retries are used only when instability is understood.

