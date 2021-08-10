# Work with Excel files
*Simple methods for working with exel files.*

```java

package bg.bulsi.fmfib.camunda.service;

import org.apache.poi.ss.usermodel.*;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.util.CellRangeAddress;
import org.apache.poi.ss.util.CellReference;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.springframework.stereotype.Service;

import java.io.*;
import java.util.*;

@Service
public class POIServiceImpl implements POIService {
//      Special characters for marking the row, which should be reading.
    private static final String SPECIAL_CHAR_FOR_MAIN_HEADER = "^";
    private static final String SPECIAL_CHAR_FOR_SECONDARY_HEADER = "`";

// Write a simple excel file
    @Override
    public String excelWrite(String fileName) {
        StringBuilder sb = new StringBuilder();

        XSSFWorkbook workbook = new XSSFWorkbook();
        XSSFSheet sheet = workbook.createSheet("Data types in JAVA");

        Object[][] dataTypes = {
                {"Datatype", "Type", "Size(in bytes)"},
                {"int", "Primitive", 2},
                {"float", "Primitive", 4},
                {"double", "Primitive", 8},
                {"char", "Primitive", 1},
                {"String", "Non-Primitive", "No fixed size"}
        };

        int rowNum = 0;

        sb.append("Creating excel");
        sb.append("\n");
//        System.out.println("\n\n Creating excel \n\n");

        for (Object[] datatype : dataTypes) {
            Row row = sheet.createRow(rowNum++);
            int colNum = 0;

            for (Object field : datatype) {
                Cell cell = row.createCell(colNum++);
                if (field instanceof String) {
                    cell.setCellValue((String) field);
                } else if (field instanceof Integer) {
                    cell.setCellValue((Integer) field);
                }
            }
        }

        try {
            FileOutputStream outputStream = new FileOutputStream(fileName);
            workbook.write(outputStream);
//            workbook.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        sb.append("Done");
        sb.append("\n");
//        System.out.println("\n\n Done \n\n");

        return sb.toString();
    }

// Read a data from cell with formula
    @Override
    public String readExcelFormula(String fileName) {
        StringBuilder sb = new StringBuilder();

        try {
            FileInputStream fis = new FileInputStream(fileName);

            //assuming xlsx file
            Workbook workbook = new XSSFWorkbook(fis);
            Sheet sheet = workbook.getSheetAt(0);

            for (Row row : sheet) {
                Iterator<Cell> cellIterator = row.cellIterator();

                while (cellIterator.hasNext()) {

                    Cell cell = cellIterator.next();
                    FormulaEvaluator evaluator = workbook
                            .getCreationHelper()
                            .createFormulaEvaluator();

                    if (cell.getCellType() == CellType.FORMULA) {
                        switch (cell.getCachedFormulaResultType()) {
                            case BOOLEAN:
                                sb.append("\n")
                                        .append("Cell Boolean Value = ")
                                        .append(cell.getBooleanCellValue())
                                        .append("\n");
                                break;
                            case NUMERIC:
                                sb.append("\n")
                                        .append("Cell Formula Result = ")
                                        .append(cell.getNumericCellValue())
                                        .append("\n");
                                break;
                            case STRING:
                                sb.append("\n")
                                        .append("Cell Formula Result = ")
                                        .append(cell.getStringCellValue())
                                        .append("\n");
                                break;
                        }
                    }

// Get all cells data (for each one):
                    switch (cell.getCellType()) {
                        case NUMERIC:
                            sb.append("\n")
                                    .append(cell.getNumericCellValue())
                                    .append("\n");
                            break;
                        case FORMULA:
                            sb.append("\n").append("\n")
                                    .append("Cell Formula = ")
                                    .append(cell.getCellFormula())
                                    .append("\n");

                            sb.append("\n")
                                    .append("Cell Formula Result Type = ")
                                    .append(cell.getCachedFormulaResultType())
                                    .append("\n");

                            Row rowContent = cell.getRow();
                            sb.append("\n")
                                    .append("Row content: ")
                                    .append("\n")
                                    .append(rowContent)
                                    .append("\n");

                            double resultFromFormula = cell.getNumericCellValue();
                            sb.append("\n")
                                    .append("Cell Formula Result = ")
                                    .append(resultFromFormula)
                                    .append("\n");

                        if (cell.getCachedFormulaResultType() == CellType.NUMERIC) {
                            sb.append("\n")
                                    .append("Formula NUMERIC Value = ")
                                    .append(cell.getNumericCellValue())
                                    .append("\n");
                        }
                    }
                }
            }
        } catch (Exception e) {
            sb.append("\n")
                    .append("EXCEPTION: ")
                    .append(e.getMessage())
                    .append("\n");
        }

        return sb.toString();
    }

// Write a formula
    @Override
    public String writeExcelFormula(String fileName) {
        StringBuilder sb = new StringBuilder();

        try {
            Workbook workbook = new XSSFWorkbook();
            Sheet sheet = workbook.createSheet("Numbers");
            Row row = sheet.createRow(0);
            row.createCell(0).setCellValue(20);
            row.createCell(1).setCellValue(20);
            row.createCell(2).setCellValue(30);
            //set formula cell
            row.createCell(3).setCellFormula("A1*B1*C1");

            //lets write to file
            FileOutputStream fos = new FileOutputStream(fileName);
            workbook.write(fos);
            fos.close();

            sb.append("\n")
                    .append(row.getCell(3))
                    .append("\n")
                    .append(fileName)
                    .append(" ")
                    .append("written successfully");

        } catch (Exception e) {
            sb.append("\n")
                    .append("EXCEPTION: ")
                    .append(e.getMessage())
                    .append("\n");
        }

        return sb.toString();
    }

    public String readMergedCells(String fileName) {
        StringBuilder sb = new StringBuilder();

        try {
            FileInputStream excelFile = new FileInputStream(new File(fileName));
            Workbook workbook = new XSSFWorkbook(excelFile);
            Sheet sheet = workbook.getSheetAt(0);

            for (int i = 0; i < sheet.getNumMergedRegions(); i++) {
                String a = sheet
                        .getMergedRegion(i)
                        .formatAsString()
                        .split("[:]")[0];
                sb.append("\n")
                        .append("sheet.getNumMergedRegions() = ")
                        .append(a)
                        .append("\n");

                CellReference cr = new CellReference(a);
                sb.append("\n")
                        .append("CR = ")
                        .append(cr.formatAsString())
                        .append("\n");

                CellRangeAddress mergesRegions = sheet.getMergedRegion(i);
                sb.append("\n")
                        .append("MR = ")
                        .append(mergesRegions)
                        .append("\n");

                Row row = sheet.getRow(cr.getRow());
                Cell cell = row.getCell(cr.getCol());
                sb.append("\n")
                        .append("Cell Value = ")
                        .append(cell.getStringCellValue())
                        .append("\n");
            }
        } catch (Exception e) {
            sb.append("\n")
                    .append("EXCEPTION: ")
                    .append(e.getMessage())
                    .append("\n");
        }

        return sb.toString();
    }

// Read data from excel sheets with start in row 12 and column 2
// Can be change with another indexes
    @Override
    public String excelRead(String fileName) {
        StringBuilder sb = new StringBuilder();
        Map<Integer, DtoSheetTitles> sheetsDtos = new HashMap<>();

        try {
            // Take a file
            FileInputStream excelFile = new FileInputStream(new File(fileName));
            // Create workbook from the file
            Workbook workbook = new XSSFWorkbook(excelFile);
            // Get the number of sheets in the workbook
            int numberOfSheets = workbook.getNumberOfSheets();

            // Go through all sheets
            for (int i = 0; i < numberOfSheets; i++) {

                // Get the sheet by index
                Sheet datatypeSheet = workbook.getSheetAt(i);
                
                // Get the sheet name
                String sheetName = datatypeSheet.getSheetName();

                // Get all merged regions from the sheet
                List<CellRangeAddress> mergedRegions = workbook
                        .getSheetAt(i)
                        .getMergedRegions();

                // Create DTO object for the titles in the sheet
                DtoSheetTitles dtoSheetTitles = new DtoSheetTitles();

                // Check is the sheet hidden
                boolean isHidden = workbook.isSheetHidden(i);

                if (!isHidden) {
                    dtoSheetTitles.setSheetName(sheetName);

                    // Create a map for the titles
                    Map<String, Map<String, ArrayList<String>>> titles = new HashMap<>();

                    // Go through the rows in current sheet
                    for (Row currentRow : datatypeSheet) {

                            // Go through the cells in the row
                            for (Cell currentCell : currentRow) {
                                int currentRowIndex = currentCell.getRowIndex();
                                // Check the index of the row is 12 "ex.: B12"
                            if (currentRowIndex == 11) {
                                // Check the cell
                                if (currentCell.getCellType() == CellType.STRING ||
                                        currentCell.getCellType() != CellType.BLANK  ||
                                        currentCell.getCellType() != CellType._NONE)
                                {
                                    // Get the value from the current cell
                                    String cellValue = currentCell.getStringCellValue();
                                    // Check is it empty
                                    if (!cellValue.isEmpty()) {
                                        // Add the name of the sheet


                                        // Call the method for getting the titles and sub titles of the current cell
                                        titles = this.getTitles(mergedRegions, cellValue, currentRow);

                                        // If there are titles, set them to the DTO
                                        if (titles.size() > 0) {
                                            dtoSheetTitles.setTitles(titles);
                                        }
                                    }
                                }
                            }
                        }
                    }

                    sheetsDtos.put(i, dtoSheetTitles);
                }
            }
        } catch (Exception e) {
            sb.append("\n")
                    .append("Exception: ")
                    .append(e.getMessage())
                    .append("\n");

            return  e.getMessage();
        }

        sb.append(this.printTitles(sheetsDtos));

        return sb.toString();
    }

// Print in the console
    private String printTitles(Map<Integer, DtoSheetTitles> sheetsDtos) {
        StringBuilder sb = new StringBuilder();

        int count = sheetsDtos.keySet().size();
        sb.append("\n")
                .append("DTOs: \n")
                .append("\n");

        for (int i = 0; i < count; i++) {
            DtoSheetTitles dto = sheetsDtos.get(i);

            if (dto != null){
                sb.append("Sheet name: ")
                        .append(dto.getSheetName())
                        .append("\n");

                for (int k = 0; k < dto.getTitles().keySet().size(); k++) {
                    String key = dto.getTitles()
                            .keySet()
                            .toArray()[k]
                            .toString();

                    sb.append("\n")
//                            .append("\n MAIN TITLE: ")
                            .append(key)
                            .append("\n");

                    Map<String, ArrayList<String>> subtitles = dto.getTitles().get(key);

                    if (subtitles.size() > 0) {
//                        sb.append("\n - SUB TITLE: \n");
                        for (var st : subtitles.keySet()) {
                            if (!st.isEmpty()) {
                                sb.append(" -- ").append(st).append("\n");
                            }
                        }
                    }
                    sb.append("\n");
                }
            }
        }

        return sb.toString();
    }

// Check for specila symbol for row with titles
    private Map<String, Map<String, ArrayList<String>>> getTitles (
            List<CellRangeAddress> mergedRegions,
            String cellValue,
            Row currentRow) {

        Map<String, Map<String, ArrayList<String>>> titles = new HashMap<>();

        // Check for special symbol about rows with titles and sub titles
        if (cellValue.contains(SPECIAL_CHAR_FOR_MAIN_HEADER) ||
                cellValue.contains(SPECIAL_CHAR_FOR_SECONDARY_HEADER)) {

            // Call the method for reading the row
            titles = this.readRow(mergedRegions, currentRow);
        }

        return titles;
    }

// Go throuth the row and get NOT empty cells and sub titles if the cell is merged
    private Map<String, Map<String, ArrayList<String>>> readRow(List<CellRangeAddress> mergedRegions, Row currentRow) {

        Map<String, Map<String, ArrayList<String>>> titles = new HashMap<>();
        String mainTitle = "";

        // Go through cell in the row
        for (Cell cell : currentRow) {
            // Should skip column A

            // Get the main title (ex.: from B12)
            if (cell.getColumnIndex() != 0 &&
                cell.getRowIndex() == 11 &&
                !cell.getStringCellValue().isEmpty() &&
                !cell.getStringCellValue().equals("") &&
                !cell.getStringCellValue().equals(" ") &&
                cell.getCellType() == CellType.STRING) {

                mainTitle = cell.getStringCellValue();
                Map<String, ArrayList<String>> subTitles = this.getSubTitles(mergedRegions, cell);
                titles.put(mainTitle, subTitles);
            }
        }

        return titles;
    }

    // Get the lower cell and checks is it merged
    // If it isn't, add to subtitles
    private Map<String, ArrayList<String>> getSubTitles(List<CellRangeAddress> mergedRegions, Cell cell) {

        // Checks the cells in the column for formula
        Iterator<Row> rows = cell.getSheet().iterator();
        boolean hasFormula = this.hasFormula(rows, cell);
        Map<String, ArrayList<String>> titles = new HashMap<>();
        ArrayList<String> subTitles = new ArrayList<>();

        String title = "";

        // To check is the formula more than 3 times
//        if (!hasFormula) {

            // Check is current cell is merged
            boolean isCurrentCellMerged = isMerged(cell);

            if (isCurrentCellMerged) {

                String cellLastMergedAddress = this.getMergedLastAddress(
                        mergedRegions,
                        cell.getAddress()
                                .formatAsString());

                CellReference lastCellRef = new CellReference(cellLastMergedAddress);
                Row lastCellRow = cell.getSheet().getRow(lastCellRef.getRow());
                Cell lastCellFormRef = lastCellRow.getCell(lastCellRef.getCol());

                int lastCellIndex = lastCellFormRef.getColumnIndex();
                int lastRowIndex = lastCellFormRef.getRowIndex();
                int currentCellIndex = cell.getColumnIndex();

                CellReference lastMergedCellRef = new CellReference(cellLastMergedAddress);
                Row lastMergedCellLastCellRow = cell.getSheet().getRow(lastMergedCellRef.getRow());
                Cell lastMerged = lastMergedCellLastCellRow.getCell(lastMergedCellRef.getCol());

                int lastMergedCellIndex = lastMerged.getColumnIndex();
                int cellIndex = cell.getColumnIndex();
                int mergedColumnsCount = lastMergedCellIndex - cellIndex;
                int count = cellIndex + mergedColumnsCount;

                for (int i = cellIndex; i <= count; i++) {
                    Cell c = cell.getRow().getCell(i);
                    Cell nextRowCell = this.getNextRowCell(c);

                    if (nextRowCell != null &&
                            nextRowCell.getCellType() != CellType._NONE &&
                            nextRowCell.getCellType() != CellType.BLANK &&
                            !nextRowCell.getStringCellValue().equals("") &&
                            !nextRowCell.getStringCellValue().equals(" ")){

                        title = nextRowCell.getStringCellValue();
                        boolean isNextCellMerged = this.isMerged(nextRowCell);

                        if (isNextCellMerged && lastRowIndex < 13) {
                            String nextRowCellLastMergedAddress = this.getMergedLastAddress(
                                    mergedRegions,
                                    nextRowCell.getAddress().formatAsString());

                            var subCellRef = new CellReference(nextRowCellLastMergedAddress);
                            Row nextRowCellLastCellRow = cell.getSheet().getRow(subCellRef.getRow());
                            Cell nextRowCellLastCellFormRef = nextRowCellLastCellRow.getCell(subCellRef.getCol());
                            int nextRowCellLastCellIndex = nextRowCellLastCellFormRef.getColumnIndex();

                            subTitles = this.getRowValues(nextRowCell, nextRowCellLastCellIndex);

                            for (String t : subTitles) {
                                title = "- " + t;
                            }
                        }

                        titles.put(title, subTitles);
                    }
                }
            }

            titles.put(title, subTitles);
//        }

        return titles;
    }

    private ArrayList<String> getRowValues(Cell nextRowCell, int lastCellIndex) {

        ArrayList<String> subTitles = new ArrayList<>();
        Cell subCell = this.getNextRowCell(nextRowCell);

        if (subCell != null) {
            Row subCellRow = subCell.getRow();
            Iterator<Cell> subCellIterator = subCellRow.cellIterator();
            int subCellRowIndex = subCell.getRowIndex();

            while (subCellIterator.hasNext()) {

                Cell newCell = subCellIterator.next();

                if (newCell.getColumnIndex() != 0 &&
                    subCellRowIndex <= lastCellIndex &&
                    !newCell.getStringCellValue().isEmpty() &&
                    !newCell.getStringCellValue().equals("")) {

                    String subTitle = newCell.getStringCellValue();
                    subTitles.add(subTitle);
                }
            }
        }

        return subTitles;
    }

    private Cell getNextRowCell(Cell cell) {
        try {
            int currentColumnIndex = cell.getColumnIndex();
            int currentRowIndex = cell.getRowIndex();
            int nextCellRowIndex = currentRowIndex + 1;

            // Get the cell from next row and same column
            CellReference nextRowCellRef = new CellReference(nextCellRowIndex, currentColumnIndex);

            CellReference ref = new CellReference(nextRowCellRef.formatAsString());
            Row nextCellRow = cell.getSheet().getRow(ref.getRow());
            Cell cellFormRef = nextCellRow.getCell(ref.getCol());

            return  cellFormRef;
        } catch (Exception e) {
            System.out.println("\n" + "ERROR: " + e.getMessage() + "\n");
            return null;
        }
    }

// Check is the cell merged
    private boolean isMerged(Cell cell) {

        Sheet sheet = cell.getSheet();
        int numberOfMergedRegions = sheet.getNumMergedRegions();

        for (int i = 0; i < numberOfMergedRegions; i++) {
            CellRangeAddress mergedCell = sheet.getMergedRegion(i);

            if (mergedCell.isInRange(cell.getRowIndex(), cell.getColumnIndex())) {

                return true;
            }
        }

        return false;
    }

// Check is there a fromula in cell's column
    private boolean hasFormula(Iterator<Row> rows, Cell cell) {
        int counter = 0;

        while (rows.hasNext()) {

            Row rowForCheck = rows.next();
            Iterator<Cell> cellsForCheck = rowForCheck.cellIterator();

            while (cellsForCheck.hasNext()) {
                Cell cellForCheck = cellsForCheck.next();

                if (cellForCheck.getColumnIndex() == cell.getColumnIndex() &&
                        cellForCheck.getCellType() == CellType.FORMULA) {
                    counter++;
                }
            }
        }

        return counter >= 3;
    }

// Get last address from merged cells
    private String getMergedLastAddress(List<CellRangeAddress> mergedRegions, String cellAddress) {

        String lastAddress = "";

        // Go through all merged regions from the sheet
        for (CellRangeAddress region : mergedRegions ) {
            boolean isContains = region
                    .formatAsString()
                    .contains(cellAddress);

            // Check is the address of the cell is in merged list
            // and get the address of the last cell in the range,
            // it will be needed when taking the values
            if (isContains) {
                lastAddress = region.formatAsString().split("[:]")[1];

               return lastAddress;
            }
        }

        return lastAddress;
    }
}

class DtoSheetTitles {
    private String SheetName;
    private Map<String, Map<String, ArrayList<String>>> titles;
    private String valueRangeUpperLeft;
    private String valueRangeBottomRight;

    DtoSheetTitles() {
        this.titles = new HashMap<String, Map<String, ArrayList<String>>>();
    }

    public String getSheetName() {
        return SheetName;
    }

    public void setSheetName(String sheetName) {
        SheetName = sheetName;
    }

    public Map<String, Map<String, ArrayList<String>>> getTitles() {
        return titles;
    }

    public void setTitles(Map<String, Map<String, ArrayList<String>>> titles) {
        this.titles = titles;
    }

    public String getValueRangeUpperLeft() {
        return valueRangeUpperLeft;
    }

    public void setValueRangeUpperLeft(String valueRangeUpperLeft) {
        this.valueRangeUpperLeft = valueRangeUpperLeft;
    }

    public String getValueRangeBottomRight() {
        return valueRangeBottomRight;
    }

    public void setValueRangeBottomRight(String valueRangeBottomRight) {
        this.valueRangeBottomRight = valueRangeBottomRight;
    }
}
```
