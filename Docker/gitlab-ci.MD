// name file: .gitlab-ci.yml

image: docker:latest
services:
  - docker:dind

variables:
  DOCKER_DRIVER: overlay
  SPRING_PROFILES_ACTIVE: gitlab-ci
  USER_GITLAB: truyenminhtoan
  APP_NAME: spring-boot-gitlab-ci
  REPO: spring-demo-ci
  STAGING_SERVER: 13.231.247.69
  STAGING_SERVER_USER: root
  STAGING_SERVER_PASSWORD: 0978712200

stages:
  - build
  - package

build:
  image: gradle:5.0-jdk8-alpine
  stage: build
  script:
    - gradle build
  artifacts:
    paths:
      - build/libs/*.jar
package:
  stage: package
  script:
    - docker build -t truyenminhtoan/spring-boot-gitlabci-demo:latest .
    - docker login -u truyenminhtoan -p "123456789aA@" docker.io
    - docker push truyenminhtoan/spring-boot-gitlabci-demo:latest
