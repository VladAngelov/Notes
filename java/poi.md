# Apache POI
*The Java API for Microsoft Documents*

Apache POI provides excellent support for working with Microsoft Excel documents. 
Apache POI is able to handle both XLS and XLSX formats of spreadsheets.

Some important points about Apache POI API are:
1. Apache POI contains HSSF implementation for Excel ’97(-2007) file format i.e XLS.
2. Apache POI XSSF implementation should be used for Excel 2007 OOXML (.xlsx) file format.
3. Apache POI HSSF and XSSF API provides mechanisms to read, write or modify excel spreadsheets.
4. Apache POI also provides SXSSF API that is an extension of XSSF to work with very large excel sheets. SXSSF API requires less memory and is suitable when working with very large spreadsheets and heap memory is limited.
5. There are two models to choose from – event model and user model. Event model requires less memory because the excel file is read in tokens and requires processing them. User model is more object oriented and easy to use and we will use this in our examples.
6. Apache POI provides excellent support for additional excel features such as working with Formulas, creating cell styles by filling colors and borders, fonts, headers and footers, data validations, images, hyperlinks etc.

## Apache POI Maven Dependencies

```java
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>4.1.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>3.10-FINAL</version>
    </dependency>
```


## Mission Statement
The Apache POI Project's mission is to create and maintain Java APIs for manipulating various file formats based upon the Office Open XML standards (OOXML) and Microsoft's OLE 2 Compound Document format (OLE2). 
You can read and write MS Excel files using Java. 
In addition, you can read and write MS Word and MS PowerPoint files using Java. 
Apache POI is your Java Excel solution (for Excel 97-2008). 
We have a complete API for porting other OOXML and OLE2 formats and welcome others to participate. 

OLE2 files include most Microsoft Office files such as XLS, DOC, and PPT as well as MFC serialization API based file formats. 
The project provides APIs for the OLE2 Filesystem (POIFS) and OLE2 Document Properties (HPSF). 

Office OpenXML Format is the new standards based XML file format found in Microsoft Office 2007 and 2008. 
This includes XLSX, DOCX and PPTX. The project provides a low level API to support the Open Packaging Conventions using openxml4j. 

For each MS Office application there exists a component module that attempts to provide a common high level Java api to both OLE2 and OOXML document formats. 
This is most developed for Excel workbooks *(SS=HSSF+XSSF)*. 
Work is progressing for Word documents *(WP=HWPF+XWPF)* and PowerPoint presentations *(SL=HSLF+XSLF)*. 

 The project has some support for Outlook *(HSMF)*. 
 Microsoft opened the specifications to this format in October 2007.

 There are also projects for Visio *(HDGF and XDGF)*, TNEF *(HMEF)*, and Publisher *(HPBF)*.

---

* Components
    * *https://poi.apache.org/components/*

* Project News
    * *https://poi.apache.org/index.html*

* Apache POI Examples:
    * Read Exel File
    * Read Excel Formula
    * Excel Write Formula
        * *https://www.journaldev.com/2562/apache-poi-tutorial* 
    * *https://mkyong.com/java/apache-poi-reading-and-writing-excel-file-in-java/*

* Busy Developers' Guide to HSSF and XSSF Features:
    * *http://poi.apache.org/components/spreadsheet/quick-guide.html*


## Read Excel File
Steps for easy understand:
1. Create **Workbook** *instance based on the file type*
    1. **XSSFWorkbook** for *xlsx* format
    2. **HSSFWorkbook** for *xls*  format   
2. Use workbook *getNumberOfSheets()* to get the number of sheets and then use for loop to parse each of the sheets
    1. Get the **Sheet** instance using *getSheetAt(int i)* method
3. Get **Row** iterator and then **Cell** iterator to get the Cell object ( *Apache POI is using iterator pattern (https://www.journaldev.com/1716/iterator-design-pattern-java)* )
4. Use switch-case to read the type of Cell and the process it accordingly

## Write Excel File
Writing excel file in apache POI is similar to reading, **except that here we first create the workbook**. 
Then set sheets, rows and cells values and use *FileOutputStream* to write it to file. 

---

# Links for java libraries working with ODF and Excel files

### Merge Cells

* addMergedRegion(**CellRangeAddress** region)
    * Adds a merged region of cells (hence those cells form one)
    * Parameters:
        * region - (rowfrom/colfrom-rowto/colto) to merge
    * Returns:
        * index of this region

* CellRangeAddress
    * *https://poi.apache.org/apidocs/dev/org/apache/poi/ss/util/CellRangeAddress.html*

* There are two ways to set the cell range:
    1. use four zero-based indexes to define the top-left cell location and the bottom-right cell location:
       ```java
            sheet = // existing Sheet setup
            int firstRow = 0;
            int lastRow = 0;
            int firstCol = 0;
            int lastCol = 2
            sheet.addMergedRegion(new CellRangeAddress(firstRow, lastRow, firstCol, lastCol));
        ``` 
    2. use a cell range reference string to provide the merged region
        ```java
            sheet = // existing Sheet setup
            sheet.addMergedRegion(CellRangeAddress.valueOf("A1:C1"));
        ``` 

*If cells have data before we merge them, Excel will use the top-left cell value as the merged region value. For the other cells, Excel will discard their data.*

Example on how we can merge cells in excel file:
```java
    import java.io.File;
    import java.io.FileOutputStream;
    
    import org.apache.poi.ss.usermodel.Cell;
    import org.apache.poi.ss.usermodel.Row;
    import org.apache.poi.ss.util.CellRangeAddress;
    import org.apache.poi.xssf.usermodel.XSSFSheet;
    import org.apache.poi.xssf.usermodel.XSSFWorkbook;
    
    public class ExcelCellMergeExample {
        public static void main(String[] args) {
    
            // Create instance of the class and call the writeExcelFile()
            ExcelCellMergeExample mergeExample = new ExcelCellMergeExample();
            mergeExample.writeExcelFile("C:\\kscodes_temp\\SplitPaneText.xlsx");
        }
    
        public void writeExcelFile(String fileName) {
            XSSFWorkbook workbook = null;
            FileOutputStream fileOutputStream = null;
            try {
    
                fileOutputStream = new FileOutputStream(new File(fileName));
    
                // Create a Workbook
                workbook = new XSSFWorkbook();
    
                // Create an Empty Sheet
                XSSFSheet sheet = workbook.createSheet("Cell Merge Test");
    
                // Add a row and cell and some text in it.
                Row row = sheet.createRow((short) 3);
                Cell cell = row.createCell((short) 3);
                cell.setCellValue("This is a test of Cell Merging Using Apache POI in Java");
    
                // Create a cellRangeAddress to select a range to merge.
                CellRangeAddress cellRangeAddress = new CellRangeAddress(3, 5, 3, 8);
    
                // Merge the selected cells.
                sheet.addMergedRegion(cellRangeAddress);
    
                workbook.write(fileOutputStream);
    
                System.out.println("Excel File created and written !!!!");
    
            } catch (Exception e) {
                System.out.println("An Exception occured while writing Excel");
            } finally {
                try {
                    if (fileOutputStream != null) {
                        fileOutputStream.close();
                    }
                    if (workbook != null) {
                        workbook.close();
                    }
                } catch (Exception e) {
                }
            }
        }
    
    }
```

```java
    public void MergingCellExample {  
        try (OutputStream fileOut = new FileOutputStream("Javatpoint.xls")) {  
            Workbook wb = new HSSFWorkbook();  
            Sheet sheet = wb.createSheet("Sheet");    
            Row row = sheet.createRow(1);  
            Cell cell = row.createCell(1);  
            cell.setCellValue("Two cells have merged");  
              //Merging cells by providing cell index  
            sheet.addMergedRegion(new CellRangeAddress(1,1,1,2));  
            wb.write(fileOut);  
        }catch(Exception e) {  
            System.out.println(e.getMessage());  
        }  
    }  
```

```java
    public void readMergedCells() {

        FileInputStream file = null;
        Boolean flag = true;
        ArrayList<String> rows = new ArrayList<String>();

        try {

            // here uploadFolder contains the path to the Login 3.xlsx file
            file = new FileInputStream(new File(uploadFolder + "Login 3.xlsx"));

            //Create Workbook instance holding reference to .xlsx file
            XSSFWorkbook workbook = new XSSFWorkbook(file);

            //Get first/desired sheet from the workbook
            XSSFSheet sheet = workbook.getSheetAt(0);

            //Iterate through each rows one by one
            Iterator<Row> rowIterator = sheet.iterator();

            while (rowIterator.hasNext()) {
                Row row = rowIterator.next();

                //For each row, iterate through all the columns
                Iterator<Cell> cellIterator = row.cellIterator();

                outer:
                while (cellIterator.hasNext()) {
                    Cell cell = cellIterator.next();

                    //will iterate over the Merged cells
                    for (int i = 0; i < sheet.getNumMergedRegions(); i++) {
                        
                        CellRangeAddress region = sheet.getMergedRegion(i); //Region of merged cells
                        int colIndex = region.getFirstColumn(); //number of columns merged
                        int rowNum = region.getFirstRow();      //number of rows merged

                        //check first cell of the region
                        if (rowNum == cell.getRowIndex() && 
                            colIndex == cell.getColumnIndex()) {

                            System.out.println(
                                sheet
                                .getRow(rowNum)
                                .getCell(colIndex)
                                .getStringCellValue());

                            continue outer;
                        }
                    }

                    //the data in merge cells is always present on the first cell. All other cells(in merged region) are considered blank
                    if (cell.getCellType() == Cell.CELL_TYPE_BLANK || 
                        cell == null) {
                        continue;
                    }
                    
                    System.out.println(cell.getStringCellValue());
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } 
    }
```


## Convert ODS to XLSX via Java

*https://products.aspose.com/cells/java/conversion/ods-to-xlsx/*

Превръщане на ODS към XLSX, чрез Aspose.Cell 

```java
    <repository>
    <id>AsposeJavaAPI</id>
    <name>Aspose Java API</name>
    <url>https://repository.aspose.com/repo/</url>
    </repository>
```

```java
    <dependency>
    <groupId>com.aspose</groupId>
    <artifactId>aspose-cells</artifactId>
    <version>version of aspose-cells API</version>
    <classifier>jdk17</classifier>
    </dependency>
```

* Steps to Convert ODS to XLSX via Java
    1. Load ODS file with an instance of Workbook class
    2. Call Workbook.save method
    3. Pass output path with XLSX extension & SaveFormat as parameters
    4. Check specified path for resultant XLSX file

```java 
    // load the ODS file in an instance of Workbook
    Workbook book = new Workbook("template.ods");
    // save ODS as XLSX
    book.save("output.xlsx", SaveFormat.AUTO);
```


## Apache OpenOffice

*https://blogs.apache.org/OOo/entry/announcing-apache-openoffice-4-16*

### Announcing Apache OpenOffice 4.1.10

Apache OpenOffice, a leading Open Source office document productivity suite, announced today Apache OpenOffice 4.1.10, as usual available in 41 languages for Windows, macOS and Linux.

*https://www.openoffice.org/download/*

