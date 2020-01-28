# MCS.groovy
**Source code:**
```groovy
import org.openscience.cdk.smiles.SmilesParser;
import org.openscience.cdk.interfaces.*;
import org.openscience.cdk.DefaultChemObjectBuilder;
import org.openscience.cdk.isomorphism.UniversalIsomorphismTester;
@Grab(group='org.openscience.cdk', module='cdk-smiles', version='2.3')
@Grab(group='org.openscience.cdk', module='cdk-data', version='2.3')

uiTester = new UniversalIsomorphismTester();
sp = new SmilesParser(
  DefaultChemObjectBuilder.getInstance()
)
mol1 = sp.parseSmiles("NCC(=O)OC1=CC=CC=C1C(=O)O")
mol2 = sp.parseSmiles("CCC(=O)OC1=CC=CC=C1C(=O)O")
List<IAtomContainer> list = uiTester.getOverlaps(mol1, mol2);
println "Overlaps: ${list.size()}"
for (ac in list) {
  println "  atom count in overlap: ${ac.atomCount}"
}
```
**Output:**
```plain
Overlaps: 1
  atom count in overlap: 13
```