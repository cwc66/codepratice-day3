# codepratice-day3
代码随想录第3天。链表、删除链表元素、链表操作、反转链表

今天的内容没有看具体文章，只是将题目视频和题目代码打了一遍，期间询问GPT的一些内容也没细看。
第二天看回来。

这部分内容我需要背下来，之后尽量背。
这部分内容和acwing上面有些不同，和leetcode的需求基本一致，ACWING的内容是使用数组模拟的链表。

这两天的内容都没有敲多几遍，之后需要重复敲。

链表基础的内容中，代码随想录里面关于数组和链表的异同点需要记一下。我觉得能有用。

[移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/description/)
解法1.使用原链表删除元素
```CPP
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while(head!=nullptr&&head->val==val)
        {
            ListNode* tmp=head;//记录链表节点，用于删除
            head=head->next;
            delete tmp;
        }

        ListNode* cur=head;
        while(cur!=nullptr&&cur->next!=nullptr)
        {
            if(cur->next->val==val)
            {
                ListNode* tmp=cur->next;
                cur->next=cur->next->next;
                delete tmp;
            }
            else
            {
                cur=cur->next;
            }
        }
        return head;
    }
};
```

这里链表的定义一样很重要很重要！！！！！一定要背下来，不然acm模式答不出来。

解法2.使用哑节点删除元素
```CPP
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyhead=new ListNode(0,head);
        //dummyhead->next=head;
        ListNode* cur=dummyhead;
        while(cur!=nullptr&&cur->next!=nullptr)
        {
            if(cur->next->val==val)
            {
                ListNode* tmp=cur->next;
                cur->next=cur->next->next;
                delete tmp;
            }
            else
            {
                cur=cur->next;
            }
        }
        return dummyhead->next;
    }
};
```

解法3.递归，如果是要删的节点，答案为头结点的后续节点递归的结果，如果不是要删的节点，则将头结点的递归结果连接到头结点上，为答案。
```CPP
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        // 基础情况：空链表
        if (head == nullptr) {
            return nullptr;
        }

        // 递归处理
        if (head->val == val) {
            ListNode* newHead = removeElements(head->next, val);
            delete head;
            return newHead;
        } else {
            head->next = removeElements(head->next, val);
            return head;
        }
    }
};
```

[设计链表](https://leetcode.cn/problems/design-linked-list/description/)
这道题我需要打5遍以上，而且代码随想录文章没看，待完成。（2025/1/10）
我感觉难点在于n、n之前、以及节点从0开始。需要注意。
```CPP
class MyLinkedList {
public:
    struct ListNode{
        int val;
        ListNode* next;
        ListNode(int val):val(val),next(nullptr){}
    };
    //初始化链表
    MyLinkedList() {
        dummyhead=new ListNode(0);//虚节点
        _size=0;
    };
    
    int get(int index) {
        if(index>(_size-1)||index<0)
        {
            return -1;
        }
        ListNode* cur=dummyhead->next;
        while(index--)
        {
            cur=cur->next;
        }
        return cur->val;
    };
    
    void addAtHead(int val) {
        ListNode* newnode=new ListNode(val);
        newnode->next=dummyhead->next;
        dummyhead->next=newnode;
        _size++;
    };
    
    void addAtTail(int val) {
        ListNode* newnode=new ListNode(val);
        ListNode* cur=dummyhead;
        while(cur->next!=nullptr)
        {
            cur=cur->next;
        }
        cur->next=newnode;
        _size++;
    };
    
    void addAtIndex(int index, int val) {
        if(index>_size) return;

        ListNode* newnode=new ListNode(val);
        ListNode* cur=dummyhead;
        while(index--)
        {
            cur=cur->next;
        }
        newnode->next=cur->next;
        cur->next=newnode;
        _size++;
    };
    
    void deleteAtIndex(int index) {
        if(index>=_size)
            return ;
        
        ListNode* cur=dummyhead;
        while(index--)
        {
            cur=cur->next;
        }//从index到1能进入循环，但是一共有index+1个节点，因此cur到的是第index个节点的前一个，而cur->next才是index个节点
        ListNode* tmp=cur->next;
        cur->next=cur->next->next;
        delete tmp;
        tmp=nullptr;
        _size--;
    };

private:
    int _size;
    ListNode* dummyhead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

[反转链表](https://leetcode.cn/problems/reverse-linked-list/)
这道题我跪过一次，需要多熟悉，背下来。
因为我都做过3次了，但是这次还是没啥印象。
有双指针和递归两种写法，先背下双指针吧，别贪多嚼不烂。
```CPP
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev=nullptr;
        ListNode* cur=head;
        while(cur)
        {
            ListNode* tmp=cur->next;
            cur->next=prev;
            prev=cur;
            cur=tmp;
        }
        return prev;
    }
};
```
```CPP
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==nullptr||head->next==nullptr)
        {
            return head;
        }
        ListNode* newhead=reverseList(head->next);
        head->next->next=head;
        head->next=nullptr;
        return newhead;
    }
};
```