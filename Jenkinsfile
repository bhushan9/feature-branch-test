properties([[$class: 'BuildConfigProjectProperty', name: '', namespace: '', resourceVersion: '', uid: ''], parameters([choice(choices: "1\n0", description: 'If 1 (default) will recompile assets. If in doubt, choose 1.', name: 'RAKE_ASSETS'), string(defaultValue: 'master', description: '', name: 'REV'), string(defaultValue: 'el6', description: '', name: 'DIST'), string(defaultValue: '', description: 'name of Juicer cart. Leave blank to skip cart creation', name: 'JUICER_CART'), string(defaultValue: 'rhsm-web', description: 'name of destination ZPulp repo for RPM', name: 'JUICER_REPO')]), pipelineTriggers([])])
pipeline {
        agent any
        stages {
          stage("Checkout SCM") {
            steps {
              git url:'https://gitlab.corp.redhat.com/it-pnt/rhsm/rhsm-web.git'
            }
          }

          stage("Build RPM") {
            steps {
              sh 'make docker/rpm'
              archiveArtifacts 'rpmbuild/RPMS/**/*.rpm'
            }
          }

          stage("Push Juicer Cart") {
            when {
              not {
                environment name: 'JUICER_CART', value: ''
              }
            }
            steps {
              withCredentials([file(credentialsId: 'juicer_conf', variable: 'JUICER_CONF')]) {
                sh 'docker pull itpnt-registry.op.redhat.com/juicer'
                sh '''
                   docker run --rm -v "${JUICER_CONF}:/root/.config/juicer/config:ro" \\
                   -v "`pwd`/rpmbuild/RPMS/noarch":/rpms:ro -w /rpms \\
                   itpnt-registry.op.redhat.com/juicer \\
                   "juicer cart create $JUICER_CART -r ${JUICER_REPO} *.rpm; juicer cart push $JUICER_CART --in re"
                '''
              }
            }
          }

        }

        post {
          always {
            cleanWs()
          }

        }

      }
