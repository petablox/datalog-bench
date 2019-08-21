DatalogBench is a collection of Datalog programs from the literature in various fields including databases, information retrieval, and program analysis.

The primary goal of this collection is to benchmark techniques for learning Datalog programs from input-output data. This is an important problem studied in fields such as program synthesis and Inductive Logic Programming, with applications in a variety of domains such as bioinformatics, big-data analytics, natural language processing, networking, program analysis, and robotics.

Besides the target Datalog programs themselves, this collection includes input-output datasets for each program, as well as code to generate candidate rules using various methods of inducing syntactic bias, akin to syntax-guided synthesis (SyGuS) or template rules in meta-interpretive learning.

## Benchmark Structure
Note that each benchmark may not contain all files listed below.

Additionally, a few benchmarks (*quickfoil-webkb* and *quickfoil-bongard*) contain variants: 
<br>
&emsp;*quickfoil-webkb* has 8 variants which are generated from different data sources but have similar size candidate rule sets.
<br>
&emsp;*quickfoil-bongard* has 10 variants which are generated from the same data source but have varying size candidate rule sets.

### Input-Output Data

Input-output datasets are provided in two different formats, called primary and alternate.

##### Primary format
This format stores input data in <b>\*.facts</b> files and output data in <b>\*.expected</b> files.  Each input relation's tuples are stored in a separate <b>.facts</b> file, and each output relation's expected tuples are stored in a separate <b>.expected</b> file. 
<br>
##### Alternate format
This format stores all input-output data combined in a single <b>.d</b> file.
<br>

### Rule Generation Data
<b>rules.t</b> -- template file. Defines input and output relations for the brute force candidate rule generation algorithm. There should be one rules.t file per benchmark. The rule generation algorithm takes <b>rules.t</b> as input, and enumerates every possible combination of candidate rules. It applies a number of filters to remove redundant and clearly incorrect rules, such as ensuring that the variable numbers are minimized left to right and that variables match the types of the relations throughout the rules. Finally, it produces a <b>rules.large.dl</b> file.
<br>
<b>rules.large.dl</b> -- candidate rules set generated by the brute force candidate rule generation algorithm (using the provided rules.t file). There should be one rules.large.dl file per benchmark. Note that rules.large.dl usually contains between 2 - 10 times the number of candidate rules as rules.small.dl. 
<br>
<b>rules_notexists.\*.dl</b> -- auxillary rule set used to generate coprovenance - note that these rules are not candidate rules. Created automatically by the candidate rule generation algorithm when generating the candidate rule set. There should be at most one rules_notexists.small.dl and at most one rules_notexists.large.dl file per benchmark - these respectively correspond with rules.small.dl and rules.large.dl.
<br>
<b>rules_exists.\*.dl</b> -- auxillary rule set used to generate allprovenance - note that these rules are not candidate rules. Created automatically by the candidate rule generation algorithm when generating the candidate rule set. There should be at most one rules_exists.small.dl and at most one rules_exists.large.dl file per benchmark - these respectively correspond with rules.small.dl and rules.large.dl.
<br>
<b>ruleNames.\*.txt</b> -- list of rule numbers which define the candidate rules the algorithm has access to. By default, this is a list of rule numbers for all candidate rules.  Created automatically by the candidate rule generation algorithm when generating the candidate rule set. There should be at most one ruleNames.small.txt and at most one ruleNames.large.txt file per benchmark - these respectively correspond with ruleNames.small.txt and ruleNames.large.txt.
<br>
##### Alternate format
<b>\*.tp</b> -- alternate format template file. Contains meta-rules required for alternative rule generation procedure. There may be one .tp file per benchmark.
<br>
<b>rules.small.dl</b> -- candidate rules set sourced from an alternate rule generation algorithm from a previous project. There should be one rules.small.dl file per benchmark.
<br>

## Rule Generation
All rule generation algorithms can be found in the <b>rule-gen</b> folder. There are four variants:<br><br>
<b>generate</b> -- standard rule generation. Enumerates all possible combinations of relations and variables, then applies a number of filters to remove redundant and clearly incorrect rules. Notably, it filters any rules that have variables that match different types in different relations.
<br>
<b>generate-fast</b> -- does not enumerate combinations that match different types in different relations, cutting down on the algorithm's runtime (see *iterate-args* function). However, offers less control over candidate rule set generated. The algorithm applies a number of filters to remove redundant and clearly incorrect rules as well.
<br>
<b>generate-back</b> -- standard rule generation, but inserts *Rule* relation at the front of each rule rather than the back.
<br>
<b>generate-negation</b> -- standard rule generation, but is capable of handling negation.
<br>
