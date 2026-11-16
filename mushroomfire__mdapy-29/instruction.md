# Issue

**Title:** Plain XYZ comments, NaN in text files, damaged files and fuzzing

## Description

- **Plain XYZ.** A comment line without `=` holds no `key=value` pair: it is the free comment of a plain XYZ file, kept whole as `info["comment"]`, and its words are no longer read as keys. A comment that repeated a word, such as `MMM12 made of C atoms on grid; generated for MMM12 logo + H atoms`, used to be refused as a key given twice. Such a file holds an element and x, y, z on each line, and the error for any other line says so.
- **NaN in text files.** Every NaN is written as `nan` and the infinities as `inf` and `-inf`, as Python writes them. NaNs went to `std::to_chars`, which writes `-nan(ind)` or `nan(snan)` on some systems, so a NaN with its sign bit set, as x86 computes 0/0, could be written in a form that no reader takes, MDAPY's included.
- **A damaged atom count.** A count of atoms beyond 64 bits in an XYZ header gave a TypeError; it now gives a FileFormatError, as other counts beyond the end of the file do.
- **Tests.** `tests/io/test_damaged_files.py` damages a sample of every format at random from a fixed seed and requires an error of MDAPY for each, for one frame and for whole trajectories. `tests/fuzz/` holds two libFuzzer targets of the text kernels: any text is read or refused without a fault, numbers agree with `strtod` bit for bit and come back unchanged when written, with the fewest digits, and so do tables. A new CI job runs each for a minute. The NaN and the atom count were found this way.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bd619e816d1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.