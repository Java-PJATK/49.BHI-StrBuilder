# BHI-StrBuilder
Listing 49 BHI-StrBuilder/StrBuilder.java Page 85

Cont. from [48.BHH-Strings](https://github.com/Java-PJATK/48.BHH-Strings)

## 9.2 Class StringBuilder

The fact that `String`s are immutable has many advantages but the downside is that manipulating strings always involves creation of new objects of type `String`. 

For example, when we want to append something to a string using `s=s+"something"`, we actually do _not_ append anything to an existing string; we just create a completely new object and the reference to this new object assign back to `s` erasing its previous contents (but the string that was referenced to by s before still exists on the heap in memory).

Problems related to string being immutable are addressed by the class `StringBuilder`.

Object of this type are similar to `String` objects but _are_ modifiable. 

Internally, they allocate an array of characters and operate on it; when it becomes too small, another, twice as big array, is allocated and all existing elements are copied to it. 

As at each reallocation much bigger array is allocated, its size grows rapidly and hence not many such reallocations will ever be required. 

Probably the most useful methods of `StringBuilder` are `append`, which appends to a string another string, `insert`, which inserts a string on an arbitrary position into an existing string; as a matter of fact almost anything can be inserted, in particular numbers and also objects (in that case, the result of invocation of `toString` will be inserted). 

Also, the method `delete` is often useful: it can remove a fragment of the string. Invoking `toString` on a `StringBuilder` object gives us the final, immutable String object. Many of the methods mentioned above return this object, so their invocations may be chained, as in the example below:

## Listing 49 BHI-StrBuilder/StrBuilder.java

```java
public class StrBuilder {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("three");
        sb.append(",four")
          .insert(0,"twoxx,")
          .insert(0,"one,")
          .delete(sb.indexOf("x"), sb.indexOf("x")+2);
        System.out.println("sb = " + sb.toString());

        final int NUMB = 50_000;
        long startTime;

        startTime = System.nanoTime();
        String s = "0";
        for (int i = 1; i <= NUMB; ++i)
            s = s + i;
        long elapsedS = System.nanoTime() - startTime;
        System.out.printf(" String: %10d ns = %.3f sec%n",
                                 elapsedS, elapsedS*1e-9);

        startTime = System.nanoTime();
        StringBuilder builder = new StringBuilder("0");
        for (int i = 0; i <= NUMB; ++i)
            builder.append(i);
        long elapsedB = System.nanoTime() - startTime;
        System.out.printf("Builder: %10d ns = %.3f sec%n",
                                 elapsedB, elapsedB*1e-9);

        System.out.printf("elapsedS/elapsedB = %.2f%n",
                           (double)elapsedS/elapsedB);
    }
}

```

The program prints

```
    sb = one,two,three,four
     String:  735807986 ns = 0,736 sec
    Builder:    2263932 ns = 0,002 sec
    elapsedS/elapsedB = 325,01
```

which shows that operations on `StringBuilder` objects can be orders of magnitude faster than analogous operations on `String`s.

Next [Section 10 Introduction to inheritance 50.51.52.DJV-Inherit](https://github.com/Java-PJATK/50.51.52.DJV-Inherit)
