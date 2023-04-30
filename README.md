# Profile-Application-using-Python-Flask-and-MySQL


### Installation
#### Please ensure that the necessary prerequisites are installed on your computer. 
#### 1. #### - Install Visual Studio Code from: [https://code.visualstudio.com/download](https://code.visualstudio.com/download "https://code.visualstudio.com/download")
<img width="738" alt="Download VS Code" src="https://user-images.githubusercontent.com/40535130/230751065-af35edeb-c22f-494b-b53a-2074eec29e68.png">

#### 2.  Python and MySQL Workbench should be installed in the system.
##### - Installing Python: [https://www.python.org/downloads/](https://www.python.org/downloads/ "https://www.python.org/downloads/")

![download Python](https://user-images.githubusercontent.com/40535130/232453153-14bffa98-7291-4b4c-8be5-fcac82d340c6.png)


##### - Installing MySQL Workbench: [https://dev.mysql.com/downloads/workbench/](https://dev.mysql.com/downloads/workbench/ "https://dev.mysql.com/downloads/workbench/")

![download MySQL](https://user-images.githubusercontent.com/40535130/232453241-46392e07-d514-44ae-908b-0cc3b3abb64e.png)

##### - Installing MySQL Community Server: [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/ "https://dev.mysql.com/downloads/mysql/")

![MySQL Community Server](https://user-images.githubusercontent.com/40535130/232611764-8e18082a-f1fb-4720-9e7f-7b728b6bd74a.png)

##### - Install "mysqlbd" module in your venv.
    pip install flask-mysqldb

###### After completing the installation of both Python and MySQL Workbench, you should be able to use them on your system.

#### 3. Technologies used in the project: Flask framework, MySQL Workbench.
##### - Flask framework is a lightweight and flexible Python web framework that allows you to build web applications quickly and easily. It is known for its simplicity and ease of use, making it a popular choice for beginners and small to medium-sized projects.

##### - MySQL Workbench is a graphical tool for designing, managing, and administering MySQL databases. It provides an intuitive interface for creating and managing databases, tables, users, and more.
![MySQL Workbench](https://user-images.githubusercontent.com/40535130/232454268-99ee1c16-766f-4268-84ed-b6c0d22a19e2.png)

------------
## Implementation of the Project:

### Collect the Code into Azure DevOps Git repository.

#### - Create an account on Azure DevOps: [https://aex.dev.azure.com/me?mkt=en-US](https://aex.dev.azure.com/me?mkt=en-US "https://aex.dev.azure.com/me?mkt=en-US")

#### - Create new Organization in your Azure DevOps account.
<img width="848" alt="Create new Organization" src="https://user-images.githubusercontent.com/40535130/235338789-cec43278-561d-416b-8859-58ace2decaaa.png">

#### - Create a new project in your Azure DevOps account.
<img width="858" alt="Create new Project" src="https://user-images.githubusercontent.com/40535130/235338819-965141f2-089f-4955-bc94-c1e0aec97ae9.png">


#### - Create a new Git repository and Clone the repository using the command:
    git clone <REPOSITORY_URL>

<img width="860" alt="git clone" src="https://user-images.githubusercontent.com/40535130/235339109-1601482e-09c0-4a62-b592-0f390086dd17.png">

#### - Open a terminal in VS Code and navigate to the directory where you want to clone the repository.
------------

### Creating Project
#### - Create an empty folder "geeksprofile"
#### - open your code editor and open this "geeksprofile" folder
#### - Create "app.py" folder and write the code given below.

    # Store this code in 'app.py' file
    from flask import Flask, render_template, request, redirect, url_for, session
    from flask_mysqldb import MySQL
    import MySQLdb.cursors
    import re
    
    
    app = Flask(__name__)
    
    
    app.secret_key = 'your secret key'
    
    
    app.config['MYSQL_HOST'] = 'localhost'
    app.config['MYSQL_USER'] = 'geeksprofile_user'
    app.config['MYSQL_PASSWORD'] = 'geeksprofile_password'
    app.config['MYSQL_DB'] = 'geekprofile'
    
    mysql = MySQL(app)
    
    @app.route('/')
    @app.route('/login', methods=['GET', 'POST'])
    def login():
    	msg = ''
    	if request.method == 'POST' and 'username' in request.form and 'password' in request.form:
    		username = request.form['username']
    		password = request.form['password']
    		cursor = mysql.connection.cursor(MySQLdb.cursors.DictCursor)
    		cursor.execute(
    			'SELECT * FROM accounts WHERE username = % s \
    			AND password = % s', (username, password, ))
    		account = cursor.fetchone()
    		if account:
    			session['loggedin'] = True
    			session['id'] = account['id']
    			session['username'] = account['username']
    			msg = 'Logged in successfully !'
    			return render_template('index.html', msg=msg)
    		else:
    			msg = 'Incorrect username / password !'
    	return render_template('login.html', msg=msg)
    
    
    @app.route('/logout')
    def logout():
        ession.pop('loggedin', None)
        session.pop('id', None)
        session.pop('username', None)
        return redirect(url_for('login'))
    
    
    @app.route('/register', methods=['GET', 'POST'])
    def register():
    	msg = ''
    	if request.method == 'POST' and 'username' in request.form and 'password' in request.form and 'email' in request.form and 'address' in request.form and 'city' in request.form and 'country' in request.form and 'postalcode' in request.form and 'organisation' in request.form:
    		username = request.form['username']
    		password = request.form['password']
    		email = request.form['email']
    		organisation = request.form['organisation']
    		address = request.form['address']
    		city = request.form['city']
    		state = request.form['state']
    		country = request.form['country']
    		postalcode = request.form['postalcode']
    		cursor = mysql.connection.cursor(MySQLdb.cursors.DictCursor)
    		cursor.execute(
    			'SELECT * FROM accounts WHERE username = % s', (username, ))
    		account = cursor.fetchone()
    		if account:
    			msg = 'Account already exists !'
    		elif not re.match(r'[^@]+@[^@]+\.[^@]+', email):
    			msg = 'Invalid email address !'
    		elif not re.match(r'[A-Za-z0-9]+', username):
    			msg = 'name must contain only characters and numbers !'
    		else:
    			cursor.execute('INSERT INTO accounts VALUES \
    			(NULL, % s, % s, % s, % s, % s, % s, % s, % s, % s)',
    						(username, password, email,
    							organisation, address, city,
    							state, country, postalcode, ))
    			mysql.connection.commit()
    			msg = 'You have successfully registered !'
    	elif request.method == 'POST':
    		msg = 'Please fill out the form !'
    	return render_template('register.html', msg=msg)
    
    
    @app.route("/index")
    def index():
    	if 'loggedin' in session:
    		return render_template("index.html")
    	return redirect(url_for('login'))
    
    
    @app.route("/display")
    def display():
    	if 'loggedin' in session:
    		cursor = mysql.connection.cursor(MySQLdb.cursors.DictCursor)
    		cursor.execute('SELECT * FROM accounts WHERE id = % s',
    					(session['id'], ))
    		account = cursor.fetchone()
    		return render_template("display.html", account=account)
    	return redirect(url_for('login'))
    
    
    @app.route("/update", methods=['GET', 'POST'])
    def update():
    	msg = ''
    	if 'loggedin' in session:
    		if request.method == 'POST' and 'username' in request.form and 'password' in request.form and 'email' in request.form and 'address' in request.form and 'city' in request.form and 'country' in request.form and 'postalcode' in request.form and 'organisation' in request.form:
    			username = request.form['username']
    			password = request.form['password']
    			email = request.form['email']
    			organisation = request.form['organisation']
    			address = request.form['address']
    			city = request.form['city']
    			state = request.form['state']
    			country = request.form['country']
    			postalcode = request.form['postalcode']
    			cursor = mysql.connection.cursor(MySQLdb.cursors.DictCursor)
    			cursor.execute(
    				'SELECT * FROM accounts WHERE username = % s',
    					(username, ))
    			account = cursor.fetchone()
    			if account:
    				msg = 'Account already exists !'
    			elif not re.match(r'[^@]+@[^@]+\.[^@]+', email):
    				msg = 'Invalid email address !'
    			elif not re.match(r'[A-Za-z0-9]+', username):
    				msg = 'name must contain only characters and numbers !'
    			else:
    				cursor.execute('UPDATE accounts SET username =% s,\
    				password =% s, email =% s, organisation =% s, \
    				address =% s, city =% s, state =% s, \
    				country =% s, postalcode =% s WHERE id =% s', (
    					username, password, email, organisation,
    				address, city, state, country, postalcode,
    				(session['id'], ), ))
    				mysql.connection.commit()
    				msg = 'You have successfully updated !'
    		elif request.method == 'POST':
    			msg = 'Please fill out the form !'
    		return render_template("update.html", msg=msg)
    	return redirect(url_for('login'))
    
    
    if __name__ == "__main__":
    	app.run(host="localhost", port=int("5000"))
    
------------

#### - Create the folder "templates" (inside the folder "geeksprofile")
#### - Create "login.html" folder and write the code given below.
#### - In this file we have two fields i.e. username and password. When user enters correct username and password, it will route you to index page otherwise "Incorrect username/password" is displayed.

    <!--Store this code in 'login.html' file inside the 'templates' folder-->
    <html>
    	<head>
    		<meta charset="UTF-8">
    		<title> Login </title>
    		<link rel="stylesheet" href="../static/style.css">
    	</head>
    	<body>
    		<div class="logincontent" align="center">
    			<div class="logintop">
    				<h1>Login</h1>
    			</div></br></br></br></br>
    			<div class="loginbottom">
    			<form action="{{ url_for('login')}}" method="post" autocomplete="off">
    			<div class="msg">{{ msg }}</div>
    			<input type="text" name="username" placeholder="Enter Your Username" class="textbox" id="username" required></br></br>
    			<input type="password" name="password" placeholder="Enter Your Password" class="textbox" id="password" required></br></br></br>
    			<input type="submit" class="btn" value="Login">
    			</form></br></br>
    			<p class="worddark">New to this page? <a class="worddark" href="{{ url_for('register')}}">Register here</a></p>
    			</div>
    		</div>
    	</body>
    </html>

------------

#### - Create "register.html" folder and write the code given below.
#### - In this file we have nine fields i.e. username, password, email, organisation, address, city, state, country, postal code. When user enters all the information, it stored the data in the database and "Registration successful" is displayed.

    <!--Store this code in 'register.html' file inside the 'templates' folder-->
    <html>
    	<head>
    		<meta charset="UTF-8">
    		<title> register </title>
    		<link rel="stylesheet" href="../static/style.css">
    	</head>
    	<body>
    		<div class="registercontent" align="center">
    			<div class="registertop">
    				<h1>Register</h1>
    			</div></br></br>
    			<div class="registerbottom">
    			<form action="{{ url_for('register')}}" method="post" autocomplete="off">
    			<div class="msg">{{ msg }}</div>
    			<input type="text" name="username" placeholder="Enter Your Username" class="textbox" id="username" required></br></br>
    			<input type="password" name="password" placeholder="Enter Your Password" class="textbox" id="password" required></br></br>
    			<input type="email" name="email" placeholder="Enter Your Email ID" class="textbox" id="email" required></br></br>
    			<input type="text" name="organisation" placeholder="Enter Your Organisation" class="textbox" id="organisation" required></br></br>
    			<input type="text" name="address" placeholder="Enter Your Address" class="textbox" id="address" required></br></br>
    			<input type="text" name="city" placeholder="Enter Your City" class="textbox" id="city" required></br></br>
    			<input type="text" name="state" placeholder="Enter Your State" class="textbox" id="state" required></br></br>
    			<input type="text" name="country" placeholder="Enter Your Country" class="textbox" id="country" required></br></br>
    			<input type="text" name="postalcode" placeholder="Enter Your Postal Code" class="textbox" id="postalcode" required></br></br>
    			<input type="submit" class="btn" value="Register">
    			</form>
    			<p class="worddark">Already have account? <a class="worddark" href="{{ url_for('login')}}">Login here</a></p>
    			</div>
    		</div>
    	</body>
    </html>

------------

#### - Create "index.html" folder and write the code given below.
#### - When user logs in successfully, this page is displayed and "Logged in successful!" is displayed.

    <!--Store this code in 'index.html' file inside the 'templates' folder-->
    <html lang="en">
    	<head>
    		<title>index</title>
    		<link rel="stylesheet" href="../static/style.css">
    	</head>
    
    	<body background-color="#e6ffee">
    		<div class="one">
    			<div class="two">
    				<h1>Side Bar</h1>
    				<ul>
    					<li class="active"><a href="{{url_for('index')}}">Index</a></li>
    					<li><a href="{{url_for('display')}}">Display</a></li>
    					<li><a href="{{url_for('update')}}">Update</a></li>
    					<li><a href="{{url_for('logout')}}">Log out</a></li>
    				</ul>
    			</div>
    			<div class="content" align="center">
    				<div class="topbar">
    					<h2>Welcome!! You are in Index Page!! </h2>				
    				</div></br></br>
    				<div class="contentbar">
    					<div class="msg">{{ msg }}</div>
    				</div>
    				
    			</div>
    		</div>
    	</body>
    </html>

------------

#### - Create "display.html" folder and write the code given below.
#### - Here, the user information stored in database are displayed.

    <!--Store this code in 'display.html' file inside the 'templates' folder-->
    
    <html lang="en">
    	<head>
    		<title>display</title>
    		<link rel="stylesheet" href="../static/style.css">
    	</head>
    	<body background-color="#e6ffee">
    		<div class="one">
    			<div class="two">
    				<h1>Side Bar</h1>
    				<ul>
    					<li><a href="{{url_for('index')}}">Index</a></li>
    					<li class="active"><a href="{{url_for('display')}}">Display</a></li>
    					<li><a href="{{url_for('update')}}">Update</a></li>
    					<li><a href="{{url_for('logout')}}">Log out</a></li>
    				</ul>
    			</div>
    			<div class="content" align="center">
    				<div class="topbar">
    					<h2>Welcome!! You are in Display Page!! </h2>				
    				</div> </br>
    				<div class="contentbar">
    					<h1>Your Details</h1> </br>
    					{% block content %}
    						<div class="border">
    							<table class="worddark"></br></br></br></br>
    								<tr>
    									<td>Username:</td>
    									<td>{{ account['username'] }}</td>
    								</tr>
    								<tr>
    									<td>Password:</td>
    									<td>{{ account['password'] }}</td>
    								</tr>
    								<tr>
    									<td>Email ID:</td>
    									<td>{{ account['email'] }}</td>
    								</tr>
    								<tr>
    									<td>Organisation:</td>
    									<td>{{ account['organisation'] }}</td>
    								</tr>
    								<tr>
    									<td>Address:</td>
    									<td>{{ account['address'] }}</td>
    								</tr>
    								<tr>
    									<td>City:</td>
    									<td>{{ account['city'] }}</td>
    								</tr>
    								<tr>
    									<td>State:</td>
    									<td>{{ account['state'] }}</td>
    								</tr>
    								<tr>
    									<td>Country:</td>
    									<td>{{ account['country'] }}</td>
    								</tr>
    								<tr>
    									<td>Postal code:</td>
    									<td>{{ account['postalcode'] }}</td>
    								</tr>					
    							</table>
    						</div>
    					{% endblock %}									
    				</div>
    				
    			</div>
    		</div>
    	</body>
    </html>

------------

#### - Create "update.html" folder and write the code given below.
#### - The user can update his/her data which also updates the database.

    <!--Store this code in 'update.html' file inside the 'templates' folder-->
    <html lang="en">
    	<head>
    		<title>update</title>
    		<link rel="stylesheet" href="../static/style.css">
    	</head>
    	<body background-color="#e6ffee">
    		<div class="one">
    			<div class="two">
    				<h1>Side Bar</h1>
    				<ul>
    					<li><a href="{{url_for('index')}}">Index</a></li>
    					<li><a href="{{url_for('display')}}">Display</a></li>
    					<li class="active"><a href="{{url_for('update')}}">Update</a></li>
    					<li><a href="{{url_for('logout')}}">Log out</a></li>
    				</ul>
    			</div>
    			<div class="content" align="center">
    				<div class="topbar">
    					<h2>Welcome!! You are in Update Page!! </h2>				
    				</div></br></br>
    				<div class="contentbar">
    				<h1>Fill Your Details to Update</h1></br>
    			<form action="{{ url_for('update') }}" method="post" autocomplete="off">
    					<div class="msg">{{ msg }}</div>
    					<input type="text" name="username" placeholder="Enter Your Username" class="textbox" id="username" required></br></br>
    					<input type="password" name="password" placeholder="Enter Your Password" class="textbox" id="password" required></br></br>
    					<input type="email" name="email" placeholder="Enter Your Email ID" class="textbox" id="email" required></br></br>
    					<input type="text" name="organisation" placeholder="Enter Your Organisation" class="textbox" id="organisation" required></br></br>
    					<input type="text" name="address" placeholder="Enter Your Address" class="textbox" id="address" required></br></br>
    					<input type="text" name="city" placeholder="Enter Your City" class="textbox" id="city" required></br></br>
    					<input type="text" name="state" placeholder="Enter Your State" class="textbox" id="state" required></br></br>
    					<input type="text" name="country" placeholder="Enter Your Country" class="textbox" id="country" required></br></br>
    					<input type="text" name="postalcode" placeholder="Enter Your Postal Code" class="textbox" id="postalcode" required></br></br>
    					<input type="submit" class="btn" value="Update">
    			</form>				
    				</div>
    				
    			</div>
    		</div>
    	</body>
    </html>

------------

#### - Create the folder "static". create the file "style.css" inside the "static" folder and paste the given CSS code.

    /*Store this code in 'style.css' file inside the 'static' folder*/
    
    .logincontent{
    	margin: 0 auto;
    	height: 500px;
    	width: 400px;
    	background-color: #e6ffee;
    	border-radius: 10px;
    }
    
    .registercontent{
    	margin: 0 auto;
    	height: 720px;
    	width: 400px;
    	background-color: #e6ffee;
    	border-radius: 10px;
    }
    
    .logintop{
    	height: 60px;
    	width: 400px;
    	background-color: #009933;
    	color: #ffffff;
    }
    
    .registertop{
    	height: 60px;
    	width: 400px;
    	background-color: #009933;
    	color: #ffffff;
    }
    
    .textbox{
    	padding: 10px 40px;
    	background-color: #009933;
    	border-radius: 10px;
    }
    
    ::placeholder {
    	color: #FFFFFF;
    	opacity: 1;
    	font-style: oblique;
    	font-weight: bold;
    }
    
    .btn {
    	padding: 10px 40px;
    	background-color: #009933;
    	color: #FFFFFF;
    	font-style: oblique;
    	font-weight: bold;
    	border-radius: 10px;
    }
    
    .worddark{
    	color: #009933;
    	font-style: oblique;
    	font-weight: bold;
    }
    
    .wordlight{
    	color: #FFFFFF;
    	font-style: oblique;
    	font-weight: bold;
    }
    
    *{
    	margin: 0;
    	padding: 0;
    	box-sizing: border-box;
    	list-style: none;
    	text-decoration: none;
    	font-family: 'Josefin Sans', sans-serif;
    }
    
    .one{
    	display: flex;
    	position: relative;
    }
    
    .one .two{
    	width: 225px;
    	height: 100%;
    	background: #009933;
    	padding: 30px 0px;
    	position: fixed;
    }
    
    .one .two h1{
    	color: #fff;
    	text-transform: uppercase;
    	text-align: center;
    	margin-bottom: 30px;
    	font-style: oblique;
    	font-weight: bold;
    }
    		
    .one .two h2{
    	color: #fff;
    	text-align: center;
    }
    		
    .one .two .active{
    	background: #0a8032;
    }
    
    .one .two ul li{
    	text-align: center;
    	padding: 15px;
    	border-bottom: 0.1px solid white;
    	border-top: 0.1px solid white;
    }
    
    .one .two ul li a{
    	color: #ffffff;
    	display: block;
    }
    
    .one .two ul li a .side{
    	width: 25px;
    	text-align:center;
    }
    
    .one .content{
    	width: 100%;
    	margin-left: 200px;
    }
    
    .one .content .topbar{
    	text-align: center;
    	padding: 20px;
    	background: #00b33c;
    	color: white;
    }
    		
    .one .content .contentbar{
    	margin: auto;
    }
    
    .one .content .contentbar h1{
    	color: #11a844;
    	text-align: center;
    	font-style: oblique;
    	font-weight: bold;
    }

------------

#### - The project structure will look like this:

<img width="859" alt="code in VS Code" src="https://user-images.githubusercontent.com/40535130/235339157-3274fa5b-adeb-46ed-99d4-6c706924ef15.png">

------------

#### - Commit and push the changes to the remote repository.
    git status
    git add .
    git commit -m "Added profile application from tutorial"
    git push origin master

<img width="860" alt="push code into repo" src="https://user-images.githubusercontent.com/40535130/235339193-666e24d0-524e-47e1-97bc-9d8fd50aa1fa.png">

------------

### Run the application on a Linux server in Azure.
#### - Create your own free account in Microsoft Azure platform:
#### [https://azure.microsoft.com/en-us/](https://azure.microsoft.com/en-us/ "https://azure.microsoft.com/en-us/")
<img width="843" alt="new account in azure" src="https://user-images.githubusercontent.com/40535130/230751633-dbb06b1c-4241-4a44-a0d3-09927caf83ec.png">

#### - Create a Resource groups:
##### - from the top portal menu Select Resource groups
<img width="858" alt="select RG" src="https://user-images.githubusercontent.com/40535130/235340502-34da95fe-545d-4f4b-a771-1ece4992e0ec.png">

##### - Select Create resource group
<img width="859" alt="Create RG" src="https://user-images.githubusercontent.com/40535130/235339262-092a76da-eb3f-44ea-be91-711f5ff2fe10.png">

##### - Enter the following values:
- Subscription: Select your Azure subscription.
- Resource group: Enter a new resource group name.
- Region: Select an Azure location, such as Central US.
- Select Review + Create

##### - Select Create, It takes a few seconds to create a resource group.
<img width="701" alt="RG" src="https://user-images.githubusercontent.com/40535130/235339922-f8e0dc67-c708-4574-aa77-2b700d9368e9.png">

##### - Click the "Create a resource" button.

##### - Search for "Ubuntu Server" and select the latest LTS version (e.g., Ubuntu Server 20.04 LTS).
<img width="859" alt="Search Ubuntu Server" src="https://user-images.githubusercontent.com/40535130/235339962-329ac542-046a-4f63-ade0-412bda6f7787.png">


#####  - Click "Create" and fill in the required information for your virtual machine:
###### - Subscription
###### - Resource group (create a new one or use an existing one)
###### - Virtual machine name
###### - Region
###### - Availability options (optional)
###### - Image (should be preselected)
###### - Size (choose based on your requirements)
###### - Administrator account (username and either password or SSH public key)
###### - Inbound port rules (allow at least SSH, and also the port for your Flask application, e.g., 5000 (on Networking))
###### - Click "Review + create" and review your settings. 
<img width="860" alt="deployment VM" src="https://user-images.githubusercontent.com/40535130/235340415-268def3f-508c-4b34-a712-8fbb3ce4f393.png">


##### - connect to your Ubuntu VM using SSH:
- At your prompt, open an SSH connection to your virtual machine. Replace the IP address with the one from your VM, and replace the path to the .pem with the path to where the key file was downloaded.

		ssh -i <private key path> azureuser@20.83.162.184

<img width="674" alt="connected to VM" src="https://user-images.githubusercontent.com/40535130/235340761-5789979f-a4b4-42a0-a9b6-3253af90100b.png">

##### - Update the package list and upgrade the system:
    sudo apt-get update
    sudo apt-get upgrade

##### - Install Git, Python, and pip:
    sudo apt-get install git python3 python3-pip

##### - Install MySQL Server:
    sudo apt-get install mysql-server

##### -  Secure the MySQL installation:
    sudo mysql_secure_installation
###### - Follow the prompts to set up a root password and secure the installation.

##### - Clone the repository from Azure DevOps:
    git clone <your_repository_url>
    cd <your_repository_name>

##### - Set up a Python virtual environment and install the required packages:
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

##### Configure the MySQL server and create the necessary database and tables:

##### - Log in to MySQL as the root user:
    sudo mysql -u root -p

##### - Create a new MySQL user and database for the application (replace placeholders with your desired credentials):
    CREATE USER '<your_mysql_user>'@'localhost' IDENTIFIED BY '<your_mysql_password>';
    CREATE DATABASE <your_database_name>;
    GRANT ALL PRIVILEGES ON <your_database_name>.* TO '<your_mysql_user>'@'localhost';
    FLUSH PRIVILEGES;

##### - Create the accounts table:
    CREATE TABLE IF NOT EXISTS accounts (
      id int NOT NULL AUTO_INCREMENT,
      username VARCHAR(50) NOT NULL,
      password VARCHAR(50) NOT NULL,
      email VARCHAR(100) NOT NULL,
      organisation VARCHAR(100) NOT NULL,
      address VARCHAR(100) NOT NULL,
      city VARCHAR(100) NOT NULL,
      state VARCHAR(100) NOT NULL,
      country VARCHAR(100) NOT NULL,
      postalcode VARCHAR(100) NOT NULL,
      PRIMARY KEY (id)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=UTF8MB4;







##### - Install the necessary Python packages on the Linux server:
    pip3 install Flask
    pip3 install Flask-MySQLdb

##### - Run the Flask application on the Linux server:
    export FLASK_APP=app.py
    export FLASK_ENV=development
    flask run --host=0.0.0.0

###### - Your profile application should now be running on your Azure Linux server, and you can access it using the server's public IP address followed by the port number, http://<SERVER_IP>:5000.

<img width="861" alt="test1" src="https://user-images.githubusercontent.com/40535130/233801819-7baeb9f9-7a5b-4e66-bb26-17e3b1776579.png">


<img width="859" alt="test2" src="https://user-images.githubusercontent.com/40535130/233801827-e1b17e23-7664-4392-941a-89f79459b84e.png">

<img width="839" alt="welcome" src="https://user-images.githubusercontent.com/40535130/233896020-45872f7e-8e4e-46db-85bf-3b4023709074.png">

<img width="891" alt="database table" src="https://user-images.githubusercontent.com/40535130/233896043-00f64eec-804d-4af3-802e-36eba263b214.png">

------------

### Dockerize the application with Docker and Docker Compose

##### - Update the package index and install dependencies:
    sudo apt update
    sudo apt install apt-transport-https ca-certificates curl software-properties-common

##### - Add Docker's official GPG key:
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

##### - Add the Docker repository:
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

##### - Install Docker:
    sudo apt update
    sudo apt install docker-ce

##### - Install Docker Compose:
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

##### - Make the Docker Compose binary executable:
    sudo chmod +x /usr/local/bin/docker-compose

##### - Create a Dockerfile in the project directory:
    touch Dockerfile

##### - Open the Dockerfile in your preferred text editor:
    nano Dockerfile

##### - Add the necessary Docker instructions to define your application's container environment. For a Python Flask app, you might use the following:
    # Use an official Python runtime as a parent image
    FROM python:3.8-slim
    
    # Set the working directory to /app
    WORKDIR /app
    
    # Copy the current directory contents into the container at /app
    COPY . /app
    
    # Install any needed packages specified in requirements.txt
    RUN apt-get update && \
        apt-get install -y gcc libmariadb-dev-compat libmariadb-dev
    
    RUN pip install --trusted-host pypi.python.org -r requirements.txt
    
    # Make port 5000 available to the world outside this container
    EXPOSE 5000
    
    # Define environment variable
    ENV NAME World
    
    # Run app.py when the container launches
    CMD ["python", "app.py"]

##### - Create a 'requirements.txt' file:
    Flask==2.1.1
    Flask-SQLAlchemy==2.5.1
    Flask-WTF==1.1.1
    mysqlclient

##### - Create a docker-compose.yml file in the project directory.
    touch docker-compose.yml

##### - Open the docker-compose.yml in your preferred text editor:
    nano docker-compose.yml

    version: '3'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
      mysql:
        image: "mysql:5.7"
        environment:
          MYSQL_ROOT_PASSWORD: EyadAmer2022
          MYSQL_DATABASE: geekprofile
          MYSQL_USER: EyadAmer
          MYSQL_PASSWORD: EyadAmer2022
        volumes:
          - "./mysql-data:/var/lib/mysql"
        ports:
          - "3307:3306"

#### - Build and run the Docker containers
##### - In the terminal, navigate to the project directory.

##### - Build the Docker containers:
    sudo docker-compose build

<img width="673" alt="build docker" src="https://user-images.githubusercontent.com/40535130/233807897-93082b7c-14ae-40fc-87c5-e299b458f129.png">


##### - If the build is successful, you can start the Docker container using the following command:
    sudo docker-compose up

<img width="595" alt="compuse" src="https://user-images.githubusercontent.com/40535130/233809264-41cba892-d897-469b-a0dd-2043f1b8f190.png">

------------
### - Create pipeline from build to deploy using Azure DevOps

##### - Log in to your Azure DevOps account and go to your project.
##### - Click on 'Pipelines' in the left-hand menu, then click on "Create Pipeline".
<img width="859" alt="create pipeline" src="https://user-images.githubusercontent.com/40535130/233810424-18089a1e-bc40-418c-aed8-85e0ad86fe50.png">

##### - Choose the location of your code, which is 'Azure Repos Git' in this case.
<img width="860" alt="pipeline repo" src="https://user-images.githubusercontent.com/40535130/233810447-82da2f9e-fbf2-454d-a0b2-6f663001f4da.png">

##### - Select your repository and click "Continue".
<img width="859" alt="chose repo" src="https://user-images.githubusercontent.com/40535130/233810638-9a564963-9477-4555-83fe-afbb9d13eb16.png">

##### - Configure your pipeline by choosing "Docker":

![configure your pipeline](https://user-images.githubusercontent.com/40535130/233854816-143ff0ad-4397-4175-8c72-0dae87efad1f.png)


##### - Save and run the pipeline. The pipeline will build and push the Docker images to your Azure Container Registry, and then deploy the application to Azure App Service.

![ci-cd](https://user-images.githubusercontent.com/40535130/233854780-117b5ede-3cb3-46f0-9616-19f5735e2d2b.png)


