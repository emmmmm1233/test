#include<stdio.h>            //用到输入输出函数
#include<string.h>           //用到字符串函数
#include <unistd.h>
#include <stdlib.h>
#include"windows.h"          //用到windows函数


#define MAX_TYPE 4    //房间类型的数目  3
#define MAX_NUM 4	 //每种类型房间的数目 3
#define MAX_GUEST 10	// 最大的客户数 9

//# define M 30                 // 用户数
//# define N 20                 //房间数

int z0=123456;                //管理员初始账号
int s0=123456;               //管理员初始密码


int mimaxiugai();            //密码修改函数
void guanliyuan();           //管理员登录函数
void jiemian();              //主菜单函数
void fangjianxinxi();        //房间信息函数
void yuding();               //预订函数
int read_printf_guest(void);                 //读取并显示客户函数
int save_guest(int count);           		//保存客户信息函数
int save_room(int type,int num);          //保存房间信息函数
void Outsystem();            //退出系统函数
void chaxun();               //资料查询函数
int read_guest();             //读取客户信息函数
void tuiding();              //退订函数
int read_room(void);               //读取房间信息函数
void dingdanchaxun();        //订单查询函数
void fangjian();

struct room       /*定义各房间信息的结构体*/
{
    int type;         //房间类型
    int num;          //房间号码
    int date;		  //入住日期
    int flag;         // 0-无人入住 1-入住一天 2-入住两天
};
struct room g_room[MAX_TYPE][MAX_NUM];    //定义一个结构体保存房间的信息

struct guest       /*定义各房间信息的结构体*/
{
    char name[30];      //客户名字
    char ID[30];        //客户身份证号
    char phone[15];     //客户电话
    int type;           //房间类型
    int number;         //房间号
    int date;           //入住日期
};
struct guest guest[MAX_GUEST];  //定义一个结构体保存客户的个人信息

struct room__message   //入住时填写的房间信息
{
    int type ;
    int num;
    int date;
};



void empty_file(char * fname)
{
    FILE * fp;
    fp = fopen(fname, "w");//以只写方式打开文件时 其实就是创建了一个新文件(空的)。 如果本来存在文件，也同样会被覆盖
    if(fp == NULL)
        printf("do empty file %s failed\n", fp);
    else fclose(fp);
}






void delete(char* FileName, int lineno)           //退订 
{
   int   Lid=0;
   int   MaxLine=0;
   FILE*   fp=NULL;
   char   Buf[256]="";
   char   tmp[20][256]={0};
   char   *p   =   Buf;

      if ((fp = fopen(FileName, "r+")) == NULL)
   {
    printf("Can't   open   file!/n");
    return;
   }

   while ((p = fgets(Buf, 256, fp)) != NULL)
   {
     Lid++;
     if (Lid == lineno)
     {
      if ((p = fgets(Buf, 256, fp)) != NULL)
      {
      strcpy(tmp[Lid], Buf);
     }
      }
    else
    {
      strcpy(tmp[Lid], Buf);
     }
   }

   MaxLine = Lid;
   rewind(fp);
   fclose(fp);
   remove(FileName);   //   删除原文件

   if((fp = fopen(FileName, "w")) == NULL)
   {
     printf("Can't   open   file!/n");
     return;
   }

    for(Lid = 1; Lid <= MaxLine; Lid++)
    {
    fputs(tmp[Lid], fp);
    }
    fclose(fp);
}


//*保存房间信息函数*/
int save_room(int type,int num)
{
	//int i;
	//char c;
	FILE *fp;
	if((fp=fopen("room_data.txt","a"))==NULL)
	{
            perror("in save_room open error:");
			exit(-1);
	}

    fprintf(fp,"%d %d %d %d\n",g_room[type][num].type,g_room[type][num].num,g_room[type][num].date,g_room[type][num].flag);
    fclose(fp);
    return 0;
}


/*保存客户信息函数*/
int save_guest(int count)
{
	//int i;
	//char c;
	FILE *fp;
	if((fp=fopen("guest_data.txt","a"))==NULL)
	{
            perror("in save_guest open error:");
			exit(-1);
	}
    fprintf(fp,"%s %s %s %d %d %d\n",guest[count].name,guest[count].ID,guest[count].phone,guest[count].type,guest[count].number,guest[count].date);
    fclose(fp);
    return 0;
}


int save_mima()
{
	//int i;
	//char c;
	FILE *fp;
	if((fp=fopen("mima_data.txt","a"))==NULL)
	{
            perror("save_mima open error:");
			exit(-1);
	}

    fprintf(fp,"%d \n",s0);
    fclose(fp);
    return 0;
}


int read_mima()
{

	FILE *fp;
	if((fp=fopen("mima_data.txt","r"))==NULL)
	{
			perror("in read_mima oepn error:");
			exit(-1);
	}



    fscanf(fp,"%d\n",s0);

	fclose(fp);
	return 0;
}

/*读取并且输出客户信息函数*/
int read_printf_guest(void)
{
	int i,j;
	FILE *fp;
	if((fp=fopen("guest_data.txt","r"))==NULL)

	{
			printf("cannot open file\n");
			exit(-1);
	}
	printf("客户姓名\t身份证号\t电话号码\t房间类型\t房间号\t入住日期\n");

	for(i=1;i<MAX_GUEST;i++)
	{
        //	fread(&room[i],sizeof(struct room),1,fp);
		//	fscanf(fp,"%d %d %d %d\n",&room[i].number,&room[i].type,&room[i].date,&room[i].flag);
		fscanf(fp,"%s %s %s %d %d %d\n",guest[i].name,guest[i].ID,guest[i].phone,&guest[i].type,&guest[i].number,&guest[i].date);
		if(guest[i].number != 0 && guest[i].date != 0)
        	printf("%s\t %s\t %s\t %d\t \t%d\t     %d\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].type,guest[i].number,guest[i].date);

	}
	fclose(fp);
	return 0;
}

/*读取客户信息函数*/
int read_guest()
{
	int i;
	FILE *fp;
	if((fp=fopen("guest_data.txt","r"))==NULL)
	{
			perror("in read_guest oepn error:");
			exit(-1);
	}

	for(i=1;i<MAX_GUEST;i++)
	{
		fscanf(fp,"%s %s %s %d %d %d\n",guest[i].name,guest[i].ID,guest[i].phone,&guest[i].type,&guest[i].number,&guest[i].date);
	}
	fclose(fp);
	return 0;
}

/*读取房间信息函数*/
int read_room(void)
{
	int i,j;
	int type = 0,num = 0,date = 0,flag = 0;
    FILE *fp;
	if((fp=fopen("room_data.txt","r"))==NULL)
	{
            perror("in read_room open error:");
			exit(-1);
	}

	for(i = 1;i<MAX_TYPE;i++)
	{
        for(j = 1;j<MAX_NUM;j++)
        {
            fscanf(fp,"%d %d %d %d\n",&type,&num,&date,&flag);
            if(type != i && num != j)               //如果读到到的房间类型和房间号当前
                                                    //保存的目标房间不一致时要作出调整
            {
                g_room[type][num].type = type;
                g_room[type][num].num = num;
                g_room[type][num].date = date;
                g_room[type][num].flag= flag;
            }
            else if(type != i )
            {
                g_room[type][j].type = type;
                g_room[type][j].num = num;
                g_room[type][j].date = date;
                g_room[type][j].flag = flag;
            }
            else if(num != j)
            {
                g_room[i][num].type = type;
                g_room[i][num].num = num;
                g_room[i][num].date = date;
                g_room[i][num].flag = flag;
            }
            else if(type == i && num == j)
            {
                g_room[i][j].type = type;
                g_room[i][j].num = num;
                g_room[i][j].date = date;
                g_room[i][j].flag = flag;
            }
        }


	}
    fclose(fp);
    return 0;
}

//*******************************************主函数界面****************************************************

int main()
{
	unsigned int z;
	char out[] = "\t\t\t欢迎使用酒店房间预订系统\n\n";
    system("color 3F");           //用来对显示界面颜色修改的
	printf("★★******************************★★******************************★★\n\n");
	for( z=0; z<strlen(out); z++ )
	{
		printf("%c",out[z]);  //动态输出字符串
		Sleep(30);//睡眠30毫秒。
	}
	printf("★★******************************★★******************************★★\n");
    printf("\n");
	printf("\n");
    printf("\t本系统设计者：罗钦源、麦伟伦、陈志炜、陈淑娴、古丽米拉*阿山江\n\n");
	printf("\t\t\t★★c语言课程设计★★\n");
	Sleep(3000); //是等待时间
    jiemian();  //界面函数
	return 0;
}
//*******************************************界面****************************************************
void jiemian()
{
      system("cls");
      system("color 74");
	  int a;
	  printf("\t-----------------------------------------------------------\n");
	  printf("\t★★************* ★欢迎来到宇宙工业酒店★ *************★★\n");
	  printf("\t-----------------------------------------------------------\n");
	  printf("                   |&        1.房间信息        &|          \n");
  	  printf("                   |&        2.预订            &|          \n");
      printf("                   |&        3.退订            &|          \n");
	  printf("                   |&        4.订单查询        &|          \n");
 	  printf("                   |&        5.管理员登陆      &|          \n");
	  printf("                   |&        6.退出系统        &|          \n\n");

	  printf("              请输入1-3后按enter键选择您所需的服务\n ");
Loop_1:                                                              //用来做返回的循环标识符
	scanf("%d",&a);
	if(a!=1&&a!=2&&a!=3&&a!=4&&a!=5&&a!=6)
	{
		  printf("请正确输入对应序号！！！\n");
		   goto Loop_1;                                              //返回前面定义了的标识符
	}
	switch(a)
    {
		  case 1:
			  fangjianxinxi();break;
		  case 2:
			 fangjian();break;
          case 3:
			 tuiding();break;
		  case 4:
			 dingdanchaxun();break;
		  case 5:
			 guanliyuan();break;
		  case 6:
			 Outsystem();break;

	}
}
//*******************************************房间信息函数***********************************************
void fangjianxinxi()
{
    	int j;
    system("cls");//清屏
	system("color 3E");//预设屏幕文字颜色
	printf("★★========================================================================★★\n");
	printf("                   欢迎来到宇宙工业酒店房间信息浏览界面\n");
    printf("                   ====================================\n\n");
	printf("★★*********************★★★★★★★★************************★★\n");
	printf("                         ☆  房间信息  ☆\n");
	printf("★★*********************★★★★★★★★************************★★\n");
	printf("          ||     普通单人房：  单人床  1-2楼   ￥200/天     ||\n");
    printf("          ||     标准双人房：  双人床  3-4楼   ￥300/天     ||\n");
	printf("          ||     豪华大床房：   大 床   5 楼   ￥500/天     ||\n");
	printf("\n  有合适您的房间吗，是否要登记入住:\n");
	printf("                            1、进入预订界面\n");
    printf("                            2、返回上一层\n");
	printf("                            3、退出系统\n");
    printf("请输入数字1-3选择：\n");
Loop_7:

	scanf("%d",&j);
	if (j!=1&&j!=2&&j!=3)
	{
		printf("\n输入有误!  请重新输入,谢谢！\n\n\n");
		goto Loop_7;

	}
	else
	{
		switch(j)
		{
		case 1:
			 fangjian();break;
		case 2:
			 jiemian();break;
		case 3:
			 Outsystem();break;
		}
	}
}
//******************************************* 登记入住函数***********************************************



void fangjian(void)
{
    struct room__message room;
    static int count = 1;
    int num_day = 0;
    int sure = 0;
    char c;
    read_room();
	system("cls");//清屏
	system("color 74");//预设界面颜色
    printf(" ★★*******************以下为本店政策,请客户予以配合*******************★★\n");
    printf("         1、请携带入住人身份证,凭姓名和预订手机号直接办理入住\n");
    printf("         2、入住时间:请当天14:00之后入住并于次日12:00之前退房;\n");
    printf("            如需提前入住或延时退房,请咨询商家\n");
    printf("         3、入住需要押金,金额以前台为准\n");
    printf(" ★★******************************★★******************************★★\n");

    printf("\n\n********************************登记入住********************************\n\n");
    printf("\n请输入您打算入住的房间类型    1、豪华大床房    2、标准双人房    3、普通单人房\n");
    scanf("%d",&room.type);
    while(room.type!=1&&room.type!=2&&room.type!=3)
    {
        printf("您输入有误,请输入数字1或2或3,请再次输入:\n");
        scanf("%d",&room.type);
    }

    printf("\n\n请输入您入住的房间号(1-2-3):\n");
    scanf("%d",&room.num);
    while(room.num!=1&&room.num!=2&&room.num!=3)
    {
        printf("您输入有误,请输入数字1或2或3,请再次输入:\n");
        scanf("%d",&room.num);
    }

    printf("\n\n请输入您打算入住的日期：\n");
    scanf("%d",&room.date);
    while(room.date<1||room.date>31)
    {
        printf("您输入有误,请重新输入:\n");
        scanf("%d",&room.date);
    }

    printf("\n\n请您输入预订的天数:(最多只能预订两天):\n");
    scanf("%d",&num_day);
    while(num_day>2||num_day<=0)
    {
        printf("您输入有误,请重新输入(最多只能预订两天):\n");
        scanf("%d",&num_day);
    }
    if(num_day ==  1)
    {
        if((g_room[room.type][room.num].flag == 1)  && (g_room[room.type][room.num].date == room.date) || ((g_room[room.type][room.num].flag == 2) && (g_room[room.type][room.num].date + 1 == room.date)) )
        {
            printf("抱歉，当天您要预订的房间已满！\n\n");
            Sleep(3000); //是等待时间
            printf("请按任意键，重新进行预订:\n");
    		scanf("%c",&c);
    		scanf("%c",&c);
            fangjian();
          	return;
        }
        else
        {
            //登记输入信息填写个人信息
            if(room.type == 1)
                printf("\n您选择的是豪华大床房, ￥500/天,您的房号%d!\n",room.num);
            else if(room.type == 2)
                printf("\n您选择的是标准双人房,￥300/天,您的房号%d!\n",room.num);
            else if(room.type == 3)
                printf("\n您选择的是普通单人房,￥200/天,您的房号%d!\n",room.num);

            printf("\n\n请输入您的姓名：\n");
            scanf("%s",guest[count].name);
            while(strlen(guest[count].name)<0)
             {
                 printf("\n\n您的姓名输入有误,请您重新输入:\n");
                 scanf("%s",guest[count].name);
             }

            printf("\n\n请输入您的身份证号码：\n");
            scanf("%s",guest[count].ID);
            while((strlen(guest[count].ID)) != 18)
            {
                printf("\n\n您的身份证号码输入有误,请您重新输入:\n");
                scanf("%s",guest[count].ID);
            }

            printf("\n\n请输入您的手机号码：\n");
            scanf("%s",guest[count].phone);
            while((strlen(guest[count].phone)) != 11)
            {
                printf("\n\n您的手机号码输入有误,请您重新输入:\n");
                scanf("%s",guest[count].phone);
            }

            printf("\n\n您是否确定您的信息:(1-是,2-否):\n");
            scanf("%d",&sure);
            while(sure<1 || sure>2)
            {
                printf("\n\n您的选择有误,请您重新输入:\n");
                scanf("%d",&sure);
            }
             if(sure == 1)
             {
                 printf("\n\n您已登记入住成功,祝您在本店住得愉快!\n\n");
                 Sleep(2000);
            }
             else
             {
                 printf("\n\n请重新登记\n");
                 Sleep(1000);
                 fangjian();
                 return;
             }
        }
        guest[count].number = room.num;
	    guest[count].date = room.date;
	    guest[count].type = room.type;
	    save_guest(count);      //保存当前客户的信息
	    count++;
	    if(count == 10)
	    {
	    	count = 1;
		}

	    g_room[room.type][room.num].type = room.type;
	    g_room[room.type][room.num].num = room.num;
	    g_room[room.type][room.num].date = room.date;
	    g_room[room.type][room.num].flag = 1;
	    save_room(room.type,room.num);
    }
    else if(num_day == 2)  //如果连续预订两天
    {

    	if((g_room[room.type][room.num].date == room.date) && (g_room[room.type][room.num].flag == 2) || (g_room[room.type][room.num].date == room.date) && (g_room[room.type][room.num].flag == 1))
        {
            printf("抱歉，当天您要预订的房间已满！\n");
            Sleep(3000); //是等待时间
            printf("请按任意键，重新进行预订:\n");
    		scanf("%c",&c);
    		scanf("%c",&c);
            fangjian();
          	return;
        }
        else
        {
            //登记输入信息填写个人信息
            if(room.type == 1)
                printf("\n您选择的是豪华大床房,￥500/天,您的房号%d!\n",room.num);
            else if(room.type == 2)
                printf("\n您选择的是标准双人房,￥300/天,您的房号%d!\n",room.num);
            else if(room.type == 3)
                printf("\n您选择的是普通单人房,￥200/天,您的房号%d!\n",room.num);

            printf("\n\n请输入您的姓名：\n");
            scanf("%s",guest[count].name);
            while(strlen(guest[count].name)<0)
             {
                 printf("\n\n您的姓名输入有误,请您重新输入:\n");
                 scanf("%s",guest[count].name);
             }

            printf("\n\n请输入您的身份证号码：\n");
            scanf("%s",guest[count].ID);
            while((strlen(guest[count].ID)) != 18)
            {
                printf("\n\n您的身份证号码输入有误,请您重新输入:\n");
                scanf("%s",guest[count].ID);
            }

            printf("\n\n请输入您的手机号码：\n");
            scanf("%s",guest[count].phone);
            while((strlen(guest[count].phone)) != 11)
            {
                printf("\n\n您的手机号码输入有误,请您重新输入:\n");
                scanf("%s",guest[count].phone);
            }

            printf("\n\n您是否确定您的信息:(1-是,2-否):\n");
            scanf("%d",&sure);
            while(sure<1 || sure>2)
            {
                printf("\n\n您的选择输入有误,请您重新输入:\n");
                scanf("%d",&sure);
            }
             if(sure == 1)
             {
                 printf("\n\n您已预订成功,祝您在本店住得愉快!\n\n");
                 Sleep(2000);
            }
             else
             {
                 printf("\n\n请重新登记\n");
                 Sleep(1000);
                 fangjian();
                 return;
             }
        }
        guest[count].number = room.num;
	    guest[count].date = room.date;
	    guest[count].type = room.type;
	    save_guest(count);      //保存当前客户的信息
	    count++;
	    if(count == 10)
	    {
	    	count = 1;
		}

	    g_room[room.type][room.num].type = room.type;
	    g_room[room.type][room.num].num = room.num;
	    g_room[room.type][room.num].date = room.date;
	    g_room[room.type][room.num].flag = 2;
	    save_room(room.type,room.num);
	}

    printf("按任意键返回主界面:");
    scanf("%c",&c);
    scanf("%c",&c);
    jiemian();
}







//******************************************* 用户订单信息查询函数***********************************************
void dingdanchaxun()
{
	read_guest();
	static label = 0;
	int i,flag=1;  //flag也是用来进行位标志
	int a;
	char id[19];
	printf("请输入您的身份证号:（18位）\n");
	scanf("%s",id);
	for(i=1;i<MAX_GUEST;i++)
	{                                             // 在所有的纪录中查找
		if(strcmp(id,guest[i].ID) == 0)          //找到纪录，输出信息
		{
            printf("\n\t\t\t您已成功进入用户订单界面······\n\n");
            Sleep(1000);
			printf("\n\n★★******************************★★******************************★★\n");
			printf("★★             ★★         您的订单信息         ★★             ★★\n");
			printf("★★******************************★★******************************★★\n\n");
		    printf(" 姓名        : %s\n",guest[i].name);
		    printf("身份证号     : %s\n",guest[i].ID);
			printf("电话号码     : %s\n\n",guest[i].phone);
			if(guest[i].type == 3)
				{
					printf("房间类型     : 普通单人房\t\t\n");
					printf("房间号       : %d 号\n",guest[i].number);
					a=200;
				}

			if(guest[i].type == 2)
			 	{
					printf("房间类型     : 标准双人床\t\t\n");
					printf("房间号       : %d 号\n",guest[i].number);
					a=300;
				}

			if(guest[i].type == 1)
				{
					printf("房间类型     : 豪华大床房\t\t\n");
					printf("房间号       : %d 号\n",guest[i].number);
					a=500;
				}

		    flag=0;//用来判断顾客输入的信息查询是否有错
		    printf("您的预订天数 ：%d 号\t\t",guest[i].date);
		    printf("您应付费用   ：%d 元 \n" ,guest[i].date*a);
			printf("\n您可以进行下列操作 ：\n  \t\t\t1-返回界面\n  \t\t\t\n");
			scanf ("%d",&i);
			while(i!=1&&i!=2)
		 {  printf("输入有误，请重新选择：\n");
		    scanf("%d",&i);
		 }
	        switch(i)
		    {
			case 1: jiemian();break;
	//	    case 2:tuiding();break;
		    }

        }

	//	else
	//		continue;  //继续查找
    }
	if(flag)
	{
		label++;
		if(label == 3)
		{
			printf("输入三次错误，返回主界面:");
			Sleep(3000);
			jiemian();
			return;
		}
		printf("没有查询到您需要的纪录!!\n");
		printf("请重新输入:\n");
		Sleep(1000);
		jiemian();
		return;
	}

	Sleep(3000);
	jiemian();
}

//*******************************************管理员登录函数***********************************************
void guanliyuan()
{
	system("cls");//清屏
	system("color 74");//设置界面颜色
	int a,c;
	int s;
	int flag = 0;
	fflush(stdin);
	system("cls");
	printf("\n");
	printf("             ★********************************★\n");
	printf("                   ☆ 欢迎进入管理员系统 ☆\n");
	printf("             ★********************************★\n");
	printf("\n");
	read_mima();
Loop_22:
	printf("请输入管理员账号：\n");
	scanf("%d",&c);
	printf("请输入管理员密码：\n");

    scanf("%d",&s);                                           // 输入账号密码 

    if((s-s0)== 0 && (c-z0)== 0)                             // 如果账号密码都输入正确
	{
		char message_hello[] = "\t\t\t管理员,你好!请选择您需要的服务类型：\n\n";
		unsigned int i;
		for(i = 0;i < strlen(message_hello); i++)              //动态输出字符串
		{
			printf("%c",message_hello[i]);
			Sleep(50);
		}
		Sleep(100);
		printf("1、房间查询             2、密码修改            3、退出系统\n\n");
		printf("请选择:\n");
		scanf("%d",&a);
		if(a!=1&&a!=2&&a!=3)
		{
			printf("输入错误，请输入数字1或2或3，请再次输入：\n");
			scanf("%d",&a);

		}
			switch(a)
         {
		  case 1:
			  chaxun();break;
		  case 2:
			  mimaxiugai();break;
		  case 3:
			  Outsystem();break;
	     }
    }
    else
	{
		flag ++;
		if(flag == 3)
		{
			printf("输入错误超过三次，系统自动退出.....");
			Sleep(2000);
			Outsystem();
		}
		printf("输入错误，请重新输入:\n\n");
		goto Loop_22;
	}

}
//*******************************************房间查询函数***********************************************
void chaxun()
{
	system("color 74");
	system("cls");
	int a;
	int b;
	int c;
	int e;
	int f;
	int i;

	printf("请选择您想要的查询类型：\n     1、历史预订信息     2、按房号查询     3、按日期查询\n");
	scanf("%d",&a);
	while(a!=1&&a!=2&&a!=3)
	{
		printf("请重新输入：\n");
		scanf("%d",&a);
	}
	if(a==1)
	{
		system("cls");
		read_printf_guest();
		printf("\n1、返回上一层         2、返回主菜单         3、退出系统\n");
		scanf("%d",&b);
		if(b!=1&&b!=2&&b!=3)
		{
		    printf("请输入数字0~3，请重新输入：\n");
		    scanf("%d",&b);
		}
		switch(b)
		{
		case 1:
			chaxun();
		case 2:
			jiemian();
		case 3:
			Outsystem();
		}
	}
	if(a==2)
	{
		system("cls");
		printf("请输入您需要查询的房间号：\n");
		scanf("%d",&c);
		while(c!=1&&c!=2&&c!=3)
		{
		    printf("请重新输入：\n");
		    scanf("%d",&c);
		}
        printf("客户姓名\t身份证号\t电话号码\t房间类型\t房间号\t入住日期\n");
		read_guest();
	    for(i=0;i<MAX_GUEST;i++)
		{
			if(guest[i].number==c)
		    printf("%s\t %s\t %s\t %d\t %d\t %d\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].type,guest[i].number,guest[i].date);
		}
			printf("\n\t1、返回上一层     2、返回主菜单     3、退出系统\n");
		scanf("%d",&b);
		while(b!=1&&b!=2&&b!=3)
		{
		    printf("请重新输入：\n");
		    scanf("%d",&b);
		}
		switch(b)
		{
		case 1:
			chaxun();
		case 2:
			jiemian();
		case 3:
			Outsystem();
		}
	}
	if(a==3)
	{
		system("cls");
		printf("请输入您需要查询的日期：\n");
		scanf("%d",&e);
        printf("客户姓名\t身份证号\t电话号码\t房间类型\t房间号\t入住日期\n");
	   int read_guest();
		for( i=0;i<MAX_GUEST;i++)
		{
			if(guest[i].date==e)
			printf("%s\t %s\t %s\t %d\t %d\t %d\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].type,guest[i].number,guest[i].date);
		}
			printf("\n1、返回上一层；2、返回主菜单；3、退出系统\n");
		scanf("%d",&b);
		while(b!=1&&b!=2&&b!=3)
		{
		    printf("请重新输入：\n");
		    scanf("%d",&b);
		}
		switch(b)
		{
		case 1:
			chaxun();
		case 2:
			jiemian();
		case 3:
			Outsystem();
		}
	}

}
//*******************************************改密码函数***********************************************
int mimaxiugai()
{
    system("cls");

    int x;
	printf("是否更改管理员登陆密码？\n");
	printf("请选择：\n\n");
	printf("\t\t       ********************************\n");
	printf("\t\t       ||      1、更改密码           ||\n");
    printf("\t\t       ||      2、返回管理员界面     ||\n");
Loop_m:
	scanf("%d",&x);
	if (x == 1)
	{
		int *p;
        int m;
		p=&s0;
		printf("请输入新的密码：");
		scanf("%d",&m);
		*p = m;
		printf("修改密码成功！");
		empty_file("mima_data.txt");
		save_mima();
		Sleep(1000);
	    guanliyuan();
		return s0;    //返回的是修改后的管理员登陆密码
	}
  else if(x == 2)
  {
	  guanliyuan();

  }
  else
  {
	  printf("你输入的数据有误，请重新输入：\n");
      Sleep(1000);
	  goto Loop_m;                   //当数据输入有错时的跳转标志
  }
}
//*******************************************退出界面函数***********************************************
void Outsystem()
{
	fflush(stdin);
	system("cls");
	printf("\n");
	printf("                    ★******************************★\n");
	printf("                              ☆ 感谢使用 ☆\n");
	printf("                    ★******************************★\n");
	printf("\n");
	int i;
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	printf("                         ^-^ ^-^ ^-^ ^-^ ^-^ ^-^   \n");
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	char message_out[] = "\n\t宇宙工业酒店房间预订系统,非常感谢你的使用，欢迎再来！\n\n\t\t\t\n\n\n\n";
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	printf("                         ^-^ ^-^ ^-^ ^-^ ^-^ ^-^   \n");
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	for( i = 0; i < strlen(message_out ); i++ )
	{
		printf("%c",message_out[i]);
		Sleep(30);//睡眠30毫秒。
	}
	printf("\n");
	exit(0);
}


void tuiding()

{

    char id[19];
    int i,r;



      printf("请输入您需要退订的身份号：\n");
      scanf("%s",id);

	    read_guest();
		for( i=1;i<MAX_GUEST;i++)
		{

            if(strcmp(id,guest[i].ID) == 0)
           {


			printf(" 姓名：%s\n 身份证号码：%s\n 手机号码：%s\n 房号：%d号\n 预订日期：%d\n\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].number,guest[i].date);
			printf("YES(输入1) or NO(输入2)\n");
			 scanf("%d",&r);
			 if(r==1)
             {
             delete("guest_data.txt",i);
            delete("room_data.txt",i);
             printf("退订成功");
             Sleep(2000);

              jiemian();
             }
             if(r==2)
             {
               jiemian();
             }
           }
		}





}
