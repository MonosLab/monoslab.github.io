---
title: "[ VC++ ] 콘솔 프로그램 실행 후 출력 메시지 갈무리" 
sub: post
author: Kwangsoo Seo
date: 2022-06-28 13:52:00 +0900
categories: [VC++]
tags: [VC++, console, redirection, capture]
---
[![HitCount](https://hits.dwyl.com/MonosLab/post1.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post1)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# **콘솔 프로그램 출력 메시지 갈무리**  
어플리케이션에서 콘솔 프로그램의 출력된 메시지를 갈무리(Console redirection) 하는 방법에 대한 정리입니다.   
콘솔창의 메시지 갈무리를 위해서는 CreateProcess와 CreatePipe 함수를 이용하며 아래의 내용에 유의 하여야 합니다.
* dwFlags에 STARTF_USESTDHANDLES을 지정. 
   - 사용자에 의해 지정된 핸들(hStdInput, hStdOutput, hStdError)을 사용.
   - 지정하지 않을 경우 표준 출력의 기본 값인 콘솔 창에 출력.
* hStdOutput에 모니터링에 사용할 핸들 지정.
* hStdError는 오류 모니터링에 사용할 핸들 지정.


```cpp
#include <windows.h>
#include <stdio.h>
#include <tchar.h>

#define PEEK_NAMED_PIPE

TCHAR APP[] = _T("C:\\Windows\\System32\\NETSTAT.EXE");

int _tmain(int argc, _TCHAR *argv[]) 
{
	TCHAR szMsgW[256];
	char szMsg[256];
	char szBuff[256];
	DWORD dwRead = 0, dwOut = 0, dwErr = 0;
	HANDLE hStdOutWrite = NULL, hStdOutRead = NULL;
#ifdef PEEK_NAMED_PIPE
	HANDLE hStdErrWrite = NULL, hStdErrRead = NULL;
#endif

	STARTUPINFO si;
	SECURITY_ATTRIBUTES sa;
	PROCESS_INFORMATION pi;

	sa.nLength = sizeof(SECURITY_ATTRIBUTES);
	sa.lpSecurityDescriptor = NULL;
	sa.bInheritHandle = TRUE;

	HANDLE hReadPipe;
	CreatePipe(&hStdOutRead, &hStdOutWrite, &sa, 0);  // 콘솔에 출력되는 정보와 연결할 파이프 생성
#ifdef PEEK_NAMED_PIPE
	CreatePipe(&hStdErrRead, &hStdErrWrite, &sa, 0);  // 콘솔에 출력되는 오류와 연결할 파이프 생성
#endif

	ZeroMemory(&si, sizeof(STARTUPINFO));
	si.cb = sizeof(STARTUPINFO);
	si.dwFlags = STARTF_USESTDHANDLES | STARTF_USESHOWWINDOW;
	si.hStdInput = NULL;
	si.hStdOutput = hStdOutWrite;
#ifdef PEEK_NAMED_PIPE
	si.hStdError = hStdErrWrite;
#else
	si.hStdError = hStdOutWrite;
#endif
	si.wShowWindow = SW_HIDE;
	if (!CreateProcess(NULL, APP, NULL, NULL, TRUE, CREATE_NEW_CONSOLE, NULL, NULL, &si, &pi))
	{
		memset(szMsg, 0x00, sizeof(szMsg));
		sprintf(szMsg, "LastError : %ld", GetLastError());
		OutputDebugStringA(szMsg);
	}
	CloseHandle(pi.hThread);

#ifdef PEEK_NAMED_PIPE
	while (PeekNamedPipe(hStdOutRead, NULL, 0, NULL, &dwOut, NULL) ||
		PeekNamedPipe(hStdErrRead, NULL, 0, NULL, &dwErr, NULL))  // 읽을 데이터가 있는지 체크
	{
		if (dwOut <= 0 && dwErr <= 0 && WaitForSingleObject(pi.hProcess, 0) != WAIT_TIMEOUT)
			break;  // 콘솔 프로그램이 종료된 경우 loop를 빠져나간다.

		while (PeekNamedPipe(hStdOutRead, NULL, 0, NULL, &dwOut, NULL) && dwOut > 0)
		{
			memset(szBuff, 0x00, sizeof(szBuff));
			ReadFile(hStdOutRead, szBuff, sizeof(szBuff), &dwRead, NULL);
			szBuff[dwRead] = 0;

			memset(szMsg, 0x00, sizeof(szMsg));
			sprintf(szMsg, ">>[R] %s", szBuff);
			OutputDebugStringA(szMsg);
		}

		while (PeekNamedPipe(hStdErrRead, NULL, 0, NULL, &dwErr, NULL) && dwErr > 0)
		{
			memset(szBuff, 0x00, sizeof(szBuff));
			ReadFile(hStdErrRead, szBuff, sizeof(szBuff), &dwRead, NULL);
			szBuff[dwRead] = 0;

			memset(szMsg, 0x00, sizeof(szMsg));
			sprintf(szMsg, ">>[E] %s", szBuff);
			OutputDebugStringA(szMsg);
		}
	}
#else
	char buf[128];
	CString strOutput, strTemp;
	BOOL result;
	do
	{
		memset(buf, 0x00, sizeof(buf));
		result = ::ReadFile(hStdOutRead, buf, sizeof(buf), &dwRead, 0);

		memset(szMsgW, 0x00, sizeof(szMsgW));
		TCHAR *pBuff = CA2CT(buf);
		_stprintf(szMsgW, _T(">>[O] %s"), (TCHAR *)CA2CT(buf));
		OutputDebugString(szMsgW);

		strTemp = CA2CT(buf);
		strOutput += strTemp.Left(dwRead);
	} while (result);

	memset(szMsgW, 0x00, sizeof(szMsgW));
	_stprintf(szMsgW, _T(">>[T] %s"), strOutput);
	OutputDebugString(szMsgW);
#endif

	CloseHandle(pi.hProcess);
	CloseHandle(hStdOutRead);
	CloseHandle(hStdOutWrite);
#ifdef PEEK_NAMED_PIPE
	CloseHandle(hStdErrRead);
	CloseHandle(hStdErrWrite);
#endif

	return 0;
}
```
