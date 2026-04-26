## 1. What is a Job?
A Job (often called a Project) is a user-defined description of work. It is the "Plan." You define what to do, where the code is, and when it should run.

- Configurational: It contains the Git repository URL, the environment variables, and the specific shell commands or scripts to execute.
- Persistent: A Job stays in Jenkins forever (until you delete it), even if it hasn't run yet.

## 2. What is a Build?
A Build is a single execution of a Job. It is the "Action." If a Job is the blueprint for a car, a Build is one specific car coming off the assembly line.

- Result-Oriented: Every build has a specific outcome, such as SUCCESS, FAILURE, or ABORTED.
- Numbered: Jenkins tracks every build with a unique ID (e.g., Build #1, Build #2).
- Historical: It stores the console logs, artifacts (like a .jar or .zip file), and test results from that specific run.

## 3. Internal Working: Job vs. Build
When you click "Build Now" or a webhook triggers a process, Jenkins follows a specific internal lifecycle to move from a static Job to an active Build.

The Lifecycle
- Triggering: The process starts via a manual click, a code commit (Git hook), or a scheduled timer (Cron).
- The Queue: Jenkins places the request into the Build Queue. If all "Executors" (workers) are busy, the build waits here.
- Resource Allocation: The Jenkins Master identifies an available Executor. This can be on the Master itself or a remote Agent/Node (like a separate Linux server or a Docker container).
- Workspace Creation: Jenkins creates a dedicated directory on the Agent called a Workspace. It then "clones" your code from the repository into this folder.
- Execution: Jenkins runs the steps defined in your Job (e.g., mvn clean install or docker build).
- Post-Build Actions: After the commands finish, Jenkins performs cleanup, archives the resulting files, and sends notifications (like an email or Slack message).
- Finalization: The Build status is recorded in the Jenkins filesystem, and the Workspace is usually cleared or left for the next run.

### Behind the Scenes: The File System
On the server, Jenkins stores these internally in the $JENKINS_HOME directory:

- Jobs: Located in /jobs/[job_name]/config.xml.
- Builds: Located in /jobs/[job_name]/builds/[build_number]/. This folder contains the build.xml (metadata) and the log file.
