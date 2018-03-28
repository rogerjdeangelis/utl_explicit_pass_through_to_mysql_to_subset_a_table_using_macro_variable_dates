# utl_explicit_pass_through_to_mysql_to_subset_a_table_using_macro_variable_dates
Explicit Pass Through to mySQL to Subset a table using macro variable dates. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Explicit Pass Through to mySQL to Subset a table using macro variable dates

    Basically just pull the record where payment_date is 2005-06-16 15:18:57

    github
    https://tinyurl.com/y6vxfq63
    https://github.com/rogerjdeangelis/utl_explicit_pass_through_to_mysql_to_subset_a_table_using_macro_variable_dates

    mySQL is quite similar to SQL Server

    How to convert mySQL code to Sql Server
    http://www.sqlines.com/sql-server-to-mysql/functions/convert_string

    see
    SAS forum
    https://communities.sas.com/t5/Base-SAS-Programming/Explicit-Pass-Through-SQL-Error/m-p/449315

      Two Solutions  (Win7 64bit SAS 64bit 9.4M2

          1. Implicit libname engine
          2. Explicit Passthru


    INPUT
    =====

      Sakila database payment table which is installed  with mySQL

      %let qTym='2005-06-16 15:18:57';  * identifies the obsevation we want in mySQL format;

    PROCESS and OUTPUT
    ==================

     1. Implicit libname engine

        libname mysqllib mysql user=root password="xxxxxxxx" database=sakila;

        proc print data=MYSQLLIB.payment(where=(payment_date=&qTym.dt));
           format payment_date datetime20.;
           var payment_id rental_id payment_date;
        run;quit;

              PAYMENT_
        Obs       ID         RENTAL_ID            PAYMENT_DATE
          1          6            1725      16JUN2005:15:18:57

     2. Explicit Passthru

        proc sql;
             connect to mysql ( user=root password="xxxxxxxx" database=sakila);
             create
                table want as
             select
                 *
             from
                 connection to mysql
                 (select
                       payment_id
                      ,rental_id
                      ,timestamp(&qTym)                         as tymStmpSql
                      ,DATE_FORMAT(payment_date, '%Y-%m-%d %T') as tymFmt
                      ,payment_date
                  from
                      payment
                  where
                      DATE_FORMAT(payment_date, '%Y-%m-%d %T') = &qTym
                 );
             disconnect from mysql;
          quit;


       WORK.WANT total obs=1

       PAYMENT_    RENTAL_                                          PAYMENT_
          ID          ID      TYMSTMPSQL          TYMFMT              DATE

           6         1725     1434554337    2005-06-16 15:18:57    1434554337


