# Introduction

JPF-Doop is a testing tool for Java libraries. It is based on the
[Java PathFinder][8]'s concolic execution engine [jDART][0] and
[Randoop][1], a feedback-directed random testing engine.

# Dependencies

JPF-Doop includes a few tools that ship independently of JPF-Doop, but
they are included in the JPF-Doop distribution for user's
convenience. Those are [JaCoCo][6], [JUnit][7] version 4, and
[Randoop][1].

The following need to be obtained and configured by the user:

* [Java PathFinder][8] (version 6),
* Java PathFinder's [jDART][0] (version 6),
* [Python][3], version 2.7.

If the user wishes to generate code coverage reports, [Apache Ant][4]
is needed too.

# Installation and Configuration

After you've cloned the repository, there is no installation of
JPF-Doop because it is comprised of scripts written in Python. In
other words, you can run it as soon as you obtained a copy of JPF-Doop
(and you have all the dependencies in place).

You can configure various parameters in `jpfdoop.ini`. Most
importantly, change `jpf-core` and `jpf-jdart` in the `jpfdoop`
section so that they point to the main directories of jpf-core and
jpf-jdart, respectively. Do not use `~` as a shorthand for your home
directory. JPF-Doop needs version 6 of Java PathFinder (core module)
and jDART.

## Configuration file

JPF-Doop uses a configuration file. By default, it is
`jpfdoop.ini`. The file is in the INI file format. It has 4 sections:
`[jpfdoop]`, `[sut]`, `[tests]`, and `[lib]`. For an example, see the
contents of `jpfdoop.ini`.

Section `[jpfdoop]` has two options. Option `jpf-core` specifies a
path to Java PathFinder (core module), while option `jpf-jdart`
specifies a path to Java PathFinder's jDART.

Section `[sut]` has one option: `compilation-directory` specifies a
directory where class files of the package being tested can be found.

Section `[tests]` has one option: `compilation-directory` specifies
a directory where should generated JUnit tests be compiled to.

Section `[lib]` has three options that specify where various libraries
can be found. Option `junit` is a path to a JUnit *jar*
archive. Option `randoop` is a path to a Randoop *jar*
archive. Finally, option `jacoco` is a path to a JaCoCo *jar* archive.


# Usage

To run JPF-Doop, a few parameters need to be passed to it, including a
package name and where the package's source and class files are. Other
arguments are optional. For example, to test the
`org.apache.commons.collections` package from the Apache Commons
library from the [jpf-doop-examples][2] repository, assuming that
JPF-Doop and `jpf-doop-examples` are on the same directory hierarchy
level, run the following:

```
#!bash
$ python jpfdoop.py --package org.apache.commons.collections --root ../jpf-doop-examples/
```

A code coverage report will be generated and can be found in the
`jacoco-site/` directory.

For further information on how to run JPF-Doop, you can execute:

```
#!bash
$ python jpfdoop.py --help
```

## Command line parameters

JPF-Doop has several command line parameters:

* `--package-name` - a required parameter with the name of a package
  you want to test (e.g. `org.apache.commons.collections`).
* `--root` - a required parameter with a directory where source files
  of the package are.
* `--classlist` - an optional parameter (with the default value
  `classlist.txt`) where the list of classes from the package will be
  written to.
* `--timelimit`- an optional parameter (with the default value `120`)
  with a time limit for JPF-Doop, in seconds.
* `--configuration-file` - an optional parameter (with the default
  value `jpfdoop.ini`) with a name of a configuration file in the INI
  file format. More about options within the configuration file are
  given in section *Configuration file*.
* `--randoop-only` - an optional parameter (with the default value
  false) that specifies if only Randoop should be executed.
* `--baseline` - an optional parameter (with the default value false)
  that specifies if only baseline JPF-Doop should be executed. For
  more information on the baseline solution, see an
  [extended abstract][5] on JPF-Doop.
* `--generate-report` - an optional parameter (with the default value
  false) that specifies if a code coverage report should be generated
  once JPF-Doop is done with its execution. If the parameter is
  provided, [JaCoCo][6] will be executed to generate a code coverage
  report for JUnit tests that JPF-Doop generated. If this option is
  not provided, the user can generate the code coverage report
  herself, as described in section *Generating code coverage reports*.

# Generating code coverage reports

JPF-Doop has a script that generates a code coverage report for JUnit
tests that can be used without JPF-Doop, i.e. unit tests can be
generated or obtained in some other way. The script is `report.py`,
and it has a few dependencies. First of all, clone this repository. In
addition, the following is needed:

- [Python][3], version 2.7,
- [Apache Ant][4].

To get help on how to run the code coverage script from the command
line, type:

```
#!bash
$ python report.py --help
```

The script takes 6 arguments, where the `--jacocopath` argument is
optional. There is an example package named `branching` in the
`report-examples/` directory. If you build the example in such a way
that `.class` files are in the same directory where respective `.java`
files are, this is how you would generate a code coverage report for
the example:

```
#!bash
$ python report.py --unittests TestBranching --classpath report-examples/:report-examples/branching/tests/ --sourcepath report-examples/ --packagepath branching --buildpath report-examples/
```

The `report.py` script will generate a code coverage report in two
formats - HTML and XML. Both can be found in a `jacoco-site/`
directory.

[0]: https://bitbucket.org/psycopaths/jpf-jdart
[1]: https://code.google.com/p/randoop/
[2]: https://bitbucket.org/psycopaths/jpf-doop-examples
[3]: http://python.org
[4]: https://ant.apache.org/
[5]: http://dimjasevic.net/marko/wp-content/uploads/2013/09/jpf-workshop-2013.pdf
[6]: http://www.eclemma.org/jacoco/
[7]: http://junit.org/
[8]: http://babelfish.arc.nasa.gov/trac/jpf/wiki
