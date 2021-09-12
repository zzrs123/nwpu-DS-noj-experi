#          noj实验2报告

## 0.0 实验题目//需求分析

![image-20210404030930478](C:\Users\shandaiwang\AppData\Roaming\Typora\typora-user-images\image-20210404030930478.png)

需求分析：

用三角函数的展开式计算PI的高精度值。

问题在于存储展开式数据。采用类似于多项式，但加上双向链表特点即可解决。

## 1.0 概要设计

### 1.1  建立链表

```
typedef struct node {

	int data;
	struct node*next;
	struct node*pre;
}node,*list;
```

### 1.2  设计函数

#### 1.2.1  申请空间函数

```
void init(list &l)
```

主要实现malloc操作。

#### 1.2.2  释放空间

```
void destorylist(list &l)
```

#### 1.2.3  插入元素操作

```
void insert(list &l, int data)
```

#### 1.2.4  主操作函数

```
main
```

## 2.0  代码//详细设计

```
#include<stdio.h>
#include<stdlib.h>

typedef struct node {
	int data;
	struct node*next;
	struct node*pre;
}node,*list;

int n;

void init(list &l)//初始化链表
{
    list tail = l, p;
	int i = 0;
	p=(list)malloc(sizeof(node));
	tail->next = p;
	p->pre = tail;
	tail = p;
	p->data = 2;
	tail->next = NULL;
}

void destorylist(list &l)//释放节点
{
	list p=l->next;
	list q;
	while (p->next)
	{
		q = p->next;
		free(p);
		p = q;
	}
	free(l);
	l = NULL;
}
void insert(list &l, int data)//插入函数
{
	list p = l;
	if(p == NULL)
	{
		return;
	}
	else
	{
		while (p->next)  
			p = p->next;
		list temp = (list)malloc(sizeof(node));
		p->next = temp; 
		temp->data = data;
		temp->pre = p;
		temp->next = NULL;		
	}
}

void tran(list l)//空时
{
	printf("%d.",l->next->data);
	l=l->next;
	list p;
	p=l->next;
	while(p!=NULL&&n)
	{
		printf("%d",p->data);
		p=p->next;
		n--;
	}
	printf("\n");
}

int main()//主要操作函数
{	
	list p,sum1;
	int i;
```


	scanf("%d",&n);
	p=(list)malloc(sizeof(node));
	sum1=(list)malloc(sizeof(node));
	p->next = NULL;
	sum1->next=NULL;
	p->pre = NULL;
	sum1->pre=NULL;
	
	init(p);
	init(sum1);
	
	for(i=1;i<=1000;i++)
	{
		insert(p,0);
		insert(sum1,0);
	}
	list q,sum;
	int ret,tmp,cnt=3,num=1;
	int flag=1;
	while(flag)
	{			
		q=p->next;
		sum=sum1->next;
		ret=0;
		while(q->next)
		{
			q=q->next;			
		}
		while (q)
		{
			tmp=q->data*num+ret;
			q->data=tmp%10;
			ret=tmp/10;
			if(q->pre==NULL)break;
			else
			{
				q=q->pre;
			}
		}
		
		ret=0;
		q=p->next;
		while (q)
		{
			tmp=q->data+ret*10;
			q->data=tmp/cnt;
			ret=tmp%cnt;
			if(q->next==NULL)break;
			else
			{
				q=q->next;
			}
		}
	 
		q=p->next;
		sum=sum1->next;
		while(q->next&&sum->next)
		{
			q=q->next;
			sum=sum->next;
		}
		flag=0;
		while (q&&sum)
		{
			
			tmp=sum->data+q->data ;
			sum->data=tmp%10;
			if(sum->pre==NULL||q->pre==NULL)
			{
				break;
			}
			else
			{
				sum->pre->data+=tmp/10;
				flag |= q->data;
				sum=sum->pre;
				q=q->pre;
			}
			
		}
		num++;
		cnt+=2;
	}
	tran(sum1);
	destorylist(p);
	destorylist(sum1);
	return 0;

```
}
```

## 3.0测试结果

![image-20210404032606969](C:\Users\shandaiwang\AppData\Roaming\Typora\typora-user-images\image-20210404032606969.png)

![image-20210404032728915](C:\Users\shandaiwang\AppData\Roaming\Typora\typora-user-images\image-20210404032728915.png)

## 4.0  实验心得

实现链表真的是麻烦而不太难的事情，反复在调指针的bug。

在纸上的分析确实很重要，刚开始毫无思路，到后来纸上分析后，就简化为双向链表的存取及运算。

注：参考了网上资料思路。



