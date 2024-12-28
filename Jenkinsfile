pipeline 
{
    agent any

    environment 
    {
        VENV_PATH = "venv"  // Virtual environment directory
        FLASK_APP = "app.py"  // Flask application entry point
    }

    stages 
    {

        stage('Clone Repository') 
        {
            steps 
            {
                echo 'Cloning the repository...'
                git branch: 'main', url: 'https://github.com/Ch-HaiQa/Flask_Lab.git'
            }
        }

        stage('Install Dependencies') 
        {
            steps 
            {
                echo 'Setting up Python virtual environment and installing dependencies...'
                sh '''
                    python3 -m venv $VENV_PATH
                    source $VENV_PATH/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Unit Tests') 
        {
            steps 
            {
                echo 'Running unit tests with pytest...'
                sh '''
                    source $VENV_PATH/bin/activate
                    pytest --junitxml=results.xml
                '''
            }

            post 
            {
                always 
                {
                    junit 'results.xml'  // Archive test results
                }
            }
        }

        stage('Deploy Application') 
        {
            steps 
            {
                echo 'Deploying the Flask application...'
                sh '''
                    source $VENV_PATH/bin/activate
                    export FLASK_APP=$FLASK_APP
                    export FLASK_ENV=production
                    nohup flask run --host=0.0.0.0 --port=8000 &
                '''
            }
        }
    }

    post 
    {
        success 
        {
            echo 'Pipeline completed successfully!'
        }
        failure 
        {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
