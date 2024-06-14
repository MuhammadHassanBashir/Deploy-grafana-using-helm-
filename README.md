# Deploying Grafana on GKE Using Helm and Integrating with GCP PostgreSQL
Overview
This document provides step-by-step instructions for deploying Grafana on a Google Kubernetes Engine (GKE) cluster using Helm, connecting it to a Google Cloud Platform (GCP) PostgreSQL database, and setting up a Grafana dashboard with alerts.

Prerequisites
A GKE cluster.
Helm installed on your local machine.
kubectl configured to interact with your GKE cluster.
A GCP PostgreSQL database.
Step-by-Step Instructions
Step 1: Set Up Helm and Kubernetes Cluster
Ensure Helm is installed and your kubectl is configured to interact with your GKE cluster.

Step 2: Add the Grafana Helm Repository
Add the official Grafana Helm repository:

sh
Copy code
helm repo add grafana https://grafana.github.io/helm-charts
Step 3: Update the Helm Repositories
Update your local Helm repositories to fetch the latest available charts:

sh
Copy code
helm repo update
Step 4: Install Grafana Chart
Install Grafana using Helm:

sh
Copy code
helm install grafana grafana/grafana --set adminPassword=<your_password>
Replace <your_password> with a secure password for the Grafana admin user.

Step 5: Verify Grafana Deployment
Check the status of the Grafana pod:

sh
Copy code
kubectl get pods
Ensure the Grafana pod is in the “Running” state.

Step 6: Access Grafana Dashboard
Expose the Grafana service to access the dashboard:

sh
Copy code
kubectl port-forward svc/grafana <desired_local_port>:3000
Replace <desired_local_port> with the local port number you want to use. Access the Grafana dashboard by navigating to http://localhost:<desired_local_port> in your web browser. Log in with the username "admin" and the password you set during installation.

Step 7: Add GCP PostgreSQL Data Source
Log in to the Grafana dashboard.
Go to Configuration > Data Sources.
Click Add data source and select PostgreSQL.
Fill in the connection details for your GCP PostgreSQL database:
Host: The IP address or hostname of your PostgreSQL server.
Database: The name of your PostgreSQL database.
User: The PostgreSQL user with access to the database.
Password: The password for the PostgreSQL user.
SSL Mode: Set to "require" if SSL is enabled.
Step 8: Create a Grafana Dashboard
Go to Dashboards > Manage and click New Dashboard.
Click Add new panel.
Select your PostgreSQL data source.
Enter your SQL query to retrieve the data you want to visualize.
Example query to check if the PostgreSQL database is up and running:

sql
Copy code
SELECT 1;
Step 9: Set Up Alerts
In the panel editor, go to the Alert tab.
Click Create Alert.
Configure the alert conditions and thresholds. For example, set an alert if the SELECT 1; query does not return a result.
Configure the notification channel:
Go to Alerting > Notification channels.
Click New channel.
Select Microsoft Teams.
Enter the details for your Teams channel webhook.
Example notification message:

json
Copy code
{
  "text": "Alert: PostgreSQL database check failed."
}
Step 10: Verify and Test
Ensure that the alerts are working by simulating a failure condition and verifying that a message is sent to your Teams channel.

Conclusion
By following these steps, you have successfully deployed Grafana on GKE using Helm, connected it to your GCP PostgreSQL database, created a dashboard with SQL queries, and set up alerts to notify you via Microsoft Teams if any issues arise. This setup provides a robust monitoring and alerting solution for your PostgreSQL database.
