pipeline {
    agent any
    
    stages {
        stage('Find URLs and AWS endpoints') {
            steps {
                script {
                    def urls = []
                    def awsEndpoints = []
                    
                    // Search for URLs
                    def urlPattern = /"((http|https):\/\/)?[a-zA-Z0-9\/?=_-]+"/
                    def urlFinder = new FileFinder("**/*.html")
                    urlFinder.eachFileRecurse { file ->
                        urls.addAll(file.text.findAll(urlPattern))
                    }
                    
                    writeFile(file: 'urls.txt', text: urls.join('\n'))
                    
                    // Search for AWS endpoints
                    def awsEndpointPattern = /"(https?:\/\/[a-z0-9-]+.execute-api.[a-z]+.amazonaws.com)"/
                    def awsEndpointFinder = new FileFinder("**/*.js")
                    awsEndpointFinder.eachFileRecurse { file ->
                        awsEndpoints.addAll(file.text.findAll(awsEndpointPattern))
                    }
                    
                    writeFile(file: 'aws_endpoints.txt', text: awsEndpoints.join('\n'))
                    
                    // Display search results
                    echo "URLs found:"
                    sh "cat urls.txt"
                    
                    echo "AWS endpoints found:"
                    sh "cat aws_endpoints.txt"
                }
            }
        }
    }
}
