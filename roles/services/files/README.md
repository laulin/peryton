# Student Documentation

Welcome to the **Peryton** platform documentation. This guide will help you get started with the essential tools and services provided.

## Table of Contents
- [Git Basics](#git-basics)
- [SSH Keys](#ssh-keys)
- [Gogs](#gogs-tutorial)
- [PKI & Certificate Authority](#pki--certificate-authority)
- [AI Usage & Ethics](#ai-usage--ethics)

---

## Git Basics

Git is a distributed version control system that tracks changes in any set of computer files. It is essential for collaboration and code management.

### Common Commands

-   **`git clone <url>`**
    Downloads a repository from a remote server (like Gogs) to your local machine.
    ```bash
    git clone git@gogs.cyber:username/repository.git
    ```

-   **`git status`**
    Shows the working tree status. It lets you see which changes have been staged, which haven't, and which files aren't being tracked by Git.

-   **`git add <file>`**
    Adds a file (or changes) to the staging area. To add all changes, use `git add .`
    ```bash
    git add my_script.py
    ```

-   **`git commit -m "message"`**
    Records changes to the repository. Always write a clear, descriptive message about what you changed.
    ```bash
    git commit -m "Added login feature"
    ```

-   **`git push`**
    Uploads your local commits to the remote repository.
    ```bash
    git push origin main
    ```

-   **`git pull`**
    Fetches and integrates changes from the remote repository to your local machine.

---

## SSH Keys

SSH (Secure Shell) keys provide a more secure way of logging into a server with SSH than using a password alone. Git uses SSH keys for authentication, allowing you to push and pull without typing your password every time.

### Why use SSH?
-   **Security**: Extremely difficult to crack compared to passwords.
-   **Convenience**: No need to type credentials for every Git operation.

### How to create an SSH key

1.  Open your terminal.
2.  Run the generation command:
    ```bash
    ssh-keygen -t ed25519
    ```
3.  Press Enter to save the key to the default file (`~/.ssh/id_ed25519`).
4.  (Optional) Enter a passphrase for extra security.

### Retrieving your Public Key
You will need your **public key** to add it to Gogs. Do not share your *private key*.
```bash
cat ~/.ssh/id_ed25519.pub
```
Copy the output starting with `ssh-ed25519 ...`.

---

## Gogs Tutorial

[Gogs](https://gogs.cyber) is the Git service hosting your repositories.

### 1. Creating a Repository
1.  Log in to [https://gogs.cyber](https://gogs.cyber).
2.  Click the **+** (plus) icon in the top right header and select **New Repository**.
3.  Enter a **Repository Name**.
4.  **IMPORTANT**: ensuring visibility is set to **Public**.
    > [!IMPORTANT]
    > **Your repositories MUST be PUBLIC to be graded by the instructors.**
5.  Click **Create Repository**.

### 2. Adding your SSH Key
To push code from your computer, you must link your SSH key.
1.  Go to your **Account Settings** (avatar > Settings).
2.  Select **SSH Keys** tab.
3.  Click **Add Key**.
4.  Give it a name (e.g., "My Laptop").
5.  Paste the content of your public key (from the previous section).
6.  Click **Add Key**.

### 3. Cloning your Repository
Once created, copy the **SSH URL** from the repository page (e.g., `git@gogs.cyber:user/repo.git`) and run:
```bash
git clone git@gogs.cyber:user/repo.git
```

---

## PKI & Certificate Authority

To ensure secure communication (HTTPS), the Peryton platform uses an internal **Public Key Infrastructure (PKI)**.

### What is a PKI?
PKI is a system of processes, technologies, and policies that allows you to encrypt and sign data. At its core is the **Certificate Authority (CA)**, a trusted entity that issues digital certificates.
When you visit a website like `https://google.com`, your browser trusts it because Google's certificate was signed by a CA already trusted by your operating system.

### Why do I see "Not Secure"?
Since Peryton uses its own private CA (not a public one like Let's Encrypt), your computer doesn't know it should trust it. This results in security warnings when accessing services like `https://gogs.cyber` or `https://homepage.cyber`.

### How to Trust the Platform
To fix this, you need to install the Peryton Root CA certificate into your computer's trust store.

1.  **Download the Certificate**:
    -   Go to [http://crl.cyber](http://crl.cyber).
    -   Look for the Root CA file (usually named `root_ca.crt`, `ca.pem`, or similar).
    -   Download it to your machine.

2.  **Install Certificate**:

    -   **Windows**:
        1.  Double-click the downloaded file.
        2.  Click **Install Certificate**.
        3.  Select **Local Machine** (requires authorization).
        4.  **Crucial Step**: Choose **"Place all certificates in the following store"** and select **"Trusted Root Certification Authorities"**.
        5.  Finish the wizard.

    -   **Linux (Ubuntu/Debian)**:
        1.  Copy the file to `/usr/local/share/ca-certificates/peryton.crt`.
        2.  Run `sudo update-ca-certificates`.

    -   **macOS**:
        1.  Double-click the certificate to open Keychain Access.
        2.  Add it to the **System** keychain.
        3.  Double-click the added certificate, expand **Trust**, and set **"When using this certificate"** to **Assign Trust**.

3.  **Verify**:
    Restart your browser and visit [https://homepage.cyber](https://homepage.cyber). The "Not Secure" warning should be gone, and you should see a secure lock icon.

---

## AI Usage & Ethics

Artificial Intelligence (AI) tools (like ChatGPT, Claude, Copilot) are powerful assistants for engineers.

### Rules of Engagement
1.  **Allowed Usage**: You are authorized to use AI assistants for all coursework, labs, and projects.
2.  **Prohibited Usage**: AI usage is **STRICTLY FORBIDDEN** during formal evaluations and exams.

### Ethical Guidelines & Best Practices

-   **Understand, Don't Just Copy**: Use AI to explain concepts or debug errors. If you copy code you don't understand, you are not learning, and you will fail the exams where AI is not available.
-   **Verification is Mandatory**: LLMs (Large Language Models) hallucinations are real. They can produce code that looks correct but is buggy or insecure. You are responsible for the code you commit.
-   **Data Privacy**: Never paste sensitive data (passwords, private keys, personal data, proprietary code) into public AI prompts.
-   **Academic Integrity**: If a significant portion of your project was generated by AI, it is good practice to acknowledge it in your project's documentation.
