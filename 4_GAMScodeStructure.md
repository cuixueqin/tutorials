Look at the GAMS code and its structure
================
Kristine Karstens (<karstens@pik-potsdam.de>)
25 April, 2019

-   [1. Introduction](#introduction)
    -   [Learning objectives](#learning-objectives)
-   [2.1. Structure of a module](#structure-of-a-module)
-   [2.2. Structure within each realization](#structure-within-each-realization)
-   [2.3 Coding etiquette variable and parameter naming](#coding-etiquette-variable-and-parameter-naming)
    -   [Coding Etiquette](#coding-etiquette)

1. Introduction
===============

The inner core of the MAgPIE model is written in GAMS. For code execution all parts of the code have to be put into a single file, the `full.gms`. All the code chunks are stored within the `modules` folder. Every module, representing a component of the model, has several realizations. Each module is included with exactly one realization within the final model execution. Within the `modules` folder nevertheless you will find all possible realizations. The configuration settings as e.g. set in `default.cfg` (or inside the run scripts) will determine the realization entering the `full.gms`.

### Learning objectives

The goal of this exercise is to understand the basic structure of the gams code. After completion of this exercise, you'll be able to:

1.  Navigate though the gams code.
2.  Understand the basic structure of modules and realizations.
3.  Understand the basic rules what a variable or parameter name can tell you about its usage.

2.1. Structure of a module
==========================

When you open the `modules` folder you will see a long list of all module and the `include.gms` (ensuring inclusion of all modules into the `full.gms`). Every module folder is build upon the same pattern:

<img src="figures/module_struc.png" alt="structure of any module" width="100%" />
<p class="caption">
structure of any module
</p>

This specific structure ensures

-   listing of all realizations by the module gams-file
-   linking to the specific source code by realization gms-file
-   containing of the source code by realization folders
-   containing overarching input for all realization by input folder.

New realization can be added by keeping that structure (more in `7_advanced_changeCode.Rmd`). In that sense MAgPIE is easily extentable.

2.2. Structure within each realization
======================================

Within a realization the source code is distributed over several gams-files. This is nessessary for ensuring correct order of calculations (before, within and after the optimization). Also the correct interfaces for model input and output is defined by this structure. Within the following table you see the purpose of each gms-file. Note that not every gams-file type is needed in each realization.

<table style="width:100%;">
<colgroup>
<col width="22%" />
<col width="77%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">gms.file</th>
<th align="left">function</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">sets.gms</td>
<td align="left">List sets that are used within this specific realization or are needed for interfaces defined within this realilzation</td>
</tr>
<tr class="even">
<td align="left">input.gms</td>
<td align="left">Load input from <code>any_module/input</code> or <code>any_module/a_realization/input</code></td>
</tr>
<tr class="odd">
<td align="left">declarations.gms</td>
<td align="left">Declare all variables, equations, parameters that are central to this realization.</td>
</tr>
<tr class="even">
<td align="left">preloop.gms</td>
<td align="left">Evaluate containg calculation before the whole run.</td>
</tr>
<tr class="odd">
<td align="left">presolve.gms</td>
<td align="left">Evaluate containg calculation before each time step.</td>
</tr>
<tr class="even">
<td align="left">nl_fix.gms</td>
<td align="left">Fix non-linear behaviour to linear behaviour.</td>
</tr>
<tr class="odd">
<td align="left">nl_release.gms</td>
<td align="left">Release restrictions to linear behaviour again.</td>
</tr>
<tr class="even">
<td align="left">equations.gms</td>
<td align="left">Containing functional relationships that should be fullfilled within the optimization.</td>
</tr>
<tr class="odd">
<td align="left">postsolve.gms</td>
<td align="left">Evaluate containg calculation after each time step, define output.</td>
</tr>
<tr class="even">
<td align="left">not_used.txt</td>
<td align="left">List interfaces (declarated in other modules) that are not used within this realization, but within other realizations of the same module</td>
</tr>
</tbody>
</table>

2.3 Coding etiquette variable and parameter naming
==================================================

MAgPIE module structure is build upon the idea that every module itself is in a way encapsulated just interacting on some **clear defined interface** with other modules. This also reflects the idea that every module is in a way representing a seperated part of the model, that could be representated in a simple our more sophisticated way, without relaying on other modules. In that sense realization are replacable within a module, since all realization of a module have to deliver/interact with the same interface variables. The is ensured by defined rules for variable and parameter naming.

### Coding Etiquette

Use the following prefixes:

    q_ eQuations
    v_ Variables
    s_ Scalars
    f_ File parameters - these parameters contain data as it was read from file
    i_ Input parameters - influencing the optimzation but are not influenced by it
    p_ Processing parameters - influencing optimization and are being influenced by it
    o_ Output parameters - only being influenced by optimization but without effect on the optimization
    x_ eXtremely important output parameters - output parameters, that are necessary for the model to run properly (required by external postprocessing). They must not be removed.
    c_ switches from the Config.gms - parameters, that are switches to choose different scenarios
    m_ Macros

The prefixes have to be extended in some cases by a second letter

    ?m_ module-relevant object - This object is used by at least one module and the core code. Changes related to this object have to be performed carefully.
    ?00_ (a 2-digit number) module-only object This 2-digit number defines the module the object belongs to. The number is used here to make sure that different modules cannot have the same object

Sets are treated slightly different: Instead of adding a prefix sets should get a 2-digit number suffix giving the number of the module in which the set is exclusively used. If the set is used in more than one module no suffix should be given.

In MAgPIE the prefixes have to be extended by a second letter in some more cases

    ?c_ value for the Current timestep - necessary for constraints. Each *c_-object must have a time-depending counterpart
    ?q_ parameter containing the values of an equation
    ?v_ parameter containing the values of a variable

Besides prefixes in MAgPIE also Suffixes should be used. Suffixes should indicate the aggregation of an object:

    (no suffix) highest disaggregation available
    _(setname) aggregation over set
    _reg regional aggregation (exception)
    _glo global aggregation (exception)
