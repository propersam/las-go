![](https://github.com/laslibs/las-go/workflows/Test/badge.svg?)


# las-go is a GoLang library/package for parsing .Las file (Geophysical well log files).

### Currently supports only version 2.0 of LAS Specification. For more information about this format, see the Canadian Well Logging Society [web page](http://www.cwls.org/las/)

## How to use

- Installing

  > GO GET

  ```sh
    go get github.com/laslibs/las-go
  ```
- Test

  ```sh
    go test ./...
  ```

- Usage

  ```go
  import (
    lasgo "github.com/iykekings/las-go"
  )
  ```

- Read data

  > Use `Las.Data()` to get a 2-dimensional slice containing the readings of each log

  ```go
      func main() {
      las, err := lasgo.Las("sample/A10.las")
      if err != nil {
        panic(err)
      }
      fmt.Println(las.Data())
    /**
       [[2650.0 177.825 -999.25 -999.25],
        [2650.5 182.5 -999.25-999.25]
        [2651.0180.162 -999.25 -999.25]
        [2651.5 177.825 -999.25 -999.25]
        [2652.0 177.825 -999.25 -999.25] ...]
      */
     }
  ```

- Get the log headers


    ```go
        // ...
        headers := las.Header();
        fmt.Println(headers);
        // [DEPTH GR NPHI RHOB]
        // ...
    ```

- Get the log headers descriptions as `map[string]string`


    ```go
        //...
        headerAndDesc := las.headerAndDesc()
        fmt.Println(headerAndDesc)
        // [DEPTH: DEPTH GR: Gamma Ray NPHI: Neutron Porosity RHOB: Bulk density]
        // ...
    ```

- Get a particular column, say Gamma Ray log


    ```go
        // ...
        gammaRay := las.Column("GR");
        fmt.Println(gammaRay);
        // [-999.25 -999.25 -999.25 -999.25 -999.25 122.03 123.14 ...]
        // ...
    ```

- Get the Well Parameters

  ### Presents a way of accessing the details of individual well parameters.

  ### The details include the following:

        1. description - Description/ Full name of the well parameter
        2. unit - Its unit of measurement
        3. value - Value measured

  ```go
    // ...
    well := las.WellParams()
    start := well["STRT"].value // 1670.0
    stop := well["STOP"].value // 1669.75
    null_value := well["NULL"].value //  -999.25
    // Any other well parameter present in the file, can be gotten with the same syntax above
    // ...
  ```

- Get the Curve Parameters

  ### Presents a way of accessing the details of individual log columns.

  ### The details include the following:

        1. description - Description/ Full name of the log column
        2. unit - Unit of the log column measurements
        3. value - API value of the log column

  ```go
    // ...
    curve := las.CurveParams()
    NPHI := curve["NPHI"].description // Neutron Porosity
    RHOB := curve["RHOB"].description // Bulk density
    // This is the same for all log column present in the file
    // ...
  ```

- Get the Parameters of the well

  ### The details include the following:

        1. description - Description/ Full name of the log column
        2. unit - Unit of the log column measurements
        3. value - API value of the log column

  ```go
    // ...
    param := await las.LogParams(); // BOTTOM HOLE TEMPERATURE
    BHT := param["BHT"].description // BOTTOM HOLE TEMPERATURE
    BHTValaue := param["BHT"].value // 35.5
    BHTUnits := param["BHT"].unit // DEGC
    // This is the same for all well parameters present in the file
    // ...
  ```

- Get the number of rows and columns


    ```go
        // ...
        numRows := las.RowCount() // 4
        numColumns := las.ColumnCount() // 3081
        // ...
    ```

- Get the version and wrap


    ```go
        // ...
        version := las.Version() // 2.0
        wrap := las.Wrap() // true
        // ...
    ```

- Get other information

  ```go
      // ...
      other := myLas.Other()
      fmt.Println(other)
      // Note: The logging tools became stuck at 625 metres causing the data between 625 metres and 615 metres to be invalid.
      // ...
  ```

- Export to CSV

  ### This writes a csv file to the current working directory, with headers of the well and data section

  ```go
      //...
      las.ToCSV("result")
      //...
  ```

  > result.csv

  | DEPT | RHOB    | GR      | NPHI  |
  | ---- | ------- | ------- | ----- |
  | 0.5  | -999.25 | -999.25 | -0.08 |
  | 1.0  | -999.25 | -999.25 | -0.08 |
  | 1.5  | -999.25 | -999.25 | -0.04 |
  | ...  | ...     | ...     | ...   |
  | 1.3  | -999.25 | -999.25 | -0.08 |
