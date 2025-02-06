import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class AuthTests {
    private WebDriver driver;

    @BeforeMethod
    public void setUp() {
        // Set up WebDriver (ensure chromedriver path is set correctly)
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.get("https://xaltsocnportal.web.app");
    }

    @Test
    public void testSignUpSuccess() {
        driver.findElement(By.linkText("Sign Up"))
              .click();

        // Fill in valid email and password
        driver.findElement(By.id("email"))
              .sendKeys("testuser@example.com");
        driver.findElement(By.id("password"))
              .sendKeys("ValidPass123!");

        // Click on Sign Up
        driver.findElement(By.id("signup-button"))
              .click();

        // Verify user is redirected to dashboard or success message is displayed
        WebElement successMessage = driver.findElement(By.id("success-message"));
        Assert.assertTrue(successMessage.isDisplayed(), "Sign Up Success message not displayed!");
    }

    @Test
    public void testSignUpFailureInvalidEmail() {
        driver.findElement(By.linkText("Sign Up"))
              .click();

        // Fill in invalid email and valid password
        driver.findElement(By.id("email"))
              .sendKeys("invalid-email");
        driver.findElement(By.id("password"))
              .sendKeys("ValidPass123!");

        // Click on Sign Up
        driver.findElement(By.id("signup-button"))
              .click();

        // Verify error message is displayed
        WebElement errorMessage = driver.findElement(By.id("error-message"));
        Assert.assertTrue(errorMessage.isDisplayed(), "Error message not displayed for invalid email!");
    }

    @Test
    public void testSignInSuccess() {
        driver.findElement(By.linkText("Sign In"))
              .click();

        // Fill in valid email and password
        driver.findElement(By.id("email"))
              .sendKeys("testuser@example.com");
        driver.findElement(By.id("password"))
              .sendKeys("ValidPass123!");

        // Click on Sign In
        driver.findElement(By.id("signin-button"))
              .click();

        // Verify user is redirected to dashboard
        WebElement dashboard = driver.findElement(By.id("dashboard"));
        Assert.assertTrue(dashboard.isDisplayed(), "Dashboard not displayed after sign in!");
    }

    @Test
    public void testSignOut() {
        // Sign in first
        testSignInSuccess();

        // Click on Sign Out
        driver.findElement(By.id("signout-button"))
              .click();

        // Verify user is redirected to the login page
        WebElement loginPage = driver.findElement(By.id("login-page"));
        Assert.assertTrue(loginPage.isDisplayed(), "Login page not displayed after sign out!");
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
