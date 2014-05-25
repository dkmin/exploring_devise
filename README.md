Devise 젬 파헤치기

---

![](https://camo.githubusercontent.com/b1c21cc10f2f94857dea5135fe55f2e4d451e028/68747470733a2f2f7261772e6769746875622e636f6d2f706c617461666f726d617465632f6465766973652f6d61737465722f6465766973652e706e67)

---


레일스를 이용한 웹어프릴케이션 개발시에 회원가입 또는 인증 및 권한설정을 구현하는데 도움이 되는 여러 젬들이 있지만, 이 중에서도 `Devise` 젬을 사용하는 것은 필수적이다.

사실 [루비툴박스](https://www.ruby-toolbox.com/categories/rails_authentication) 서비스를 검색해 보아도 직관적으로 알 수 있다.

![](http://i1373.photobucket.com/albums/ag392/rorlab/Photobucket%20Desktop%20-%20RORLAB/exploring_devise/2014-05-25_21-49-56_zps4843eb84.png)

`Devise` 젬은 레일스 초보자들에게는 사용하기가 까다로와 제대로 코딩에 적용하는 것이 어렵게만 느껴진다. 그래서 이 글에서는 `Devise` 젬을 좀 더 깊이 있게 알아 봄으로써 회원 인증을 보다 쉽게 구현할 수 있도록 할 것이다.

[`여기`](https://github.com/plataformatec/devise)를 클릭하면 Github 저장소로 이동할 수 있다.

> **Info** 이 책에서는 독자들의 이해를 보다 효과적으로 돕기 위해  간단한 샘플 프로젝트를 작성할 것이다. [`여기`](https://github.com/luciuschoi/sample_project_for_devise)를   클릭하면 샘플 프로젝트 저장소로 이동할 수 있다.
