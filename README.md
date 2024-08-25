# Automated Testing of Login Screen

## Objective

The goal of this project is to automate the testing of the login screen for the web application "Germany is Calling" using Selenium with Java. The test scripts cover both successful and unsuccessful login attempts.

## Prerequisites

Before you can run the test scripts, ensure you have the following installed:
- Java JDK 8 or later
- Maven
- ChromeDriver (make sure it's compatible with your Chrome version)

## Project Setup

### 1. Clone the Repository

First, clone the repository to your local machine:

```bash
git clone https://github.com/abdul2810/automated-testing-of-loginscreen.git
cd automated-testing-of-loginscreen
```

### 2. Install Maven Dependencies

Navigate to the project directory where the `pom.xml` file is located and run the following command to install the required dependencies:

```bash
mvn clean install
```

### 3. Update ChromeDriver Path

Make sure to update the path to the `chromedriver.exe` in the `assignment.java` file to point to the location where you have ChromeDriver installed.

## Test Cases

### Test 1: Successful Login

- **Description**: Verifies that a user can successfully log in using valid credentials.
- **Steps**:
  1. Navigate to the login page.
  2. Enter the username: `abdulrzkm28102000@gmail.com`.
  3. Enter the password: `Razack@2810`.
  4. Click the login button.
  5. Verify that the user is redirected to the homepage and click on the profile name.
  6. Use virtual keys to select "Logout" from the dropdown menu.
  7. Verify that the user is redirected back to the login page.

### Test 2: Unsuccessful Login

- **Description**: Verifies that an appropriate error message is displayed when incorrect credentials are provided.
- **Steps**:
  1. Navigate to the login page.
  2. Enter the username: `abdulrzkm28102000@gmail.com`.
  3. Enter an incorrect password: `Razack2810`.
  4. Click the login button.
  5. Verify that an error message is displayed.

## Source Code

Here is the source code for the test cases:

```java
package assignment;

import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

public class assignment {
    WebDriver driver;
    WebDriverWait wait;
    Actions actions;

    @BeforeClass
    public void setup() {
        System.setProperty("webdriver.chrome.driver", "C:\\Users\\abdul\\eclipse-workspace\\Project\\assignment\\Driver\\chromedriver.exe");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.get("https://app.germanyiscalling.com/common/login/?next=https%3A%2F%2Fapp.germanyiscalling.com%2Fcv%2Fhome%2F");

        wait = new WebDriverWait(driver, 10);  // Timeout of 10 seconds
        actions = new Actions(driver);
    }

    @Test
    public void testsuccessfullogin() {
        // Perform login
        WebElement email = wait.until(ExpectedConditions.visibilityOfElementLocated(By.name("username")));
        email.sendKeys("abdulrzkm28102000@gmail.com");

        WebElement pass = wait.until(ExpectedConditions.visibilityOfElementLocated(By.name("password")));
        pass.sendKeys("Razack@2810");

        WebElement loginbtn = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//button[@type='submit']")));
        loginbtn.click();

        // Wait for the profile name element to be visible and clickable
        WebElement profileName = wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//span[contains(text(), 'Abdul Razack M')]")));
        profileName.click();

        actions.sendKeys(Keys.ARROW_DOWN).sendKeys(Keys.ARROW_DOWN).sendKeys(Keys.ENTER).perform();    
    }

    @Test
    public void testunsuccessfullogin() {
        WebElement email = wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//input[@name='username']")));
        email.sendKeys("abdulrzkm28102000@gmail.com");

        WebElement pass = wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//input[@name='password']")));
        pass.sendKeys("Razack2810");

        WebElement loginbtn = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//button[@type='submit']")));
        loginbtn.click();

        // Verify unsuccessful login by checking the error message
        WebElement errorMessage = wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("errorMessageID"))); // Replace with actual element
        Assert.assertTrue(errorMessage.isDisplayed(), "Error message not displayed for unsuccessful login.");
    }

    @AfterClass
    public void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

## Generating Test Reports

Maven Surefire plugin generates test reports after running the tests. You can find these reports in the `target/surefire-reports` directory.

## Assumptions

- The login credentials used in the tests are valid and have access to the application.
- The application URL and element locators (such as `errorMessageID`) are correct and might need to be updated based on changes in the application.

## Additional Information

- **Limitations**: The script relies on hardcoded locators and may need adjustments if the application changes.
- **Challenges**: Handling dynamic content and ensuring compatibility with different versions of Chrome and ChromeDriver.
- **Potential Improvements**: Implement additional test cases for edge cases, such as testing the login page’s response to empty fields or special characters.
