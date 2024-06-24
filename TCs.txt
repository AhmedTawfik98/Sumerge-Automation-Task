import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import static org.testng.Assert.assertEquals;
import static org.testng.Assert.assertTrue;

public class TestSuite {

    private WebDriver driver;
    private final By usernameFld = By.id("user-name");
    private final By passwordFld = By.id("password");
    private final By loginBtn = By.id("login-button");
    private final By logo = By.cssSelector(".app_logo");
    private final By errorMsg = By.cssSelector("[data-test='error']");

    // executed before all test Cases
    @BeforeMethod
    public void setUp(){
        System.setProperty("webdriver.chrome.driver", "E:\\DEPI\\SumrgeAssessment\\drivers\\chromedriver-win64\\chromedriver.exe");
        driver = new ChromeDriver();

        // Maximize the window
        driver.manage().window().maximize();

        // Navigate to the login page
        driver.get("https://www.saucedemo.com/");
    }

    // executed after all test Cases
    @AfterMethod
    public void tearDown(){
        driver.quit();
    }


    // 1. Check if the username and password fields are on the main screen of the application
    @Test
    public void testLoginAndPasswordFieldsExists() {
        assertTrue(driver.findElement(usernameFld).isDisplayed());
        assertTrue(driver.findElement(passwordFld).isDisplayed());
        assertTrue(driver.findElement(loginBtn).isDisplayed());
    }

    //2. Check if the given valid credentials work
    @Test
    public void testLoginWithValidCredentials(){
        driver.findElement(usernameFld).sendKeys("standard_user");
        driver.findElement(passwordFld).sendKeys("secret_sauce");
        driver.findElement(loginBtn).click();
        assertTrue(driver.findElement(logo).isDisplayed());
    }


    // 3. Check if the given wrong credentials work
    @Test
    public void testLoginWithInvalidCredentials(){
        driver.findElement(usernameFld).sendKeys("Ahmed Tawfik");
        driver.findElement(passwordFld).sendKeys("12345");
        driver.findElement(loginBtn).click();
        assertTrue(driver.findElement(errorMsg).isDisplayed());
        assertEquals(driver.findElement(errorMsg).getText(),
                "Epic sadface: Username and password do not match any user in this service");
    }


    // 4- Check for empty credentials
    // i-test Login With  Empty username and valid Password
    @Test
    public void testLoginWithEmptyUsername(){
        driver.findElement(usernameFld).clear();
        driver.findElement(passwordFld).sendKeys("secret_sauce");
        driver.findElement(loginBtn).click();
        assertTrue(driver.findElement(errorMsg).isDisplayed());
        assertEquals(driver.findElement(errorMsg).getText(), "Epic sadface: Username is required");
    }

    // ii- test Login With  Empty Password and valid username
    @Test
    public void testLoginWithEmptyPassword(){
        driver.findElement(usernameFld).sendKeys("standard_user");
        driver.findElement(passwordFld).clear();
        driver.findElement(loginBtn).click();
        assertTrue(driver.findElement(errorMsg).isDisplayed());
        assertEquals(driver.findElement(errorMsg).getText(), "Epic sadface: Password is required");
    }


        // iii- test Login With  Empty Credentials in Both of them
    @Test
    public void testLoginWithEmptyCredentials(){
        driver.findElement(usernameFld).clear();
        driver.findElement(passwordFld).clear();
        driver.findElement(loginBtn).click();
        assertTrue(driver.findElement(errorMsg).isDisplayed());
        assertEquals(driver.findElement(errorMsg).getText(), "Epic sadface: Username is required");
        // in this case both user and password are empty and the actual result is only  a div with class error-message-container error containing a message Epic sadface: Username is required is visible and there is no message "Epic sadface: Password is required" is visible

    }
}
