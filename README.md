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
❗ You must install the QShell on i library - QSHONI  
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

# Test RREGEN command after install initial test command
The sample call to the RREGEN command listed below should create a sample PDF report based on the ```IBMIJASPER/EMPLOYEE``` table.   

The output file should gen generated to file name: ```/tmp/EmployeeListing.pdf```    

❗For this example we left ```DSPTDOUT=*YES``` so you can see the log that gets generated. For batch or production usage you would use ```DSPSTDOUT=*NO```.   

```ADDLIBLE IBMIJASPER```

```
RREGEN LIBLIST(IBMIJASPER) 
RPTNAME('/ibmijasper/reports/templates/Employee_Listing.jrxml') 
RPTOUTPUT('/tmp/Employee_listing') OUTPUTFMT(PDF) REPLACE(*YES) DSPSTDOUT(*YES)  
```

After running the command, locate and view PDF file: `/tmp/Employee_Listing.pdf`

# RREGEN command parms

**Overview** - The RREGEN CL command can be used to execute a JasperReports JRXML report template and output the appropriate PDF or other format.   

**HOSTNAME - Host name** - Enter the host name database to connect to. Use localhost for local IBM i database. Default: ```localhost```    

**LIBLIST- Library list** - Enter the library list for the jt400.jar connection to use. Default: ```QGPL```   

**USERNAME - User ID** - IBM i user id to use for connection. Default: ```*NONE``` which uses current job user info so no user/password required.   

**PASSWORD - Password** - IBM i password to use for connection. Default: ```*NONE``` which uses current job user info so no user/password required.   

**RPTNAME - Report name** - Enter the IFS file path for the JRXML template file to compile and execute.   

**RPTOUTPUT - Report output without extension** - Enter the IFS file path for the output file you would like to generate without the file extension. The file extension is derived from the Output format parameter.   

**OUTPUTFMT - Output format** - Specify the desired Jasper output format. Default: PDF.   

**REPLACE - Replace existing output file** - Replace the output file if it already exists. Default: ```*NO```    

**DSPSTDOUT - Display standard output result** - Display the outfile contents. Nice when debugging. 

**DLTSTDOUT - Delete standard output** - This option insures that the STDOUT IFS temp files get cleaned up after processing. All IFS log files get created in the /tmp/qsh directory.   

**PRTSTDOUT - Print standard output** - Print STDOUT to a spool file. Use this if you want a spool file of the log output.

