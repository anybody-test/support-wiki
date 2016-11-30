This small example displays how to run a simple parameter study and have the results saved in a .h5 file for each of the steps in the paramter study.

First the output is generated as a .h5 file, then the file is renamed using a bat file to have a name with a counter include. Currently the design variable values (mass) is being used as the counter. After running the model there will exist 10 files named "test" + "number(mass)" + "anydata.h5"

To run the model please unzip and run the ParamStudy.