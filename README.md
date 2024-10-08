
# SecureCodingReview
 It is a tool for secure source code review. It conducts a thorough assessment of provided code to identify potential vulnerabilities and risks, evaluating for common attack vectors such as SQL injection, XSS, buffer overflow, and remote code execution. It analyzes the code for secure coding practices, including input validation, output sanitization, authentication and access controls, and error handling. Based on its findings, it provides detailed recommendations for improving the code's security posture and mitigating any risks found. The tool may suggest changes to code architecture, libraries, and frameworks. SecureEye's comprehensive report includes a detailed description of each vulnerability found, its severity, and a snippet of affected code. The report also includes a walkthrough of the mitigation steps taken and a snippet of the updated, mitigated code, helping to ensure the application's security and integrity while minimizing the risk of future security incidents.
### Create virtualenv
```bash
python3 -m venv venv 
source ./venv/bin/activate
```
### Install dependencies
```bash
python3 -m pip install requirements.txt
```
### use as cmdline
```bash
python3 cli.py
```
### use as web app
```bash
streamlit run app.py
```
### OutPut

Upon examining the provided code, I have identified the following security concern:

Vulnerability Type: Unvalidated Input

Vulnerability Description: The 'username' parameter is passed through GET request without being validated or sanitized before being included in the `$links` array. This may allow an attacker to inject malicious input, potentially leading to security issues such as XSS or SQLi attacks.

Severity: Medium

A snippet of affected code:
```
'User Filter' => '/index.php?username=jared',
```

Mitigation walkthrough: All user input should be considered potentially harmful and should thus be sanitized appropriately. In this case, the 'username' parameter should be validated and sanitized before being used in the `$links` array. One effective way of mitigating this vulnerability is to use the `filter_input()` function to sanitize the input parameter. For example:
```
$username = filter_input(INPUT_GET, 'username', FILTER_SANITIZE_STRING);
```

A snippet of mitigated code:
```
$raw_username = 'jared';
$username = filter_var($raw_username, FILTER_SANITIZE_STRING);
$links = array(
                'User Filter' => '/index.php?username=' . $username,
                'Login' => '/login.php', 
                'Send Message' => '/sendmessage.php', 
                'View Messages' => '/messages.php', 
                'Edit Profile' => '/editprofile.php',
                'Load Template' => '/template.php?load=loadme'
          );
```

This code filters $_GET['username'] and apply a filter to validate that it is a string. The sanitized result is then assigned to $username which is subsequently used in the `$links` array. This makes sure that any malicious input to the `username` parameter will be sanitized and cannot be used to execute a successful attack.
821

![image](https://user-images.githubusercontent.com/51442494/230654799-691528ac-8cc4-4032-b753-90d1152661de.png)

![image](https://github.com/abdulr7mann/SecureEye/assets/51442494/78658a7f-5d7b-478c-bd0c-cf7465092fc4)

