# Project Database

### Armaan Shamsaasef
### Date: 12/14/2025
### CISC191 Week 15 Assignments


# Here is the flowchart!

<img width="2601" height="1925" alt="image" src="https://github.com/user-attachments/assets/91680ed6-b9b7-41b3-bc9e-59b7cf651ba7" />



## Challenges

### Technical/Setup Challenges:
#### MySQL Installation & Path Configuration

#### JDBC Driver Issues

#### Database Connection Problems


### Data Processing Challenges:


#### Data Parsing & Insertion
#### Data Format Complexities
###  SQL & Query Challenges:
#### SQL Syntax Errors
#### Query Interface Design


### Java Programming Challenges:

#### GUI Development Issues

#### Exception Handling

#### Data Display Challenges

### Integration Challenges:

#### Multi-component Integration

#### Development Environment Issues




## This is my code!

```.java

import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.BufferedReader;
import java.io.StringReader;
import java.sql.*;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class AutoMPGDatabase {
    // Database connection parameters
    private static final String DB_URL = "jdbc:mysql://localhost:3306/Auto";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "Armaan2017#";

    // Data string from the problem
    private static final String DATA = """
            18.0   8.   307.0      130.0      3504.      12.0   70.  1.	"chevrolet chevelle malibu"
            15.0   8.   350.0      165.0      3693.      11.5   70.  1.	"buick skylark 320"
            18.0   8.   318.0      150.0      3436.      11.0   70.  1.	"plymouth satellite"
            16.0   8.   304.0      150.0      3433.      12.0   70.  1.	"amc rebel sst"
            17.0   8.   302.0      140.0      3449.      10.5   70.  1.	"ford torino"
            15.0   8.   429.0      198.0      4341.      10.0   70.  1.	"ford galaxie 500"
            14.0   8.   454.0      220.0      4354.       9.0   70.  1.	"chevrolet impala"
            14.0   8.   440.0      215.0      4312.       8.5   70.  1.	"plymouth fury iii"
            14.0   8.   455.0      225.0      4425.      10.0   70.  1.	"pontiac catalina"
            15.0   8.   390.0      190.0      3850.       8.5   70.  1.	"amc ambassador dpl"
            NA     4.   133.0      115.0      3090.      17.5   70.  2.	"citroen ds-21 pallas"
            NA     8.   350.0      165.0      4142.      11.5   70.  1.	"chevrolet chevelle concours (sw)"
            NA     8.   351.0      153.0      4034.      11.0   70.  1.	"ford torino (sw)"
            NA     8.   383.0      175.0      4166.      10.5   70.  1.	"plymouth satellite (sw)"
            NA     8.   360.0      175.0      3850.      11.0   70.  1.	"amc rebel sst (sw)"
            15.0   8.   383.0      170.0      3563.      10.0   70.  1.	"dodge challenger se"
            14.0   8.   340.0      160.0      3609.       8.0   70.  1.	"plymouth 'cuda 340"
            NA     8.   302.0      140.0      3353.       8.0   70.  1.	"ford mustang boss 302"
            15.0   8.   400.0      150.0      3761.       9.5   70.  1.	"chevrolet monte carlo"
            14.0   8.   455.0      225.0      3086.      10.0   70.  1.	"buick estate wagon (sw)"
            24.0   4.   113.0      95.00      2372.      15.0   70.  3.	"toyota corona mark ii"
            22.0   6.   198.0      95.00      2833.      15.5   70.  1.	"plymouth duster"
            18.0   6.   199.0      97.00      2774.      15.5   70.  1.	"amc hornet"
            21.0   6.   200.0      85.00      2587.      16.0   70.  1.	"ford maverick"
            27.0   4.   97.00      88.00      2130.      14.5   70.  3.	"datsun pl510"
            26.0   4.   97.00      46.00      1835.      20.5   70.  2.	"volkswagen 1131 deluxe sedan"
            25.0   4.   110.0      87.00      2672.      17.5   70.  2.	"peugeot 504"
            24.0   4.   107.0      90.00      2430.      14.5   70.  2.	"audi 100 ls"
            25.0   4.   104.0      95.00      2375.      17.5   70.  2.	"saab 99e"
            26.0   4.   121.0      113.0      2234.      12.5   70.  2.	"bmw 2002"
            21.0   6.   199.0      90.00      2648.      15.0   70.  1.	"amc gremlin"
            10.0   8.   360.0      215.0      4615.      14.0   70.  1.	"ford f250"
            10.0   8.   307.0      200.0      4376.      15.0   70.  1.	"chevy c20"
            11.0   8.   318.0      210.0      4382.      13.5   70.  1.	"dodge d200"
            9.0   8.   304.0      193.0      4732.      18.5   70.  1.	"hi 1200d"
            27.0   4.   97.00      88.00      2130.      14.5   71.  3.	"datsun pl510"
            28.0   4.   140.0      90.00      2264.      15.5   71.  1.	"chevrolet vega 2300"
            25.0   4.   113.0      95.00      2228.      14.0   71.  3.	"toyota corona"
            25.0   4.   98.00      NA         2046.      19.0   71.  1.	"ford pinto"
            NA     4.   97.00      48.00      1978.      20.0   71.  2.	"volkswagen super beetle 117"
            19.0   6.   232.0      100.0      2634.      13.0   71.  1.	"amc gremlin"
            16.0   6.   225.0      105.0      3439.      15.5   71.  1.	"plymouth satellite custom"
            17.0   6.   250.0      100.0      3329.      15.5   71.  1.	"chevrolet chevelle malibu"
            19.0   6.   250.0      88.00      3302.      15.5   71.  1.	"ford torino 500"
            18.0   6.   232.0      100.0      3288.      15.5   71.  1.	"amc matador"
            14.0   8.   350.0      165.0      4209.      12.0   71.  1.	"chevrolet impala"
            14.0   8.   400.0      175.0      4464.      11.5   71.  1.	"pontiac catalina brougham"
            14.0   8.   351.0      153.0      4154.      13.5   71.  1.	"ford galaxie 500"
            14.0   8.   318.0      150.0      4096.      13.0   71.  1.	"plymouth fury iii"
            12.0   8.   383.0      180.0      4955.      11.5   71.  1.	"dodge monaco (sw)"
            13.0   8.   400.0      170.0      4746.      12.0   71.  1.	"ford country squire (sw)"
            13.0   8.   400.0      175.0      5140.      12.0   71.  1.	"pontiac safari (sw)"
            18.0   6.   258.0      110.0      2962.      13.5   71.  1.	"amc hornet sportabout (sw)"
            22.0   4.   140.0      72.00      2408.      19.0   71.  1.	"chevrolet vega (sw)"
            19.0   6.   250.0      100.0      3282.      15.0   71.  1.	"pontiac firebird"
            18.0   6.   250.0      88.00      3139.      14.5   71.  1.	"ford mustang"
            23.0   4.   122.0      86.00      2220.      14.0   71.  1.	"mercury capri 2000"
            28.0   4.   116.0      90.00      2123.      14.0   71.  2.	"opel 1900"
            30.0   4.   79.00      70.00      2074.      19.5   71.  2.	"peugeot 304"
            30.0   4.   88.00      76.00      2065.      14.5   71.  2.	"fiat 124b"
            31.0   4.   71.00      65.00      1773.      19.0   71.  3.	"toyota corolla 1200"
            35.0   4.   72.00      69.00      1613.      18.0   71.  3.	"datsun 1200"
            27.0   4.   97.00      60.00      1834.      19.0   71.  2.	"volkswagen model 111"
            26.0   4.   91.00      70.00      1955.      20.5   71.  1.	"plymouth cricket"
            24.0   4.   113.0      95.00      2278.      15.5   72.  3.	"toyota corona hardtop"
            25.0   4.   97.50      80.00      2126.      17.0   72.  1.	"dodge colt hardtop"
            23.0   4.   97.00      54.00      2254.      23.5   72.  2.	"volkswagen type 3"
            20.0   4.   140.0      90.00      2408.      19.5   72.  1.	"chevrolet vega"
            21.0   4.   122.0      86.00      2226.      16.5   72.  1.	"ford pinto runabout"
            13.0   8.   350.0      165.0      4274.      12.0   72.  1.	"chevrolet impala"
            14.0   8.   400.0      175.0      4385.      12.0   72.  1.	"pontiac catalina"
            15.0   8.   318.0      150.0      4135.      13.5   72.  1.	"plymouth fury iii"
            14.0   8.   351.0      153.0      4129.      13.0   72.  1.	"ford galaxie 500"
            17.0   8.   304.0      150.0      3672.      11.5   72.  1.	"amc ambassador sst"
            11.0   8.   429.0      208.0      4633.      11.0   72.  1.	"mercury marquis"
            13.0   8.   350.0      155.0      4502.      13.5   72.  1.	"buick lesabre custom"
            12.0   8.   350.0      160.0      4456.      13.5   72.  1.	"oldsmobile delta 88 royale"
            13.0   8.   400.0      190.0      4422.      12.5   72.  1.	"chrysler newport royal"
            19.0   3.   70.00      97.00      2330.      13.5   72.  3.	"mazda rx2 coupe"
            15.0   8.   304.0      150.0      3892.      12.5   72.  1.	"amc matador (sw)"
            13.0   8.   307.0      130.0      4098.      14.0   72.  1.	"chevrolet chevelle concours (sw)"
            13.0   8.   302.0      140.0      4294.      16.0   72.  1.	"ford gran torino (sw)"
            14.0   8.   318.0      150.0      4077.      14.0   72.  1.	"plymouth satellite custom (sw)"
            18.0   4.   121.0      112.0      2933.      14.5   72.  2.	"volvo 145e (sw)"
            22.0   4.   121.0      76.00      2511.      18.0   72.  2.	"volkswagen 411 (sw)"
            21.0   4.   120.0      87.00      2979.      19.5   72.  2.	"peugeot 504 (sw)"
            26.0   4.   96.00      69.00      2189.      18.0   72.  2.	"renault 12 (sw)"
            22.0   4.   122.0      86.00      2395.      16.0   72.  1.	"ford pinto (sw)"
            28.0   4.   97.00      92.00      2288.      17.0   72.  3.	"datsun 510 (sw)"
            23.0   4.   120.0      97.00      2506.      14.5   72.  3.	"toyouta corona mark ii (sw)"
            28.0   4.   98.00      80.00      2164.      15.0   72.  1.	"dodge colt (sw)"
            27.0   4.   97.00      88.00      2100.      16.5   72.  3.	"toyota corolla 1600 (sw)"
            13.0   8.   350.0      175.0      4100.      13.0   73.  1.	"buick century 350"
            14.0   8.   304.0      150.0      3672.      11.5   73.  1.	"amc matador"
            13.0   8.   350.0      145.0      3988.      13.0   73.  1.	"chevrolet malibu"
            14.0   8.   302.0      137.0      4042.      14.5   73.  1.	"ford gran torino"
            15.0   8.   318.0      150.0      3777.      12.5   73.  1.	"dodge coronet custom"
            12.0   8.   429.0      198.0      4952.      11.5   73.  1.	"mercury marquis brougham"
            13.0   8.   400.0      150.0      4464.      12.0   73.  1.	"chevrolet caprice classic"
            13.0   8.   351.0      158.0      4363.      13.0   73.  1.	"ford ltd"
            14.0   8.   318.0      150.0      4237.      14.5   73.  1.	"plymouth fury gran sedan"
            13.0   8.   440.0      215.0      4735.      11.0   73.  1.	"chrysler new yorker brougham"
            12.0   8.   455.0      225.0      4951.      11.0   73.  1.	"buick electra 225 custom"
            13.0   8.   360.0      175.0      3821.      11.0   73.  1.	"amc ambassador brougham"
            18.0   6.   225.0      105.0      3121.      16.5   73.  1.	"plymouth valiant"
            16.0   6.   250.0      100.0      3278.      18.0   73.  1.	"chevrolet nova custom"
            18.0   6.   232.0      100.0      2945.      16.0   73.  1.	"amc hornet"
            18.0   6.   250.0      88.00      3021.      16.5   73.  1.	"ford maverick"
            23.0   6.   198.0      95.00      2904.      16.0   73.  1.	"plymouth duster"
            26.0   4.   97.00      46.00      1950.      21.0   73.  2.	"volkswagen super beetle"
            11.0   8.   400.0      150.0      4997.      14.0   73.  1.	"chevrolet impala"
            12.0   8.   400.0      167.0      4906.      12.5   73.  1.	"ford country"
            13.0   8.   360.0      170.0      4654.      13.0   73.  1.	"plymouth custom suburb"
            12.0   8.   350.0      180.0      4499.      12.5   73.  1.	"oldsmobile vista cruiser"
            18.0   6.   232.0      100.0      2789.      15.0   73.  1.	"amc gremlin"
            20.0   4.   97.00      88.00      2279.      19.0   73.  3.	"toyota carina"
            21.0   4.   140.0      72.00      2401.      19.5   73.  1.	"chevrolet vega"
            22.0   4.   108.0      94.00      2379.      16.5   73.  3.	"datsun 610"
            18.0   3.   70.00      90.00      2124.      13.5   73.  3.	"maxda rx3"
            19.0   4.   122.0      85.00      2310.      18.5   73.  1.	"ford pinto"
            21.0   6.   155.0      107.0      2472.      14.0   73.  1.	"mercury capri v6"
            26.0   4.   98.00      90.00      2265.      15.5   73.  2.	"fiat 124 sport coupe"
            15.0   8.   350.0      145.0      4082.      13.0   73.  1.	"chevrolet monte carlo s"
            16.0   8.   400.0      230.0      4278.      9.50   73.  1.	"pontiac grand prix"
            29.0   4.   68.00      49.00      1867.      19.5   73.  2.	"fiat 128"
            24.0   4.   116.0      75.00      2158.      15.5   73.  2.	"opel manta"
            20.0   4.   114.0      91.00      2582.      14.0   73.  2.	"audi 100ls"
            19.0   4.   121.0      112.0      2868.      15.5   73.  2.	"volvo 144ea"
            15.0   8.   318.0      150.0      3399.      11.0   73.  1.	"dodge dart custom"
            24.0   4.   121.0      110.0      2660.      14.0   73.  2.	"saab 99le"
            20.0   6.   156.0      122.0      2807.      13.5   73.  3.	"toyota mark ii"
            11.0   8.   350.0      180.0      3664.      11.0   73.  1.	"oldsmobile omega"
            20.0   6.   198.0      95.00      3102.      16.5   74.  1.	"plymouth duster"
            21.0   6.   200.0      NA         2875.      17.0   74.  1.	"ford maverick"
            19.0   6.   232.0      100.0      2901.      16.0   74.  1.	"amc hornet"
            15.0   6.   250.0      100.0      3336.      17.0   74.  1.	"chevrolet nova"
            31.0   4.   79.00      67.00      1950.      19.0   74.  3.	"datsun b210"
            26.0   4.   122.0      80.00      2451.      16.5   74.  1.	"ford pinto"
            32.0   4.   71.00      65.00      1836.      21.0   74.  3.	"toyota corolla 1200"
            25.0   4.   140.0      75.00      2542.      17.0   74.  1.	"chevrolet vega"
            16.0   6.   250.0      100.0      3781.      17.0   74.  1.	"chevrolet chevelle malibu classic"
            16.0   6.   258.0      110.0      3632.      18.0   74.  1.	"amc matador"
            18.0   6.   225.0      105.0      3613.      16.5   74.  1.	"plymouth satellite sebring"
            16.0   8.   302.0      140.0      4141.      14.0   74.  1.	"ford gran torino"
            13.0   8.   350.0      150.0      4699.      14.5   74.  1.	"buick century luxus (sw)"
            14.0   8.   318.0      150.0      4457.      13.5   74.  1.	"dodge coronet custom (sw)"
            14.0   8.   302.0      140.0      4638.      16.0   74.  1.	"ford gran torino (sw)"
            14.0   8.   304.0      150.0      4257.      15.5   74.  1.	"amc matador (sw)"
            29.0   4.   98.00      83.00      2219.      16.5   74.  2.	"audi fox"
            26.0   4.   79.00      67.00      1963.      15.5   74.  2.	"volkswagen dasher"
            26.0   4.   97.00      78.00      2300.      14.5   74.  2.	"opel manta"
            31.0   4.   76.00      52.00      1649.      16.5   74.  3.	"toyota corona"
            32.0   4.   83.00      61.00      2003.      19.0   74.  3.	"datsun 710"
            28.0   4.   90.00      75.00      2125.      14.5   74.  1.	"dodge colt"
            24.0   4.   90.00      75.00      2108.      15.5   74.  2.	"fiat 128"
            26.0   4.   116.0      75.00      2246.      14.0   74.  2.	"fiat 124 tc"
            24.0   4.   120.0      97.00      2489.      15.0   74.  3.	"honda civic"
            26.0   4.   108.0      93.00      2391.      15.5   74.  3.	"subaru"
            31.0   4.   79.00      67.00      2000.      16.0   74.  2.	"fiat x1.9"
            19.0   6.   225.0      95.00      3264.      16.0   75.  1.	"plymouth valiant custom"
            18.0   6.   250.0      105.0      3459.      16.0   75.  1.	"chevrolet nova"
            15.0   6.   250.0      72.00      3432.      21.0   75.  1.	"mercury monarch"
            15.0   6.   250.0      72.00      3158.      19.5   75.  1.	"ford maverick"
            16.0   8.   400.0      170.0      4668.      11.5   75.  1.	"pontiac catalina"
            15.0   8.   350.0      145.0      4440.      14.0   75.  1.	"chevrolet bel air"
            16.0   8.   318.0      150.0      4498.      14.5   75.  1.	"plymouth grand fury"
            14.0   8.   351.0      148.0      4657.      13.5   75.  1.	"ford ltd"
            17.0   6.   231.0      110.0      3907.      21.0   75.  1.	"buick century"
            16.0   6.   250.0      105.0      3897.      18.5   75.  1.	"chevroelt chevelle malibu"
            15.0   6.   258.0      110.0      3730.      19.0   75.  1.	"amc matador"
            18.0   6.   225.0      95.00      3785.      19.0   75.  1.	"plymouth fury"
            21.0   6.   231.0      110.0      3039.      15.0   75.  1.	"buick skyhawk"
            20.0   8.   262.0      110.0      3221.      13.5   75.  1.	"chevrolet monza 2+2"
            13.0   8.   302.0      129.0      3169.      12.0   75.  1.	"ford mustang ii"
            29.0   4.   97.00      75.00      2171.      16.0   75.  3.	"toyota corolla"
            23.0   4.   140.0      83.00      2639.      17.0   75.  1.	"ford pinto"
            20.0   6.   232.0      100.0      2914.      16.0   75.  1.	"amc gremlin"
            23.0   4.   140.0      78.00      2592.      18.5   75.  1.	"pontiac astro"
            24.0   4.   134.0      96.00      2702.      13.5   75.  3.	"toyota corona"
            25.0   4.   90.00      71.00      2223.      16.5   75.  2.	"volkswagen dasher"
            24.0   4.   119.0      97.00      2545.      17.0   75.  3.	"datsun 710"
            18.0   6.   171.0      97.00      2984.      14.5   75.  1.	"ford pinto"
            29.0   4.   90.00      70.00      1937.      14.0   75.  2.	"volkswagen rabbit"
            19.0   6.   232.0      90.00      3211.      17.0   75.  1.	"amc pacer"
            23.0   4.   115.0      95.00      2694.      15.0   75.  2.	"audi 100ls"
            23.0   4.   120.0      88.00      2957.      17.0   75.  2.	"peugeot 504"
            22.0   4.   121.0      98.00      2945.      14.5   75.  2.	"volvo 244dl"
            25.0   4.   121.0      115.0      2671.      13.5   75.  2.	"saab 99le"
            33.0   4.   91.00      53.00      1795.      17.5   75.  3.	"honda civic cvcc"
            28.0   4.   107.0      86.00      2464.      15.5   76.  2.	"fiat 131"
            25.0   4.   116.0      81.00      2220.      16.9   76.  2.	"opel 1900"
            25.0   4.   140.0      92.00      2572.      14.9   76.  1.	"capri ii"
            26.0   4.   98.00      79.00      2255.      17.7   76.  1.	"dodge colt"
            27.0   4.   101.0      83.00      2202.      15.3   76.  2.	"renault 12tl"
            17.5   8.   305.0      140.0      4215.      13.0   76.  1.	"chevrolet chevelle malibu classic"
            16.0   8.   318.0      150.0      4190.      13.0   76.  1.	"dodge coronet brougham"
            15.5   8.   304.0      120.0      3962.      13.9   76.  1.	"amc matador"
            14.5   8.   351.0      152.0      4215.      12.8   76.  1.	"ford gran torino"
            22.0   6.   225.0      100.0      3233.      15.4   76.  1.	"plymouth valiant"
            22.0   6.   250.0      105.0      3353.      14.5   76.  1.	"chevrolet nova"
            24.0   6.   200.0      81.00      3012.      17.6   76.  1.	"ford maverick"
            22.5   6.   232.0      90.00      3085.      17.6   76.  1.	"amc hornet"
            29.0   4.   85.00      52.00      2035.      22.2   76.  1.	"chevrolet chevette"
            24.5   4.   98.00      60.00      2164.      22.1   76.  1.	"chevrolet woody"
            29.0   4.   90.00      70.00      1937.      14.2   76.  2.	"vw rabbit"
            33.0   4.   91.00      53.00      1795.      17.4   76.  3.	"honda civic"
            20.0   6.   225.0      100.0      3651.      17.7   76.  1.	"dodge aspen se"
            18.0   6.   250.0      78.00      3574.      21.0   76.  1.	"ford granada ghia"
            18.5   6.   250.0      110.0      3645.      16.2   76.  1.	"pontiac ventura sj"
            17.5   6.   258.0      95.00      3193.      17.8   76.  1.	"amc pacer d/l"
            29.5   4.   97.00      71.00      1825.      12.2   76.  2.	"volkswagen rabbit"
            32.0   4.   85.00      70.00      1990.      17.0   76.  3.	"datsun b-210"
            28.0   4.   97.00      75.00      2155.      16.4   76.  3.	"toyota corolla"
            26.5   4.   140.0      72.00      2565.      13.6   76.  1.	"ford pinto"
            20.0   4.   130.0      102.0      3150.      15.7   76.  2.	"volvo 245"
            13.0   8.   318.0      150.0      3940.      13.2   76.  1.	"plymouth volare premier v8"
            19.0   4.   120.0      88.00      3270.      21.9   76.  2.	"peugeot 504"
            19.0   6.   156.0      108.0      2930.      15.5   76.  3.	"toyota mark ii"
            16.5   6.   168.0      120.0      3820.      16.7   76.  2.	"mercedes-benz 280s"
            16.5   8.   350.0      180.0      4380.      12.1   76.  1.	"cadillac seville"
            13.0   8.   350.0      145.0      4055.      12.0   76.  1.	"chevy c10"
            13.0   8.   302.0      130.0      3870.      15.0   76.  1.	"ford f108"
            13.0   8.   318.0      150.0      3755.      14.0   76.  1.	"dodge d100"
            31.5   4.   98.00      68.00      2045.      18.5   77.  3.	"honda accord cvcc"
            30.0   4.   111.0      80.00      2155.      14.8   77.  1.	"buick opel isuzu deluxe"
            36.0   4.   79.00      58.00      1825.      18.6   77.  2.	"renault 5 gtl"
            25.5   4.   122.0      96.00      2300.      15.5   77.  1.	"plymouth arrow gs"
            33.5   4.   85.00      70.00      1945.      16.8   77.  3.	"datsun f-10 hatchback"
            17.5   8.   305.0      145.0      3880.      12.5   77.  1.	"chevrolet caprice classic"
            17.0   8.   260.0      110.0      4060.      19.0   77.  1.	"oldsmobile cutlass supreme"
            15.5   8.   318.0      145.0      4140.      13.7   77.  1.	"dodge monaco brougham"
            15.0   8.   302.0      130.0      4295.      14.9   77.  1.	"mercury cougar brougham"
            17.5   6.   250.0      110.0      3520.      16.4   77.  1.	"chevrolet concours"
            20.5   6.   231.0      105.0      3425.      16.9   77.  1.	"buick skylark"
            19.0   6.   225.0      100.0      3630.      17.7   77.  1.	"plymouth volare custom"
            18.5   6.   250.0      98.00      3525.      19.0   77.  1.	"ford granada"
            16.0   8.   400.0      180.0      4220.      11.1   77.  1.	"pontiac grand prix lj"
            15.5   8.   350.0      170.0      4165.      11.4   77.  1.	"chevrolet monte carlo landau"
            15.5   8.   400.0      190.0      4325.      12.2   77.  1.	"chrysler cordoba"
            16.0   8.   351.0      149.0      4335.      14.5   77.  1.	"ford thunderbird"
            29.0   4.   97.00      78.00      1940.      14.5   77.  2.	"volkswagen rabbit custom"
            24.5   4.   151.0      88.00      2740.      16.0   77.  1.	"pontiac sunbird coupe"
            26.0   4.   97.00      75.00      2265.      18.2   77.  3.	"toyota corolla liftback"
            25.5   4.   140.0      89.00      2755.      15.8   77.  1.	"ford mustang ii 2+2"
            30.5   4.   98.00      63.00      2051.      17.0   77.  1.	"chevrolet chevette"
            33.5   4.   98.00      83.00      2075.      15.9   77.  1.	"dodge colt m/m"
            30.0   4.   97.00      67.00      1985.      16.4   77.  3.	"subaru dl"
            30.5   4.   97.00      78.00      2190.      14.1   77.  2.	"volkswagen dasher"
            22.0   6.   146.0      97.00      2815.      14.5   77.  3.	"datsun 810"
            21.5   4.   121.0      110.0      2600.      12.8   77.  2.	"bmw 320i"
            21.5   3.   80.00      110.0      2720.      13.5   77.  3.	"mazda rx-4"
            43.1   4.   90.00      48.00      1985.      21.5   78   2.	"volkswagen rabbit custom diesel"
            36.1   4.   98.00      66.00      1800.      14.4   78   1.	"ford fiesta"
            32.8   4.   78.00      52.00      1985.      19.4   78.  3.	"mazda glc deluxe"
            39.4   4.   85.00      70.00      2070.      18.6   78.  3.	"datsun b210 gx"
            36.1   4.   91.00      60.00      1800.      16.4   78.  3.	"honda civic cvcc"
            19.9   8.   260.0      110.0      3365.      15.5   78.  1.	"oldsmobile cutlass salon brougham"
            19.4   8.   318.0      140.0      3735.      13.2   78.  1.	"dodge diplomat"
            20.2   8.   302.0      139.0      3570.      12.8   78.  1.	"mercury monarch ghia"
            19.2   6.   231.0      105.0      3535.      19.2   78.  1.	"pontiac phoenix lj"
            20.5   6.   200.0      95.00      3155.      18.2   78.  1.	"chevrolet malibu"
            20.2   6.   200.0      85.00      2965.      15.8   78.  1.	"ford fairmont (auto)"
            25.1   4.   140.0      88.00      2720.      15.4   78.  1.	"ford fairmont (man)"
            20.5   6.   225.0      100.0      3430.      17.2   78.  1.	"plymouth volare"
            19.4   6.   232.0      90.00      3210.      17.2   78.  1.	"amc concord"
            20.6   6.   231.0      105.0      3380.      15.8   78.  1.	"buick century special"
            20.8   6.   200.0      85.00      3070.      16.7   78.  1.	"mercury zephyr"
            18.6   6.   225.0      110.0      3620.      18.7   78.  1.	"dodge aspen"
            18.1   6.   258.0      120.0      3410.      15.1   78.  1.	"amc concord d/l"
            19.2   8.   305.0      145.0      3425.      13.2   78.  1.	"chevrolet monte carlo landau"
            17.7   6.   231.0      165.0      3445.      13.4   78.  1.	"buick regal sport coupe (turbo)"
            18.1   8.   302.0      139.0      3205.      11.2   78.  1.	"ford futura"
            17.5   8.   318.0      140.0      4080.      13.7   78.  1.	"dodge magnum xe"
            30.0   4.   98.00      68.00      2155.      16.5   78.  1.	"chevrolet chevette"
            27.5   4.   134.0      95.00      2560.      14.2   78.  3.	"toyota corona"
            27.2   4.   119.0      97.00      2300.      14.7   78.  3.	"datsun 510"
            30.9   4.   105.0      75.00      2230.      14.5   78.  1.	"dodge omni"
            21.1   4.   134.0      95.00      2515.      14.8   78.  3.	"toyota celica gt liftback"
            23.2   4.   156.0      105.0      2745.      16.7   78.  1.	"plymouth sapporo"
            23.8   4.   151.0      85.00      2855.      17.6   78.  1.	"oldsmobile starfire sx"
            23.9   4.   119.0      97.00      2405.      14.9   78.  3.	"datsun 200-sx"
            20.3   5.   131.0      103.0      2830.      15.9   78.  2.	"audi 5000"
            17.0   6.   163.0      125.0      3140.      13.6   78.  2.	"volvo 264gl"
            21.6   4.   121.0      115.0      2795.      15.7   78.  2.	"saab 99gle"
            16.2   6.   163.0      133.0      3410.      15.8   78.  2.	"peugeot 604sl"
            31.5   4.   89.00      71.00      1990.      14.9   78.  2.	"volkswagen scirocco"
            29.5   4.   98.00      68.00      2135.      16.6   78.  3.	"honda accord lx"
            21.5   6.   231.0      115.0      3245.      15.4   79.  1.	"pontiac lemans v6"
            19.8   6.   200.0      85.00      2990.      18.2   79.  1.	"mercury zephyr 6"
            22.3   4.   140.0      88.00      2890.      17.3   79.  1.	"ford fairmont 4"
            20.2   6.   232.0      90.00      3265.      18.2   79.  1.	"amc concord dl 6"
            20.6   6.   225.0      110.0      3360.      16.6   79.  1.	"dodge aspen 6"
            17.0   8.   305.0      130.0      3840.      15.4   79.  1.	"chevrolet caprice classic"
            17.6   8.   302.0      129.0      3725.      13.4   79.  1.	"ford ltd landau"
            16.5   8.   351.0      138.0      3955.      13.2   79.  1.	"mercury grand marquis"
            18.2   8.   318.0      135.0      3830.      15.2   79.  1.	"dodge st. regis"
            16.9   8.   350.0      155.0      4360.      14.9   79.  1.	"buick estate wagon (sw)"
            15.5   8.   351.0      142.0      4054.      14.3   79.  1.	"ford country squire (sw)"
            19.2   8.   267.0      125.0      3605.      15.0   79.  1.	"chevrolet malibu classic (sw)"
            18.5   8.   360.0      150.0      3940.      13.0   79.  1.	"chrysler lebaron town @ country (sw)"
            31.9   4.   89.00      71.00      1925.      14.0   79.  2.	"vw rabbit custom"
            34.1   4.   86.00      65.00      1975.      15.2   79.  3.	"maxda glc deluxe"
            35.7   4.   98.00      80.00      1915.      14.4   79.  1.	"dodge colt hatchback custom"
            27.4   4.   121.0      80.00      2670.      15.0   79.  1.	"amc spirit dl"
            25.4   5.   183.0      77.00      3530.      20.1   79.  2.	"mercedes benz 300d"
            23.0   8.   350.0      125.0      3900.      17.4   79.  1.	"cadillac eldorado"
            27.2   4.   141.0      71.00      3190.      24.8   79.  2.	"peugeot 504"
            23.9   8.   260.0      90.00      3420.      22.2   79.  1.	"oldsmobile cutlass salon brougham"
            34.2   4.   105.0      70.00      2200.      13.2   79.  1.	"plymouth horizon"
            34.5   4.   105.0      70.00      2150.      14.9   79.  1.	"plymouth horizon tc3"
            31.8   4.   85.00      65.00      2020.      19.2   79.  3.	"datsun 210"
            37.3   4.   91.00      69.00      2130.      14.7   79.  2.	"fiat strada custom"
            28.4   4.   151.0      90.00      2670.      16.0   79.  1.	"buick skylark limited"
            28.8   6.   173.0      115.0      2595.      11.3   79.  1.	"chevrolet citation"
            26.8   6.   173.0      115.0      2700.      12.9   79.  1.	"oldsmobile omega brougham"
            33.5   4.   151.0      90.00      2556.      13.2   79.  1.	"pontiac phoenix"
            41.5   4.   98.00      76.00      2144.      14.7   80.  2.	"vw rabbit"
            38.1   4.   89.00      60.00      1968.      18.8   80.  3.	"toyota corolla tercel"
            32.1   4.   98.00      70.00      2120.      15.5   80.  1.	"chevrolet chevette"
            37.2   4.   86.00      65.00      2019.      16.4   80.  3.	"datsun 310"
            28.0   4.   151.0      90.00      2678.      16.5   80.  1.	"chevrolet citation"
            26.4   4.   140.0      88.00      2870.      18.1   80.  1.	"ford fairmont"
            24.3   4.   151.0      90.00      3003.      20.1   80.  1.	"amc concord"
            19.1   6.   225.0      90.00      3381.      18.7   80.  1.	"dodge aspen"
            34.3   4.   97.00      78.00      2188.      15.8   80.  2.	"audi 4000"
            29.8   4.   134.0      90.00      2711.      15.5   80.  3.	"toyota corona liftback"
            31.3   4.   120.0      75.00      2542.      17.5   80.  3.	"mazda 626"
            37.0   4.   119.0      92.00      2434.      15.0   80.  3.	"datsun 510 hatchback"
            32.2   4.   108.0      75.00      2265.      15.2   80.  3.	"toyota corolla"
            46.6   4.   86.00      65.00      2110.      17.9   80.  3.	"mazda glc"
            27.9   4.   156.0      105.0      2800.      14.4   80.  1.	"dodge colt"
            40.8   4.   85.00      65.00      2110.      19.2   80.  3.	"datsun 210"
            44.3   4.   90.00      48.00      2085.      21.7   80.  2.	"vw rabbit c (diesel)"
            43.4   4.   90.00      48.00      2335.      23.7   80.  2.	"vw dasher (diesel)"
            36.4   5.   121.0      67.00      2950.      19.9   80.  2.	"audi 5000s (diesel)"
            30.0   4.   146.0      67.00      3250.      21.8   80.  2.	"mercedes-benz 240d"
            44.6   4.   91.00      67.00      1850.      13.8   80.  3.	"honda civic 1500 gl"
            40.9   4.   85.00      NA         1835.      17.3   80.  2.	"renault lecar deluxe"
            33.8   4.   97.00      67.00      2145.      18.0   80.  3.	"subaru dl"
            29.8   4.   89.00      62.00      1845.      15.3   80.  2.	"vokswagen rabbit"
            32.7   6.   168.0      132.0      2910.      11.4   80.  3.	"datsun 280-zx"
            23.7   3.   70.00      100.0      2420.      12.5   80.  3.	"mazda rx-7 gs"
            35.0   4.   122.0      88.00      2500.      15.1   80.  2.	"triumph tr7 coupe"
            23.6   4.   140.0      NA         2905.      14.3   80.  1.	"ford mustang cobra"
            32.4   4.   107.0      72.00      2290.      17.0   80.  3.	"honda accord"
            27.2   4.   135.0      84.00      2490.      15.7   81.  1.	"plymouth reliant"
            26.6   4.   151.0      84.00      2635.      16.4   81.  1.	"buick skylark"
            25.8   4.   156.0      92.00      2620.      14.4   81.  1.	"dodge aries wagon (sw)"
            23.5   6.   173.0      110.0      2725.      12.6   81.  1.	"chevrolet citation"
            30.0   4.   135.0      84.00      2385.      12.9   81.  1.	"plymouth reliant"
            39.1   4.   79.00      58.00      1755.      16.9   81.  3.	"toyota starlet"
            39.0   4.   86.00      64.00      1875.      16.4   81.  1.	"plymouth champ"
            35.1   4.   81.00      60.00      1760.      16.1   81.  3.	"honda civic 1300"
            32.3   4.   97.00      67.00      2065.      17.8   81.  3.	"subaru"
            37.0   4.   85.00      65.00      1975.      19.4   81.  3.	"datsun 210 mpg"
            37.7   4.   89.00      62.00      2050.      17.3   81.  3.	"toyota tercel"
            34.1   4.   91.00      68.00      1985.      16.0   81.  3.	"mazda glc 4"
            34.7   4.   105.0      63.00      2215.      14.9   81.  1.	"plymouth horizon 4"
            34.4   4.   98.00      65.00      2045.      16.2   81.  1.	"ford escort 4w"
            29.9   4.   98.00      65.00      2380.      20.7   81.  1.	"ford escort 2h"
            33.0   4.   105.0      74.00      2190.      14.2   81.  2.	"volkswagen jetta"
            34.5   4.   100.0      NA         2320.      15.8   81.  2.	"renault 18i"
            33.7   4.   107.0      75.00      2210.      14.4   81.  3.	"honda prelude"
            32.4   4.   108.0      75.00      2350.      16.8   81.  3.	"toyota corolla"
            32.9   4.   119.0      100.0      2615.      14.8   81.  3.	"datsun 200sx"
            31.6   4.   120.0      74.00      2635.      18.3   81.  3.	"mazda 626"
            28.1   4.   141.0      80.00      3230.      20.4   81.  2.	"peugeot 505s turbo diesel"
            NA     4.   121.0      110.0      2800.      15.4   81.  2.	"saab 900s"
            30.7   6.   145.0      76.00      3160.      19.6   81.  2.	"volvo diesel"
            25.4   6.   168.0      116.0      2900.      12.6   81.  3.	"toyota cressida"
            24.2   6.   146.0      120.0      2930.      13.8   81.  3.	"datsun 810 maxima"
            22.4   6.   231.0      110.0      3415.      15.8   81.  1.	"buick century"
            26.6   8.   350.0      105.0      3725.      19.0   81.  1.	"oldsmobile cutlass ls"
            20.2   6.   200.0      88.00      3060.      17.1   81.  1.	"ford granada gl"
            17.6   6.   225.0      85.00      3465.      16.6   81.  1.	"chrysler lebaron salon"
            28.0   4.   112.0      88.00      2605.      19.6   82.  1.	"chevrolet cavalier"
            27.0   4.   112.0      88.00      2640.      18.6   82.  1.	"chevrolet cavalier wagon"
            34.0   4.   112.0      88.00      2395.      18.0   82.  1.	"chevrolet cavalier 2-door"
            31.0   4.   112.0      85.00      2575.      16.2   82.  1.	"pontiac j2000 se hatchback"
            29.0   4.   135.0      84.00      2525.      16.0   82.  1.	"dodge aries se"
            27.0   4.   151.0      90.00      2735.      18.0   82.  1.	"pontiac phoenix"
            24.0   4.   140.0      92.00      2865.      16.4   82.  1.	"ford fairmont futura"
            23.0   4.   151.0      NA         3035.      20.5   82.  1.	"amc concord dl"
            36.0   4.   105.0      74.00      1980.      15.3   82.  2.	"volkswagen rabbit l"
            37.0   4.   91.00      68.00      2025.      18.2   82.  3.	"mazda glc custom l"
            31.0   4.   91.00      68.00      1970.      17.6   82.  3.	"mazda glc custom"
            38.0   4.   105.0      63.00      2125.      14.7   82.  1.	"plymouth horizon miser"
            36.0   4.   98.00      70.00      2125.      17.3   82.  1.	"mercury lynx l"
            36.0   4.   120.0      88.00      2160.      14.5   82.  3.	"nissan stanza xe"
            36.0   4.   107.0      75.00      2205.      14.5   82.  3.	"honda accord"
            34.0   4.   108.0      70.00      2245       16.9   82.  3.	"toyota corolla"
            38.0   4.   91.00      67.00      1965.      15.0   82.  3.	"honda civic"
            32.0   4.   91.00      67.00      1965.      15.7   82.  3.	"honda civic (auto)"
            38.0   4.   91.00      67.00      1995.      16.2   82.  3.	"datsun 310 gx"
            25.0   6.   181.0      110.0      2945.      16.4   82.  1.	"buick century limited"
            38.0   6.   262.0      85.00      3015.      17.0   82.  1.	"oldsmobile cutlass ciera (diesel)"
            26.0   4.   156.0      92.00      2585.      14.5   82.  1.	"chrysler lebaron medallion"
            22.0   6.   232.0      112.0      2835       14.7   82.  1.	"ford granada l"
            32.0   4.   144.0      96.00      2665.      13.9   82.  3.	"toyota celica gt"
            36.0   4.   135.0      84.00      2370.      13.0   82.  1.	"dodge charger 2.2"
            27.0   4.   151.0      90.00      2950.      17.3   82.  1.	"chevrolet camaro"
            27.0   4.   140.0      86.00      2790.      15.6   82.  1.	"ford mustang gl"
            44.0   4.   97.00      52.00      2130.      24.6   82.  2.	"vw pickup"
            32.0   4.   135.0      84.00      2295.      11.6   82.  1.	"dodge rampage"
            28.0   4.   120.0      79.00      2625.      18.6   82.  1.	"ford ranger"
            31.0   4.   119.0      82.00      2720.      19.4   82.  1.	"chevy s-10"
            """;

    public static void main(String[] args) {
        // Initialize database and insert data
        initializeDatabase();

        // Create and show GUI
        SwingUtilities.invokeLater(() -> createAndShowGUI());
    }

    private static void initializeDatabase() {
        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {
            System.out.println("Connected to database successfully!");

            // Clear existing data
            Statement stmt = conn.createStatement();
            stmt.executeUpdate("DELETE FROM auto_mpg");
            System.out.println("Cleared existing data.");

            // Parse and insert data
            insertData(conn);

            System.out.println("Data inserted successfully!");

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void insertData(Connection conn) throws SQLException {
        BufferedReader reader = new BufferedReader(new StringReader(DATA));
        String line;
        int count = 0;

        String sql = "INSERT INTO auto_mpg (mpg, cylinders, displacement, horsepower, weight, acceleration, model_year, origin, car_name) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)";
        PreparedStatement pstmt = conn.prepareStatement(sql);

        try {
            while ((line = reader.readLine()) != null) {
                line = line.trim();
                if (line.isEmpty()) continue;

                System.out.println("Processing line: " + line);

                try {
                    // Use regex to split by spaces/tabs, but keep quoted strings together
                    String[] parts = line.split("\\s+", 9); // Split into max 9 parts

                    if (parts.length < 9) {
                        System.err.println("Skipping - only " + parts.length + " parts: " + line);
                        continue;
                    }

                    // Parse each field
                    String mpgStr = parts[0];
                    String cylindersStr = parts[1];
                    String displacementStr = parts[2];
                    String horsepowerStr = parts[3];
                    String weightStr = parts[4];
                    String accelerationStr = parts[5];
                    String modelYearStr = parts[6];
                    String originStr = parts[7];
                    String carName = parts[8];

                    // Remove quotes from car name if present
                    carName = carName.replace("\"", "").trim();

                    // Debug output for first few records
                    if (count < 5) {
                        System.out.println("Parsed - MPG: " + mpgStr + ", Car: " + carName);
                    }

                    // Set parameters with null handling for "NA"
                    // MPG
                    if (mpgStr.equals("NA") || mpgStr.equals("NA.")) {
                        pstmt.setNull(1, Types.FLOAT);
                    } else {
                        pstmt.setFloat(1, Float.parseFloat(mpgStr));
                    }

                    // Cylinders
                    pstmt.setInt(2, Integer.parseInt(cylindersStr.replace(".", "")));

                    // Displacement
                    if (displacementStr.equals("NA")) {
                        pstmt.setNull(3, Types.FLOAT);
                    } else {
                        pstmt.setFloat(3, Float.parseFloat(displacementStr));
                    }

                    // Horsepower
                    if (horsepowerStr.equals("NA") || horsepowerStr.equals("NA.")) {
                        pstmt.setNull(4, Types.FLOAT);
                    } else {
                        pstmt.setFloat(4, Float.parseFloat(horsepowerStr));
                    }

                    // Weight
                    if (weightStr.equals("NA")) {
                        pstmt.setNull(5, Types.FLOAT);
                    } else {
                        pstmt.setFloat(5, Float.parseFloat(weightStr));
                    }

                    // Acceleration
                    if (accelerationStr.equals("NA")) {
                        pstmt.setNull(6, Types.FLOAT);
                    } else {
                        pstmt.setFloat(6, Float.parseFloat(accelerationStr));
                    }

                    // Model Year (remove trailing dot if present)
                    String cleanYear = modelYearStr.replace(".", "");
                    pstmt.setInt(7, Integer.parseInt(cleanYear));

                    // Origin (remove trailing dot if present)
                    String cleanOrigin = originStr.replace(".", "");
                    pstmt.setInt(8, Integer.parseInt(cleanOrigin));

                    // Car Name
                    pstmt.setString(9, carName);

                    pstmt.executeUpdate();
                    count++;

                } catch (NumberFormatException e) {
                    System.err.println("Number format error in line: " + line);
                    System.err.println("Error: " + e.getMessage());
                    e.printStackTrace();
                }
            }

            System.out.println("Successfully inserted " + count + " records!");

            // Verify in database
            Statement checkStmt = conn.createStatement();
            ResultSet rs = checkStmt.executeQuery("SELECT COUNT(*) as total FROM auto_mpg");
            if (rs.next()) {
                System.out.println("Database now contains " + rs.getInt("total") + " records.");
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            pstmt.close();
        }
    }

    private static void createAndShowGUI() {
        JFrame frame = new JFrame("Auto MPG Database Query");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(1000, 600);

        // Main panel
        JPanel mainPanel = new JPanel(new BorderLayout());

        // Input panel
        JPanel inputPanel = new JPanel(new FlowLayout());
        JLabel instructionLabel = new JLabel("Enter query (ALL for all records, or SQL WHERE clause):");
        JTextField queryField = new JTextField(40);
        JButton executeButton = new JButton("Execute Query");
        JButton clearButton = new JButton("Clear");

        inputPanel.add(instructionLabel);
        inputPanel.add(queryField);
        inputPanel.add(executeButton);
        inputPanel.add(clearButton);

        // Results panel
        JTextArea resultsArea = new JTextArea();
        resultsArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(resultsArea);

        // Table for displaying results
        JTable resultsTable = new JTable();
        JScrollPane tableScrollPane = new JScrollPane(resultsTable);

        // Tabbed pane for different views
        JTabbedPane tabbedPane = new JTabbedPane();
        tabbedPane.addTab("Text Results", scrollPane);
        tabbedPane.addTab("Table View", tableScrollPane);

        // Add components to main panel
        mainPanel.add(inputPanel, BorderLayout.NORTH);
        mainPanel.add(tabbedPane, BorderLayout.CENTER);

        frame.add(mainPanel);

        // Action listeners
        executeButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String query = queryField.getText().trim();
                executeQuery(query, resultsArea, resultsTable);
            }
        });

        clearButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                queryField.setText("");
                resultsArea.setText("");
                resultsTable.setModel(new DefaultTableModel());
            }
        });

        // Add sample queries button
        JPanel samplePanel = new JPanel(new FlowLayout());
        JLabel sampleLabel = new JLabel("Sample queries:");
        
        String[] sampleQueries = {
                "ALL",
                "mpg > 30",
                "cylinders = 4",
                "origin = 1",
                "model_year >= 80",
                "horsepower > 150",
                "car_name LIKE '%ford%'",
                "weight < 2000",
                "car_name LIKE '%toyota%'",
                "car_name LIKE '%chevrolet%'",
                "acceleration BETWEEN 10 AND 15",
                "displacement < 200"
        };

        for (String sample : sampleQueries) {
            JButton sampleBtn = new JButton(sample);
            sampleBtn.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    queryField.setText(sample);
                    executeQuery(sample, resultsArea, resultsTable);
                }
            });
            samplePanel.add(sampleBtn);
        }

        mainPanel.add(samplePanel, BorderLayout.SOUTH);

        frame.setVisible(true);
    }

    private static void executeQuery(String query, JTextArea resultsArea, JTable resultsTable) {
        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {
            Statement stmt = conn.createStatement();
            String sql;

            // Trim and clean the query
            query = query.trim();

            // Handle special cases
            if (query.equalsIgnoreCase("ALL") || query.isEmpty()) {
                sql = "SELECT * FROM auto_mpg ORDER BY id";
            }
            // If it looks like a WHERE clause (contains operators)
            else if (query.contains("=") || query.contains(">") || query.contains("<") ||
                    query.contains("LIKE") || query.contains("BETWEEN") || query.contains("IN") ||
                    query.contains("AND") || query.contains("OR") || query.contains("NOT")) {
                sql = "SELECT * FROM auto_mpg WHERE " + query + " ORDER BY id";
            }
            // If it's just text, assume it's a car name search
            else {
                sql = "SELECT * FROM auto_mpg WHERE car_name LIKE '%" + query + "%' ORDER BY id";
            }

            System.out.println("Executing SQL: " + sql);

            try {
                ResultSet rs = stmt.executeQuery(sql);
                ResultSetMetaData rsmd = rs.getMetaData();
                int columnCount = rsmd.getColumnCount();

                // Display in text area
                StringBuilder sb = new StringBuilder();
                sb.append("Query: ").append(query).append("\n");
                sb.append("SQL: ").append(sql).append("\n\n");

                // Build header
                for (int i = 1; i <= columnCount; i++) {
                    sb.append(String.format("%-15s", rsmd.getColumnName(i)));
                }
                sb.append("\n");

                for (int i = 1; i <= columnCount; i++) {
                    sb.append("---------------");
                }
                sb.append("\n");

                // Build table model for JTable
                DefaultTableModel tableModel = new DefaultTableModel();

                // Add column names to table model
                for (int i = 1; i <= columnCount; i++) {
                    tableModel.addColumn(rsmd.getColumnName(i));
                }

                int rowCount = 0;
                while (rs.next()) {
                    rowCount++;
                    Object[] row = new Object[columnCount];

                    // Build text row
                    for (int i = 1; i <= columnCount; i++) {
                        Object value = rs.getObject(i);
                        sb.append(String.format("%-15s", value == null ? "NULL" : value.toString()));
                        row[i-1] = value;
                    }
                    sb.append("\n");

                    // Add row to table model
                    tableModel.addRow(row);
                }

                sb.append("\nTotal records found: ").append(rowCount);

                resultsArea.setText(sb.toString());
                resultsTable.setModel(tableModel);

                // Auto-adjust column widths
                for (int i = 0; i < resultsTable.getColumnModel().getColumnCount(); i++) {
                    resultsTable.getColumnModel().getColumn(i).setPreferredWidth(100);
                }

            } catch (SQLException e) {
                // Show user-friendly error message
                String errorMessage = "Error executing query!\n\n";
                errorMessage += "Your input: '" + query + "'\n";
                errorMessage += "Error: " + e.getMessage() + "\n\n";
                errorMessage += "Try one of these formats:\n";
                errorMessage += "1. ALL (to see all records)\n";
                errorMessage += "2. car_name LIKE '%ford%'\n";
                errorMessage += "3. mpg > 30\n";
                errorMessage += "4. cylinders = 4\n";
                errorMessage += "5. origin = 1\n";
                errorMessage += "6. model_year >= 80\n";
                errorMessage += "7. weight < 2000\n";
                errorMessage += "8. toyota (will search for toyota in car names)\n";
                errorMessage += "9. ford mustang (will search for 'ford mustang' in car names)\n";

                resultsArea.setText(errorMessage);
                resultsTable.setModel(new DefaultTableModel());
            }

        } catch (SQLException e) {
            resultsArea.setText("Database connection error: " + e.getMessage());
            e.printStackTrace();
        }
    }

}

```

## This is my result!

```.out

"C:\Users\Armaan Shamsaasef\.jdks\openjdk-25.0.1\bin\java.exe" "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2025.2.5\lib\idea_rt.jar=64414" -Dfile.encoding=UTF-8 -Dsun.stdout.encoding=UTF-8 -Dsun.stderr.encoding=UTF-8 -classpath "C:\Users\Armaan Shamsaasef\IdeaProjects\W15\out\production\W15;C:\Users\Armaan Shamsaasef\Downloads\mysql-connector-j-9.5.0.jar" AutoMPGDatabase
Connected to database successfully!
Cleared existing data.
Processing line: 18.0   8.   307.0      130.0      3504.      12.0   70.  1.	"chevrolet chevelle malibu"
Parsed - MPG: 18.0, Car: chevrolet chevelle malibu
Processing line: 15.0   8.   350.0      165.0      3693.      11.5   70.  1.	"buick skylark 320"
Parsed - MPG: 15.0, Car: buick skylark 320
Processing line: 18.0   8.   318.0      150.0      3436.      11.0   70.  1.	"plymouth satellite"
Parsed - MPG: 18.0, Car: plymouth satellite
Processing line: 16.0   8.   304.0      150.0      3433.      12.0   70.  1.	"amc rebel sst"
Parsed - MPG: 16.0, Car: amc rebel sst
Processing line: 17.0   8.   302.0      140.0      3449.      10.5   70.  1.	"ford torino"
Parsed - MPG: 17.0, Car: ford torino
Processing line: 15.0   8.   429.0      198.0      4341.      10.0   70.  1.	"ford galaxie 500"
Processing line: 14.0   8.   454.0      220.0      4354.       9.0   70.  1.	"chevrolet impala"
Processing line: 14.0   8.   440.0      215.0      4312.       8.5   70.  1.	"plymouth fury iii"
Processing line: 14.0   8.   455.0      225.0      4425.      10.0   70.  1.	"pontiac catalina"
Processing line: 15.0   8.   390.0      190.0      3850.       8.5   70.  1.	"amc ambassador dpl"
Processing line: NA     4.   133.0      115.0      3090.      17.5   70.  2.	"citroen ds-21 pallas"
Processing line: NA     8.   350.0      165.0      4142.      11.5   70.  1.	"chevrolet chevelle concours (sw)"
Processing line: NA     8.   351.0      153.0      4034.      11.0   70.  1.	"ford torino (sw)"
Processing line: NA     8.   383.0      175.0      4166.      10.5   70.  1.	"plymouth satellite (sw)"
Processing line: NA     8.   360.0      175.0      3850.      11.0   70.  1.	"amc rebel sst (sw)"
Processing line: 15.0   8.   383.0      170.0      3563.      10.0   70.  1.	"dodge challenger se"
Processing line: 14.0   8.   340.0      160.0      3609.       8.0   70.  1.	"plymouth 'cuda 340"
Processing line: NA     8.   302.0      140.0      3353.       8.0   70.  1.	"ford mustang boss 302"
Processing line: 15.0   8.   400.0      150.0      3761.       9.5   70.  1.	"chevrolet monte carlo"
Processing line: 14.0   8.   455.0      225.0      3086.      10.0   70.  1.	"buick estate wagon (sw)"
Processing line: 24.0   4.   113.0      95.00      2372.      15.0   70.  3.	"toyota corona mark ii"
Processing line: 22.0   6.   198.0      95.00      2833.      15.5   70.  1.	"plymouth duster"
Processing line: 18.0   6.   199.0      97.00      2774.      15.5   70.  1.	"amc hornet"
Processing line: 21.0   6.   200.0      85.00      2587.      16.0   70.  1.	"ford maverick"
Processing line: 27.0   4.   97.00      88.00      2130.      14.5   70.  3.	"datsun pl510"
Processing line: 26.0   4.   97.00      46.00      1835.      20.5   70.  2.	"volkswagen 1131 deluxe sedan"
Processing line: 25.0   4.   110.0      87.00      2672.      17.5   70.  2.	"peugeot 504"
Processing line: 24.0   4.   107.0      90.00      2430.      14.5   70.  2.	"audi 100 ls"
Processing line: 25.0   4.   104.0      95.00      2375.      17.5   70.  2.	"saab 99e"
Processing line: 26.0   4.   121.0      113.0      2234.      12.5   70.  2.	"bmw 2002"
Processing line: 21.0   6.   199.0      90.00      2648.      15.0   70.  1.	"amc gremlin"
Processing line: 10.0   8.   360.0      215.0      4615.      14.0   70.  1.	"ford f250"
Processing line: 10.0   8.   307.0      200.0      4376.      15.0   70.  1.	"chevy c20"
Processing line: 11.0   8.   318.0      210.0      4382.      13.5   70.  1.	"dodge d200"
Processing line: 9.0   8.   304.0      193.0      4732.      18.5   70.  1.	"hi 1200d"
Processing line: 27.0   4.   97.00      88.00      2130.      14.5   71.  3.	"datsun pl510"
Processing line: 28.0   4.   140.0      90.00      2264.      15.5   71.  1.	"chevrolet vega 2300"
Processing line: 25.0   4.   113.0      95.00      2228.      14.0   71.  3.	"toyota corona"
Processing line: 25.0   4.   98.00      NA         2046.      19.0   71.  1.	"ford pinto"
Processing line: NA     4.   97.00      48.00      1978.      20.0   71.  2.	"volkswagen super beetle 117"
Processing line: 19.0   6.   232.0      100.0      2634.      13.0   71.  1.	"amc gremlin"
Processing line: 16.0   6.   225.0      105.0      3439.      15.5   71.  1.	"plymouth satellite custom"
Processing line: 17.0   6.   250.0      100.0      3329.      15.5   71.  1.	"chevrolet chevelle malibu"
Processing line: 19.0   6.   250.0      88.00      3302.      15.5   71.  1.	"ford torino 500"
Processing line: 18.0   6.   232.0      100.0      3288.      15.5   71.  1.	"amc matador"
Processing line: 14.0   8.   350.0      165.0      4209.      12.0   71.  1.	"chevrolet impala"
Processing line: 14.0   8.   400.0      175.0      4464.      11.5   71.  1.	"pontiac catalina brougham"
Processing line: 14.0   8.   351.0      153.0      4154.      13.5   71.  1.	"ford galaxie 500"
Processing line: 14.0   8.   318.0      150.0      4096.      13.0   71.  1.	"plymouth fury iii"
Processing line: 12.0   8.   383.0      180.0      4955.      11.5   71.  1.	"dodge monaco (sw)"
Processing line: 13.0   8.   400.0      170.0      4746.      12.0   71.  1.	"ford country squire (sw)"
Processing line: 13.0   8.   400.0      175.0      5140.      12.0   71.  1.	"pontiac safari (sw)"
Processing line: 18.0   6.   258.0      110.0      2962.      13.5   71.  1.	"amc hornet sportabout (sw)"
Processing line: 22.0   4.   140.0      72.00      2408.      19.0   71.  1.	"chevrolet vega (sw)"
Processing line: 19.0   6.   250.0      100.0      3282.      15.0   71.  1.	"pontiac firebird"
Processing line: 18.0   6.   250.0      88.00      3139.      14.5   71.  1.	"ford mustang"
Processing line: 23.0   4.   122.0      86.00      2220.      14.0   71.  1.	"mercury capri 2000"
Processing line: 28.0   4.   116.0      90.00      2123.      14.0   71.  2.	"opel 1900"
Processing line: 30.0   4.   79.00      70.00      2074.      19.5   71.  2.	"peugeot 304"
Processing line: 30.0   4.   88.00      76.00      2065.      14.5   71.  2.	"fiat 124b"
Processing line: 31.0   4.   71.00      65.00      1773.      19.0   71.  3.	"toyota corolla 1200"
Processing line: 35.0   4.   72.00      69.00      1613.      18.0   71.  3.	"datsun 1200"
Processing line: 27.0   4.   97.00      60.00      1834.      19.0   71.  2.	"volkswagen model 111"
Processing line: 26.0   4.   91.00      70.00      1955.      20.5   71.  1.	"plymouth cricket"
Processing line: 24.0   4.   113.0      95.00      2278.      15.5   72.  3.	"toyota corona hardtop"
Processing line: 25.0   4.   97.50      80.00      2126.      17.0   72.  1.	"dodge colt hardtop"
Processing line: 23.0   4.   97.00      54.00      2254.      23.5   72.  2.	"volkswagen type 3"
Processing line: 20.0   4.   140.0      90.00      2408.      19.5   72.  1.	"chevrolet vega"
Processing line: 21.0   4.   122.0      86.00      2226.      16.5   72.  1.	"ford pinto runabout"
Processing line: 13.0   8.   350.0      165.0      4274.      12.0   72.  1.	"chevrolet impala"
Processing line: 14.0   8.   400.0      175.0      4385.      12.0   72.  1.	"pontiac catalina"
Processing line: 15.0   8.   318.0      150.0      4135.      13.5   72.  1.	"plymouth fury iii"
Processing line: 14.0   8.   351.0      153.0      4129.      13.0   72.  1.	"ford galaxie 500"
Processing line: 17.0   8.   304.0      150.0      3672.      11.5   72.  1.	"amc ambassador sst"
Processing line: 11.0   8.   429.0      208.0      4633.      11.0   72.  1.	"mercury marquis"
Processing line: 13.0   8.   350.0      155.0      4502.      13.5   72.  1.	"buick lesabre custom"
Processing line: 12.0   8.   350.0      160.0      4456.      13.5   72.  1.	"oldsmobile delta 88 royale"
Processing line: 13.0   8.   400.0      190.0      4422.      12.5   72.  1.	"chrysler newport royal"
Processing line: 19.0   3.   70.00      97.00      2330.      13.5   72.  3.	"mazda rx2 coupe"
Processing line: 15.0   8.   304.0      150.0      3892.      12.5   72.  1.	"amc matador (sw)"
Processing line: 13.0   8.   307.0      130.0      4098.      14.0   72.  1.	"chevrolet chevelle concours (sw)"
Processing line: 13.0   8.   302.0      140.0      4294.      16.0   72.  1.	"ford gran torino (sw)"
Processing line: 14.0   8.   318.0      150.0      4077.      14.0   72.  1.	"plymouth satellite custom (sw)"
Processing line: 18.0   4.   121.0      112.0      2933.      14.5   72.  2.	"volvo 145e (sw)"
Processing line: 22.0   4.   121.0      76.00      2511.      18.0   72.  2.	"volkswagen 411 (sw)"
Processing line: 21.0   4.   120.0      87.00      2979.      19.5   72.  2.	"peugeot 504 (sw)"
Processing line: 26.0   4.   96.00      69.00      2189.      18.0   72.  2.	"renault 12 (sw)"
Processing line: 22.0   4.   122.0      86.00      2395.      16.0   72.  1.	"ford pinto (sw)"
Processing line: 28.0   4.   97.00      92.00      2288.      17.0   72.  3.	"datsun 510 (sw)"
Processing line: 23.0   4.   120.0      97.00      2506.      14.5   72.  3.	"toyouta corona mark ii (sw)"
Processing line: 28.0   4.   98.00      80.00      2164.      15.0   72.  1.	"dodge colt (sw)"
Processing line: 27.0   4.   97.00      88.00      2100.      16.5   72.  3.	"toyota corolla 1600 (sw)"
Processing line: 13.0   8.   350.0      175.0      4100.      13.0   73.  1.	"buick century 350"
Processing line: 14.0   8.   304.0      150.0      3672.      11.5   73.  1.	"amc matador"
Processing line: 13.0   8.   350.0      145.0      3988.      13.0   73.  1.	"chevrolet malibu"
Processing line: 14.0   8.   302.0      137.0      4042.      14.5   73.  1.	"ford gran torino"
Processing line: 15.0   8.   318.0      150.0      3777.      12.5   73.  1.	"dodge coronet custom"
Processing line: 12.0   8.   429.0      198.0      4952.      11.5   73.  1.	"mercury marquis brougham"
Processing line: 13.0   8.   400.0      150.0      4464.      12.0   73.  1.	"chevrolet caprice classic"
Processing line: 13.0   8.   351.0      158.0      4363.      13.0   73.  1.	"ford ltd"
Processing line: 14.0   8.   318.0      150.0      4237.      14.5   73.  1.	"plymouth fury gran sedan"
Processing line: 13.0   8.   440.0      215.0      4735.      11.0   73.  1.	"chrysler new yorker brougham"
Processing line: 12.0   8.   455.0      225.0      4951.      11.0   73.  1.	"buick electra 225 custom"
Processing line: 13.0   8.   360.0      175.0      3821.      11.0   73.  1.	"amc ambassador brougham"
Processing line: 18.0   6.   225.0      105.0      3121.      16.5   73.  1.	"plymouth valiant"
Processing line: 16.0   6.   250.0      100.0      3278.      18.0   73.  1.	"chevrolet nova custom"
Processing line: 18.0   6.   232.0      100.0      2945.      16.0   73.  1.	"amc hornet"
Processing line: 18.0   6.   250.0      88.00      3021.      16.5   73.  1.	"ford maverick"
Processing line: 23.0   6.   198.0      95.00      2904.      16.0   73.  1.	"plymouth duster"
Processing line: 26.0   4.   97.00      46.00      1950.      21.0   73.  2.	"volkswagen super beetle"
Processing line: 11.0   8.   400.0      150.0      4997.      14.0   73.  1.	"chevrolet impala"
Processing line: 12.0   8.   400.0      167.0      4906.      12.5   73.  1.	"ford country"
Processing line: 13.0   8.   360.0      170.0      4654.      13.0   73.  1.	"plymouth custom suburb"
Processing line: 12.0   8.   350.0      180.0      4499.      12.5   73.  1.	"oldsmobile vista cruiser"
Processing line: 18.0   6.   232.0      100.0      2789.      15.0   73.  1.	"amc gremlin"
Processing line: 20.0   4.   97.00      88.00      2279.      19.0   73.  3.	"toyota carina"
Processing line: 21.0   4.   140.0      72.00      2401.      19.5   73.  1.	"chevrolet vega"
Processing line: 22.0   4.   108.0      94.00      2379.      16.5   73.  3.	"datsun 610"
Processing line: 18.0   3.   70.00      90.00      2124.      13.5   73.  3.	"maxda rx3"
Processing line: 19.0   4.   122.0      85.00      2310.      18.5   73.  1.	"ford pinto"
Processing line: 21.0   6.   155.0      107.0      2472.      14.0   73.  1.	"mercury capri v6"
Processing line: 26.0   4.   98.00      90.00      2265.      15.5   73.  2.	"fiat 124 sport coupe"
Processing line: 15.0   8.   350.0      145.0      4082.      13.0   73.  1.	"chevrolet monte carlo s"
Processing line: 16.0   8.   400.0      230.0      4278.      9.50   73.  1.	"pontiac grand prix"
Processing line: 29.0   4.   68.00      49.00      1867.      19.5   73.  2.	"fiat 128"
Processing line: 24.0   4.   116.0      75.00      2158.      15.5   73.  2.	"opel manta"
Processing line: 20.0   4.   114.0      91.00      2582.      14.0   73.  2.	"audi 100ls"
Processing line: 19.0   4.   121.0      112.0      2868.      15.5   73.  2.	"volvo 144ea"
Processing line: 15.0   8.   318.0      150.0      3399.      11.0   73.  1.	"dodge dart custom"
Processing line: 24.0   4.   121.0      110.0      2660.      14.0   73.  2.	"saab 99le"
Processing line: 20.0   6.   156.0      122.0      2807.      13.5   73.  3.	"toyota mark ii"
Processing line: 11.0   8.   350.0      180.0      3664.      11.0   73.  1.	"oldsmobile omega"
Processing line: 20.0   6.   198.0      95.00      3102.      16.5   74.  1.	"plymouth duster"
Processing line: 21.0   6.   200.0      NA         2875.      17.0   74.  1.	"ford maverick"
Processing line: 19.0   6.   232.0      100.0      2901.      16.0   74.  1.	"amc hornet"
Processing line: 15.0   6.   250.0      100.0      3336.      17.0   74.  1.	"chevrolet nova"
Processing line: 31.0   4.   79.00      67.00      1950.      19.0   74.  3.	"datsun b210"
Processing line: 26.0   4.   122.0      80.00      2451.      16.5   74.  1.	"ford pinto"
Processing line: 32.0   4.   71.00      65.00      1836.      21.0   74.  3.	"toyota corolla 1200"
Processing line: 25.0   4.   140.0      75.00      2542.      17.0   74.  1.	"chevrolet vega"
Processing line: 16.0   6.   250.0      100.0      3781.      17.0   74.  1.	"chevrolet chevelle malibu classic"
Processing line: 16.0   6.   258.0      110.0      3632.      18.0   74.  1.	"amc matador"
Processing line: 18.0   6.   225.0      105.0      3613.      16.5   74.  1.	"plymouth satellite sebring"
Processing line: 16.0   8.   302.0      140.0      4141.      14.0   74.  1.	"ford gran torino"
Processing line: 13.0   8.   350.0      150.0      4699.      14.5   74.  1.	"buick century luxus (sw)"
Processing line: 14.0   8.   318.0      150.0      4457.      13.5   74.  1.	"dodge coronet custom (sw)"
Processing line: 14.0   8.   302.0      140.0      4638.      16.0   74.  1.	"ford gran torino (sw)"
Processing line: 14.0   8.   304.0      150.0      4257.      15.5   74.  1.	"amc matador (sw)"
Processing line: 29.0   4.   98.00      83.00      2219.      16.5   74.  2.	"audi fox"
Processing line: 26.0   4.   79.00      67.00      1963.      15.5   74.  2.	"volkswagen dasher"
Processing line: 26.0   4.   97.00      78.00      2300.      14.5   74.  2.	"opel manta"
Processing line: 31.0   4.   76.00      52.00      1649.      16.5   74.  3.	"toyota corona"
Processing line: 32.0   4.   83.00      61.00      2003.      19.0   74.  3.	"datsun 710"
Processing line: 28.0   4.   90.00      75.00      2125.      14.5   74.  1.	"dodge colt"
Processing line: 24.0   4.   90.00      75.00      2108.      15.5   74.  2.	"fiat 128"
Processing line: 26.0   4.   116.0      75.00      2246.      14.0   74.  2.	"fiat 124 tc"
Processing line: 24.0   4.   120.0      97.00      2489.      15.0   74.  3.	"honda civic"
Processing line: 26.0   4.   108.0      93.00      2391.      15.5   74.  3.	"subaru"
Processing line: 31.0   4.   79.00      67.00      2000.      16.0   74.  2.	"fiat x1.9"
Processing line: 19.0   6.   225.0      95.00      3264.      16.0   75.  1.	"plymouth valiant custom"
Processing line: 18.0   6.   250.0      105.0      3459.      16.0   75.  1.	"chevrolet nova"
Processing line: 15.0   6.   250.0      72.00      3432.      21.0   75.  1.	"mercury monarch"
Processing line: 15.0   6.   250.0      72.00      3158.      19.5   75.  1.	"ford maverick"
Processing line: 16.0   8.   400.0      170.0      4668.      11.5   75.  1.	"pontiac catalina"
Processing line: 15.0   8.   350.0      145.0      4440.      14.0   75.  1.	"chevrolet bel air"
Processing line: 16.0   8.   318.0      150.0      4498.      14.5   75.  1.	"plymouth grand fury"
Processing line: 14.0   8.   351.0      148.0      4657.      13.5   75.  1.	"ford ltd"
Processing line: 17.0   6.   231.0      110.0      3907.      21.0   75.  1.	"buick century"
Processing line: 16.0   6.   250.0      105.0      3897.      18.5   75.  1.	"chevroelt chevelle malibu"
Processing line: 15.0   6.   258.0      110.0      3730.      19.0   75.  1.	"amc matador"
Processing line: 18.0   6.   225.0      95.00      3785.      19.0   75.  1.	"plymouth fury"
Processing line: 21.0   6.   231.0      110.0      3039.      15.0   75.  1.	"buick skyhawk"
Processing line: 20.0   8.   262.0      110.0      3221.      13.5   75.  1.	"chevrolet monza 2+2"
Processing line: 13.0   8.   302.0      129.0      3169.      12.0   75.  1.	"ford mustang ii"
Processing line: 29.0   4.   97.00      75.00      2171.      16.0   75.  3.	"toyota corolla"
Processing line: 23.0   4.   140.0      83.00      2639.      17.0   75.  1.	"ford pinto"
Processing line: 20.0   6.   232.0      100.0      2914.      16.0   75.  1.	"amc gremlin"
Processing line: 23.0   4.   140.0      78.00      2592.      18.5   75.  1.	"pontiac astro"
Processing line: 24.0   4.   134.0      96.00      2702.      13.5   75.  3.	"toyota corona"
Processing line: 25.0   4.   90.00      71.00      2223.      16.5   75.  2.	"volkswagen dasher"
Processing line: 24.0   4.   119.0      97.00      2545.      17.0   75.  3.	"datsun 710"
Processing line: 18.0   6.   171.0      97.00      2984.      14.5   75.  1.	"ford pinto"
Processing line: 29.0   4.   90.00      70.00      1937.      14.0   75.  2.	"volkswagen rabbit"
Processing line: 19.0   6.   232.0      90.00      3211.      17.0   75.  1.	"amc pacer"
Processing line: 23.0   4.   115.0      95.00      2694.      15.0   75.  2.	"audi 100ls"
Processing line: 23.0   4.   120.0      88.00      2957.      17.0   75.  2.	"peugeot 504"
Processing line: 22.0   4.   121.0      98.00      2945.      14.5   75.  2.	"volvo 244dl"
Processing line: 25.0   4.   121.0      115.0      2671.      13.5   75.  2.	"saab 99le"
Processing line: 33.0   4.   91.00      53.00      1795.      17.5   75.  3.	"honda civic cvcc"
Processing line: 28.0   4.   107.0      86.00      2464.      15.5   76.  2.	"fiat 131"
Processing line: 25.0   4.   116.0      81.00      2220.      16.9   76.  2.	"opel 1900"
Processing line: 25.0   4.   140.0      92.00      2572.      14.9   76.  1.	"capri ii"
Processing line: 26.0   4.   98.00      79.00      2255.      17.7   76.  1.	"dodge colt"
Processing line: 27.0   4.   101.0      83.00      2202.      15.3   76.  2.	"renault 12tl"
Processing line: 17.5   8.   305.0      140.0      4215.      13.0   76.  1.	"chevrolet chevelle malibu classic"
Processing line: 16.0   8.   318.0      150.0      4190.      13.0   76.  1.	"dodge coronet brougham"
Processing line: 15.5   8.   304.0      120.0      3962.      13.9   76.  1.	"amc matador"
Processing line: 14.5   8.   351.0      152.0      4215.      12.8   76.  1.	"ford gran torino"
Processing line: 22.0   6.   225.0      100.0      3233.      15.4   76.  1.	"plymouth valiant"
Processing line: 22.0   6.   250.0      105.0      3353.      14.5   76.  1.	"chevrolet nova"
Processing line: 24.0   6.   200.0      81.00      3012.      17.6   76.  1.	"ford maverick"
Processing line: 22.5   6.   232.0      90.00      3085.      17.6   76.  1.	"amc hornet"
Processing line: 29.0   4.   85.00      52.00      2035.      22.2   76.  1.	"chevrolet chevette"
Processing line: 24.5   4.   98.00      60.00      2164.      22.1   76.  1.	"chevrolet woody"
Processing line: 29.0   4.   90.00      70.00      1937.      14.2   76.  2.	"vw rabbit"
Processing line: 33.0   4.   91.00      53.00      1795.      17.4   76.  3.	"honda civic"
Processing line: 20.0   6.   225.0      100.0      3651.      17.7   76.  1.	"dodge aspen se"
Processing line: 18.0   6.   250.0      78.00      3574.      21.0   76.  1.	"ford granada ghia"
Processing line: 18.5   6.   250.0      110.0      3645.      16.2   76.  1.	"pontiac ventura sj"
Processing line: 17.5   6.   258.0      95.00      3193.      17.8   76.  1.	"amc pacer d/l"
Processing line: 29.5   4.   97.00      71.00      1825.      12.2   76.  2.	"volkswagen rabbit"
Processing line: 32.0   4.   85.00      70.00      1990.      17.0   76.  3.	"datsun b-210"
Processing line: 28.0   4.   97.00      75.00      2155.      16.4   76.  3.	"toyota corolla"
Processing line: 26.5   4.   140.0      72.00      2565.      13.6   76.  1.	"ford pinto"
Processing line: 20.0   4.   130.0      102.0      3150.      15.7   76.  2.	"volvo 245"
Processing line: 13.0   8.   318.0      150.0      3940.      13.2   76.  1.	"plymouth volare premier v8"
Processing line: 19.0   4.   120.0      88.00      3270.      21.9   76.  2.	"peugeot 504"
Processing line: 19.0   6.   156.0      108.0      2930.      15.5   76.  3.	"toyota mark ii"
Processing line: 16.5   6.   168.0      120.0      3820.      16.7   76.  2.	"mercedes-benz 280s"
Processing line: 16.5   8.   350.0      180.0      4380.      12.1   76.  1.	"cadillac seville"
Processing line: 13.0   8.   350.0      145.0      4055.      12.0   76.  1.	"chevy c10"
Processing line: 13.0   8.   302.0      130.0      3870.      15.0   76.  1.	"ford f108"
Processing line: 13.0   8.   318.0      150.0      3755.      14.0   76.  1.	"dodge d100"
Processing line: 31.5   4.   98.00      68.00      2045.      18.5   77.  3.	"honda accord cvcc"
Processing line: 30.0   4.   111.0      80.00      2155.      14.8   77.  1.	"buick opel isuzu deluxe"
Processing line: 36.0   4.   79.00      58.00      1825.      18.6   77.  2.	"renault 5 gtl"
Processing line: 25.5   4.   122.0      96.00      2300.      15.5   77.  1.	"plymouth arrow gs"
Processing line: 33.5   4.   85.00      70.00      1945.      16.8   77.  3.	"datsun f-10 hatchback"
Processing line: 17.5   8.   305.0      145.0      3880.      12.5   77.  1.	"chevrolet caprice classic"
Processing line: 17.0   8.   260.0      110.0      4060.      19.0   77.  1.	"oldsmobile cutlass supreme"
Processing line: 15.5   8.   318.0      145.0      4140.      13.7   77.  1.	"dodge monaco brougham"
Processing line: 15.0   8.   302.0      130.0      4295.      14.9   77.  1.	"mercury cougar brougham"
Processing line: 17.5   6.   250.0      110.0      3520.      16.4   77.  1.	"chevrolet concours"
Processing line: 20.5   6.   231.0      105.0      3425.      16.9   77.  1.	"buick skylark"
Processing line: 19.0   6.   225.0      100.0      3630.      17.7   77.  1.	"plymouth volare custom"
Processing line: 18.5   6.   250.0      98.00      3525.      19.0   77.  1.	"ford granada"
Processing line: 16.0   8.   400.0      180.0      4220.      11.1   77.  1.	"pontiac grand prix lj"
Processing line: 15.5   8.   350.0      170.0      4165.      11.4   77.  1.	"chevrolet monte carlo landau"
Processing line: 15.5   8.   400.0      190.0      4325.      12.2   77.  1.	"chrysler cordoba"
Processing line: 16.0   8.   351.0      149.0      4335.      14.5   77.  1.	"ford thunderbird"
Processing line: 29.0   4.   97.00      78.00      1940.      14.5   77.  2.	"volkswagen rabbit custom"
Processing line: 24.5   4.   151.0      88.00      2740.      16.0   77.  1.	"pontiac sunbird coupe"
Processing line: 26.0   4.   97.00      75.00      2265.      18.2   77.  3.	"toyota corolla liftback"
Processing line: 25.5   4.   140.0      89.00      2755.      15.8   77.  1.	"ford mustang ii 2+2"
Processing line: 30.5   4.   98.00      63.00      2051.      17.0   77.  1.	"chevrolet chevette"
Processing line: 33.5   4.   98.00      83.00      2075.      15.9   77.  1.	"dodge colt m/m"
Processing line: 30.0   4.   97.00      67.00      1985.      16.4   77.  3.	"subaru dl"
Processing line: 30.5   4.   97.00      78.00      2190.      14.1   77.  2.	"volkswagen dasher"
Processing line: 22.0   6.   146.0      97.00      2815.      14.5   77.  3.	"datsun 810"
Processing line: 21.5   4.   121.0      110.0      2600.      12.8   77.  2.	"bmw 320i"
Processing line: 21.5   3.   80.00      110.0      2720.      13.5   77.  3.	"mazda rx-4"
Processing line: 43.1   4.   90.00      48.00      1985.      21.5   78   2.	"volkswagen rabbit custom diesel"
Processing line: 36.1   4.   98.00      66.00      1800.      14.4   78   1.	"ford fiesta"
Processing line: 32.8   4.   78.00      52.00      1985.      19.4   78.  3.	"mazda glc deluxe"
Processing line: 39.4   4.   85.00      70.00      2070.      18.6   78.  3.	"datsun b210 gx"
Processing line: 36.1   4.   91.00      60.00      1800.      16.4   78.  3.	"honda civic cvcc"
Processing line: 19.9   8.   260.0      110.0      3365.      15.5   78.  1.	"oldsmobile cutlass salon brougham"
Processing line: 19.4   8.   318.0      140.0      3735.      13.2   78.  1.	"dodge diplomat"
Processing line: 20.2   8.   302.0      139.0      3570.      12.8   78.  1.	"mercury monarch ghia"
Processing line: 19.2   6.   231.0      105.0      3535.      19.2   78.  1.	"pontiac phoenix lj"
Processing line: 20.5   6.   200.0      95.00      3155.      18.2   78.  1.	"chevrolet malibu"
Processing line: 20.2   6.   200.0      85.00      2965.      15.8   78.  1.	"ford fairmont (auto)"
Processing line: 25.1   4.   140.0      88.00      2720.      15.4   78.  1.	"ford fairmont (man)"
Processing line: 20.5   6.   225.0      100.0      3430.      17.2   78.  1.	"plymouth volare"
Processing line: 19.4   6.   232.0      90.00      3210.      17.2   78.  1.	"amc concord"
Processing line: 20.6   6.   231.0      105.0      3380.      15.8   78.  1.	"buick century special"
Processing line: 20.8   6.   200.0      85.00      3070.      16.7   78.  1.	"mercury zephyr"
Processing line: 18.6   6.   225.0      110.0      3620.      18.7   78.  1.	"dodge aspen"
Processing line: 18.1   6.   258.0      120.0      3410.      15.1   78.  1.	"amc concord d/l"
Processing line: 19.2   8.   305.0      145.0      3425.      13.2   78.  1.	"chevrolet monte carlo landau"
Processing line: 17.7   6.   231.0      165.0      3445.      13.4   78.  1.	"buick regal sport coupe (turbo)"
Processing line: 18.1   8.   302.0      139.0      3205.      11.2   78.  1.	"ford futura"
Processing line: 17.5   8.   318.0      140.0      4080.      13.7   78.  1.	"dodge magnum xe"
Processing line: 30.0   4.   98.00      68.00      2155.      16.5   78.  1.	"chevrolet chevette"
Processing line: 27.5   4.   134.0      95.00      2560.      14.2   78.  3.	"toyota corona"
Processing line: 27.2   4.   119.0      97.00      2300.      14.7   78.  3.	"datsun 510"
Processing line: 30.9   4.   105.0      75.00      2230.      14.5   78.  1.	"dodge omni"
Processing line: 21.1   4.   134.0      95.00      2515.      14.8   78.  3.	"toyota celica gt liftback"
Processing line: 23.2   4.   156.0      105.0      2745.      16.7   78.  1.	"plymouth sapporo"
Processing line: 23.8   4.   151.0      85.00      2855.      17.6   78.  1.	"oldsmobile starfire sx"
Processing line: 23.9   4.   119.0      97.00      2405.      14.9   78.  3.	"datsun 200-sx"
Processing line: 20.3   5.   131.0      103.0      2830.      15.9   78.  2.	"audi 5000"
Processing line: 17.0   6.   163.0      125.0      3140.      13.6   78.  2.	"volvo 264gl"
Processing line: 21.6   4.   121.0      115.0      2795.      15.7   78.  2.	"saab 99gle"
Processing line: 16.2   6.   163.0      133.0      3410.      15.8   78.  2.	"peugeot 604sl"
Processing line: 31.5   4.   89.00      71.00      1990.      14.9   78.  2.	"volkswagen scirocco"
Processing line: 29.5   4.   98.00      68.00      2135.      16.6   78.  3.	"honda accord lx"
Processing line: 21.5   6.   231.0      115.0      3245.      15.4   79.  1.	"pontiac lemans v6"
Processing line: 19.8   6.   200.0      85.00      2990.      18.2   79.  1.	"mercury zephyr 6"
Processing line: 22.3   4.   140.0      88.00      2890.      17.3   79.  1.	"ford fairmont 4"
Processing line: 20.2   6.   232.0      90.00      3265.      18.2   79.  1.	"amc concord dl 6"
Processing line: 20.6   6.   225.0      110.0      3360.      16.6   79.  1.	"dodge aspen 6"
Processing line: 17.0   8.   305.0      130.0      3840.      15.4   79.  1.	"chevrolet caprice classic"
Processing line: 17.6   8.   302.0      129.0      3725.      13.4   79.  1.	"ford ltd landau"
Processing line: 16.5   8.   351.0      138.0      3955.      13.2   79.  1.	"mercury grand marquis"
Processing line: 18.2   8.   318.0      135.0      3830.      15.2   79.  1.	"dodge st. regis"
Processing line: 16.9   8.   350.0      155.0      4360.      14.9   79.  1.	"buick estate wagon (sw)"
Processing line: 15.5   8.   351.0      142.0      4054.      14.3   79.  1.	"ford country squire (sw)"
Processing line: 19.2   8.   267.0      125.0      3605.      15.0   79.  1.	"chevrolet malibu classic (sw)"
Processing line: 18.5   8.   360.0      150.0      3940.      13.0   79.  1.	"chrysler lebaron town @ country (sw)"
Processing line: 31.9   4.   89.00      71.00      1925.      14.0   79.  2.	"vw rabbit custom"
Processing line: 34.1   4.   86.00      65.00      1975.      15.2   79.  3.	"maxda glc deluxe"
Processing line: 35.7   4.   98.00      80.00      1915.      14.4   79.  1.	"dodge colt hatchback custom"
Processing line: 27.4   4.   121.0      80.00      2670.      15.0   79.  1.	"amc spirit dl"
Processing line: 25.4   5.   183.0      77.00      3530.      20.1   79.  2.	"mercedes benz 300d"
Processing line: 23.0   8.   350.0      125.0      3900.      17.4   79.  1.	"cadillac eldorado"
Processing line: 27.2   4.   141.0      71.00      3190.      24.8   79.  2.	"peugeot 504"
Processing line: 23.9   8.   260.0      90.00      3420.      22.2   79.  1.	"oldsmobile cutlass salon brougham"
Processing line: 34.2   4.   105.0      70.00      2200.      13.2   79.  1.	"plymouth horizon"
Processing line: 34.5   4.   105.0      70.00      2150.      14.9   79.  1.	"plymouth horizon tc3"
Processing line: 31.8   4.   85.00      65.00      2020.      19.2   79.  3.	"datsun 210"
Processing line: 37.3   4.   91.00      69.00      2130.      14.7   79.  2.	"fiat strada custom"
Processing line: 28.4   4.   151.0      90.00      2670.      16.0   79.  1.	"buick skylark limited"
Processing line: 28.8   6.   173.0      115.0      2595.      11.3   79.  1.	"chevrolet citation"
Processing line: 26.8   6.   173.0      115.0      2700.      12.9   79.  1.	"oldsmobile omega brougham"
Processing line: 33.5   4.   151.0      90.00      2556.      13.2   79.  1.	"pontiac phoenix"
Processing line: 41.5   4.   98.00      76.00      2144.      14.7   80.  2.	"vw rabbit"
Processing line: 38.1   4.   89.00      60.00      1968.      18.8   80.  3.	"toyota corolla tercel"
Processing line: 32.1   4.   98.00      70.00      2120.      15.5   80.  1.	"chevrolet chevette"
Processing line: 37.2   4.   86.00      65.00      2019.      16.4   80.  3.	"datsun 310"
Processing line: 28.0   4.   151.0      90.00      2678.      16.5   80.  1.	"chevrolet citation"
Processing line: 26.4   4.   140.0      88.00      2870.      18.1   80.  1.	"ford fairmont"
Processing line: 24.3   4.   151.0      90.00      3003.      20.1   80.  1.	"amc concord"
Processing line: 19.1   6.   225.0      90.00      3381.      18.7   80.  1.	"dodge aspen"
Processing line: 34.3   4.   97.00      78.00      2188.      15.8   80.  2.	"audi 4000"
Processing line: 29.8   4.   134.0      90.00      2711.      15.5   80.  3.	"toyota corona liftback"
Processing line: 31.3   4.   120.0      75.00      2542.      17.5   80.  3.	"mazda 626"
Processing line: 37.0   4.   119.0      92.00      2434.      15.0   80.  3.	"datsun 510 hatchback"
Processing line: 32.2   4.   108.0      75.00      2265.      15.2   80.  3.	"toyota corolla"
Processing line: 46.6   4.   86.00      65.00      2110.      17.9   80.  3.	"mazda glc"
Processing line: 27.9   4.   156.0      105.0      2800.      14.4   80.  1.	"dodge colt"
Processing line: 40.8   4.   85.00      65.00      2110.      19.2   80.  3.	"datsun 210"
Processing line: 44.3   4.   90.00      48.00      2085.      21.7   80.  2.	"vw rabbit c (diesel)"
Processing line: 43.4   4.   90.00      48.00      2335.      23.7   80.  2.	"vw dasher (diesel)"
Processing line: 36.4   5.   121.0      67.00      2950.      19.9   80.  2.	"audi 5000s (diesel)"
Processing line: 30.0   4.   146.0      67.00      3250.      21.8   80.  2.	"mercedes-benz 240d"
Processing line: 44.6   4.   91.00      67.00      1850.      13.8   80.  3.	"honda civic 1500 gl"
Processing line: 40.9   4.   85.00      NA         1835.      17.3   80.  2.	"renault lecar deluxe"
Processing line: 33.8   4.   97.00      67.00      2145.      18.0   80.  3.	"subaru dl"
Processing line: 29.8   4.   89.00      62.00      1845.      15.3   80.  2.	"vokswagen rabbit"
Processing line: 32.7   6.   168.0      132.0      2910.      11.4   80.  3.	"datsun 280-zx"
Processing line: 23.7   3.   70.00      100.0      2420.      12.5   80.  3.	"mazda rx-7 gs"
Processing line: 35.0   4.   122.0      88.00      2500.      15.1   80.  2.	"triumph tr7 coupe"
Processing line: 23.6   4.   140.0      NA         2905.      14.3   80.  1.	"ford mustang cobra"
Processing line: 32.4   4.   107.0      72.00      2290.      17.0   80.  3.	"honda accord"
Processing line: 27.2   4.   135.0      84.00      2490.      15.7   81.  1.	"plymouth reliant"
Processing line: 26.6   4.   151.0      84.00      2635.      16.4   81.  1.	"buick skylark"
Processing line: 25.8   4.   156.0      92.00      2620.      14.4   81.  1.	"dodge aries wagon (sw)"
Processing line: 23.5   6.   173.0      110.0      2725.      12.6   81.  1.	"chevrolet citation"
Processing line: 30.0   4.   135.0      84.00      2385.      12.9   81.  1.	"plymouth reliant"
Processing line: 39.1   4.   79.00      58.00      1755.      16.9   81.  3.	"toyota starlet"
Processing line: 39.0   4.   86.00      64.00      1875.      16.4   81.  1.	"plymouth champ"
Processing line: 35.1   4.   81.00      60.00      1760.      16.1   81.  3.	"honda civic 1300"
Processing line: 32.3   4.   97.00      67.00      2065.      17.8   81.  3.	"subaru"
Processing line: 37.0   4.   85.00      65.00      1975.      19.4   81.  3.	"datsun 210 mpg"
Processing line: 37.7   4.   89.00      62.00      2050.      17.3   81.  3.	"toyota tercel"
Processing line: 34.1   4.   91.00      68.00      1985.      16.0   81.  3.	"mazda glc 4"
Processing line: 34.7   4.   105.0      63.00      2215.      14.9   81.  1.	"plymouth horizon 4"
Processing line: 34.4   4.   98.00      65.00      2045.      16.2   81.  1.	"ford escort 4w"
Processing line: 29.9   4.   98.00      65.00      2380.      20.7   81.  1.	"ford escort 2h"
Processing line: 33.0   4.   105.0      74.00      2190.      14.2   81.  2.	"volkswagen jetta"
Processing line: 34.5   4.   100.0      NA         2320.      15.8   81.  2.	"renault 18i"
Processing line: 33.7   4.   107.0      75.00      2210.      14.4   81.  3.	"honda prelude"
Processing line: 32.4   4.   108.0      75.00      2350.      16.8   81.  3.	"toyota corolla"
Processing line: 32.9   4.   119.0      100.0      2615.      14.8   81.  3.	"datsun 200sx"
Processing line: 31.6   4.   120.0      74.00      2635.      18.3   81.  3.	"mazda 626"
Processing line: 28.1   4.   141.0      80.00      3230.      20.4   81.  2.	"peugeot 505s turbo diesel"
Processing line: NA     4.   121.0      110.0      2800.      15.4   81.  2.	"saab 900s"
Processing line: 30.7   6.   145.0      76.00      3160.      19.6   81.  2.	"volvo diesel"
Processing line: 25.4   6.   168.0      116.0      2900.      12.6   81.  3.	"toyota cressida"
Processing line: 24.2   6.   146.0      120.0      2930.      13.8   81.  3.	"datsun 810 maxima"
Processing line: 22.4   6.   231.0      110.0      3415.      15.8   81.  1.	"buick century"
Processing line: 26.6   8.   350.0      105.0      3725.      19.0   81.  1.	"oldsmobile cutlass ls"
Processing line: 20.2   6.   200.0      88.00      3060.      17.1   81.  1.	"ford granada gl"
Processing line: 17.6   6.   225.0      85.00      3465.      16.6   81.  1.	"chrysler lebaron salon"
Processing line: 28.0   4.   112.0      88.00      2605.      19.6   82.  1.	"chevrolet cavalier"
Processing line: 27.0   4.   112.0      88.00      2640.      18.6   82.  1.	"chevrolet cavalier wagon"
Processing line: 34.0   4.   112.0      88.00      2395.      18.0   82.  1.	"chevrolet cavalier 2-door"
Processing line: 31.0   4.   112.0      85.00      2575.      16.2   82.  1.	"pontiac j2000 se hatchback"
Processing line: 29.0   4.   135.0      84.00      2525.      16.0   82.  1.	"dodge aries se"
Processing line: 27.0   4.   151.0      90.00      2735.      18.0   82.  1.	"pontiac phoenix"
Processing line: 24.0   4.   140.0      92.00      2865.      16.4   82.  1.	"ford fairmont futura"
Processing line: 23.0   4.   151.0      NA         3035.      20.5   82.  1.	"amc concord dl"
Processing line: 36.0   4.   105.0      74.00      1980.      15.3   82.  2.	"volkswagen rabbit l"
Processing line: 37.0   4.   91.00      68.00      2025.      18.2   82.  3.	"mazda glc custom l"
Processing line: 31.0   4.   91.00      68.00      1970.      17.6   82.  3.	"mazda glc custom"
Processing line: 38.0   4.   105.0      63.00      2125.      14.7   82.  1.	"plymouth horizon miser"
Processing line: 36.0   4.   98.00      70.00      2125.      17.3   82.  1.	"mercury lynx l"
Processing line: 36.0   4.   120.0      88.00      2160.      14.5   82.  3.	"nissan stanza xe"
Processing line: 36.0   4.   107.0      75.00      2205.      14.5   82.  3.	"honda accord"
Processing line: 34.0   4.   108.0      70.00      2245       16.9   82.  3.	"toyota corolla"
Processing line: 38.0   4.   91.00      67.00      1965.      15.0   82.  3.	"honda civic"
Processing line: 32.0   4.   91.00      67.00      1965.      15.7   82.  3.	"honda civic (auto)"
Processing line: 38.0   4.   91.00      67.00      1995.      16.2   82.  3.	"datsun 310 gx"
Processing line: 25.0   6.   181.0      110.0      2945.      16.4   82.  1.	"buick century limited"
Processing line: 38.0   6.   262.0      85.00      3015.      17.0   82.  1.	"oldsmobile cutlass ciera (diesel)"
Processing line: 26.0   4.   156.0      92.00      2585.      14.5   82.  1.	"chrysler lebaron medallion"
Processing line: 22.0   6.   232.0      112.0      2835       14.7   82.  1.	"ford granada l"
Processing line: 32.0   4.   144.0      96.00      2665.      13.9   82.  3.	"toyota celica gt"
Processing line: 36.0   4.   135.0      84.00      2370.      13.0   82.  1.	"dodge charger 2.2"
Processing line: 27.0   4.   151.0      90.00      2950.      17.3   82.  1.	"chevrolet camaro"
Processing line: 27.0   4.   140.0      86.00      2790.      15.6   82.  1.	"ford mustang gl"
Processing line: 44.0   4.   97.00      52.00      2130.      24.6   82.  2.	"vw pickup"
Processing line: 32.0   4.   135.0      84.00      2295.      11.6   82.  1.	"dodge rampage"
Processing line: 28.0   4.   120.0      79.00      2625.      18.6   82.  1.	"ford ranger"
Processing line: 31.0   4.   119.0      82.00      2720.      19.4   82.  1.	"chevy s-10"
Successfully inserted 406 records!
Database now contains 406 records.
Data inserted successfully!
Executing SQL: SELECT * FROM auto_mpg ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE mpg > 30 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE origin = 1 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE model_year >= 80 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE horsepower > 150 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE car_name LIKE '%ford%' ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE weight < 2000 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE mpg > 30 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE cylinders = 4 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE origin = 1 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE model_year >= 80 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE horsepower > 150 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE car_name LIKE '%ford%' ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE weight < 2000 ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE car_name LIKE '%toyota%' ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE car_name LIKE '%AUDI%' ORDER BY id
Executing SQL: SELECT * FROM auto_mpg WHERE car_name LIKE '%ford mustang%' ORDER BY id

Process finished with exit code 130


```


<img width="1465" height="890" alt="image" src="https://github.com/user-attachments/assets/c42657a3-8550-4e8c-ac65-d8f0e71cae3b" />


<img width="1459" height="867" alt="image" src="https://github.com/user-attachments/assets/b53947a1-16ad-44aa-a5ff-4ece60dd9ad1" />


<img width="1460" height="880" alt="image" src="https://github.com/user-attachments/assets/5c539d8d-2393-49da-87e3-3b90290b0004" />


<img width="1469" height="883" alt="image" src="https://github.com/user-attachments/assets/7e2fd068-9036-4ef2-ba89-0e2cb275fab8" />


# VIDEO EXPLANATION!








