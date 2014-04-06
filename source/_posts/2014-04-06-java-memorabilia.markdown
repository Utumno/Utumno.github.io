---
layout: post
title: "Java memorabilia"
date: 2014-04-06 17:31:00 +0100
comments: true
categories:
---

Remember erasure ?

```
import java.util.ArrayList;
import java.util.List;

public class Main {

	public static void main(String[] args) {
		List<String> strings = new ArrayList<>();
		((List) strings).add(42); // no problem at all
		// String s = strings.get(0); //  CCE - Compiler-generated cast
		System.out.println("strings: " + strings); // strings: [42]
		System.out.println(strings.contains(42)); // true
	}
}
```

From: [here](http://stackoverflow.com/questions/19610569/android-sharedpreferences-null-keys-values-and-sets-corner-cases)
and [here](http://books.google.gr/books?id=ka2VUBqHiWkC&pg=PA144&lpg=PA144&source=bl&ots=yYKmRmr5Q3&sig=HESfg8Y3UaprOvN7GyF1XYN-DW8&hl=en&sa=X&ei=0Pe6UunkBMavygPz64CAAw&redir_esc=y#v=onepage&q&f=false)
