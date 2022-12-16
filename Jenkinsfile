pipeline {
  agent any

  environment {
    // Expose the api token as an environment variable
    BOOST_API_TOKEN = credentials('boost-api-token')

    // For BitBucket it may be necessary to overwrite autodetection and
    // manually define the project name.
    // ex.: boostsecurity/scanner
    // BOOST_GIT_PROJECT = "GIT_SCM_ORG_NAME/GIT_SCM_REPO_NAME"
  }

  parameters {
    string name: "enabled",
      defaultValue: "true",
      description: ""
    booleanParam name: "BOOST_API_ENABLED",
      defaultValue: true,
      description: ""
    string name: "BOOST_API_ENDPOINT",
      defaultValue: "",
      description: ""
    string name: "BOOST_CLI_ARGUMENTS",
      defaultValue: "",
      description: ""
    string name: "BOOST_CLI_VERSION",
      defaultValue: "",
      description: ""
    string name: "BOOST_GIT_MAIN_BRANCH",
      defaultValue: "",
      description: ""
    booleanParam name: "BOOST_IGNORE_FAILIRE",
      defaultValue: false,
      description: ""
    string name: "BOOST_LOG_LEVEL",
      defaultValue: "INFO",
      description: ""
    string name: "BOOST_SCAN_LABEL",
      defaultValue: "",
      description: ""
    string name: "BOOST_SCANNER_ID",
      defaultValue: "",
      description: ""
    string name: "BOOST_SCANNER_REGISTRY_MODULE",
      defaultValue: "boostsecurityio/native-scanner",
      description: ""
  }

  stages {
    stage('BoostSecurityScanner') {
      when {
        // multibranch plugin executes without params on first init
        expression {
          return params.enabled == "true"
        }
      }

      steps {
        sh label: "download the boost cli",
          script: """
            curl -s https://assets.build.boostsecurity.io/boost-cli/get-boost-cli | bash
          """

        sh """
          env
        """

        withCredentials([gitUsernamePassword(credentialsId: "github-token")]) {
          sh label: "scan with ${params.BOOST_SCANNER_REGISTRY_MODULE}",
            script: """
              export BOOST_API_ENABLED=false
              "${env.WORKSPACE_TMP}/boost-cli/latest" scan repo
            """
        }
      }
    }
  }
}

