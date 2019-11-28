// name file: .gitlab-ci.yml

image: docker:latest
services:
  - docker:dind

variables:
  DOCKER_DRIVER: overlay
  SPRING_PROFILES_ACTIVE: gitlab-ci
  USER_GITLAB: user_name_replace
  APP_NAME: spring-boot-gitlab-ci
  REPO: spring-demo-ci
  STAGING_SERVER: xxxxx
  STAGING_SERVER_USER: root
  STAGING_SERVER_PASSWORD: xxxxxx

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
    - docker build -t xxxxx/spring-boot-gitlabci-demo:latest .
    - docker login -u xxxxx -p "xxxxx" docker.io
    - docker push xxxxx/spring-boot-gitlabci-demo:latest
