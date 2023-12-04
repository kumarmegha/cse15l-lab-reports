# LAB REPORT 5: Putting it All Together

## Part 1- Debugging Scenario

1. PRETEND ED-STEM POST:
Hi,
I'm facing a strange issue with my Java code.
I'm working on a list merging function, and even though I've written tests,
the merged list is not sorted as expected.

In the `ListExamplesTests.java` file, I'm merging two lists ["apple", "banana"] and ["orange", "pear"]. 
The expected result should be ["apple", "banana", "orange", "pear"], but the actual result is unexpected. 
I suspect there's a bug in my code, possibly related to the sorting logic in the `merge` method.

2. RESPONSE FROM TA:

Hello! Thanks for providing the details. 
To investigate further, could you add some debugging information to your `merge` method? 
Specifically, print the values of `list1.get(index1)` and `list2.get(index2)` inside the loop where you're merging the lists. 
This will help us understand what values are being compared during the sorting process. 

3. OUTPUT FROM SUGGESTION:
```
while (index1 < list1.size() && index2 < list2.size()) {
    System.out.println("Comparing: " + list1.get(index1) + " and " + list2.get(index2));
    if (list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
    } else {
        result.add(list2.get(index2));
        index2 += 1;
    }
}
```

The output from the added debugging statements reveals that the values being compared during the sorting process are concatenated 
instead of being compared individually. The bug in the merge method leads to unexpected behavior in the sorting process.

4. SUMMARY OF INFORMATION:

DIRECTORY STRUCTURE:

A directory named debugging_scenario.
Inside this directory, the following files:
ListExamples.java (with the bug introduced)
ListExamplesTests.java (containing the test with the symptom)
run_and_debug.sh (Bash script for compiling and running tests)
The lib directory containing hamcrest-core-1.3.jar and junit-4.13.2.jar

CODE BEFORE:
```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

    static List<String> filter(List<String> list, StringChecker sc) {
        List<String> result = new ArrayList<>();
        for(String s: list) {
            if(sc.checkString(s)) {
                result.add(0, s);
            }
        }
        return result;
    }

    static List<String> merge(List<String> list1, List<String> list2) {
        List<String> result = new ArrayList<>();
        int index1 = 0, index2 = 0;
        while(index1 < list1.size() && index2 < list2.size()) {
            if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
                // Intentional bug: Concatenate the elements instead of comparing them
                result.add(list1.get(index1) + list2.get(index2));
                index1 += 1;
            }
            else {
                result.add(list2.get(index2));
                index2 += 1;
            }
        }
        while(index1 < list1.size()) {
            result.add(list1.get(index1));
            index1 += 1;
        }
        while(index2 < list2.size()) {
            result.add(list2.get(index2));
            index2 += 1;
        }
        return result;
    }
}
```

CODE AFTER:
```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;
import java.util.ArrayList;

public class ListExamplesTests {

    @Test(timeout = 500)
    public void testMergeWithConcatenationBug() {
        List<String> l1 = new ArrayList<String>(Arrays.asList("apple", "banana"));
        List<String> l2 = new ArrayList<String>(Arrays.asList("orange", "pear"));
        
        // This is what we expect to happen, but the bug will cause unexpected behavior
        List<String> expected = new ArrayList<String>(Arrays.asList("apple", "banana", "orange", "pear"));
        
        // Uncomment the line below to see the unexpected behavior caused by the bug
        // List<String> result = ListExamples.merge(l1, l2);

        // Uncomment the line below to see the expected behavior
        List<String> result = ListExamples.mergeFixed(l1, l2);
        
        assertArrayEquals(expected.toArray(), result.toArray());
    }
}
```
Contents of run_and_debug.sh Before Fixing the Bug:

bash
#!/bin/bash
```
javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests
```
Full Command Line to Trigger the Bug:

bash
```
./run_and_debug.sh
```
## Part 2- Reflection

I had heard about Vim through my research lab before, 
but I had no idea what it was used for, or how to use it.
Now that this class has taught me the fundamental commands
and how-to for Vim, I can already see myself applying it in 
my lab and in the future, so for me that's the coolest takeaway
from this class!
