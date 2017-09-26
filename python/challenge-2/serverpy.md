from flask import *
from hash import SLHA1
app = Flask(__name__)

key = file('SECRET').read().strip()

@app.route('/')
def root():
	return redirect(url_for('login'))

@app.route('/login',methods = ['GET', 'POST'])
def login():
	if request.method == 'POST':
		if  not request.form.get('username'):
			return render_template('login.html')
		else:
			username = str(request.form.get('username'))
			if request.cookies.get('data') and request.cookies.get('user'):
				data = str(request.cookies.get('data')).decode('base64').strip()
				user = str(request.cookies.get('user')).decode('base64').strip()				
				temp = '|'.join([key,username,user])
				if data != SLHA1(temp).digest():
					temp = SLHA1(temp).digest().encode('base64').strip().replace('\n','')
					resp = make_response(render_template('welcome_new.html',name = username))
					resp.set_cookie('user','user'.encode('base64').strip())
					resp.set_cookie('data',temp)
					return resp
				else:
					if 'admin' in user: # too lazy to check properly :p
						return "Here you go : CTF{XXXXXXXXXXXXXXXXXXXXXXXXX}"
					else:
						return render_template('welcome_back.html',name = username)
			else:
				resp = make_response(render_template('welcome_new.html',name = username))
				temp = '|'.join([key,username,'user'])
				resp.set_cookie('data',SLHA1(temp).digest().encode('base64').strip().replace('\n',''))
				resp.set_cookie('user','user'.encode('base64').strip())
				return resp

	else:
		return render_template('login.html')

@app.route('/logout')
def logout():
	resp = make_response(render_template('login.html'))
	resp.set_cookie('data','',expires=0)
	resp.set_cookie('user','',expires=0)
	return (resp)

if __name__=="__main__":
	app.run()
