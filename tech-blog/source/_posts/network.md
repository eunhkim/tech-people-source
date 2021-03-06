---
title: Network Engine 1부
date: 2019-10-12 16:15:30
thumbnail : /images/network.jpg
tags: [Network,IoT,RTLS,LBS,VertX,Node.js]
category : [IT Tech, 1. IoT]
---

### IoT 에서의 네트워크 엔진
> 작성자 : 플랫폼 개발실 실장 김완철(David Kim)
#### 1. 개요

- 인터넷이 보편화된 시점부터 지금까지 데이터 처리에 대한 관심과 이슈는 언제나 있어 왔다. 인터넷의 속도가 느렸던 과거에는 데이터량와 처리속도에 대한 이슈가 그다지 많지는 않았지만, 에러없이 데이터 처리를 하고자 하는 생각은 속도와 데이터량에 상관없이 주요 관심사였다. 5G로 가고 있는 현시점도 마찬가지로 속도와 데이터량에 대한 관심은 실시간으로 데이터를 처리하는 솔루션 특히 서비스파트에서 많이 관심을 보이고 있다.

##### 1.1. 데이터 수집

- 데이터 수집이란 말은 쉽게 생각하면, 그냥 저절로 들어오는 데이터를 모으는 행위 또는 특정한 장소에 저장되어 있는 데이터를 가져와서 저장하는 개념이 포함되어 있다. 데이터 수집은 초기 인터넷 서비스부터 회원가입이라는 개념이 있는 그 시점부터 당연스럽게 생겼던 개념이다. 초고속 인터넷망이 보편화된 현재에도 데이터 수집은 지속적으로 이루어져 왔고, 특히 빠르고, 네트워크망에 부담없이 데이터를 수집할지에 대한 고민은 여전히 숙제로 남아 있다.

##### 1.2. 데이터 가공

- 데이터를 수집하고, 특정 위치에 저장하고, 해당 데이터를 읽는 행위는 데이터를 수집하는 솔루션 특히 서비스업체에서는 당연한 업무이며, 비즈니스의 한 부분이라는것에는 이견이 없다. 하지만, 가공이라는 역할은 그리 쉽지 않는 업무프로세스이다. 특히 쓰레기 데이터가 들어오면 결과물도 쓰레기라는 얘기가 있듯이, 가공하는 그 시점에 어떤 데이터를 선별하고 어떤 데이터를 이쁘게 가공할지에 대한 고민은 한순간의 문제로 끝나지 않는다.

##### 1.3. 데이터 분석
- 데이터가 가공이 되면, 그 다음 프로세스로 가공된 데이터를 필요한 요건에 맞게 분석하는 일이다. 데이터 분석이 반드시 인사이트(insight) 있는 리포트를 생산하는 업무라기 보다는, 리포트를 만들수 있도록 그 전단계까지 데이터 자체를 분석하는것도 이 범위에 속한다고 생각한다. 업무 범위나 프로세스에는 충분히 이견이 있을수 있다고 생각한다. 분석단계가 데이터 처리의 마지막이라고 생각하는 경우가 보편적이기 때문이다. 하지만 내가 생각하는 분석과 인사이트(insight)는 특정한 업무범위나 분석능력 그리고 특정 도메인에 정통한 사람이 있느냐 없느냐에 따른 분석과 인사이트(insight) 업무가 분리된다고 생각한다. 

##### 1.4. 데이터 인사이트(insight)
- 가공 및 분석된 데이터를 특정 도메인에 정통한 멤버가 우리가 보지 못한, 또는 우리가 분석하지 못한 내용을 도출하는 액션 또는 업무프로세스가 데이터 인사이트라고 생각한다. 데이터는 누구나 수집하고 가공하고 분석할수는 있으나 그것을 가지고, 미래에 대한 예측, 예견 또는 현재의 문제점을 파악하고 도출해내는 행위야 말로 데이터 인사이트라는 용어를 붙일수 있다고 생각한다. 이 프로세스 역시 여러가지 이견이 있을수 있지만, 본인이 생각하는 것이 분석과 인사이트는 엄연히 다르다고 생각한다.

#### 2. IoT 에서의 데이터 수집

##### 2.1. 디바이스 
- IoT 란 Internet of Things 의 약자로 말 그대로 어떠한 물건(에셋)에 인터넷이 결합된 것을 의미한다. 최근 4차 산업이라고 불리우는 것들중 IoT 는 빠지지 않고 단골손님처럼 회자된다. 물건하나 하나를 인터넷 망이라는 또는 무선망으로 연결한다며, 그 연결 과정에서 나오는 데이터 특히 IoT 제품(상품) 이라고 보편적으로 불리고 있는 인터넷 연결이 가능한 세탁기나 공기청정기 같은 전자제품에서 특정 서버로 데이터를 전송하게 된다면, 엄청난 데이터가 수집이 가능해지게 된다. 하지만, 인터넷 연결이 불가능한 디바이스도 존재하기 때문에, 데이터 전송을 위한 보완책이 필수적으로 마련되어야 한다. 

 
##### 2.2. 디바이스 서버 게이트웨이(브릿지)
- 인터넷망으로 연결된 IoT 제품의 경우는 서버로 특정한 데이터를 전송할수 있는 기본적인 구성은 되어 있다. 하지만, 인터넷이 가능하도록 부품을 심기 힘든 디바이스 특히 비컨이나 센서와 같은 작은 사이즈의 디바이스는 데이터를 서버로 보낼수 있는 방법이 쉽지가 않다. 이런한 문제를 해결하기 위해 브릿지 또는 게이트웨이라는 디바이스를 가지고, 작은 사이즈의 센서나 비컨들의 데이터를 서버로 전송하게 된다. 참고 :  [게이트웨이(브릿지)](http://pntbiz.co.kr/index.php/home/product/indoorplus-rtls-hw/)

##### 2.3 데이터 수집 서버(클라우드)
- 원하는 데이터를 수집하기 위해서는 어딘가에는 저장을 해야 한다. 대부분은 데이터베이스라는 전통적인 RDB를 이용하게 된다. 인터넷이 발달하기 시작한 시점의 인터넷 서비스 또는 솔루션에서는 RDB(오라클, MSSQL, MYSQL)는 매우 유용한 없어서는 안될 신과 같은 존재였다. 요즘도 마찬가지로 반드시 있어야 하는 애플리케이션이다. 하지만, 최근들어 인터넷 속도가 빨라지고, IoT 라는 디바이스에서 수집되는 데이터 그리고, 인터넷 역사에 길어짐에 따라 엄청나게 축된 데이터들 처리하기 위해서는 전통적인 아키텍처 또는 전통적인 애플리케이션으로는 해결하기 쉽지가 않다. 한가지 예로, 어느순간 예상치 못하게 수집되는 데이터량이 폭발적으로 늘어나거나, 데이터 처리를 위한 프로세스가 길어짐에 따라 컴퓨팅 리소스가 부족하게 되는 기존에 예상하지도 않았던 것들이 나타나고 있다


- 이와 같은 경우는 전통적인 방식 또는 아키텍처 또는 생각으로는 해결할 수가 없다. NoSQL 또는 유연하게 확장이 가능한 아키텍처 구성 또는 데이터를 수집하고 처리할수 있는 애플리케이션(네트워크 엔진)의 성능이 엄청난 데이터를 흘려다니는 현대에는 반드시 필요한 기본 필수 요건이 되어 가고 있다. 


##### 다음글 : [2부 보기](/2019/10/12/network2/)

> 본 내용은 작성자 개인적인 의견입니다. 다른 의견이 있으시면, 피드백 환영합니다.