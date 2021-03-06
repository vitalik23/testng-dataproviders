###  TestNg-DataProviders [![BuildStatus](https://travis-ci.org/sergueik/testng-dataproviders.svg?branch=master)](https://travis-ci.org/sergueik/testng-dataproviders.svg?branch=maste://travis-ci.org/sergueik/testng-dataproviders.svg?branch=master)

This project exercises [testng dataProviders](http://testng.org/doc/documentation-main.html#parameters-dataproviders)

  * Excel 2003 OLE documents - Horrible SpreadSheet Format [org.apache.poi.hssf.usermodel.*)](http://shanmugavelc.blogspot.com/2011/08/apache-poi-read-excel-for-use-of.html)
  * Excel 2007 OOXML (.xlsx) - XML SpreadSheet Format [org.apache.poi.xssf.usermodel.*](http://howtodoinjava.com/2013/06/19/readingwriting-excel-files-in-java-poi-tutorial/)
  * OpenOffice SpreadSheet (.ods) [example1](http://www.programcreek.com/java-api-examples/index.php?api=org.jopendocument.dom.spreadsheet.Sheet) ,[example 2]http://half-wit4u.blogspot.com/2011/05/read-openoffice-spreadsheet-ods.html
  * Custom JSON [org.json.JSON](http://www.docjar.com/docs/api/org/json/JSONObject.html)
  * csv [testnt csv file](http://stackoverflow.com/questions/26033985/how-to-pass-parameter-to-data-provider-in-testng-from-csv-file)
  * fillo [fillo](http://codoid.com/fillo/)

### Testing

For example test case performs Selenium link count test with the data providers of the following supported data types:

* Excel 2003
* Excel 2007
* Open Office Spreadsheet
* JSON

The test inputs are defined as spreadsheet with columns

| ROWNUM |  SEARCH | COUNT |
|--------|---------|-------|
| 1      | junit   | 100   |

or a JSON file with the following structure:
```javascript
{
    "test": [{
        "keyword": "junit",
        "count": 101.0,
        "comment": "",
        "other unused column": "",
    }, {
        "comment": "",
        "keyword": "testng",
        "count": 31.0
    },
...
]
}
```

which represent the test `id`, the *seach term* and *expected minimum count* of articles found on the forum through title search.

The following annotations are provided to the test methods:

```java
@Test(enabled = true, singleThreaded = false, threadPoolSize = 1, invocationCount = 1, description = "searches publications for a keyword", dataProvider = "Excel 2003")
@DataFileParameters(name = "data_2003.xls", path = "${USERPROFILE}\\Desktop", sheetName = "Employee Data")
	public void test_with_Excel_2003(double rowNum, String search_keyword,
			double expected_count) throws InterruptedException {
		parseSearchResult(search_keyword, expected_count);
	}
```
or
```java
@Test(enabled = true, singleThreaded = false, threadPoolSize = 1, invocationCount = 1, description = "searches publications for a keyword", dataProvider = "Excel 2007")
@DataFileParameters(name = "data_2007.xlsx", path = ".")
	public void test_with_Excel_2007(double rowNum, String search_keyword,
			double expected_count) throws InterruptedException {
		parseSearchResult(search_keyword, expected_count);
	}
```
or
```java
@Test(enabled = true, singleThreaded = false, threadPoolSize = 1, invocationCount = 1, description = "searches publications for a keyword", dataProvider = "OpenOffice Spreadsheet")
@DataFileParameters(name = "data.ods", path = "src/main/resources") // when datafile path is relative assume it is under ${user.dir}
	public void test_with_OpenOffice_Spreadsheet(double rowNum,
			String search_keyword, double expected_count)
			throws InterruptedException {
		parseSearchResult(search_keyword, expected_count);
	}
```
or
```java
@Test(enabled = true, singleThreaded = false, threadPoolSize = 1, invocationCount = 1, description = "searches publications for a keyword", dataProvider = "JSON")
@JSONDataFileParameters(name = "data.json", dataKey = "test", columns = "keyword,count"
  // one need to list value columns explicitly with JSON due to the way org.json.JSONObject is implemented
	public void test_with_JSON(String search_keyword, double expected_count)
			throws InterruptedException {
		parseSearchResult(search_keyword, expected_count);
	}
```
The data provider class will load all columns from Excel 2003, Excel 2007 or OpenOffice spreadsheet respectively and columns defined for JSON data provider
and run test method with every row of data. It is up to the test developer to make the test method consume the correct number and type or parameters as the columns
in the spreadsheet.

To enable debug messages during the data loading, set the `debug` flag with `@DataFileParameters` attribute:
```java
	@Test(enabled = true, singleThreaded = true, threadPoolSize = 1, invocationCount = 1, description = "# of articless for specific keyword", dataProvider = "Excel 2007", dataProviderClass = ExcelParametersProvider.class)
	@DataFileParameters(name = "data_2007.xlsx", path = ".", sheetName = "Employee Data", debug = true)
	public void test_with_Excel_2007(double rowNum, String searchKeyword,
			double expectedCount) throws InterruptedException {
		dataTest(searchKeyword, expectedCount);
	}
```

this will show the following:
```shell
Data Provider Caller Suite: Suite 1
Data Provider Caller Test: Parse Search Result
Data Provider Caller Method: test_with_Excel_2007
0 = A ID
1 = B SEARCH
2 = C COUNT
Cell Value: 1.0 class java.lang.Double
Cell Value: junit class java.lang.String
Cell Value: 104.0 class java.lang.Double
...
row 0 : [1.0, junit, 104.0]
...```


### Links

 * [MySQL testng dataprovider](https://github.com/sskorol/selenium-camp-samples/tree/master/mysql-data-provider)
 * [xml testng DataProviders](http://testngtricks.blogspot.com/2013/05/how-to-provide-data-to-dataproviders.html)
 * [javarticles.com](http://javarticles.com/2015/03/example-of-testng-dataprovider.html)
 * [testng-users forum](https://groups.google.com/forum/#!topic/testng-users/J437qa5PSx8)
 * [passing parameters to provider via Method](http://stackoverflow.com/questions/666477/possible-to-pass-parameters-to-testng-dataprovider)
 * [JUnitParams](https://github.com/Pragmatists/JUnitParams) - TestNg-style `JUnitParamsRunner` and `ParametersProvider` classes.
 * [testng samples](https://habrahabr.ru/post/121234/)
 * [barancev/testng_samples](https://github.com/barancev/testng_samples)
 * [ahussan/DataDriven](https://github.com/ahussan/DataDriven)
 * [poi ppt](https://www.tutorialspoint.com/apache_poi_ppt/apache_poi_ppt_quick_guide.htm)
 * [paypal/SeLion data providers](https://github.com/paypal/SeLion/tree/develop/dataproviders/src/main/java/com/paypal/selion/platform/dataprovider)
 * [RestAPIFramework-TestNG/.../ExcelLibrary](https://github.com/hemanthsridhar/RestAPIFramework-TestNG/blob/master/src/main/java/org/framework/utils/ExcelLibrary.java)
 * [GladsonAntony/WebAutomation_Allure ExcelUtils.java](https://github.com/GladsonAntony/WebAutomation_Allure/blob/master/src/main/java/utils/ExcelUtils.java)
 * [sskorol/tesst-data-supplier](https://github.com/sskorol/test-data-supplier)
 * [converting gradle to pom](https://stackoverflow.com/questions/12888490/gradle-build-gradle-to-maven-pom-xml)
 * [using gradle maven plugin to produce pom.xml](https://stackoverflow.com/questions/17281927/how-to-make-gradle-generate-a-valid-pom-xml-file-at-the-root-of-a-project-for-ma)

### Maven Central

The snapshot versions are deployed to `https://oss.sonatype.org/content/repositories/snapshots/com/github/sergueik/dataprovider/`
The release versions status: [Release pending](https://issues.sonatype.org/browse/OSSRH-36773?page=com.atlassian.jira.plugin.system.issuetabpanels:all-tabpanel)

To use the snapshot version, add the following to `pom.xml`:
```xml
<dependency>
  <groupId>com.github.sergueik.testng</groupId>
  <artifactId>dataprovider</artifactId>
  <version>1.3-SNAPSHOT</version>
</dependency>
<repositories>
  <repository>
    <id>ossrh</id>
    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
  </repository>
</repositories>
```

### Author
[Serguei Kouzmine](kouzmine_serguei@yahoo.com)
