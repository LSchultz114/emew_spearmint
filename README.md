# A Spearmint-based EMEWS model exploration (ME) module

The python-based ME algorithm attempts to minimize an objective function for a parameter space defined using the Spearmint package for Bayesian optimization.

This repository integrates the Spearmint code method with EMEWS.

## ME Requirements

This package requires the following:
 
* Python 2.7
* Swift-t with python enabled - http://swift-lang.org/Swift-T/
* Spearmint with Spearmint-lite included - https://github.com/JasperSnoek/spearmint
* EQ-Py Swift-t extension installed - see the EMEWS templates section in the EMEWS tutorial (http://www.mcs.anl.gov/~emews/tutorial/).

For spearmint to run, a json configuration file (`config.json`) describing the domain of the optimized parameters is expected. Within the provided code in this repositiory, this file is expected to reside in the `data` folder within the EMEWS structure file. For more information on the config file, please refer to the spearmint documents linked above. 


## Handshake Protocol
`emews_spearmint` begins the handshake by inserting an empty string into its output queue, expecting the Swift-t workflow to retrieve it with an `EQPy_get` call. `emews_spearmint` then expects to receive the following initialization parameters from the Swift-t workflow (inserted with an `EQPy_put` call):

* **Total number of Trials (-nt):** the number of iterations to perform
* **Number of Data Points (-np):** the number of data points spearmint should suggest in each loop

The ME expects to receive these parameters respectively when it calls `IN_get()` for the first time in the following format:

```
'2,3'
```

## Final Protocol
The ME pushes the string "DONE" to the OUT queue to indicate that the algorithm has completed. It will subsequently push the message "Refer to results.dat file in data directory" into the OUT queue and complete.


## Results
The results of this ME code will be placed in a `results.dat` file located in the EMEWS `data` directory. The `results.dat` will contain a white-space delimited line for each experiment, of the format: `<result> <time-taken> <list of parameters in the same order as config.json>`
 
 
## Testing ME model

The `test` directory contains a test (`test.py`) for `emews_spearmint` that runs the ME algorithm with python and eqpy (included in the `test` directory), but without Swift/T. To run the test, run the `run_test.sh` bash script in the test directory.


## Acknowledgments

Spearmint is a package developed by Jasper Snoek, according to the algorithms outlined in the paper:

Practical Bayesian Optimization of Machine Learning Algorithms
Jasper Snoek, Hugo Larochelle and Ryan P. Adams
Advances in Neural Information Processing Systems, 2012

Spearmint is the result of a collaboration primarily between machine learning researchers at Harvard University and the University of Toronto.
