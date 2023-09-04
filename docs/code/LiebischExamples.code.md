# LiebischExamples.groovy
**Source code:**
```groovy
@Grab(group='org.openscience.cdk', module='cdk-smiles', version='2.9')
@Grab(group='org.openscience.cdk', module='cdk-silent', version='2.9')
@Grab(group='org.openscience.cdk', module='cdk-depict', version='2.9')

import org.openscience.cdk.smiles.SmilesParser;
import org.openscience.cdk.interfaces.*;
import org.openscience.cdk.silent.SilentChemObjectBuilder;
import org.openscience.cdk.isomorphism.UniversalIsomorphismTester;
import org.openscience.cdk.depict.DepictionGenerator;

 import java.util.regex.Pattern;
 import java.util.regex.Matcher;

 class LMAbbrevTool {

  static Pattern CEpattern = Pattern.compile("CE\\s(\\d+):(\\d+)")
  static Pattern LPCpattern = Pattern.compile("LPC\\s(\\d+):(\\d+)")

  static boolean canParse(String abbrev) {
    return (cxsmiles(abbrev) != "[*]")
  }

  static String[] makeChain(int intOne, int intTwo, int attachPosition) {
    String[] results = new String[2];
    if (intTwo == 0) {
      results[0] = "OC(=O)" + "C".repeat(intOne-1)
      results[1] = ""
    } else if (intTwo == 1) {
      results[0] = "OC(=O)CC=CC[H]"
      results[1] = " |Sg:n:" + (attachPosition + 2) + 
                   ":x:ht,Sg:n:" + (attachPosition + 5) +
                   ":y:ht| x+y=" + (intOne-3) // -1 for the carbonly + -2 for the C=C
    } else {
      results[0] = "OC(=O)[*]"
      // + 2 because of the 4 atom in the above String minus the starting at 0
      //       and minus because we need one less ';' separator
      results[1] = " |\$" + ";".repeat(attachPosition+2) + "R\$|"
    }
    return results
  }

  static String cxsmiles(String abbrev) {
    if (abbrev.startsWith("CE ")) {
      Matcher matcher = CEpattern.matcher(abbrev)
      if (matcher.find()) {
        String intOne = matcher.group(1);
        String intTwo = matcher.group(2);
        String[] chainInfo = makeChain(
          Integer.valueOf(intOne), Integer.valueOf(intTwo),
          3 // the third atom in the below SMILES
        );
        return "C1[C@H](" + chainInfo[0] +
               ")CC2=CC[C@@]3([H])[C@]4([H])CC[C@]([H])([C@]([H])(C)CCCC(C)C)[C@@]4(C)CC[C@]3([H])[C@@]2(C)C1" + chainInfo[1] 
      }
    } else if (abbrev.startsWith("LPC ")) {
      Matcher matcher = LPCpattern.matcher(abbrev)
      if (matcher.find()) {
        String intOne = matcher.group(1);
        String intTwo = matcher.group(2);
        String[] chainInfo = makeChain(
          Integer.valueOf(intOne), Integer.valueOf(intTwo),
          17 // the 17th atom in the below SMILES
        );
        return "[C@](COP(=O)([O-])OCC[N+](C)(C)C)([H])(O)C" + chainInfo[0] + chainInfo[1]  
      }
    }
    return "[*]";
  }

}

sp = new SmilesParser(
  SilentChemObjectBuilder.getInstance()
)

println LMAbbrevTool.cxsmiles("CE 8:0")
println LMAbbrevTool.cxsmiles("LPC 10:1")
```
**Output:**
```plain
C1[C@H](OC(=O)CCCCCCC)CC2=CC[C@@]3([H])[C@]4([H]...
  )CC[C@]([H])([C@]([H])(C)CCCC(C)C)[C@@]4(C)CC[...
  C@]3([H])[C@@]2(C)C1
[C@](COP(=O)([O-])OCC[N+](C)(C)C)([H])(O)COC(=O)...
  CC=CC[H] |Sg:n:19:x:ht,Sg:n:22:y:ht| x+y=7
```