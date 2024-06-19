# IBM i JasperReports Report Generation Commands  
This GitHub repository will be used to house a few sample CL commands, Java sample code and Jasper report templates for generating JasperReports from the IBM i command line or as part of an existing application if desired.    

Since 2007 I have done a lot of work building reports and forms using the JasperReports library in conjunction with IBM i, so I can attest to Jasper as a stable Java based report and form creation engine.   

There used to be a community edition of Jasper Server available from JasperSoft for FREE but the that version has been discontinued. But never fear because you can still use the JasperReports library to generate reports without JasperServer. And JasperReports Studio is still FREE and can be used to design and build reports with our without a server. And the reports can be run from IBM i or other platforms.

The IBMIJASPER library packages a forked copy of the Java files that are part of Pete Helgren's Report Generator project.   

The library also contains the `RREGEN` CL command for generating a JasperReport from a Jasper JRXML template file.   

:exclamation: If you haven't used JasperReports before, you may want to reach out for training, consulting and mentoring on getting started with JasperReports on IBM i or other platforms. Or you may need assistance with creating report templates or integrating JasperReports with your IBM i work streams.  

# Training, Support and Consulting for IBMIJASPER and JasperReports
If you desire assistance in setting up the IBMIJASPER library or training to create report and form templates so you can generate reports, forms and data exports from your IBM i system with Jasper Reports, feel free to reach out about consulting assistance.  

Web Site: https://www.mobigogo.net   
Email: richard@mobigogo.net -or- richard@richardschoen.net   

# JasperReports Links
JasperReports is an open-source report generation library for Java that has been available since 2001. Jaspersoft was acquired by Tibco in 2014.   
https://en.wikipedia.org/wiki/JasperReports   

JasperReports Studio and the JasperReports Java library are available for FREE from Tibco.  
https://community.jaspersoft.com/download-jaspersoft/community-edition   

JasperSoft - Getting Started   
https://www.jaspersoft.com/getting-started

Learning JasperReports - Tutorialspoint   
https://www.tutorialspoint.com/jasper_reports   

Building reports in Java with JasperReports - Udemy Class   
https://www.udemy.com/course/build-java-reports-with-jasperreports-and-jasperstudio   

Google search: ```learning jasperreports```   
https://www.google.com/search?q=learning+jasperreports&oq=learning+jasperreports   

### Pete Helgren's Original Repos
RPG Report Generator Java Project    
https://github.com/phelgren/RRE_java   

RPG Report Engine Project    
https://github.com/phelgren/RRE    

# Pre-requisites
You must install the QShell on i library - QSHONI  
https://github.com/richardschoen/qshoni

# Installing IBMIJASPER library via save file and creating command objects

Download the **ibmijasper.savf** save file from the selected releases page. 

https://github.com/richardschoen/ibmijasper/releases   

Upload the **ibmijasper.savf** to the IFS and place it in **/tmp/ibmijasper.savf**

Run the following commands to copy the save file from the IFS into a SAVF object

```CRTSAVF FILE(QGPL/IBMIJASPER)```
 
```CPYFRMSTMF FROMSTMF('/tmp/ibmijasper.savf') TOMBR('/qsys.lib/qgpl.lib/ibmijasper.file') MBROPT(*REPLACE) CVTDTA(*NONE)```

Restore the IBMIJASPER library

```RSTLIB SAVLIB(IBMIJASPER) DEV(*SAVF) SAVF(QGPL/IBMIJASPER)```

Build the IBMIJASPER commands

 ***(Important to CHGJOB CCSID(37) before building from SAVF)***

```CHGJOB CCSID(37)```

```ADDLIBLE IBMIJASPER```

```CRTCLPGM PGM(IBMIJASPER/SRCBLDC) SRCFILE(IBMIJASPER/SOURCE) SRCMBR(SRCBLDC) REPLACE(*YES)```

```CALL PGM(IBMIJASPER/SRCBLDC)```

The following command restores the Java objects to IFS folder: ```/ibmijasper``` 
```IBMIJASPER/RSTRRE```

If all runs successfully the IBMIJASPER library commands should be ready to use.   

# After install initial test command
The sample call to the RREGEN command listed below should create a sample PDF report based on the ```IBMIJASPER/EMPLOYEE``` table.   

The output file should gen generated to file name: ```/tmp/EmployeeListing.pdf```

```ADDLIBLE IBMIJASPER```

```
RREGEN LIBLIST(IBMIJASPER) 
RPTNAME('/ibmijasper/reports/templates/Employee_Listing.jrxml') 
RPTOUTPUT('/tmp/Employee_listing') OUTPUTFMT(PDF) REPLACE(*YES) DSPSTDOUT(*YES)  
```

After running the command, locate and view PDF file: `/tmp/Employee_Listing.pdf`
