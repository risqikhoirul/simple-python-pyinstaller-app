node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash name: 'compiled-results', includes: 'sources/*.py*'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test sources/test_calc.py'
        }
    }

    stage('Persetujuan Manual') {
        input message: 'Lanjutkan ke tahap Deploy?', submitterParameter: 'APPROVAL'
    }

    stage('Deploy') {
        docker.image('python:2-alpine').inside {
            unstash 'compiled-results'
            sh 'python sources/add2vals.py 10 20'  // Ganti 10 dan 20 dengan nilai input yang diinginkan
            // Tambahkan langkah penyebaran tambahan untuk aplikasi Python Anda
        }
        sleep time: 60, unit: 'SECONDS'  // Jeda eksekusi selama 1 menit (60 detik)
    }

}
