flask_xsrf
==========

a flask extension for defending against cross-site request forgery attacks
(xsrf/csrf).


[![Build Status](https://travis-ci.org/gregorynicholas/flask-xsrf.png?branch=master)](https://travis-ci.org/gregorynicholas/flask-xsrf)


* [docs](http://gregorynicholas.github.io/flask-xsrf)
* [source](http://github.com/gregorynicholas/flask-xsrf)
* [package](http://packages.python.org/flask-xsrf)
* [changelog](https://github.com/gregorynicholas/flask-xsrf/blob/master/CHANGES.md)
* [travis-ci](http://travis-ci.org/gregorynicholas/flask-xsrf)


-----


### overview

* [todo]


### features

* [todo]


-----

### install with pip

    pip install flask-xsrf


### example usage

    from flask import Flask, Response, session
    app = Flask(__name__)
    app.debug = True
    app.secret_key = 'session_secret_key'
    app.config['session_cookie_secure'] = True
    app.config['remember_cookie_name'] = 'testdomain.com'
    app.config['remember_cookie_duration_in_days'] = 1

    @app.before_request
    def before_request():
      if 'user_id' not in session:
        session['user_id'] = 'random_generated_anonymous_id'

    def get_user_id():
      return session.get('user_id')

    xsrfh = xsrf.XSRFTokenHandler(
      user_func=get_user_id, secret='xsrf_secret', timeout=3600)

    @app.route('/test', methods=['GET'])
    @xsrfh.send_token()
    def test_get():
      return Response('success')

    @app.route('/test', methods=['POST'])
    @xsrfh.handle_token()
    def test_post():
      return Response('success')
