---
permalink: /install/
title: "Installation"
---

In principle, installing SIRIUS is as simple as extracting the downloaded archive to a directory where you have write permission. 
The *Java Runtime Environment* (JRE) needed to run SIRIUS is already included.

**For Windows/MacOS** we also provide **installer packages** (msi/pkg), which **should be preferred** 
but may require admin permissions. As we do not pay Microsoft/Apple for certification, 
you may need to confirm that you want to trust software from an unknown source on Windows/MacOS.

The installation should take no more than **10 minutes**.

If you have any problems installing SIRIUS, please contact support or ask for help in the [community](https://matrix.to/#/#sirius-ms:gitter.im). You can also use the forum to let us know if you find that our installation guide is incomplete, or if you have
 tricks that you would like to share with other SIRIUS users. Feel also free to 
[contribute to this documentation](https://github.com/sirius-ms/sirius-docs.github.io#contributing-to-the-sirius-documentation) yourself.

**WARNING:** Any advice given here on how to get SIRIUS to work on your
system is given without warranty! If you are not sure what you are
doing, please contact someone who is. (Remember, the last
Friday in July is System Administrator Appreciation Day!)


## Windows {#windows}
Built and tested on Windows 10 x64
##### MSI installer (preferred) {#msi-installer}
Execute the installer, trust the unknown source (if asked) and follow the instructions.
You will be asked to choose an installation location and to 
accept the SIRIUS license agreement. The installer should also create a start menu entry for SIRIUS.

##### Zip package {#zip-windows}
Extract the archive to any directory where you have write
permissions, e.g. `C:\SIRIUS`.

##### Execution {#exe-windows}
Go to the SIRIUS directory. Run `sirius-gui.exe` to start the graphical user interface.
You may want to create a shortcut on your desktop: Click and drag the file
to the desktop while holding down the ALT key. You can rename the desktop shortcut. Double-click it to start SIRIUS.
 

Run `sirius.exe` for the SIRIUS command line tool. To execute the SIRIUS command line
tool from anywhere on your system, you must add the location of
the `sirius.exe` to your PATH environment variable: Open the Windows Settings, type
"advanced" in the search window, say "yes" if Windows asks you. Press
the "Environment Variables" button, select the "Path" variable in the
lower panel, press "Edit", press "New", enter the full directory path
of SIRIUS, press RETURN. Close the Command Prompt, open a new one, type
`sirius`.

## Mac OSX {#macosx}
Built and tested on macOS Catalina 10.15 x64
##### pkg installer (preferred) {#pkg-installer}
Execute the installer, trust the unknown source (if blocked by Gatekeeper). The option to confirm the execution of 
an installer from an unknown source may be "hidden" in `"System Settings" -> "Security & Privacy"`. 
**This should not happen with the signed installer**  

Follow the installer's instructions. You will be asked to select an installation disk and to accept the SIRIUS licence agreement.

##### Zip package {#zip-mac}
Extract the archive to any directory where you have write
permissions, e.g. the `Download` folder. You can then move/copy the extracted `.app` 
folder to your application directory. You may need to define gatekeeper exceptions
for all libraries in the SIRIUS `.app` directory. If you are not sure how to do this
we recommend using the installer version.  

##### Execution {#exe-mac}
To run the SIRIUS GUI, simply go to your `Applications` directory and double-click the `sirius-gui` application.
You can also add SIRIUS to your `Dock` if you prefer.  

To run the SIRIUS command line tool, open a terminal and execute 
the `sirius` launcher in your `Applications` directory (usually `/Applications/sirius.app/Contents/MacOS/sirius`).

To execute SIRIUS from any location, you need to add the sirius binary directory `<SIRIUS_DIR>/Contents/MacOS` to your PATH
variable. To do this, open `/etc/paths` in a text editor and add the following line:

```
/Applications/sirius.app/Contents/MacOS/
```

If you have changed the directory, replace the line with the respective sirius installation directory.
Note that the SIRIUS GUI version also contains the command line runner, but uses a slightly different location per default:
```
/Applications/sirius-gui.app/Contents/MacOS/
```

## Linux {linux}
Built and tested on Ubuntu 18.04+ x64
##### Zip version {zip-linux}
Extract the archive to any directory where you have write
permissions, e.g. `/home/opt/sirius`.

To start the command line version of SIRIUS, execute the 
`<SIRIUS_DIR>/bin/sirius` starter in the terminal.

To start the graphical user interface of SIRIUS, execute the 
`<SIRIUS_DIR>/bin/sirius-gui` starter in the terminal.

To execute SIRIUS from any location, you must add the `<SIRIUS_DIR>/bin` to your PATH
variable. To do this, open it in an editor and add the following line
(replacing the placeholder path) to your `~/.bashrc`:

```
export PATH=PATH:<SIRIUS_DIR>/bin/
```

Note that you will need to reopen your "bash" shell for the changes to take effect.

## User account and License (since `v5.0.0`) {#account-license}

Certain features of SIRIUS require access to the SIRIUS web services:
- structure elucidation with CSI:FingerID, 
- compound classification with CANOPUS,
- *de novo* structure generation with MSNovelist.

From version `5` onwards, a licence and a user account are required to use these features.
The **SIRIUS web services are free for academic use.** Academic institutions are identified by their
email domain and access will be granted automatically. In case further validation is required, see [Account and License]({{ "/account-and-license" | relative_url }}).


## Installing Gurobi and/or CPLEX {#ILP-solvers}

SIRIUS comes with the *COIN-OR* integer linear
program solver which allows us to swiftly compute fragmentation trees in
most cases. However, if you are analysing large molecules and/or
spectra with many peaks and/or many spectra, you can
improve running time by using a faster solver. SIRIUS
supports the *Gurobi* and *CPLEX* integer linear program solvers. These are
commercial solvers that offer a free academic licence for university
members. Installation instructions can be found on their websites. Using
*Gurobi* or *CPLEX* will improve the speed of the fragmentation tree
computation, which is the most time-consuming step of the computational
analysis. Other than that, there is no differences when using *Gurobi* or
*CPLEX*. To use Gurobi, set the environment variable `GUROBI_HOME`
to a valid Gurobi installation location. 
Similarly, to use *CPLEX*, set `CPLEX_HOME` to a valid Gurobi installation location.

**CPLEX:**
On Windows, the CPLEX installer usually sets the system variable automatically, and you should be good to go.
* Windows: set `CPLEX_HOME` to e.g. `C:\Program Files\IBM\ILOG\CPLEX_Studio1271\cplex`.
* Linux: Set `CPLEX_HOME` to the CPLEX install directory e.g. `/opt/ibm/ILOG/CPLEX_Studio1271/cplex`.
* MacOS: Set `CPLEX_HOME` to the CPLEX install directory e.g. `/Library/ibm/ILOG/CPLEX_Studio1271/cplex`.

**Gurobi:**
* Windows: Set `GUROBI_HOME` to the Gurobi lib directory e.g. `C:\gurobi702\win64`.
* Linux: Set `GUROBI_HOME` to the Gurobi lib directory e.g. `/opt/gurobi702/linux64`.
* MacOS: Set `GUROBI_HOME` to the Gurobi lib directory e.g. `/Library/gurobi702/mac64`.


SIRIUS will automatically detect Gurobi or CPLEX as possible solvers if the respective environment variables are specified. You can specify the preferred solver in the settings 
dialog (GUI) or in the command line with the `--ilp-solver` parameter.
To permanently change the setting (e.g. for SIRIUS version 4.6.x) open: 
`<USER_HOME>/.sirius-4.6/sirius.properties`  and edit the following line as required:

```properties
de.unijena.bioinf.sirius.treebuilder.solvers = cplex,gurobi,clp
```
The order of the solvers specifies the priority in which SIRIUS uses them.

**Error message: "Could not load a valid TreeBuilder (ILP solvers), tried '[GUROBI, CPLEX, CLP, GLPK]'. Please read the installation instructions."**
SIRIUS ships with the non-commercial CLP solver and should work out-of-the box. If you are nevertheless experiencing this issues, [please contact us]({{"/bugs/#bug-report" | relative_url}}). 
It is almost impossible to reproduce and fix such issues without additional information from users. Thanks.

## Proxy servers {#proxy-servers}

To use the web service functionality of SIRIUS, an internet
connection is required. You must ensure that SIRIUS is not blocked by any
security software on your computer.

If you need to use a proxy server to connect to the Internet, you can specify the proxy configuration in the
Sirius user interface settings (see [Settings]({{ "/gui/#settings" | relative_url }})).

If SIRIUS cannot connect to the Internet, it will [report]({{ "/gui/#webservice" | relative_url }}) at what stage
the error occurred.

## System Requirements {#system-requirements}
The hardware and software requirements for SIRIUS depend on the complexity of the data, the size of the molecules being analyzed, and the desired processing throughput. Below are the minimum and recommended specifications for optimal performance.

* **Supported Operating Systems:** Windows 10+ (x86-64), MacOS (x86-64, aarch64), Linux  (x86-64, aarch64)
* **CPU:** Quad Core CPU
* **RAM:** 16 GB is recommended. As a general rule, allocate at least 2 GB of RAM per CPU core. Certain modules, such as ZODIAC (used for dataset-wide formula optimization), may require additional memory proportional to the number of compounds in the dataset.
* **Storage:** An SSD is strongly recommended. SIRIUS frequently accesses project files and local databases; the high I/O speed of an SSD drastically reduces processing bottlenecks compared to traditional HDDs.
* **Internet Connection:** A stable connection (minimum 1 Mbit/s) is required for communication with the SIRIUS web services.