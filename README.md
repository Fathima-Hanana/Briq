# Web Table to CSV Generate

```sh
package test;

import java.io.FileWriter;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.time.Duration;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.edge.EdgeDriver;

public class AddTableDataInCSVFile {
	
	public static void generateCSVFile(List<List<String>> data) {
		
        Date currentDate = new Date();
        SimpleDateFormat dateFormat = new SimpleDateFormat("MM-dd-yy-HH-mm-ss");
        String formattedDate = dateFormat.format(currentDate);

        String fileName = "./CSVFiles/" + formattedDate + ".csv";

        try {
            FileWriter writer = new FileWriter(fileName);
            for (List<String> row : data) {
                writer.append(String.join(",", row));
                writer.append("\n");
            }
            writer.flush();
            writer.close();
            System.out.println("CSV file created: " + fileName);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
	public static void main(String[] args) {
		WebDriver driver = new EdgeDriver();
		
		driver.get("https://the-internet.herokuapp.com/challenging_dom");
		
		driver.manage().window().maximize();
		driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(15));
		
		List<List<String>> data = new ArrayList<>();
		
		ArrayList<String> headers = new ArrayList<>(); 
		
		List<WebElement> headerElements = driver.findElements(By.xpath("//table//tr//th"));
		
		for(WebElement h : headerElements) {
			headers.add(h.getText());
		}
		
		data.add(headers);
			
		int rowSize = driver.findElements(By.xpath("//table//tbody//tr")).size();
		
		for(int i =1;i<=rowSize;i++) {
			List<WebElement> row = driver.findElements(By.xpath("//table//tbody//tr["+i+"]//td"));
			List<String> allData = new ArrayList<>();
			for(WebElement r1 : row) {
				allData.add(r1.getText());
			}
			data.add(allData);
		}
		generateCSVFile(data);
		
		driver.quit();
	}
}
```

EXPLANATION 

 To gets the current date and time, Formats it ans uses it to create a unique file name in the "CSVFiles" directory with the ".csv" Extension.

"FileWriter" to write the data to the CSV file.

Implicit waits tell WebDriver to pull the DOM for a certain amount of time when trying to locate elements.

Finally, it calls the "GenerateCSVFile" method, passing the "data" list as an argument to create the CSV file.

| Lorem     | Ipsum     | Dolor    | Sit       | Amet           | Diceret   | Action      |
|-----------|-----------|----------|-----------|----------------|-----------|-------------|
| Iuvaret0  | Apeirian0 | Adipisci0| Definiebas0| Consequuntur0  | Phaedrum0 | edit delete |
| Iuvaret1  | Apeirian1 | Adipisci1| Definiebas1| Consequuntur1  | Phaedrum1 | edit delete |
| Iuvaret2  | Apeirian2 | Adipisci2| Definiebas2| Consequuntur2  | Phaedrum2 | edit delete |
| Iuvaret3  | Apeirian3 | Adipisci3| Definiebas3| Consequuntur3  | Phaedrum3 | edit delete |
| Iuvaret4  | Apeirian4 | Adipisci4| Definiebas4| Consequuntur4  | Phaedrum4 | edit delete |
| Iuvaret5  | Apeirian5 | Adipisci5| Definiebas5| Consequuntur5  | Phaedrum5 | edit delete |
| Iuvaret6  | Apeirian6 | Adipisci6| Definiebas6| Consequuntur6  | Phaedrum6 | edit delete |
| Iuvaret7  | Apeirian7 | Adipisci7| Definiebas7| Consequuntur7  | Phaedrum7 | edit delete |
| Iuvaret8  | Apeirian8 | Adipisci8| Definiebas8| Consequuntur8  | Phaedrum8 | edit delete |
| Iuvaret9  | Apeirian9 | Adipisci9| Definiebas9| Consequuntur9  | Phaedrum9 | edit delete |

# Upload/Download attachments

```sh
package test;

import java.time.Duration;
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Set;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.edge.EdgeDriver;

public class Download_Upload {

	public static void main(String[] args) throws InterruptedException {

		WebDriver driver = new EdgeDriver();

		driver.get("https://the-internet.herokuapp.com/download");

		driver.manage().window().maximize();
		driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(15));

		List<WebElement> allFileLinks = driver.findElements(By.xpath("//div[@id='content']//div//child::a"));

		Set<String> files = new LinkedHashSet<>();
		
		for(WebElement link : allFileLinks) {

			String text = link.getText();
			if(!text.toLowerCase().contains("png")) {
				files.add(text);
				link.click();	
			}
		}
		
		String userDirectory = System.getProperty("user.dir");
		int usernameIndex = userDirectory.lastIndexOf(System.getProperty("user.name"));

		String desiredPath = userDirectory.substring(0, usernameIndex + System.getProperty("user.name").length());

		driver.get("https://the-internet.herokuapp.com/upload");
		
		
		
		try {
			for(String f: files) {
				WebElement upload = driver.findElement(By.xpath("(//input[@type='file'])[2]"));
				Thread.sleep(1000);
				upload.sendKeys(desiredPath+"\\Downloads\\"+f);
			}
		} catch (Exception e) {
		
		}
		
		Thread.sleep(2000);
		driver.quit();
		
	}
}

```

EXPLANATION 

It finds all the file download links on the Web page using an Xpath expression. These links are stored in the "allFileLinks" list of "webElement" objects.

It iterates through each element in "allFileLinks" to extract the text (file names) and check if they do not contain "png" (i.e., excluding PNG files)

If a file name is not a PNG file, it adds it to the "Files" set and clicks on the download link to initiate the download.

"desiredPath" is used for Extracting the path from the beginning of the current directory up to the user's name. once the downloaded files are stored , stop to construct the path.

For each file name 'f' in the set, it finds the file input element using an Xpath expression and assigns it to the 'upload' variable.

The above code is automates the process of downloading files from ONE WEB PAGE, stored in the specific directory and then uploading the files to ANOTHER WEB PAGE.

