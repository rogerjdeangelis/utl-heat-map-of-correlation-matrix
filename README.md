# utl-heat-map-of-correlation-matrix
Heat map of correlation matrix.
    Heat map of correlation matrix (SAS/IML/R)

    Output graph
    https://tinyurl.com/y948bqwy
    https://github.com/rogerjdeangelis/utl-heat-map-of-correlation-matrix/blob/master/utl-heat-map-of-correlation-matrix.pdf

    github
    https://github.com/rogerjdeangelis/utl-heat-map-of-correlation-matrix

    SAS forum
    https://tinyurl.com/ycleqebp
    https://communities.sas.com/t5/Graphics-Programming/creating-heat-map-of-correlation-matrix/m-p/502349

    Source
    https://tinyurl.com/ya5m8lg9
    http://www.sthda.com/english/wiki/ggplot2-quick-correlation-matrix-heatmap-r-software-and-data-visualization


    INPUT
    =====

     SD1.HAVE total obs=428
                                               MPG_
      ENGINESIZE    HORSEPOWER    MPG_CITY    HIGHWAY    WEIGHT    LENGTH

          3.5           265          17          23       4451       189
          2.0           200          24          31       2778       172
          2.4           200          22          29       3230       183
          3.2           270          20          28       3575       186
          3.5           225          18          24       3880       197
          3.5           225          18          24       3893       197
    ....


    PROCESS
    =======

    %utlfkil(d:/pdf/utl-heat-map-of-correlation-matrix.pdf);

    %utl_submit_r64('
    library(reshape2);
    library(ggplot2);
    library(haven);
    get_upper_tri <- function(cormat){
        cormat[lower.tri(cormat)]<- NA;
        return(cormat)
    };

    have<-read_sas("d:/sd1/have.sas7bdat");

    mydata <- read_sas("d:/sd1/have.sas7bdat");
    cormat <- round(cor(mydata),2);
    upper_tri <- get_upper_tri(cormat);
    melted_cormat <- melt(upper_tri, na.rm = TRUE);

    pdf(file="d:/pdf/utl-heat-map-of-correlation-matrix.pdf");

    ggplot(data = melted_cormat, aes(Var2, Var1, fill = value))+
      geom_tile(color = "white")+
      scale_fill_gradient2(low = "blue", high = "red", mid = "white",
      midpoint = 0, limit = c(-1,1), space = "Lab", name="Pearson\nCorrelation") +
      theme_minimal()+
      theme(axis.text.x = element_text(angle = 45, vjust = 1, size = 12, hjust = 1))+
      coord_fixed();

    dev.off();
    ');

    OUTPUT
    ======

    https://tinyurl.com/y948bqwy

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
        set sashelp.cars(
          keep=
            MPG_CITY
            MPG_HIGHWAY
            HORSEPOWER
            ENGINESIZE
            WEIGHT
            LENGTH
          rename=(
            MPG_CITY    =  mpgc
            MPG_HIGHWAY =  mpgh
            HORSEPOWER  =  hp
            ENGINESIZE  =  eng
            WEIGHT      =  wt
            LENGTH      =  len)
         )
        ;
    run;quit;

