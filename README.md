# You can override the included template(s) by including variable overrides
# SAST customization: https://docs.gitlab.com/ee/user/application_security/sast/#customizing-the-sast-settings
# Secret Detection customization: https://docs.gitlab.com/ee/user/application_security/secret_detection/#customizing-settings
# Dependency Scanning customization: https://docs.gitlab.com/ee/user/application_security/dependency_scanning/#customizing-the-dependency-scanning-settings
# Note that environment variables can be set in several places
# See https://docs.gitlab.com/ee/ci/variables/#cicd-variable-precedence
stages:
  - build
  - test
  - deploy

variables:
  # Specify the Ruby version required by your project (align with Gemfile)
  RUBY_VERSION: 3.0.4
  BUNDLER_VERSION: 2.2.33

build-job:
  stage: build
  image: ruby:${RUBY_VERSION}  # Use a Ruby image with the correct version
  before_script:
    - gem install bundler -v ${BUNDLER_VERSION}  # Ensure correct Bundler version
  script:
    - bundle install
    - echo "Compiling the code..."
    - echo "Compile complete."

unit-test-job:
  stage: test
  script:
    - echo "Running unit tests... This will take about 60 seconds."
    - sleep 60
    - echo "Code coverage is 90%"

lint-test-job:
  stage: test
  script:
    - echo "Linting code... This will take about 10 seconds."
    - sleep 10
    - echo "No lint issues found."

deploy-job:
  stage: deploy
  environment: production
  script:
    - echo "Deploying application..."
    - echo "Starting deployment to production..."
    - ./deploy.sh  # Replace with your actual deployment script
    - echo "Application successfully deployed."

sast:
  stage: test
include:
- template: Security/SAST.gitlab-ci.yml


