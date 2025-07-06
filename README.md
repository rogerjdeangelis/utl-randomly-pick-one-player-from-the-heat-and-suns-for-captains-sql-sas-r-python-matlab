# utl-randomly-pick-one-player-from-the-heat-and-suns-for-captains-sql-sas-r-python-matlab
Randomly pick one player from the heat and suns for captains sql sas r python matlab
    %let pgm=utl-randomly-pick-one-player-from-the-heat-and-suns-for-captains-sql-sas-r-python-matlab;

    %stop_submission;

    Randomly pick one player from the heat and suns for captains sql sas r python matlab;

    CONTENTS

     1 sas sql
     2 r sql
     3 python sql
     4 excel r sql
     5 matlab-octave sql
       1 octave read sqlite table
       2 octave run sql code
       3 octave create sqlite table want
       4 sas read sqlite
     UPDATED (see repofiles)
       5 sas create sas dataset inferred data type
       6 sas create sas dataset sqlite data types

    github
    https://tinyurl.com/bded7jc6
    https://github.com/rogerjdeangelis/utl-randomly-pick-one-player-from-the-heat-and-suns-for-captains-sql-sas-r-python-matlab

    communities.sas.com
    https://tinyurl.com/bdead4sx
    https://communities.sas.com/t5/SAS-Programming/How-to-Randomly-Pick-a-Value-from-Other-Table/m-p/970012#M376988

    SOAPBOX ON

    Sqlite and other sql dialects, tend to be a little more
    complicated becuase ,unlike sas, other sqlite dialects do not do automatic remerging

    NOTE: The query requires remerging summary statistics back with the original data.

    SOAPBOX OFF


    /**************************************************************************************************************************/
    /* INPUT                                  | PROCESS                                        |  OUTPUT                      */
    /* =====                                  | =======                                        |  ======                      */
    /* SD1.PLAYERS                            | 1 SAS SQL                                      |  intermediate rndOne         */
    /*                                        | =========                                      |  random selections           */
    /* TEAM    PLAYER                         | proc sql;                                      |                              */
    /*                                        |   create                                       |  TEAM PLAYER   RAN           */
    /* HEAT    ADEBAYO                        |     table rndOne as                            |                              */
    /* HEAT    BUTLER                         |   select                                       |  HEAT HERRO  0.73728         */
    /* HEAT    HERRO                          |     *                                          |  SUNS BOOKER 0.60532         */
    /* SUNS    BOOKER                         |   from                                         |                              */
    /* SUNS    ALLE                           |     (select                                    |  JOIN WITH TEAMS             */
    /* SUNS    BOL                            |       team                                     |                              */
    /*                                        |      ,player                                   |  TEAM    PLAYER              */
    /*                                        |      ,uniform(357) as ran                      |                              */
    /* SD1.TEAMS                              |     from                                       |  HEAT    HERRO               */
    /*                                        |       sd1.players)                             |  JAZZ                        */
    /* TEAM                                   |   group                                        |  SUNS    BOOKER              */
    /*                                        |     by team                                    |                              */
    /* HEAT                                   |   having                                       |                              */
    /* SUNS                                   |     ran = max(ran)                             |                              */
    /* JAZZ                                   | ;                                              |                              */
    /*                                        |   create                                       |                              */
    /* options validvarname=upcase;           |     table want as                              |                              */
    /* libname sd1 "d:/sd1";                  |   select                                       |                              */
    /* data sd1.players;                      |     l.team                                     |                              */
    /* input  team$ player$;                  |    ,r.player                                   |                              */
    /* cards4;                                |   from                                         |                              */
    /* HEAT ADEBAYO                           |     sd1.teams as l left join rndOne as r       |                              */
    /* HEAT BUTLER                            |   on                                           |                              */
    /* HEAT HERRO                             |     l.team = r.team                            |                              */
    /* SUNS BOOKER                            |   order                                        |                              */
    /* SUNS ALLEN                             |     by team                                    |                              */
    /* SUNS BOL                               | ;quit;                                         |                              */
    /* ;;;;                                   |                                                |                              */
    /* run;quit;                              |-------------------------------------------------------------------------------*/
    /*                                        |                                                |                              */
    /* options validvarname=upcase;           | 2 R SQL                                        |  > "want"                    */
    /* libname sd1 "d:/sd1";                  | =======                                        |                              */
    /* data sd1.teams;                        |                                                |    TEAM player               */
    /* input team$;                           | proc datasets lib=sd1 nolist nodetails;        |  1 HEAT  HERRO               */
    /* cards;                                 |  delete want;                                  |  2 JAZZ   <NA>               */
    /* HEAT                                   | run;quit;                                      |  3 SUNS BOOKER               */
    /* SUNS                                   |                                                |                              */
    /* JAZZ                                   | %utl_rbeginx;                                  |  SAS                         */
    /* ;;;;                                   | parmcards4;                                    |  TEAM    PLAYER              */
    /* run;quit;                              | library(haven)                                 |                              */
    /*                                        | library(sqldf)                                 |  HEAT    ADEBAYO             */
    /*                                        | source("c:/oto/fn_tosas9x.r")                  |  JAZZ                        */
    /*                                        | options(sqldf.dll = "d:/dll/sqlean.dll")       |  SUNS    BOL                 */
    /*                                        | teams<-read_sas("d:/sd1/teams.sas7bdat")       |                              */
    /*                                        | players<-read_sas("d:/sd1/players.sas7bdat")   |                              */
    /*                                        | print(players)                                 |                              */
    /*                                        | print(teams)                                   |                              */
    /*                                        | want<-sqldf('                                  |                              */
    /*                                        | with rndone as (                               |                              */
    /*                                        | select                                         |                              */
    /*                                        |    team                                        |                              */
    /*                                        |   ,player                                      |                              */
    /*                                        | from (                                         |                              */
    /*                                        |   select                                       |                              */
    /*                                        |     team,                                      |                              */
    /*                                        |     player,                                    |                              */
    /*                                        |     random() as ran,                           |                              */
    /*                                        |     row_number() over                          |                              */
    /*                                        |       (partition by team                       |                              */
    /*                                        |        order by random()) as rn                |                              */
    /*                                        |   from players                                 |                              */
    /*                                        |   )                                            |                              */
    /*                                        | where rn = 1)                                  |                              */
    /*                                        | select                                         |                              */
    /*                                        |   l.team                                       |                              */
    /*                                        |  ,r.player                                     |                              */
    /*                                        | from                                           |                              */
    /*                                        |   teams as l left join rndone as r             |                              */
    /*                                        | on                                             |                              */
    /*                                        |   l.team = r.team                              |                              */
    /*                                        | order                                          |                              */
    /*                                        |   by l.team                                    |                              */
    /*                                        | ')                                             |                              */
    /*                                        | "want"                                         |                              */
    /*                                        | want                                           |                              */
    /*                                        | fn_tosas9x(                                    |                              */
    /*                                        |       inp    = want                            |                              */
    /*                                        |      ,outlib ="d:/sd1/"                        |                              */
    /*                                        |      ,outdsn ="want"                           |                              */
    /*                                        |      )                                         |                              */
    /*                                        | ;;;;                                           |                              */
    /*                                        | %utl_rendx;                                    |                              */
    /*                                        |                                                |                              */
    /*                                        | proc print data=sd1.pywant;                    |                              */
    /*                                        | run;quit;                                      |                              */
    /*                                        |                                                |                              */
    /*                                        |-------------------------------------------------------------------------------*/
    /*                                        | 3 PYTHON SQL                                   |     TEAM player              */
    /*                                        | ============                                   |                              */
    /*                                        | proc datasets lib=sd1 nolist nodetails;        |  0  HEAT  HERRO              */
    /*                                        |  delete pywant;                                |  1  JAZZ   None              */
    /*                                        | run;quit;                                      |  2  SUNS    BOL              */
    /*                                        |                                                |                              */
    /*                                        | %utl_pybeginx;                                 |                              */
    /*                                        | parmcards4;                                    |                              */
    /*                                        | exec(open('c:/oto/fn_pythonx.py').read());     |                              */
    /*                                        | teams,meta \                                   |                              */
    /*                                        |  =ps.read_sas7bdat('d:/sd1/teams.sas7bdat');   |                              */
    /*                                        | players,meta \                                 |                              */
    /*                                        |  =ps.read_sas7bdat('d:/sd1/players.sas7bdat'); |                              */
    /*                                        | want=pdsql('''                                 |                              */
    /*                                        | with rndone as (                               |                              */
    /*                                        | select                                         |                              */
    /*                                        |    team                                        |                              */
    /*                                        |   ,player                                      |                              */
    /*                                        | from (                                         |                              */
    /*                                        |   select                                       |                              */
    /*                                        |     team,                                      |                              */
    /*                                        |     player,                                    |                              */
    /*                                        |     random() as ran,                           |                              */
    /*                                        |     row_number() over (                        |                              */
    /*                                        |      partition by team                         |                              */
    /*                                        |      order by random()) as rn                  |                              */
    /*                                        |   from players                                 |                              */
    /*                                        |   )                                            |                              */
    /*                                        | where rn = 1)                                  |                              */
    /*                                        | select                                         |                              */
    /*                                        |   l.team                                       |                              */
    /*                                        |  ,r.player                                     |                              */
    /*                                        | from                                           |                              */
    /*                                        |   teams as l left join rndone as r             |                              */
    /*                                        | on                                             |                              */
    /*                                        |   l.team = r.team                              |                              */
    /*                                        | order                                          |                              */
    /*                                        |   by l.team                                    |                              */
    /*                                        |    ''');                                       |                              */
    /*                                        | print(want);                                   |                              */
    /*                                        | fn_tosas9x(want                                |                              */
    /*                                        |  ,outlib='d:/sd1/'                             |                              */
    /*                                        |  ,outdsn='pywant'                              |                              */
    /*                                        |  ,timeest=3);                                  |                              */
    /*                                        | ;;;;                                           |                              */
    /*                                        | %utl_pyendx;                                   |                              */
    /*                                        |                                                |                              */
    /*                                        | proc print data=sd1.pywant;                    |                              */
    /*                                        | run;quit;                                      |                              */
    /*                                        |                                                |                              */
    /*                                        |-------------------------------------------------------------------------------*/
    /*                                        |                                                |                              */
    /*                                        | 4  EXCEL R SQL                                 |   --------------------+      */
    /*                                        | ==============                                 |   | A1| fx    |TEAM   |      */
    /*                                        |                                                |   --------------------+      */
    /*                                        | %utlfkil(d:/xls/wantxl.xlsx);                  |   [_] |    A |    B   |      */
    /*                                        |                                                |   --------------------|      */
    /*                                        | %utl_rbeginx;                                  |    1  | TEAM | PLAYER |      */
    /*                                        | parmcards4;                                    |    -- |------+--------|      */
    /*                                        | library(openxlsx)                              |    2  |  HEAT| HERRO  |      */
    /*                                        | library(sqldf)                                 |    -- |------+--------|      */
    /*                                        | library(haven)                                 |    3  |  JAZZ|        |      */
    /*                                        | players<-read_sas("d:/sd1/players.sas7bdat")   |    -- |------+--------|      */
    /*                                        | teams<-read_sas("d:/sd1/teams.sas7bdat")       |    4  |  SUNS| BOOKER |      */
    /*                                        | wb <- createWorkbook()                         |    -- |------+--------|      */
    /*                                        | addWorksheet(wb, "players")                    |   [WANT]                     */
    /*                                        | writeData(wb,sheet="players",x=players)        |                              */
    /*                                        | addWorksheet(wb,"teams")                       |                              */
    /*                                        | writeData(wb,sheet="teams",x=teams)            |                              */
    /*                                        | saveWorkbook(                                  |                              */
    /*                                        |     wb                                         |                              */
    /*                                        |    ,"d:/xls/wantxl.xlsx"                       |                              */
    /*                                        |    ,overwrite=TRUE)                            |                              */
    /*                                        | ;;;;                                           |                              */
    /*                                        | %utl_rendx;                                    |                              */
    /*                                        |                                                |                              */
    /*                                        | %utl_rbeginx;                                  |                              */
    /*                                        | parmcards4;                                    |                              */
    /*                                        | library(openxlsx)                              |                              */
    /*                                        | library(sqldf)                                 |                              */
    /*                                        | source("c:/oto/fn_tosas9x.R")                  |                              */
    /*                                        |  wb<-loadWorkbook("d:/xls/wantxl.xlsx")        |                              */
    /*                                        |  players<-read.xlsx(wb,"players")              |                              */
    /*                                        |  teams<-read.xlsx(wb,"teams")                  |                              */
    /*                                        |  addWorksheet(wb, "want")                      |                              */
    /*                                        |  want <- sqldf('                               |                              */
    /*                                        |  with rndone as (                              |                              */
    /*                                        |  select                                        |                              */
    /*                                        |     team                                       |                              */
    /*                                        |    ,player                                     |                              */
    /*                                        |  from (                                        |                              */
    /*                                        |    select                                      |                              */
    /*                                        |      team,                                     |                              */
    /*                                        |      player,                                   |                              */
    /*                                        |      random() as ran,                          |                              */
    /*                                        |      row_number() over (                       |                              */
    /*                                        |       partition by team                        |                              */
    /*                                        |       order by random()) as rn                 |                              */
    /*                                        |    from players                                |                              */
    /*                                        |    )                                           |                              */
    /*                                        |  where rn = 1)                                 |                              */
    /*                                        |  select                                        |                              */
    /*                                        |    l.team                                      |                              */
    /*                                        |   ,r.player                                    |                              */
    /*                                        |  from                                          |                              */
    /*                                        |    teams as l left join rndone as r            |                              */
    /*                                        |  on                                            |                              */
    /*                                        |    l.team = r.team                             |                              */
    /*                                        |  order                                         |                              */
    /*                                        |    by l.team                                   |                              */
    /*                                        |                                                |                              */
    /*                                        |    ')                                          |                              */
    /*                                        |  print(want)                                   |                              */
    /*                                        |  writeData(wb,sheet="want",x=want)             |                              */
    /*                                        |  saveWorkbook(                                 |                              */
    /*                                        |      wb                                        |                              */
    /*                                        |     ,"d:/xls/wantxl.xlsx"                      |                              */
    /*                                        |     ,overwrite=TRUE)                           |                              */
    /*                                        | fn_tosas9x(                                    |                              */
    /*                                        |       inp    = want                            |                              */
    /*                                        |      ,outlib ="d:/sd1/"                        |                              */
    /*                                        |      ,outdsn ="want"                           |                              */
    /*                                        |      )                                         |                              */
    /*                                        | ;;;;                                           |                              */
    /*                                        | %utl_rendx;                                    |                              */
    /*                                        |                                                |                              */
    /*                                        | proc print data=sd1.want;                      |                              */
    /*                                        | run;quit;                                      |                              */
    /*                                        |                                                |                              */
    /*                                        |                                                |                              */
    /*                                        |-------------------------------------------------------------------------------*/
    /*                                        |                                                |                              */
    /* * LOAD SAS DATASET INTO SQLITE         | 5 MATLAB-OCTAVE SQL                            | CONTENTS SQLITE TABLE WANT   */
    /*                                        | ===================                            |                   not        */
    /* > dbGetQuery(                          |                                                | cid name   type  null dfl pk */
    /*  con                                   | 1 octave read sqlite table                     | ___ ______ ____  ____ ___ __ */
    /* "SELECT                                | 2 octave run sql code                          |                              */
    /*    *                                   | 3 octave create sqlite table want              | 0   TEAM   TEXT    0      0  */
    /*  FROM                                  | 4 sas read sqlite                              | 1   player TEXT    0      0  */
    /*    players")                           | 5 sas create sas dataset inferred data type    |                              */
    /*                                        | 6 sas create sas dataset sqlite data types     |                              */
    /*   TEAM  PLAYER                         |                                                |                              */
    /*                                        | %utl_mbegin;                                   |  SQLITE TABLE WANT           */
    /* 1 HEAT ADEBAYO                         | parmcards4;                                    |                              */
    /* 2 HEAT  BUTLER                         | pkg load database                              |  TEAM  player                */
    /* 3 HEAT   HERRO                         | pkg load sqlite                                |  ____  ______                */
    /* 4 SUNS  BOOKER                         | pkg load io                                    |                              */
    /* 5 SUNS    ALLE                         | pkg load tablicious                            |  HEAT  HERRO                 */
    /* 6 SUNS     BOL                         | db = sqlite("d:/sqlite/have.db");              |  JAZZ                        */
    /*                                        | execute(db, 'drop table if exists want')       |  SUNS  BOL                   */
    /* > dbGetQuery(con                       | execute(db                      ...            |                              */
    /* "SELECT                                |  ,[" create                              " ... |                              */
    /*   *                                    |    "  table want as                      " ... |  SAS TABLE                   */
    /* FROM                                   |    "  with rndone as (                   " ... |                              */
    /*  pragma_table_info('players')          |    "  select                             " ... |   TEAM    PLAYER             */
    /*                    not                 |    "     team                            " ... |                              */
    /*   cid   name type null dflt pk         |    "    ,player                          " ... |   HEAT    HERRO              */
    /*                                        |    "  from (                             " ... |   JAZZ                       */
    /* 1   0   TEAM TEXT  0    NA  0          |    "    select                           " ... |   SUNS    BOOKER             */
    /* 2   1 PLAYER TEXT  0    NA  0          |    "      team,                          " ... |                              */
    /*                                        |    "      player,                        " ... |                              */
    /*                                        |    "      row_number() over              " ... |                              */
    /* %utlfkil(d:/sqlite/have.db);           |    "       (partition by team            " ... |                              */
    /*                                        |    "       order by random()) as rn      " ... |                              */
    /* %utl_rbeginx;                          |    "    from players                     " ... |                              */
    /* parmcards4;                            |    "    )                                " ... |                              */
    /* library(haven)                         |    "  where rn = 1)                      " ... |                              */
    /* library(DBI)                           |    "  select                             " ... |                              */
    /* library(RSQLite)                       |    "    l.team                           " ... |                              */
    /* teams<-read_sas(                       |    "   ,r.player                         " ... |                              */
    /*  "d:/sd1/teams.sas7bdat")              |    "  from                               " ... |                              */
    /* players<-read_sas(                     |    "    teams as l left join rndone as r " ... |                              */
    /*  "d:/sd1/players.sas7bdat")            |    "  on                                 " ... |                              */
    /* con <- dbConnect(                      |    "    l.team = r.team                  " ... |                              */
    /*     RSQLite::SQLite()                  |    "  order                              " ... |                              */
    /*    ,"d:/sqlite/have.db")               |    "    by l.team                        " ... |                              */
    /* dbWriteTable(                          | ]);                                            |                              */
    /*     con                                |                                                |                              */
    /*   ,"players"                           |                                                |                              */
    /*   ,players)                            | % check sqlite result                          |                              */
    /* dbWriteTable(                          | dic=fetch(db,"PRAGMA table_info('want');")     |                              */
    /*     con                                | chk=fetch(db,"select * from want")             |                              */
    /*   ,"teams"                             | close(db)                                      |                              */
    /*   ,teams)                              | ;;;;                                           |                              */
    /* dbListTables(con)                      | %utl_mend;                                     |                              */
    /* dbGetQuery(                            |                                                |                              */
    /*    con                                 | *CHECK MACROS ;                                |                              */
    /*  ,"SELECT                              |                                                |                              */
    /*      *                                 | %inc "c:/oto/sqlitex.sas";                     |                              */
    /*    FROM                                | %inc "c:/oto/sqlitet.sas";                     |                              */
    /*      players")                         |                                                |                              */
    /* dbGetQuery(con                         | * INFERRED TYPES;                              |                              */
    /*  ,"SELECT                              | %sqlitex(                                      |                              */
    /*     *                                  |       dbpath = d:/sqlite/have.db               |                              */
    /*   FROM                                 |      ,inp    = want                            |                              */
    /*    pragma_table_info('players')")      |      ,out    = wantx                           |                              */
    /* dbDisconnect(con)                      |    )                                           |                              */
    /* ;;;;                                   |                                                |                              */
    /* %utl_rendx;                            | * SQLITE TYPES;                                |                              */
    /*                                        | %sqlitet(                                      |                              */
    /*                                        |       dbpath = d:/sqlite/have.db               |                              */
    /*                                        |      ,inp    = want                            |                              */
    /*                                        |      ,out    = wantt                           |                              */
    /*                                        |    )                                           |                              */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.players;
    input  team$ player$;
    cards4;
    HEAT ADEBAYO
    HEAT BUTLER
    HEAT HERRO
    SUNS BOOKER
    SUNS ALLEN
    SUNS BOL
    ;;;;
    run;quit;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.teams;
    input team$;
    cards;
    HEAT
    SUNS
    JAZZ
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                    |                                                                                                   */
    /*  SD1.PLAYERS       |    SD1.TEAMS                                                                                      */
    /*                    |                                                                                                   */
    /*  TEAM    PLAYER    |    TEAM                                                                                           */
    /*                    |                                                                                                   */
    /*  HEAT    ADEBAYO   |    HEAT                                                                                           */
    /*  HEAT    BUTLER    |    SUNS                                                                                           */
    /*  HEAT    HERRO     |    JAZZ                                                                                           */
    /*  SUNS    BOOKER    |                                                                                                   */
    /*  SUNS    ALLE      |                                                                                                   */
    /*  SUNS    BOL       |                                                                                                   */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    proc sql;
      create
        table rndOne as
      select
        *
      from
        (select
          team
         ,player
         ,uniform(357) as ran
        from
          sd1.players)
      group
        by team
      having
        ran = max(ran)
    ;
      create
        table want as
      select
        l.team
       ,r.player
      from
        sd1.teams as l left join rndOne as r
      on
        l.team = r.team
      order
        by team
    ;quit;

    /**************************************************************************************************************************/
    /* INTERMEDIATE RNDONE                                                                                                    */
    /* RANDOM SELECTIONS                                                                                                      */
    /*                                                                                                                        */
    /* TEAM PLAYER   RAN                                                                                                      */
    /*                                                                                                                        */
    /* HEAT HERRO  0.73728                                                                                                    */
    /* SUNS BOOKER 0.60532                                                                                                    */
    /*                                                                                                                        */
    /* FINAL WANT DATASET                                                                                                     */
    /*                                                                                                                        */
    /* JOIN WITH TEAMS                                                                                                        */
    /*                                                                                                                        */
    /* TEAM    PLAYER                                                                                                         */
    /*                                                                                                                        */
    /* HEAT    HERRO                                                                                                          */
    /* JAZZ                                                                                                                   */
    /* SUNS    BOOKER                                                                                                         */
    /**************************************************************************************************************************/

    /*                _
     _ __   ___  __ _| |
    | `__| / __|/ _` | |
    | |    \__ \ (_| | |
    |_|    |___/\__, |_|
                   |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.r")
    options(sqldf.dll = "d:/dll/sqlean.dll")
    teams<-read_sas("d:/sd1/teams.sas7bdat")
    players<-read_sas("d:/sd1/players.sas7bdat")
    print(players)
    print(teams)
    want<-sqldf('
    with rndone as (
    select
       team
      ,player
    from (
      select
        team,
        player,
        random() as ran,
        row_number() over
          (partition by team
           order by random()) as rn
      from players
      )
    where rn = 1)
    select
      l.team
     ,r.player
    from
      teams as l left join rndone as r
    on
      l.team = r.team
    order
      by l.team
    ')
    "want"
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*   R              |  SAS                                                                                                */
    /*     TEAM player  |  TEAM    PLAYER                                                                                     */
    /*                  |                                                                                                     */
    /*   1 HEAT  HERRO  |  HEAT    ADEBAYO                                                                                    */
    /*   2 JAZZ   <NA>  |  JAZZ                                                                                               */
    /*   3 SUNS BOOKER  |  SUNS    BOL                                                                                        */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__,_|_|
            |_|    |___/
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_pythonx.py').read());
    teams,meta \
     =ps.read_sas7bdat('d:/sd1/teams.sas7bdat');
    players,meta \
     =ps.read_sas7bdat('d:/sd1/players.sas7bdat');
    want=pdsql('''
    with rndone as (
    select
       team
      ,player
    from (
      select
        team,
        player,
        random() as ran,
        row_number() over (
         partition by team
         order by random()) as rn
      from players
      )
    where rn = 1)
    select
      l.team
     ,r.player
    from
      teams as l left join rndone as r
    on
      l.team = r.team
    order
      by l.team
       ''');
    print(want);
    fn_tosas9x(want
     ,outlib='d:/sd1/'
     ,outdsn='pywant'
     ,timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;


    /**************************************************************************************************************************/
    /*   PYTHON          |  SAS                                                                                               */
    /*      TEAM player  |  TEAM    PLAYER                                                                                    */
    /*                   |                                                                                                    */
    /*   0  HEAT  HERRO  |  HEAT    HERRO                                                                                     */
    /*   1  JAZZ   None  |  JAZZ                                                                                              */
    /*   2  SUNS    BOL  |  SUNS    BOOKER                                                                                    */
    /**************************************************************************************************************************/

    /*  _                       _                    _
    | || |     _____  _____ ___| |  _ __   ___  __ _| |
    | || |_   / _ \ \/ / __/ _ \ | | `__| / __|/ _` | |
    |__   _| |  __/>  < (_|  __/ | | |    \__ \ (_| | |
       |_|    \___/_/\_\___\___|_| |_|    |___/\__, |_|
                                                  |_|
    */

    %utlfkil(d:/xls/wantxl.xlsx);

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    library(haven)
    players<-read_sas("d:/sd1/players.sas7bdat")
    teams<-read_sas("d:/sd1/teams.sas7bdat")
    wb <- createWorkbook()
    addWorksheet(wb, "players")
    writeData(wb,sheet="players",x=players)
    addWorksheet(wb,"teams")
    writeData(wb,sheet="teams",x=teams)
    saveWorkbook(
        wb
       ,"d:/xls/wantxl.xlsx"
       ,overwrite=TRUE)
    ;;;;
    %utl_rendx;

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
     wb<-loadWorkbook("d:/xls/wantxl.xlsx")
     players<-read.xlsx(wb,"players")
     teams<-read.xlsx(wb,"teams")
     addWorksheet(wb, "want")
     want <- sqldf('
     with rndone as (
     select
        team
       ,player
     from (
       select
         team,
         player,
         random() as ran,
         row_number() over (
          partition by team
          order by random()) as rn
       from players
       )
     where rn = 1)
     select
       l.team
      ,r.player
     from
       teams as l left join rndone as r
     on
       l.team = r.team
     order
       by l.team

       ')
     print(want)
     writeData(wb,sheet="want",x=want)
     saveWorkbook(
         wb
        ,"d:/xls/wantxl.xlsx"
        ,overwrite=TRUE)
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
    /* d:/xls/wantxl.xlsx                                                                                                     */
    /*                                                                                                                        */
    /*  --------------------+                                                                                                 */
    /*  | A1| fx    |TEAM   |                                                                                                 */
    /*  --------------------+                                                                                                 */
    /*  [_] |    A |    B   |                                                                                                 */
    /*  --------------------|                                                                                                 */
    /*   1  | TEAM | PLAYER |                                                                                                 */
    /*   -- |------+--------|                                                                                                 */
    /*   2  |  HEAT| HERRO  |                                                                                                 */
    /*   -- |------+--------|                                                                                                 */
    /*   3  |  JAZZ|        |                                                                                                 */
    /*   -- |------+--------|                                                                                                 */
    /*   4  |  SUNS| BOOKER |                                                                                                 */
    /*   -- |------+--------|                                                                                                 */
    /*  [WANT]                                                                                                                */
    /**************************************************************************************************************************/

    /*___                    _   _       _                      _                             _
    | ___|   _ __ ___   __ _| |_| | __ _| |__         ___   ___| |_ __ ___   _____  ___  __ _| |
    |___ \  | `_ ` _ \ / _` | __| |/ _` | `_ \ _____ / _ \ / __| __/ _` \ \ / / _ \/ __|/ _` | |
     ___) | | | | | | | (_| | |_| | (_| | |_) |_____| (_) | (__| || (_| |\ V /  __/\__ \ (_| | |
    |____/  |_| |_| |_|\__,_|\__|_|\__,_|_.__/       \___/ \___|\__\__,_| \_/ \___||___/\__, |_|
     _                   _                                                                   |_|
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(DBI)
    library(RSQLite)
    teams<-read_sas(
     "d:/sd1/teams.sas7bdat")
    players<-read_sas(
     "d:/sd1/players.sas7bdat")
    con <- dbConnect(
        RSQLite::SQLite()
       ,"d:/sqlite/have.db")
    dbWriteTable(
        con
      ,"players"
      ,players)
    dbWriteTable(
        con
      ,"teams"
      ,teams)
    dbListTables(con)
    dbGetQuery(
       con
     ,"SELECT
         *
       FROM
         players")
    dbGetQuery(con
     ,"SELECT
        *
      FROM
       pragma_table_info('players')")
    dbDisconnect(con)
    ;;;;
    %utl_rendx;


    /**************************************************************************************************************************/
    /* * LOAD SAS DATASETS INTO SQLITE |                                                                                       */
    /*                                 |                                                                                       */
    /* PLAYERS                         | TEAMS                                                                                 */
    /*  > dbGetQuery(                  |  > dbGetQuery(con                                                                     */
    /*   con                           |  "SELECT                                                                              */
    /*  "SELECT                        |    *                                                                                  */
    /*     *                           |  FROM                                                                                 */
    /*   FROM                          |   pragma_table_info('players')                                                        */
    /*     players")                   |                     not                                                               */
    /*                                 |    cid   name type null dflt pk                                                       */
    /*                                 |                                                                                       */
    /*                                 |  1   0   TEAM TEXT  0    NA  0                                                        */
    /*                                 |  2   1 PLAYER TEXT  0    NA  0                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|

    MATLAB-OCTAVE SQL PROCESS
    =========================

    1 octave read sqlite table
    2 octave run sql code
    3 octave create sqlite table want
    4 sas read sqlite
    5 sas create sas dataset inferred data type
    6 sas create sas dataset sqlite data types
    */

    %utl_mbegin;
    parmcards4;
    pkg load database
    pkg load sqlite
    pkg load io
    pkg load tablicious
    db = sqlite("d:/sqlite/have.db");
    execute(db, 'drop table if exists want')
    execute(db                      ...
     ,[" create                              " ...
       "  table want as                      " ...
       "  with rndone as (                   " ...
       "  select                             " ...
       "     team                            " ...
       "    ,player                          " ...
       "  from (                             " ...
       "    select                           " ...
       "      team,                          " ...
       "      player,                        " ...
       "      row_number() over              " ...
       "       (partition by team            " ...
       "       order by random()) as rn      " ...
       "    from players                     " ...
       "    )                                " ...
       "  where rn = 1)                      " ...
       "  select                             " ...
       "    l.team                           " ...
       "   ,r.player                         " ...
       "  from                               " ...
       "    teams as l left join rndone as r " ...
       "  on                                 " ...
       "    l.team = r.team                  " ...
       "  order                              " ...
       "    by l.team                        " ...
    ]);


    % check sqlite result
    dic=fetch(db,"PRAGMA table_info('want');")
    chk=fetch(db,"select * from want")
    close(db)
    ;;;;
    %utl_mend;

    *CHECK MACROS ;

    %inc "c:/oto/sqlitex.sas";
    %inc "c:/oto/sqlitet.sas";

    * INFERRED TYPES;
    %sqlitex(
          dbpath = d:/sqlite/have.db
         ,inp    = want
         ,out    = wantx
       )

    * SQLITE TYPES;
    %sqlitet(
          dbpath = d:/sqlite/have.db
         ,inp    = want
         ,out    = wantt
       )


    /**************************************************************************************************************************/
    /* CONTENTS SQLITE TABLE WANT      SQLITE TABLE WANT| SAS TABLE                                                           */
    /*                   not                            |                                                                     */
    /* cid name   type  null dfl pk    TEAM  player     |  TEAM    PLAYER                                                     */
    /* ___ ______ ____  ____ ___ __    ____  ______     |                                                                     */
    /*                                                  |  HEAT    HERRO                                                      */
    /* 0   TEAM   TEXT    0      0     HEAT  HERRO      |  JAZZ                                                               */
    /* 1   player TEXT    0      0     JAZZ             |  SUNS    BOOKER                                                     */
    /*                                 SUNS  BOL        |                                                                     */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
