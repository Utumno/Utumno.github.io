---
layout: post
title: "Java memorabilia"
date: 2014-04-06 17:31:00 +0100
comments: true
categories:
---

Remember erasure ?

	import java.util.ArrayList;
	import java.util.List;

	public class Main {

		public static void main(String[] args) {
			List<String> strings = new ArrayList<>();
			((List) strings).add(42); // compiler warning - ok at runtime
			// String s = strings.get(0); //  CCE - Compiler-generated cast
			System.out.println("strings: " + strings); // strings: [42]
			System.out.println(strings.contains(42)); // true
		}
	}

In other words:

		final HashSet<Integer> hashSet = new HashSet<>();
		// Set<String> cast = (Set<String>) hashSet; // error
		Set<String> cast = (Set) hashSet; // warning

From: [here](http://stackoverflow.com/questions/19610569/android-sharedpreferences-null-keys-values-and-sets-corner-cases)
and [here](http://books.google.gr/books?id=ka2VUBqHiWkC&pg=PA144&lpg=PA144&source=bl&ots=yYKmRmr5Q3&sig=HESfg8Y3UaprOvN7GyF1XYN-DW8&hl=en&sa=X&ei=0Pe6UunkBMavygPz64CAAw&redir_esc=y#v=onepage&q&f=false)

Remember printing (and [varargs](http://stackoverflow.com/questions/2925153/can-i-pass-an-array-as-arguments-to-a-method-with-variable-arguments-in-java/2926653#2926653)) ?

[```Arrays.toString(map.entrySet().toArray());```][1]

as opposed to:

[```int[] a = { 1, 2, 3 };
System.out.println(Arrays.asList(a)); // __I@70cdd2_ // _ for brackets
```][2]

[1]: http://stackoverflow.com/a/19638956/281545
[2]: http://stackoverflow.com/a/17520983/281545
