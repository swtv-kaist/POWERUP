# POWERUP

The readme of POWERUP is under construction,
for the time being, please refer [readme](README_AFL++.md) of AFL++.


POWERUP is impelmented based on AFL++, and you can use POWERUP as similar way to AFL++.
The below documentation focuses on the difference from AFL++.

## Instrumentation
You can perform instrumentation as same to AFL++.
I recommend you to use [gllvm](https://github.com/SRI-CSL/gllvm) and get a whole bitcode of the subject program before performing instrumentation.

For example, `${POWERUP_repo}/afl-clang-lto++ <target.bc> -o <target.afl> <ld flags...>` will give you an instrumented program.

## Structure of a test case
POWERUP considers a test case as a pair of a command-line input and a file input.
To conveniently mutate both inputs, POWERUP generates two seperate files for each test case.
(One for command line option, and the other for file.)

The file inputs are saved in the ordinary `queue` directory in output directory,
while the command-line option inputs are saved in `queue_argv` directory.
Each file pair with same id will be considered as a test case.

The instrumented program will take only one command-line option,
the path to command-line option input. The instrumented code in main function
will interpret the given command-line option input file, and it will begin the execution.

## Run POWERUP
You can start execution as similar to AFL++.

For example, you can run `afl-fuzz` with the following command.

`${POWERUP_repo}/afl-fuzz -i <initial seed dir> -o <output dir> -K 5 -a <dictinary file path> -- <target.afl> <initial args...>`

You can find dictinary file examples in `powerup_keyword_dict/`.
The dictionary file is a simple list of keywords that can be used in command-line options.

## Replay
The saved command-line option input files should contain the path to the file input.
However, to make it easier to mutate the command-lin option input files,
The path strings are replaced with "@@".
If you want to replay the saved test case, you should replace the first @@ in a command-line option input
with the corresponding file input path.