---
title: "[ VC++ ] Windows OS 버전 체크" 
sub: post
author: Kwangsoo Seo
date: 2022-07-18 20:00:00 +0900
categories: [정보, 포맷]
tags: [VC++, OSVERSIONINFOEX, GetVersionEx]
---
[![HitCount](https://hits.dwyl.com/MonosLab/post5.svg?style=flat-square)](http://hits.dwyl.com/MonosLab/post5)
{:.no_toc}
---
**Contents**
{:.no_toc}

* ToC와 교체됨
{:toc}  

---
# **윈도우 버전 체크**  
레지스트리의 정보를 이용하여 윈도우의 버전을 체크하는 방법에 대한 정리입니다.

```cpp
enum  OS_INFO_TYPE
{
	SZ_BUILD = 0,		// Current build number
	SZ_CUR_VER,		// Current version
	SZ_DSP_VER,		// Display version
	SZ_ROOT,		// System Root
	DW_MAJOR,		// Current major version number
	DW_MINOR,		// Current minor version number
	DW_UBR,			// UBR (Update Build Revision)
};

namespace osi
{
	const TCHAR kBuildNumber[]	= _T("CurrentBuildNumber");
	const TCHAR kMajorVersion[]	= _T("CurrentMajorVersionNumber");
	const TCHAR kMinorVersion[]	= _T("CurrentMinorVersionNumber");
	const TCHAR kVersion[]		= _T("CurrentVersion");
	const TCHAR kDisplayVersion[]	= _T("DisplayVersion");
	const TCHAR kBuildRevision[]	= _T("UBR");
	const TCHAR kSystemRoot[]	= _T("SystemRoot");
};

struct OS_INFOS
{
	OS_INFO_TYPE dwKey;
	const TCHAR *pszValue;
};

OS_INFOS OSINF[] =	{ {OS_INFO_TYPE::SZ_BUILD	, osi::kBuildNumber	}
			, {OS_INFO_TYPE::SZ_CUR_VER	, osi::kVersion		}
			, {OS_INFO_TYPE::SZ_DSP_VER	, osi::kDisplayVersion	}
			, {OS_INFO_TYPE::SZ_ROOT	, osi::kSystemRoot	}
			, {OS_INFO_TYPE::DW_MAJOR	, osi::kMajorVersion	}
			, {OS_INFO_TYPE::DW_MINOR	, osi::kMinorVersion	}
			, {OS_INFO_TYPE::DW_UBR		, osi::kBuildRevision	}
};

int GetWindowVersion(OS_INFO_TYPE oit)
{
	LONG lResult;
	HKEY hKey;
	DWORD dwType;
	DWORD dwBytes;
	DWORD dwRes = 0;

	// Open Regstry
	lResult = RegOpenKeyEx(HKEY_LOCAL_MACHINE, _T("SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion"), 0, KEY_QUERY_VALUE, &hKey);
	if (lResult != ERROR_SUCCESS)
	{
		goto END;
	}

	// Read Regstry Value
	lResult = RegQueryValueEx(hKey, OSINF[oit].pszValue, 0, &dwType, (LPBYTE)&dwRes, &dwBytes);
	if (lResult == ERROR_SUCCESS)
	{
		RegCloseKey(hKey);
		return dwRes;
	}

END:
	RegCloseKey(hKey);
	return -1;
}

bool GetWindowVersion(OS_INFO_TYPE oit, TCHAR *szData, int nSize = 128)
{
	LONG lResult;
	HKEY hKey;
	DWORD dwType;
	DWORD dwBytes = nSize;

	// Open Regstry
	lResult = RegOpenKeyEx(HKEY_LOCAL_MACHINE, _T("SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion"), 0, KEY_QUERY_VALUE, &hKey);
	if (lResult != ERROR_SUCCESS)
	{
		goto END;
	}

	TCHAR szMsg[256];
	memset(szMsg, 0x00, sizeof(szMsg));
	_stprintf(szMsg, _T("Key : %s\r\n"), OSINF[oit].pszValue);
	OutputDebugString(szMsg);

	// Read Regstry Value
	lResult = RegQueryValueEx(hKey, OSINF[oit].pszValue, 0, &dwType, (LPBYTE)szData, &dwBytes);
	if (lResult == ERROR_SUCCESS)
	{
		return true;
	}

END:
	RegCloseKey(hKey);
	return false;
}

..[중략]...

// 사용법
int _tmain(int argc, _TCHAR *argv[])
{
	TCHAR szCurVer[128];
	TCHAR szDspVer[128];
	TCHAR szBuildVer[128];
	TCHAR szRoot[256];
	int nMajor = 0;
	int nMinor = 0;
	int nUbrvs = 0;
	bool bRet = false;
	memset(szCurVer, 0x00, sizeof(szCurVer));
	memset(szDspVer, 0x00, sizeof(szDspVer));
	memset(szBuildVer, 0x00, sizeof(szBuildVer));
	memset(szRoot, 0x00, sizeof(szRoot));
	bRet = GetWindowVersion(OS_INFO_TYPE::SZ_CUR_VER, szCurVer, sizeof(szCurVer));
	bRet = GetWindowVersion(OS_INFO_TYPE::SZ_DSP_VER, szDspVer, sizeof(szDspVer));
	bRet = GetWindowVersion(OS_INFO_TYPE::SZ_BUILD, szBuildVer, sizeof(szBuildVer));
	bRet = GetWindowVersion(OS_INFO_TYPE::SZ_ROOT, szRoot, sizeof(szRoot));
	nMajor = GetWindowVersion(OS_INFO_TYPE::DW_MAJOR);
	nMinor = GetWindowVersion(OS_INFO_TYPE::DW_MINOR);
	nUbrvs = GetWindowVersion(OS_INFO_TYPE::DW_UBR);

	TCHAR szMsg[2048];
	memset(szMsg, 0x00, sizeof(szMsg));
	_stprintf(szMsg, _T("Version : Windows %ld.%ld(%ld)\r\nCurrentVersion : %s\r\n") \
		_T("DisplayVersion : %s\r\nBuildVersion : %s\r\nRoot : %s\r\n")
		, nMajor, nMinor, nUbrvs, szCurVer, szDspVer, szBuildVer, szRoot);
	AfxMessageBox(szMsg);
}
```

* 참고 : https://neodreamer-dev.tistory.com/795

|운영체제|버전|Major|Minor|기타|   
| :--- | :--- | :--- | :--- | :--- |   
|Windows 11|10.0|10|0|OSVERSIONINFOEX.dwBuildNumber >= 220000|   
|Windows 10|10.0|10|0|OSVERSIONINFOEX.wProductType == VER_NT_WORKSTATION|   
|Windows Server 2016|10.0|10|0|OSVERSIONINFOEX.wProductType != VER_NT_WORKSTATION|   
|Windows 8.1|6.3|6|3|OSVERSIONINFOEX.wProductType == VER_NT_WORKSTATION|   
|Windows Server 2012 R2|6.3|6|3|OSVERSIONINFOEX.wProductType != VER_NT_WORKSTATION|   
|Windows 8|6.2|6|2|OSVERSIONINFOEX.wProductType == VER_NT_WORKSTATION|   
|Windows Server 2012|6.2|6|2|OSVERSIONINFOEX.wProductType != VER_NT_WORKSTATION|   
|Windows 7|6.1|6|1|OSVERSIONINFOEX.wProductType == VER_NT_WORKSTATION|   
|Windows Server 2008 R2|6.1|6|1|OSVERSIONINFOEX.wProductType != VER_NT_WORKSTATION|   
|Windows Server 2008|6.0|6|0|OSVERSIONINFOEX.wProductType != VER_NT_WORKSTATION|   
|Windows Vista|6.0|6|0|OSVERSIONINFOEX.wProductType == VER_NT_WORKSTATION|   
|Windows Server 2003 R2|5.2|5|2|GetSystemMetrics(SM_SERVERR2) != 0|   
|Windows Home Server|5.2|5|2|OSVERSIONINFOEX.wSuiteMask & VER_SUITE_WH_SERVER|   
|Windows Server 2003|5.2|5|2|GetSystemMetrics(SM_SERVERR2) == 0|   
|Windows XP Professional<br>x64 Edition|5.2|5|2|(OSVERSIONINFOEX.wProductType == VER_NT_WORKSTATION)<br>SYSTEM_INFO.wProcessorArchitecture==PROCESSOR_ARCHITECTURE_AMD64
|Windows XP|5.1|5|1|Not applicable|   
|Windows 2000|5.0|5|0|Not applicable|   

