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

## DAY-1
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

  <img width="1312" height="318" alt="Screenshot 2025-11-14 192436" src="https://github.com/user-attachments/assets/b801caa8-d40f-46fd-b024-fcba0811ea39" />


* No Argument:

  <img width="1312" height="318" alt="Screenshot 2025-11-14 192436" src="https://github.com/user-attachments/assets/bd273a12-a931-411a-b134-74d9ea47bb39" />

  
* Wrong/Non-existent Argument:

  <img width="982" height="314" alt="Screenshot 2025-11-14 192529" src="https://github.com/user-attachments/assets/ff67da68-3659-499b-a383-0bec737ce609" />

  
* '-help' Argument:

  <img width="1373" height="674" alt="Screenshot 2025-11-14 192858" src="https://github.com/user-attachments/assets/7e8e6d45-0ecf-488d-a978-162d8aa4622c" />

  
3. Source the UNIX shell to TCL script by passing the required .csv file:
  
```bash
tclsh vsdsyntha.tcl $argv[1]
```



## DAY-2
Sub-Task: Convert all inputs into the specified format[1] and configure variables, including directory-existence checks, to correctly feed the data into Yosys for synthesis.

* 1.Creation of Variables:

  <img width="1086" height="594" alt="Screenshot 2025-11-19 211754" src="https://github.com/user-attachments/assets/0c5f13f1-2756-4003-9eff-bc079eeb68fc" />

  
* 2.Check the existence of directories/files in the obtained path post normalizing:

  <img width="1503" height="645" alt="image" src="https://github.com/user-attachments/assets/44b75a50-1186-43a4-8131-bbd0c0f5e1cb" />

  
* 3.Throw an Error out at the absence of directory/file at the path obtained:

 <img width="1010" height="82" alt="image" src="https://github.com/user-attachments/assets/3bf43a71-ff5e-4bef-aafc-aaa69f9fd464" />

  
* 4.Read the Constraint file and proccess parameters for the SDC file:

  <img width="718" height="272" alt="image" src="https://github.com/user-attachments/assets/371f7533-eab3-40cf-8338-8a8ff7b7992f" />

  
  * Convert the constraint file into a matrix and define the SDC constraints

	<img width="718" height="272" alt="image" src="https://github.com/user-attachments/assets/dabbb938-1dc6-4989-a5d7-53772c9aefb8" />

    
* Categorization of each clocks, inputs and outputs features by porting their location in .csv file:

<img width="827" height="476" alt="image" src="https://github.com/user-attachments/assets/e41c9567-49e8-46bc-a609-26228b56f556" />

  
## DAY-3
Sub-Task: Read the constraint file and convert it into an SDC file

* 1. Generation of Clock Constraints:

<img width="1037" height="86" alt="image" src="https://github.com/user-attachments/assets/0b9cf4b0-2ba9-40a7-8ce3-a8f3d0bdfbdb" />


  * snap of SDC file with Clock Constraints:

	<img width="597" height="232" alt="image" src="https://github.com/user-attachments/assets/11d8ed9a-fbfb-4416-add0-e30f104bb238" />

    
* 2. Generation of Input constraints, and classifying each input port whether they are bussed or not:

  <img width="572" height="587" alt="image" src="https://github.com/user-attachments/assets/6930f3ea-6541-4e3c-ba3b-6c9dc38cb76c" />

     
     * Snap of SDC file with Input Constraints post processing:

	   <img width="945" height="697" alt="image" src="https://github.com/user-attachments/assets/34ec5a4e-b366-4046-935b-31e340f1c158" />



## DAY-4
Sub_Task: Obtain the final output constraints and feeding the SDC file, standard lib and RTL Netlist into the Yosys EDA tool for synthesizing

* 1.Generation of Output Constraints:

<img width="438" height="621" alt="image" src="https://github.com/user-attachments/assets/43d9a337-d3ee-4527-adb0-572dc4f5dd17" />


  * Snap of SDC file with Output Constraints:

	<img width="842" height="692" alt="image" src="https://github.com/user-attachments/assets/bdf6c332-06c8-40af-8b9c-b9af9ad902ad" />

	
* 2.Creation of Hierarchy Check script (.hier.ys) to assure proper instantiations of different modules in the top-module
  
*3.Error Handling of Hierarchy Check script

## DAY-5
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

<img width="1528" height="740" alt="image" src="https://github.com/user-attachments/assets/6451e885-7375-4eab-a900-c228afdaa340" />

  
* 3.Static timing analysis using Opentimer:


* Usage of Procs: what is a proc? The proc file system acts as an interface to internal data structures in the kernel. It can be used to obtain information about the system and to change certain kernel parameters at runtime.

    set_multi_cpu_usage proc:

		<img width="728" height="417" alt="image" src="https://github.com/user-attachments/assets/59d1aab1-3fd8-43c5-9293-03f60e45be39" />


    usage of read_sdc proc to obtain parameters from .sdc file:

		<img width="465" height="723" alt="image" src="https://github.com/user-attachments/assets/8627e051-3cd4-4751-8c0b-9678ddb063c4" />


    Execution of Procs to obtain .conf file:
  
  * 4.Creation of .conf and .spef file:
  
    *.spef file:

	<img width="1577" height="223" alt="image" src="https://github.com/user-attachments/assets/65eda5b3-ba71-4269-9091-227c740a4ca0" />

  
    

	
    
5.Quality of Results (QoR):

Quality of Results is a term used in evaluating technological processes. In this case, we have different parameters with respect to instances such as setup violations, hold violations, worst negative slack and Run-time to determing the Quality of Results.

 Display of Outputs after parameters are filed in .results file:
6. Final Output of TCL Script

<img width="1392" height="145" alt="image" src="https://github.com/user-attachments/assets/84adc2fd-5e2c-4e53-999d-98ff90be7902" />


## CONCLUSION
* Usage of basic TCL syntax, variables, control structures, and procedures has been understood.
* Learned about various TCL commands and their functionalities. Explored and Learned commands for file handling, string manipulation, mathematical operations, and interacting with the system.
* Learned Linux based Procs and its application in TCL Scripting.
* Learned effective scripting techniques using TCL and discovered techniques for error handling and debugging.
