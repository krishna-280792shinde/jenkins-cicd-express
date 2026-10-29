# Express Frontend – Jenkins CI/CD Deployment

## 1. Project Overview

This project demonstrates Continuous Integration and Deployment (CI/CD) for a Node.js Express application using GitHub, Jenkins, and AWS EC2.

When a developer pushes code to GitHub, a webhook sends a notification to Jenkins. Jenkins executes the configured pipeline and deploys the application to the EC2 instance.

## 2. Technologies Used

- Node.js
- Express.js
- npm
- Git and GitHub
- Jenkins
- AWS EC2 (Ubuntu)
- Linux systemd
- GitHub Webhooks

## 3. Architecture

Developer → GitHub Repository → GitHub Webhook → Jenkins Pipeline → AWS EC2 → Express Application

The Express application runs as a systemd service and listens on port `3000`.

## 4. Prerequisites

- An AWS EC2 Ubuntu instance
- Node.js and npm
- Git
- Jenkins installed and running
- An accessible GitHub repository
- A configured GitHub webhook
- Appropriate AWS Security Group rules

## 5. GitHub Repository

Repository: https://github.com/krishna-280792shinde/jenkins-cicd-express

## 6. Run the Application Locally

Clone the repository:

```bash
git clone https://github.com/krishna-280792shinde/jenkins-cicd-express.git
cd jenkins-cicd-express
```

Install dependencies:

```bash
npm install
```

Start the application using the start script defined in `package.json`:

```bash
npm start
```

If the project uses a different start script, use the command specified in its `package.json`.

Open the application locally:

http://localhost:3000/

## 7. Jenkins Pipeline Configuration

Create or configure the Jenkins job:

`Express-CI-Pipeline`

The pipeline should use the Jenkinsfile committed to the Express repository. Its configured stages may include:

1. Checkout source code from GitHub.
2. Check the Node.js and npm environment.
3. Install dependencies.
4. Validate the application.
5. Run configured tests.
6. Deploy the application if a deployment stage is configured.

The Express deployment service on EC2 is managed by systemd:

```text
Service: express-frontend
Port: 3000
```

## 8. Configure the GitHub Webhook

In the GitHub repository, navigate to:

**Settings → Webhooks → Add webhook**

Configure the webhook:

| Setting | Value |
|---|---|
| Payload URL | `http://xx.xxx.xxx.xxx:8080/github-webhook/` |
| Content type | `application/json` |
| Events | Just the push event |
| Active | Enabled |

In Jenkins, open the `Express-CI-Pipeline` job and enable **GitHub hook trigger for GITScm polling** under Build Triggers.

A push to the branch configured in Jenkins should automatically trigger the pipeline.

## 9. Deployment and Verification

The deployed application is accessible at:

http://xx.xxx.xxx.xxx:3000/

Check the systemd service:

```bash
sudo systemctl status express-frontend --no-pager
```

Check the application locally on EC2:

```bash
curl -i http://127.0.0.1:3000/
```

Check the listening port:

```bash
sudo ss -ltnp | grep ':3000'
```

View service logs:

```bash
sudo journalctl -u express-frontend -n 100 --no-pager
```

Restart the service if necessary:

```bash
sudo systemctl restart express-frontend
```

## 10. Troubleshooting

### Application is unavailable

Verify that the service is running:

```bash
sudo systemctl status express-frontend --no-pager
```

### Port 3000 is not listening

```bash
sudo ss -ltnp | grep ':3000'
```

### Check application logs

```bash
sudo journalctl -u express-frontend -n 100 --no-pager
```

### GitHub webhook does not trigger Jenkins

- Verify that the webhook delivery succeeds in GitHub.
- Confirm the Jenkins job has the GitHub hook trigger enabled.
- Check that the webhook points to port `8080`, not port `3000`.
- Ensure the commit is pushed to the branch configured in the Jenkins job.

## 11. Security Considerations

- Never commit credentials or private keys to GitHub.
- Restrict SSH access to trusted IP addresses.
- Avoid exposing application ports to all internet addresses unless necessary.
- Protect Jenkins with authentication and appropriate authorization.
- Use HTTPS and appropriate webhook security controls for production deployments.

## 12. Conclusion

This project demonstrates the use of GitHub webhooks and Jenkins to automate the CI/CD workflow for a Node.js Express frontend deployed on AWS EC2.

The pipeline was tested successfully, and a GitHub push triggered a new Jenkins build automatically.