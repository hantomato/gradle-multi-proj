# gradle로 멀티 프로젝트 구성하기

gradle로 멀티/서브 프로젝트를 구성하는 방법은 순서대로 정리하였다.

Intellij를 사용하여 웹프로젝트를 만드는 샘플이다.

#### 만들고자 하는 프로젝트 구성
- [root] gradle-multi-proj
  - [sub] gradle-multi-proj-core
  - [sub] gradle-multi-proj-front

#### 1.Root 프로젝트 만들기
intellij의 메뉴 [File > New > Project...] 을 실행해서 프로젝트 생성창을 띄운 후,
왼쪽 리스트박스의 'Gradle' 선택 후, 우측 Additional Libraries and Frameworks 에서는 'Java'를 선택한다.

다음 화면에서 아래 내용 입력한다.
- GroupId : com.nmj.demo
- ArtifactId : gradle-multi-proj

다음 화면에서 아래 2개 체크 해제되있는것을 체크한다.
- Use auto-import
- Create directories for empty content roots automatically

생성된 root 프로젝트의 build.gradle 에서 아래 두줄 빼고 모두 삭제한다.
- group 'com.nmj.demo'
- version '1.0-SNAPSHOT'

만약 프로젝트 폴더에 src 폴더가 생성되있다면 이 폴더도 삭제한다.

#### 2.Sub 프로젝트 만들기
intellij의 메뉴 [File > New > Module...] 을 실행해서 프로젝트 생성창을 띄운 후,
왼쪽 리스트박스의 'Spring Initializr' 선택해서 일반적인 Spring boot 프로젝트 생성하듯이 프로젝트를 생성한다.

gradle-multi-proj-core, gradle-multi-proj-front 이렇게 두 개의 프로젝트를 생성하면, 프로젝트의 폴더 구조는 다음과 같을 것이다.

- gradle-multi-proj
  - gradle-multi-proj-core
  - gradle-multi-proj-core

아직까지는 폴더 구조만 루트, 서브 프로젝트 처럼 보일 뿐이지 아직 서로간에 종속 관계가 없다.

#### 3. 프로젝트 관계 정하기
루트 프로젝트의 settings.gradle 파일을 열면 아래 한줄만 있을텐데

rootProject.name = 'gradle-multi-proj'

여기에 아래 줄을 추가한다.

include 'gradle-multi-proj-front',
        'gradle-multi-proj-core'

#### 4. 각 build.gradle 파일들 수정
이제 sub 프로젝트의 설정파일은 build.gradle을 root 프로젝트로 통합하려고 한다.
일단 root 프로젝트의 build.gradle에 group, version 을 아래처럼 allprojects로 감싼다

allprojects {
    group 'com.nmj.demo'
    version '1.0-SNAPSHOT'
}

sub 프로젝트의 build.gradle 파일의 buildscript {} 로 되있는 코드를 잘라내기해서 이걸 root 프로젝트의 build.gradle 로 붙인다.

sub 프로젝트의 build.gradle 파일의 나머지 코드를 잘라내기해서 root 프로젝트에 붙인 후, subprojects {} 로 감싼다.

여기까지 했다면
sub 프로젝트들의 build.gradle 파일 내용은 비어있을테고,
root 프로젝트의 build.gradle에는 아래 3개 항목이 있을 것이다.

- buildscript
- allprojects
- subprojects

이제 각 sub 프로젝트들의 build.gradle에 아래 코드 추가해두자

dependencies {

}

#### 기타
root 프로젝트의 subprojects 의 dependencies 에는 모든 서브 프로젝트 공통 dependencies를 넣고
특정 sub 프로젝트에서만 사용하는 dependency 는 해당 build.gradle의 dependencies에 넣으면 된다.
