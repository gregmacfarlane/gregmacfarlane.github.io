---
title: "Java systems and OpenTripPlanner for R"
author: Gregory Macfarlane
date: '2021-06-04'
slug: []
categories: []
tags: []
---

I am using the `opentripplanner` package, which requires a specific version of Java (1.8, to be precise). This is an older version of Java, and so I have more than one JDK on my system (MacOS 11.2.3)

I have been able to wrangle my java versions using the `jenv` command line tool. But Rstudio seems to ignore both the global and locally-defined java versions that I have specified. If I open a standard terminal pointed at my project folder, I get the same version of java with both the terminal and with R inside of that terminal.

``` sh
~/p/C$ java -version                                                                                                                                     (base)
java version "1.8.0_192"
Java(TM) SE Runtime Environment (build 1.8.0_192-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.192-b12, mixed mode)
~/p/Community_Resources_2021 (main↑1|✚3…) $ R                                                                                                                                                 (base)

R version 4.0.5 (2021-03-31) -- "Shake and Throw"
Copyright (C) 2021 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin17.0 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> system2("java", "-version")
java version "1.8.0_192"
Java(TM) SE Runtime Environment (build 1.8.0_192-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.192-b12, mixed mode)
```

If I open a terminal in Rstudio, I also get the correct version of Java. But in the Rstudio R console, I get the most recent version of Java no matter what I have set.

    system2("java", "-version")
    # openjdk version "15.0.1" 2020-10-20
    # OpenJDK Runtime Environment (build 15.0.1+9)
    # OpenJDK 64-Bit Server VM (build 15.0.1+9, mixed mode, sharing)

Turns out the issue is that Rstudio will use the `JAVA_HOME` variable regardless of what is set to run in the shell. This might not be ideal! But it is certainly not alone. I tried to follow the instructions to export this variable using `jenv` that I could piece together from [this issue](https://github.com/jenv/jenv/issues/44), but I was not entirely successful.

I was able to fix the issue by creating a project-level `.Renviron` file setting the `JAVA_HOME` variable explicitly.

    JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_192.jdk/Contents/Home"
