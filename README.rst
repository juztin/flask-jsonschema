Flask-JsonSchemer
================

JSON request validation for Flask applications.

Place schemas in the specified ``JSONSCHEMER_DIR``. ::

    import os

    from flask import Flask, request
    from flask.ext.jsonschemer import JsonSchemer, ValidationError, validate

    app = Flask(__name__)
    app.config['JSONSCHEMER_DIR'] = os.path.join(app.root_path, 'schemas')

    JsonSchemer(app)

    @app.errorhandler(ValidationError)
    def on_validation_error(e):
        return "error"

    @app.route('/books', methods=['POST'])
    @validate('books', 'create')
    def create_book():
        # create the book
        return 'success'

The schema for the example above should be named ``books.json`` and should
reside in the configured folder. It should look like so::

    {
      "create": {
        "type": "object",
        "properties": {
          "title": {},
          "author": {}
        },
        "required": ["title", "author"]
      },
      "update": {
        "type": "object",
        "properties": {
          "title": {},
          "author": {}
        }
      }
    }

Notice the top level action names. Flask-JsonSchemer supports one "path" level so
that you can organize related schemas in one file. If you do not wish to use this
feature you can simply use one schema per file and remove the second parameter
to the ``@validate`` call.


Resources
---------

- `Issue Tracker <http://github.com/juztin/flask-jsonschemer/issues>`_
- `Code <http://github.com/juztin/flask-jsonschemer/>`_
