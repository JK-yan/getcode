# getcode
搬运来的东西
```
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import java.io.File;
import java.io.FileInputStream;
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * Created by Administrator on 2016/5/31.
 */
public class ExcelUtil {


    //读取Excel中的数据

    public static list<Param> read(String filename, String filesyyle )throws Exception{

        try {
            //获取excel所在路径
            File classpathRoot= new File(System.getProperty("user.dir"));
            FileInputStream fis = new FileInputStream(classpathRoot+filename+filesyyle);
            //读取excel文件
            Workbook book =null;
            if(filesyyle.equals("xls")){
                book = new HSSFWorkbook(fis);

            }else if(filesyyle.equals("xlsx")){

                book = new XSSFWorkbook(fis);
            }

            Sheet sheet = book.getSheetAt(0);

            Iterator<Row> itr = sheet.iterator();

            System.out.println(itr.hasNext());
            List<Param> Params=new ArrayList<Param>();
            // Iterating over Excel file in Java
            while (itr.hasNext()) {
                Row row = itr.next();
                System.out.println(row.getLastCellNum());
                // Iterating over each column of Excel file
                Iterator<Cell> cellIterator = row.cellIterator();
                while (cellIterator.hasNext()) {

                    Cell cell = cellIterator.next();

                    switch (cell.getCellType()) {
                        case Cell.CELL_TYPE_STRING:
                            System.out.print(cell.getStringCellValue() + "\t");
                            break;
                        case Cell.CELL_TYPE_NUMERIC:
                            if(DateUtil.isCellDateFormatted(cell)){
                                System.out.print(cell.getDateCellValue().toString() + "\t");
                            }else{
                                //去除科学计数法问题
                                DecimalFormat df = new DecimalFormat("0");
                                String whatYourWant = df.format(cell.getNumericCellValue());
                                System.out.print(whatYourWant + "\t");
                            }

                            break;
                        case Cell.CELL_TYPE_BOOLEAN:
                            System.out.print(cell.getBooleanCellValue() + "\t");
                            break;
                        default:

                    }
                }
                //  System.out.println("");
            }

        }catch(Exception ex){
            ex.printStackTrace();
        }
          return  Params;

    }
}

```
