# Utility Classes

- [Arrays](#arrays)
- [Collections](#collections)
- [Math](#math)

## Arrays
| Function | Description |
| ---- | ---- |
| `sort(array)` | Sort the elements in ascending order. |
| `sort(array, Collections.reverseOrder())` | Sort the elements in descending order. |
| `copyOfRange(array, i, j)` | Get the sub-array [i, j-1]. |
| `fill(array, value)` | Fill all the elements in the array by that value. |

- Custom sorting `Arrays.sort` (`String[] arr`)
  ```java
  Arrays.sort(arr, (a, b)->a.length() - b.length());
  Arrays.sort(arr, (a, b)->{ return a.length() - b.length(); });
  ```
- Custom sorting `Arrays.sort` (`int[][] arr`)
  ```java
  Arrays.sort(arr, (a, b) -> a[0] - b[0]);
  Arrays.sort(arr, (a, b) -> Integer.compare(a[0], b[0]));
  ```
  
## Collections
| Function | Description |
| ---- | ---- |
| `sort()` | Sort the elements in the collection (Default is ascending order). |
| `max()` | Get the maximum element in the collection. |
| `min()` | Get the minimum element in the collection. |
| `reverse()` | Reverse the order of the elements (Use this function to get the descending order). |

- Custom Sorting `Collections.sort` (`List<int[]> list`)
  ```java
  Collections.sort(list, (a, b) -> {
      int cmp = Integer.compare(a[0], b[0]);
      if (cmp != 0) {
          return cmp;
      }
      return Integer.compare(a[1], b[1]);
  });
  ```

## Math
| Function | Description |
|----|----|
| `max()` | Get the max value among 2 values. |
| `min()` | Get the min value among 2 values. |
