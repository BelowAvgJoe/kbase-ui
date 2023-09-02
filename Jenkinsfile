#!/bin/env groovy

def githubApiTokenCredentialsId = 'cb-docs-robot'
def githubApiTokenCredentials = string(credentialsId: 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, variable: 'GITHUB_API_TOKEN')

// Jenkins job configuration
// -------------------------
// Category: Multibranch Pipeline
// Pipeline name: kbase-ui
// Branch Sources: Single repository & branch
// Name: main
// Source Code Management: Git
// Repository URL: https://github.com/BelowAvgJoe/kbase-ui
// Credentials: - none -
// Branch specifier: refs/heads/master
// Advanced clone behaviors: [ ] Fetch tags, [x] Honor refspec on initial clone, [x] Shallow clone, Shallow clone depth: 5
// Polling ignores commits with certain messages: (?s)(?:Release |.*\[skip .+?\]).*
// Build Configuration:
// Mode: by Jenkinsfile
// Script Path: Jenkinsfile
// [x] Discard old items: Days to keep old items: 60
pipeline {
  agent any
  options {
    disableConcurrentBuilds()
  }
  stages {
    stage('Configure') {
      steps {
        script {
          properties([
            [$class: 'GithubProjectProperty', projectUrlStr: env.GIT_URL],
            pipelineTriggers([githubPush()]),
          ])
        }
      }
    }
    stage('Install') {
      steps {
        nodejs('node10') {
          sh 'npm install --quiet --no-progress --cache=.cache/npm --no-audit'
        }
      }
    }
    stage('Release') {
      steps {
        dir('public') {
          deleteDir()
        }
        withCredentials([githubApiTokenCredentials]) {
          nodejs('node10') {
            sh '$(npm bin)/gulp release'
          }
        }
      }
    }
  }
}
