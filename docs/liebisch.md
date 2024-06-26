# Liebisch abbreviations

The Liebisch abbrevations are a shorthand notation for lipid structures. This is particularly useful
when the lipid is incompletely characterized [<a href="#citeref1">1</a>,<a href="#citeref2">2</a>].

## CXSMILES

We can use the following code to depict `CE 16:1`, using CXSMILES as intermediate step:

**Script** [code/DepictLipidAbbrev.groovy](code/DepictLipidAbbrev.code.md)
```groovy
cxsmiles = LMAbbrevTool.cxsmiles("CE 16:1")
mol1 = sp.parseSmiles(cxsmiles)
new DepictionGenerator()
  .withMolTitle()
  .depict(mol1)
  .writeTo("CE_16_1.svg");
```

The output looks like this:

<img src="./images/generated/CE_16_1.svg" width="400" alt="Depiction of the 2D structure of CE 16:1" />

### Some examples

Instead of depicting, we can just output the CXSMILES generated from the shorthand notation:

**Script** [code/LiebischExamples.groovy](code/LiebischExamples.code.md)
```groovy
println LMAbbrevTool.cxsmiles("CE 8:0")
println LMAbbrevTool.cxsmiles("FA 14:1")
println LMAbbrevTool.cxsmiles("LPC 10:1")
```

The output looks like this:

```
C1[C@H](OC(=O)CCCCCCC)CC2=CC[C@@]3([H])[C@]4([H]...
  )CC[C@]([H])([C@]([H])(C)CCCC(C)C)[C@@]4(C)CC[...
  C@]3([H])[C@@]2(C)C1
OC(=O)CC=CC[H] |Sg:n:3:x:ht,Sg:n:6:y:ht| x+y=11
[C@](COP(=O)([O-])OCC[N+](C)(C)C)([H])(O)COC(=O)...
  CC=CC[H] |Sg:n:19:x:ht,Sg:n:22:y:ht| x+y=7
```

## Structure enumeration

The abbreviation `FA 14:1` indicates the structure is a fatty acid with an chain with 13 carbon atoms
(and 14 carbons in the compound in total), and with one double bond equivalent. There can be branching
and there can even be rings. For example, this is said to be a valid `FA 14:1`:

<img src="./images/generated/FA_14_1.svg" width="400" alt="Depiction of a theoretical structure of FA 14:1" />
<!-- <code>FA14Branching</code> -->

If we take this loose definition, the number of possible structures is high. With Surge we can enumerate
the number of structures

## References

1. <a name="citeref1"></a>Liebisch G, Vizcaíno JA, Köfeler H, Trötzmüller M, Griffiths WJ, Schmitz G, et al. Shorthand notation for lipid structures derived from mass spectrometry. J Lipid Res. 2013 Apr 2;54(6):1523–30.  doi:[10.1194/JLR.M033506](https://doi.org/10.1194/JLR.M033506) ([Scholia](https://scholia.toolforge.org/doi/10.1194/JLR.M033506))
2. <a name="citeref2"></a>Liebisch G, Fahy E, Aoki J, Dennis EA, Durand T, Ejsing C, et al. Update on LIPID MAPS Classification, Nomenclature and Shorthand Notation for MS-derived Lipid Structures. J Lipid Res. 2020 Oct 9;  doi:[10.1194/JLR.S120001025](https://doi.org/10.1194/JLR.S120001025) ([Scholia](https://scholia.toolforge.org/doi/10.1194/JLR.S120001025))


