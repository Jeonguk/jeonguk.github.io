---
layout: post
title:  "Arraylist vs LinkedList Performance Test "
date:   2018-05-17 11:09:10 +0700
categories: [java,language]
---

# Arraylist vs LinkedList Performance Test

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.LinkedList;

public class PerformanceTest {

	@Test
	public void performance_test() {
		ArrayList arrayList = new ArrayList();
		LinkedList linkedList = new LinkedList();

		// ArrayList add
		long startTime = System.nanoTime();

		for (int i = 0; i < 100000; i++) {
			arrayList.add(i);
		}
		long endTime = System.nanoTime();
		long duration = endTime - startTime;
		System.out.println("ArrayList add:  " + duration);

		// LinkedList add
		startTime = System.nanoTime();

		for (int i = 0; i < 100000; i++) {
			linkedList.add(i);
		}
		endTime = System.nanoTime();
		duration = endTime - startTime;
		System.out.println("LinkedList add: " + duration);

		// ArrayList get
		startTime = System.nanoTime();

		for (int i = 0; i < 10000; i++) {
			arrayList.get(i);
		}
		endTime = System.nanoTime();
		duration = endTime - startTime;
		System.out.println("ArrayList get:  " + duration);

		// LinkedList get
		startTime = System.nanoTime();

		for (int i = 0; i < 10000; i++) {
			linkedList.get(i);
		}
		endTime = System.nanoTime();
		duration = endTime - startTime;
		System.out.println("LinkedList get: " + duration);

//
//		// ArrayList remove
//		startTime = System.nanoTime();
//
//		for (int i = 9999; i >=0; i--) {
//			arrayList.remove(i);
//		}
//		endTime = System.nanoTime();
//		duration = endTime - startTime;
//		System.out.println("ArrayList remove:  " + duration);
//
//		// LinkedList remove
//		startTime = System.nanoTime();
//
//		for (int i = 9999; i >=0; i--) {
//			linkedList.remove(i);
//		}
//		endTime = System.nanoTime();
//		duration = endTime - startTime;
//		System.out.println("LinkedList remove: " + duration);


		// ArrayList replace
		startTime = System.nanoTime();
		for (int i = 9999; i >=0; i--) {
			arrayList.set(i, i * 2);
		}
		endTime = System.nanoTime();
		duration = endTime - startTime;
		System.out.println("ArrayList replace:  " + duration);

		// LinkedList replace
		startTime = System.nanoTime();

		for (int i = 9999; i >=0; i--) {
			linkedList.set(i, i * 2);
		}
		endTime = System.nanoTime();
		duration = endTime - startTime;
		System.out.println("LinkedList replace: " + duration);
	}
}
```

```
ArrayList add:  12593567
LinkedList add: 8288236
ArrayList get:  98902
LinkedList get: 86939774
ArrayList replace:  273708
LinkedList replace: 86691311
```