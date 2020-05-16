# utl-simple-datastep-replacement-for-fcmp-use-stored-program-
In a phase 1 clinical trial convert fahrenheit to celsius in all cdisc tables
    Simple datastep replacement for fcmp use stored program                                 
                                                                                            
    In a phase 1 clinical trial convert fahrenheit to celsius in all cdisc tables           
                                                                                            
    This is an academic exercise, there are perfomance issues.                              
                                                                                            
       Method                                                                               
                                                                                            
         1. Pass fahenheit temp to the stored program using macro variable temp.            
         2. Stored program converts temp in a macro                                         
            variable value from parent to celsius                                           
                                                                                            
                                                                                            
    With feasibility help                                                                   
    Richard DeVenezia                                                                       
    rdevenezia@gmail.com                                                                    
                                                                                            
    Simple datastep replacement for fcmp                                                    
    There are performance issues but SAS can fix that.                                      
    Like SAS does with FCMP.                                                                
                                                                                            
    Less is more, just enhance base and deprecate FCMP                                      
    and a massive number of other functionality                                             
                                                                                            
    *_                   _                                                                  
    (_)_ __  _ __  _   _| |_                                                                
    | | '_ \| '_ \| | | | __|                                                               
    | | | | | |_) | |_| | |_                                                                
    |_|_| |_| .__/ \__,_|\__|                                                               
            |_|                                                                             
    ;                                                                                       
                                                                                            
    data have;                                                                              
     input pid trt$ trtcd sex$ temp;                                                        
    cards4;                                                                                 
    1 Placebo 2 Male 96                                                                     
    2 Placebo 2 Female 99                                                                   
    3 Placebo 1 Female 96                                                                   
    4 Placebo 1 Male 95                                                                     
    5 Placebo 1 Male 100                                                                    
    6 Aspirin 2 Female 94                                                                   
    7 Aspirin 2 Female 95                                                                   
    8 Aspirin 2 Female 99                                                                   
    9 Placebo 1 Female 101                                                                  
    10 Placebo 2 Male 96                                                                    
    ;;;;                                                                                    
    run;quit;                                                                               
                                                                                            
    WORK.HAVE total obs=10                                                                  
                                                                                            
      PID      TRT      TRTCD     SEX      TEMP                                             
                                                                                            
        1    Placebo      2      Male        96                                             
        2    Placebo      2      Female      99                                             
        3    Placebo      1      Female      96                                             
        4    Placebo      1      Male        95                                             
        5    Placebo      1      Male       100                                             
        6    Aspirin      2      Female      94                                             
        7    Aspirin      2      Female      95                                             
        8    Aspirin      2      Female      99                                             
        9    Placebo      1      Female     101                                             
       10    Placebo      2      Male        96                                             
                                                                                            
    *            _               _                                                          
      ___  _   _| |_ _ __  _   _| |_                                                        
     / _ \| | | | __| '_ \| | | | __|                                                       
    | (_) | |_| | |_| |_) | |_| | |_                                                        
     \___/ \__,_|\__| .__/ \__,_|\__|                                                       
                    |_|                                                                     
    ;                                                                                       
                                                                                            
     WORK.WANT total obs=10                                                                 
                                                                                            
      PID      TRT      TRTCD     SEX      TEMP                                             
                                                                                            
        1    Placebo      2      Male       36  ==> Temp is now celsius                     
        2    Placebo      2      Female     37                                              
        3    Placebo      1      Female     36                                              
        4    Placebo      1      Male       35                                              
        5    Placebo      1      Male       38                                              
        6    Aspirin      2      Female     34                                              
        7    Aspirin      2      Female     35                                              
        8    Aspirin      2      Female     37                                              
        9    Placebo      1      Female     38                                              
       10    Placebo      2      Male       36                                              
                                                                                            
    *                                                                                       
     _ __  _ __ ___   ___ ___  ___ ___                                                      
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                     
    | |_) | | | (_) | (_|  __/\__ \__ \                                                     
    | .__/|_|  \___/ \___\___||___/___/                                                     
    |_|                                                                                     
    ;                                                                                       
                                                                                            
    libname sto "d:/sto";                                                                   
                                                                                            
    data sto.f2c/ pgm=sto.f2c;                                                              
       celsius=round((symgetn('temp')-32)*5/9);                                             
       call symputx('temp',celsius);                                                        
    run;quit;                                                                               
                                                                                            
    data want;                                                                              
                                                                                            
      set have;                                                                             
                                                                                            
      call symputx('temp',temp);                                                            
                                                                                            
      rc=dosubl('                                                                           
         data pgm=sto.f2c;                                                                  
         run;quit;                                                                          
      ');                                                                                   
                                                                                            
      temp=symgetn('temp');                                                                 
                                                                                            
      drop rc;                                                                              
                                                                                            
    run;quit;                                                                               
                                                                                            
