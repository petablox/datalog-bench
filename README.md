DatalogBench is a collection of Datalog programs from the literature in various fields including databases, information retrieval, and program analysis.

Its main purpose is to serve as a benchmark suite to evaluate techniques for learning Datalog programs from input-output data. This is an important problem studied in fields such as program synthesis and Inductive Logic Programming, with applications in a variety of domains such as bioinformatics, big-data analytics, natural language processing, networking, program analysis, and robotics.

Besides the target Datalog programs themselves, this collection includes input-output datasets for each program, as well as code to generate candidate rules using various methods of inducing syntactic bias, akin to syntax-guided synthesis (SyGuS) or template rules in meta-interpretive learning.

## 1. Rule Generation
All rule generation algorithms produce candidate rule sets in the format supported by <a href="https://souffle-lang.github.io/">Souffle</a>. We select a subset of these candidate rule sets as our final program. All rule generation algorithms can be found in the <b>rule-gen</b> folder. There are four variants:<br><br>
<b>generate</b>, which is a standard brute-force rule generation algorithm. It first enumerates all possible combinations of relations and variables, then applies a number of filters to remove redundant and clearly incorrect rules. Notably, it filters any rules that have variables that match different types in different relations.
<br>
<b>generate-fast</b>, which, unlike <b>generate</b>, does not enumerate any rules whose types do not match the types of the relations, thus dramatically cutting down on the algorithm's runtime. However, this algorithm offers less control over the candidate rule set generated. The algorithm applies a number of filters to remove redundant and clearly incorrect rules as well.
<br>
<b>generate-back</b>, which is equivalent to <b>generate</b>, but inserts the *Rule* relation, which is what switches on a candidate rule within Souffle, at the front of each rule rather than the back. This increases the speed in which Souffle compiles the candidate rules, since it reads rules from left to right.
<br>
<b>generate-negation</b>, which is equivalent to <b>generate</b>, but is capable of handling negation.
<br>

## 2. Benchmark Structure
Each benchmark is represented by a folder and has a number of supporting files. Note that each benchmark may not contain all files listed below.

### 2.1 Input-Output Data

Input-output datasets are provided in two different formats, called primary and alternate, as described below.

##### Primary format
This format provides input data in <b>\*.facts</b> files and output data in <b>\*.expected</b> files. In particular, the tuples of each input relation <b>R</b> are provided in a file named <b>R.facts</b>, and the expected tuples of each output relation <b>S</b> are provided in a file named <b>S.expected</b>. 
<br>
##### Alternate format
This format provides all input-output data combined in a single <b>.d</b> file. In particular, the input-output data for benchmark <b>B</b> is provided in a file named <b>B.d</b>.
<br>

### 2.2 Rule Generation Data

Rule generation data is provided in two different formats, called primary and alternate, as described below. The primary format stores the candidate rule set in <b>rules.large.dl</b> 

##### Primary format
This format defines input and output relations for the rule generation algorithm in a <b>rules.t</b> file. The rule generation algorithm takes <b>rules.t</b> as input, and enumerates every possible combination of candidate rules. It applies a number of filters to remove redundant and clearly incorrect rules, such as ensuring that the variable numbers are minimized left to right and that variables match the types of the relations throughout the rules. Finally, it produces a <b>rules.large.dl</b> file. There should be one <b>rules.t</b> file per benchmark.
<br><br>
The candidate rules set generated by the rule generation algorithm using the provided <b>rules.t</b> file is stored in a <b>rules.large.dl</b> file. Note that <b>rules.large.dl</b> usually contains between 2 - 10 times the number of candidate rules as <b>rules.small.dl</b> (see [Alternate format]). There should be at most one <b>rules.large.dl</b> file per benchmark.
###### For all these files, X may be "small" or "large". If X is "small," it represents files derived from the alternate (see [Alternate format]) rule generation algorithm. If X is "large", it represents files derived from the primary rule generation algorithm.
The auxillary rule set used to generate coprovenance, the intersection of all possible derivation trees for a given tuple under a candidate rule set, for <b>rules.X.dl</b> is stored in <b>rules_notexists.X.dl</b>. Note that these rules are not part of the candidate rule set. <b>rules_notexists.large.dl</b> is created automatically by the candidate rule generation algorithm when generating <b>rules.large.dl</b>. There should be at most two <b>rules_notexists.X.dl</b> files per benchmark.
<br><br>
The auxillary rule set used to generate allprovenance, the union of all possible derivation trees for a given tuple under a candidate rule set, for <b>rules.X.dl</b> is stored in <b>rules_exists.X.dl</b>. Note that these rules are not part of the candidate rule set. <b>rules_exists.large.dl</b> is created automatically by the candidate rule generation algorithm when generating <b>rules.large.dl</b>. There should be at most two <b>rules_exists.X.dl</b> files per benchmark.
<br><br>
The list of rule numbers which define the candidate rules the algorithm is able to access is stored in <b>ruleNames.X.txt</b> for <b>rules.X.dl</b>. By default, rule numbers for all candidate rules are listed in the file. <b>ruleNames.large.txt</b> is created automatically by the candidate rule generation algorithm when generating <b>rules.large.dl</b>. There should be at most two <b>ruleNames.X.txt</b> files per benchmark.
<br>
##### Alternate format
This format provides rule template data in a <b>\*.tp</b> file, which is required for an alternate rule generation procedure described in this <a href="https://www.cis.upenn.edu/~mhnaik/papers/fse18.pdf">FSE 2018</a> paper. In particular, the template data for benchmark <b>B</b> is provided in a file named <b>B.tp</b>. There should be at most one <b>\*.tp</b> file per benchmark.
<br><br>
The candidate rules set sourced from an alternate rule generation algorithm from a previous project is stored in a <b>rules.small.dl</b> file. There should be at most one <b>rules.small.dl</b> file per benchmark.
<br>

### 2.3 Variants
Additionally, a few benchmarks (*quickfoil-webkb* and *quickfoil-bongard*) contain variants: 
<br>
&emsp;*quickfoil-webkb* has 8 variants which are generated from different data sources but have similar size candidate rule sets.
<br>
&emsp;*quickfoil-bongard* has 10 variants which are generated from the same data source but have varying size candidate rule sets.


