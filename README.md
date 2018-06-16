# utl_refer_to_a_macrolist_which_name_depends_on_a_variable
Refer to a macrolist which name depends on a variable.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
     Refer to a macrolist which name depends on a variable

     Nice examplle of symget?

     Same result in WPS and SAS

     github
     https://tinyurl.com/ycyd5squ
     https://github.com/rogerjdeangelis/utl_refer_to_a_macrolist_which_name_depends_on_a_variable

     see
     https://stackoverflow.com/questions/50872257/refer-to-a-macrolist-which-name-depends-on-a-variable

     Josh profile
     https://tinyurl.com/y8zfxgsu
     https://stackoverflow.com/users/9098731/josh


    INPUT
    =====

      %let list_2=a,b,c;
      %let list_3=d,e,f;

        LIST_2=a,b,c
        LIST_3=d,e,f

                             |  RULES
      WORK.HAVE total obs=4  |
                             |
       BIN    RANK           |  Anser
                             |
        a       2            |   1  --> a is in list_(rank) = list_2
        a       3            |   0  --> a is NOT in list_(rank) = list_3
        e       2            |   0  --> e is NOT in list_(rank) = list_2
        e       3            |   1  --> e is in list_(rank) = list_2
                             |

    EXAMPLE OUTPUT
    ==============

      BIN    RANK    TEST    SYMGETC

       a       2       1      a,b,c   a is in symgetc
       a       3       0      d,e,f
       e       2       0      a,b,c
       e       3       1      d,e,f


    PROCESS
    =======

     proc sql ;
       create
         table want as
       select
         bin
        ,rank
        /*  look for bin(ie "a") in list_(rank) ie("a,b,c") if preset set to 1 */
        ,case when findw(symget(cats("list_",rank))
                        ,bin
                        ) then 1
         else 0
         end as test
        ,symget(cats("list_",rank)) as symgetc /*debugging*/
        from have ;
     quit ;


    OUTPUT;

     WORK.WANT total obs=4

       BIN    RANK    TEST    SYMGETC

        a       2       1      a,b,c
        a       3       0      d,e,f
        e       2       0      a,b,c
        e       3       1      d,e,f

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    %put &=list_2;
    %put &=list_3;

    data have ;
      input bin $1. rank ;
      cards4 ;
    a 2
    a 3
    e 2
    e 3
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;
    * for SAS solution see process;

    * you may have to run this using the WPS eclipse interface;
    %utl_submit_wps64('
     libname wrk sas7bdat "%sysfunc(pathname(work))";
     %let list_2=a,b,c;
     %let list_3=d,e,f;
     proc sql ;
       select
         bin
        ,rank
        ,case when findw(symget(cats("list_",rank))
                        ,bin
                        ) then 1
         else 0
         end as test
        ,symget(cats("list_",rank)) as symgetc
        from wrk.have ;
     quit ;
    run;quit;
    ');

