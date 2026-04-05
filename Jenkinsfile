node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace'){
        echo 'Cleaning Jenkins Workspace'
        deleteDir()
    }

    stage('Clone Repo'){
        echo 'Cloning the repo'
        git(
            branch: 'main',
            url: 'https://github.com/farzeen-ali/CICD-Jenkins-AWS'
        )
    }

    stage('Deploy to EC2'){
        echo 'Deploying to EC2'
        sh """
            # 1. Directory aur Permissions setup
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # 2. Files Copy karna
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            # 3. App Build aur PM2 Deployment
            cd ${appDir}
            npm install
            npm run build
            
            # Purana PM2 process delete karna (agar chal raha ho) taaki conflict na ho
            pm2 delete next-app || true
            
            # Naya process background mein start karna
            pm2 start npm --name "next-app" -- run start
            
            # Server restart par bhi app apne aap start ho jaye
            pm2 save
        """
    }
}