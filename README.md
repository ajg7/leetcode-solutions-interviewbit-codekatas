My solutions for leetcode & codekatas

# Leetcode

## Graphs

### 997. Find the Town Judge
```javascript
const findJudge = (N, trust) => {
    const map = {};
    
    for(const [person1, person2] of trust) {
        if (!map[person1]) map[person1] = { "in": 0, "out": 0 };
        if (!map[person2]) map[person2] = { "in": 0, "out": 0 };
        
        map[person1].out++;
        map[person2].in++;
    }
    
    for (const [person, degrees] of Object.entries(map)) {
        if (degrees.in === N - 1 && degrees.out === 0) {
            return +person;
        }
    }
    
    return -1;
};
```

## Trees

### 111. Minimum Depth of Binary Tree (BFS)
```javascript
const minDepth = root => { 
    if (!root) return 0;
    if (!root.left) return minDepth(root.right) + 1;
    if (!root.right) return minDepth(root.left) + 1;
    return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
};
```

### 104. Maximum Depth of Binary Tree (DFS)
```javascript
const maxDepth = root => {
    const checkDepth = (node, depth) => {
        if (node === null) return depth;
        depth++;
        const leftSide = checkDepth(node.left, depth);
        const rightSide = checkDepth(node.right, depth);
        return Math.max(leftSide, rightSide);
    }
    return checkDepth(root, 0)
};
```

### 700. Search in a Binary Search Tree
```javascript
const searchBST = (root, val) => {
    if (!root) return null;
    if (root.val === val) return root;
    if (root.val > val) return searchBST(root.left, val);
    if (root.val < val) return searchBST(root.right, val);
};
```

### 617. Merge Two Binary Trees
```javascript
const mergeTrees = (t1, t2) => {
    if (!t1 || !t2) return t1 || t2;
    t1.val += t2.val;
    t1.left = mergeTrees(t1.left, t2.left);
    t1.right = mergeTrees(t1.right, t2.right);
    return t1;
};
```

### 938. Range Sum of BST
```javascript
const rangeSumBST = (root, low, high) => {
    if (!root) return 0;
    if (root.val < low)  return rangeSumBST(root.right, low, high);
    if (root.val > high) return rangeSumBST(root.left, low, high);
    return root.val + rangeSumBST(root.left, low, high) + rangeSumBST(root.right, low, high);
};
```

## Linked Lists

### 141. Linked List Cycle
```javascript
const hasCycle = head => {
    let slow = head;
    let fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) return true;
    }
    return false;
    
};
```

### 21. Merge Two Sorted Lists
```javascript
const mergeTwoLists = (l1, l2) => {
    let dummyhead = new ListNode();
    let curr = dummyhead;
    
    while (l1 && l2) {
        if (l1.val < l2.val) {
            curr.next = l1;
            l1 = l1.next
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }
    while (l1) {
        curr.next = l1;
        l1 = l1.next;
        curr = curr.next;
    }
    while (l2) {
        curr.next = l2;
        l2 = l2.next;
        curr = curr.next
    }
    return dummyhead.next;
};
```

### 234. Palindrome Linked List
```javascript
// Stack Solution

const isPalindrome = head => {
    const stack = []
    
    let curr = head
    while (curr) {
        stack.push(curr.val)
        curr = curr.next
    }
    
    while (head) {
        const top = stack.pop()
        if (head.val !== top) 
            return false
        
        head = head.next
    }
    
    return true
};

// Reverse Linked List Solution

const isPalindrome = head => {
    if (!head) return true
    const copyHead = copy(head)
    const reversedHead = reverse(copyHead)
    return isEqual(head, reversedHead)
};

const isEqual = (l1, l2) => {
    while (l1 && l2) {
        if (l1.val !== l2.val) {
            return false
        }
        
        l1 = l1.next
        l2 = l2.next
    }
    
    return !l1 && !l2
}

const reverse = head => {
    let prev = null
    let next = null
    while (head) {
        next = head.next
        head.next = prev
        prev = head
        head = next
    }
    return prev
}

const copy = head => {
    const copyHead = new ListNode(head.val)
    let copyCurr = copyHead
    head = head.next
    
    while (head) {
        copyCurr.next = new ListNode(head.val)
        copyCurr = copyCurr.next
        head = head.next
    }
    
    return copyHead
}
```

### 83. Remove Duplicates from Sorted List
```javascript
const deleteDuplicates = head => {
    if (!head) return null;
    let current = head;
    while (current && current.next) {
        if (current.val === current.next.val) {
            current.next = current.next.next;
        } else {
            current = current.next
        }
    }
    return head
};
```

### 203. Remove Linked List Elements
```javascript
 const removeElements = (head, val) => {
    const dummy = new ListNode(NaN)
    dummy.next = head
    
    let current = dummy
    while (current && current.next) {
        const next = current.next
        if (next.val === val) {
            current.next = current.next.next
        } else {
            current = current.next
        }
    }
    
    return dummy.next
};
```

### 206. Reverse a Linked List
```javascript
const reverseList = head => {
    let curr = head;
    let prev = null;
    let nextTemp = null;
    
    while (curr) {
        nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev
};
```

### 237. Delete Node in a Linked List
```javascript
const deleteNode = node => {
    node.val = node.next.val;
    node.next = node.next.next;
};
```

### 876. Middle of the Linked List
```javascript
const middleNode = head => {
    let fast = head;
    let slow = head;
    
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow
};
```

### Convert Binary Number in a Linked List to Integer
```javascript
const getDecimalValue = head => {
    const result = [];
    while (head) {
        result.push(String(head.val));
        head = head.next
    }
    return parseInt(result.join(""), 2)
};
```

## Binary Search

### 1351. Count Negative Numbers in a Sorted Matrix
```javascript
const countNegatives = grid => {
    let count = 0;
    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
            if (Math.sign(grid[i][j]) === -1) count++;
        }
    } 
    return count
};
```

## Arrays

### Remove Element
```javascript
// Pointer Solution
const removeElement = (nums, val) => {
    let pointer = 0;
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== val) {
            nums[pointer] = nums[i]
            pointer++
        }
    }
    return pointer
};

// Splice Solution
const removeElement = (nums, val) => {
    for (let i = nums.length - 1; i >= 0; i--) {
        if (nums[i] === val) nums.splice(i, 1);
    }
    return nums.length;
};
```

### Merge Sorted Array
```javascript
const merge = (nums1, m, nums2, n) => {
    let first = m - 1;
    let second = n  -1;
    
    for (let i = m + n - 1; i >= 0; i--) {
        if (second < 0) break;
        if (nums1[first] > nums2[second]) {
            nums1[i] = nums1[first];
            first--;
        } else {
            nums1[i] = nums2[second];
            second--;
        }
    }
};
```

### Duplicate Zeros
```javascript
const duplicateZeros = arr => {
    for (let i = 0; i < arr.length; i++) {
        if (!arr[i]) {
            arr.splice(i, 0, 0);
            i++
            arr.pop();
        }  
    }
};
```


# Code Katas

## Unique In Order
```javascript
var uniqueInOrder=function(iterable){
  const result = []
  let prevChar = ''
  
  for (const char of iterable) {
    if (prevChar !== char) {
      prevChar = char
      result.push(char)
    }
  }
  
  return result
}
```

## A Square of Squares
```javascript
const isSquare = num => {
  const rootNum = Math.sqrt(num)
  if(rootNum % 2 === 0 || rootNum % 2 === 1 && num >= 0) {
    return true
  } else {
    return false
  }
}
```

## Training on Dubstep
```javascript
const songDecoder = dubStepSong => {
    const splitArray = dubStepSong.split("WUB");
    const filteredArray = splitArray.filter(character => {
      if(character !== "") {
        return true;
      } else {
        return false;
      }
    })
    return filteredArray.join(" ")
}

console.log(songDecoder("WUBWEWUBAREWUBWUBTHEWUBCHAMPIONSWUBMYWUBFRIENDWUB"));
```

## Create Phone Number
```javascript
const createPhoneNumber = nums => {
  const phoneNumber = []
  const sectionOne = ["(", nums[0], nums[1], nums[2], ")"]
  const sectionTwo = [" ", nums[3], nums[4], nums[5], "-"]
  const sectionThree = [nums[6], nums[7], nums[8], nums[9]]
  return [...sectionOne, ...sectionTwo, ...sectionThree].join("")
}



console.log(createPhoneNumber([6,6,0,8,6,4,4,6,7,8]))
```
## Training on Roman Numerals
```javascript
const solution = (romanNum) => {
  const numerals = {
      M: 1000,
      D: 500,
      C: 100,
      L: 50,
      X: 10,
      V: 5,
      I: 1
    };
  const numbers = romanNum.split("");
  let solution = 0;
  let precedingNumeral;
  for (const ele of numbers) {
    if(precedingNumeral < numerals[ele]) {
        solution += numerals[ele] - 2
    } else {
      solution += numerals[ele]
    }
    precedingNumeral = numerals[ele]
  }
  return solution
}

solution("IX")
```

## Convert String to Camel Case
```javascript
const toCamelCase = str => {
  const splitStr = str.split("");
  const result = [];
  let underscorePresent = false;
  	for(const ele of splitStr) {
	    if(ele === "_" || ele === "-") {
		result.unshift()
		underscorePresent = true;
	    } else {
		if(underscorePresent) {
		    result.push(ele.toUpperCase())
		    underscorePresent = false;
		} else {
		    result.push(ele)
		  }
	      }
	    }
   return result.join("")
}

console.log(toCamelCase("the_camel_and_the_snake"))
```
### Convert Camel Case to Snake Case
- This is my own original challenge
```javascript
const toSnakeCase = str => {
  const splitStr = str.split("");
  const result = [];
  for(const ele of splitStr) {
      if(ele === ele.toUpperCase()) {
         result.push("_", ele.toLowerCase())
      } else {
	result.push(ele)
        }
      }
   return result.join("")
}

console.log(toSnakeCase("theCamelAndTheSnake"))
```

