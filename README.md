## Use of the QcNooq project<br>
The QcNooq project is available for download here or at the page: www.zonabit.it/QcNooq. Are available separately:<br>
•	the setup of the QcNooq executable program for Microsoft Windows (QcNooqSetup.exe);<br>
•	the source code (QcNooq.zip).<br>
QcNooq is a free toolbox to implement the contents of the book: <i>Quantum Computing for Programmers and Investors</i>, Published by GogLiB in 2020 - ISBN: 9788897527541<br>
If you don’t want to compile QcNooq, simply install the software and run it to check the output of the emulation code as you read the book.<br>
If you want to compile QcNooq under Windows, unzip QcNooq.zip in any folder and open the project. The source code includes a project for Microsoft Visual Studio (QcNooq.vcproj) in a version from a few years ago. Any recent version of Microsoft Visual Studio will convert the project automatically. The project has 32-bit configuration for compatibility with older versions, but users can easily compile for 64-bit, and will experience some slight increase in execution speed
There is also another project (QcNooqConsole.vcproj) that allows you to compile QcNooq as a console application, without a Windows GUI: this does not give any advantage in a Windows environment, but can help the porting of QcNooq as a console application in a different environment.<br>
If you want to compile QcNooq in an environment other than Windows, you must use the code following the instructions below.<br>
1.	some modules should not be included in the project because they contain the implementation of the Windows interface, and are the following:<br>
QcNooqButton.h<br>
QcNooqDialog.h<br>
QcNooqDlg.h<br>
QcNooqStatic.h<br>
QcNooq_ReadMe.h<br>
stdafx.cpp<br>
QcNooqButton.cpp<br>
QcNooqDialog.cpp<br>
QcNooqDlg.cpp<br>
QcNooqStatic.cpp<br>
QcNooq_ReadMe.cpp<br>
2.	some modules make up a fully portable library of mathematical functions, and can be recompiled without difficulty in any environment:<br>
QCM_math.h<br>
QCM_tools.h<br>
QCM_complex0.cpp<br>
QCM_format0.cpp<br>
QCM_matrix0.cpp<br>
QCM_matrix1.cpp<br>
QCM_procs0.cpp<br>
QCM_state0.cpp<br>
QCM_tools0.cpp<br>
QCM_vector0.cpp<br>
3.	the last group of sources corresponds to the chapters of the book:<br>
3: QCP_matrix.cpp<br>
4: QCP_bitqubit.cpp<br>
5: QCP_gates.cpp<br>
6.1: QCP_deutsch.cpp<br>
6.2: QCP_deutsch_josza.cpp<br>
6.3: QCP_simon.cpp<br>
6.4: QCP_grover.cpp<br>
6.5: QCP_shor.cpp<br>
and for the console application there is an entry point sample in:<br>
QCNooqConsole.cpp<br>
The source code of the modules of the third group contains first the implementation of the dialog that hosts buttons and other objects in the Windows interface, and then the code corresponding to the examples discussed in the corresponding chapters. Throughout the program, the application code concerning the Windows interface is under the condition of the definition of the symbol QCNOOQ_WINDOWS, which must not be defined outside the Windows environment, nor in the console version. In most cases the code under the QCNOOQ_WINDOWS condition can be simply omitted, and only in some cases is it necessary to intervene to provide a requested data input. For example, to test Shor’s algorithm we find the case in which the number N is read from a box in the Windows environment, and must be fixed in the sources or asked to the user:<br>
#ifdef QCNOOQ_WINDOWS<br>
m_edit_number_16.GetWindowText(buf, 200); only_digits(buf);<br>
#else<br>
strcpy(buf, "15"); // to be done: input number<br>
#endif<br>
In all cases, the code uses the following function for output, implemented with three variants:<br>
extern void ListMatrix(char init, char *comment, int mrow, int ncol, qx *mtr, int hexacomment );<br>
extern void ListMatrix(char init, char *comment, int mrow, int ncol, qx *mtr );<br>
extern void ListMatrix(char init, char *comment);<br>
which directs the output to the two boxes provided by the Windows interface, the upper one for generic information and the lower one to represent the matrices resulting from the calculations. The code of ListMatrix() is located in the QCM_tools0.cpp file and must be implemented in the new environment, redirecting the output to the console or to the available graphic interface. The function to be modified is global_ListMatrix(), of which in QCM_tools0.cpp there are both the Windows version and a rudimentary version for a console application.<br>
The program built outside the Windows environment must have an entry point that must include the definitions of the modules with the QCP_ prefix that you want to test, allocate the appropriate class and call the function to be tested. For example, to try the first of the tests provided while reading the book, multiplication of a vector by a matrix, the program will be:<br>
#include <stdio.h><br>
// include and create the QCP_ modules and<br>
// CQCP_ classes you want to test:<br>
#include "QCP_matrix.h"<br>
static CQCP_matrix test_matrix;<br>
int main (int argc, char *argv[] )<br>
{<br>
char buf[100];<br>
 // call the test you want to perform<br>
 test_matrix.QCF_Mul_Vector_Matrix();<br>
 printf ("TEST FINISHED - PRESS ENTER"); gets(buf);<br>
}<br>
and for any other case, all you have to do is include the appropriate QCP_xxx.h module, allocate the class and call the test. The code inside the classes with the prefix CQCP_ can be modified as desired to study the behaviour of the algorithms in different conditions.<br>
 

<!--
**qcnooq/qcnooq** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.



-->
