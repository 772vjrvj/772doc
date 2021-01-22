https://admin-dev-mps.s3.ap-northeast-2.amazonaws.com/img_mrDT_bg_04.png







# JavaScript용 AWS SDK

가이드 : https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/s3-browser-examples.html











AWSAccessKeyId=AKIAJPVCTHA4UN7ZFV4A  

AWSSecretKey=yJzDoR5kpD7UsRe8KS026b2rod3T3nKqPZvE1Tyb









# node.js용 AWS SDK

가이드 : https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/s3-node-examples.html

소스 : https://aws.amazon.com/ko/sdk-for-node-js/

사용법 : https://opentutorials.org/course/2717/11797

























# java용 AWS SDK

가이드 : https://docs.aws.amazon.com/ko_kr/sdk-for-java/v1/developer-guide/examples-s3.html

개발자 안내서 : https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/dev/Welcome.html

가이드, Maven 다운로드, 예시: https://aws.amazon.com/ko/sdk-for-java/



## SDK 가이드

https://docs.aws.amazon.com/ko_kr/sdk-for-java/v1/developer-guide/aws-sdk-java-dg.pdf

(참고 : https://galid1.tistory.com/590)



Credentials(IAM)

Cost(비용)에 대한 고려



### IAM 사용자 생성

ㄴAccount(계정) 식별

ㄴ권한부여

 ㄴ 사용자 생성시 액세스 키 ID, 비밀 액세스 키 발급. .csv 파일 다운로드



==========================================================

### Dependency(maven, gradle 설정)

ㄴ BOM을 이용한 개별 의존성 지정 방법과, 각 AWS 서비스별 별도 SDK를 사용하는 방법

com.amazonaws

aws-java-sdk-bom

1.11.327



com.amasonaws

aws-java-sdk-s3



ㄴ전체 SDK를 추가하는 방법

com.amazonaws

aws-java-sdk

1.11.327





4.6 이전 Gradle 버전의 프로젝트 설정

\1. 종속성 관리 플러그인을 build.gradle 파일에 추가합니다.
buildscript {
repositories {
mavenCentral()
}
dependencies {
classpath "io.spring.gradle:dependency-management-plugin:1.0.9.RELEASE"
}
}
apply plugin: "io.spring.dependency-management"
\2. 파일의 dependencyManagement 섹션에 BOM을 추가합니다.
dependencyManagement {
imports {
mavenBom 'com.amazonaws:aws-java-sdk-bom:1.11.X'
}
}
\3. 종속성 섹션에 사용할 SDK 모듈을 지정합니다. 예를 들어 다음은 Amazon S3에 대한 종속성을 포함합
니다.
dependencies {
compile 'com.amazonaws:aws-java-sdk-s3'
}
Gradle에서는 BOM의 정보를 사용하여 올바른 SDK 종속성 버전을 자동으로 확인합니다



### S3 이용

ClientBuilder



credentials



S3 Bucket 작업



createBucket(String bucketName)



Bucket 이름 주의점



### Object 업로드

두 가직 방식

ㄴ putObject(PutObjectRequest)

ㄴ putObject(String, String, File)



### 환경변수로 부터 Credentials 얻어오기

환경변수에 AccessKey 등록

ㄴ SDKGlobalConfiguration

ㄴ withCredentials(new EnvironmentVariableCredentialsProvider())