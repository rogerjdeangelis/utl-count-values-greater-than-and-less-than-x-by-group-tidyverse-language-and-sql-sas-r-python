# utl-count-values-greater-than-and-less-than-x-by-group-tidyverse-language-and-sql-sas-r-python
Count values greater than and less than x by group tidyverse language and sql sas r python 
    %let pgm=utl-count-values-greater-than-and-less-than-x-by-group-tidyverse-language-and-sql-sas-r-python;

    Count values greater than and less than x by group tidyverse language and sql sas r python

    SOAPBOX ON
      Even though there wer 6 solutions non of them were base R solutions
    SOAPBOX OFF

    https://stackoverflow.com/questions/79318761/how-to-summarize-counts-based-on-multiple-conditions-in-r

         SOLUTIONS
             1 sas sql
             2 r sql
             3 pyhton sql
             4 r tidyverse language
               (syntax: %>% .by and summarize)

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                        |                                     |                                                         */
    /*                        |                                     |                                                         */
    /*       INPUT            |       PROCESS                       |          OUTPUT                                         */
    /*       =====            |   SELF EXPLANATORY SQL              |          ======                                         */
    /*                        |   ====================              |                                                         */
    /*                        |                                     |   Species Gtr_4.5 Less_4.5                              */
    /*  SPECIES    LEN        | SORTED FOR DOCUMENTATION            |                                                         */
    /*                        | PURPOSES. DATA DES NOT              | 1     BRK       2        2                              */
    /*    BRK      4.2        | HAVE TO BE SORTED                   | 2     RGN       1        1                              */
    /*    BRK      4.4        |                                     |                                                         */
    /*    BRK      5.3        |SPECIES LEN  FILTER                  |                                                         */
    /*    RGN      1.6        |                                     |                                                         */
    /*    BRK      5.9        | BRK    4.2 < 4.5                    |                                                         */
    /*    RGN      8.2        | BRK    4.4 < 4.5  count=2           |                                                         */
    /*                        |                                     |                                                         */
    /*                        | BRK    5.3 > 4.5                    |                                                         */
    /*                        | BRK    5.9 > 4.5  count=2           |                                                         */
    /*                        |                                     |                                                         */
    /*                        |                                     |                                                         */
    /*                        | RGN    1.6  < 4.5 count=1           |                                                         */
    /*                        |                                     |                                                         */
    /*                        | RGN    8.2  > 4.5 count=1           |                                                         */
    /*                        |                                     |                                                         */
    /*                        |                                     |                                                         */
    /*                        | 1-3 SAS SQL SAME IN R & PYTHON)     |                                                         */
    /*                        |                                     |                                                         */
    /*                        |                                     |                                                         */
    /*                        | select                              |                                                         */
    /*                        |   species                           |                                                         */
    /*                        |  ,sum(len>4.5)  as gt45             |                                                         */
    /*                        |  ,sum(len<4.5)  as lt45             |                                                         */
    /*                        | from                                |                                                         */
    /*                        |   sd1.have                          |                                                         */
    /*                        | group                               |                                                         */
    /*                        |   by species                        |                                                         */
    /*                        |                                     |                                                         */
    /*                        |                                     |                                                         */
    /*                        | 4 r tidyverse                       |                                                         */
    /*                        | =============                       |                                                         */
    /*                        |                                     |                                                         */
    /*                        | want <- have %>%                    |                                                         */
    /*                        |   summarise(                        |                                                         */
    /*                        |     Gtr_4.5 = sum(LEN > 4.5),       |                                                         */
    /*                        |     Less_4.5 = sum(LEN < 4.5),      |                                                         */
    /*                        |     .by = SPECIES                   |                                                         */
    /*                        |   )                                 |                                                         */
    /*                        |                                     |                                                         */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input Species$ Len;
    cards4;
    BRK 4.2
    BRK 4.4
    BRK 5.3
    RGN 1.6
    BRK 5.9
    RGN 8.2
    ;;;;
    run;quit;

    proc sql;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  SPECIES    LEN                                                                                                        */
    /*                                                                                                                        */
    /*    BRK      4.2                                                                                                        */
    /*    BRK      4.4                                                                                                        */
    /*    BRK      5.3                                                                                                        */
    /*    RGN      1.6                                                                                                        */
    /*    BRK      5.9                                                                                                        */
    /*    RGN      8.2                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    proc sql;

       select
         species
        ,sum(len>4.5)  as gt45
        ,sum(len<4.5)  as lt45
       from
         sd1.have
       group
         by species

    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*     Species Gtr_4.5 Less_4.5                                                                                           */
    /*                                                                                                                        */
    /*   1     BRK       2        2                                                                                           */
    /*   2     RGN       1        1                                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    library(tidyverse)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    have
    want<-sqldf('
       select
         species
        ,sum(len>4.5)  as gt45
        ,sum(len<4.5)  as lt45
       from
         have
       group
         by species
       ')
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;


    /**************************************************************************************************************************/
    /*                          |                                                                                             */
    /*  R                       | SAS                                                                                         */
    /*                          |                                                                                             */
    /*     SPECIES GT45 LT45    | ROWNAMES    SPECIES    GT45    LT45                                                         */
    /*                          |                                                                                             */
    /*   1     BRK    2    2    |     1         BRK        2       2                                                          */
    /*   2     RGN    1    1    |     2         RGN        1       1                                                          */
    /*                          |                                                                                             */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    want=pdsql('''
        select                        \
          species                     \
         ,sum(len>4.5)  as gt45       \
         ,sum(len<4.5)  as lt45       \
        from                          \
          have                        \
        group                         \
          by species                  \
       ''');
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;


    ************************************************************************************************************************/
    *                                 |                                                                                    */
    *  PYTHON                         |  SAS                                                                               */
    *                                 |                                                                                    */
    *    SPECIES  gt45  lt45     Obs  |  SPECIES    GT45    LT45                                                           */
    *                                 |                                                                                    */
    *  0     BRK     2     2      1   |    BRK        2       2                                                            */
    *  1     RGN     1     1      2   |    RGN        1       1                                                            */
    *                                 |                                                                                    */
    ************************************************************************************************************************/

    /*  _            _   _     _
    | || |    _ __  | |_(_) __| |_   ___   _____ _ __ ___  ___
    | || |_  | `__| | __| |/ _` | | | \ \ / / _ \ `__/ __|/ _ \
    |__   _| | |    | |_| | (_| | |_| |\ V /  __/ |  \__ \  __/
       |_|   |_|     \__|_|\__,_|\__, | \_/ \___|_|  |___/\___|
                                 |___/
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    library(tidyverse)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)
    have
    want <- have %>%
      summarise(
        Gtr_4.5 = sum(LEN > 4.5),
        Less_4.5 = sum(LEN < 4.5),
        .by = SPECIES
      )
    want
    fn_tosas9x(
          inp    = want1
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
