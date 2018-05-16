# demo1
#include<iostream>
#include<stdio.h>
#include<vector>
/*20180508demo1
版本1：将一个类的 一个对象 序列化到文件
*/
using namespace std;
class Demo_1 {
	int i;
public:
	Demo_1() {
		i = 0;
	}
	Demo_1(int j) {
		i = j;
	}
	virtual ~Demo_1() {

	}
	bool Serialize(const char*pFilePath) {
		int temp = -1;
		FILE *fp;
		//https://blog.csdn.net/hjjph/article/details/7090770
		//打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。
		if ((fp = fopen(pFilePath, "w+")) == NULL) {
			printf("error\n");
			return false;
		}
		//	https://baike.baidu.com/item/fwrite/10942398?fr=aladdin
		//数据地址 /写入内容的单字节数/要进行写入size字节的数据项的个数/目标文件指针
		//返回实际写入的数据个数
		//size_t fwrite(const void* buffer, size_t size, size_t count, FILE* stream);
		temp = fwrite(&i, sizeof(int), 1, fp);

		//判断写入成功
		if (temp == -1) {
			printf("error\n");
			return false;
		}
		if (fclose(fp) != 0) {
			printf("close error");
			return false;
		}
		printf("write success\n");
		return 0;
	}
	bool Deserialize(const char*pFilePath) {
		int temp = -1;
		FILE *fp;
		if ((fp = fopen(pFilePath, "r+")) == NULL) {
			printf("error\n");
			return false;
		}
		//size_t fread ( void *buffer, size_t size, size_t count, FILE *stream) ;
		temp = fread(&i, sizeof(int), 1, fp);

		//判断读入成功
		if ((temp == -1) || (temp == 0)) {
			printf("error\n");
			return false;
		}
		if (fclose(fp) != 0) {
			printf("close error");
			return false;
		}
		printf("read success\n");
		return 0;
	}
	void f() {
		cout << endl;
		cout << "demo1=" << i << endl;
	 }
};


void main() {
	//Demo_1

	{
		Demo_1 a(131516);
		a.Serialize("demo.txt");
	}
	{
		Demo_1 a;
		a.Deserialize("demo.txt");
		a.f();
	}


}

