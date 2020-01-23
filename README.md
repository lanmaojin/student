#include<stdio.h>

#define LEN sizeof(struct score)//赋值LEN的值是score 结构体的字节长度


struct mes {
	int stu;
	char name[20];
	char sourse[10][20];
	float score[10];
	float avescore;
	float groscore;
};
//使用结构体嵌套结构体的方法，可以相对简化对后期整个链表的排序
struct score {		//定义储存学生信息的结构体；
	struct mes mes1;
	struct score * next;
}typedef score ;		//声明结构体变量名

int n;
int x;
score *p3;

void prin(score * head);		//输出函数
score * creat();//链表创建信息输入函数
void sert(score * head);//插入函数
void sort(score* head);//排序函数  此函数已包含学号和姓名排序
void stusort(score * head);//学号排序
void namesort(score * head);//姓名排序
void query(score *head);//查询函数
void mesalt(score * head);//修改函数
void main() {
	p3 = (score *)malloc(LEN);	//给中转结构体指针分配内存空间
	//结构体赋值初始值
	strcpy(p3->mes1.sourse, "1");
	strcpy(p3->mes1.name, "1");
	p3->mes1.stu = 1;
	for (int i = 0; i < 10; i++) {
		p3->mes1.score[i] = 1;
	}
	int c = 0;
	score * creat();		//声明创建链表函数
	score *	head;		//定义链表头

	head = creat();
	
	for (;;) {
		printf("1、插入信息\t2、查询信息\t3、修改信息\t4、信息排序\t5、输出信息\t0、程序结束\n");
		scanf("%d", &c);
		if (c == 1) 
			sert(head);
		else if (c == 2) 
			query(head);
		else if(c == 3)
			mesalt(head);
		else if (c == 4) 
			sort(head);
		else if (c == 5) 
			prin(head);
		else if (c == 0) {
			printf("谢谢使用！\n");
			break;
		}
		else 
			printf("输入错误请重新输入\n");
	}
}

//链表创建信息输入函数
score * creat() {		
	score *head;		//定义链表头
	score *p1, *p2;

	head = NULL;		//初始化头指针

	printf("请先输入录入科目数并录入学生成绩信息\n");
	scanf("%d", &x);		//获取将要录入科目的数量
	n = 0;

	p1 = p2 = (score *)malloc(LEN);		//结构体指针分配内存

	printf("输入学号和姓名以空格隔开\n");
	scanf("%d %s", &p1->mes1.stu, &p1->mes1.name);		//第一次录入信息
	for (int i = 0; i < x; i++) {
		printf("输入第%d科目的名称和成绩\n", i + 1);
		scanf("%s %f", p1->mes1.sourse[i], &p1->mes1.score[i]);
	}
	p1->mes1.groscore = 0;
	for (int i = 0; i < x; i++) {
		p1->mes1.groscore += p1->mes1.score[i];
	}
	p1->mes1.avescore = p1->mes1.groscore / (x * 1.0);

	while (p1->mes1.stu != 0) {
		n++;		//累计链表节点数量
		if (n == 1)		//判断是否为头指针
			head = p1;
		else p2->next = p1;	//非头指针进行连接
		p2 = p1;	//
		p1 = (score *)malloc(LEN);		//给下一节点分配内存

		p1->mes1 = p2->mes1;		//将科目信息与下一节点同步，其他非科目名称信息会在后续录入时覆盖
		printf("输入学号和姓名以空格隔开\n");	//录入学号和姓名
		scanf("%d %s", &p1->mes1.stu, &p1->mes1.name);
		if (p1->mes1.stu == 0)		//判断是否结束录入
			break;
		for (int i = 0; i < x; i++) {		//录入每一科目的成绩
			printf("输入%s的成绩\n", p1->mes1.sourse[i]);
			scanf("%f", &p1->mes1.score[i]);
		}
		p1->mes1.groscore = 0;		//总分初始化
		for (int i = 0; i < x; i++) {
			p1->mes1.groscore += p1->mes1.score[i];
		}
		p1->mes1.avescore = p1->mes1.groscore / (x * 1.0);		//平均分计算
		
	}

	p2->next = NULL;		//尾指针指向赋空值		
	return head;		//返回头指针

}

//插入函数
void sert(score * head) {		//插入函数
	int i, y;		//选项 学号的变量
	int c = 0;		//是否插入成功的变量
	score *head1, *p1;		//声明结构体指针
			//结构体指针赋值

	for (;;) {
		head1 = head;
		printf("输入插入方式\n1、学号后插入2、尾端插入0、返回上一层\n");
		scanf("%d", &i);		//录入选项
		//判断选项
		if (i == 1) {
			printf("输入学号\n");
			scanf("%d", &y);
			while (head1 != NULL) {
				if (head1->mes1.stu == y) {
					p1 = (score *)malloc(LEN);		//给插入的链表分配内存空间
					p1->next = head1->next;		//插入节点连向链表的后节点
					head1->next = p1;		//将插入的节点接入到链表中
					p1->mes1 = head1->mes1;		//基本科目内容赋值，需要同步的相同信息
					printf("输入插入学号和姓名以空格隔开\n");
					scanf("%d %s", &p1->mes1.stu, p1->mes1.name);
					for (int j = 0; j < x; j++) {
						printf("输入%s的成绩:", p1->mes1.sourse[j]);
						scanf("%f", &p1->mes1.score[j]);
					}
					c = 1;		//如果录入成功赋值为1
				}
				head1 = head1->next;		//遍历向下一节点
			}
			//判断是否插入成功
			if (c == 1) {
				printf("插入成功\n");
			}
			else
				printf("查无此人！！！！（请确认信息）\n");
		}
		else if (i == 2) {
			head1 = head;
			for (;;) {		//遍历链表使指针指向尾节点
				if (head1->next == NULL)		//判断是否为尾节点
					break;
				head1 = head1->next;
			}
			p1 = (score *)malloc(LEN);		//给插入的节点分配内存
			p1->mes1 = head1->mes1;		//赋值科目信息，需要同步的信息
			p1->next = head1->next;		//插入的节点连向链表尾的空指针
			head1->next = p1;		//将插入的节点接入到链表中
			printf("输入插入的学号和姓名以空格隔开\n");
			scanf("%d %s", &p1->mes1.stu, p1->mes1.name);
			for (int j = 0; j < x; j++) {
				printf("输入%s的成绩:\n", p1->mes1.sourse[j]);
				scanf("%f", &p1->mes1.score[j]);
			}
			printf("插入成功\n");
		}
		else if (i == 0)		//选项为0时跳出循环回到主函数
			break;
		else
			printf("输入选项错误请重新输入");
	}
}

//输出函数
void prin(score * head) {//信息输出
	score *head1;
	head1 = head;
	printf("__________成绩信息____________\n");

	while (head1 != NULL) {
		printf("\t学号\t\t姓名\n");
		printf("\t%d\t\t%s\n", head1->mes1.stu, head1->mes1.name);
		for (int i = 0; i < x; i++) {
			printf("%s:%5.2f\t\t", head1->mes1.sourse[i], head1->mes1.score[i]);
			if (i % 2 == 0 && i != 0) {
				printf("\n");
			}
		}
		printf("\n平均分:%5.2f\t总分：%5.2f", head1->mes1.avescore, head1->mes1.groscore);
		printf("\n");
		head1 = head1->next;
	}
	printf("__________输出完成____________\n");
}

//此处一下是排序
void sort(score * head) {
	void stusort(score * head);//学号排序
	void namesort(score * head);//姓名排序
	
	int i = 100;
	for(;;) {
		printf("1、学号排序\t2、姓名排序\t0、返回上一层\n");
		scanf("%d", &i);
		if (i == 1)
			stusort(head);
		else if (i == 2)
			namesort(head);
		else if (i == 0) 
			break; 
		else 
			printf("输入错误请重新输入\n");
		
	}

}

//学号排序
void  stusort(score * head) {
	score *p1, *p2;		//

	p1 = head;

	while (p1 != NULL) {
		p2 = p1->next;
		while (p2 != NULL) {
			//判断学号的小前大后
			//排序的方式是交换学生信息而不改变链表的指针顺序
			if (p1->mes1.stu > p2->mes1.stu) {
				p3->mes1 = p1->mes1;
				p1->mes1 = p2->mes1;
				p2->mes1 = p3->mes1;
			}
			p2 = p2->next;
		}
		p1 = p1->next;
	}
	//所有排序的方式都相同

	return head;
}

//姓名排序
void namesort(score * head) {
	score *p1, *p2;

	p1 = head;

	while (p1 != NULL) {
		p2 = p1->next;
		while (p2 != NULL) {
			if (strcmp(p1->mes1.name, p2->mes1.name) > 0) {
				p3->mes1 = p1->mes1;
				p1->mes1 = p2->mes1;
				p2->mes1 = p3->mes1;
			}
			p2 = p2->next;
		}
		p1 = p1->next;
	}
}

//查询函数
void query(score *head) {    //查询函数总
	score * head1, *p1;
	head1 = head;
	int i = 3, n;   //i在此时的赋值是为了初始化
	char n1[20];
	while (i != 0) {
		printf("1、学号查询\t2、姓名查询\t0、返回上一层\n");
		scanf("%d", &i);
		if (i == 1) {			//判断输入的选项
			printf("输入需要查询的学号:\n");
			scanf("%d", &n);
			
			while (head1 != NULL) {
				if (head1->mes1.stu == n) {		//遍历学号寻找需要查询的学号
					printf("\t学号\t\t姓名\n");
					printf("\t%d\t\t%s\n", head1->mes1.stu, head1->mes1.name);
					for (int i = 0; i < x; i++) {
						printf("%s:%5.2f\t\t", head1->mes1.sourse[i], head1->mes1.score[i]);
						if (i % 2 == 0 && i != 0) {
							printf("\n");
						}
					}
					printf("\n平均分:%5.2f\t总分：%5.2f", head1->mes1.avescore, head1->mes1.groscore);
					printf("\n");
				}
				head1 = head1->next;
			}
			head1 = head;
		}
		else if (i == 2) {
			printf("输入需要查询的姓名：\n");
			scanf("%s", n1);
			while (head1 != NULL) {
				if (strcmp(head1->mes1.name,n1) == 0) {		//遍历查询项目寻找
					printf("\t学号\t\t姓名\n");
					printf("\t%d\t\t%s\n", head1->mes1.stu, head1->mes1.name);
					for (int i = 0; i < x; i++) {
						printf("%s:%5.2f\t\t", head1->mes1.sourse[i], head1->mes1.score[i]);
						if (i % 2 == 0 && i != 0) {
							printf("\n");
						}
					}
					printf("\n平均分:%5.2f\t总分：%5.2f", head1->mes1.avescore, head1->mes1.groscore);
					printf("\n");
				}
				head1 = head1->next;
			}
			head1 = head;
		}
		else if (i == 0)
			break;
		else {
			printf("输入选项错误请重新输入\n");
		}
	}
}

//修改函数
void mesalt(score * head) {		//修改函数
	score * head1;
	head1 = head;

	int i, s;
	char n[20], n1[20];
	for (;;) {
		printf("1、输入学号\t2、返回上一层\n");
		scanf("%d", &i);
		if (i == 1) {			//判断选项
			printf("输入需要修改的学号\n");
			scanf("%d", &s);
			while (head1 != NULL) {

				if (head1->mes1.stu == s) {			//遍历学号
					printf("输入需要的项目名称（例：姓名 XXX）\n");
					scanf("%s %s", n, n1);
					if (n1[0] <= 'z'&& n1[0] >= 'A') {			//判断输入的信息是姓名或成绩
						strcpy(head1->mes1.name, n1);
						printf("修改完成\n");
					}
					else if (n1[0] <= '9' && n1[0] >= '0') {
						int s1 = 0, n2 = 0;
						while (n1[s1] != '\0') {		//当判断为成绩时，将字符类型的数字转换为整形类型
							n2 = n2 + (n1[s1] - 48);
							n2 = n2 * 10;
							s1++;
						}
						n2 = n2 / 10;
						for (int y = 0; y < x; y++) {
							if (strcmp(head1->mes1.sourse[y], n) == 0) {		//遍历科目名称并将成绩正确修改
								head1->mes1.score[y] = n2;
							}
						}
						printf("修改成功\n");
					}
				}
				head1 = head1->next;
			}
		}
		else if (i == 2)
			break;
		else {
			printf("输入选项错误请重新输入\n");
			head1 = head;
		}
	}
}
