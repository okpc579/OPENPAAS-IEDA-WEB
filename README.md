# Table of Contents

1. [문서 개요](#1)

   * [목적](#2)
   * [범위](#3)

2. [플랫폼 설치 가이드](#4)

   * [플랫폼 설치 자동화 관리](#5)

   * [로그인 계정 관리](#6)

   * [스템셀과 릴리즈](#7)

   * [BOOTSTRAP 설치하기](#8)

     * [스템셀 다운로드](#9)
     * [릴리즈 다운로드](#10)
     * [디렉터 인증서 생성](#11)
     * [BOOTSTRAP 설치](#12)

   * [CF-Deployment 설치하기](#13)

     * [스템셀 업로드](#14)
     * [PaaS-TA 릴리즈 사용](#15)
     * [CF-Deployment 설치](#16)

     

# <div id='1'/>1.  문서 개요

## <div id='2'/>1.1.  목적

본 문서는 플랫폼 설치 자동화 시스템의 사용 절차에 대해 기술하였다.

## <div id='3'/>1.2.  범위

본 문서에서는 Warden 인프라 환경을 기준으로 플랫폼 설치 자동화를 사용하여 PaaS-TA를 설치하는 방법에 대해 작성되었다.

# <div id='4'/>2.  플랫폼 설치 가이드

BOSH는 클라우드 환경에 서비스를 배포하고 소프트웨어 릴리즈를 관리해주는 오픈 소스로 Bootstrap은 하나의 VM에 디렉터의 모든 컴포넌트를 설치한 것으로 PaaS-TA 설치를 위한 관리자 기능을 담당한다.

플랫폼 설치 자동화를 이용해서 클라우드 환경에 PaaS-TA를 설치하기 위해서는 인프라 설정, 스템셀 소프트웨어 릴리즈, Manifest 파일, 인증서 파일 5가지 요소가 필요하다. 스템셀은 클라우드 환경에 VM을 생성하기 위해 사용할 기본 이미지이고, 소프트웨어 릴리즈는 VM에 설치할 소프트웨어 패키지들을 묶어 놓은 파일이고, Manifest파일은 스템셀과 소프트웨어 릴리즈를 이용해서 서비스를 어떤 식으로 구성할지를 정의해 놓은 명세서이다. 다음 그림은 BOOTSTRAP을 이용하여 PaaS-TA를 설치하는 절차이다.

![install_flow.png](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/install_flow.png?raw=true)

## <div id='5'/>2.1  플랫폼 설치 자동화 관리

플랫폼 디렉터의 코드/권한/사용자 관리를 통해 전체 사용자의 생성 및 사용 권한과 공통 Manifest 버전/신규 스템셀 확장 등의 공통 코드를 사용할 수 있다.

## <div id='6'/>2.2.  로그인 계정 관리

플랫폼 설치 자동화 웹 화면에서 “플랫폼 관리자 관리” -> “로그인 계정 관리” 메뉴로 이동한다. 플랫폼 설치 자동화는 “로그인 계정 관리” 메뉴에서 기본적으로 플랫폼 설치 자동화 관리자 정보(admin/admin)를 제공한다.

### 1. *로그인 계정 등록*

1.	사용자 “등록” 버튼을 클릭 후 사용자 정보 입력 및 해당 사용자의 권한을 선택하여 “확인” 버튼을 클릭한다.
2.	사용자 등록 후 초기 비밀번호는 “1234” 이며, 최초 로그인 후 비밀번호를 변경할 수 있다.

![user_add.png](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/user_add.png?raw=true)

### 2. *로그인 계정 수정*

1.	사용자 “수정” 버튼을 클릭 후 사용자 정보 및 해당 권한을 수정하여 “확인” 버튼을 클릭한다.
2.	관리자는 선택한 사용자의 아이디는 수정할 수 없지만 비밀번호를 변경할 수 있다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/user_modify.png?raw=true)



## <div id='7'/>2.3  스템셀과 릴리즈

플랫폼 설치 자동화를 통해 배포 가능한 BOOTSTRAP 버전은 아래와 같으며, 아래의 지원 가능한 버전을 이용하지 않을 경우 에러가 발생할 수 있다. 따라서 아래의 릴리즈 버전으로 다운로드 및 설치한다.

<table>
    <tr>
        <td>BOSH 릴리즈</td>
        <td>CPI 릴리즈</td>
        <td>BPM</td>
        <td>스템셀</td>
    </tr>   
	<tr>
        <td>bosh/270.2.0</td>
        <td>bosh-warden-cpi/40.0.0</td>
        <td>bpm/1.1.0</td>
        <td>bosh-vsphere-esxi-ubuntu-xenial-go_agent/315.64</td>
    </tr>
</table>



플랫폼 설치 자동화를 통해 배포 가능한 CF-Deployment 버전은 아래와 같으며, 아래의 릴리즈 버전으로 다운로드&업로드 및 설치한다.

<table>
    <tr>
        <td>BOSH 릴리즈</td>
        <td>CPI 릴리즈</td>
    </tr>
		<tr>
        <td>paasta/5.0</td>
        <td>bosh-warden-boshlite-ubuntu-xenial-go_agent/315.64</td>
    </tr>
</table>



## <div id='8'/>2.4  BOOTSTRAP 설치하기

플랫폼 설치 자동화를 이용하여 BOOTSTRAP을 설치하고, 디렉터로 등록하는 절차는 다음과 같다.

### <div id='9'/>2.4.1 *스템셀 다운로드*

플랫폼 설치 자동화 웹 화면에서 “환경설정 및 관리” -> “스템셀 관리” 메뉴로 이동한다. “스템셀 관리” 메뉴에서는 Cloud Foundry에서 제공하는 공개 스템셀을 다운로드할 수 있는 기능을 제공한다.
스템셀 다운로드 유형은 총 3가지이며 Version유형으로 다운로드가 안될 경우 로컬에서 다운로드 후 로컬에서 선택 유형/스템셀 다운로드 URL을 통해 다운로드 받는 유형을 이용한다.
상단에 위치한 “등록” 버튼을 클릭 후 스템셀 정보를 입력하고 “확인” 버튼을 클릭한다.


	https://bosh.io/stemcells/bosh-vsphere-esxi-ubuntu-xenial-go_agent

**가이드에서는 버전 Ubuntu Xenial 315.64를 다운로드 하였다.**

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/%EC%8A%A4%ED%85%9C%EC%85%80%EB%93%B1%EB%A1%9D.png?raw=true)

※	스템셀 등록 입력 정보

  - 스템셀 명: 등록할 스템셀 명
  - IaaS 유형: PaaS-TA 설치 인프라를 선택
  - OS 유형: 스템셀 OS 유형 선택 현재 Warden에서는 Ubuntu를 지원한다.
  - OS 버전: OS의 버전 선택
  - 스템셀 다운 유형: 다운로드 유형 선택

### <div id='10'/>2.4.2 *릴리즈 다운로드*

BOOTSTRAP을 설치하기 위해서는 BOSH 릴리즈와 BOSH CPI릴리즈 2개의 릴리즈가 필요하며, BOSH 릴리즈 버전 266 이상일 경우 BPM 릴리즈가 더 추가되어 총 3개의 릴리즈가 필요하다.
릴리즈 다운로드 유형은 총 3가지이며 Version유형으로 다운로드가 안될 경우 로컬에서 다운로드 후 로컬에서 선택 유형/릴리즈 다운로드 URL을 통해 다운로드 받는 유형을 이용한다.
릴리즈를 다운로드하기 위해 플랫폼 설치 자동화 웹 화면에서 “환경설정 및 관리” -> “릴리즈 관리” 메뉴로 이동 후 상단에 위치한 “등록” 버튼을 클릭하고, 릴리즈 등록 팝업 화면에서 릴리즈 정보 입력 후 “등록” 버튼을 클릭한다.

#### 1. *BOSH 릴리즈*

1. 릴리즈 등록 팝업 화면에서BOSH 릴리즈 정보를 입력하고, “등록” 버튼 클릭한다.

2. BOSH 릴리즈 참조 사이트

   [http://bosh.io/releases/github.com/cloudfoundry/bosh?all=1](http://bosh.io/releases/github.com/cloudfoundry/bosh?all=1)

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/%EB%A6%B4%EB%A6%AC%EC%A6%88%EB%93%B1%EB%A1%9D.png?raw=true)

**본 가이드에서는 v270.2.0을 다운로드 하였다.**

#### 2. *BOSH CPI 릴리즈*

1.	릴리즈 등록 팝업화면에서 BOSH CPI릴리즈 정보를 입력하고, “등록” 버튼 클릭한다.
2.	BOSH-CPI 릴리즈 참조 사이트

<table>
  <tr>
    <td>인프라 환경</td>
    <td>참조 사이트</td>
  </tr>
  <tr>
    <td>Warden</td>
    <td>https://bosh.io/releases/github.com/cppforlife/bosh-warden-cpi-release?all=1</td>
  </tr>
</table>



![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/%EB%A6%B4%EB%A6%AC%EC%A6%88%EB%93%B1%EB%A1%9D(cpi).png?raw=true)

**본 가이드에서는 v40.0.0을 다운로드 하였다.**

#### 3. *BPM 릴리즈*

1. 릴리즈 등록 팝업화면에서 BPM릴리즈 정보를 입력하고, “등록” 버튼 클릭한다.

2. BPM 릴리즈 참조 사이트

   https://bosh.io/releases/github.com/cloudfoundry/bpm-release?all=1

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/%EB%A6%B4%EB%A6%AC%EC%A6%88%EB%93%B1%EB%A1%9D(bpm).png?raw=true)

**본 가이드에서는 v1.1.0을 다운로드 하였다.**

#### 4. *OS CONF 릴리즈*

1. 릴리즈 등록 팝업화면에서 OS CONF 릴리즈 정보를 입력하고, “등록” 버튼 클릭한다.

2. OS CONF 릴리즈 참조 사이트

   https://bosh.io/releases/github.com/cloudfoundry/os-conf-release?all=1

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/%EB%A6%B4%EB%A6%AC%EC%A6%88%EB%93%B1%EB%A1%9D(os-conf).png?raw=true)

**본 가이드에서는 v21.0.0을 다운로드 하였다.**

### <div id='11'/>2.4.3 *디렉터 인증서 생성*

BOOTSTRAP을 설치하기 위해서는 Nats/Director 컴포넌트를 사용하기 위한 인증서 정보, 디렉터 인증서가 필요하며 디렉터 인증서를 생성하기 위해 플랫폼 설치 자동화 웹 화면에서 “환경설정 및 관리” -> “디렉터 인증서 관리” 메뉴로 이동 후 상단에 위치한 “등록” 버튼을 클릭하고, 디렉터 인증서 팝업 화면에서 디렉터 인증서 정보 입력 후 “등록” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/diretor_credential_add.png?raw=true)

※	디렉터 인증서 등록 정보

  - 디렉터 인증서 명: 등록할 디렉터 인증서 명

  - BOOTSTRAP 공인 IPs 입력항목은 사용자의 설정에 따라 사용시 값을 입력할 수 있고, 사용하지 않을 경우 이를 제외한 항목만 채워 넣어도 디렉터 인증서 생성이 가능하다.

  - BOOTSTRAP 내부 IPs: BOOTSTRAP 설치 시 사용할 Warden Bosh Subnet 대역의 아이피

    

### <div id='12'/>2.4.4 *BOOTSTRAP 설치*

BOOTSTRAP 설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -> “BOOTSTRAP 설치” 메뉴로 이동 후 상단에 위치한 “설치” 버튼을 클릭한다.

#### 1. *클라우드 환경 선택*

1.	설치할 클라우드 환경을 선택하는 팝업화면에서 Warden를 선택한다.
2.	“확인” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/bootstrap%ED%8C%9D%EC%97%85%EC%B0%BD(%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C%ED%99%98%EA%B2%BD).png?raw=true)

#### 2. *BOOTSTRAP 설치 – 기본 정보*

1.	아래의 기본 정보 입력 후 “다음” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/bootstrap%ED%8C%9D%EC%97%85%EC%B0%BD(%EA%B8%B0%EB%B3%B8%EC%A0%95%EB%B3%B4%EB%82%B4%EC%97%AD).png?raw=true)

※	BOOTSTRAP 기본 정보 입력 정보

  - 배포 명: 등록할 BOOTSTRAP 배포 명
  - 디렉터 명: 등록할 BOOTSTRAP의 디렉터 명 설치 후 실제 디렉터의 Alias 명칭이 된다.
  - 디렉터 접속 인증서: 디렉터 인증서 관리에서 등록 한 디렉터 인증서 선택
  - NTP: NTP 서버 시간
  - BOSH 릴리즈: 설치할 BOSH 릴리즈를 선택
  - BOSH-CPI 릴리즈: 설치할 BOSH-CPI 릴리즈를 선택
  - BOSH-BPM 릴리즈: 설치할 BPM 릴리즈 선택, 특정 BOSH 버전 이상일 경우 사용
  - OS-CONF 릴리즈: 설치할 OS-CONF 릴리즈를 선택
  - GARDEN-RUNC 릴리즈: 설치할 GARDEN-RUNC 릴리즈 선택
  - BOSH-VIRTUAL-BOX-CPI 릴리즈: 설치할 VIRTUAL-BOX-CPI 선택
  - 스냅샷기능 사용 여부: 스냅샷 사용 여부 (특정 인프라에서만 사용 가능)
  - 스냅샷 스케쥴: 스냅샷 생성 스케쥴
  - PaaS-TA 모니터링 정보: PaaS-TA 모니터링을 이용하려면 BOSH Release v270.2.0를 선택하고 PaaS-TA 모니터링 사용을 선택한다.

**BOOTSTRAP 릴리즈 Name Tag의 “?” 아이콘을 통해 현재 플랫폼 설치 자동화에서 설치 가능한 BOSH의 버전을 확인한다.**

#### 3. *BOOTSTRAP 설치 – 클라우드 환경 별 네트워크 정보*

1.	Warden 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/bootstrap%ED%8C%9D%EC%97%85%EC%B0%BD(%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%A0%95%EB%B3%B4).png?raw=true)

※	BOOTSTRAP 네트워크 등록 정보

  -  Internal디렉터 내부 IP: Warden Internal Subnet 대역의 아이피, 디렉터 인증서 생성 정보와 같아야 한다.
  -  Internal 네트워크 명: Internal 네트워크의 Network 명
  -  Internal Outbound_network 명: Internal 네트워크의 Outbound_network 명
  -  Internal 서브넷 범위: Internal 네트워크의 Subnet의 범위
  -  Internal 게이트웨이: Internal 네트워크의 Subnet의 게이트웨이 주소
  -  Internal DNS: 도메인 네임 서버

#### 4. *BOOTSTRAP 설치 – 리소스 정보*

1.	Warden 리소스 정보 입력 후 “다음” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/bootstrap%ED%8C%9D%EC%97%85%EC%B0%BD(%EB%A6%AC%EC%86%8C%EC%8A%A4%EC%A0%95%EB%B3%B4).png?raw=true)

※	BOOTSTRAP 리소스 등록 정보

  - 스템셀: 스템셀 관리에서 다운로드 한 스템셀 선택

#### 5. *BOOTSTRAP 설치 – 설치*

1.	생성된 배포 Manifest파일 정보를 이용하여 BOOTSTRAP설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다
2.	설치가 완료되면 “닫기” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/bootstrap/bootstrap%ED%8C%9D%EC%97%85%EC%B0%BD(%EC%84%A4%EC%B9%98).png?raw=true)

### <div id='13'/>2.4.5 *디렉터 설정*

BOOTSTRAP설치가 완료되면 BOOTSTRAP 디렉터 정보를 이용해서 플랫폼 설치 자동화의 디렉터로 설정한다.
디렉터를 등록 위해서는 플랫폼 설치 자동화 웹 화면에서 “환경설정 및 관리” -> “디렉터 설정” 메뉴로 이동 후 상단에 위치한 “등록” 버튼을 클릭하고, 디렉터 등록 팝업 화면에서 디렉터 정보 입력 후 “등록” 버튼을 클릭한다.
이미 디렉터가 존재할 경우 디렉터를 선택하고 “기본 디렉터로 설정” 버튼을 클릭한다.

계정 및 비밀번호, 포트번호는 “admin/admin/25555”이다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/%EB%94%94%EB%A0%89%ED%84%B0%EC%84%A4%EC%A0%95.png?raw=true)

※	디렉터 설정 등록 정보

  - 디렉터 IP: BOOTSTRAP 설치 Warden IP 정보를 입력
  - 포트번호: BOOTSTRAP 설치 Manifest의 Director Port 번호 입력(default 25555)
  - 계정: BOOTSTRAP 설치 Manifest의 user_management 아래 Director User 입력
  - 비밀번호: BOOTSTRAP 설치 Manifest의 user_management아래 Director Password 입력

## <div id='14'/>2.5 *CF-Deployment 설치하기*

BOSH를 설치하고 플랫폼 설치 자동화의 디렉터로 설정이 완료되면 CF-Deployment를 설치할 준비가 된 상태로 PaaS-TA를 설치하는 절차는 다음과 같다.



![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf_flow.png?raw=true)

### <div id='15'/>2.5.1 *스템셀 업로드*

플랫폼 설치 자동화에서 다운받은 스템셀을 “스템셀 업로드” 화면을 통해 디렉터에 315.64 버전의 스템셀을 업로드 한다. (먼저 스템셀 관리에서 bosh-warden-boshlite-ubuntu-xenial-go_agent 315.64를 다운로드 한다.)

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/%EC%8A%A4%ED%85%9C%EC%85%80%EC%97%85%EB%A1%9C%EB%93%9C.png?raw=true)

### 2.5.2 *PaaS-TA 릴리즈 사용*

※	해당 절차는 PaaS-TA를 설치하기 위해 반드시 필요한 Compiled Local 릴리즈를 다운로드하는 절차이다. PaaS-TA를 설치하기 위해 필요한 절차이다.
※	플랫폼 설치 자동화를 통해 배포 가능한 PaaS-TA 버전에 맞는 릴리즈와 스템셀을 PaaS-TA 공식 홈페이지 https://paas-ta.kr/download/package에서 다운로드 받는다.

1. PaaS-TA 릴리즈 사용

   1.1.	다운로드 한 paasta 릴리즈 압축 파일을 scp 명령어를 통해 플랫폼 설치 자동화가 동작하고 있는 Inception 서버로 이동시킨다.

   	ex) $ scp -i {inception.key} ubuntu@172.xxx.xxx.xx {릴리즈 압축 일 명} # Key Pair를 사용할 경우
   	ex) $ scp ubuntu@172.xxx.xxx.xx {릴리즈 압축 일 명} # Password를 사용할 경우

   1.2.	릴리즈 디렉토리를 생성하고 릴리즈 디렉토리에서 해당 릴리즈 파일의 압축을 해제한다. 릴리즈 디렉토리의 위치는 반드시 {home}/workspace/paasta-5.0/release/paasta여야 한다.

   	-	디렉토리 생성
   	ex) $ mkdir -p workspace/paasta-5.0/release/paasta
   	-	릴리즈 압축 파일 이동
   	ex) $ mv {릴리즈 압축 파일 명} workspace/paasta-5.0/release/paasta/
   	-	릴리즈 파일 압축 해제
   	ex) $ tar xvf {릴리즈 압축 파일 명} # 릴리즈 파일 확장자가 tar인 경우
   	ex) $ unzip {릴리즈 압축 파일 명} # 릴리즈 파일 확장자가 zip인 경우

   1.3.	아래는 릴리즈 디렉토리의 PaaS-TA 릴리즈 형상 예시 그림이다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/6_%EB%A6%B4%EB%A6%AC%EC%A6%88%EC%97%85%EB%A1%9C%EB%93%9C.jpg?raw=true)

### <div id='16'/>2.5.3 *CF-Deployment 설치*

CF-Deployment를 설치하기 위해 플랫폼 설치 자동화 웹 화면에서 “플랫폼 설치” -> “CF-Deployment설치” 메뉴로 이동 후 상단의 “설치” 버튼을 클릭한다.

#### 1.	*CF-Deployment설치 – 기본 정보 등록*

1.	배포에 필요한 기본정보와 도메인 / 로그인 비밀번호를 입력 후 “다음” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf%ED%8C%9D%EC%97%85%EC%B0%BD(%EA%B8%B0%EB%B3%B8%EC%A0%95%EB%B3%B4).png?raw=true)

**본 가이드에서는 CF-Deployement 버전으로 Paasta 5.0을 사용하였다.**

※	CF-Deployment 기본 등록 정보

  - 설치 관리자 UUID: 기본 디렉터의 UUID (자동 입력)
  - 배포 명: CF-Deployment 설치 배포 명 입력
  - CF-Deployment 버전: 플랫폼 설치 자동화에서 지원하는 CF Deployment 버전 선택
  - CF Database 유형: CF Database 컴포넌트의 유형, Mysql로 설치 시 컴파일 시간이 오래 걸릴 수 있음
  - Inception User Name: Inception 서버의 계정 명 ex) vcap
  - CF Admin Password: CF Login 패스워드 입력
  - 도메인: CF 설치에 사용 할 도메인 입력 ex) {Target IP}.xip.io
  - Portal 도메인: Portal을 설치 및 접속할 도메인 주소를 입력한다. Portal을 설치하지 않고 CF-Deployment를 실행할 경우 해당 값을 입력하지 않는다.
  - PaaS-TA 모니터링 정보: PaaS-TA 모니터링을 이용하려면 paasta/5.0을 선택하고 PaaS-TA 모니터링 사용을 선택한다.


#### 2.	*CF-Deployment설치 – 네트워크 정보 등록*

1.	Warden의 네트워크 정보 입력 후 “다음” 버튼을 클릭한다.
2.	“추가” 버튼을 클릭하여 네트워크를 추가하여 AZ를 분산 배치할 수 있다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf%ED%8C%9D%EC%97%85%EC%B0%BD(%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%A0%95%EB%B3%B4).png?raw=true)

※	CF-Deployment 네트워크 등록 정보

  - CF API TARGET IP: CF Deployment 설치 시 사용되는 Target IP
  - 서브넷 범위: Internal 네트워크의 서브넷 범위
  - 게이트웨이: Internal 네트워크의 게이트웨이 주소
  - DNS: DNS 서버 주소
  - IP 할당 제외 대역: CF Deployment VM을 배치하지 않을 IP 주소 시작/끝 입력
  - IP 할당 대역: CF Deployment VM을 배치할 IP 주소 시작/끝 입력

#### 3.	*CF-Deployment설치 – Key 생성 및 정보 등록*

1.	Key 생성 정보 입력 후 “Key 생성” 버튼을 클릭한다.
2.	Key 생성 확인 후 “다음” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf%ED%8C%9D%EC%97%85%EC%B0%BD(key%EC%83%9D%EC%84%B1).png?raw=true)

※	CF-Deployment Key 생성 등록 정보

-	도메인: CF Deployment 도메인 주소 (자동 입력)
-	국가 코드: 국가 코드 선택
-	시/도: 시/도 입력
-	시/구/군: 시/구/군 입력
-	회사명: 회사명 입력
-	부서명: 부서 명 입력
-	Email: 이메일 주소 입력

#### 4.	*CF-Deployment설치 – 리소스 정보 등록*

1.	CF-Deployment 설치에 필요한 리소스 정보를 입력 후 “다음” 버튼을 클릭한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf%ED%8C%9D%EC%97%85%EC%B0%BD(%EB%A6%AC%EC%86%8C%EC%8A%A4%EC%A0%95%EB%B3%B4).png?raw=true)

※	CF-Deployment 설치 리소스 생성 등록 정보

  - Stemcell: 기본 디렉터에 업로드 한 스템셀 선택
  - Small Resource Type: Warden 환경의 Small Instance Type
  - Medium Resource Type: Warden 환경의 Medium Instance Type
  - Large Resource Type: Warden 환경의 Large Instance Type

#### 5.	*CF-Deployment설치 – 인스턴스 정보 등록*

1.	Warden 환경일 경우 아래의 정보를 입력 후 “다음” 버튼을 클릭한다.
2.	인스턴스 수가 늘어나게 되면 해당 수만큼 네트워크 대역이 필요해 네트워크 할당 대역을 늘려줄 필요가 있다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf%ED%8C%9D%EC%97%85%EC%B0%BD(%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%EC%A0%95%EB%B3%B4).png?raw=true)

※	CF-Deployment Key 생성 등록 정보

-	인스턴스 수: VM에 할당할 인스턴스 수

#### 6.	*CF-Deployment설치 – 설치*

1.	생성된 배포 Manifest파일 정보를 이용하여 CF-Deployment설치를 실행하고 설치 진행 과정에 대한 로그를 확인한다.

![](https://github.com/okpc579/OPENPAAS-IEDA-WEB/blob/bosh/use-guide/platform/images/install-guide/warden/cf/cf%ED%8C%9D%EC%97%85%EC%B0%BD(%EC%84%A4%EC%B9%98).png?raw=true)
