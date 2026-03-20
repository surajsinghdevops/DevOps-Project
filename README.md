<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DevOps Project - CI/CD Pipeline Setup Task</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 10px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 40px 20px;
            text-align: center;
        }

        header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        header p {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .content {
            padding: 40px;
        }

        .info-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .info-card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }

        .info-card h3 {
            color: #667eea;
            margin-bottom: 10px;
            font-size: 0.9em;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .info-card p {
            font-size: 1.1em;
            color: #333;
            font-weight: 500;
        }

        .section {
            margin-bottom: 40px;
        }

        .section h2 {
            color: #667eea;
            font-size: 1.8em;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #667eea;
        }

        .section h3 {
            color: #764ba2;
            font-size: 1.3em;
            margin-top: 25px;
            margin-bottom: 15px;
        }

        .section p {
            margin-bottom: 15px;
            color: #555;
        }

        .pipeline-flow {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 8px;
            margin: 20px 0;
            font-family: 'Courier New', monospace;
            overflow-x: auto;
            border-left: 4px solid #667eea;
        }

        .task-list {
            list-style: none;
            padding: 0;
        }

        .task-list li {
            padding: 12px 0;
            padding-left: 30px;
            position: relative;
            color: #555;
        }

        .task-list li:before {
            content: "✓";
            position: absolute;
            left: 0;
            color: #667eea;
            font-weight: bold;
            font-size: 1.2em;
        }

        .task-list li ul {
            list-style: none;
            margin-top: 8px;
            margin-left: 20px;
        }

        .task-list li ul li:before {
            content: "→";
            color: #764ba2;
        }

        .requirements {
            background: #fff3cd;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #ffc107;
            margin: 20px 0;
        }

        .requirements h4 {
            color: #856404;
            margin-bottom: 10px;
        }

        .evaluation-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
        }

        .evaluation-table th,
        .evaluation-table td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        .evaluation-table th {
            background: #667eea;
            color: white;
            font-weight: 600;
        }

        .evaluation-table tr:nth-child(even) {
            background: #f8f9fa;
        }

        .evaluation-table td:last-child {
            text-align: center;
            font-weight: 600;
            color: #667eea;
        }

        .bonus-section {
            background: #d4edda;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #28a745;
            margin: 20px 0;
        }

        .bonus-section h4 {
            color: #155724;
            margin-bottom: 10px;
        }

        .submission-box {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 8px;
            margin-top: 40px;
            text-align: center;
        }

        .submission-box h3 {
            color: white;
            margin-bottom: 15px;
        }

        .submission-box p {
            color: rgba(255, 255, 255, 0.9);
            margin-bottom: 10px;
        }

        footer {
            background: #f8f9fa;
            padding: 20px;
            text-align: center;
            color: #666;
            border-top: 1px solid #ddd;
        }

        code {
            background: #f4f4f4;
            padding: 2px 6px;
            border-radius: 3px;
            font-family: 'Courier New', monospace;
            color: #e83e8c;
        }

        .highlight {
            background: #fff3cd;
            padding: 1px 3px;
        }

        @media (max-width: 768px) {
            header h1 {
                font-size: 1.8em;
            }

            .content {
                padding: 20px;
            }

            .info-grid {
                grid-template-columns: 1fr;
            }

            .evaluation-table {
                font-size: 0.9em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🚀 DevOps Internship</h1>
            <p>Practical Assessment Task: CI/CD Pipeline Setup</p>
        </header>

        <div class="content">
            <!-- Quick Info Cards -->
            <div class="info-grid">
                <div class="info-card">
                    <h3>📋 Title</h3>
                    <p>CI/CD Pipeline Setup: Git → GitHub → Jenkins → Tomcat</p>
                </div>
                <div class="info-card">
                    <h3>⚡ Difficulty</h3>
                    <p>Intermediate</p>
                </div>
                <div class="info-card">
                    <h3>⏱️ Time Estimate</h3>
                    <p>4–8 Hours</p>
                </div>
                <div class="info-card">
                    <h3>📤 Submission Format</h3>
                    <p>GitHub Repo + Documentation</p>
                </div>
            </div>

            <!-- Objective -->
            <section class="section">
                <h2>🎯 Objective</h2>
                <p>You are required to set up a fully automated CI/CD pipeline that mirrors a real-world DevOps workflow. The goal is to demonstrate your ability to integrate multiple tools and configure automation from code commit to application deployment — with <strong>zero manual intervention</strong> once the pipeline is running.</p>
            </section>

            <!-- Scenario -->
            <section class="section">
                <h2>📌 Scenario</h2>
                <p>A development team pushes Java web application code to a GitHub repository. Your job is to ensure that every push to the main branch automatically triggers a Jenkins build job, compiles the project with Maven, and deploys the resulting WAR file to a running Apache Tomcat server — all via SSH.</p>

                <h3>Pipeline Flow</h3>
                <div class="pipeline-flow">
Developer → git push → GitHub Repo → Webhook Event
                    ↓
Jenkins (Webhook Trigger) → Pull Code → mvn clean package
                    ↓
SSH Deploy → Apache Tomcat → Application Live
                </div>
            </section>

            <!-- Tools Required -->
            <section class="section">
                <h2>🛠️ Tools Required</h2>
                <p>Git, GitHub, Jenkins, Apache Maven, Apache Tomcat, Linux/Ubuntu Server</p>
            </section>

            <!-- Tasks Breakdown -->
            <section class="section">
                <h2>✅ Tasks Breakdown</h2>

                <h3>Task 1 — Version Control Setup (Git & GitHub)</h3>
                <ul class="task-list">
                    <li>Create a new public GitHub repository named <code>ci-cd-pipeline-demo</code></li>
                    <li>Initialize a local Git repository and connect it to the remote GitHub repo</li>
                    <li>Add a simple Java Maven web application (WAR project) to the repository
                        <ul>
                            <li>Use any simple servlet or Spring Boot app that builds into a .war file</li>
                            <li>Ensure a valid pom.xml is present with packaging set to war</li>
                        </ul>
                    </li>
                    <li>Push the initial code to the main branch</li>
                    <li>Create a .gitignore file to exclude build artifacts (target/, *.class, etc.)</li>
                </ul>

                <h3>Task 2 — Jenkins Installation & Configuration</h3>
                <ul class="task-list">
                    <li>Install Jenkins on a Linux server (Ubuntu/CentOS) or a local VM/cloud instance</li>
                    <li>Ensure Jenkins is accessible via browser on port 8080 (or a custom port)</li>
                    <li>Install the following required Jenkins plugins:
                        <ul>
                            <li>Git Plugin</li>
                            <li>GitHub Integration Plugin</li>
                            <li>Maven Integration Plugin</li>
                            <li>Publish Over SSH Plugin</li>
                        </ul>
                    </li>
                    <li>Configure Maven under Jenkins → Manage Jenkins → Global Tool Configuration</li>
                    <li>Add your GitHub credentials (token-based) to Jenkins Credentials Manager</li>
                </ul>

                <h3>Task 3 — GitHub Webhook Configuration</h3>
                <ul class="task-list">
                    <li>In your GitHub repository, navigate to Settings → Webhooks → Add webhook</li>
                    <li>Set the Payload URL to your Jenkins server's webhook endpoint:
                        <ul>
                            <li>Format: <code>http://&lt;JENKINS_IP&gt;:8080/github-webhook/</code></li>
                        </ul>
                    </li>
                    <li>Set Content type to <code>application/json</code></li>
                    <li>Select the trigger event: Just the push event</li>
                    <li>Verify the webhook delivers a 200 OK response from Jenkins</li>
                    <li>Ensure your Jenkins server is publicly accessible (use ngrok if running locally)
                        <ul>
                            <li>Document the ngrok/public URL used, if applicable</li>
                        </ul>
                    </li>
                </ul>

                <h3>Task 4 — Jenkins Pipeline / Freestyle Job Setup</h3>
                <ul class="task-list">
                    <li>Create a new Jenkins Freestyle Job named <code>deploy-to-tomcat</code></li>
                    <li>Under Source Code Management, select Git and enter your GitHub repo URL</li>
                    <li>Under Build Triggers, enable GitHub hook trigger for GITScm polling
                        <ul>
                            <li>This is what listens for the incoming webhook event from GitHub</li>
                        </ul>
                    </li>
                    <li>Under Build Steps, add Invoke top-level Maven targets:
                        <ul>
                            <li>Goals: <code>clean package</code></li>
                            <li>Ensure Maven picks up your pom.xml from the workspace root</li>
                        </ul>
                    </li>
                    <li>Document the exact Jenkins job configuration with screenshots</li>
                </ul>

                <h3>Task 5 — Apache Tomcat Setup & SSH Deployment</h3>
                <ul class="task-list">
                    <li>Install Apache Tomcat (version 9 or 10) on the target server</li>
                    <li>Configure a Tomcat manager user with role manager-script in tomcat-users.xml</li>
                    <li>Ensure Tomcat is running on port 8080 (or configure a non-conflicting port)</li>
                    <li>Configure SSH-based deployment from Jenkins to Tomcat server:
                        <ul>
                            <li>Generate SSH key pair on the Jenkins server</li>
                            <li>Copy the public key to the Tomcat server's authorized_keys</li>
                            <li>Configure the SSH server in Jenkins under Manage Jenkins → Configure System → Publish over SSH</li>
                        </ul>
                    </li>
                    <li>Add a Post-Build Action in the Jenkins job: Send build artifacts over SSH
                        <ul>
                            <li>Source files: <code>target/*.war</code></li>
                            <li>Remote directory: Tomcat webapps/ folder</li>
                            <li>Exec command: Restart Tomcat service or invoke manager deploy endpoint</li>
                        </ul>
                    </li>
                    <li>Confirm the WAR file is deployed and accessible via the Tomcat URL</li>
                </ul>

                <h3>Task 6 — End-to-End Verification</h3>
                <ul class="task-list">
                    <li>Make a visible code change (e.g., update a message in a servlet or HTML page)</li>
                    <li>Commit and push to the main branch</li>
                    <li>Verify the following chain completes automatically without manual intervention:
                        <ul>
                            <li>GitHub sends webhook to Jenkins</li>
                            <li>Jenkins pulls the latest code</li>
                            <li>Maven build succeeds and generates a new WAR file</li>
                            <li>WAR is deployed to Tomcat via SSH</li>
                            <li>The updated application is accessible in the browser</li>
                        </ul>
                    </li>
                    <li>Take a screenshot or screen recording of the complete automated flow</li>
                </ul>
            </section>

            <!-- Deliverables -->
            <section class="section">
                <h2>📦 Deliverables</h2>
                <p>Submit all of the following:</p>
                <ul class="task-list">
                    <li><strong>GitHub Repository</strong> — Contains the Java Maven project, pom.xml, and .gitignore</li>
                    <li><strong>Setup Documentation</strong> — A PDF or Markdown file (SETUP.md) inside the repo covering:
                        <ul>
                            <li>Step-by-step installation and configuration notes</li>
                            <li>Any commands used (install, configure, run)</li>
                            <li>Screenshots of Jenkins job config, webhook delivery, and Tomcat deployment</li>
                        </ul>
                    </li>
                    <li><strong>Jenkins Job Config Export</strong> — Export the Jenkins job XML (config.xml) and include it in the repo</li>
                    <li><strong>Demo Evidence</strong> — At least one screenshot or short screen recording showing a successful end-to-end pipeline run triggered by a git push</li>
                </ul>
            </section>

            <!-- Evaluation Criteria -->
            <section class="section">
                <h2>📊 Evaluation Criteria</h2>
                <table class="evaluation-table">
                    <thead>
                        <tr>
                            <th>Criterion</th>
                            <th>Weight</th>
                            <th>Max Score</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Git repository structure & best practices</td>
                            <td>15%</td>
                            <td>15</td>
                        </tr>
                        <tr>
                            <td>Jenkins installation and plugin configuration</td>
                            <td>15%</td>
                            <td>15</td>
                        </tr>
                        <tr>
                            <td>GitHub Webhook correctly configured and triggering Jenkins</td>
                            <td>20%</td>
                            <td>20</td>
                        </tr>
                        <tr>
                            <td>Maven build succeeds within Jenkins job</td>
                            <td>15%</td>
                            <td>15</td>
                        </tr>
                        <tr>
                            <td>Tomcat setup and SSH deployment configured correctly</td>
                            <td>20%</td>
                            <td>20</td>
                        </tr>
                        <tr>
                            <td>End-to-end automated pipeline verified with evidence</td>
                            <td>10%</td>
                            <td>10</td>
                        </tr>
                        <tr>
                            <td>Documentation quality and clarity</td>
                            <td>5%</td>
                            <td>5</td>
                        </tr>
                    </tbody>
                </table>
            </section>

            <!-- Rules & Guidelines -->
            <section class="section">
                <h2>⚠️ Rules & Guidelines</h2>
                <ul class="task-list">
                    <li>All tools must be self-hosted by you — no managed CI/CD SaaS platforms allowed (e.g., no CircleCI, no GitHub Actions)</li>
                    <li>The pipeline must be triggered automatically by a git push — do not manually trigger Jenkins builds</li>
                    <li>Do not use pre-built Docker images for the entire pipeline; Jenkins and Tomcat must be separately installed and configured</li>
                    <li>You may use any cloud provider (AWS EC2, GCP, Azure, DigitalOcean) or a local VM — document your choice</li>
                    <li>Plagiarism is not tolerated. All configuration and documentation must be your own work</li>
                </ul>
            </section>

            <!-- Bonus Points -->
            <div class="bonus-section">
                <h4>⭐ Bonus Points (Optional)</h4>
                <ul class="task-list">
                    <li>Dockerize Jenkins and/or Tomcat</li>
                    <li>Use a Jenkins Pipeline (Jenkinsfile) instead of a Freestyle job</li>
                    <li>Add a build status badge to your GitHub README</li>
                    <li>Implement a rollback mechanism if the deployment fails</li>
                </ul>
            </div>

            <!-- Submission Instructions -->
            <section class="section">
                <h2>📮 Submission Instructions</h2>
                <ul class="task-list">
                    <li>Push all code and documentation to your GitHub repository</li>
                    <li>Share the GitHub repository link via the submission form or email</li>
                    <li>Deadline: [Insert deadline here]. Late submissions will not be evaluated</li>
                    <li><span class="highlight">Ensure the repo is set to Public before submitting</span></li>
                </ul>
                <div class="requirements">
                    <h4>💡 Troubleshooting Tip</h4>
                    <p>If you face any blockers or environment issues, document them clearly in your SETUP.md — showing your troubleshooting process counts positively toward evaluation.</p>
                </div>
            </section>

            <div class="submission-box">
                <h3>Good luck! 🚀</h3>
                <p>We look forward to seeing your pipeline in action.</p>
                <p><strong>Questions?</strong> Reach out at least 24 hours before the deadline.</p>
            </div>
        </div>

        <footer>
            <p>&copy; 2024 DevOps Internship Assessment | All rights reserved</p>
        </footer>
    </div>
</body>
</html>
