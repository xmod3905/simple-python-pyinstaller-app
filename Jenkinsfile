node {
    checkout scm

    docker.image('python:3.9-slim').inside('-u root') {
        try {
            stage('Build') {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
            }

            stage('Test') {
                sh '''
                    export PYTHONUSERBASE=$WORKSPACE/.pip-packages
                    export PATH=$PATH:$PYTHONUSERBASE/bin
                    pip install --user pytest
                    py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py
                '''
                junit 'test-reports/results.xml'
            }

            stage('Deliver') {
                sh '''
                    apt-get update && apt-get install -y binutils
                    export PYTHONUSERBASE=$WORKSPACE/.pip-packages
                    export PATH=$PATH:$PYTHONUSERBASE/bin
                    pip install --user pyinstaller
                    pyinstaller --onefile sources/add2vals.py
                '''
                archiveArtifacts 'dist/add2vals'
            }
        } catch (exc) {
            currentBuild.result = 'FAILURE'
            throw exc
        }
    }
}