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
        <version>3.10-FINAL</version>
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
* Steps for easy understand:
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
