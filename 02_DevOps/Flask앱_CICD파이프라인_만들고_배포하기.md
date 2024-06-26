# 02. DevOps 세션 : Flask앱 CI/CD 파이프라인 만들고 배포하기
### 이 세션은 Flask 앱의 CI/CD 파이프라인을 만들어 보는 세션입니다.    
<br>
<br>

## 1. **DevOps**가 무엇인지 아시나요?  
**DevOps**는  **Development(개발)** 과 **Operation(운영)** 의 약자입니다.
 
![image](https://github.com/KNU-MLSA/2024_3_Event/assets/114579651/052fc717-826e-4953-8e98-188d2d88c446)


#### 만약 웹 서비스를 출시하고자 한다면, 보통 아래와 같은 과정을 거칩니다.   

  
먼저 웹 사이트를 개인 PC에서 **개발**하고 난 후에, 클라우드 환경에서 웹을 **배포**합니다.  
  
앞서 배포한 웹 사이트에 많은 사람들이 동시 접속하는 경우, 해당 웹 사이트가 다운되지 않도록 지속적으로  
웹 사이트를 *모니터링*을 하거나 이외의 관리를 해줘야 합니다.  
  
그 후엔, 이렇게 배포된 사이트의 서비스가 멈추지 않고 잘 돌아가게 하는 **운영**을 해줘야 합니다.  

### 기능 추가
만약 이 웹사이트에 새로운 기능이나 내용들을 추가하려면,  
특정 기능에 관해 코드를 새로 **개발**하고 다시 클라우드 환경에 **배포**해야 합니다.
  
배포와,   
배포 후 기존의 웹사이트에 새로 **개발**한 변경된 사항이 잘 적용됐는지 확인하는 **운영** 과정을 또 거쳐야 합니다.  

  
또, 이렇게 새로운 기능을 추가하려면 해당 기능을 **개발** 후 **운영**을 해야겠죠?



이처럼 웹이나 앱을 서비하려면 **개발**과 **운영** 과정이 필요합니다.
  
**개발**과 **운영** 과정이 잘 진행될 수 있게 하고자 하는 방법론이 **Devops**입니다.
  

 ## 2. CI/CD?

**DevOps** 개념이 조금 추상적이게 느껴질 수 있을 것 같네요.  
**DevOps**, **개발**과 **운영** 과정이 원활하게 진행될 수 있도록 하는 중요한 방법이 **CI/CD**입니다.  
  
**CI/CD** 개념을 통해 **DevOps**에 대해 더 자세히 이해하실 수 있습니다.  

![image](https://github.com/KNU-MLSA/2024_3_Event/assets/114579651/84961af8-29d2-4c8d-a0a5-db93f7882f0d)
  
**CI/CD**는 *Continuous Integration(계속 합치기)/Continuous Deploy(계속 배포하기)* 의 약자입니다.  
  
> ### 기존 서비스에 새로운 기능을 추가하는 상황
>만약 이 웹사이트에 새로운 기능이나 내용들을 추가해야 된다면,  
>특정 기능에 관해 코드를 새로 **개발**하고 다시 클라우드 환경에 배포해야 합니다.
>  
>배포와,   
>배포 후 기존의 웹사이트에 새로 **개발**한 변경된 사항이 잘 적용됐는지 확인하는 **운영** 과정을 또 거쳐야 합니다.  
  
새로 **개발**한 코드를 기존 코드(기존 개발해 뒀던 내용)에 통합하고,  
클라우드 환경에 **배포**까지 하는 것을 "계속 합치기/계속 배포",   
**CI/CD**라고 합니다.  

**"계속"** 합치기 또는 배포이다 보니, 뭔가 자동적으로 되는 게 있는 느낌입니다.  

이 **CI/CD**는 코드에 **수정/변동 사항**이 생기면 그 수정이 기존 코드에 **통합**되고, 클라우드에 **배포**되는게
자동으로 된다는 것입니다.  

코드 변경 사항이 생겼을 때, 알아서 기존 코드랑 합치고 배포한다는 것이죠.

알아서 이렇게 기존 코드에 합쳐지고 배포되면, 정말 서비스를 운영하기 편할 것입니다.  

<br>  
<br>  
  
따라서, 서비스를 운영하기 위해선 **개발**과 **운영**이 필요하고,  
**개발**과 **운영**을 "편하게 해보자!" 라는 방법론이 **DevOps**입니다.  

*어떻게 편하게 할건데?* **개발**과 **운영**을 **CI/CD**를 통해(코드 수정 사항 생기면 알아서 기존 코드에 합치고 배포까지 하게 해서)  
편하게 할 수 있습니다.

  <br>

## 3. 실습하기

간단한 웹 앱을 **CI/CD**를 수행할 수 있는 파이프라인으로 배포해보겠습니다.  
**CI/CD** 할 수 있게 하는 도구로 **GitHub Actions**을 사용할 것입니다.  

<br>  

**Azure**를 사용해 본 적이 없다면
> 👉 [Azure for Students 가입 방법](https://github.com/KNU-MLSA/2023_10_Sessions/blob/main/1_AI%EB%A1%9C%EC%97%B0%EC%95%A0%ED%99%95%EB%A5%A0%EC%98%88%EC%B8%A1%ED%95%98%EA%B8%B0/Azure%20for%20Students%20%EA%B0%80%EC%9E%85%20%EB%B0%A9%EB%B2%95.pdf)  

위 링크를 따라 Azure에 가입 후, Azure for Students로 클라우드 사용권(크레딧)을 받아 주세요.  
  
그 다음  
  
![image](https://github.com/KNU-MLSA/2024_3_Event/assets/114579651/f897d53e-c9a1-4e9e-920f-5b131115eaf5)

실습 가이드 문서(DevOps.pdf), app.py, requirements.txt를 다 다운받아 주세요.  
    
> ![image](https://github.com/KNU-MLSA/2024_3_Event/assets/114579651/816bea05-2b4b-45b1-8e0d-fdd83ece8b3b)  
> 파일을 클릭하면, 위와 같이 다운 버튼이 나옵니다. 세 파일을 모두 각각 이 다운 버튼을 눌러 다운 받아 주세요.

<br>
<br>
  
본인 GitHub 계정에서 아무 리포지토리를 생성 후, 
실습 가이드 문서(DevOps.pdf) 6p 부터 따라 해주시면 됩니다.
> 생성한 리포지토리에 app.py와 requirements.txt를 올리면 됩니다.

<br>
<br>

만약, 동영상으로 실습 방법을 배우고 싶다면 아래 유투브 영상을 참고해 주세요.   
[Flask앱 CI/CD 파이프라인으로 배포하기](https://youtu.be/huNRWtL-GF8?si=pwFd_zzlKYJ3l9Kx)
