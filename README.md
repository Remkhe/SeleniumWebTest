# SeleniumWebTest
Снитчи для селениума
package NewInternet;

import io.github.bonigarcia.seljup.SeleniumJupiter;
import org.junit.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;

import java.io.File;
import java.nio.file.Files;
import java.nio.file.Path;
import java.time.Duration;
import java.time.temporal.ChronoUnit;
import java.util.List;

import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(SeleniumJupiter.class)

public class TestWebElementInterfaceTest2 {
    @Test
    public void testFind(ChromeDriver driver) {
        driver.get("https://the-internet.herokuapp.com/upload");

        String locator = "div";

        WebElement button = driver.findElement(By.cssSelector("#file-submit"));
        List<WebElement> buttons = (List<WebElement>) driver.findElement(By.cssSelector("#file-submit"));
        assertEquals(1, buttons.size());


        //driver.navigate().refresh();
        //WebElement button1   =  driver.findElement(By.cssSelector("#file-submit"));
        //button.click();


    }

    @Test
    public void testFindElements(ChromeDriver driver) {
        driver.get("https://the-internet.herokuapp.com/upload");

        String locator = "div";

        WebElement div = driver.findElement(By.cssSelector(locator));
        List<WebElement> divs = (List<WebElement>) driver.findElement(By.cssSelector(locator));
        assertEquals(div, divs.get(0));
    }
    @Test
    public void searchContextTest(ChromeDriver driver){
        //сужаем контекст поиска
        driver.get("https://the-internet.herokuapp.com/login");
        WebElement form = (WebElement) (WebElement) driver.findElements(By.cssSelector("form#login[name='login']"));
        form.findElement(By.cssSelector("button")).click();
        //form.submit(); это одно и тоже.


    }
    @Test
    public void cssVsXpathTest(ChromeDriver driver){
        driver.get("https://the-internet.herokuapp.com/login");
        String classes = "large - 6 small - 12 columns";
        WebElement css = driver.findElement(By.cssSelector(".large - 6 .small - 12 .columns"));
        WebElement XPath = driver.findElement(By.xpath("//div[@class='large - 6 small - 12 columns']"));
        assertEquals(css,XPath);
    }
    
    
    @Test
    public void nosuchTest(ChromeDriver driver){
        driver.get("https://the-internet.herokuapp.com/login");
        driver.findElement(By.cssSelector("#nosuch-element")); //nosuchElementException
        List <WebElement> elements = driver.findElements(By.cssSelector("#nosuch-element"));
        //если мы ищем список то тест не падает
        assertEquals(0,elements.size());


        
    }


    @Test
    public void clickTest(ChromeDriver driver){
        //что мы умеем делать с элементами, тест проверяет клик по кнопке
        driver.get("https://the-internet.herokuapp.com/status_codes");
        driver.findElement(By.cssSelector("li a")).click();
        assertTrue(driver.getCurrentUrl().endsWith("/status_codes/200"));


    }
    @Test
    public void sendKeysTest(ChromeDriver driver){
        //отправка в инпут значения любого текста цифр
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(4, ChronoUnit.SECONDS.ordinal()));
        driver.get("https://ya.ru/");

       WebElement input =  driver.findElement(By.cssSelector("#text"));
       input.sendKeys("Test", "", "12345");
       input.sendKeys(Keys.chord(Keys.LEFT_SHIFT, Keys.COMMAND,Keys.ARROW_LEFT),Keys.BACK_SPACE);//все клавиши нажмет паралельно
       input.sendKeys("Test", Keys.RETURN);//имитация клавиш с клавиатуры

    }
    @Test
    public void clearInputTest(ChromeDriver driver){
        //тест чтобы чистить ввод, это про то чтобы гарантировано вычистить поле, может понадобиться при очистке формы авторизации
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(4, ChronoUnit.SECONDS.ordinal()));
        driver.get("https://ya.ru/");
        WebElement input =  driver.findElement(By.cssSelector("#text"));
        input.sendKeys("Test", "", "12345");
        input.clear();
        input.sendKeys("inno");

        }

    @Test
    public void getPropsTest (ChromeDriver driver){
        driver.get("https://the-internet.herokuapp.com/key_presses");
        WebElement h3 =  driver.findElement(By.cssSelector("h3"));
        String text = h3.getText();
        boolean isDisplayed = h3.isDisplayed();// проверить виден ли элемент, отображается ли цена, отображается ли кнопка
        boolean isEnabled = h3.isEnabled();
        driver.get("https://the-internet.herokuapp.com/checkboxes");
        List<WebElement> checkboxes = (List<WebElement>) driver.findElement(By.cssSelector("#checkboxes")).findElement(By.cssSelector("#input"));
        boolean cb1Selected = checkboxes.get(0).isSelected(); // проставлено ли то что выбрано
        boolean cb2Selected = checkboxes.get(1).isSelected();
        String fontSize =  h3.getCssValue("font-size");
        // h3.getAttribute();//спросить у элемента какой нибудь атрибут
        assertEquals("27px",fontSize);
        assertFalse(cb1Selected);
        assertTrue(cb2Selected);


        assertEquals("Key Presses",h3);
        assertTrue(isDisplayed);
        assertTrue(isEnabled); // отображает бонусные баллы например если они есть енэблед есть, если их нет он исчезает.
        String href = driver.findElement(By.cssSelector("a")).getAttribute("href");


    }
@Test
    public void uploadFileTest(ChromeDriver driver){
        //тест на написание нужного имени файла во кладке загрузить файлы, должно написать pom.xml
        driver.get("https://the-internet.herokuapp.com/upload");
        driver.findElement(By.cssSelector("#file-upload")).sendKeys("C:\\Users\\user\\IdeaProjects\\Internet\\pom.xml");
        driver.findElement(By.cssSelector("#file-submit")).click();
        String text = driver.findElement(By.cssSelector("#uploaded-files")).getText();
        assertEquals("pom.xml", text);



}

@Test
    public  void screenTest(ChromeDriver driver){
        driver.get("https://the-internet.herokuapp.com/login");
       File formScreenShot =  driver.findElement(By.cssSelector("form")).getScreenshotAs(OutputType.FILE);
       Files.move(formScreenShot.toPath(), Path.of("./my_screen.png"));
}

