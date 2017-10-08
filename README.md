#include <stdio.h>
#include <stdlib.h>
#include <memory.h>
#include <time.h>
#include<string.h>
using namespace std;
int grid[9][9];
bool generateall(int m, int n)
{
	if (m > 8 || n> 8)//此时数独已完成
		return true;
	int i, j, k;
	for (k = 1; k <= 9; k++)
	{		
		bool flag = true;//flag标记k能否可以放在此位置
		for (i = 0; i<m; i++)
		{		//查询此列是否有相同的数
			if (grid[i][n] == k)
			{
				flag = false;				
				break;
			}
		}
		if (flag)
		{
			for (j = 0; j <n; j++)
			{//查询此行是否有相同的数
				if (grid[m][j] == k)
				{
					flag = false;
					break;
				}

			}
		}
		if (flag)
		{
			//查询所在九宫格是否有相同的数
			int p, q, p1, q1;
			p = m - m % 3;p1 = p + 3; //所在九宫格的行上下限
			q = n - n % 3;q1 = q + 3;//所在九宫格的列上下限
			for (i = p; i <p1; i++)
			{
				for (j = q; j <q1; j++)
				{
					if (grid[i][j] == k)
					{
						flag = false;
						break;
					}
				}
			}
		}
		if (flag)
		{
			grid[m][n] = k;
			if (n < 8)
			{
				if (generateall(m, n + 1)) return true;//到同行的下一列
	
			}
			else
			{
				if (m < 8)
				{
					if (generateall(m + 1, 0))//到下一行的第一列
						return true;
				
				}
				else return true;//此时数独已完成
			}
			grid[m][n] = 0;//当数字1到9都测试失败时，恢复原值
		}

	}

	return false;
}

void generate(int num)
{
	int index = 0, sum = 36, i, j;
	
		srand(num);//设置种子
		for (i = 0; i < 9; i++)
			for (j = 0; j < 9; j++)
			{
				grid[i][j] = 0;//初始化矩阵
			}
		for (int i = 0; i < 8; ++i)//随机产生第一行
		{
			index = rand() % 9;//产生随机数
			while (grid[0][index] != 0)
			{
				index = rand() % 9;
			}
			grid[0][index] = i + 1;
			sum -= index;
		}
		grid[0][sum] = 9;

	}


void show()
{
	int i, j;//输出生成数独
	for (i = 0; i < 9; i++)
	{
		for (j = 0; j < 9; j++)
		{
			printf("%d", grid[i][j]);
		}
		printf("\n");
	}

	FILE *f; //将生成数独写入文件
	f = fopen("shudu.txt","ab+");
	if (f == NULL)
	   {
		  printf("文件打开失败!\n");
		  return;
	    }
	else
	{
          for (i = 0; i < 9; i++)
	      {
		     for (j = 0; j < 9; j++)
		      {			
				  fprintf(f, "%d", grid[i][j]);		
	        	}	
			 fprintf(f,"\r\n");
       	}
		  fprintf(f, "\r\n");
	}		
	 fclose(f);
	

}

void main()
{
	char  a[11] = { 0 },b; 
	int flag = true,i,k=0;
	scanf("%s",&a);	

	for ( i = 0; a[i]!=0; i++)
	{
		
		if (a[i]< 48 || a[i] > 57)
		{
			printf("输入有错！"); flag = false; break;
		}
		else { k = k * 10 + (a[i]-48);
		}
	}
	
	if (flag)
	{
		while (k)
		{
			printf("\n");
			generate(k);
			generateall(1, 0);
			show();
			k--;
		}
		
	}
	
	system("pause");
}
