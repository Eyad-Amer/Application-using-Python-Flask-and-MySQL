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

##### - Create requirements.txt file that lists all the Python packages required for your application:

###### 1. In your project directory, create a new file called requirements.txt:
    touch requirements.txt

###### 2. Open the file in a text editor, such as nano:
    nano requirements.txt

###### 3. Add the required packages to the file:
    Flask
    Flask-MySQLdb

##### - Set up a Python virtual environment and install the required packages:
    	sudo apt install python3.8-venv
        python3 -m venv venv
        source venv/bin/activate
		sudo apt-get update
		sudo apt-get install libmysqlclient-dev
		pip install wheel
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

##### - Update your Flask application's database configuration with the MySQL credentials you created.

##### - To access the Flask application running on your remote Azure VM, you need to use the public IP address of the VM and update the Flask app to listen on all available IP addresses.

###### - Update the app.py file to bind the server to 0.0.0.0, which allows connections from any IP address. Open the file in a text editor:
    app.run(host='0.0.0.0', port=5000)


##### - Run the Flask application :
		python app.py

##### - In your web browser, use the public IP address and the port number to access the Flask application. Replace your_vm_public_ip with the actual IP address:

    http://your_vm_public_ip:5000

##### - Login Page:
<img width="859" alt="running on linux" src="https://user-images.githubusercontent.com/40535130/235344397-8cb316a4-2b61-4256-a467-8e12182a3392.png">

##### - successfully registered:
<img width="859" alt="reqister successfully" src="https://user-images.githubusercontent.com/40535130/235344493-15f4af33-fa2b-49a9-bb87-7739412d18ab.png">

##### - Index Page:
<img width="860" alt="Index Page" src="https://user-images.githubusercontent.com/40535130/235344577-7d91cc4a-4c61-4433-9ee3-bc8743ac03f1.png">

##### - Display Page:
<img width="860" alt="Display Page" src="https://user-images.githubusercontent.com/40535130/235344596-e294109a-e20c-4ba6-a5cf-9dfc32137a78.png">

------------

### Dockerize the application with Docker and Docker Compose

##### - Install Docker on your Azure VM:
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo usermod -aG docker $USER

##### - Install Docker Compose:
    sudo apt-get install docker-compose

##### - In your Flask application directory, create a Dockerfile:
    touch Dockerfile
    nano Dockerfile

##### - Add the following contents to the Dockerfile:
    # Use the official Python image as the base image
    FROM python:3.8-slim
    
    # Set the working directory
    WORKDIR /app
    
    # Copy requirements.txt into the container
    COPY requirements.txt .
    
    # Install the dependencies
    RUN pip install --no-cache-dir -r requirements.txt
    
    # Copy the rest of the application code into the container
    COPY . .
    
    # Expose the port the app will run on
    EXPOSE 5000
    
    # Start the application
    CMD ["python", "app.py"]


##### - In the same directory, create a docker-compose.yml file:
    touch docker-compose.yml
    nano docker-compose.yml

##### - Add the following contents to the docker-compose.yml file:
    version: "3.3"
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        depends_on:
          - db
      db:
        image: "mysql:8.0"
        environment:
          MYSQL_ROOT_PASSWORD: your_mysql_root_password
          MYSQL_DATABASE: your_database_name
          MYSQL_USER: your_database_user
          MYSQL_PASSWORD: your_database_password
        volumes:
          - "db_data:/var/lib/mysql"
    volumes:
      db_data:

###### - Replace your_mysql_root_password, your_database_name, your_database_user, and your_database_password with the respective values you used for your MySQL database.

##### - Run your Docker Compose setup:
    sudo docker-compose up -d

<img width="671" alt="Docker done" src="https://user-images.githubusercontent.com/40535130/235346593-2c6ad9fe-92c1-4dd3-8172-c059b86fd2c5.png">


##### - Here's a summary of what happened:
1. The Dockerfile was used to build an image, which includes installing the necessary packages and dependencies from the requirements.txt file.
2. The docker-compose.yml file defined two services: geeksprofile-db-1 (the database) and geeksprofile-web-1 (the web application).
3. A new network (geeksprofile_default) was created for the containers to communicate with each other.
4. A new volume (geeksprofile_db_data) was created to persist the database data.
5. The geeksprofile-db-1 container was started, running the database service.
6. The geeksprofile-web-1 container was started, running the web application service.

##### - To view the logs for the web service:
    sudo docker-compose logs web

<img width="673" alt="run in docker" src="https://user-images.githubusercontent.com/40535130/235346890-8fda3c6b-a17d-4c29-8ab0-c373860b5f53.png">
------------
### - Create pipeline from build to deploy using Azure DevOps

#### -  Push the repository to the source control system supported by Azure DevOps.

####  - Create a Build Pipeline for the Docker image:
##### - Log in to your Azure DevOps account and go to your project.
##### - Click on 'Pipelines' in the left-hand menu, then click on "Create Pipeline".
<img width="860" alt="Create pipeline" src="https://user-images.githubusercontent.com/40535130/235356351-f37f5aa3-a735-4abc-bf91-8f12b04e4d34.png">


##### - Choose the location of your code, which is 'Azure Repos Git' in this case.
<img width="859" alt="where the code" src="https://user-images.githubusercontent.com/40535130/235356472-76a1888a-495d-422c-87bb-122df8da4173.png">


##### - Select your repository and click "Continue".
<img width="860" alt="select a repo" src="https://user-images.githubusercontent.com/40535130/235356508-e867054a-892c-4288-8292-6763e4283480.png">


##### - Configure your pipeline by choosing "Docker" template:

<img width="859" alt="docker" src="https://user-images.githubusercontent.com/40535130/235361865-c70882bc-cf42-46ad-9b0c-e407c28b54c7.png">


##### - Save and run the pipeline. The pipeline will build and push the Docker images to your Azure Container Registry, and then deploy the application to Azure App Service.

<img width="859" alt="pipeline done" src="https://user-images.githubusercontent.com/40535130/235362518-e4c6703e-7efd-49ee-87d8-15a3d3a7ded2.png">


#### - Create a Release Pipeline for deploying the Docker container:

##### - In your Azure DevOps project, navigate to the "Pipelines" section and select "Releases."

<img width="860" alt="releases" src="https://user-images.githubusercontent.com/40535130/235358742-f4161a24-6f07-4d2a-bad6-04077c046749.png">


##### - Click on "New pipeline" to create a new release pipeline.
##### - Choose a template based on your target environment for deployment.

<img width="859" alt="releasesss" src="https://user-images.githubusercontent.com/40535130/235371425-72e16087-f3ec-464e-bdbb-107122c78ca8.png">

##### - In the "Artifacts" section, click "Add an artifact" and select the "Build" source type. Choose the build pipeline you created earlier as the source of the artifacts.

<img width="858" alt="add artifact" src="https://user-images.githubusercontent.com/40535130/235372786-d31f2521-8874-4649-8c6e-19605989c5be.png">

<img width="859" alt="build artifact" src="https://user-images.githubusercontent.com/40535130/235372818-db38699b-ed84-43f7-bec4-cc628338588c.png">

##### - Configure the deployment tasks and settings according to your target environment and project requirements.

##### - Save the release pipeline, and create a new release to test the deployment process.

#### - Automate the CI/CD process:
##### - In your build pipeline, enable the trigger for continuous integration. This ensures that the build pipeline runs automatically whenever changes are pushed to the source repository.

##### - In your release pipeline, enable the continuous deployment trigger. This will automatically create a new release and deploy the application whenever a successful build is completed.




