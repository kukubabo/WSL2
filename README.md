# Windows10 환경에서 WSL2 설치하기
## WSL(Windows Subsystem for Linux)이란?  
Windows10 에서 Native로 리눅스를 실행할 수 있도록 제공하는 리눅스 호환 커널 인터페이스를 제공하는 기능이다.
이를 통해서 별도 Virtual Machine 프로그램(ex. Oracle Virtual Box) 없이 Linux 머신을 Windows 위에서 실행시킬 수 있다.

## (사전작업) Windows Terminal 설치
PowerShell이나 WSL 명령 수행을 위해 Windows Terminal 을 설치한다.  
`WSL 구성 후 Linux 설치 및 접속을 위한 필수 프로그램이다.`  
1. Microsoft Store 에서 "Windows Terminal" 검색한다.  
2. "받기" 를 눌러서 설치한다.  

## 1단계. Linux용 Windows 하위 시스템 사용
Windows에서 Linux 배포를 설치하려면 먼저 "Linux용 Windows 하위 시스템" 옵션 기능을 사용하도록 설정한다.  
`명령 수행은 0단계에서 설치한 Windows Terminal 을 "관리자 모드"로 실행시켜서 수행한다.`
```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## 2단계. WSL2 실행을 위한 요구 사항 확인
WSL2로 업데이트하려면 다음의 OS 버전 이상 설치되어 있어야 한다.  
* x64 시스템의 경우: 버전 1903 이상, 빌드 18362 이상  
* ARM64 시스템의 경우: 버전 2004 이상, 빌드 19041 이상  
* 18362보다 낮은 빌드는 WSL2를 지원하지 않는다. [윈도우 업데이트](https://www.microsoft.com/software-download/windows10)를 한 뒤 진행한다.

버전 및 빌드 확인은 <kbd>Windows 로고 키</kbd> + <kbd>R</kbd> 버튼 을 누른 뒤 "winver" 를 실행하면 정보 확인할 수 있다.

## 3단계. Virtual Machine 기능 사용
WSL2를 설치하려면 먼저 Virtual Machine 플랫폼 옵션 기능을 사용하도록 설정해야 한다.  
`명령 수행은 0단계에서 설치한 Windows Terminal 을 "관리자 모드"로 실행시켜서 수행한다.`
```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## 4단계. WSL2를 기본 버전으로 설정
WSL 사용시 WSL2 버전을 기본 버전으로 사용하도록 설정한다.  
`명령 수행은 0단계에서 설치한 Windows Terminal 을 "관리자 모드"로 실행시켜서 수행한다.`
```PowerShell
wsl --set-default-version 2
```

## 5단계. Linux 배포판 설치
### 1. Microsoft Store 에서 설치
Microsoft Store에 다양한 리눅스 배포판이 지원된다. (2021-03-03 기준)  
#### 무료버전
* Ubuntu - [16.04 LTS](https://www.microsoft.com/store/apps/9pjn388hp8c9), [18.04 LTS](https://www.microsoft.com/store/apps/9N9TNGVNDL3Q), [20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
* openSUSE Leap - [15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
* SUSE Linux Enterprise Server - [12 SP5](https://www.microsoft.com/store/apps/9MZ3D1TRP8T1), [15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)
* [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
* [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
* [Alpine WSL](https://www.microsoft.com/store/apps/9p804crf0395)
#### 유료버전
* [Fedora Remix for WSL](https://www.microsoft.com/store/apps/9n6gdm4k2hnc)
* [Pengwin](https://www.microsoft.com/store/apps/9NV1GV1PXZ6P)
* [Pengwin Enterprise](https://www.microsoft.com/store/apps/9N8LP0X93VCP)
### 2. CentOS 수동 설치
CentOS는 Microsoft Store에 공식 지원되지 않아서 3rd-party 이미지를 설치해서 사용하면 된다.  
#### 설치 방법
1. https://github.com/mishamosher/CentOS-WSL 에서 원하는 버전(CentOS6, 7, 8)의 링크를 타고 들어가서 설치 파일(zip 파일)을 다운로드 한다.
2. 다운로드 받은 zip 파일의 압축을 풀고, 적절한 경로에 이동시켜둔다.(ex. C:\WSL\CentOS7)
3. 해당 경로(ex. C:\WSL\CentOS7)로 이동해서 "CentOS.exe" 파일을 "관리자 모드"로 실행시킨다. (실치 수행)  
실행 완료되면 동일 경로상에 ext4.vhdx 라는 파일(Linux에서 사용하는 파일시스템 파일)이 생성된다.
#### 삭제 방법
1. Windows Terminal을 "관리자 모드"로 실행한다.
2. Terminal 화면에서 CentOS 설치 경로로 이동 후 "CentOS.exe /claen" 명령을 두 번 수행한다.  
(3rd-party 제품이라 그런지 2번 수행해야 정상적으로 삭제되는 것으로 확인됨)
```PowerShell
PS C:\> cd C:\WSL\CentOS7
PS C:\WSL\CentOS7> CentOS.exe /clean
PS C:\WSL\CentOS7> CentOS.exe /clean
```

## 6단계. WSL을 이용하여 Linux 실행 및 접속
Windows Terminal 실행 후 다음의 명령을 수행하여 Linux에 접속한다.
```PowerShell
## WSL 배포 목록 확인 ( * 표시된 이미지가 Default 이미지 )
PS C:\> wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu                 Stopped         2
  CentOS7                Stopped         2

## CentOS7을 Default 이미지로 설정
PS C:\> wslconfig /setdefault CentOS7
PS C:\> wsl -l -v
  NAME                   STATE           VERSION
* CentOS7                Stopped         2
  Ubuntu                 Stopped         2

## CentOS7 실행 및 접속
## wsl 만 입력하면 Default 이미지를 실행하고 해당 Linux로 접속된다. (CentOS는 기본 Root 계정으로 접속된다.)
## Windows 로컬 파일시스템에도 /mnp/드라이브명... 으로 접근 가능하다.
PS C:\> wsl
[root@PCNAME Administrator]# pwd
/mnt/c/Users/Administrator
```



\# 참고사이트  
https://docs.microsoft.com/ko-kr/windows/wsl/install-win10#manual-installation-steps
