Q9. Transfer files to VM using FTP, initialize Git Repo and push to GitHub
Aim: To transfer files from the host computer to Ubuntu VM using FTP, initialize a Git repository and push the project to GitHub.
Part A: Configure FTP in Ubuntu VM
Step 1: Open Ubuntu Terminal

Enter the following commands:
sudo apt update
sudo apt install vsftpd -y


Step 2: Start FTP service
sudo systemctl start vsftpd
sudo systemctl enable vsftpd


Step 3: Check FTP status
sudo systemctl status vsftpd


You should see active (running).

Step 4: Find Ubuntu IP address
hostname -I


Note down the IP address. For example:
192.168.1.100


Use your actual IP address.

Step 5: Configure FTP
sudo nano /etc/vsftpd.conf


Find or add these settings:
anonymous_enable=NO
local_enable=YES
write_enable=YES


Save using Ctrl + O, Enter, and exit using Ctrl + X.

Restart FTP:
sudo systemctl restart vsftpd


Part B: Transfer files using FileZilla

Step 6: Open FileZilla on Windows.

Enter the following details in the top connection bar:
Field	Value
Host	Ubuntu VM IP address
Username	Your Ubuntu username
Password	Your Ubuntu password
Port	21

Click Quickconnect.

Step 7: Transfer files
- Left side: Windows host files.
- Right side: Ubuntu VM files.
- On the right side, open your Ubuntu home folder.
- On the left side, select your project folder.
- Drag and drop it to the right side.
Your files will be transferred to Ubuntu.
Part C: Initialize Git and push to GitHub
Step 8: Install Git in Ubuntu
sudo apt install git -y


Step 9: Configure Git
git config --global user.name "Your Name"
git config --global user.email "your-email@gmail.com"


Use the name and email associated with your GitHub account.

Step 10: Open your project folder
cd ~/project


Replace project with your actual folder name.

Step 11: Initialize Git
git init


Step 12: Add project files
git add .


Step 13: Commit files
git commit -m "First commit"


Step 14: Create GitHub repository
1. Open GitHub.
2. Sign in.
3. Click the + icon and select New repository.
4. Enter repository name: ftp-project.
5. Click Create repository.
Step 15: Connect local repository to GitHub

Copy your repository HTTPS URL and enter:
git branch -M main
git remote add origin https://github.com/USERNAME/ftp-project.git
git push -u origin main


Replace USERNAME with your GitHub username.

If GitHub asks for authentication, use its browser sign-in flow or a Personal Access Token when prompted for HTTPS credentials.
Expected result
Your project files should be visible inside your GitHub repository.


Q10. Remote access and Git Synchronization – Clone repo on host and VM, sync changes using Git and validate using FileZilla
Aim: To clone a GitHub repository on both the host and VM, synchronize changes using Git and verify files using FileZilla.
Part A: Clone repository on Ubuntu VM
Step 1: Open Ubuntu Terminal.

Install Git if it is not installed:
sudo apt install git -y


Step 2: Clone your GitHub repository
git clone https://github.com/USERNAME/ftp-project.git


Step 3: Open the cloned folder
cd ftp-project


Step 4: Check files
ls


Part B: Clone repository on Windows host

Step 5: Open Command Prompt or VS Code Terminal.

Go to the Desktop:
cd Desktop


Step 6: Clone the same repository
git clone https://github.com/USERNAME/ftp-project.git


Now the same repository is available on Windows and Ubuntu.
Part C: Synchronize changes using Git
Step 7: Make a change on Windows.

Open the cloned project folder and create a file:
hostfile.txt


Write:
This file was created on Windows Host.


Step 8: Push changes to GitHub.

Open the terminal inside the Windows project folder:
git add .
git commit -m "Added host file"
git push origin main


Step 9: Pull changes on Ubuntu VM.

Open the Ubuntu project folder:
cd ~/ftp-project


Run:
git pull origin main


The new file will appear in Ubuntu.

Step 10: Make a change on Ubuntu.
nano vmfile.txt


Enter:
This file was created on Ubuntu VM.


Save and exit.

Step 11: Push changes from Ubuntu.
git add .
git commit -m "Added VM file"
git push origin main


Step 12: Pull changes on Windows.

In the Windows project folder, run:
git pull origin main


Now both systems have the latest files.
Part D: Validate using FileZilla

Step 13: Open FileZilla.

Connect using your Ubuntu VM IP, username, password and port 21.

Step 14: Navigate to the project folder.

On the remote (right) side, open:
/home/your-username/ftp-project


Step 15: Verify the files.

Check that both files are present:
- hostfile.txt
- vmfile.txt
This confirms that the Git synchronization has worked and the files are available on the VM.
Expected result
Changes made on Windows and Ubuntu are synchronized through GitHub and verified using FileZilla.

Q11. Implement Split Deployment Website with Form and MySQL Backend
Important practical
Aim: To create a website with a frontend, backend and MySQL database, and deploy them separately.

In split deployment:
- Frontend: HTML, CSS and JavaScript – Vercel.
- Backend: Node.js and Express – Render.
- Database: MySQL – MySQL server accessible to the deployed backend.

Part A: Create MySQL database
Step 1: Open MySQL Workbench.

Connect to your MySQL server.

Step 2: Create a database.

Execute:
CREATE DATABASE splitdb;

USE splitdb;


Step 3: Create a table.
CREATE TABLE contacts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    message TEXT
);


Part B: Create frontend
Step 4: Create a project folder.

Create a folder named:
split-deployment


Inside it, create another folder:
frontend


Inside frontend, create three files:
index.html
style.css
script.js


Step 5: Write HTML code in index.html.
<!DOCTYPE html>
<html>
<head>
    <title>Contact Form</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <h1>Contact Us</h1>

    <form id="contactForm">
        <input type="text" id="name"
        placeholder="Enter Name" required>

        <input type="email" id="email"
        placeholder="Enter Email" required>

        <textarea id="message"
        placeholder="Enter Message" required></textarea>

        <button type="submit">Submit</button>
    </form>

    <p id="result"></p>

    <script src="script.js"></script>
</body>
</html>


Step 6: Write CSS in style.css.
body {
    text-align: center;
    font-family: Arial;
    background: lightblue;
}

form {
    margin: 50px auto;
    width: 300px;
}

input, textarea, button {
    width: 100%;
    padding: 10px;
    margin: 10px;
}


Step 7: Write JavaScript in script.js.
document.getElementById("contactForm")
.addEventListener("submit", async function(e) {

    e.preventDefault();

    const data = {
        name: document.getElementById("name").value,
        email: document.getElementById("email").value,
        message: document.getElementById("message").value
    };

    const response = await fetch(
        "https://YOUR-BACKEND-URL.onrender.com/contact",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );

    const result = await response.json();

    document.getElementById("result").innerText =
        result.message;
});


Replace YOUR-BACKEND-URL after deploying your backend.
Part C: Create backend
Step 8: Create a backend folder.

Inside split-deployment, create:
backend


Open the backend folder in VS Code.

Step 9: Initialize Node.js.

Open the VS Code terminal:
npm init -y


Step 10: Install required packages.
npm install express mysql2 cors dotenv


Step 11: Create server.js.
const express = require("express");
const mysql = require("mysql2");
const cors = require("cors");
require("dotenv").config();

const app = express();

app.use(cors());
app.use(express.json());

const db = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME
});

db.connect((err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("MySQL Connected");
    }
});

app.post("/contact", (req, res) => {

    const { name, email, message } = req.body;

    const sql = "INSERT INTO contacts (name, email, message) VALUES (?, ?, ?)";

    db.query(sql, [name, email, message], (err) => {

        if (err) {
            return res.status(500).json({
                message: "Database error"
            });
        }

        res.json({
            message: "Form submitted successfully!"
        });
    });
});

app.listen(process.env.PORT || 5000, "0.0.0.0", () => {
    console.log("Server running");
});


Step 12: Create a .env file.
DB_HOST=your_mysql_host
DB_USER=your_mysql_username
DB_PASSWORD=your_mysql_password
DB_NAME=splitdb
PORT=5000


Use the actual connection details from your hosted MySQL provider. Do not upload .env to GitHub.

Step 13: Create .gitignore.
node_modules
.env


Part D: Test website locally
Step 14: Start backend.

In the backend terminal:
node server.js


You should see:
MySQL Connected
Server running


Step 15: Open frontend.

Open index.html using Live Server in VS Code.

Step 16: Test the form.
1. Enter your name.
2. Enter your email.
3. Enter a message.
4. Click Submit.
Check MySQL Workbench:
SELECT * FROM contacts;


You should see the submitted information.
Part E: Deploy backend on Render

Step 17: Upload backend to GitHub.

Create a GitHub repository named split-backend.

Inside the backend terminal:
git init
git add .
git commit -m "Backend project"
git branch -M main
git remote add origin https://github.com/USERNAME/split-backend.git
git push -u origin main


Step 18: Deploy using Render.
1. Open Render.
2. Sign up or log in.
3. Click New.
4. Select Web Service.
5. Connect your GitHub account.
6. Select split-backend.
7. Enter the following settings:
Setting	Value
Runtime	Node
Build Command	npm install
Start Command	node server.js

8. Add the database environment variables from your hosted MySQL provider under Environment.
9. Click Deploy.
Render will provide a backend URL, for example:
https://split-backend.onrender.com


See Render's Express deployment guide.

Step 19: Connect frontend to backend.

Open frontend/script.js.

Replace:
https://YOUR-BACKEND-URL.onrender.com/contact


with your actual Render URL.
Part F: Deploy frontend on Vercel
Step 20: Upload frontend to GitHub.

Create a repository named split-frontend.

Upload the three frontend files:
- index.html
- style.css
- script.js
Step 21: Deploy frontend.
1. Open Vercel.
2. Sign in using GitHub.
3. Click Add New Project.
4. Import split-frontend.
5. Select Other as the framework preset if needed.
6. Keep the root directory as ./.
7. Click Deploy.
Step 22: Test the deployed website.
1. Open your Vercel website URL.
2. Fill in the contact form.
3. Click Submit.
4. Check whether the success message appears.
5. Open MySQL Workbench or your hosted database's SQL interface.
6. Execute:
SELECT * FROM contacts;


Expected result
The frontend is deployed on Vercel, the backend is deployed on Render, and form submissions are stored in MySQL.

Important: Your backend must use a publicly reachable hosted MySQL database for this deployment. A MySQL database running only on your local computer or VM will not be accessible to Render by default. Also configure CORS to allow your actual Vercel domain before final testing.

Practical 12 – Static Website Deployment Using GitHub and Vercel

Aim

To create a static website and deploy it using GitHub and Vercel.

Software Requirements

HTML

CSS

JavaScript

VS Code

Git

GitHub

Vercel

Procedure

1. Create Project

Create a folder named static-website and create three files:

index.html

style.css

script.js

2. Write HTML Code

index.html

<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <h1>Welcome to My Website</h1>

    <p>This is my first static website.</p>

    <button onclick="showMessage()">Click Me</button>

    <script src="script.js"></script>

</body>
</html>

3. Write CSS Code

style.css

body {
    background-color: lightblue;
    text-align: center;
    font-family: Arial;
}

h1 {
    color: darkblue;
}

button {
    padding: 10px;
    background-color: blue;
    color: white;
}

4. Write JavaScript Code

script.js

function showMessage() {
    alert("Welcome to my website!");
}

5. Test Website Locally

Open the project in VS Code.

Right-click index.html.

Select Open with Live Server.

Check the website and click the button.

6. Upload Project to GitHub

Open the VS Code terminal inside the project folder.

Initialize Git:

git init

Add files:

git add .

Commit files:

git commit -m "Initial website"

Create a new GitHub repository named static-website.

Connect the repository:

git branch -M main
git remote add origin https://github.com/USERNAME/static-website.git
git push -u origin main

7. Deploy Website Using Vercel

Open Vercel and log in using GitHub.

Click Add New Project.

Import the static-website repository.

Select Other as the framework preset.

Set Root Directory to ./.

Leave Build Command empty.

Set Output Directory to ./.

Click Deploy.

8. Verify Deployment

Open the Vercel deployment URL.

Check whether the heading is displayed.

Verify CSS styling.

Click the button and check the alert message.

Result

Successfully created a static website, uploaded it to GitHub and deployed it using Vercel.




# Practical 9 – FTP File Transfer and GitHub Repository

## Aim
To transfer files from the host computer to Ubuntu VM using FTP, initialize a Git repository and push files to GitHub.

## Software Requirements
- Ubuntu VirtualBox
- FileZilla
- VS Code / Terminal
- Git
- GitHub
- VSFTPD

## Procedure

1. Install FTP server in Ubuntu.
   ```bash
   sudo apt update
   sudo apt install vsftpd -y
   ```

2. Start FTP service.
   ```bash
   sudo systemctl start vsftpd
   sudo systemctl enable vsftpd
   ```

3. Check FTP status.
   ```bash
   sudo systemctl status vsftpd
   ```

4. Find Ubuntu IP address.
   ```bash
   hostname -I
   ```

5. Open FileZilla in Windows and enter:
   - Host: Ubuntu IP address
   - Username: Ubuntu username
   - Password: Ubuntu password
   - Port: 21

6. Connect and transfer project files from Windows to Ubuntu using drag and drop.

7. Install Git.
   ```bash
   sudo apt install git -y
   ```

8. Configure Git.
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your-email@gmail.com"
   ```

9. Initialize Git repository.
   ```bash
   git init
   git add .
   git commit -m "First commit"
   ```

10. Create a repository on GitHub and connect it.
    ```bash
    git branch -M main
    git remote add origin https://github.com/USERNAME/ftp-project.git
    git push -u origin main
    ```

## Result
Successfully transferred files from Windows to Ubuntu VM using FTP and pushed the project to GitHub.

---

# Practical 10 – Remote Access and Git Synchronization

## Aim
To clone a GitHub repository on Windows and Ubuntu VM, synchronize changes using Git and validate files using FileZilla.

## Software Requirements
- Windows
- Ubuntu VirtualBox
- Git
- GitHub
- FileZilla

## Procedure

1. Install Git in Ubuntu.
   ```bash
   sudo apt install git -y
   ```

2. Clone the GitHub repository in Ubuntu.
   ```bash
   git clone https://github.com/USERNAME/ftp-project.git
   ```

3. Open the project folder.
   ```bash
   cd ftp-project
   ```

4. Clone the same repository on Windows using Command Prompt.
   ```bash
   git clone https://github.com/USERNAME/ftp-project.git
   ```

5. Create a file named `hostfile.txt` in Windows.

6. Add and commit the changes.
   ```bash
   git add .
   git commit -m "Added host file"
   git push origin main
   ```

7. Pull the changes in Ubuntu.
   ```bash
   git pull origin main
   ```

8. Create a file named `vmfile.txt` in Ubuntu.
   ```bash
   nano vmfile.txt
   ```

9. Push the changes from Ubuntu.
   ```bash
   git add .
   git commit -m "Added VM file"
   git push origin main
   ```

10. Pull the changes in Windows.
    ```bash
    git pull origin main
    ```

11. Open FileZilla and connect to Ubuntu using its IP address and FTP credentials.

12. Navigate to the project folder and verify that both `hostfile.txt` and `vmfile.txt` are present.

## Result
Successfully cloned the repository on Windows and Ubuntu, synchronized changes using Git and verified files using FileZilla.

---

# Practical 11 – Split Deployment Website with Form and MySQL Backend

## Aim
To implement a website with a frontend, backend and MySQL database and deploy them separately.

## Software Requirements
- HTML
- CSS
- JavaScript
- Node.js
- Express.js
- MySQL
- GitHub
- Render
- Vercel

## Procedure

### 1. Create MySQL Database

Open MySQL Workbench and execute:

```sql
CREATE DATABASE splitdb;
USE splitdb;

CREATE TABLE contacts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    message TEXT
);
```

### 2. Create Frontend

Create a folder named `frontend` and create these files:

- index.html
- style.css
- script.js

**index.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Contact Form</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <h1>Contact Us</h1>

    <form id="contactForm">
        <input type="text" id="name" placeholder="Enter Name" required>
        <input type="email" id="email" placeholder="Enter Email" required>
        <textarea id="message" placeholder="Enter Message" required></textarea>
        <button type="submit">Submit</button>
    </form>

    <p id="result"></p>

    <script src="script.js"></script>
</body>
</html>
```

**style.css**

```css
body {
    text-align: center;
    font-family: Arial;
    background: lightblue;
}

form {
    margin: 50px auto;
    width: 300px;
}

input, textarea, button {
    width: 100%;
    padding: 10px;
    margin: 10px;
}
```

**script.js**

```javascript
document.getElementById("contactForm")
.addEventListener("submit", async function(e) {

    e.preventDefault();

    const data = {
        name: document.getElementById("name").value,
        email: document.getElementById("email").value,
        message: document.getElementById("message").value
    };

    const response = await fetch(
        "https://YOUR-BACKEND-URL.onrender.com/contact",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );

    const result = await response.json();
    document.getElementById("result").innerText = result.message;
});
```

### 3. Create Backend

Create a folder named `backend`.

Initialize Node.js and install packages.

```bash
npm init -y
npm install express mysql2 cors dotenv
```

Create `server.js`.

```javascript
const express = require("express");
const mysql = require("mysql2");
const cors = require("cors");
require("dotenv").config();

const app = express();

app.use(cors());
app.use(express.json());

const db = mysql.createConnection({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_NAME
});

db.connect((err) => {
    if (err) {
        console.log(err);
    } else {
        console.log("MySQL Connected");
    }
});

app.post("/contact", (req, res) => {
    const { name, email, message } = req.body;

    const sql = "INSERT INTO contacts (name, email, message) VALUES (?, ?, ?)";

    db.query(sql, [name, email, message], (err) => {
        if (err) {
            return res.status(500).json({ message: "Database error" });
        }

        res.json({ message: "Form submitted successfully!" });
    });
});

app.listen(process.env.PORT || 5000, "0.0.0.0", () => {
    console.log("Server running");
});
```

Create a `.env` file with the MySQL connection details.

```text
DB_HOST=your_mysql_host
DB_USER=your_mysql_username
DB_PASSWORD=your_mysql_password
DB_NAME=splitdb
PORT=5000
```

Create `.gitignore`.

```text
node_modules
.env
```

### 4. Test Locally

1. Start the backend using `node server.js`.
2. Open the frontend using Live Server.
3. Enter name, email and message.
4. Click Submit.
5. Check the database using:

```sql
SELECT * FROM contacts;
```

### 5. Deploy Backend Using Render

1. Create a GitHub repository named `split-backend`.
2. Upload the backend files.
3. Open Render and sign in using GitHub.
4. Select New Web Service.
5. Connect the `split-backend` repository.
6. Set Build Command to `npm install`.
7. Set Start Command to `node server.js`.
8. Add the MySQL environment variables.
9. Click Deploy.
10. Copy the deployed backend URL.

### 6. Deploy Frontend Using Vercel

1. Create a GitHub repository named `split-frontend`.
2. Upload the frontend files.
3. Update the backend URL in `script.js`.
4. Open Vercel and sign in using GitHub.
5. Import the `split-frontend` repository.
6. Select Other as the framework preset.
7. Click Deploy.
8. Open the deployed website and test the contact form.
9. Check MySQL to verify submitted records.

## Result
Successfully implemented a split deployment website with a contact form, Node.js backend and MySQL database. Deployed the frontend on Vercel and backend on Render.

---

# Practical 12 – Static Website Deployment Using GitHub and Vercel

## Aim
To create a static website and deploy it using GitHub and Vercel.

## Software Requirements
- HTML
- CSS
- JavaScript
- VS Code
- Git
- GitHub
- Vercel

## Procedure

### 1. Create Project

Create a folder named `static-website` and create three files:

- index.html
- style.css
- script.js

### 2. Write HTML Code

**index.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <h1>Welcome to My Website</h1>

    <p>This is my first static website.</p>

    <button onclick="showMessage()">Click Me</button>

    <script src="script.js"></script>

</body>
</html>
```

### 3. Write CSS Code

**style.css**

```css
body {
    background-color: lightblue;
    text-align: center;
    font-family: Arial;
}

h1 {
    color: darkblue;
}

button {
    padding: 10px;
    background-color: blue;
    color: white;
}
```

### 4. Write JavaScript Code

**script.js**

```javascript
function showMessage() {
    alert("Welcome to my website!");
}
```

### 5. Test Website Locally

1. Open the project in VS Code.
2. Right-click `index.html`.
3. Select Open with Live Server.
4. Check the website and click the button.

### 6. Upload Project to GitHub

Open the VS Code terminal inside the project folder.

Initialize Git:

```bash
git init
```

Add files:

```bash
git add .
```

Commit files:

```bash
git commit -m "Initial website"
```

Create a new GitHub repository named `static-website`.

Connect the repository:

```bash
git branch -M main
git remote add origin https://github.com/USERNAME/static-website.git
git push -u origin main
```

### 7. Deploy Website Using Vercel

1. Open Vercel and log in using GitHub.
2. Click Add New Project.
3. Import the `static-website` repository.
4. Select Other as the framework preset.
5. Set Root Directory to `./`.
6. Leave Build Command empty.
7. Set Output Directory to `./`.
8. Click Deploy.

### 8. Verify Deployment

1. Open the Vercel deployment URL.
2. Check whether the heading is displayed.
3. Verify CSS styling.
4. Click the button and check the alert message.

## Result
Successfully created a static website, uploaded it to GitHub and deployed it using Vercel.