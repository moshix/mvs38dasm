# mvs38dasm

This is a re-share of Gerhard Postpischil's simply amazing S/360 and S/370 disassembler for MVS3.8


What it is
----------

It has become somewhat difficult to find a disassembler which can itself be assembled with the old MVS IFOX assembler. A lot of the disassemblers you find on the CBT tape require newer assemblers (such as the H assembler) or HLASM. While we do have support for s390x instruction in Hercules and in MVS 3.8 as delieved by TK4-, the lack of a modern assembler often means we cannot easily build a good, solid disassembler. 

CBT file 217 contains an ok disassemblr by Alan Field. The listing produced by that disassembler was a bit too simple, and even though often it handled SVCs correctly, often it didn't. 

Gerhard Pospischil went and created, with powerful, beautiful, but clearly in casual programming style (and that's a compliment!) a very powerful disassembler which assembles perfectly and runs perfectly in MVS3.8j 8505 as delivered in TK4. 

HOWEVER, there seem to be two versions of this Postpischil disassembler, both being distributed by the Moseley SYSCPK compiler pack. One assembles cleanly, the other doesn't. Let me explain:

The SYSCPK most folks download and install is an IBM 3350 DASD image from Jay Moseley's website. That SYSCPK image was recently (in January 2018) updated by Jay Moseley and contains a newer version of the disassembler. That version does not assemble cleanly on TK4- (which of course is the MVS 3.8 system most folks run nowadays). 

I happen to have an IBM 3390 SYSCPK image I created myself back in 2015 from the 3350 SYSCPK image back then, and that contains an older version of the Postpischil disassembler which assembles and runs very cleanly on TK4-. It still has all the same features as the newer version, including support for 31bit OSs. 

In short, this repository contains the version from my 3390 SYSCPK which assembles and runs beautifully on MVS3.8 TK4-, on OS390 and even on z/OS 2.10. 





How to install it
-----------------

- Start FTPD on your TK4
- Upload this whole directory, WIHTOUT THE INCLUDED PDFs!, to a PDS called HERC01.DASM.SOURCE
- Make sure you uploaded as ASCI, obviously
- Login to TK4 as HERC01
- Go to member ASMJCL and open in the editor
- Check everything is ok, then SUBMIT it to JES2
- It takes a good 45 seconds to execute about 20 steps, there is quite a bit of source code
- If all RC=00 in all steps, then the disassembler will be installed in SYS1.LINKLIB as DISASM01
- Test it with $$TESTJCL
- The job outputs a large listing, go have a look at the source code written by Gerhard. It's beautiful and there are a ton of cool tricks he does. Gerhard is a true assembler virtuozzo. 

How to run it
-------------


check out the documentation in member $$DOC and RTFM, but here is an example JCL to run the disassembler:

  //DIASM     EXEC PGM=DISASM01,REGION=950K  
  //STEPLIB  DD DSN=SYS1.LINKLIB,DISP=SHR  
  //SYSPRINT DD DSN=&&PRT,DISP=(NEW,PASS),  
  //            UNIT=SYSDA,                                     
  //            SPACE=(TRK,(15,15)),                            
  //            DCB=(RECFM=FBM,LRECL=121,BLKSIZE=12100)         
  //SYSIN    DD DSN=&&IN,DISP=(NEW,PASS),
  //            UNIT=SYSDA,                                     
  //            SPACE=(TRK,(15,15)),                            
  //            DCB=(RECFM=FB,LRECL=80,BLKSIZE=3120)            
  //SYSLIB   DD DSN=xxxx,DISP=SHR       
  //SYSUT1   DD UNIT=SYSDA,SPACE=(CYL,(1,1))  
  //SYSUT2   DD UNIT=SYSDA,SPACE=(CYL,(1,1))   
  //SYSUT3   DD UNIT=SYSDA,SPACE=(CYL,(1,1))  
  //SYSPUNCH DD DUMMY                        
  //DISDEBUG DD SYSOUT=*                    
  //DISPRINT DD SYSOUT=*                   
  //DISPUNCH DD SYSOUT=class              
  //DISMOD   DD DISP=SHR,DSN=load.mod.pds 
  //         DD DISP=SHR,DSN=load.mod.pds2 
  //DISIN    DD *                        
        control statements                                      
  //                                                            


Other Disassemblers
===================

While we are on this topic, check out this disassembler written by A. Armstrong
in Rexx with support with the latest IBM z15 instructions. 
https://github.com/abend0c1/da

have fun!

Moshix

October 2020
