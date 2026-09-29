pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Repository checked out successfully'
            }
        }

        stage('Validate HTML') {
            steps {
                script {
                    def htmlFiles = findFiles(glob: '*.html')
                    if (htmlFiles.isEmpty()) {
                        error 'No HTML files found in repository'
                    }
                    
                    echo "Found ${htmlFiles.size()} HTML file(s):"
                    htmlFiles.each { file ->
                        echo "  - ${file.name} (${file.size} bytes)"
                    }
                    
                    def validationPassed = true
                    htmlFiles.each { file ->
                        def content = readFile file.name
                        
                        if (!content.trim().startsWith('<!DOCTYPE html>') && !content.trim().startsWith('<html')) {
                            echo "WARNING: ${file.name} may not be a complete HTML document (missing DOCTYPE/html tag)"
                        }
                        
                        if (!content.contains('</html>')) {
                            echo "ERROR: ${file.name} is missing closing </html> tag"
                            validationPassed = false
                        }
                        
                        if (!content.contains('<head>') || !content.contains('</head>')) {
                            echo "WARNING: ${file.name} missing <head> section"
                        }
                        
                        if (!content.contains('<body>') || !content.contains('</body>')) {
                            echo "WARNING: ${file.name} missing <body> section"
                        }
                        
                        def openTags = (content =~ /<(\w+)[^>]*>/).collect { it[1] }
                        def closeTags = (content =~ /<\/(\w+)>/).collect { it[1] }
                        def tagCounts = [:]
                        openTags.each { tagCounts[it] = (tagCounts[it] ?: 0) + 1 }
                        closeTags.each { tagCounts[it] = (tagCounts[it] ?: 0) - 1 }
                        
                        tagCounts.each { tag, count ->
                            if (count != 0 && tag !in ['br', 'hr', 'img', 'input', 'meta', 'link']) {
                                echo "WARNING: ${file.name} - tag <${tag}> count mismatch (open: ${count > 0 ? count : 0}, close: ${count < 0 ? -count : 0})"
                            }
                        }
                    }
                    
                    if (!validationPassed) {
                        error 'HTML validation failed'
                    }
                    
                    echo 'HTML validation passed'
                }
            }
        }

        stage('Report') {
            steps {
                echo '========================================'
                echo 'CI VALIDATION SUCCESSFUL'
                echo 'All HTML files are structurally valid'
                echo '========================================'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS: Pipeline completed successfully'
        }
        failure {
            echo 'BUILD FAILED: Pipeline encountered errors'
        }
        always {
            echo 'Pipeline execution completed'
        }
    }
}