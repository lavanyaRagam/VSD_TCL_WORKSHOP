# VSD_TCL_WORKSHOP

Introduction

Tcl (Tool Command Language) is a high-level, general-purpose, interpreted, dynamic programming language. It is designed to be simple yet powerful. In this project, we create a UNIX-shell command vsdsynth that passes a .csv file to a Tcl script vsdsynth.tcl. This helps automate synthesis-related tasks and CSV-based processing using Tcl.

Everything in Tcl is treated as a command, including variable assignments, loops, procedures, and even help messages.

## Links to Navigate Each Day

- [DAY-1](#day-1)
- [DAY-2](#day-2)
- [DAY-3](#day-3)
- [DAY-4](#day-4)
- [DAY-5](#day-5)
- [CONCLUSION](#conclusion)

### DAY-1
Sub-Task: Create a UNIX command (vsdsynth) and pass a .csv file to a Tcl script
1. Create the UNIX Shell Wrapper

Create a file named vsdsynth:

Make it executable:
```bash
chmod +x vsdsynth
```

```bash
#!/bin/tcsh -f
```
2.Creating the Logo and if-else cases for each of three types of arguements:
* LOGO:
* No Argument:
* Wrong/Non-existent Argument:
* '-help' Argument:
3. Source the UNIX shell to TCL script by passing the required .csv file:
```bash
tclsh vsdsyntha.tcl $argv[1]
```  

### DAY-2
Sub-Task: Convert all inputs into the specified format[1] and configure variables, including directory-existence checks, to correctly feed the data into Yosys for synthesis.

* 1.Creation of Variables:
* 2.Check the existence of directories/files in the obtained path post normalizing:
* 3.Throw an Error out at the absence of directory/file at the path obtained:
* 4.Read the Constraint file and proccess parameters for the SDC file:
  * Convert the constraint file into a matrix and define the SDC constraints
* Categorization of each clocks, inputs and outputs features by porting their location in .csv file:

  
### DAY-3
Sub-Task: Read the constraint file and convert it into an SDC file
* 1. Generation of Clock Constraints:
  * snap of SDC file with Clock Constraints:
* 2. Generation of Input constraints, and classifying each input port whether they are bussed or not:
     * Snap of SDC file with Input Constraints post processing:


### DAY-4
Sub_Task: Obtain the final output constraints and feeding the SDC file, standard lib and RTL Netlist into the Yosys EDA tool for synthesizing

* 1.Generation of Output Constraints:
  * Snap of SDC file with Output Constraints:
* 2.Creation of Hierarchy Check script (.hier.ys) to assure proper instantiations of different modules in the top-module:
*3.Error Handling of Hierarchy Check script:

### DAY-5
Sub-Task: Converting to an opentimer compatible format from a Yosys-synthesized netlist.
* 1. Creation of Synthesis Script (.ys) and running the synthesis script:
* 2. Conversion of synthesized file (.synth.v) to a format that Opentimer is compatible with:
The primary reason of this conversion is that Opentimer can't identify or doesnt need few syntaxes that .synth.v is susceptible to contain. Such as "*" and "//". So we have to process .synth.v file such that it no longer contains such symbols.


```bash
set fileID [open /tmp/1 "w"]
puts -nonewline $fileID [exec grep -v -w "*" $OutputDirectory/$DesignName.synth.v]
close $fileID

set output [open $OutputDirectory/$DesignName.final.synth.v "w"]

set filename "/tmp/1"
set fid [open $filename r]
	while {[gets $fid line] != -1} {
		puts -nonewline $output [string map {"\\" ""} $line]
		puts -nonewline $output "\n"
	}
close $fid
close $output
```
* using a temporary file in write mode to extract the information apart from the information that is involved with "*" as it is not required, and copy the content to the .final.synth.v file.
* open the .final.synth.v file in write mode and read the file till the end of the it ( [gets $fid line] != -1 ) to remove all the "//" symbols.
* Hence by making it a format compatible to Opentimer (.final.synth.v).
* A Snap of .final.synth.v vs .synth.v :
* 3.Static timing analysis using Opentimer:
* Usage of Procs: what is a proc? The proc file system acts as an interface to internal data structures in the kernel. It can be used to obtain information about the system and to change certain kernel parameters at runtime.

    set_multi_cpu_usage proc:
    usage of read_sdc proc to obtain parameters from .sdc file:
    Execution of Procs to obtain .conf file:
  * 4.Creation of .conf and .spef file:
  
    *.spef file:
    
    *.conf file:
    
5.Quality of Results (QoR):

Quality of Results is a term used in evaluating technological processes. In this case, we have different parameters with respect to instances such as setup violations, hold violations, worst negative slack and Run-time to determing the Quality of Results.

 Display of Outputs after parameters are filed in .results file:
6. Final Output of TCL Script

### CONCLUSION
* Usage of basic TCL syntax, variables, control structures, and procedures has been understood.
* Learned about various TCL commands and their functionalities. Explored and Learned commands for file handling, string manipulation, mathematical operations, and interacting with the system.
* Learned Linux based Procs and its application in TCL Scripting.
* Learned effective scripting techniques using TCL and discovered techniques for error handling and debugging.
