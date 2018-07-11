# mvs38dasm

This is a re-share of Gerhard Postpischil's simply amazing S/360 and S/370 disassembler for MVS3.8


What it is
----------

It has become somewhat difficult to find a disassembler which can itself be assembled with the old MVS IFOX assembler. A lot of the disassemblers you find on the CBT tape require newer assemblers (such as the H assembler) or HLASM. While we do have support for s390x instructiosn in Hercules and in MVS 3.8 as delieved by TK4-, the lack of a modern assembler often means we can easily build a good, solid disassembler. 

CBT file 217 contains an ok disassemblr by Alan Field. The listing produced by that disassembler was a bit too simple, and even though often it handled SVCs correctly, often it didn't. 

Gerhard Pospischil went and created, with powerful, beautiful, but clearly in casual programming style (and that's a compliment!) a very powerful disassembler which assembles perfectly and runs perfectly in MVS3.8j 8505 as delivered in TK4. 



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


check out the documentation in member $$DOC and RTFM, but here is an example JCL:

  //....     EXEC PGM=DISASM01,REGION=nnnnK               REQ   
  //STEPLIB  DD DSN=xxxx,DISP=SHR                         OPT   
  //SYSPRINT DD DSN=&&PRT,DISP=(NEW,PASS),                OPT   
  //            UNIT=SYSDA,                                     
  //            SPACE=(TRK,(15,15)),                            
  //            DCB=(RECFM=FBM,LRECL=121,BLKSIZE=12100)         
  //SYSIN    DD DSN=&&IN,DISP=(NEW,PASS),                 OPT   
  //            UNIT=SYSDA,                                     
  //            SPACE=(TRK,(15,15)),                            
  //            DCB=(RECFM=FB,LRECL=80,BLKSIZE=3120)            
  //SYSLIB   DD DSN=xxxx,DISP=SHR                         OPT   
  //SYSUT1   DD UNIT=SYSDA,SPACE=(CYL,(1,1))              OPT   
  //SYSUT2   DD UNIT=SYSDA,SPACE=(CYL,(1,1))   XF ONLY    OPT   
  //SYSUT3   DD UNIT=SYSDA,SPACE=(CYL,(1,1))   XF ONLY    OPT   
  //SYSPUNCH DD DUMMY                                     OPT   
  //DISDEBUG DD SYSOUT=*                                  OPT   
  //DISPRINT DD SYSOUT=*                                  REQ   
  //DISPUNCH DD SYSOUT=class                              OPT   
  //DISMOD   DD DISP=SHR,DSN=load.mod.pds                 REQ   
  //         DD DISP=SHR,DSN=load.mod.pds2 .....          OPT   
  //DISIN    DD *                                         REQ   
        control statements                                      
  //                                                            


have fun!

Moshix
July 2018
