pipeline
{
  agent any
  stages
  {
    stage('Build')
    {
      steps
      {
      echo 'Build app'
      }
    }
  
  stage('Test')
  {
    steps
    {
      echo 'Test app'
    }
  }

  stage('Deploy')
  {
    steps
    {
      echo 'Deply app'
    }
  }
  }
  post
    {
      failure
      {
        emailext body: 'Summary',subject:'Pipeline Status', to:'kabilan848964@gmail.com'
      }
    }
  
}
