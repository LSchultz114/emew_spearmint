# A SPEARMINT-based EMEWS model exploration (ME) module

The python-based ME algorithm attempts to minimize an objective function for a parameter space defined using the Spearmint package for Bayesian optimization.

This repository integrates the Spearmint code method with EMEWS.

## ME Requirements

This package requires the following:
 
* Python 2.7
* Swift-t with python enabled - http://swift-lang.org/Swift-T/
* Spearmint with Spearmint-lite included - https://github.com/JasperSnoek/spearmint
* EQ-Py Swift-t extension installed - see the EMEWS templates section in the EMEWS tutorial (http://www.mcs.anl.gov/~emews/tutorial/).

In a folder within the python directory of the EMEWS framework (noted as `wolfe` in the provided code here), a config.json file must be placed. This is the same folder where the final results.dat file will be located when the run is complete.

## Handshake Protocol
`emews_spearmint` begins the handshake by inserting an empty string into its output queue, expecting the Swift-t workflow to retrieve it with an `EQPy_get` call. `emews_spearmint` then expects to receive the following initialization parameters from the Swift-t workflow (inserted with an `EQPy_put` call):

* **Total number of Trials (-nt):** the number of iterations to perform
* **Number of Data Points (-np):** the number of data points spearmint should suggest in each loop

The ME expects to receive these parameters respectively when it calls `IN_get()` for the first time in the following format:

```
'2,3'
```

## Final Protocol
The ME pushes the string "DONE" to the OUT queue to indicate that the algorithm has completed. It will subsequently push the message "Refer to results.dat file in python/wolfe directory" into the OUT queue and complete.

**Note:** in the provided ME repository, `wolfe` is the name of the file containing the config.json file necessary for the spearmint-lite to run. This can be changed.

## Testing ME model

The `test` directory contains a test (`test.py`) for `emews_spearmint` that runs the ME algorithm with python and eqpy (included in the `test` directory), but without Swift/T. To run the test, run the `run_test.sh` bash script in the test directory.


## Acknowledgments

Spearmint is a package developed by Jasper Snoek, according to the algorithms outlined in the paper:

Practical Bayesian Optimization of Machine Learning Algorithms
Jasper Snoek, Hugo Larochelle and Ryan P. Adams
Advances in Neural Information Processing Systems, 2012

Spearmint is the result of a collaboration primarily between machine learning researchers at Harvard University and the University of Toronto.
