---
layout : post
title : 구글 드라이브를 이용해 jekyll chirpy에 이미지 업로드
date : 2023-03-14
categories : [development, blog]
tags : [jekyll, chirpy, github pages, google drive, 구글 드라이브, 클라우드, 이미지, 호스팅]
---

## **GitHub Pages의 저장소 용량 제한**
---  
현재 이 깃허브 블로그는 Github Pages로 작성하면서, 깃허브 블로그의 총 용량이 `1GB`를 넘지 못하고, 트래픽은 `한달에 100GB`로 제한되어 있다는 것을 알게 되었다.   

![용량제한](https://drive.google.com/uc?export=view&id=1-SVVkdSS6rs9cEj8fcmFP4lBdHWqqv2m)
출처: [GitHub Pages 정보 - GitHub Docs](https://docs.github.com/ko/pages/getting-started-with-github-pages/about-github-pages)  

이렇게 되면 불로그에 2MB짜리 이미지를 대략 500장 올리기만 해도 용량이 다 찬다는 이야기다!   
그냥 포스팅으로 텍스트만 적는다면 상관없겠지만 나는 이미지를 넣고 싶었기에 내 깃허브 블로그 리파지토리에 이미지를 직접 넣어 불러오는 방법 말고 다른 방법이 무엇이 있는지 알아보다가 클라우드에 이미지를 업로드 한 다음에 파일을 공유하고, 그 링크를 사용해 이미지를 올리는 것이다.  


## **클라우드 선택**
---  
선택지로는 `원 드라이브`와 `구글 드라이브`가 있었는데 깃허브와 관련된 파일들은 하나의 구글 계정에 연관지어 두고 싶어서 구글 드라이브를 선택했다.  

> `원 드라이브`는 `5GB`를 기본 용량으로 제공하고, `구글 드라이브`는 `15GB`를 기본 용량으로 제공한다.   
> 여러 비교글을 보니 업로드, 다운로드 속도는 `원 드라이브`가 `구글 드라이브`보다 빠른 모양이다.  
> 본인의 상황에 맞게 결정하도록 하자.


## **구글 드라이브로 이미지 호스팅**
--- 
방법은 정말 간단하다!  
두 가지 방법이 있는데, 데스크톱용 드라이브를 사용하거나, 웹페이지를 이용하면 된다.  


### **데스크톱용 드라이브로 호스팅**

![데스크톱용 드라이브](https://drive.google.com/uc?export=view&id=1-fX-r12pTfJm4ltGjsHwwgofJLMvc8cy)

올리고 싶은 이미지를 우클릭 한 다음에 `Google Drive로 공유`를 클릭해준다.  

### **웹페이지로 호스팅** 

![웹페이지](https://drive.google.com/uc?export=view&id=1-hMG74SJvX1Zq97Z9a9uwY2iioWxnr4j)

### **링크 생성**

![주의](https://drive.google.com/uc?export=view&id=1-dhPT8p1c89YPmk1I_xtvmK5VfB_lrxN)

두 방법 모두 이 화면을 마주하게 될 텐데 반드시 `일반 엑세스`를 `링크가 있는 모든 사용자`로 변경해줘야 한다. 제한됨으로 놔두면 이미지가 정상적으로 보일리가 없을 것이다...  
액세스를 변경했다면 링크 복사를 클릭하면 된다.  

![호스팅사진](https://drive.google.com/file/d/1-dIFvBLumCeSCDlQzOwYvxPNM8jglEz9/view?usp=sharing)  

근데 복사한 링크를 그대로 올리면 우리가 원하는 모습이 나오질 않는다.  

![호스팅사진2](https://drive.google.com/uc?export=view&id=1-okp8sGn84yIiQdWt-F0nTu01aR2nETJ)  

우리는 이미지만 보이고 싶은거지 클라우드에 있는 파일을 공유하고 싶진 않다...  
링크를 한번 손 볼 필요가 있다.  


>복사된 링크를 살펴보면 아래와 같다. 이곳에서 필요한 것은 주황색 부분이다.   
> - https://drive.google.com/file/d/`1-dIFvBLumCeSCDlQzOwYvxPNM8jglEz9`/view?usp=sharing  


주황색 부분을 복사한 다음에...  
> https://drive.google.com/uc?export=view&id= `이곳에 붙여넣기`
>
> 위 이미지를 이 링크처럼 만들면...  
> https://drive.google.com/uc?export=view&id=`1-dIFvBLumCeSCDlQzOwYvxPNM8jglEz9`
  

## **완성!**

![완성이미지](https://drive.google.com/uc?export=view&id=1-dIFvBLumCeSCDlQzOwYvxPNM8jglEz9)

이제 정상적인 이미지가 나온다!  
우리 모두 이 방법을 이용해서 이미지를 마구마구 올리도록 하자!