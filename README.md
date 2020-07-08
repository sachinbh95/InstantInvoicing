# InstantInvoicing
Selenium script for Instant Invoicing of the tracking numbers

public class Invoicing {

	public static void main(String[] args) throws Exception {
		
		System.setProperty("webdriver.chrome.driver","D:\\SOFTWARES\\Drivers\\chromedriver.exe");
		WebDriver driver = new ChromeDriver();
		driver.manage().window().maximize();
	        driver.get("https://testsso.secure.fedex.com/l2/instant-invoicing/");
		driver.findElement(By.xpath("//*[@id='username']")).sendKeys(Keys.chord(Keys.CONTROL,Keys.SUBTRACT));
		driver.findElement(By.xpath("//*[@id='username']")).sendKeys("804218");
		driver.findElement(By.xpath("//*[@id='password']")).sendKeys("804218");
		driver.findElement(By.xpath("//*[@id='submit']")).click();
		driver.manage().timeouts().implicitlyWait(40, TimeUnit.SECONDS);
		

		File src=new File("D:\\SOFTWARES\\InstantInvoicing\\shipments.xlsx");
		FileInputStream fs=new FileInputStream(src);
		XSSFWorkbook wb=new XSSFWorkbook(fs);
		XSSFSheet sheet1= wb.getSheet("Sheet1");
		Cell c=null;
		int acctnbr=0,trknngr=0;
		String acct,trkn;
		int row= sheet1.getLastRowNum();
		for(int i=1;i<= row;i++ )
		{
			acctnbr=(int) sheet1.getRow(i).getCell(0).getNumericCellValue();
			acct=Integer.toString(acctnbr);
			trkn=sheet1.getRow(i).getCell(1).getRawValue();
			//trkn=Integer.toString(trknngr);
		
		driver.switchTo().frame("content");
		driver.findElement(By.xpath("//*[@id='iiForm:account_input']")).sendKeys(acct);
		driver.findElement(By.xpath("//*[@id='iiForm:trkno_input']")).sendKeys(trkn);
		System.out.println(trkn);
		driver.findElement(By.xpath("//*[@id='iiForm:search_button']")).click();
		driver.manage().timeouts().implicitlyWait(40, TimeUnit.SECONDS);
		driver.findElement(By.xpath("//*[@id='iiForm:instantInvoiceDynTable:instantInvoiceTable:0:_t47']")).click();
		driver.manage().timeouts().implicitlyWait(40, TimeUnit.SECONDS);
		driver.findElement(By.xpath("//*[@id='iiForm:instanceInvoice_button']")).click();
		//*[@id='iiForm:instanceInvoice_button']
		//driver.switchTo().defaultContent();
		driver.navigate().refresh();
		//("//*[@id='iiForm:instantInvoiceDynTable:instantInvoiceTable:0:_t47']")
		}		
		driver.close();
		
		

	}

}
