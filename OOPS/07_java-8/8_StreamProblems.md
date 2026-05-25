## Stream Problems:

## Level 1 — Basic Stream Problems

**1. Filter Even Numbers**

```java
List<Integer> nums = Arrays.asList(1,2,3,4,5,6);

List<Integer> even =
        nums.stream()
            .filter(n -> n % 2 == 0)
            .toList();

System.out.println(even);
```

**2. Convert Names to Uppercase**

```java
List<String> names = Arrays.asList("java", "spring", "boot");

List<String> result = names.stream()
                    	.map(String::toUpperCase)
                        .toList();

        //Using Lambda
        result =
                names.stream()
                        .map(x->x.toUpperCase())
                        .toList();

        System.out.println(result);
```

**3. Find Sum of Numbers**

```java
        List<Integer> nums = Arrays.asList(10,20,30);

        int sum =
                nums.stream()
                        .reduce(0, Integer::sum);

        //Using Lambda
        sum = nums.stream().reduce(0, (a,b)->a+b);

        System.out.println(sum);
```

**4. Count Strings Starting With A**

```java
List<String> names = Arrays.asList("Ajay", "Bob", "Anu");

long count =
        names.stream()
             .filter(s -> s.startsWith("A"))
             .count();

System.out.println(count);
```

## Level 2 — Important Interview Questions

**5. Find Maximum Number**

```java
        List<Integer> nums = Arrays.asList(4,7,2,9);

        int max =
                nums.stream()
                        .max(Integer::compare)
                        .get();

        //Using Lambda
        max = nums.stream().max((a,b)->a<b?-1:a>b?1:0).get();

        System.out.println(max);
```

**6. Remove Duplicates**

```java
List<Integer> nums = Arrays.asList(1,2,2,3,3,4);

List<Integer> unique =
        nums.stream()
            .distinct()
            .toList();

System.out.println(unique);
```

**7. Sort Employees by Salary**

```java
employees.stream()
         .sorted(Comparator.comparing(Employee::getSalary))
         .forEach(System.out::println);

//Using Lambda
employees.stream()
                .sorted((a,b)->a.getSalary()<b.getSalary()?-1:a.getSalary()>b.getSalary()?1:0)
                .forEach(System.out::println);
```

**8. Find First Element**

```java
List<Integer> nums = Arrays.asList(5,10,15);

int first =
        nums.stream()
            .findFirst()
            .get();

System.out.println(first);
```

## Level 3 — Frequently Asked Stream Questions

**9. Find Second Highest Number**

```java
        List<Integer> nums = Arrays.asList(10,40,20,50,30);

        int secondHighest =
                nums.stream()
                        .distinct()
                        .sorted(Comparator.reverseOrder())
                        .skip(1)
                        .findFirst()
                        .get();

        //Using Lambda
        secondHighest = nums.stream().distinct().sorted((a,b)->a<b?1:a>b?-1:0).skip(1).findFirst().get();

        System.out.println(secondHighest);
```

**10. Find Frequency of Characters**

```java
String str = "banana";

Map<Character, Long> map =
        str.chars()
           .mapToObj(c -> (char)c)
           .collect(Collectors.groupingBy(
                   c -> c,
                   Collectors.counting()
           ));

System.out.println(map);
```

**11. Group Employees by Department**

```java
        Map<String, List<Employee>> map =
                employees.stream()
                        .collect(Collectors.groupingBy(
                                Employee::getDepartment
                        ));

        //Using Lambda
        Map<String, List<Employee>> map =
                employees.stream()
                        .collect(Collectors.groupingBy(
                                x->x.getDepartment()
                        ));

		//Get count;

        Map<String, Long> map =
                employees.stream()
                        .collect(Collectors.groupingBy(
                                x->x.getDepartment(),
                                Collectors.counting()
                        ));

```

**12. Find Duplicate Elements**

```java
List<Integer> nums = Arrays.asList(1,2,3,2,4,1);

Set<Integer> set = new HashSet<>();

List<Integer> duplicates =
        nums.stream()
            .filter(n -> !set.add(n))
            .toList();

System.out.println(duplicates);
```

**13. Join Strings**

```java
List<String> names = Arrays.asList("Java", "Spring", "Boot");

String result =
        names.stream()
             .collect(Collectors.joining(","));

System.out.println(result);
```

**14. Partition Even and Odd Numbers**

```java
Map<Boolean, List<Integer>> map =
        nums.stream()
            .collect(Collectors.partitioningBy(
                    n -> n % 2 == 0
            ));

// Get count
Map<Boolean, Long> map =
        nums.stream()
            .collect(Collectors.partitioningBy(
                    n -> n % 2 == 0,
					Collectors.counting()
            ));
```

**15. Find Employee With Highest Salary**

```java
        Employee emp =
                employees.stream()
                        .max(Comparator.comparing(Employee::getSalary))
                        .get();

        //Using Lambda
        Employee emp =
                employees.stream()
                        .max((a,b)->a.getSalary()<b.getSalary()?-1:a.getSalary()>b.getSalary()?1:0)
                        .get();
```

**16.  Given a list of transactions, find the sum of transaction amounts for each day using Java streams:**

```java
List<Transaction> transactions = Arrays.asList(
    new Transaction("2022-01-01", 100),
    new Transaction("2022-01-01", 200),
    new Transaction("2022-01-02", 300),
    new Transaction("2022-01-02", 400),
    new Transaction("2022-01-03", 500)
);

Map<String, Integer> sumByDay = transactions
                        .stream()
                        .collect(Collectors.groupingBy(Transaction::getDate,
                               Collectors.summingInt(Transaction::getAmount)));
```