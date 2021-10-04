# utl-Untransposing-wide-to-log-with-1-line-of-code
Untransposing wide to log with 1 line of code 
   Untransposing wide to log with 1 line of code

    Github
    https://tinyurl.com/5aypt5v2
    https://github.com/rogerjdeangelis/utl-Untransposing-wide-to-log-with-1-line-of-code

    Stackoverflow
    https://stackoverflow.com/questions/64344781/sas-proc-tranpose-wide-to-long

    Macros
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    DATA HAVE;
    INPUT DATE :yymmdd10. Team1 $ Team2 $ Rate1 Rate2 ;
    format DATE yymmddd10.;
    datalines;
    2020-07-26 Arsenal Watford 2.0 3.6
    2020-07-26 Burnley Brighton 2.6 2.8
    ;
    RUN;


    Up to 40 obs WORK.HAVE total obs=2

    Obs     DATE     TEAM1      TEAM2      RATE1    RATE2

     1     22122    Arsenal    Watford      2.0      3.6
     2     22122    Burnley    Brighton     2.6      2.8

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %untranspose(data=haveX, out=want, by=date, id=time,  var=team rate )

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    Up to 40 obs from WANT total obs=4

    Obs     DATE    TIME      TEAM      RATE

     1     22122      1     Arsenal      2.0
     2     22122      2     Watford      3.6
     3     22122      1     Burnley      2.6
     4     22122      2     Brighton     2.8


                  Variables in Creation Order

    #    Variable    Type    Len    Format

    1    DATE        Num       8    YYMMDDD10.
    2    TIME        Num       8    8.
    3    TEAM        Char      8
    4    RATE        Num       8

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    NOTE: CALL EXECUTE generated line.
    1    + data work.want
    2    + ( keep=date time team rate );
    3    + set work.haveX;
    4    + informat time 8. ;
    5    + format time 8. ;
    6    + length TEAM $ 8 ;
    7    + TEAM=TEAM1;
    8    + length RATE 8 ;
    9    + RATE=RATE1;
    10   + if (not missing(TEAM)) or (not missing(RATE)) then do;
    11   + time=1;
    12   + output;
    13   + end;
    14   + TEAM=TEAM2;
    15   + RATE=RATE2;
    16   + if (not missing(TEAM)) or (not missing(RATE)) then do;
    17   + time=2;
    18   + output;
    19   + end;
    20   + run;


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
