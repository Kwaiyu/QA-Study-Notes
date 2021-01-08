追加hosts：

```c++
#include<iostream>
#include<fstream>
#include <STRING>
#include <WTypes.h>
#include <Winbase.h>
using namespace std;
int main()
{
    ofstream outf;
    ifstream inf;
    cout << " 开始设置\n";
    char* lpFileName = "C:\\Windows\\System32\\drivers\\etc\\hosts";
    DWORD   dwAttribute = ::GetFileAttributes(lpFileName);
    DWORD   wrAttribute = dwAttribute;
    if (dwAttribute & FILE_ATTRIBUTE_READONLY)
    {
        wrAttribute = dwAttribute & ~FILE_ATTRIBUTE_READONLY;
        SetFileAttributes(lpFileName, wrAttribute);
    }
    inf.open(lpFileName, ios::in);
    string line;
    bool haskinpan = false;
    while (getline(inf, line))
    {
        if (line == "127.0.0.1 www.baidu.com")
        {
            haskinpan = true;
            cout << " 已经设置\n";
            break;
        }
    }
    inf.close();
    if (haskinpan == false) {
        outf.open("C:\\Windows\\System32\\drivers\\etc\\hosts", ios::app);
        outf << "\n127.0.0.1 www.baidu.com";
        outf << "\n";
        outf.close();
        cout << "设置成功\n";
    }
    SetFileAttributes(lpFileName, dwAttribute);
    cout << " 按任意键关闭\n";
    system("pause");
    return 0;
}
```

删除文件中的指定内容：

```c++
#include<iostream>
#include<fstream>
#include <STRING>
#include <WTypes.h>
#include <Winbase.h>
using namespace std;
int main()
{
    ofstream outf;
    ifstream inf;
    cout << " 开始设置\n";
    char* lpFileName = "C:\\Windows\\System32\\drivers\\etc\\hosts";
    DWORD   dwAttribute = ::GetFileAttributes(lpFileName);
    DWORD   wrAttribute = dwAttribute;
    if (dwAttribute & FILE_ATTRIBUTE_READONLY)
    {
        wrAttribute = dwAttribute & ~FILE_ATTRIBUTE_READONLY;
        SetFileAttributes(lpFileName, wrAttribute);
    }
    inf.open(lpFileName, ios::in);
    string line;
    bool haskinpan = false;
    while (getline(inf, line))
    {
        if (line == "127.0.0.1 www.baidu.com")
        {
            haskinpan = true;
            cout << " 已经设置\n";
            break;
        }
    }
    inf.close();
    if (haskinpan == false) {
        outf.open("C:\\Windows\\System32\\drivers\\etc\\hosts", ios::app);
        outf << "\n127.0.0.1 www.baidu.com";
        outf << "\n";
        outf.close();
        cout << "设置成功\n";
    }
    SetFileAttributes(lpFileName, dwAttribute);
    cout << " 按任意键关闭\n";
    system("pause");
    return 0;
}
```

