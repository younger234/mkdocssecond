# 21.合并两个有序链表

```cpp
lass Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* head=new ListNode();
        ListNode* tempt=head;
        
        while(list1&&list2){
            if(list1->val<list2->val){
                ListNode *p=new ListNode();
                p->val=list1->val;
                tempt->next=p;
                tempt=tempt->next;
                list1=list1->next;
            }else{
                ListNode *p=new ListNode();
                p->val=list2->val;
                tempt->next=p;
                tempt=tempt->next;
                list2=list2->next;
            }
        }
        if(list1){
            tempt->next=list1;

        }
        if(list2){
            tempt->next=list2;
        }
        return head->next;
    }
};
```
方法一就是最常规的迭代法。新建一个tempt节点每次右移。然后每次比较后新建一个p节点接在tempt后面。然后tempt右移即可，最后把没比较完的剩余序列直接接上

方法二是简便的递归法，很妙。
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == nullptr) {
            return l2;
        } else if (l2 == nullptr) {
            return l1;
        } else if (l1->val < l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```