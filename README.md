# Starbucks Korea 💚 🧜‍♀️ 🇰🇷

## ✍️ 기획

- Starbucks Korea 홈페이지를 clone하여 기본적인 HTML5/CSS3을 익히고자 함
- 사용 목적에 따른 적절한 js 라이브러리를 가져와 사용하고자 함
- 참고 링크 : [☕️ Starbucks Korea](https://www.starbucks.co.kr/index.do)

## 🔗 완성 링크 (2023.07 기준 제작)

[☕️ my-Starbucks-Korea](https://my-starbucks-korea.netlify.app)

## 👩‍💻 구현 기능

- 헤더 메뉴 `hover`시 드롭다운 메뉴 보이기
  
  ![드롭다운](https://github.com/noSPkeepgoing/starbucks/assets/125979833/d200d639-e0b3-4521-acac-cf5af0b348a6)
  
- 로딩 시 메인 `top banner`에 순차적으로 요소 보이기
 
  ![순차적으로요소보이기](https://github.com/noSPkeepgoing/starbucks/assets/125979833/fed72266-0f4f-4d3a-b0ca-e980de006a4c)
  
- `swiper.js`를 이용한 요소 슬라이드
 
  - 공지사항 슬라이드
    
    ![공지사항슬라이드](https://github.com/noSPkeepgoing/starbucks/assets/125979833/b1764295-9155-4477-8d83-e0a820bc86bd)
    
  - 프로모션 슬라이드
    
    ![프로모션슬라이드](https://github.com/noSPkeepgoing/starbucks/assets/125979833/af26be67-5a4b-4c2e-a0e8-2f15b5aa0a72)
    
- `scrollMagic`을 이용한 스크롤 위치 계산 애니메이션
  
  ![스크롤위치](https://github.com/noSPkeepgoing/starbucks/assets/125979833/d6bb8999-b65c-47a8-9d3f-23d8100dfe9a)
  

## 🍝 구현코드

- 요소를 순차적으로 보이는 애니메이션
  
  - `index.html`
    
    ```html
    ...
    <!-- gsap cdn 삽입 -->
    <script
        src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"
        integrity="sha512-16esztaSRplJROstbIIdwX3N97V1+pZvV33ABoG1H2OyTttBxEGkTsoIVsiP1iaTtM8b3+hu2kB6pQ4Clr5yug=="
        crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    </head>

    ...

    <section class="visual">
          <div class="inner">
            <div class="title">
              <img src="./images/visual_title.png" alt="TURN UP YOUR SUMMER" class="fade-in"/>
              <a href="javascript:void(0)" class="btn fade-in">자세히 보기</a>
              </div>
              <img src="./images/visual_drink1.png" alt="Sea Salt Caramel Cold Brew" class="cup1 image fade-in"/>
              <img src="./images/visual_drink2.png" alt="Raspberry Pop Saken Tea" class="cup2 image fade-in"/>
              <img src="./images/visual_drink3.png" alt="Mandarine Verbena Starbucks Fizzio" class="cup3 image fade-in"/>
            </div>
        </section>
    ```
    
  - `main.js`
    
    ```jsx
    const fadeEls = document.querySelectorAll('.visual .fade-in');

    fadeEls.forEach(function (fadeEl, index) {
      // gsap.to(요소, 지속시간, 옵션)
      gsap.to(fadeEl, 1, {
        delay: (index + 1) * 0.6, // 각 요소마다 딜레이 시간을 다르게 주어 순차적으로 보이게
        opacity: 1,
      });
    });
    ```
    
- 요소 슬라이드 애니메이션
  
  - `main.js`
    
    ```jsx
    // 공지사항 슬라이드
    new Swiper('.notice-line .swiper', {
      direction: 'vertical', // 수직방향 슬라이드
      autoplay: true,
      loop: true,
    });

    // 프로모션 슬라이드
    new Swiper('.promotion .swiper', {
      slidesPerView: 3, // 한번에 세개씩만 보여주기
      spaceBetween: 10, // 요소 사이 공간
      centeredSlides: true,
      loop: true,
      autoplay: {
        delay: 5000, // 슬라이드 전환 속도
      },
      pagination: {
        // 페이지네이션 설정
        el: '.promotion .swiper-pagination',
        clickable: true,
      },
      navigation: {
        // 네비게이션 버튼 설정
        prevEl: '.promotion .swiper-prev',
        nextEl: '.promotion .swiper-next',
      },
    });
    ```
    
- 스크롤 위치 계산 애니메이션
  
  - `main.js`
    
    ```jsx
    const spyEls = document.querySelectorAll('section.scroll-spy');
    spyEls.forEach(function (spyEl) {
      new ScrollMagic.Scene({
        triggerElement: spyEl,
        triggerHook: 0.8,
      }) //
        .setClassToggle(spyEl, 'show') //
        .addTo(new ScrollMagic.Controller());
    });
    ```
    
  - `main.css`
    
    ```jsx
    /* 요소를 숨기고 있다가 변화가 생기면 1.2s의 시간을 거쳐 변화시킴 */
    .back-to-position {
      opacity: 0;
      transition: 1.2s;
    }

    /* 요소를 기존 위치보다 왼쪽에 배치 */
    .back-to-position.to-right {
      transform: translateX(-200px);
    }

    /* 요소를 기존 위치보다 오른쪽에 배치 */
    .back-to-position.to-left {
      transform: translateX(200px);
    }

    /* 요소를 보이면서 원래 위치로 이동 */
    .show .back-to-position {
      opacity: 1;
      transform: translate(0);
    }

    /* 딜레이 시간을 다르게 하여 좀 더 역동적으로 보이게 */
    .show .back-to-position.delay-0 {
      transition-delay: 0s;
    }

    .show .back-to-position.delay-1 {
      transition-delay: 0.4s;
    }

    .show .back-to-position.delay-2 {
      transition-delay: 0.7s;
    }

    .show .back-to-position.delay-3 {
      transition-delay: 1s;
    }
    ```

## 👏 아쉬운 점

- 파일 분할을 하지않고 만들다 보니 유지 보수를 하기엔 힘들 것 같다.
- 하드코딩을 하다보니 지나치게 비대해진 `html`파일
- 반응형을 전혀 고려하지 않은 코드
