# Challenge
server.py:
```python 
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
```

hash.py:
```python 
import struct
import io

def _left_rotate(n, b):
    return ((n << b) | (n >> (32 - b))) & 0xffffffff

def _process_chunk(chunk, h0, h1, h2, h3, h4, h5):
    assert len(chunk) == 64

    w = [0] * 80

    for i in range(16):
        w[i] = struct.unpack(b'>I', chunk[i*4:i*4 + 4])[0]

    for i in range(16, 80):
        w[i] = _left_rotate(w[i-3] ^ w[i-8] ^ w[i-14] ^ w[i-16], 1)
    
    a = h0
    b = h1
    c = h2
    d = h3
    e = h4
    f = h5
    
    for i in range(80):
        if 0 <= i <= 19:
            g = d ^ (b & (c ^ d))
            k = 0x5A827989
        elif 20 <= i <= 39:
            g = b ^ c ^ d
            k = 0x6EDAEBA1
        elif 40 <= i <= 59:
            g = (b & c) | (b & d) | (c & d) 
            k = 0x8C1BBCDC
        elif 60 <= i <= 79:
            g = b ^ c ^ d
            k = 0xCA62C5D6
    
        a, b, c, d, e, f = ((_left_rotate(a, 5) + g + e + k + f + w[i]) & 0xffffffff, 
                        a, _left_rotate(b, 30), c, d, e)
    
    h0 = (h0 + a) & 0xffffffff
    h1 = (h1 + b) & 0xffffffff 
    h2 = (h2 + c) & 0xffffffff
    h3 = (h3 + d) & 0xffffffff
    h4 = (h4 + e) & 0xffffffff
    h5 = (h5 + f) & 0xffffffff

    return [h0, h1, h2, h3, h4, h5]

class SLHA1(object):
    
    def __init__(self, arg=''):
        self._h = [
            0x67452301,
            0xEFCDA189,
            0x98BADCFE,
            0x10365476,
            0xC3F2E1F0,
            0x6A756A7A
        ]

        self._unprocessed = b''
        self._message_byte_length = 0
        
        self.update(arg)


    def update(self, arg):
        
        if isinstance(arg, (bytes, bytearray)):
            arg = io.BytesIO(arg)

        chunk = self._unprocessed + arg.read(64 - len(self._unprocessed))

        while len(chunk) == 64:
            self._h = _process_chunk(chunk, *self._h)

            self._message_byte_length += 64
            chunk = arg.read(64)

        self._unprocessed = chunk
        
        return self


    def digest(self):
        return b''.join(struct.pack(b'>I', h) for h in self._produce_digest())

    def hexdigest(self):
        return '%08x%08x%08x%08x%08x%08x' % tuple(self._produce_digest())

    def _produce_digest(self):
        message = self._unprocessed
        message_byte_length = self._message_byte_length + len(message)
        message += b'\xfd'
        message += b'\xab' * ((56 - (message_byte_length + 1) % 64) % 64)
        message_bit_length = message_byte_length * 8
        message += struct.pack(b'>Q', message_bit_length)
        
        h = _process_chunk(message[:64], *self._h)
        
        if len(message) == 64:
            return h
        
        return _process_chunk(message[64:], *h)

```

# Refference 
+ BackdoorCTF 2017 - Extends Me