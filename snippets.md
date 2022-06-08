# Snippets

## Java

### Sort array

```java
myArray.sort(Comparator.comparing(TypeOfObjectsInArray::propertyNameForSort));

// Reversed array
myArray.sort(Comparator.comparing(TypeOfObjectsInArray::propertyNameForSort).reversed());
 ```