# Data Structure

- [Array](#array)
- [List](#list)
- [Map](#map)
- [Stack](#stack)
- [Queue](#queue)
- [PriorityQueue](#priorityqueue)
- [Set](#set)
- [TreeMap](#treemap)

## Array
- **1-D array**
   - Initialization
     ```java
     int[] array1 = new int[10];
     int[] array2 = new int[] {10,20,30,40,50,60};      // Intialize 1-D array with values
     ```
- **2-D array**
   - Initialization
     ```java
     int[][] array1 = new int[10][9];
     int[][] array2 = new int[10][];                    // OK
     int[][] array3 = {{1, 2, 3}, {4, 5, 6}};           // Intialize 1-D array with values
     ```
   - Traversal
     ```java
     int[][] array = new int[3][3]; 
     for (int i = 0; i < array.length; i++) { 
         for (int j = 0; j < array[i].length; j++) { 
             array[i][j] = i + j; 
         } 
     }
     ```
   - Assign by row
     ```java
     int[][] array = new int[3][3];
     for (int i = 0; i < array.length; i++) { 
         for (int j = 0; j < array[i].length; j++) { 
             array[i] = new int[] {10,20,30,40,50,60};
         } 
     }
     ```  

## List
- **Initialization**
   - **Initialize a list from array**
     ```java
     Integer[] array = {1, 2, 3, 4, 5};
     List<Integer> list = Arrays.asList(array);
     ```
   - **Initialize a list of lists**
     ```java
     List<List<Integer>> list = new ArrayList<>();
     ```
   - **Initialize a linked list**
     ```java
     LinkedList<String> linkedList = new LinkedList<>();
     ```
- **Sorting**
   - **Sort a list in ascending order**
     ```java
     Collections.sort(list);
     ```
     > NOTE: This function will not return a sorted list. It will sort the `list` in-place.
   - **Sort a list in descending order**
     ```java
     Collections.sort(list);
     Collections.reverse(list);
     ```
- **Copying**
   - **Deep copy a list**
     ```java
     List<Integer> list2 = new ArrayList<>(list1);
     ```
- **Common functions for list**
  | Function | Description |
  | ---- | ---- |
  | `sublist(i,j)` | Get the sub-list `[i,j-1]` of the existing list. |
  | `disjoint(list1, list2)` | Return true if there is no common elements in both lists. | 
  
## Map
- **Initialize a map with keys and values**
  ```java
  Map<String, String> map = new HashMap<String, String>() {
      {
          put("2", "abc");
          put("3", "def");
          put("4", "ghi");
          put("5", "jkl");
      }
  };
  ```
- **Traverse**
   - Solution 1:
     ```java
     for(Map.Entry<String, String> entry : map.entrySet()) {
         System.out.println(entry.getKey() + " " + entry.getValue());
     }
     ```
   - Solution 2:
     ```java
     for(String key : map.keySet()) {
         System.out.println(key + " " + map.get(key));
     }
     ```
- **Build 1:n relationship**
  ```java
  Map<Integer, List<Integer>> map = new HashMap<>();
    for (int i = 0; i < edges.length; i++) {
        /* get key and value */
    
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(value);
    }
  ```
- **Convert all the values into a list**
  ```java
  Map<Integer, String> map = new HashMap<>();
  List<String> list = new ArrayList<>(map.values());
  ```
- **Common functions for stack**
  | Function | Description |
  | ---- | ---- |
  | `get` | Get the value by key. |
  | `getOrDefault(key, defaultValue)` | Get the value by key. If key is not in the map, return the default the value. |
  | `put` | Add or update the key-value pair into map. |
  | `putIfAbsent(key, value)` | Only add the value if the key doesn't exist in the map. |
  | `keySet` | Get all the keys. |
  | `values` | Get all the values. |

### Stack
- **Initialize a stack**
  ```java
  Stack<Integer> stack = new Stack<>();
  ```
- **Use `pop()` with caution**
   - Before you call `pop()`, you need to check the stack is empty or not.
- **Common functions for stack**
  | Function | Description |
  | ---- | ---- |
  | `push` | Push an element on the top of the stack. |
  | `pop` | Remove and return the top element of the stack. An `EmptyStackException` exception is thrown if we call `pop()` when the invoking stack is empty. |
  | `peek` | Return the element on the top of the stack, but does not remove it. |
  | `empty` | Return `true` if nothing is on the top of the stack. Else, returns `false`. |
  | `search` | It determines whether an object exists in the stack. If the element is found, it returns the position of the element from the top of the stack. Else, it returns -1. |



### Queue
- **Initialize a queue**
  ```java
  Queue<Integer> queue = new LinkedList<>();
  ```
- **Common functions for queue**

  | Functions | Description |
  | ---- | ---- |
  | `add` | Add element at the tail of queue. |
  | `isEmpty` | Check the queue is empty or not. |
  | `peek` | Return (but NOT remove) the element at the head element of queue. Return `null` if the queue is empty. |
  | `remove` | Remove and returns the head element of the queue. An `NoSuchElementException` exception is thrown if we call `remove()` when the invoking queue is empty. |
  | `poll` | Remove and returns the head element of the queue. Return `null` if the queue is empty. |

- **No `empty()` function for queue**
   - Use `peek()` to check the queue is empty or not.

### PriorityQueue
- Concept
   - Sorted on ascending order automatically.

     ![Untitled (6)](https://user-images.githubusercontent.com/8989447/115980643-88bab600-a54b-11eb-9a2e-b9ab805da8e2.png)

- **Initialize a priority queue**
  ```java
  Queue<Integer> queue = new PriorityQueue<>();
  ```
- **Initialize a max priority queue**
  ```java
  Queue<Integer> queue = new PriorityQueue<>(10, Collections.reverseOrder());
  ```
- **Initialize a priority queue with custom sorting**
  ```
  PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b)->(a.val - b.val));
  ```
- **Common functions for priority queue**
  | Functions | Description |
  | ---- | ---- |
  | `add` | Add one element. |
  | `contains` | Returns true if this queue contains the specified element. |
  | `isEmpty` | Check the queue is empty or not. |
  | `peek` | Return (but NOT remove) the head (smallest) element. Return `null` if the queue is empty. |
  | `poll` | Removes and returns the head (smallest) element. Return `null` if the queue is empty. |

- **No `empty()` function for priority queue**
   - Use `peek()` to check the priority queue is empty or not.

### Set
- **Initialize a set**
  ```java
  Set<String> set = new HashSet<>();
  ```
- **Common functions for priority queue**
  | Functions | Description |
  | ---- | ---- |
  | `add` | Add one element. |
  | `contains` | Check the element is existing in the set or not. |

### TreeMap
- Concept
   - Sorted by key in ascending order automatically.
- **Initialize a tree map**
  ```java
  TreeMap<Integer, Integer> sortedMap = new TreeMap<>();
  ```
- **Common functions for tree map**

  | Functions | Description |
  | ---- | ---- |
  | `put` | Add one key-value pair. |
  | `firstKey` | Get the key of the first entry (smallest in key) from tree map. |
  | `firstEntry` | Get the first entry (smallest in key) from tree map. |
  | `lastKey` | Get the key of the last entry (greatest in key) from tree map. |
  | `lastEntry` | Get the last entry (greatest in key) from tree map. |
