# ProfileSync RPA Automation

## Overview
ProfileSync is an enterprise-grade Robotic Process Automation (RPA) solution built with **UiPath**. It utilizes a robust **Dispatcher-Performer architecture** to automate social media management, specifically handling the distribution of posts and profile synchronizations across multiple platforms (e.g., Twitter, LinkedIn).

This project relies on UiPath Orchestrator for workload distribution, utilizing Queues and Triggers to achieve seamless, unattended execution.

---

## 🏗️ Architecture

The solution is divided into two primary operational phases to ensure scalability and fault tolerance:

### 1. The Dispatcher (`ProfileSync_Dispatcher`)
* **Role:** The data producer.
* **Functionality:** Reads the local `SocialMediaPosts.xlsx` database. It processes complex rows where a single entry might target multiple platforms (e.g., `"Twitter, LinkedIn"`).
* **Data Transformation:** Uses synchronized array indexing to dynamically split distinct credentials (Emails, Passwords, Usernames) and map them to their specific target platforms.
* **Output:** Pushes perfectly structured Transaction Items into the UiPath Orchestrator Queue, appending specific content (backpack) for the performers to consume.

### 2. The Performers (Inside `Performers/` Folder)
* **Role:** The data consumers.
* **Modules:** * `Social_Performer.xaml` (Handles social media posting workflows)
  * `ProfilePic_Performer.xaml` (Handles profile picture synchronization workflows)
* **Functionality:** Woken up automatically by Orchestrator Queue Triggers. Each Performer extracts the `SpecificContent` (TargetPlatform, Email, Password, UserName) from the queue item, logs into the designated web platform via Edge/Chrome, and executes the UI automation.

---

## ⚙️ Orchestrator Configuration

To deploy this project to the cloud, the following Orchestrator components are required within **My Workspace**:

1. **Queues:**
   * `SocialMedia_Queue` (Auto-retry enabled, Specific Content validation).
2. **Processes:**
   * `Process_Dispatcher` (Entry Point: `Main.xaml` or `ProfileSync_Dispatcher.xaml`)
   * `Process_SocialPerformer` (Entry Point: `Performers/Social_Performer.xaml`)
   * `Process_ProfilePics` (Entry Point: `Performers/ProfilePic_Performer.xaml`)
3. **Triggers:**
   * **Time Trigger:** Scheduled to run `Process_Dispatcher` unattended at designated intervals.
   * **Queue Triggers:** Linked to the Queues to instantly launch the Performers when new items arrive. *(Set to: 1 minimum item, 1 maximum simultaneous job, and "reassess conditions" enabled to handle bulk drops).*

---

## 📊 Data Input Structure

The Dispatcher expects an Excel file (`SocialMediaPosts.xlsx`) with the following column structure. The bot is capable of reading comma-separated values to generate cloned transactions for cross-platform posting.

| Platforms | Emails | Passwords | Usernames |
| :--- | :--- | :--- | :--- |
| Twitter, LinkedIn | user1@mail.com, user2@mail.com | pass1, pass2 | usr_1, usr_2 |

---

## 🚀 Setup & Installation

1. Clone the repository to your local machine.
2. Open the project folder in **UiPath Studio**.
3. Ensure the UiPath Web Automation extension is installed and enabled in your browser.
4. Verify that `Social_Performer.xaml` and `ProfilePic_Performer.xaml` have **Enable Entry Point** activated in the Project panel.
5. Click **Publish** to send the consolidated package to your Orchestrator Tenant.
6. Provision an **Unattended Robot** profile in Orchestrator (linking your Windows `domain\username` and password) if utilizing Time Triggers.

---

## 🎓 Academic Context

Developed by students at **Ain Shams University, Faculty of Computer and Information Sciences**. This project demonstrates the practical application of enterprise RPA design patterns, queue management, and unattended cloud automation.
