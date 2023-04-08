---
title: Anatomy of an HLA Program
categories: assembly
tags: assembly programming
---

**HLA Program template**

```bash
program pgmID ;

	<< Declarations >>

begin pgmID ;

	<< Statements >>

end pgmID ;
```

The Declarations section is where you declare constants, types, variables, procedures, and other objects in an HLA program.
The Statements section is where you place the executable statements for your main program.
 
The **program**, **begin**, and **end** are HLA reserved words that delineate the program. Note the placement of the semicolons in this program. These identifiers specify the name of the program. They must all be the same identifier. pgmID in the template above is a user-defined program identifier.

HLA identifiers may begin with an underscore or an alphabetic character and may be followed by zero or more alphanumeric or underscore characters. HLA’s identifiers are case sensitive. 


```bash
program helloWorld;
#include( "stdlib.hhf" );

begin helloWorld;
	stdout.put( "Hello, World of Assembly Language", nl );

end helloWorld;
```
 
The **#include** statement in this program tells the HLA compiler to include a set of declarations from the **stdlib.hhf** (standard library, HLA Header File). Among other things, this file contains the declaration of the **stdout.put** code that this program uses, which is simalar to print. The **nl** is a constant, also defined in stdlib.hhf, that corresponds to the newline sequence.