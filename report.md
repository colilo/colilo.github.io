# LeedCode functions

2. [Add two Numbers](https://leetcode.com/problems/add-two-numbers/)

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

>Example:
>
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)  
>Output: 7 -> 0 -> 8  
>Explanation: 342 + 465 = 807.  
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

int adder(int a, int b, int *o)
{
    int res = a + b + *o;
    *o = res / 10;
    return (res % 10);
}

struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    int o = 0;
    int a = 0;
    int b = 0;
    struct ListNode *p = NULL;
    struct ListNode **q = &p;
    struct ListNode *p1 = l1;
    struct ListNode *p2 = l2;
    while (p1 != NULL || p2 != NULL) {
        a = 0;
        b = 0;

        if (p1 != NULL) {
            a = p1->val;
            p1 = p1->next;
        }
        
        if (p2 != NULL) {
            b = p2->val;
            p2 = p2->next;
        }
        *q = malloc(sizeof(struct ListNode));
        (*q)->next = NULL;
        (*q)->val = adder(a, b, &o);
        q = &((*q)->next);
    }
    if (o > 0) {
        *q = malloc(sizeof(struct ListNode));
        (*q)->val = o;
        (*q)->next = NULL;
        
    }
    return p;
}
```

3. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```c
int lengthOfLongestSubstring(char* s) {
    int flag[256];
    int count = 0;
    int max = 0;
    int lidx = 0;
    int i = 0;
    int len = strlen(s);
    memset(flag, -1, 256 * 4);
    for (i = 0; i < len; i++) {
        if(flag[s[i]] == -1) {
            flag[s[i]] = i;
            count++;
        } else {
            max = max > count ? max : count;
            count = 0;
            i = flag[s[i]];
            memset(flag, -1, 256 * 4);
        }
    }
    
    max = max > count ? max : count;
    
    return max;
}
```

4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

**Example 1:**

>nums1 = [1, 3]  
>nums2 = [2]  
>  
>The median is 2.0  

**Example 2:**

>nums1 = [1, 2]  
>nums2 = [3, 4]  
>  
>The median is (2 + 3)/2 = 2.5

```c
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) {
    int mid_idx = (nums1Size + nums2Size) / 2;
    int i = 0, j = 0;
    int min_len = nums1Size < nums2Size ? nums1Size : nums2Size;
    int index = 0;
    double mid_val1 = 0;
    double mid_val2 = 0;
    int odd = ((nums2Size - mid_idx) + (nums1Size - mid_idx) != 0);
    while((odd && (index <= mid_idx)) || ((!odd) && (index < mid_idx))) {
        if (i < nums1Size && j < nums2Size) {
            if (nums1[i] <= nums2[j]) {
                mid_val1 = nums1[i];
                i++;
            } else {
                mid_val1 = nums2[j];
                j++;
            }

        } else {
            if (i < nums1Size) {
                mid_val1 = nums1[i];
                i++;
            } 
            if( j < nums2Size) {
                mid_val1 = nums2[j];
                j++;
            }
        }
        
        index++;
    }
    
    if(odd) {
        return mid_val1;
    } else {
        if (i < nums1Size && j < nums2Size) {
            mid_val2 = nums1[i] <= nums2[j] ? nums1[i] : nums2[j];
        } else {
            if (i < nums1Size) {
                mid_val2 = nums1[i];
                i++;
            } 
            if( j < nums2Size) {
                mid_val2 = nums2[j];
                j++;
            }
        }
        
        return (mid_val1 + mid_val2) / 2;
    }
    
}
```

6. [ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N  
A P L S I I G  
Y   I   R  
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

```c string convert(string s, int numRows);```

Example 1:

>Input: s = "PAYPALISHIRING", numRows = 3  
>Output: "PAHNAPLSIIGYIR"

Example 2:

>Input: s = "PAYPALISHIRING", numRows = 4
>Output: "PINALSIGYAHRPI"
>**Explanation:**

>P     I    N  
>A   L S  I G  
>Y A   H R  
>P     I  
```c
char* convert(char* s, int numRows) {
    int i;
    int j = 0;
    int k = 0;
    int step1;
    int step2;
    int s_len = strlen(s);
    char *res = malloc(sizeof(char) * (s_len + 1));
    for(i = 1; i <= numRows; i++) {
        k = i - 1;
        res[j] = s[k];
        j++;
        while(1) {
            step1 = 2 * numRows -2 * i;
            if (step1 > 0) {
                k = k + step1;
                if (k < s_len) {
                    res[j] = s[k];
                    j++;
                } else {
                    break;
                }
            }
            
            step2 = 2 * numRows - 2 * (numRows - i + 1);
            if (step2 > 0) {
                k = k + step2;
                if (k < s_len) {
                    res[j] = s[k];
                    j++;
                } else {
                    break;
                }
            }
            if (step1 == 0 && step2 == 0) {
                k++;
                if (k < s_len) {
                    res[j] = s[k];
                    j++;
                } else {
                    break;
                }
            }
        }
    }
    res[j] = '\0';
    return res;
}
```

11. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and n is at least 2.

 ```c
 int maxArea(int* height, int heightSize) {
    int left = 0;
    int right = heightSize - 1;
    int area = 0;
    
    while(left < right) {
        int a = (right - left) * ((height[left] > height[right])? height[right] : height[left]);
        if(a > area)
            area = a;
        if(height[left] > height[right])
            right--;
        else
            left++;
    }
    
    return area;
}
 ```

21. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2) {
    struct ListNode *head = NULL;
    struct ListNode **iter = &head;
    for (;l1 && l2;) {
        if (l1->val < l2->val) {
            *iter = l1;
            l1 = l1->next;
        } else {
            *iter = l2;
            l2 = l2->next;
        }
        iter = &((*iter)->next);
    }
    
    if (l1) {
        *iter = l1;
    } else if (l2) {
        *iter = l2;
    }
    
    return head;
}
```

 24. [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

**Example:**

>Given 1->2->3->4, you should return the list as 2->1->4->3.

 ```c
 /**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* swapPairs(struct ListNode* head) {
    struct ListNode *iter = NULL;
    struct ListNode *newHead = NULL;
    struct ListNode **pre = &newHead;
    int i = 0;
    for (iter = head; iter; ) {
        i++;
        
        if (i & 1) {
            *pre = iter;
            iter = iter->next;
        } else {
            (*pre)->next = iter->next;
            iter->next = *pre;
            *pre = iter;
            pre = &((*pre)->next->next);
            iter = *pre;
        }
    }

    return newHead;
}
 ```

26. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
```c
int removeDuplicates(int* nums, int numsSize) {
    int i;
    int *iter = nums;
    int count = 0;
    
    if (numsSize <= 0) {
        return numsSize;
    }
    
    for (i = 1; i < numsSize; i++) {
        if (*iter != nums[i]) {
            iter++;
            *iter = nums[i];
        } 
    }
    
    return iter - nums + 1;
}
```

27. [Remove Element](https://leetcode.com/problems/remove-element/)

```c
int removeElement(int* nums, int numsSize, int val) {

    int left = 0;
    int right = numsSize -1;
    int flag = 0;
    while (left <= right) {
        while (nums[left] != val && left <= right) {
            left++;
        }
        
        while(nums[right] == val && left <= right) {
            right--;
        }
        
        if (left <= right) {
            nums[left] = nums[left] ^ nums[right];
            nums[right] = nums[left] ^ nums[right];
            nums[left] = nums[left] ^ nums[right];
            left++;
            right--;
            flag = 1;
        } else {
            break;
        }

    }
    
    return left;
}
```

101. [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
 
int compareNode(struct TreeNode* l, struct TreeNode* r)
{
    int flg = 1;
    if (l == NULL && r == NULL) {
        return 1;
    } else if (l != NULL && r != NULL){
        if (l->val == r->val) {
            flg &= compareNode(l->left, r->right);
            flg &= compareNode(l->right, r->left);
            return flg;
        }
    }
    return 0;
}

bool isSymmetric(struct TreeNode* root) {
    if (root == NULL) {
        return true;
    } else {
        return compareNode(root->left, root->right);
    }
}
```
111. [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int minDepth(struct TreeNode* root) {
    int depth = 0;
    int ldepth = 0;
    int rdepth = 0;
    if (root == NULL) {
        depth = 0;
    } else if (root->left == NULL && root->right == NULL) {
        depth = 1;
    } else {
        ldepth = minDepth(root->left); 
        rdepth = minDepth(root->right);
        if (ldepth > 0 && rdepth > 0){
            depth = ldepth < rdepth ? ldepth : rdepth;
        } else {
            depth = ldepth + rdepth;
        }
        depth += 1;
    }
    return depth;
}
```

112. [Path Sum](https://leetcode.com/problems/path-sum/)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
bool hasPathSum(struct TreeNode* root, int sum) {
    bool ret = false;
    if (root == NULL) {
        return ret;
    }
    
    if (root->left == NULL && root->right == NULL) {
        if (sum - root->val == 0) {
            ret = true;
        }
        return ret;
    }

    ret = hasPathSum(root->left, sum - root->val);
    ret |= hasPathSum(root->right, sum - root->val);
    
    return ret;
}
```

118. [Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)

```c
/**
 * Return an array of arrays.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** generate(int numRows, int** columnSizes) {
    int i;
    int l, r;
    int **ret = malloc(numRows * sizeof(int *));
    *columnSizes = malloc(numRows * sizeof(int));
    for (i = 0; i < numRows; i++) {
        (*columnSizes)[i] = i + 1;
        ret[i] = malloc((*columnSizes)[i] * sizeof(int));
        l = 0;
        r = (*columnSizes)[i] - 1;
        ret[i][l++] = ret[i][r--] = 1;
        for (; l <= r; l++, r--) {
            ret[i][l] = ret[i][r] = ret[i - 1][l - 1] + ret[i - 1][l];
        }
    }
    return ret;
}

```

162. [Find Peak Element](https://leetcode.com/problems/find-peak-element/)

```c
int findPeakElement(int* nums, int numsSize) {
    int max = nums[0];
    int index = 0;
    for (int i = 1; i < numsSize; i++) {
        if (nums[i] > max) {
            max = nums[i];
            index = i;
        }
    }
    return index;

}
```

206. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
Reverse a singly linked list.
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode emptyHead;
    emptyHead.next = head;
    struct ListNode *tmp;
    if (head == NULL) {
        return NULL;
    }
    while(head->next != NULL) {
        tmp = emptyHead.next;
        emptyHead.next = head->next;
        head->next = emptyHead.next->next;
        emptyHead.next->next = tmp;
    }
    return emptyHead.next;
}
```

228. [Summary Ranges](https://leetcode.com/problems/summary-ranges/)

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
char** summaryRanges(int* nums, int numsSize, int* returnSize) {
    
    int i, j = 0;
    int begin, end;
    *returnSize = 0;
    if (numsSize == 0) {
        return NULL;
    }
    
    char **summary = malloc(sizeof(char*) * numsSize);
    if (summary == NULL) {
        printf("malloc error\n");
        return NULL;
    }
    for (i = 0; i < numsSize; i++) {
        *(summary + i) = malloc(sizeof(char) * 23);
        if (*(summary +i) == NULL) {
            printf("malloc error\n");
            return NULL;
        }
    }
    
    begin = end = nums[0];
    for (i = 1; i < numsSize; i++) {
        
        end = nums[i];
        
        if (end != nums[i - 1] + 1) {
            if (begin == nums[i - 1]) {
                sprintf(summary[(*returnSize)++], "%d", begin);
            } else {
                sprintf(summary[(*returnSize)++], "%d->%d", begin, nums[i - 1]);
            }
            begin = nums[i];
        }
            
    }
    if (begin == end) {
        sprintf(summary[(*returnSize)++], "%d", begin);
    } else {
        sprintf(summary[(*returnSize)++], "%d->%d", begin, end);
    }
    
    return summary;
}
```

230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note:
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.
```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

int inOrderTravelling(struct TreeNode* root, int *count)
{
    int val = 0;
    if (root == NULL) {
        return 0;
    }
    
    val |= inOrderTravelling(root->left, count);
    (*count)--;
    if (*count == 0) {
        val |= root->val;
    }
    val |= inOrderTravelling(root->right, count);

    return val;
}
 
int kthSmallest(struct TreeNode* root, int k) {
    return inOrderTravelling(root, &k);
}
``` 

328. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* oddEvenList(struct ListNode* head) {
    struct ListNode *iter;
    struct ListNode *tmp, *oddTail, *evenTail;
    int i = 1;
    oddTail = NULL;
    evenTail = NULL;
    tmp = NULL;
    for (iter = head; iter != NULL; iter = iter->next) {
        if (i & 0x1) {
            if (oddTail == NULL) {
                oddTail = iter;
            } else {
                oddTail->next = iter;
                oddTail = oddTail->next;
            }
        } else {
            if (evenTail == NULL) {
                tmp = iter;
                evenTail = iter;
            } else {
                evenTail->next = iter;
                evenTail = evenTail->next;
            }
        }
        i++;
    }

    if (oddTail != NULL) {
        oddTail->next = tmp;
        if (evenTail != NULL) {
            evenTail->next = NULL;
        }
    }
    return head;
}
```

345. [Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)

```c
char* reverseVowels(char* s) {
    char temp;
    char array[128] = {0};
    array['a'] = 1;
    array['e'] = 1;
    array['i'] = 1;
    array['o'] = 1;
    array['u'] = 1;
    array['A'] = 1;
    array['E'] = 1;
    array['I'] = 1;
    array['O'] = 1;
    array['U'] = 1;
    char *l = s;
    char *r = s + strlen(s) - 1;
    
    while (l < r) {
        if (array[*l] && array[*r]) {
            temp = *l;
            *l = *r;
            *r = temp;
            l++;
            r--;
        } 
        if (array[*l] == 0) {
            l++;
        } 
        if (array[*r] == 0) {
            r--;
        }
    }
    
    return s;
}
```

389. [Find the Difference](https://leetcode.com/problems/find-the-difference/)

```c
char findTheDifference(char* s, char* t) {
    char ascii[256] = {0};
    char *iter = NULL;
    int i;
    for (iter = s; *iter != '\0'; iter++) {
        ascii[*iter]++;
    }
    
    for (iter = t; *iter != '\0'; iter++) {
        ascii[*iter]--;
    }
    
    for (i = 0; i < 256; i++) {
        if (ascii[i] < 0) {
            break;
        }
    }
    
    return ((char)i);
}
```

451. [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

Given a string, sort it in decreasing order based on the frequency of characters.

```c
char* frequencySort(char* s) {
    struct node {
        int count;
        char character;
    } ascii[256] = {0};
    char *i;
    for (i = s; *i != '\0'; i++) {
        ascii[*i].count++;
        ascii[*i].character = *i;
    }
    int tmpcount;
    char tmpcharacter;
    
    char *ret = malloc(sizeof(char) * strlen(s) + 1);
    
    
    for(int j = 0; j < 256; j++) {
        int flag = 0;
        for (int k = 0; k < 256 - j - 1; k++) {
            if (ascii[k].count < ascii[k + 1].count) {
                tmpcount = ascii[k].count;
                tmpcharacter = ascii[k].character;
                ascii[k].count = ascii[k + 1].count;
                ascii[k].character = ascii[k + 1].character;
                ascii[k + 1].count = tmpcount;
                ascii[k + 1].character = tmpcharacter;
                flag = 1;
                
            }
        }
        if (flag == 0) {
            break;
        }
    }
    
    i = ret;
    
    for(int l = 0; (l < 256) && (ascii[l].count != 0); l++) {
        for (int m = 0; m < ascii[l].count; m++) {
            *i = ascii[l].character;
            i++;
        }
    }
    
    *i = '\0';
    
    return ret;
}
```

# An example for a whole application

**echo_server.c**

```c
#include <unistd.h> 
#include <stdio.h> 
#include <sys/socket.h> 
#include <stdlib.h> 
#include <netinet/in.h> 
#include <string.h> 
#define PORT 8080 
int main(int argc, char const *argv[]) 
{ 
    int server_fd, new_socket, valread; 
    struct sockaddr_in address; 
    int opt = 1; 
    int addrlen = sizeof(address); 
    char buffer[1024] = {0}; 
    char *hello = "Hello from server"; 
       
    // Creating socket file descriptor 
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) 
    { 
        perror("socket failed"); 
        exit(EXIT_FAILURE); 
    } 
       
    // Forcefully attaching socket to the port 8080 
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, 
                                                  &opt, sizeof(opt))) 
    { 
        perror("setsockopt"); 
        exit(EXIT_FAILURE); 
    } 
    address.sin_family = AF_INET; 
    address.sin_addr.s_addr = INADDR_ANY; 
    address.sin_port = htons( PORT ); 
       
    // Forcefully attaching socket to the port 8080 
    if (bind(server_fd, (struct sockaddr *)&address,  
                                 sizeof(address))<0) 
    { 
        perror("bind failed"); 
        exit(EXIT_FAILURE); 
    } 
    if (listen(server_fd, 3) < 0) 
    { 
        perror("listen"); 
        exit(EXIT_FAILURE); 
    } 
    if ((new_socket = accept(server_fd, (struct sockaddr *)&address,  
                       (socklen_t*)&addrlen))<0) 
    { 
        perror("accept"); 
        exit(EXIT_FAILURE); 
    } 
    valread = read( new_socket , buffer, 1024); 
    printf("%s\n",buffer ); 
    send(new_socket , hello , strlen(hello) , 0 ); 
    printf("Hello message sent\n"); 
    return 0; 
} 
```

```c
#include <stdio.h> 
#include <sys/socket.h> 
#include <arpa/inet.h> 
#include <unistd.h> 
#include <string.h> 
#define PORT 8080 

int main(int argc, char const *argv[]) 
{ 
	int sock = 0, valread; 
	struct sockaddr_in serv_addr; 
	char *hello = "Hello from client"; 
	char buffer[1024] = {0}; 
	if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) 
	{ 
		printf("\n Socket creation error \n"); 
		return -1; 
	} 

	serv_addr.sin_family = AF_INET; 
	serv_addr.sin_port = htons(PORT); 
	
	// Convert IPv4 and IPv6 addresses from text to binary form 
	if(inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr)<=0) 
	{ 
		printf("\nInvalid address/ Address not supported \n"); 
		return -1; 
	} 

	if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) 
	{ 
		printf("\nConnection Failed \n"); 
		return -1; 
	} 
	send(sock , hello , strlen(hello) , 0 ); 
	printf("Hello message sent\n"); 
	valread = read( sock , buffer, 1024); 
	printf("%s\n",buffer ); 
	return 0; 
} 

```
