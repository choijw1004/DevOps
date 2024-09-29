# Jenkins
## Java 11에서 Java 17로 변경 후 GitLab 연동 트러블슈팅

### GitLab API 토큰 문제
에러 메시지:
```
Client error: HTTP 401 Unauthorized
```
#### 원인: Java 17로 업데이트 후 GitLab과 Jenkins 간의 인증 문제가 발생했습니다. 주로 GitLab API 토큰이 잘못되었거나 권한 부족으로 인해 인증이 실패한 경우입니다.

#### 해결 방법:

GitLab에서 새로운 API 토큰을 생성하고, api, read_repository, write_repository 권한을 부여합니다.
Jenkins에서 해당 토큰을 다시 설정합니다.
GitLab API 토큰 생성 방법:

GitLab에서 **"User Settings" > "Access Tokens"**로 이동하여 새 토큰을 생성하고, Jenkins의 GitLab Plugin 설정에 추가합니다.
#### GitLab Connection 설정
문제: GitLab API 토큰을 추가했음에도 Jenkins에서 GitLab에 연결되지 않는 문제가 발생했습니다.

#### 해결 방법:

Jenkins에서 GitLab API 연결 설정을 정확하게 구성합니다.
GitLab host URL: GitLab의 호스트 URL을 올바르게 입력 (예: https://gitlab.com)

Credentials: 생성한 GitLab API 토큰을 선택

설정 예시:
```
Connection Name: GitLab Connection
GitLab Host URL: https://gitlab.com
Credentials: GitLab API Token
```
#### Webhook 설정

에러 메시지:
```
Push events branch filter is not a valid branch name
```
#### 원인: GitLab에서 Webhook 설정 중 브랜치 필터에 잘못된 브랜치 이름이 입력된 경우 발생합니다. 공백이 포함되었거나 잘못된 브랜치명이 입력되었을 때 발생할 수 있습니다.

#### 해결 방법:

Webhook 설정 페이지에서 정확한 브랜치 이름을 입력합니다. 예를 들어 develop 브랜치라면, 필터에 develop을 입력해야 합니다.
Webhook 설정 단계:

GitLab 프로젝트의 **"Settings" > "Webhooks"**로 이동
Jenkins의 Webhook URL을 추가하고, Push events를 선택
Branch filter에 정확한 브랜치 이름 입력 (예: develop)
### Jenkins에서 GitLab Webhook 설정
문제: GitLab에서 Webhook 설정 후에도 Jenkins가 빌드를 트리거하지 않는 문제가 발생했습니다.

#### 해결 방법:

Jenkins에서 GitLab Plugin과 GitLab Webhook 플러그인이 제대로 설치되었는지 확인합니다.
Jenkins 프로젝트의 "Build Triggers" 섹션에서 GitLab webhook trigger 옵션을 활성화합니다.
GitLab에서 Webhook이 제대로 설정되었는지 확인하고, 올바른 Jenkins URL을 입력합니다.
Jenkins 설정:

**"Configure" > "Build Triggers"**로 이동
"GitLab webhook trigger" 활성화





