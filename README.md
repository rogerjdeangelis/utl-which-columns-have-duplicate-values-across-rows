# utl-which-columns-have-duplicate-values-across-rows
Which columns have duplicate values across rows  
    Which columns have duplicate values across rows                                              
                                                                                                 
    *_                   _                                                                       
    (_)_ __  _ __  _   _| |_                                                                     
    | | '_ \| '_ \| | | | __|                                                                    
    | | | | | |_) | |_| | |_                                                                     
    |_|_| |_| .__/ \__,_|\__|                                                                    
            |_|                                                                                  
    ;                                                                                            
                                                                                                 
    options validvarname=upcase;                                                                 
    libname sd1 "d:/sd1";                                                                        
    data sd1.have;                                                                               
      input x1-x13;                                                                              
    cards;                                                                                       
    450 1 9 8 6 0 4 3 0 0 0 0 0                                                                  
    450 1 8 8 5 5 3 3 0 0 0 0 1                                                                  
    ;;;;                                                                                         
    run;quit;                                                                                    
                                                                                                 
                                                                                                 
    SD1.HAVE total obs=2                                                                         
                                                                                                 
      X1    X2    X3    X4    X5    X6    X7    X8    X9    X10    X11    X12    X13             
                                                                                                 
     450     1     9     8     6     0     4     3     0     0      0      0      0              
     450     1     8     8     5     5     3     3     0     0      0      0      1              
                                                                                                 
    RULES                                                                                        
    =====                                                                                        
                                                                                                 
      X1    X2    X3    X4    X5    X6    X7    X8    X9    X10    X11    X12    X13             
                                                                                                 
     450     1     9     8     6     0     4     3     0     0      0      0      0              
     450     1     8     8     5     5     3     3     0     0      0      0      1              
                                                                                                 
    450=450 1=1   9^=8  8=8                                                                      
                                                                                                 
       1     1     0     1     0     0     0     1     1     1      1      1      0              
                                                                                                 
    *            _               _                                                               
      ___  _   _| |_ _ __  _   _| |_                                                             
     / _ \| | | | __| '_ \| | | | __|                                                            
    | (_) | |_| | |_| |_) | |_| | |_                                                             
     \___/ \__,_|\__| .__/ \__,_|\__|                                                            
                    |_|                                                                          
    ;                                                                                            
                                                                                                 
    WORK.WANT total obs=3                                                                        
                                                                                                 
       X1    X2    X3    X4    X5    X6    X7    X8    X9    X10    X11    X12    X13            
                                                                                                 
      450     1     9     8     6     0     4     3     0     0      0      0      0             
      450     1     8     8     5     5     3     3     0     0      0      0      1             
                                                                                                 
        1     1     0     1     0     0     0     1     1     1      1      1      0             
                                                                                                 
    *          _       _   _                                                                     
     ___  ___ | |_   _| |_(_) ___  _ __                                                          
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                         
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                        
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                        
                                                                                                 
    ;                                                                                            
                                                                                                 
    %utl_submit_r64('                                                                            
    library(haven);                                                                              
    library(data.table);                                                                         
    library(SASxport);                                                                           
    have<-read_sas("d:/sd1/have.sas7bdat");                                                      
    want<-as.data.table(rbind(have,1*(have[1,]==have[2,])));                                     
    write.xport(want,file="d:/xpt/want.xpt");                                                    
    ');                                                                                          
                                                                                                 
    libname xpt xport "d:/xpt/want.xpt";                                                         
    data want;                                                                                   
      set xpt.want;                                                                              
    run;quit;                                                                                    
    libname xpt clear;                                                                           
                                                                                                 
                                                                                                 
